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

