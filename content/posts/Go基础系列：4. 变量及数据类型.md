---  
title: Go基础系列：4. 变量及数据类型  
date: 2021-07-10T18:18:56+08:00  
share: true  
categories:  
  - Golang  
keywords:  
  - Go  
  - 变量  
  - 数据类型  
description: go语言变量，go语言数据类型  
series:  
  - Go基础系列  
slug: golang-var  
cover:  
  image: https://images.unsplash.com/photo-1488229297570-58520851e868?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHw3fHxkYXRhfGVufDB8MHx8fDE3MDMzMDE4ODF8MA&ixlib=rb-4.0.3&q=80&w=720  
---  
  
  

  

  
## 变量
  

  
### 1. 声明
  

  
变量的声明使用 var 关键字，格式：**var 名称 类型**。特别强调下，Go 语法每行末尾是**没有分号**的。
  

  
```go
  
var a int
  
```
  

  
如果存在多个变量类型相同时，可以逗号分割排列。
  

  
```go
  
var a, b int
  
```
  

  
声明一组不同类型的变量，使用小括号包裹住。
  

  
```go
  
 var (
  
	a int
  
	b bool
  
	str string
  
)
  
```
  

  
当一个变量被声明后，系统会自动赋予零值，int 为 0、bool 为 false、string 为空字符串、float 为 0.0，指针为 nil。
  

  
### 2. 初始化
  

  
变量被声明后，就可以赋值了。
  

  
```go
  
var a int
  
a = 1
  
```
  

  
但其实在声明变量的时候就可以直接赋值。
  

  
```go
  
var a int = 1
  
var b bool = true
  
var str string = "我最棒"
  
```
  

  
还可以再简单，因为 Go 编译器的智商很高了，在编译期间就可以推断变量的类型，那么就可以省略类型。
  

  
```go
  
// int
  
var a = 1
  
// bool
  
var b = true
  
// string
  
var str = "我最棒"
  
```
  

  
在实际开发中，默认推断的类型不是我们想要的时候，就可以显示声明类型。
  

  
**终极简写方式**，使用 := 赋值，省略了 var 关键字，这也是实际中最常用的，如下：
  

  
```go
  
// 单变量
  
a := 1
  
// 多变量
  
a, b, str := 1, true, "我最棒"
  
```
  

  
这种简写方式，只适用于函数体内，对于函数体外就只能用 var 关键字方式。
  

  
## 数据类型
  

  
### 1. 分类
  

  
- 基本数据类型：整型、浮点型、字符串、字符型、布尔型、复数。
  
- 容器数据类型：数组、切片 (slice)、通道 (chan)、映射 (map)。
  
- 其它数据类型：函数、结构体、接口。
  

  
本篇文章，只讲解基本数据类型，对于剩余的类型会在后续文章进行讲解。
  

  
### 2. 整型
  

  
- 无符号：uint、uint8，uint16，uint32，uint64
  
- 有符号：int、int8，int16，int32，int64
  

  
其中 int 和 uint 类型长度由操作系统类型决定，如果系统是 32 位的，那么它们都是 32 位（4 字节）；如果是 64 位的，那么它们都是 64 位（8 字节）。
  

  
还记得上面所说的，类型推断吗？对于整型数据 Go 编译器默认推断为 int 类型。如果想声明且初始化为其它类型，如下：
  

  
```go
  
var a int32 = 1
  
```
  

  
### 3. 浮点型
  

  
对于浮点类型，只有 float32 和 float64，没有 float 类型，使用**IEEE-754 标准**。
  

  
float32 精确到小数点后 7 位，float64 精确到小数点后 15 位。由于精度的问题，在进行数据比对的时候，就要考虑精度损失。如下（使用到了函数看不懂的话，后续会讲解）：
  

  
```go
  
// 比对两个数是否相等
  
func IsEqualFloat64(num1 float64, num2 float64) bool {
  
	return math.Abs(num1-num2) < 0.0000001
  
}
  
```
  

  
### 4. 字符串
  

  
字符串类型使用 string 声明，在初始化时分为单行和多行。单行使用双引号, 如下：
  

  
```go
  
str := "我最棒"
  
```
  

  
多行使用反引号，中间如果出现转义字符时，不会进行解析，原样输出，如下：
  

  
```go
  
// 文件名: string.go
  
package main
  

  
import "fmt"
  

  
func main() {
  
	str := `
  
你好，\n
  
Hello
  
`
  
	fmt.Println(str)
  
}
  
```
  

  
运行结果如下：
  

  
```bash
  
go run string.go
  

  
## 输出
  
你好，\n
  
Hello
  
```
  

  
### 5. 字符型
  

  
byte 和 rune 是字符型，但其实不是新增的类型，byte 是 uint8 的别名，占用 1 个字节，rune 是 int32 的别名，占用 4 个字节。
  

  
```go
  
// 对应ASCII码表中的97
  
var a byte = 'a'
  
或
  
var a uint8 = 'a'
  

  
var b rune = '嗨'
  
或
  
var b int32 = '嗨'
  
```
  

  
byte 代表了 ASCII 码的一个字符，rune 代表了 UTF-8 的一个字符。
  

  
### 6. 布尔型
  

  
布尔型使用 bool 声明，要么是 true, 要么是 false, 默认是 false。
  

  
```go
  
a := true
  
```
  

  
### 7. 复数
  

  
复数类型使用 complex64 和 complex128 声明，complex64 是 32 位实数和虚数，complex128 是 64 位实数和虚数。
  

  
```go
  
var c1 complex64 = 5 + 10i
  
或
  
c1 := 5 + 10i
  
```
  

  
在上面代码中，5 为实数部分，10 为虚数部分，至于在实际中怎么用，就看你项目中有牵扯到相关数学运算的没，对于这方面我也几乎忘干净了。如果真的需要进一步学习，咱可以一起学习讨论。
  

  
## 总结
  

  
本篇对 Go 语言中变量的声明、变量的初始化、基本数据类型有了一个整体的认识，但可能还会存在一些困惑问题，比如:
  

  
- 如何保留小数位和四舍五入
  
- 类型之间的转化问题
  
- 值类型和引用类型
  
- 等等
  

  
这些问题都会在后续的文章一点点解开，如果着急的想知道，可以在下方留言！