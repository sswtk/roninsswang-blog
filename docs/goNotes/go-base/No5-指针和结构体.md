## 指针和结构体

> @CreateTime: 2021年09月27日00:38:41
>
> @UpdateTime: 2021年09月27日00:38:41



## 指针

### 指针的理解

- 指针代表某个内存地址，默认值为nil,
- 使用&取变量地址，通过*访问目标对象。
- Go中直接使用`.`访问目标成员
- go中指针不支持运算（由于垃圾回收机制的存在，对指针运算造成了许多困扰）
- 支持多级指针，如 `**T`,  
- 空指针：声明但未初始化的指针
- 野指针：引用了无效地址的指针，如：`var p *int = 0`，`var p *int = 0xff00`(超出范围)

### 指针的创建

```go
package main

import "fmt"

//创建指针
func main() {
  var a int = 10
	fmt.Println("&a=", &a)			// 0xc000096008 一个十六进制数

	var p *int = &a
	fmt.Println("*p=", *p)			// 10
}

//实现变量值交换
func swap(p1,p2 *int) {
	*p1,*p2 = *p2,*p1
	}
```

### 结构体指针

```go
package main

import "fmt"

type User struct{
		name string
		age int
	}

func main() {
  var u = User{
    name:"lisi",
    age: 18,
  }
  p := &u
  fmt.Println(u.name)		//输出李四
  fmt.Println(p.name)		//输出李四
}
```

### new函数的使用

- new()函数可以在 heap堆 区申请一片内存地址空间：

```go
package main

import "fmt"

func main() {
	var p *bool
	p = new(bool)
	fmt.Println(*p)		// false
}
```

---

## 结构体

### 结构体的理解

- 结构体可以用来声明新的类型，作为其他类型的属性/字段的容器
- 结构体中的类型可以是任意类型，且存储空间是连续的，其字段按照声明时的顺序存放
- 如果结构体的所有的成员都是可以比较的， 那么结构体本身也是可以比较的，使用 == != ，不支持 > 和 <
- 如果结构体的成员要被包外调用，需要首字母大写
- 被new生成的结构体实例其实是指针类型
- 对结构体进行`&`取地址操作时，也可以视为对该类型进行一次`new`的实例化操作。

### 结构体的创建和使用

#### 基础

```go
type Person struct {
	name string
	age int
}

//按顺序初始化：每个成员都必须初始化
var p1 Person = Person{"lisi", 20}

//制定成员初始化：没有被初始化的，自动赋零值
p2 := Person{age:30}
	
// new 申请结构体
p3 := new(Person)      //被new生成的结构体实例其实是指针类型
p3.name = "zs"          //这里的.语法只是语法糖，将p3.name转换成了(*p3).name
p3.age = 27
	
//直接声明
var s4 Person
p4.name = "ww"
p4.age = 30

//结构体实例化
ins := &T{}   //T是结构体类型 ,ins为结构体的实例，类型为*T，是指针类型
```



#### 内嵌结构体

- 当前结构体可以直接访问其内嵌结构体的内部字段

```go
package main

import "fmt"

type Animal struct {
	Age int
}

type Person struct {
	Animal
	Name string
}

type Student struct {
	Person
	ClassName string
}

func main() {

	// 初始化方式1
	s1 := Student{
		Person{
			Animal: Animal {
				Age: 15,
			},
			Name:"lisi",
		},
		"一班",
	}
	fmt.Println(s1.Age)				// 正确输出15
	fmt.Println(s1.Person.Name)		// 正确输出lisi

	// 初始化方式2
	var s2 Student
	s2.Name = "zs"
	s2.Age = 30
	s2.ClassName = "二班"
	fmt.Println(s2.Age)				// 正确输出30
	fmt.Println(s2.Person.Name)		// 正确输出zs
}
```



#### 匿名字段

- 结构体的字段名与类型一一对应，如果不提供名字，则为匿名字段。
- 匿名字段如果是一个struct，这个struct拥有的全部字段都被隐式引入了当前的struct。
- 不仅仅是struct，其他所有内置类型和自定义类型都可以作为匿名字段：

```go
package main

import "fmt"

type Person struct {
    name string
    age int
}
    
type course []string

type Student struct {
    Person                   // 匿名字段，那么默认Student就包含了Person的所有字段
    course                   // 内置一个切片类型
    classroom string
}

func main() {

	// 创建一个学生
	s := Student{Person:Person{"LiLei", 17}, classroom:"二班"}

	// 访问该学生字段
	fmt.Println("name = ", s.name)
	fmt.Println("classroom =  ", s.classroom)

	// 修改学生的课程
	s.course = []string{"语文", "美术"}
	fmt.Println("course = ", s.course)			// [语文 美术]
}

// 如果Person和Student中都有同一个字段，那么Go会优先访问当前层。例如二者都有tel字段，那么s.tel将会访问的是Student中的数据。
```



