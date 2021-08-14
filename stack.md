### Stack

```go
package main

import (
	"errors"
	"fmt"
)

type IStack struct {
	max int
	top int
	arr [5]int
}

func (s *IStack) IStackIsFull() bool {
	if s.top == s.max-1 {
		return true
	}
	return false
}

func (s *IStack) IStackIsEmpty() bool {
	if s.top == -1 {
		return true
	}
	return false
}

func (s *IStack) IStackPush(val int) error {
	if s.IStackIsFull() {
		return errors.New("the stack is full")
	}

	s.top++
	s.arr[s.top] = val
	return nil
}

func (s *IStack) IStackShow() {
	if s.IStackIsEmpty() {
		fmt.Println("the stack is empty")
		return
	}

	fmt.Println("==================")
	for i := s.top; i >= 0; i-- {
		fmt.Printf("||\t\t%d\t\t||\n", s.arr[i])
		fmt.Println("==================")
	}

}

func main() {
	iStack := &IStack{
		max: 5,
		top: -1,
	}

	iStack.IStackShow()
	iStack.IStackPush(1)
	iStack.IStackPush(2)
	iStack.IStackPush(3)
	iStack.IStackPush(4)
	iStack.IStackPush(5)
	iStack.IStackShow()
	fmt.Println()
	err := iStack.IStackPush(6)
	if err != nil {
		fmt.Println(err.Error())
	}
	iStack.IStackShow()
	fmt.Println()
}

```


