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
memory leak bug got fixed. 

another example:
```go
func leak2() {
	ch := make(chan int)

	// consumer
	go func() {
		for result := range ch {
			fmt.Printf("result : %d\n", result)
		}
	}()

	// producer
	ch <- 1
	ch <- 2
	time.Sleep(time.Second)
	fmt.Println("producer exist")

}

func main() {
	fmt.Printf("before goroutine num: %d\n", runtime.NumGoroutine())
	for i := 0; i < 10; i++ {
		leak2()
	}
	fmt.Printf("after goroutine num: %d\n", runtime.NumGoroutine())
}
```
result :
```shell
before goroutine num: 1
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
result : 1
result : 2
producer exist
after goroutine num: 11
```
this happens because of this code:
```go
		for result := range ch {
			fmt.Printf("result : %d\n", result)
		}
```
for .... range code is blocking consumer, to fix this bug, we need to close channel

```go
	ch <- 1
	ch <- 2
	time.Sleep(time.Second)
	fmt.Println("producer close channel")
	close(ch)
```
result :
```shell
before goroutine num: 1
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
result : 1
result : 2
producer close channel
after goroutine num: 1
```

## panic
```go
func panic1() {
	// panic, try to send to a closed channel
	ch := make(chan int, 1)
	close(ch)
	ch <- 1
}

func main() {
	panic1()
}
```
result 

```shell
panic: send on closed channel

goroutine 1 [running]:
main.panic1(...)
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:68
main.main()
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:72 +0x45

```

another example
```go
func panic2() {
	ch := make(chan int, 1)
	done := make(chan  struct{}, 1)

	go func() {
		<- time.After(2 * time.Second)
		fmt.Println("close channel 2")
		close(ch)
		close(done)
	}()

	go func() {
		<- time.After(1*time.Second)
		fmt.Println("close 1")
		ch <- 1
		close(ch)
	}()
	<- done

}

func main() {
	panic2()
}
```


result

```shell
close 1
close channel 2
panic: close of closed channel

goroutine 18 [running]:
main.panic2.func1()
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:78 +0x86
created by main.panic2
        /Users/xin.wei/github-personal/leetcode-go/concurrency/chanel_deadlock.go:75 +0x9d

Process finished with the exit code 2
```





