# No3-基本数据类型

> Note-Time: 2021年09月17日01:56:28



## 数据类型分类概述

### 基本类型

|     类型     |   名称   | 长度 | 零值  |                     说明                      |
| :----------: | :------: | :--: | :---: | :-------------------------------------------: |
|    `bool`    | 布尔类型 |  1   | false | 其值不为真即为假，不可以用数字代表true或false |
|    `byte`    |  字节型  |  1   |   0   |                   uint8别名                   |
|     rune     | 字符类型 |  4   |   0   |      专用于存储unicode编码，等价于uint32      |
| `init,uint`  |   整型   | 4或8 |   0   |                  23位或64位                   |
|  int8,uint8  |   整型   |  1   |   0   |                -128-127,0-255                 |
| int16,uint16 |   整型   |  2   |   0   |             -32768-32768,0-65535              |
| int32,uint32 |   整型   |  4   |   0   |              -21亿-21亿，0-42亿               |
| int64,uint64 |   整型   |  8   |   0   |                                               |
|  `float32`   |  浮点型  |  4   |  0.0  |                小数位精确到7位                |
|  `float64`   |  浮点型  |  8   |  0.0  |               小数位精确到15位                |
|  complex64   | 复数类型 |  8   |       |                                               |
|  complex128  | 复数类型 |  16  |       |                                               |
|   unitptr    |   整型   | 4或8 |       |       足以存储指针的uint32或uint64整数        |
|   `string`   |  字符串  |      |  ""   |                  utf-8字符串                  |

### 值类型

- array(数组) 、 struct（结构体）、 string

### 引用类型

- slice (切片)、map(字典)，chan

### 接口类型

- interface

### 函数类型

- func

---



## 基本类型详述

###  bool —布尔

- 布尔类型只可以是常量true或false
- 数字类型和bool类型不能相互转换

```go
package main

import "fmt"

func main() {
  //bool
	var w bool // 申声明变量，如果没有初始化，默认为false
	var ww bool = true
	var www = true //自动推断类型
	fmt.Println(w, ww, www)
	a := true
	b := false
	fmt.Println(a, b)
	c := a && b
	fmt.Println(c)
	d := a || b
	fmt.Println(d)
}
```



---

### string—字符串

#### string-概述

- 字符串是字节的集合，字符串是字节的定长数组。
- 字符串是一种值类型，且值不可变，即创建某个文本后将无法再次修改这个文本的内容
- 字符串是 UTF-8 字符的一个序列（当字符为 ASCII 码表上的字符时则占用 1 个字节，其它字符根据需要占用 2-4 个字节）
- 内置函数len()返回的是字节数而不是字符数。
- 字符串是用一对双引号"或反引号``(键盘数字1的左边键)括起来定义

```go
package main

import "fmt"

func main() {
	//string
	var str1 string // 声明字符串类型
	str1 = "aaa"
	fmt.Println("str1=", str1)
	first := "Ronin"
	last := "ssWang"
	name := first + last
	fmt.Println("My name is", name)
	fmt.Printf("name 的类型是 %T\n", name)
	
	// len()，内建函数，可以返回字符串有多少个字符
	fmt.Println("len(name)=", len(name))  //len(name)=11
  //如果只想操作某个字符，可用如下方式
	fmt.Printf("name[0] = %c, name[1] = %c", name[0], name[1])
}

```

#### string的常用操作

##### 包含

```go
Contains(s, substr string) bool   包含子字符串
ContainsAny(s, chars string) bool  任意点码值是否s中出现
ContainsRune(s string, r rune) bool r unicode值是否s中出现
Count(s, sep string) int   sep 子字符串出现的次数
EqualFold(s, t string) bool   比较字符串相等忽略大小写
HasPrefix(s, prefix string) bool 是否有前缀
HasSuffix(s, suffix string) bool 是否有后缀

fmt.Println(strings.Contains("seafood", "foo"))//true
fmt.Println(strings.Contains("seafood", "bar"))//false
fmt.Println(strings.Contains("seafood", ""))//true
fmt.Println(strings.Contains("", ""))//true
fmt.Println(strings.ContainsAny("test",""))//false
fmt.Println(strings.ContainsAny("test","tr"))//true
fmt.Println(strings.Count("test", "t"))//2
```



---

##### 位置

```go
Index(s, sep string) int  返回第一个sep在s中的位置
IndexAny(s, chars string) int  返回chars中unicode码点在s中第一个所在的位置
IndexFunc(s string, f func(rune) bool) int 返回s 中unicode码点满足函数f的位置
IndexByte(s string, c byte) int 返回第一个c byte在s中出现的位置
IndexRune(s string, r rune) int 返回第一个r unicode在s中出现的位置
LastIndex(s, sep string) int
LastIndexAny(s, chars string)
LastIndexFunc(s string, f func(rune)bool)int
```

---

##### 过滤

```go
Trim(s string, cutset string) string 从两端过滤包含cutset中码点值
TrimFunc(s string, f func(rune) bool)string从两端过滤满足f的码点值
TrimLeft(s, string, cutset s string) string
TrimLeftFunc(s string, f func(rune) bool)string
TrimRight(s, string, cutset s string) string
TrimRightFunc(s string, f func(rune) bool)string
TrimSpace(s string) string 从两端过滤空白字符和空格
```

---

##### 替换

```go
Map(mapping func(rune) rune, s string) string 根据mapping函数替换里面每个rune
NewReplacer(oldnew …string) 创建一个替换器对象
Replace(s, old, new string, n int) string 把old 替换为new
```

---

##### 大小写

```go
Title(s string) string 对s中每一个单词进行标题首字母大写
ToTitle(s string) string  得到s的标题格式
ToLower(s string) string  得到小写
ToLowerSpeical(case unicode.SpecialCase, s string) string 针对特殊的编码格式小写
ToUpper(s string) string
ToUpperSpeical(case unicode.SpecialCase, s string) string
```

---

##### 分割

```go
Fields(s string) []string  对字符串按空白进行分割
FieldsFunc(s string, f func(rune) bool) 对满足f的函数进行切割
Split(s, sep string) []string  以sep对字符串s进行分割
SplitN(s, sep string, n int)[] string 以sep对字符串s进行分割成几部分
SplitAfter(s, sep string) [] string 
SplitAfterN(s, sep string, n int)[] string
TrimPrefix(s, prefix string) string 去掉前缀
TrimSuffix(s, suffix string) string  去掉后缀
```

---

##### 合并

```go
Join(a []string, sep string) string用分割符sep合并a
NewReader(s string) *Reader 创建一个字符串对象
Repeat(s string, count int) string 新生成一个s重复几次的字符串
```

---

##### 转换

- 字符串转化的函数在strconv中，如下也只是列出一些常用的：
  - Append 系列函数将整数等转换为字符串后，添加到现有的字节数组中。

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	str := make([]byte, 0, 100)
	str = strconv.AppendInt(str, 4567, 10)
	str = strconv.AppendBool(str, false)
	str = strconv.AppendQuote(str, "abcdefg")
	str = strconv.AppendQuoteRune(str, '单')
	fmt.Println(string(str))
}
```

---

- Format 系列函数把其他类型的转换为字符串

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	a := strconv.FormatBool(false)
	b := strconv.FormatFloat(123.23, 'g', 12, 64)
	c := strconv.FormatInt(1234, 10)
	d := strconv.FormatUint(12345, 10)
	e := strconv.Itoa(1023)
	fmt.Println(a, b, c, d, e)
}
```

---

- Parse 系列函数把字符串转换为其他类型

```go
package main

import (
	"fmt"
	"strconv"
)
func checkError(e error){
	if e != nil{
		fmt.Println(e)
	}
}
func main() {
	a, err := strconv.ParseBool("false")
	checkError(err)
	b, err := strconv.ParseFloat("123.23", 64)
	checkError(err)
	c, err := strconv.ParseInt("1234", 10, 64)
	checkError(err)
	d, err := strconv.ParseUint("12345", 10, 64)
	checkError(err)
	e, err := strconv.Atoi("1023")
	checkError(err)
	fmt.Println(a, b, c, d, e)
}
```

---

##### 遍历

- range 在字符串中迭代 unicode 编码。第一个返回值是rune 的起始字节位置，然后第二个是 rune 自己。

```go
for index,value := range "123ABCabc"{
   	 fmt.Println(index,value)
	}
```

---

#### byte—字符

```go
package main

import "fmt"

func main() {
	//byte
	var n byte
	n = 87
	fmt.Printf("%c, %d\n", n, n)  //%c:以字符的形式打印，%d:以整型方式打印
	var ch byte
	ch = 'a'  // 单引号表示字符， 双引号表示字符串
	fmt.Printf("%c, %d\n", ch, ch) 
}

```

---

#### rune

- rune默认的字符编码是utf-8类型的
- rune内置两种字符类型
  - byte字节类型（byte是uint的别名）
  - 表示Unicode编码的字符rune, 在go内部是int32类型的别名，占用4个字节

```go
package main

import "fmt"

func main() {
	//rune
	str := "nihao你好"
	fmt.Println(len(str))
	k := []byte(str)
	for _, v := range k {
		fmt.Print(string(v)) //nihaoä½ å¥½
	}
	fmt.Println("")
	r := []rune(str)
	for _, v := range r {
		fmt.Print(string(v)) // nihao你好
	}
	fmt.Println("")
	r[0] = 'N'
	r[5] = '您'
	fmt.Println(string(r)) // Nihao您好
}

```

---

#### float—浮点型

```go
package main

import "fmt"

func main() {
	// float
	var y float32
	y = 3.3
	yy := 6.555 // 自动推导类型，默认是float64,要比float32更精确
	fmt.Println("y=", y)
	fmt.Printf("yy的类型为 %T", yy) //yy的类型为 float64
}
```

---

#### complex—复数类型

```go
package main

import "fmt"

func main() {
	//complex
	var t complex128
	t = 2.1 + 3.14i
	fmt.Println("t=", t)
	t1 := 3.3 + 5.5i
	fmt.Printf("t1的类型是%T\n", t1)
	// 通过内建函数，取出实部和虚部
	fmt.Println("real(t1)=", real(t1), "imag(t1)=", imag(t1)) //real(t1)= 3.3 imag(t1)= 5.5

}
```

---

### 常用类型转换

```go
package main

import "fmt"

func main() {
	//不能转换的类型，叫不兼容类型，先举一个不能转换的例子
	var flag bool
	flag = true
	fmt.Printf("flag = %t\n", flag)

	//bool类型不能转换为int
	//fmt.Printf("flag = %d\n", int(flag))

	// 0就是假，非0就是真
	// 整型也不能转换为bool
	// flag = bool(1)

	//再举一个能够转换的例子。

	var ch byte
	ch = 'a' //字符类型本质上就是整型
	var t int
	t = int(ch) //类型转换，表示把ch的值取出来之后，转成int再赋给t
	fmt.Println("t =", t)
}
```

---

### 给类型取别名

```go
package main

import "fmt"

func main() {
	//给int64起一个别名bigint，别名的关键字是type
	type bigint int64
	var a bigint //等价于 var a int64
	fmt.Printf("a type is %T\n", a)

	//也可以一次性起多个别名
	type (
		long int64
		char byte
	)
	var b long = 11
	var c char = 'a'
	fmt.Printf("b = %d, c = %d\n", b, c)
}
```

---

