# 方法

Go没有类，但却可以在结构体类型、自定义类型等上定义方法。定义的方法需与类型定义放置在一个包中，但不能对来自其他包的类型或基础类型定义方法。

方法接收者出现在func关键字和方法名之间的参数中。如果接收者为指针，说明方法可以修改接收者的值。

```go
type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

