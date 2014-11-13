##包##
```
import "fmt"
// 多个的时候可以:
import (
  "fmt"
  "math"
)
```
在导入了包之后，可以用其导出的名称来调用它，例如```fmt.Println()```
在包中，首字母大写的名称会被导出，例如```Foo```


##变量##
```
var x, y, z int // 在函数外定义必须要var
var t // 默认为bool类型
func main () {
  a, b, c := 1, 2, "string" // 在函数内定义可以不用var，直接赋值
}
```


##常量##
```
const (
  ONE = 1
  TWO = 2
)
func main(){
  const World = "世界"
}
```


##指针##
```

```


##函数##
```
func FUNCTION_NAME (ARGUMENTS) RETURN_TYPE {
  //...
  return
}
```

例如:
与```func add(x int, y int)```一样
```
func add (x, y int) int{ 
  return x + y
}
// or
add := func(x, y int) int {}
```

返回多个值
```
func swap(x, y string) (string, string){
  return y, x // 类型对应
}
```

指定返回变量
```
func split(sum int) (x, y int){ 
  x = sum * 4 / 9
  y = sum - x
  return // 必需要return 
}
```

闭包
```
func addOne(begin int) func() int {
  return func() int {
    begin++
    return begin 
  }
}
func main() {
  pos := addOne(0)
  pos() // = 1
  pos() // = 2
}
```


##For##
Go只有一种循环，就是For，形如:
```
for i := 0; i < 10; i++ {
  // 不用括号，但是{}必须
  // ...
}
```

简化:
```
sum := 0
for ; sum < 100; {
  sum += sum
}
for ; ; {
  // ...
}
// 等价于
for {
}
```

实现while:
```
i := 0
for i < 10 {
  // do something
}
```

Range
可对array或者map进行迭代
```
nums := []int{"one", "two"}
for key, value := range nums {}
for index, value := range nums {}
for index := range nums {} // 忽略value
for _, value := range nums {} // 占位符_
```


##if##
```
if x < 0 {
  // 不用括号，但是{}必须
} else {
}
```

if在条件之前执行一条简单的语句，这个语句定义的变量的作用域仅在if范围之内（包括else）
```
if v := math.Pow(x, 2); v < 100 {
  // do something
}
```


##switch##
```
switch v := math.Pow(x, 2); v {
  case 2:
    // 会自动break
  case 4:
    // ..
  default:
    // ..
}
```

if-then-else
```
t := time.Now()
switch { // 同switch true
  case t.Hour() < 12:
    // ...
  case t.Hour() < 17:
    // ..
  default:
    // ..
}
```



##基本类型##
```
bool

string

int int8 int16 int32 int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8的别名

rune // int32的别名，代表一个Unicode码点

float32 float64

complex64 complex128
```


##结构体##
```
type Vertex struct {
  X int
  Y int
}
```

指针访问:
通过指针间接访问是透明的(可直接操作)
```
type Vertex struct {
  X, Y int
}
func main(){
  p := Vertex{1, 2}
  q := &p
  q.X = 1e9
}
```

结构体文法:
&构造指向结构体文法的指针
```
var (
  p = Vertex{1, 2}
  q = &Vertex{1, 2}
  r = Vertex{X: 1} // Y: 0 is implicit
  s = Vertex{}  // X: 0 and Y: 0
)
```

方法
```
// 定义结构体的函数，在函数名前面指定结构
func (v *Vertex) square() int { 
  return math.Sqrt(math.Pow(X, 2) + math.Pow(Y, 2))
}
func main(){
  v1 := Vertex{1, 2} // 用{}来初始化
  v2 := Vertex{X: 1, Y: 2} // 可以制定初始化
  v1.X = 4
  fmt.Println(v1.square())
}
```
可以对包中的任意类型定义方法
```
type MyFloat float64
func (f MyFloat) Abs() float64 {
  if f < 0 {
    return float64(-f)
  }
  return float64(f)
}
```
接收者为指针,
避免方法调用需要拷贝值；方法可以修改接收者的值
```
func (v *Vertex) square() int { 
  return math.Sqrt(math.Pow(X, 2) + math.Pow(Y, 2))
}
```


##new##
new(T)分配一个零初始化的T值，返回指向它的指针
```
var t *T = new(T)
// or:
t := new(T)
```

例如:
```
v := new(Vertex)
v.X, v.Y = 11, 9
```


##Map##
map映射健到值
```
var m map[string]Vertex // key类型为string，value类型为Vertex
```

map在使用前必须用make来创建(不是new)；值为nil的map是空的(初始)，并且不能赋值
```
m = make(map[string]Vertex)
m["first"] = Vertex{1, 2}
```

初始赋值
```
m = map[string]Vertex{
  "one": Vertex{1, 2},
  "two": Vertex{3, 4},
}
// or 简化
m = map[string]Vertex{
  "one": {1, 2},
  "two": {3, 4},
}
// or
```

增删改查:
```
m[key] = value
m[key] // 若不存在，则返回value对应类型的零值
delete(m, key)
value, ok = m[key] // 如果key在m中，则ok==true
```


##数组##
```
arrray = [5]int // 必须声明类型和长度
p := []int{1, 2, 3, 4, 5} // 赋值
a := make([]int, 5) // len(a) = 5
```
```
var a []int
// a == nil, len(a) == 0, cap(a) == 0
```

容量
```
a := make([]int, 0, 5) // len(a) = 0, cap(a) = 5
a = a[:cap(a)] // 通过切片来扩容(可增长到容量上限)
a = a[1:] // len(a) = 4, cap(a) = 4
```

切片
```
s[1:3] // 不包括第三个
s[:3]
s[1:]
```


##接口##
接口类型是由一组方法定义的集合，接口类型的值可以存放实现这些方法的任何值，(多态)
```
type Abser interface {
  Abs() float64
}
func main (){
  var a Abser
  
  f := MyFloat(math.Sqrt2)
  v := Vertex{3, 4}

  a = f
  a = &v // a *Vertex 实现了Abser
  a = v // a Vertex 没有实现Abser，出错
}
```


##error##
```
// 预定义的内建接口类型error
type error interface {
  Error() string
}
```

用fmt输出一个error时，会自动调用Error()方法
```
type MyError struct{
  When time.Time
  What string
}
func (e *MyError) Error() string {
  return fmt.Sprintf("at %v, %s", e.When, e.What)
}
func main(){
  err := &MyError{time.Now(), "it didn't work!"}
  fmt.Println(err)
}
```


##Web##
包http通过任何实现了http.Handler的值来响应HTTP请求
```
// 内建
package http
type Handler interface {
  ServeHTTP(w ResponseWriter, r *Request)
}
```

例子：
```
import "net/http"
type Hello struct {}
func (h Hello) ServeHTTP(w http.ResponseWriter, r *http.Request) {
  fmt.Fprint(w, "hello!")
}
func main () {
  var h Hello
  http.ListenAndServe("localhost:4000", h)
}
```


##图片##
```
// 内建
package image
type Image interface {
  ColorModel() color.Model
  Bounds() Rectangle
  At(x, y int) color.Color
}
```

例子：
```
func main() {
  m := image.NewRGBA(image.Rect(0, 0, 100, 100))
  fmt.Println(m.Bounds())
  fmt.Println(m.At(0, 0).RGBA())
}
```


##Goroutine##
goroutine是由Go运行时环境管理的轻量级线程
```
go f(x, y, z) // 将开启一个新的goroutine执行f
```
goroutine在相同的地址空间中运行，因此访问共享内存必须进行同步
```
func say(s string) {
  for i := 0; i < 5; i++ {
    runtime.Gosched() // 如果不加这句，将不会输出'world'
    fmt.Println(s)
  }
}
func main(){
  go say("world")
  say("Hello")
}
```


##Channel##
```
ch <- v // 将v送入channel ch中
v := <-ch // 从ch中接收，并赋值给v
```

创建
```
ch := make(chan int)
```

默认情况下，在另一端准好好之前，发送和接收端都会阻塞。这使得goroutine可以在没有明确的锁或竟态变量的情况下进行同步。
```
func sum(a []int, c chan int) {
  sum := 0
  for _, v := range a {
    sum += v
  }
  c <- sum
}
func main() {
  a := []int{1, 2, 3, 4, 5}
  ch := make(chan int) // buffer为1
  go sum(a[:len(a)/2], c)
  go sum(a[len(a)/2:], c)
  x, y := <-c, <-c

  fmt.Println(x, y, x + y)
}
```

buffer
第二个参数作为buffer长度，向channel发数据时，只有在buffer满了的时候才会阻塞
```
ch := make(chan int, 2)
```

close
```
func fi(n int, c chan int){
  x, y := 0, 1
  for i := 0; i < n; i++ {
    c <- x
    x, y = y, x + y
  }
  close(c)
}
func main(){
  c := make(chan int, 10)
  go fi(10, c)
  for i := range c {
    // ...
  }
}
```

可以通过赋值语句的第二个参数来测试channel是否被关闭了，若关闭，ok为false
```
v, ok := <-ch
```
循环```for i := range c```会不断从channel接收值，直到被close

note: 只有发送者可以关闭，而不是接收者，向一个已经关闭的channel发送数据会引起panic
note: channel与文件不同，通常无须关闭。只有在需要的时候才有必要关闭，例如中断一个range

select
```
func fi(c, quit chan int){
  x, y := 0, 1
  for {
    select {
      case c <- x:
        x, y = y, x + y
      case <-quit:
        fmt.Println("quit")
        return
    }
  }
}
func main(){
  c := make(chan int)
  quit := make(chan int)
  go func() {
    for i := 0; i < 10; i++ {
      fmt.Println(<-c)
    }
    quit <- 0
  }()
  fi(c, quit)
}
```
select会阻塞，直到其中一个可以执行。如果多个可以执行，随机执行一个。
当所有分支都没有准备的时候，default会执行，so，为了非阻塞的发送或者接收，可使用default分支
```
select {
  case i := <-c:
    // use i
  default:
    // when c is block
}
```
