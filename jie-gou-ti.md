# 结构体

在Go中，结构体（struct）就是一个字段的集合，通过如下方式定义:

```go
type Vertex struct {
	X int
	Y int
}
```
结构体字段通过点号（`.`）来访问，在通过结构体指针访问字段时也是使用点号，即指针的间接访问是透明的。

```go
p := &Vertex{1, 2}
p.X = 1e9
fmt.Println(p.Y)
```