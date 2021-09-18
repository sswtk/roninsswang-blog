## 基本数据结构

> Note-Time: 2021年09月18日01:57:14



## Array—数组

### 定义

数组是一个由固定长度的元素类型相同组成的有序集合，一个数组由零或者多个元素组成。其中n是数组长度， elementType是数组元素类型。

```go
[n]elmentType
```



#### 默认定义

数组的声明需要存储元素的长度值和存储数据的类型。其表示形式为 `[n]T`。`n` 表示数组中元素的长度值，`T` 代表每个元素的类型，数组的长度在声明的时候一定要给定，而数组一旦声明，其长度和类型都不能改变。

```
var <数组名称> [<数组长度>]<元素类型>
```

eg:

```go
package main

import (
    "fmt"
)

func main() {
    var a [3]int //int array with length 3
    fmt.Println(a)
}
```



var a[3]int 声明了一个长度为 3 的整型数组。数组中的所有元素都被自动赋值为数组类型的零值。在这种情况下，`a` 是一个整型数组，因此 `a` 的所有元素都被赋值为 `0`，即 int 型的零值。运行上述程序将输出 `[0 0 0]`。



#### 指定元素

初始化元素通过大括号包裹起来。

```go
var <数组名称> = [<数组长度>]<元素类型>{元素1,元素2,...}
```



eg:



```go
package main

import (
    "fmt"
)

func main() {
    a := [3]int{1, 2, 5} 
    fmt.Println(a)
}
```



在简略声明中，不需要将数组中所有的元素赋值。



```go
package main

import (
    "fmt"
)

func main() {
    a := [3]int{10} 
    fmt.Println(a)
}
```



在上述程序中的第 8 行 `a := [3]int{10}` 声明一个长度为 3 的数组，但只提供了一个值 `10`，剩下的 2 个元素自动赋值为 `0`。这个程序将输出`[10 0 0]`。

#### 省略号

数组的长度可以使用省略号 `...` 代替，这个并不是代表可以省略数组长度的声明，编译器会自动计算数组长度，它的长度值等于当前数组初始化元素的个数。



```go
package main

import (
    "fmt"
)

func main() {
    a := [...]int{12, 78, 50} // ... makes the compiler determine the length
    fmt.Println(a)
}
```



#### 索引定义

```go
[<数组长度>]<元素类型>{索引1:元素1,索引2:元素2,…}
```



```go
const (
	FIRST int = iota
	SECOND
	THIRD
	FOURTH
)

func main() {
    arr :=[4]string{FIRST:"1",SECOND:"2",THIRD:"3",FOURTH:"4"}
    fmt.Println(arr)
}
//输出[1 2 3 4]
```



### 比较

同样类型的数组是可以相互赋值的，相同类型的数组必须是长度一样，并且每个元素的类型也一样的数组，因此数组的大小是类型的一部分。因此 `[5]int` 和 `[25]int` 是不同类型。



```go
package main

func main() {
    a := [3]int{5, 78, 8}
    var b [5]int
    b = a 
}
```



类型 `[3]int` 的变量赋给类型为 `[5]int` 的变量，这是不允许的，因此编译器将抛出错误 main.go:6: cannot use a (type [3]int) as type [5]int in assignment。



### 值类型

注意Go 中的数组是值类型而不是引用类型。

### 数组赋值

当数组赋值给一个新的变量时，该变量会得到一个原始数组的一个副本。如果对新变量进行更改，则不会影响原始数组。



```go
package main

import "fmt"

func main() {
    a := [...]string{"USA", "China", "India", "Germany", "France"}
    b := a 
    b[0] = "Singapore"
    fmt.Println("a is ", a)
    fmt.Println("b is ", b) 
}
```



在上述程序的第 7 行，`a` 的副本被赋给 `b`。在第 8 行中，`b` 的第一个元素改为 `Singapore`。这不会在原始数组 `a` 中反映出来。该程序将 输出



```go
a is [USA China India Germany France]  
b is [Singapore China India Germany France]
```



### 函数参数传递

同样，当数组作为参数传递给函数时，它们是按值传递，而原始数组保持不变。



```go
package main

import "fmt"

func main() {
	array := [3]int{1:2}
	modify(array)
	fmt.Println("current list",array)
}

func modify(a [3]int){
	a[1] =3
	fmt.Println("modify list",a)
}
```



执行上面程序结果

```go
modify list [0 3 0]
current list [0 2 0]
```



传递数组的指针，可以实现修改数组值的

```go
package main

import "fmt"

func main() {
	array := [3]int{1:2}
	modify(&array)
	fmt.Println("current list",array)
}

func modify(a *[3]int){
	a[1] =3
	fmt.Println("modify list",*a)
}
```



### 长度

通过将数组作为参数传递给 `len` 函数，可以得到数组的长度。



```go
package main

import "fmt"

func main() {
    a := [...]float64{67.7, 89.8, 21, 78}
    fmt.Println("length of a is",len(a))
}
```



上面的程序输出为 `length of a is 4`。



### 迭代

`for` 循环可用于遍历数组中的元素。



```go
package main

import "fmt"

func main() {
    a := [...]float64{67.7, 89.8, 21, 78}
    for i := 0; i < len(a); i++ { // looping from 0 to the length of the array
        fmt.Printf("%d th element of a is %.2f\n", i, a[i])
    }
}
```



上面的程序使用 `for` 循环遍历数组中的元素，从索引 `0` 到 `length of the array - 1`。这个程序运行后打印出，



```go
0 th element of a is 67.70  
1 th element of a is 89.80  
2 th element of a is 21.00  
3 th element of a is 78.00
```



Go 提供了一种更好、更简洁的方法，通过使用 `for` 循环的 **range** 方法来遍历数组。`range` 返回索引和该索引处的值。让我们使用 range 重写上面的代码。我们还可以获取数组中所有元素的总和。



```go
package main

import "fmt"

func main() {
    a := [...]float64{67.7, 89.8, 21, 78}
    sum := float64(0)
    for i, v := range a {//range returns both the index and value
        fmt.Printf("%d the element of a is %.2f\n", i, v)
        sum += v
    }
    fmt.Println("\nsum of all elements of a",sum)
}
```



上述程序的第 8 行 `for i, v := range a` 利用的是 for 循环 range 方式。 它将返回索引和该索引处的值。 我们打印这些值，并计算数组 `a` 中所有元素的总和。 程序的 **输出是**，



```go
0 the element of a is 67.70
1 the element of a is 89.80
2 the element of a is 21.00
3 the element of a is 78.00

sum of all elements of a 256.5
```



如果你只需要值并希望忽略索引，则可以通过用 `_` 空白标识符替换索引来执行。



```go
for _, v := range a { // ignores index  
}
```



上面的 for 循环忽略索引，同样值也可以被忽略。



### 多维数组

到目前为止我们创建的数组都是一维的，Go 语言可以创建多维数组。



```go
package main

import (
    "fmt"
)

func printarray(a [3][2]string) {
    for _, v1 := range a {
        for _, v2 := range v1 {
            fmt.Printf("%s ", v2)
        }
        fmt.Printf("\n")
    }
}

func main() {
    a := [3][2]string{
        {"lion", "tiger"},
        {"cat", "dog"},
        {"pigeon", "peacock"}, // this comma is necessary. The compiler will complain if you omit this comma
    }
    printarray(a)
    var b [3][2]string
    b[0][0] = "apple"
    b[0][1] = "samsung"
    b[1][0] = "microsoft"
    b[1][1] = "google"
    b[2][0] = "AT&T"
    b[2][1] = "T-Mobile"
    fmt.Printf("\n")
    printarray(b)
}
```



在上述程序的第 17 行，用简略语法声明一个二维字符串数组 a 。20 行末尾的逗号是必需的。这是因为根据 Go 语言的规则自动插入分号。
另外一个二维数组 b 在 23 行声明，字符串通过每个索引一个一个添加。这是另一种初始化二维数组的方法。
第 7 行的 printarray 函数使用两个 range 循环来打印二维数组的内容。上述程序的 输出是



```go
lion tiger
cat dog
pigeon peacock

apple samsung
microsoft google
AT&T T-Mobile
```



这就是数组，尽管数组看上去似乎足够灵活，但是它们具有固定长度的限制，不可能增加数组的长度。这就要用到 切片。

### 总结

1.数组是同一类型元素的集合，因此存储的数据类似必须是相同的

2.数组是有固定长度的，因此需要初始化数组时声明长度。

3.长度一样且每个元素的类型和值也一样的数组，才能相等。

4.数组属于值类型的，即将一个数组赋值给另外一个数组的时候，实际上就是将整个数组拷贝一份。

---





## Slice—切片

> Note-Time: 2021年09月19日01:02:22

### 定义

- slice是对数组的一个连续片段的引用，其本身并不是数组，它指向底层的数组。
- slice是一个引用类型。
- slice默认指向一段连续的内存区域，可以是数组，也可以是slice本身。
- 如果多个slice指向相同的底层数组，其中一个的值改变了会影响全部。
- 数组是值传递，slice是引用传递

> Note: 
>
> - 使用make()创建， 
> - 使用len()获取元素个数，
> - cap()获取容量,一般从slice的开始位置直至底层数组的结束位置

### slice的结构体格式：

```go
type IntSlice struct {
  ptr       *int
  len, cap  int
}
```

### 创建的slice的方式

#### 方式一:

- var 声明一个新slice,未被初始化的slice 为nil

```go
var identifier []type
//fg:
// 声明字符串切片
var strList []string
// 声明整型切片
var numList []int
// 声明一个空切片
var numListEmpty = []int{}
```

#### 方式二：

- make,动态创建切片

```go
//make( []Type, size, cap )  
init_slice := make ([]string, cap)  //cap 为预分配的元素数量，这个值设定后不影响 size，只是提前分配空间，降低多次分配空间造成性能问题
//fg:
tmp_slice1 := make([]string, 4)
tmp_slice2 := make([]int, 2, 10)
```

#### 方式三：

- array——>slice

```go
// slice [开始位置 : 结束位置]
var names = [3]string{"bobo", "jake", "tom"}
subName := names[1:2]  // subName就是一个slice

```

#### 方式四:

- new

```go
subName1 := new([]string)
```

### 常用操作

#### append添加元素

- 如果最终长度未超过追加到slice的容量则返回原始slice，如果超过追加到的slice的容量则将重新分配内容地址并拷贝原始数据。
- 如果空间不足以容纳足够多的元素，切片就会进行“扩容”，此时新切片的长度会发生改变。容量的扩展规律是按容量的 2 倍数进行扩充，例如 1、2、4、8、16……
- 切片开头添加元素一般都会导致内存的重新分配，而且会导致已有元素全部被复制 1 次，因此，从切片的开头添加元素的性能要比从尾部追加元素的性能差很多。
- slice支持链式操作，可以将多个append操作组合起来

```go
//开头添加
var a = []int{1,2,3}
a = append([]int{0}, a...) // 在开头添加1个元素
a = append([]int{-3,-2,-1}, a...) // 在开头添加1个切片

//尾部添加
var a []int
a = append(a, 1) // 追加1个元素
a = append(a, 1, 2, 3) // 追加多个元素, 手写解包方式
a = append(a, []int{1,2,3}...) // 追加一个切片, 切片需要解包

//链式操作
var a []int
a = append(a[:i], append([]int{x}, a[i:]...)...) // 在第i个位置插入x
a = append(a[:i], append([]int{1,2,3}, a[i:]...)...) // 在第i个位置插入切片


//自动扩容
var numbers []int
for i := 0; i < 10; i++ {
    numbers = append(numbers, i)
    fmt.Printf("len: %d  cap: %d pointer: %p\n", len(numbers), cap(numbers), numbers)
}
// log
len: 1  cap: 1 pointer: 0xc0000ae2a0
len: 2  cap: 2 pointer: 0xc0000ae2b0
len: 3  cap: 4 pointer: 0xc0000c4020
len: 4  cap: 4 pointer: 0xc0000c4020
len: 5  cap: 8 pointer: 0xc0000b00c0
len: 6  cap: 8 pointer: 0xc0000b00c0
len: 7  cap: 8 pointer: 0xc0000b00c0
len: 8  cap: 8 pointer: 0xc0000b00c0
len: 9  cap: 16 pointer: 0xc0000c6000
len: 10  cap: 16 pointer: 0xc0000c6000

```

#### 删除元素

- go语音并并没有对删除slice元素提供专用的语法或者接口

- 如果有删除的需求，需要使用slice本身的特性来删除元素

- 删除slice元素的本质是：以被删除元素为分界点，将前后两个部分的内存重新连接起来

  ![](https://raw.githubusercontent.com/ronin-sswang/upload_notes_img/main/img/202109190051698.png)

```go
//从开头删除
a := []int{1, 2, 3}
 // 删除开头1个元素
a = a[1:] // [2,3]
a = append(a[:0], a[1:]...) // 删除开头1个元素
a = append(a[:0], a[N:]...) // 删除开头N个元素
//使用copy()函数删除开头的元素
a := []int{1, 2, 3}
a = a[:copy(a, a[1:])] // 删除开头1个元素
a = a[:copy(a, a[N:])] // 删除开头N个元素

//从中间位置开始删除
a = []int{1, 2, 3, ...}
a = append(a[:i], a[i+1:]...) // 删除中间1个元素
a = append(a[:i], a[i+N:]...) // 删除中间N个元素
a = a[:i+copy(a[i:], a[i+1:])] // 删除中间1个元素
a = a[:i+copy(a[i:], a[i+N:])] // 删除中间N个元素

//从尾部删除
a = []int{1, 2, 3}
a = a[:len(a)-1] // 删除尾部1个元素
a = a[:len(a)-N] // 删除尾部N个元素
```

#### copy()拷贝

- copy()可以将一个切片复制到另外一个切片中，返回值表示实际发生复制的元素个数
- 如果加入的两个切片不一样大，就会按照其中较小的那个切片的元素个数进行复制
- 目标切片必须分配过空间且足够承载复制的元素个数，并且来源和目标的类型必须一致

```go
// 将 srcSlice 复制到 destSlice
copy( destSlice, srcSlice []T) int

//切片大小不一致
slice1 := []int{1, 2, 3, 4, 5}
slice2 := []int{5, 4, 3}
copy(slice2, slice1) // 只会复制slice1的前3个元素到slice2中
fmt.Println(slice2)
copy(slice1, slice2) // 只会复制slice2的3个元素到slice1的前3个位置
fmt.Println(slice1)
// log
[1 2 3]
[1 2 3 4 5]

// 复制对原值影响
unc main() {
    // 设置元素数量为1000
    const elementCount = 1000
    // 预分配足够多的元素切片
    srcData := make([]int, elementCount)
    // 将切片赋值
    for i := 0; i < elementCount; i++ {
        srcData[i] = i
    }
    // 引用切片数据
    refData := srcData
    // 预分配足够多的元素切片
    copyData := make([]int, elementCount)
    // 将数据复制到新的切片空间中
    copy(copyData, srcData)
    // 修改原始数据的第一个元素
    srcData[0] = 999
    // 打印引用切片的第一个元素
    fmt.Println(refData[0])
    // 打印复制切片的第一个和最后一个元素
    fmt.Println(copyData[0], copyData[elementCount-1])
    // 复制原始数据从4到6(不包含)
    copy(copyData, srcData[4:6])
    for i := 0; i < 5; i++ {
        fmt.Printf("%d ", copyData[i])
    }
}
// log
999
0 999
4 5 2 3 4 

```

#### 多维slice

```go
//sliceName 为切片的名字
//sliceType为切片的类型，每个[]代表着一个维度，切片有几个维度就需要几个[]
var sliceName [][]...[]sliceType

// 声明一个二维整型切片并赋值
slice := [][]int{{10}, {100, 200}}
// 为第一个切片追加值为 20 的元素
slice[0] = append(slice[0], 20)
```





---







## Map—字典

---

