### Binary Tree

* `Pre-Order Traversal: root -> left -> right`
* `In-Order Traversal: left -> root -> right`
* `Post-Order Traversal: left -> right -> root`

```go
package main

import "fmt"

type BNode struct {
  Id    int
  Name  string
  Left  *BNode
  Right *BNode
}

func PreOrderTraversal(root *BNode) {
  //if root == nil {
  //  fmt.Println("Empty tree")
  //  return
  //}
  if root != nil {
    fmt.Printf("[id: %d, name: %s]\n", root.Id, root.Name)
    PreOrderTraversal(root.Left)
    PreOrderTraversal(root.Right)
  }
}

func InOrderTraversal(root *BNode) {
  if root != nil {
    PreOrderTraversal(root.Left)
    fmt.Printf("[id: %d, name: %s]\n", root.Id, root.Name)
    PreOrderTraversal(root.Right)
  }
}

func PostOrderTraversal(root *BNode) {
  if root != nil {
    PreOrderTraversal(root.Left)
    PreOrderTraversal(root.Right)
    fmt.Printf("[id: %d, name: %s]\n", root.Id, root.Name)
  }
}

func main() {
  node1 := &BNode{
    Id:   1,
    Name: "rey",
  }

  node2 := &BNode{
    Id:   2,
    Name: "charlotte",
  }

  node3 := &BNode{
    Id:   3,
    Name: "martin",
  }
  node4 := &BNode{
    Id:   4,
    Name: "dora",
  }

  node5 := &BNode{
    Id:   5,
    Name: "lucy",
  }

  node6 := &BNode{
    Id:   6,
    Name: "linus",
  }

  node1.Left = node2
  node1.Right = node3
  node2.Left = node4
  node2.Right = node5
  node3.Right = node6
  fmt.Println("==========PreOrder===========")
  PreOrderTraversal(node1)
  fmt.Println("==========InOrder===========")
  InOrderTraversal(node1)
  fmt.Println("==========PostOrder===========")
  PostOrderTraversal(node1)

}

```



