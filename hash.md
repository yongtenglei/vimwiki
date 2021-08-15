### HashTable 散列表

`示意图`

![SketchMap](https://raw.githubusercontent.com/yongtenglei/draw/main/draw/empHashTable.png)
```go
package main

import (
  "errors"
  "fmt"
  "log"
)

/*=========const===========*/
const (
  TABLE_SIZE int = 7
)

/*=========Emp===========*/

type Emp struct {
  Id      int
  Name    string
  NextEmp *Emp
}

func (e *Emp) ShowMe() {
  fmt.Printf("employee: [id: %d, name: %s]\n", e.Id, e.Name)
}

/*=========EmpLink===========*/

type EmpLink struct {
  Head *Emp
}

func (l *EmpLink) AddEmpById(emp *Emp) error {
  // just like singleLink.go
  current := l.Head

  if current == nil {
    l.Head = emp
    return nil
  }
  // 第一个节点先比较, 符合条件改变EmpLink的Head
  if current.Id > emp.Id {
    emp.NextEmp = current
    l.Head = emp
    return nil
  } else if current.Id == emp.Id {
    return errors.New("cannot insert newNode cause the same id")
  }

  // 从第二个节点比较
  for {
    if current.NextEmp == nil {
      break
    } else if emp.NextEmp.Id > emp.Id {
      // nice position
      break
    } else if emp.NextEmp.Id == emp.Id {
      return errors.New("cannot insert newNode cause the same id")
    }
    current = current.NextEmp
  }

  emp.NextEmp = current.NextEmp
  current.NextEmp = emp
  return nil
}

func (t *EmpLink) FindEmpById(id int) (*Emp, error) {
  current := t.Head

  for {
    if current == nil {
      return nil, errors.New("cannot find such a employee")
    }

    if current.Id == id {
      return current, nil
    }

    current = current.NextEmp
  }
}

func (t *EmpLink) ShowEmpById(id int) {
  current := t.Head

  for {
    if current == nil {
      return
    }

    if current.Id == id {
      fmt.Printf("employee: [id: %d, name: %s]\n", current.Id, current.Name)
      return
    }

    current = current.NextEmp
  }
}

func (t *EmpLink) ShowAllEmp(no int) {
  if t.Head == nil {
    fmt.Printf("Link %d is empty\n", no)
    return
  }

  fmt.Printf("link %d: ", no)
  current := t.Head
  for {
    if current == nil {
      break
    } else {
      fmt.Printf("[id: %d, name: %s] ==> ", current.Id, current.Name)
      current = current.NextEmp
    }
  }
  fmt.Println()
}

/*=========HashTable===========*/

type HashTable struct {
  LinkTable [TABLE_SIZE]EmpLink
}

// AddEmp 使用散列函数决定雇员被添加到哪个链表
func (t *HashTable) AddEmp(emp *Emp) error {

  linkNo := t.HashFunc(emp.Id)
  err := t.LinkTable[linkNo].AddEmpById(emp)
  return err
}

func (t *HashTable) FindEmpById(id int) (*Emp, error) {
  linkNo := t.HashFunc(id)
  emp, err := t.LinkTable[linkNo].FindEmpById(id)
  if err != nil {
    log.Println("FindEmpById failed, return nil by default")
    return nil, err
  }
  return emp, nil
}

func (t *HashTable) ShowEmpById(id int) {
  linkNo := t.HashFunc(id)
  t.LinkTable[linkNo].ShowEmpById(id)
}

func (t *HashTable) ShowAllEmp() {
  for i := 0; i < len(t.LinkTable); i++ {
    t.LinkTable[i].ShowAllEmp(i)
  }
}

func (t *HashTable) HashFunc(id int) int {
  return id % TABLE_SIZE
}

func main() {
  hashtable := &HashTable{}

  var flag = true
  for flag {
    fmt.Println("===========1 for add========")
    fmt.Println("===========2 for find a person========")
    fmt.Println("===========3 for show all employee========")
    fmt.Println("===========4 for exit========")
    //fmt.Println("===========1 for add========")

    fmt.Println("your chose: ")
    var key int
    fmt.Scanln(&key)
    var (
      id   int
      name string
    )

    switch key {
    case 1:

      fmt.Println("id: ")
      fmt.Scanln(&id)
      fmt.Println("name: ")
      fmt.Scanln(&name)

      emp := &Emp{
        Id:   id,
        Name: name,
      }

      err := hashtable.AddEmp(emp)
      if err != nil {
        log.Println(err)
      } else {
        fmt.Println("add employee successfully")
      }

    case 2:
      fmt.Println("id: ")
      fmt.Scanln(&id)

      hashtable.ShowEmpById(id)

      // or
      /*emp, err := hashtable.FindEmpById(id)
        if err != nil {
          fmt.Println(err.Error())
        } else {
          emp.ShowMe()
        }
      */

    case 3:
      hashtable.ShowAllEmp()

    case 4:
      flag = false
    }

  }
}

```





