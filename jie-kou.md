# 接口

Go中**接口**类型是由一组方法定义的集合，接口类型的值可以存放这些方法的任何值。

类型通过实现那些方法来实现接口。 没有显式声明的必要；所以也就没有关键字“implements”。

隐式接口解藕了实现接口的包和定义接口的包：互不依赖。因此，也就无需在每一个实现上增加新的接口名称，这样同时也鼓励了明确的接口定义。

```go
type Reader interface {
	Read(b []byte) (n int, err error)
}

type Writer interface {
	Write(b []byte) (n int, err error)
}
// 接口内嵌
type ReadWriter interface { 
	Reader
	Writer
}
```

## Stringers

一个普遍存在的接口是`fmt`包中定义的`Stringer`。

```go
type Stringer interface {
    String() string
}
```

`Stringer`是一个可以用字符串描述自己的类型。`fmt`包（还有其他包）使用这个来进行输出。

```go
type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}
```

## 接口值

概念上讲一个接口的值，**接口值**，**由两个部分组成，一个具体的类型和那个类型的值。**它们被称为接口的动态类型和动态值。对于像Go语言这种静态类型的语言，类型是编译期的概念；因此一个类型不是一个值。在我们的概念模型中，一些提供每个类型信息的值被称为类型描述符，比如类型的名称和方法。在一个接口值中，类型部分代表与之相关类型的描述符。

在Go语言中，变量总是被一个定义明确的值初始化，即使接口类型也不例外。对于一个**接口的零值**就是它的类型和值的部分都是nil。

一个接口值基于它的动态类型被描述为空或非空，所以这是一个空的接口值。你可以通过使用w==nil或者w!=nil来判读接口值是否为空。调用一个空接口值上的任意方法都会产生panic:

```go
w.Write([]byte("hello")) // panic: nil pointer dereference
```

接口值可以使用＝＝和！＝来进行比较。两个接口值相等仅当它们都是nil值或者它们的动态类型相同并且动态值也根据这个动态类型的＝＝操作相等。因为接口值是可比较的，所以它们可以用在map的键或者作为switch语句的操作数。

然而，如果两个接口值的动态类型相同，但是这个动态类型是不可比较的（比如切片），将它们进行比较就会失败并且panic:

```go
var x interface{} = []int{1, 2, 3}
fmt.Println(x == x) // panic: comparing uncomparable type []int
```

### 一个包含nil指针的接口不是nil接口

一个不包含任何值的nil接口值和一个刚好包含nil指针的接口值是不同的。这个细微区别产生了一个容易绊倒每个Go程序员的陷阱。

```go
var buf *bytes.Buffer // 值为nil的非nil接口值
```

![](/assets/chrome_2017-05-30_00-01-59.png)

