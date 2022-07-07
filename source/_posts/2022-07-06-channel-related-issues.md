title: channel-related-issues
date: 2022-07-06 20:39:18
categories:
- programming
tags:
- go
---

there are 3 pits when we use channel.
- deadlock
- panic
- oom

## deadlock
```go
// 只有生产者，没有消费者
func f1() {
	ch := make(chan int)
	ch <- 1
}


func main() {
	f1()
}
```
we will see result like this way
```shell
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send]:
main.f1(...)
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:11
main.main()
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:48 +0x31

Process finished with the exit code 2

```
the error output clearly point to the place where deadlock happens


another example
```go
// 只有消费者，没有生产者
func f2() {
	ch := make(chan int)
	<-ch
}

func main() {
	f2()
}
```

result:
```shell
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
main.f2(...)
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:17
main.main()
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:47 +0x2c

Process finished with the exit code 2

```
another one
```shell
// 生产者和消费者出现在同一个 goroutine 中
func f3() {
	ch := make(chan int)
	// producer blocked the routine, consumer has no opportunity to run
	ch <- 1
	// this line not able to reach
	<-ch

}
func main() {
	f3()
}
```

result:
```shell
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send]:
main.f3(...)
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:24
main.main()
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:49 +0x36

Process finished with the exit code 2

```

## OOM
```go
func leak1 () {
	ch := make(chan int)
	go func() {
		time.Sleep(2*time.Second)
		ch <- 100
	}()

	select {
	case <-time.After(500 * time.Millisecond):
		fmt.Println("timeout! exit...")
	case result := <-ch:
		fmt.Printf("result : %n", result)
	}
}

func main() {
	fmt.Printf("before goroutine num: %d\n", runtime.NumGoroutine())
	for i := 0; i < 10; i ++ {
		leak1()
	}
	fmt.Printf("after goroutine num: %d\n", runtime.NumGoroutine())
}
```

result 
```shell
before goroutine num: 1
timeout! exit...
timeout! exit...
timeout! exit...
timeout! exit...
timeout! exit...
timeout! exit...
timeout! exit...
timeout! exit...
timeout! exit...
timeout! exit...
after goroutine num: 11
```

all producers are stucked, because consumers quit too early. groutine number increase from 1 to 11. 

just change this line:
```go
	case <-time.After(2500 * time.Millisecond):
```
result:
```shell
before goroutine num: 1
result : %!n(int=100)
result : %!n(int=100)
result : %!n(int=100)
result : %!n(int=100)
result : %!n(int=100)
result : %!n(int=100)
result : %!n(int=100)
result : %!n(int=100)
result : %!n(int=100)
result : %!n(int=100)
after goroutine num: 1
```






