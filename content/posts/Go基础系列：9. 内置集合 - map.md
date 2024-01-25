---  
title: Go基础系列：9. 内置集合 - map  
date: 2021-07-16T17:21:56+08:00  
share: true  
keywords:  
  - go  
  - map  
description: go语言map的创建、遍历、删除键等知识讲解  
series:  
  - Go基础系列  
tags:  
  - program_golang  
slug: golang-map  
cover:  
  image: https://images.unsplash.com/photo-1547815749-838c83787de2?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHw4OHx8bWFwfGVufDB8MHx8fDE3MDMyNDkxODZ8MA&ixlib=rb-4.0.3&q=80&w=720  
---  
  
  
  
##  学到什么  
  
1. 什么是 map？  
2. 如何创建 map？  
3. 判断键是否存在？  
4. 如何获取 map 长度？  
5. 如何遍历 map？  
6. 如何删除键/值对？  
7. map 是引用类型还是值类型？  
  
##  概念  
  
map 是一种键 (key)/值 (value) 对的无序集合，在其它语言中称为字典、关联数组、哈希表等。当给定了键可以快速定位到值，而且**键必须唯一**的，不能出现相同。  
  
##  声明  
  
格式： `var 变量名 map[键类型][值类型]`   
  
举例：声明了一个键为 `int` 类型，值为 `string` 类型的 map。  
  
```go  
var dic map[int]string  
```  
  
注：未初始化，dic 为 nil。  
  
### 键类型限制  
  
声明 map 时，键不是所有类型都支持，它只支持可以使用 `!=` 或 `==` 操作符比较的类型。讲的这，也明白了在 **Go 语言中不是所有类型都可以进行比较**。  
  
哪些类型不能进行比较？  
  
- 函数  
- map  
- 切片  
- 元素是函数、map、切片的数组  
- 字段中包含函数、map、切片的结构体  
  
##  初始化  
  
初始化 map 有两种方式，第一种使用 `make` 函数，第二种是声明 map 时，初始化具体的键和值。如果 map 未初始化是不能存取值的，不然编译器报错。  
  
### 1. make 函数  
  
```go  
dic := make(map[int]string)  
```  
  
现在就可以存值了，初始化后的 map 会根据新增的键值动态伸缩，使用 `len` 函数获取长度。  
  
```go  
// collection/map-make.go  
  
dic := make(map[int]string)  
dic[1] = "lao"  
dic[3] = "miao"  
fmt.Println("dic长度:", len(dic))  
  
// 输出  
dic长度: 2  
```  
  
在初始化时，可以提前定义好 map 所需要的容量（空间大小），当**添加的键值超过容量时自动加一**。  
  
```go  
// collection/map-make-cap.go  
  
dic := make(map[int]string, 10)  
// 容量为 10 ，存了 1 个  
dic[1] = "lao"  
fmt.Println("dic长度:", len(dic))  
  
// 输出  
dic长度: 1  
```  
  
### 2. 声明时初始化  
  
声明了一个键为 `string` 类型, 值为 `int` 类型的 map，并初始化了 3 个键/值对。  
  
```go  
// collection/map-init.go  
  
m := map[string]int{  
	"a": 2,  
	"b": 3,  
	"c": 4,  
}  
fmt.Println("b:", m["b"])  
  
// 输出  
b: 3  
```  
  
初始化时也可以不指定键和值，这种情况和不指定容量的 `make` 函数是相同的。  
  
```go  
m := map[string]int{}  
  
等价于  
  
m := make(map[string]int)  
```  
  
##  键是否存在  
  
从初始化的 map 中获取一个没有存储的键时，编译器是不会报错的，它会返回值类型的默认值。例如：值类型是 int 就返回 0、值类型是 string 就返回空字符串。  
  
那怎么判断键是否存在呢？格式如下：  
  
```go  
v，ok := map[key]  
```  
  
- v：map 的值  
- ok：bool 类型，如果 key 存在为 true，反之为 false  
  
举例：  
  
```go  
// collection/map-key.go  
  
dic := map[int]string{}  
dic[0] = "a"  
  
if v, ok := dic[0]; ok {  
	fmt.Println(v)  
}  
  
// 输出  
a  
```  
  
##  删除键/值对  
  
使用 `delete` 函数可以删除 map 中的键/值对，格式如下：  
  
```go  
delete(map, 键)  
```  
  
注：如果键不存在时，编译器也可以通过。  
  
举例：  
  
```go  
m := map[string]int{  
	"a": 2,  
	"b": 3,  
	"c": 4,  
}  
delete(m, "b")  
fmt.Println(m)  
  
// 输出  
map[a:2 c:4]  
```  
  
##  遍历  
  
遍历 map 需要用到 for-range 语法，如果不懂先看看 [《流程控制》]({{< relref "Go%E5%9F%BA%E7%A1%80%E7%B3%BB%E5%88%97%EF%BC%9A6.%20%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6.md" >}})。  
  
```go  
m := map[string]int{  
	"a": 2,  
	"b": 3,  
	"c": 4,  
}  
// k 为键，v 为值  
for k, v := range m {  
	fmt.Println("key:", k, ",value:", v)  
}  
  
// 输出  
key: a ,value: 2  
key: b ,value: 3  
key: c ,value: 4  
```  
  
代码中 `v` 可以省略，这样表示只遍历键。如果只想遍历值，那 `k` 用 `_` （下划线）代替，表示省略。  
  
```go  
// 只遍历键  
for k := range m {  
	...  
}  
  
// 只遍历值  
for _, v := range m {  
	...  
}  
```  
  
##  引用类型  
  
**map 是引用类型**，因此在传递过程中它只存在一份。  
  
如果像拷贝 map，没有类似 `copy` 的函数。实现需新创建一个 map，手动遍历赋值。  
  
```go  
// collection/map-copy.go  
  
m := map[string]int{  
	"a": 2,  
	"b": 3,  
}  
  
// 遍历拷贝  
mCopy := map[string]int{}  
for k, v := range m {  
	mCopy[k] = v  
}  
```  
  
##  总结  
  
本篇讲解了 map 的创建、遍历、删除等知识点，这些都很常用，所以一定要掌握。