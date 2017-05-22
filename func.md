# func - 函数

## 函数值

函数也是值，可以像其他值一样传递，比如，函数值可以作为函数的参数或者返回值：

```go
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}
```

## 函数的闭包

Go 函数可以是一个闭包。闭包是一个函数值，它引用了函数体之外的变量。 这个函数可以对这个引用的变量进行访问和赋值；换句话说这个函数被“绑定”在这个变量上。

例如，函数 adder 返回一个闭包。每个返回的闭包都被绑定到其各自的 sum 变量上。

```go
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}
```
