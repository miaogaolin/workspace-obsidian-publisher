---  
title: Google：12 条 Golang 最佳实践  
date: 2021-10-27T11:14:56+08:00  
share: true  
categories:  
  - Golang  
tags:  
  - program_golang  
description: 12 条 Golang 最佳实践，并在原文基础上增加了解读与补充  
featured: false  
slug: go-bestpractices  
cover:  
  image: https://images.unsplash.com/photo-1526045566106-788438d25353?q=80&w=720&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D  
---  

  
这是直接总结好的 12 条，详细的再继续往下看：
  

  
1. 先处理错误避免嵌套
  
2. 尽量避免重复
  
3. 先写最重要的代码
  
4. 给代码写文档注释
  
5. 命名尽可能简洁
  
6. 使用多文件包
  
7. 使用 `go get` 可获取你的包
  
8. 了解自己的需求
  
9. 保持包的独立性
  
10. 避免在内部使用并发
  
11. 使用 Goroutine 管理状态
  
12. 避免 Goroutine 泄露
  

  
## 最佳实践
  

  
这是一篇翻译文章，为了使读者更好的理解，会在原文翻译的基础增加一些讲解或描述。
  

  
来在维基百科：
  

  
```
  
"A best practice is a method or technique that has consistently shown results superior
  
to those achieved with other means"
  

  
最佳实践是一种方法或技术，其结果始终优于其他方式。
  
```
  

  
写 Go 代码时的技术要求：
  

  
- 简单性
  
- 可读性
  
- 可维护性
  

  
## 样例代码
  

  
需要优化的代码。
  

  
```go
  
type Gopher struct {
  
    Name     string
  
    AgeYears int
  
}
  

  
func (g *Gopher) WriteTo(w io.Writer) (size int64, err error) {
  
    err = binary.Write(w, binary.LittleEndian, int32(len(g.Name)))
  
    if err == nil {
  
        size += 4
  
        var n int
  
        n, err = w.Write([]byte(g.Name))
  
        size += int64(n)
  
        if err == nil {
  
            err = binary.Write(w, binary.LittleEndian, int64(g.AgeYears))
  
            if err == nil {
  
                size += 4
  
            }
  
            return
  
        }
  
        return
  
    }
  
    return
  
}
  
```
  

  
看看上面的代码，自己先思索在代码编写方式上怎么更好，我先简单说下代码意思是啥：
  

  
- 将 `Name` 和 `AgeYears` 字段数据存入 `io.Writer` 类型中。
  
- 如果存入的数据是 `string` 或 `[]byte` 类型，再追加其长度数据。
  

  
如果对 `binary` 这个标准包不知道怎么使用，就看看我的另一篇文章 [《快速了解 “小字端” 和 “大字端” 及 Go 语言中的使用》]({{< relref "%E5%BF%AB%E9%80%9F%E4%BA%86%E8%A7%A3%20%E2%80%9C%E5%B0%8F%E5%AD%97%E7%AB%AF%E2%80%9D%20%E5%92%8C%20%E2%80%9C%E5%A4%A7%E5%AD%97%E7%AB%AF%E2%80%9D%20%E5%8F%8A%20Go%20%E8%AF%AD%E8%A8%80%E4%B8%AD%E7%9A%84%E4%BD%BF%E7%94%A8.md" >}})。
  

  
## 先处理错误避免嵌套
  

  
```go
  
func (g *Gopher) WriteTo(w io.Writer) (size int64, err error) {
  
    err = binary.Write(w, binary.LittleEndian, int32(len(g.Name)))
  
    if err != nil {
  
        return
  
    }
  
    size += 4
  
    n, err := w.Write([]byte(g.Name))
  
    size += int64(n)
  
    if err != nil {
  
        return
  
    }
  
    err = binary.Write(w, binary.LittleEndian, int64(g.AgeYears))
  
    if err == nil {
  
        size += 4
  
    }
  
    return
  
}
  
```
  

  
减少判断错误的嵌套，会使读者看起来更轻松。
  

  
## 尽量避免重复
  

  
上面代码中 `WriteTo` 方法中的 `Write` 出现了 3 次，比较重复，精简后如下：
  

  
```go
  
type binWriter struct {
  
    w    io.Writer
  
    size int64
  
    err  error
  
}
  

  
// Write writes a value to the provided writer in little endian form.
  
func (w *binWriter) Write(v interface{}) {
  
    if w.err != nil {
  
        return
  
    }
  
    if w.err = binary.Write(w.w, binary.LittleEndian, v); w.err == nil {
  
        w.size += int64(binary.Size(v))
  
    }
  
}
  
```
  

  
使用 `binWriter` 结构体。
  

  
```go
  
func (g *Gopher) WriteTo(w io.Writer) (int64, error) {
  
    bw := &binWriter{w: w}
  
    bw.Write(int32(len(g.Name)))
  
    bw.Write([]byte(g.Name))
  
    bw.Write(int64(g.AgeYears))
  
    return bw.size, bw.err
  
}
  
```
  

  
## type-switch 处理不同类型
  

  
```go
  
func (w *binWriter) Write(v interface{}) {
  
    if w.err != nil {
  
        return
  
    }
  
    switch v.(type) {
  
    case string:
  
        s := v.(string)
  
        w.Write(int32(len(s)))
  
        w.Write([]byte(s))
  
    case int:
  
        i := v.(int)
  
        w.Write(int64(i))
  
    default:
  
        if w.err = binary.Write(w.w, binary.LittleEndian, v); w.err == nil {
  
            w.size += int64(binary.Size(v))
  
        }
  
    }
  
}
  

  
func (g *Gopher) WriteTo(w io.Writer) (int64, error) {
  
    bw := &binWriter{w: w}
  
    bw.Write(g.Name)
  
    bw.Write(g.AgeYears)
  
    return bw.size, bw.err
  
}
  
```
  

  
## type-switch 精简
  

  
摒弃了上面代码的 `v.(string)` 、`v.(int)` 类型反射使用。
  

  
```go
  
func (w *binWriter) Write(v interface{}) {
  
    if w.err != nil {
  
        return
  
    }
  
    switch x := v.(type) {
  
    case string:
  
        w.Write(int32(len(x)))
  
        w.Write([]byte(x))
  
    case int:
  
        w.Write(int64(x))
  
    default:
  
        if w.err = binary.Write(w.w, binary.LittleEndian, v); w.err == nil {
  
            w.size += int64(binary.Size(v))
  
        }
  
    }
  
}
  
```
  

  
进入不同分支，`x` 变量对应的就是该分支的类型。
  

  
## 自行决定是否写入
  

  
```go
  
type binWriter struct {
  
    w   io.Writer
  
    buf bytes.Buffer
  
    err error
  
}
  

  
// Write writes a value to the provided writer in little endian form.
  
func (w *binWriter) Write(v interface{}) {
  
    if w.err != nil {
  
        return
  
    }
  
    switch x := v.(type) {
  
    case string:
  
        w.Write(int32(len(x)))
  
        w.Write([]byte(x))
  
    case int:
  
        w.Write(int64(x))
  
    default:
  
        w.err = binary.Write(&w.buf, binary.LittleEndian, v)
  
    }
  
}
  

  
// Flush writes any pending values into the writer if no error has occurred.
  
// If an error has occurred, earlier or with a write by Flush, the error is
  
// returned.
  
func (w *binWriter) Flush() (int64, error) {
  
    if w.err != nil {
  
        return 0, w.err
  
    }
  
    return w.buf.WriteTo(w.w)
  
}
  

  
func (g *Gopher) WriteTo(w io.Writer) (int64, error) {
  
    bw := &binWriter{w: w}
  
    bw.Write(g.Name)
  
    bw.Write(g.AgeYears)
  
    return bw.Flush()
  
}
  
```
  

  
`WriteTo` 方法中，分了两大部分，增加了灵活性：
  

  
- 组装信息
  
- 调用 `Flush` 方法来决定是否写入 `w`。
  

  
## 函数适配器
  

  
```go
  
func init() {
  
    http.HandleFunc("/", handler)
  
}
  

  
func handler(w http.ResponseWriter, r *http.Request) {
  
    err := doThis()
  
    if err != nil {
  
        http.Error(w, err.Error(), http.StatusInternalServerError)
  
        log.Printf("handling %q: %v", r.RequestURI, err)
  
        return
  
    }
  

  
    err = doThat()
  
    if err != nil {
  
        http.Error(w, err.Error(), http.StatusInternalServerError)
  
        log.Printf("handling %q: %v", r.RequestURI, err)
  
        return
  
    }
  
}
  
```
  

  
函数 `handler` 包含了业务的逻辑和错误处理，下来将错误处理单独写一个函数处理，代码修改如下：
  

  
```go
  
func init() {
  
    http.HandleFunc("/", errorHandler(betterHandler))
  
}
  

  
func errorHandler(f func(http.ResponseWriter, *http.Request) error) http.HandlerFunc {
  
    return func(w http.ResponseWriter, r *http.Request) {
  
        err := f(w, r)
  
        if err != nil {
  
            http.Error(w, err.Error(), http.StatusInternalServerError)
  
            log.Printf("handling %q: %v", r.RequestURI, err)
  
        }
  
    }
  
}
  

  
func betterHandler(w http.ResponseWriter, r *http.Request) error {
  
    if err := doThis(); err != nil {
  
        return fmt.Errorf("doing this: %v", err)
  
    }
  

  
    if err := doThat(); err != nil {
  
        return fmt.Errorf("doing that: %v", err)
  
    }
  
    return nil
  
}
  
```
  

  
## 组织你的代码
  

  
### 1. 先写最重要的
  

  
许可信息、构建信息、包文档。
  

  
`import` 语句：相关联组使用空行分隔。
  

  
```go
  
import (
  
    "fmt"
  
    "io"
  
    "log"
  

  
    "golang.org/x/net/websocket"
  
)
  
```
  

  
其余代码，以最重要的类型开始，以辅助函数和类型结尾。
  

  
### 2. 文档注释
  

  
包名前的相关文档。
  

  
```go
  
// Package playground registers an HTTP handler at "/compile" that
  
// proxies requests to the golang.org playground service.
  
package playground
  
```
  

  
Go 语言中的标示符（变量、结构体等等）在 godoc 导出的文章中应该被正确的记录下来。
  

  
```go
  
// Author represents the person who wrote and/or is presenting the document.
  
type Author struct {
  
    Elem []Elem
  
}
  

  
// TextElem returns the first text elements of the author details.
  
// This is used to display the author' name, job title, and company
  
// without the contact details.
  
func (p *Author) TextElem() (elems []Elem) {
  
```
  

  
**扩展**：
  

  
使用 godoc 工具在网页上查看 go 项目文档。
  

  
```bash
  
# 安装
  
go get golang.org/x/tools/cmd/godoc
  

  
# 启动服务
  
godoc -http=:6060
  
```
  

  
直接在本地访问 localhost:6060 查看文档。
  

  
### 3. 命名尽可能简洁
  

  
或者说，长命名不一定好。
  

  
尽可能找到一个可以清晰表达的简短命名，例如：
  

  
- `MarshalIndent` 比 `MarshalWithIndentation` 好。
  

  
不要忘了，在调用包内容时，会先写包名。
  

  
- 在 `encoding/json` 包内，有一个结构体 `Encoder`，不要写成 `JSONEncoder`。
  
- 这样被使用 `json.Encoder` 。
  

  
### 4. 多文件包
  

  
是否应该将一个包拆分到多个文件？
  

  
- 应避免代码太长
  

  
标准包 `net/http` 总共 15734 行代码，被拆分到 47 个文件中。
  

  
- 拆分代码和测试。
  

  
net/http/cookie.go 和 net/http/cookie_test.go 文件都放置在 http 包下。
  

  
测试代码**只有**在测试时才被编译。
  

  
- 拆分包文档
  

  
当在一个包内有多个文件时，按照惯例，创建一个 doc.go 文件编写包的文档描述。
  

  
**个人思考**：当一个包的说明信息比较多时，可以考虑创建 doc.go 文件。
  

  
### 5. 使用 go get 可获取你的包
  

  
当你的包被提供使用时，应该清晰的让使用者知道哪些可复用，哪些不可复用。
  

  
所以，当一些包可能会被复用，有些则不会的情况下怎么做？
  

  
例如：定义一些网络协议的包可能会复用，而定义一些可执行命令的包则不会。
  

  
![20231208111271.webp](/images/20231208111271.webp)
  

  
- `cmd` 可执行命令的包，不提供复用
  
- `pkg` 可复用的包
  

  
**个人思考**：如果一个项目中的可执行入口比较多，建议放置在 cmd 目录中，而对于 pkg 目录目前是不太建议，所以不用借鉴。
  

  
## API
  

  
### 1. 了解自己的需求
  

  
我们继续使用之前的 Gopher 类型。
  

  
```go
  
type Gopher struct {
  
    Name     string
  
    AgeYears int
  
}
  
```
  

  
我们可以定义这个方法。
  

  
```go
  
func (g *Gopher) WriteToFile(f *os.File) (int64, error) {
  
```
  

  
但方法的参数使用具体的类型时会变得难以测试，因此我们使用接口。
  

  
```go
  
func (g *Gopher) WriteToReadWriter(rw io.ReadWriter) (int64, error) {
  
```
  

  
并且，当使用了接口后，我们应该只需定义我们所需要的方法。
  

  
```go
  
func (g *Gopher) WriteToWriter(f io.Writer) (int64, error) {
  
```
  

  
### 2. 保持包的独立性
  

  
```go
  
import (
  
    "golang.org/x/talks/content/2013/bestpractices/funcdraw/drawer"
  
    "golang.org/x/talks/content/2013/bestpractices/funcdraw/parser"
  
)
  
```
  

  
```go
  
// Parse the text into an executable function.
  
  f, err := parser.Parse(text)
  
  if err != nil {
  
      log.Fatalf("parse %q: %v", text, err)
  
  }
  

  
  // Create an image plotting the function.
  
  m := drawer.Draw(f, *width, *height, *xmin, *xmax)
  

  
  // Encode the image into the standard output.
  
  err = png.Encode(os.Stdout, m)
  
  if err != nil {
  
      log.Fatalf("encode image: %v", err)
  
  }
  
```
  

  
代码中 `Draw` 方法接受了 `Parse` 函数返回的 `f` 变量，从逻辑上看 `drawer` 包依赖 `parser` 包，下来看看如何取消这种依赖性。 
  

  
`parser` 包：
  

  
```go
  
type ParsedFunc struct {
  
    text string
  
    eval func(float64) float64
  
}
  

  
func Parse(text string) (*ParsedFunc, error) {
  
    f, err := parse(text)
  
    if err != nil {
  
        return nil, err
  
    }
  
    return &ParsedFunc{text: text, eval: f}, nil
  
}
  

  
func (f *ParsedFunc) Eval(x float64) float64 { return f.eval(x) }
  
func (f *ParsedFunc) String() string         { return f.text }
  
```
  

  
`drawer` 包：
  

  
```go
  
import (
  
    "image"
  

  
    "golang.org/x/talks/content/2013/bestpractices/funcdraw/parser"
  
)
  

  
// Draw draws an image showing a rendering of the passed ParsedFunc.
  
func DrawParsedFunc(f parser.ParsedFunc) image.Image {
  
```
  

  
使用接口类型，避免依赖。
  

  
```go
  
import "image"
  

  
// Function represent a drawable mathematical function.
  
type Function interface {
  
    Eval(float64) float64
  
}
  

  
// Draw draws an image showing a rendering of the passed Function.
  
func Draw(f Function) image.Image {
  
```
  

  
**测试**：接口类型比具体类型更容易测试。
  

  
```go
  
package drawer
  

  
import (
  
    "math"
  
    "testing"
  
)
  

  
type TestFunc func(float64) float64
  

  
func (f TestFunc) Eval(x float64) float64 { return f(x) }
  

  
var (
  
    ident = TestFunc(func(x float64) float64 { return x })
  
    sin   = TestFunc(math.Sin)
  
)
  

  
func TestDraw_Ident(t *testing.T) {
  
    m := Draw(ident)
  
    // Verify obtained image.
  
```
  

  
### 4. 避免在内部使用并发
  

  
```go
  
func doConcurrently(job string, err chan error) {
  
    go func() {
  
        fmt.Println("doing job", job)
  
        time.Sleep(1 * time.Second)
  
        err <- errors.New("something went wrong!")
  
    }()
  
}
  

  
func main() {
  
    jobs := []string{"one", "two", "three"}
  

  
    errc := make(chan error)
  
    for _, job := range jobs {
  
        doConcurrently(job, errc)
  
    }
  
    for _ = range jobs {
  
        if err := <-errc; err != nil {
  
            fmt.Println(err)
  
        }
  
    }
  
}
  
```
  

  
如果这样做，那如果我们想同步调用 `doConcurrently` 该如何做？
  

  
```go
  
func do(job string) error {
  
    fmt.Println("doing job", job)
  
    time.Sleep(1 * time.Second)
  
    return errors.New("something went wrong!")
  
}
  

  
func main() {
  
    jobs := []string{"one", "two", "three"}
  

  
    errc := make(chan error)
  
    for _, job := range jobs {
  
        go func(job string) {
  
            errc <- do(job)
  
        }(job)
  
    }
  
    for _ = range jobs {
  
        if err := <-errc; err != nil {
  
            fmt.Println(err)
  
        }
  
    }
  
}
  
```
  

  
对外暴露同步的函数，这样并发调用时也是容易的，同样也满足同步调用。
  

  
## 最佳的并发实践
  

  
### 1. 使用 Goroutine 管理状态
  

  
Goroutine 之间使用一个 “通道” 或带有通道字段的 “结构体” 来通信。
  

  
```go
  
type Server struct{ quit chan bool }
  

  
func NewServer() *Server {
  
    s := &Server{make(chan bool)}
  
    go s.run()
  
    return s
  
}
  

  
func (s *Server) run() {
  
    for {
  
        select {
  
        case <-s.quit:
  
            fmt.Println("finishing task")
  
            time.Sleep(time.Second)
  
            fmt.Println("task done")
  
            s.quit <- true
  
            return
  
        case <-time.After(time.Second):
  
            fmt.Println("running task")
  
        }
  
    }
  
}
  

  
func (s *Server) Stop() {
  
    fmt.Println("server stopping")
  
    s.quit <- true
  
    <-s.quit
  
    fmt.Println("server stopped")
  
}
  

  
func main() {
  
    s := NewServer()
  
    time.Sleep(2 * time.Second)
  
    s.Stop()
  
}
  
```
  

  
### 2. 使用带缓冲的通道避免 Goroutine 泄露
  

  
```go
  
func sendMsg(msg, addr string) error {
  
    conn, err := net.Dial("tcp", addr)
  
    if err != nil {
  
        return err
  
    }
  
    defer conn.Close()
  
    _, err = fmt.Fprint(conn, msg)
  
    return err
  
}
  

  
func main() {
  
    addr := []string{"localhost:8080", "http://google.com"}
  
    err := broadcastMsg("hi", addr)
  

  
    time.Sleep(time.Second)
  

  
    if err != nil {
  
        fmt.Println(err)
  
        return
  
    }
  
    fmt.Println("everything went fine")
  
}
  

  
func broadcastMsg(msg string, addrs []string) error {
  
    errc := make(chan error)
  
    for _, addr := range addrs {
  
        go func(addr string) {
  
            errc <- sendMsg(msg, addr)
  
            fmt.Println("done")
  
        }(addr)
  
    }
  

  
    for _ = range addrs {
  
        if err := <-errc; err != nil {
  
            return err
  
        }
  
    }
  
    return nil
  
}
  
```
  

  
这段代码有个问题，如果提前返回了 `err` 变量，`errc` 通道将不会被读取，因此 Goroutine 将会阻塞。
  

  
**总结**：
  

  
- 在写入通道时 Goroutine 被阻塞。
  
- Goroutine 持有对通道的引用。
  
- 通道不会被 gc 回收。
  

  
使用缓冲通道解决 Goroutine 阻塞问题。
  

  
```go
  
func broadcastMsg(msg string, addrs []string) error {
  
    errc := make(chan error, len(addrs))
  
    for _, addr := range addrs {
  
        go func(addr string) {
  
            errc <- sendMsg(msg, addr)
  
            fmt.Println("done")
  
        }(addr)
  
    }
  

  
    for _ = range addrs {
  
        if err := <-errc; err != nil {
  
            return err
  
        }
  
    }
  
    return nil
  
}
  
```
  

  
如果我们不能预知通道的缓冲大小，也称容量，那该怎么办？
  

  
创建一个传递退出状态的通道来避免 Goroutine 的泄露。
  

  
```go
  
func broadcastMsg(msg string, addrs []string) error {
  
    errc := make(chan error)
  
    quit := make(chan struct{})
  

  
    defer close(quit)
  

  
    for _, addr := range addrs {
  
        go func(addr string) {
  
            select {
  
            case errc <- sendMsg(msg, addr):
  
                fmt.Println("done")
  
            case <-quit:
  
                fmt.Println("quit")
  
            }
  
        }(addr)
  
    }
  

  
    for _ = range addrs {
  
        if err := <-errc; err != nil {
  
            return err
  
        }
  
    }
  
    return nil
  
}
  
```
  

  
## 参考
  

  
原文链接：[https://talks.golang.org/2013/bestpractices.slide#1](https://talks.golang.org/2013/bestpractices.slide#1)
  

  
视频链接：[https://www.youtube.com/watch?v=8D3Vmm1BGoY](https://www.youtube.com/watch?v=8D3Vmm1BGoY)