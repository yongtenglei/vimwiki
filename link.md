### SingleLink

```go
package main

import (
  "errors"
  "fmt"
)

type SNode struct {
  Id       int
  Name     string
  NextNode *SNode
}

// InsertSNode insert to the last position
func InsertSNode(head *SNode, newNode *SNode) {
  tmp := head
  for {
    if tmp.NextNode == nil {
      break
    }
    tmp = tmp.NextNode
  }

  tmp.NextNode = newNode
}

func InsertSNodeOrderly(head *SNode, newNode *SNode) error {
  tmp := head
  for {
    if tmp.NextNode == nil {
      break
    } else if tmp.NextNode.Id > newNode.Id {
      // nice position
      break
    } else if tmp.NextNode.Id == newNode.Id {
      return errors.New("cannot insert newNode cause the same id")
    }
    tmp = tmp.NextNode
  }

  newNode.NextNode = tmp.NextNode
  tmp.NextNode = newNode
  return nil
}

func DeleteSNode(head *SNode, id int) error {
  tmp := head
  for {
    if tmp.NextNode == nil {
      return errors.New("there is no such a node with indicated id, nothing happened")
    } else if tmp.NextNode.Id == id {
      tmp.NextNode = tmp.NextNode.NextNode
      return nil
    }
    tmp = tmp.NextNode
  }
}

func ShowAllSNode(head *SNode) {
  tmp := head
  if tmp.NextNode == nil {
    fmt.Println("There is nothing")
    return
  }

  for {
    fmt.Printf("[id: %d, name: %s] ==> ", tmp.NextNode.Id, tmp.NextNode.Name)
    tmp = tmp.NextNode

    if tmp.NextNode == nil {
      break
    }
  }
}

func main() {
  head := &SNode{}

  node1 := &SNode{
    Id:   1,
    Name: "rey",
  }

  node2 := &SNode{
    Id:   2,
    Name: "charlotte",
  }

  node3 := &SNode{
    Id:   3,
    Name: "martin",
  }
  node4 := &SNode{
    Id:   4,
    Name: "dora",
  }

  node5 := &SNode{
    Id:   4,
    Name: "lucy",
  }
  InsertSNodeOrderly(head, node1)
  ShowAllSNode(head)
  fmt.Println()
  InsertSNodeOrderly(head, node4)
  ShowAllSNode(head)
  fmt.Println()
  InsertSNodeOrderly(head, node3)
  ShowAllSNode(head)
  fmt.Println()
  InsertSNodeOrderly(head, node2)
  ShowAllSNode(head)
  fmt.Println()

  err := InsertSNodeOrderly(head, node5)
  if err != nil {
    fmt.Println(err.Error())
  }

  ShowAllSNode(head)
  fmt.Println()
  err = DeleteSNode(head, 4)
  if err != nil {
    fmt.Println(err.Error())
  }
  ShowAllSNode(head)
}

```

### DualLink

```go
package main

import (
  "errors"
  "fmt"
)

type DNode struct {
  Id           int
  Name         string
  NextNode     *DNode
  PreviousNode *DNode
}

// InsertDNode insert to the last position
func InsertDNode(head *DNode, newNode *DNode) {
  tmp := head
  for {
    if tmp.NextNode == nil {
      break
    }
    tmp = tmp.NextNode
  }

  tmp.NextNode = newNode
  newNode.PreviousNode = tmp
}

func InsertDNodeOrderly(head *DNode, newNode *DNode) error {
  tmp := head
  for {
    if tmp.NextNode == nil {
      break
    } else if tmp.NextNode.Id > newNode.Id {
      // nice position
      break
    } else if tmp.NextNode.Id == newNode.Id {
      return errors.New("cannot insert newNode cause the same id")
    }
    tmp = tmp.NextNode
  }

  newNode.NextNode = tmp.NextNode
  newNode.PreviousNode = tmp
  tmp.NextNode = newNode
  if tmp.NextNode != nil {
    tmp.NextNode.PreviousNode = newNode
  }
  return nil
}

func DeleteDNode(head *DNode, id int) error {
  tmp := head
  for {
    if tmp.NextNode == nil {
      return errors.New("there is no such a node with indicated id, nothing happened")
    } else if tmp.NextNode.Id == id {
      tmp.NextNode = tmp.NextNode.NextNode
      if tmp.NextNode != nil {
        // tmp.NextNode here is equivalent with tmp.NextNode.NextNode previously
        tmp.NextNode.PreviousNode = tmp
      }
      return nil
    }
    tmp = tmp.NextNode
  }
}

func ShowAllDNodeOrder(head *DNode) {
  tmp := head
  if tmp.NextNode == nil {
    fmt.Println("There is nothing")
    return
  }

  for {
    fmt.Printf("[id: %d, name: %s] ==> ", tmp.NextNode.Id, tmp.NextNode.Name)
    tmp = tmp.NextNode
    if tmp.NextNode == nil {
      break
    }

  }
}

func ShowAllDNodeReverseOrder(head *DNode) {
  tmp := head
  if tmp.NextNode == nil {
    fmt.Println("There is nothing")
    return
  }

  for {
    if tmp.NextNode == nil {
      break
    }
    tmp = tmp.NextNode
  }

  for {
    fmt.Printf("[id: %d, name: %s] ==> ", tmp.Id, tmp.Name)
    tmp = tmp.PreviousNode

    if tmp.PreviousNode == nil {
      break
    }
  }
}

func main() {
  head := &DNode{}

  node1 := &DNode{
    Id:   1,
    Name: "rey",
  }

  node2 := &DNode{
    Id:   2,
    Name: "charlotte",
  }

  node3 := &DNode{
    Id:   3,
    Name: "martin",
  }
  node4 := &DNode{
    Id:   4,
    Name: "dora",
  }

  node5 := &DNode{
    Id:   4,
    Name: "lucy",
  }
  InsertDNode(head, node1)
  InsertDNode(head, node2)
  fmt.Println("======order===========")
  ShowAllDNodeOrder(head)
  fmt.Println()
  fmt.Println("===========reverse order===============")
  ShowAllDNodeReverseOrder(head)
  fmt.Println()

  InsertDNodeOrderly(head, node4)
  ShowAllDNodeOrder(head)
  fmt.Println()

  InsertDNodeOrderly(head, node3)
  ShowAllDNodeOrder(head)
  fmt.Println()

  err := InsertDNodeOrderly(head, node5)
  if err != nil {
    fmt.Println(err.Error())
  }
  ShowAllDNodeOrder(head)
  fmt.Println()

  err = DeleteDNode(head, 4)
  if err != nil {
    fmt.Println(err.Error())
  }
  ShowAllDNodeOrder(head)
}

```

### CycleLink

```go
package main

import (
  "errors"
  "fmt"
)

type CNode struct {
  Id       int
  Name     string
  NextNode *CNode
}

func InsertCNode(head *CNode, newNode *CNode) {
  if head.NextNode == nil {
    head.Id = newNode.Id
    head.Name = newNode.Name
    head.NextNode = head
    return
  }

  tmp := head
  for {
    if tmp.NextNode == head {
      break
    }
    tmp = tmp.NextNode
  }

  tmp.NextNode = newNode
  newNode.NextNode = head
}

// DeleteCNode 需要返回头结点，因为删除头结点后，头结点已经改变。
func DeleteCNode(head *CNode, id int) (*CNode, error) {
  tmp := head    //辅助节点指向链表头节点head
  helper := head //辅助节点指向链表最后一个节点

  //判断链表为空
  if head.NextNode == nil {
    return head, errors.New("there is nothing")
  }

  //判断只有一个节点，且这个节点的no编号等于id时删除
  if head.NextNode == head && head.Id == id {
    head.NextNode = nil
    return head, nil
  } else if head.NextNode == head && head.Id != id {
    return head, errors.New("only one node, but have another id")
  }

  // 有多个节点时， 将helper指向链表最后一个节点
  for {
    if helper.NextNode == head {
      break
    }
    helper = helper.NextNode
  }

  for {
    // 此时tmp是最后一个节点，但没有比较是否为需要删除的节点
    if tmp.NextNode == head {
      break
    }
    if tmp.Id == id {
      // 多个节点，且要删除的节点是第一个节点， 将head后移
      if tmp == head {
        head = head.NextNode
      }
      helper.NextNode = tmp.NextNode
      return head, nil
    }
    tmp = tmp.NextNode
    helper = helper.NextNode
  }

  // 比较最后的节点
  if tmp.Id == id {
    helper.NextNode = tmp.NextNode
    return head, nil
  } else {
    return head, errors.New("no such a node to delete")
  }
}

func ShowAllCNode(head *CNode) {
  tmp := head

  if tmp.NextNode == nil {
    fmt.Println("There is nothing")
    return
  }

  for {
    fmt.Printf("[id: %d, name: %s] ==> ", tmp.Id, tmp.Name)

    if tmp.NextNode == head {
      break
    }
    tmp = tmp.NextNode
  }
}

func main() {
  head := &CNode{}

  node1 := &CNode{
    Id:   1,
    Name: "rey",
  }

  node2 := &CNode{
    Id:   2,
    Name: "charlotte",
  }

  node3 := &CNode{
    Id:   3,
    Name: "martin",
  }
  node4 := &CNode{
    Id:   4,
    Name: "dora",
  }

  node5 := &CNode{
    Id:   4,
    Name: "lucy",
  }
  ShowAllCNode(head)
  InsertCNode(head, node1)
  ShowAllCNode(head)
  fmt.Println()
  InsertCNode(head, node2)
  ShowAllCNode(head)
  fmt.Println()
  InsertCNode(head, node3)
  ShowAllCNode(head)
  fmt.Println()
  InsertCNode(head, node4)
  ShowAllCNode(head)
  fmt.Println()
  InsertCNode(head, node5)
  ShowAllCNode(head)
  fmt.Println()

  head, err := DeleteCNode(head, 1)
  if err != nil {
    fmt.Printf("%v, where id = %d\n", err.Error(), 1)
  }
  ShowAllCNode(head)
  fmt.Println()

  head, err = DeleteCNode(head, 3)
  if err != nil {
    fmt.Printf("%v, where id = %d\n", err.Error(), 3)
  }
  ShowAllCNode(head)
  fmt.Println()

  head, err = DeleteCNode(head, 5)
  if err != nil {
    fmt.Printf("%v, where id = %d\n", err.Error(), 5)
  }
  ShowAllCNode(head)
  fmt.Println()
}
```

### Josephu Game

`小朋友从第k个小朋友数n个数, 自己先数1, 数到n的小朋友出列, 然后下一个小朋友数1...以此类推， 得到出队顺序。 `

```go
package main

import (
	"errors"
	"fmt"
)

type BNode struct {
	Id       int
	NextNode *BNode
}

func AddBoy(num int) (*BNode, error) {
	if num < 0 {
		return nil, errors.New("can not add node less than 0")
	}

	head := &BNode{}
	current := &BNode{}
	for i := 1; i <= num; i++ {
		boy := &BNode{
			Id: i,
		}

		if i == 1 {
			head = boy
			current = boy
			current.NextNode = head
		} else {
			current.NextNode = boy
			current = boy
			boy.NextNode = head
		}
	}
	return head, nil
}

func ShowAllBNode(head *BNode) {
	if head.NextNode == nil {
		fmt.Println("There is nothing")
		return
	}

	current := head
	for {
		fmt.Printf("[id: %d] ==> ", current.Id)

		if current.NextNode == head {
			break
		}
		current = current.NextNode
	}
}
func CountBoy(head *BNode) int {
	counter := 0
	if head.NextNode == nil {
		fmt.Println("There is nothing")
		return 0
	}

	current := head
	for {
		if current.NextNode == head {
			break
		}
		counter++
		current = current.NextNode
	}
	return counter
}
func PlayGame(head *BNode, startId int, countNum int) {
	if head.NextNode == nil {
		fmt.Println("There is nothing, game over")
		return
	}

	if startId > CountBoy(head) {
		fmt.Printf("Cannot start with startId = %d, game over", startId)
		return
	}

	tail := head

	// let tail go to last position
	for {
		if tail.NextNode == head {
			break
		}
		tail = tail.NextNode
	}

	// let head go to BNode which have startId
	for i := 0; i < startId-1; i++ {
		head = head.NextNode
		tail = tail.NextNode
	}

	for {
		// count...
		for i := 0; i < countNum-1; i++ {
			head = head.NextNode
			tail = tail.NextNode
		}

		fmt.Printf("node %d leave away\n", head.Id)
		// delete boy
		head = head.NextNode
		tail.NextNode = head

		// only one boy here
		if head == tail {
			fmt.Printf("remain node %d\n", head.Id)
			return
		}
	}
}
func main() {
	head, err := AddBoy(5)
	if err != nil {
		fmt.Println(err.Error())
	}
	ShowAllBNode(head)
	fmt.Println()
	PlayGame(head, 2, 3)
}

```
