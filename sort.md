### BubbleSort

* 总执行循环次数: n - 1
* i 次执行的循环次数: n - 1 - i
* 每次循环内部符合条件就交换

```go
package main

import "fmt"

func BubbleSort(arr *[9]int) {
  for i := 0; i < len(arr)-1; i++ {
    for j := 0; j < len(arr)-1-i; j++ {
      if arr[j] > arr[j+1] {
        arr[j], arr[j+1] = arr[j+1], arr[j]
      }
    }
  }
}

func main() {
  arr := [9]int{3, 6, 7, 2, 8, 1, 9, 4, 5}
  for _, val := range arr {
    fmt.Printf("%d ", val)
  }
  fmt.Println()
  BubbleSort(&arr)
  for _, val := range arr {
    fmt.Printf("%d ", val)
  }

}

```

### SelectionSort

`每次循环找到目前最大or最小的值, 每次循环结束后进行一次交换`

```go
package main

import "fmt"

func SelectionSort(arr *[9]int) {

  for i := 0; i < len(arr)-1; i++ {
    min := arr[i]
    minIdx := i
    for j := i + 1; j < len(*arr); j++ {
      if arr[j] < min {
        min = arr[j]
        minIdx = j
      }
    }
    arr[i], arr[minIdx] = arr[minIdx], arr[i]
  }
}
func main() {
  arr := [9]int{3, 6, 7, 2, 8, 1, 9, 4, 5}
  for _, val := range arr {
    fmt.Printf("%d ", val)
  }
  SelectionSort(&arr)
  fmt.Println()
  for _, val := range arr {
    fmt.Printf("%d ", val)
  }

}
```

### InsertionSort

`左边当作已排序好的数组, 为待排序的值寻找合适的位置`

* 待排序的值: arr[i]
* 合适的位置的前一位: i - 1
* 不满足条件: arr[合适的位置的前一位] 后移, 合适的位置的前一位 - 1
* 满足条件: 待排序的值 插入到 arr[合适的位置的前一位 - 1]

```go
package main

import "fmt"

func InsertionSort(arr *[9]int) {

  for i := 1; i < len(arr); i++ {

    insertVal := arr[i]
    insertPosition := i - 1

    for insertPosition >= 0 && arr[insertPosition] > insertVal {
      arr[insertPosition+1] = arr[insertPosition]
      insertPosition--
    }

    // 如果insertPosition没有移动, 即insertPosition + 1 == i, 则此元素不需要进行排序
    if insertPosition+1 != i {
      arr[insertPosition+1] = insertVal
    }

    fmt.Printf("After %d times, arr = %v\n", i, arr)
  }
}

func main() {
  arr := [9]int{3, 6, 7, 2, 8, 1, 9, 4, 5}
  for _, val := range arr {
    fmt.Printf("%d ", val)
  }
  fmt.Println()
  InsertionSort(&arr)
  for _, val := range arr {
    fmt.Printf("%d ", val)
  }
}

```

### QuickSort

`分成两部分排序`

```go
package main

import (
  "fmt"
)

func QuickSort(left int, right int, arr *[9]int) {
  l := left
  r := right

  pivot := arr[(left+right)/2]

  for l < r {
    for arr[l] < pivot {
      l++
    }

    for arr[r] > pivot {
      r--
    }

    if l >= r {
      break
    }

    arr[l], arr[r] = arr[r], arr[l]

    // 优化
    if arr[l] == pivot {
      r--
    }

    if arr[r] == pivot {
      l++
    }
  }

  if l == r {
    l++
    r--
  }

  if left < r {
    QuickSort(left, r, arr)
  }

  if right > l {
    QuickSort(l, right, arr)
  }
}
func main() {
  arr := [9]int{3, 6, 7, 2, 8, 1, 9, 4, 5}
  for _, val := range arr {
    fmt.Printf("%d ", val)
  }
  QuickSort(0, len(arr)-1, &arr)
  fmt.Println()
  for _, val := range arr {
    fmt.Printf("%d ", val)
  }
}

```


