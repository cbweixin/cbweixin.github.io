title: go-iota-usage
date: 2022-06-10 12:42:45
categories:
- programming
tags:
- go
---

keyword `iota` is used on const enum, it defines the auto-incremental counter starting from 0
for ex
```go
const (
	Sunday    = iota // starting from 0
	Monday           // 1
	Tuesday          // 2
	Wednesday        // 3
	Thursday         // 4
	Friday           // 5
	Saturday         // 6
)
```
[sample 1](https://go.dev/play/p/8ZXYoSb-hLE)

another example
```go
const (
	_        = iota             // iota = 0
	KB int64 = 1 << (10 * iota) // iota = 1
	MB                          // MB = 1 << (10*iota) but iota = 2
	GB                          // same as GB, but iota is 3
	TB                          // iota = 4
)

```
run it, you would see result as this :

```
1024
1048576
1073741824
1099511627776

```

[code](https://go.dev/play/p/5sKvrnw57rS)

in a single const enum, you could provide multiple iota, each of them would increase independtly

```go
const (
	AA, BB = iota, iota << 10 // 0, 0 << 10
	CC, DD                    // 1, 1 << 10
)
```

result : 
```go
0
0
1
1024
```

if `iota` auto-increment is interrupted, you have to explicitly restore it
```go
const (
	A1 = iota // 0
	B1        // 1
	C  = "c"  // c
	D         // c
	E  = iota // 4
	F         // 5
)
```
how `iota` got applied into the const enum, the pesudo code is as such :

```go
for iota, spec := range ValueSpecs {
 for i, name := range spec.Names {
 obj := NewConst(name, iota...) // pass in iota
 ....
 }
```
from this code we can see that, even `iota` appears multiple times in a single line, it won't auto-increment



