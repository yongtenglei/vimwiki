# 并发与并行的区别 The difference between concurrency and parallel

`并发: 你的程序看起来是在同时运行, 但在微观角度看，是threads在来回切换。可以仅在一个核心processer中运行。`

示意图: (from [stackoverflow](https://stackoverflow.com/questions/1897993/what-is-the-difference-between-concurrent-programming-and-parallel-programming))
```bash
        --  --  --
     /              \
>---- --  --  --  -- ---->>
```

`并行: 你的程序真的是在一台机器上同时运行, 一定在两个或以上的核心processer中运行 `

示意图: (from [stackoverflow](https://stackoverflow.com/questions/1897993/what-is-the-difference-between-concurrent-programming-and-parallel-programming))

```bash
     ------
    /      \
>-------------->>
```

# 并行实现的两种方法

1. shared-memory

  多个核心共享一片内存进行读写，会出现race condition 等问题。

2. message-passing

  每个核心有自己独立的内存空间，通过消息传递来共享内存，进行交互。


# 举例理解share-memory 与 message-passing (Go)

`1-1. shared-memory without mutux`

```go
package main

import (
  "fmt"
  "sync"
)

var ret int
var wg sync.WaitGroup
// var mtx sync.Mutex

func sum(nums []int) {
  // mtx.Lock()
  for i := 0; i < len(nums); i++ {
    ret += nums[i]
  }
  // mtx.Unlock()
  wg.Done()
}

func main() {
  wg.Add(2)
  nums := make([]int, 100000)
  for i := 0; i < 100000; i++ {
    nums[i] = i + 1
  }

  go sum(nums[:50000])
  go sum(nums[50000:100000])
  wg.Wait()
  fmt.Println("gosum = ", ret)

  var s int
  for i := 0; i < 100000; i++ {
    s += i + 1
  }
  fmt.Println("sum = ", s)

}

```

三次结果均不正确产生的原因是因为同时写内存而产生的race condition 问题。

可能的一种情况是:

```go
sum = 1
在go1写之前，go2读了sum = 1
go1 写 sum += 1， 此时 sum = 1 + 1 = 2
go2 写 sum += 2， 此时 sum = 1+ 2 = 3

则go1的写操作被覆盖而造成错误
```

可以使用mutex锁解决(Go 不经常这么做)


`1-2. share-memory with mutex`

```go
package main

import (
  "fmt"
  "sync"
)

var ret int
var wg sync.WaitGroup
var mtx sync.Mutex

func sum(nums []int) {
  mtx.Lock()
  for i := 0; i < len(nums); i++ {
    ret += nums[i]
  }
  mtx.Unlock()
  wg.Done()
}

func main() {
  wg.Add(2)
  nums := make([]int, 100000)
  for i := 0; i < 100000; i++ {
    nums[i] = i + 1
  }

  go sum(nums[:50000])
  go sum(nums[50000:100000])
  wg.Wait()
  fmt.Println("gosum = ", ret)

  var s int
  for i := 0; i < 100000; i++ {
    s += i + 1
  }
  fmt.Println("sum = ", s)

}

```

`2. message-passing`

```go
package main

import (
  "fmt"
  "sync"
)

var wg sync.WaitGroup

//var mtx sync.Mutex

func sum(nums []int, c chan int) {
  var sum int
  //mtx.Lock()
  for i := 0; i < len(nums); i++ {
    sum += nums[i]
  }
  //mtx.Unlock()
  c <- sum
  wg.Done()
}

func main() {
  c1 := make(chan int, 1)
  c2 := make(chan int, 1)
  wg.Add(2)
  nums := make([]int, 100000)
  for i := 0; i < 100000; i++ {
    nums[i] = i + 1
  }

  go sum(nums[:50000], c1)
  go sum(nums[50000:100000], c2)
  wg.Wait()
  sum1 := <-c1
  sum2 := <-c2
  fmt.Println("gosum = ", sum1+sum2)

  s := 0
  for i := 0; i < 100; i++ {
    s += i + 1
  }
  fmt.Println("sum:= ", s)

}

```

消息传递每一个 goroutine 都拥有其独立的内存空间所以不需要mutex。

每一个 goroutine(更轻量级的线程thread) 完成后，将sum传入channel中，实现消息传递。


# Summary
1. 并发与并行的区别
  * 并发在微观角度并不是同时运行的，只是thread在来回切换。
  * 并行是真正意义上的同时运行，至少在两个核心processer中运行。
2. 实现并发的两种方式
  * shared-memory: 易实现，但容易产生 race condition。
  * message-passing: Go语言的推荐实践。
