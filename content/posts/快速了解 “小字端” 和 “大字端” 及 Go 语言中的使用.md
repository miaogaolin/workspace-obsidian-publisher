---  
title: 快速了解 “小字端” 和 “大字端” 及 Go 语言中的使用  
date: 2021-10-26T11:18:56+08:00  
share: true  
categories:  
  - Golang  
tags:  
  - program_golang  
description: 小字端、大字端、Go 语言中的应用  
featured: false  
slug: go-big-small  
cover:  
  image: https://images.unsplash.com/photo-1679676292735-0b93ac00dd3a?q=80&w=720&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D  
---  

  

  
“大字端” 和 “小字端” 表示的是数据存储时的顺序区别，例如：
  

  
对于数字 `573785173` 用十六进制表示为 `0x22334455` 。如何转化的，本篇不需要搞清楚，但如果你不懂就最好了解下。
  

  
对于 `0x22334455` ，左边是高位，右边是低位，这和我们平常表示数字是一样的，例如：十二（12），1 就是高位（十位），2 就是低位（个位）。
  

  
那么给这种，**从左到右，由高位到低位的表示方法就称为 “大字端”。**
  

  
相反，**从左到右，由低位到高位的表示方法就称为 “小字端”。**
  

  
在计算机存储数据时，是以字节为单位去存储，因此把 `0x22334455` 拆分：
  

  
- 大字端：0x22 0x33 0x44 0x55
  
- 小字端：0x55 0x44 0x33 0x22
  

  
## 为啥出现两种
  

  
因为不同的使用场景下，效率是不一样。
  

  
### 大字端
  

  
例如，对于网络传输，使用的就是大字端。为什么？
  

  
因为，早年设备的缓存很小，先接收高字节能快速的判断报文信息：包长度（需要准备多大缓存）、地址范围（IP 地址是从前到后匹配的）。
  

  
在性能不是很好的设备上，高字节在先确实是会更快一些。
  

  
### 小字端
  

  
例如，对于一个加法器，选择的是小字端。为什么？
  

  
因为，加法是从低位到高位开始加，一旦有进位，就直接送到下一位，设计就很简单。
  

  
## Go 语言中应用
  

  
使用 Go 语言中 `binary` 这个标准包，该包实现了数字与字节之间的转化。
  

  
下来我们将数字 `0x22334455` 转化为大字端字节存储。
  

  
```go
  
buffer := new(bytes.Buffer)
  
binary.Write(buffer, binary.BigEndian, int32(0x22334455))
  
```
  

  
- `binary.BigEndian` 常量，表示大字端。
  

  
将数字 `0x22334455` 转化为小字端字节存储。
  

  
```go
  
buffer := new(bytes.Buffer)
  
binary.Write(buffer, binary.LittleEndian, int32(0x22334455))
  
```
  

  
- `binary.LittleEndian` 常量，表示小字端。
  

  
完整例子（仅展示大字端）：
  

  
```go
  
package main
  

  
import (
  
   "bytes"
  
   "encoding/binary"
  
   "fmt"
  
)
  

  
func main() {
  
   buffer := new(bytes.Buffer)
  
   err := binary.Write(buffer, binary.BigEndian, int32(0x22334455))
  
   if err != nil {
  
      panic(err)
  
   }
  

  
   var num int32
  
   err = binary.Read(buffer,binary.BigEndian, &num)
  
   if err != nil {
  
      panic(err)
  
   }
  

  
   fmt.Println(num)
  

  
}
  
```
  

  
- `binary.Write` 写入 `buffer` 变量。
  
- `binary.Read` 从 `buffer` 变量读取。
  
- `int32(0x22334455)` 必须使用固定长度，比如 `int` 类型就不可以，支持类型如下图：
  

  
![20231208111295.webp](/images/20231208111295.webp)
  

  
再补充一个类型 `[]byte`，它等价于 `[]uint8` 类型。 
  

  
## 参考
  

  
1. 官方文档：[https://pkg.go.dev/encoding/binary](https://pkg.go.dev/encoding/binary)