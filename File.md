# File Read
1. buffer reader

`os.Open()` `bufio.NewReader()` `reader.ReadString()`
```go
package main

import (
  "bufio"
  "fmt"
  "io"
  "os"
)

func main() {
  file, err := os.Open("./1.txt")
  if err != nil {
    fmt.Println("open file fail")
  }

  defer file.Close()

  reader := bufio.NewReader(file)
  for {
    line, err := reader.ReadString('\n')
    if err == io.EOF {
      break
    }
    fmt.Println(line)
  }
  fmt.Println("read finished")
}

```

2. reader without buffer

`ioutil.ReadFile()`

```go
package main

import (
  "fmt"
  "io/ioutil"
)

func main() {
  content, err := ioutil.ReadFile("./1.txt")
  if err != nil {
    fmt.Println("read fail: ", err)
  }

  fmt.Println(string(content))

}
```

# File Writer

1. buffer writer

`os.OpenFile()` `bufio.NewWriter()` `writer.WriteString()` `writer.Flush()`

```go
package main

import (
  "bufio"
  "fmt"
  "io/ioutil"
  "os"
)

func main() {
  file, err := os.OpenFile("./2.txt", os.O_RDWR|os.O_CREATE, 0666)
  if err != nil {
    fmt.Println("open file fail: ", err)
  }

  defer file.Close()

  writer := bufio.NewWriter(file)
  for i := 0; i < 6; i++ {
    _, err := writer.WriteString("hello, golang")
    if err != nil {
      fmt.Println("write file fail: ", err)
    }
  }

  // synchronize data directly
  writer.Flush()

  content, err := ioutil.ReadFile("./2.txt")
  if err != nil {
    fmt.Println("read file fail: ", err)
  }
  fmt.Println(string(content))
}
```

2. writer without buffer

`ioutil.WriteFile()` 

```go
package main

import (
  "fmt"
  "io/ioutil"
)

func main() {
  err := ioutil.WriteFile("./2.txt", []byte("I like Linux, I like Golang!"), 0666)
  if err != nil {
    fmt.Println("write file fail: ", err)
  }

  content, err := ioutil.ReadFile("./2.txt")
  if err != nil {
    fmt.Println("read file fail: ", err)
  }
  fmt.Println(string(content))
}
```



