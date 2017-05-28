# Slice

一个**slice**会指向一个序列的值，并且包含了长度信息。

`[]T`的一个元素类型为T的slice。`len(s)`返回slice s的长度。

slice可以包含任意的类型，包括另一个slice，例如：

```go
// Create a tic-tac-toe board.
game := [][]string{
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
}

// The players take turns.
game[0][0] = "X"
game[2][2] = "O"
game[2][0] = "X"
game[1][0] = "O"
game[0][2] = "X"

printBoard(game)
```

## 切分

slice 可以重新**切片**，创建一个新的 slice 值指向相同的数组。

表达式

```go
s[lo:hi]
```

表示从 lo 到 hi-1 的 slice 元素，含前端，不包含后端。因此`s[lo:lo]`是空的。

## 构造

slice是一个带有point（指向数组的指针），Len（**数组中实际有值的个数**），Cap（**数组的容量**）。如下图所示：

![](/assets/go-slice-len-cap.png) 

可由函数 make 创建。这会分配一个全是零值的数组并且返回一个 slice 指向这个数组：

```go
a := make([]int, 5)  // len(a)=5
```

为了指定容量，可传递第三个参数到make：

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
```

## 添加元素

Go 提供了一个内建函数 append:

```go
func append(s []T, vs ...T) []T
```

append函数就理解为往slice中加入一个值，如果未达到容量（len<cap）那么就直接往数组中加值，如果达到容量（len = cap）那么就增加一个新的元素空间，将值放在里面。

下面将通过一个程序来理解append的底层工作原理：

```go
func appendInt(x []int, y int) []int {
    var z []int
    zlen := len(x) + 1
    if zlen <= cap(x) {
        // There is room to grow.  Extend the slice.
        z = x[:zlen]
    } else {
        // There is insufficient space.  Allocate a new array.
        // Grow by doubling, for amortized linear complexity.
        zcap := zlen
        if zcap < 2*len(x) {
            zcap = 2 * len(x)
        }
        z = make([]int, zlen, zcap)
        copy(z, x) // a built-in function; see text
    }
    z[len(x)] = y
    return z
}
```
每次调用appendInt函数，必须先检测slice底层数组是否有足够的容量来保存新添加的元素。如果有足够空间的话，直接扩展slice（依然在原有的底层数组之上），将新添加的y元素复制到新扩展的空间，并返回slice。因此，输入的x和输出的z共享相同的底层数组。

如果没有足够的增长空间的话，appendInt函数则会先分配一个足够大的slice用于保存新的结果，先将输入的x复制到新的空间，然后添加y元素。**结果z和输入的x引用的将是不同的底层数组**。

为了提高内存使用效率，新分配的数组一般略大于保存x和y所需要的最低大小。通过在每次扩展数组时直接将长度翻倍从而避免了多次内存分配，也确保了添加单个元素操的平均时间是一个常数时间。

这个程序演示了效果：

```go
func main() {
    var x, y []int
    for i := 0; i < 10; i++ {
        y = appendInt(x, i)
        fmt.Printf("%d cap=%d\t%v\n", i, cap(y), y)
        x = y
    }
}
```
每一次容量的变化都会导致重新分配内存和copy操作：

```
0  cap=1    [0]
1  cap=2    [0 1]
2  cap=4    [0 1 2]
3  cap=4    [0 1 2 3]
4  cap=8    [0 1 2 3 4]
5  cap=8    [0 1 2 3 4 5]
6  cap=8    [0 1 2 3 4 5 6]
7  cap=8    [0 1 2 3 4 5 6 7]
8  cap=16   [0 1 2 3 4 5 6 7 8]
9  cap=16   [0 1 2 3 4 5 6 7 8 9]
```

内置的append函数可能使用比appendInt更复杂的内存扩展策略。因此，通常我们并不知道append调用是否导致了内存的重新分配，因此我们也不能确认新的slice和原始的slice是否引用的是相同的底层数组空间。同样，我们不能确认在原先的slice上的操作是否会影响到新的slice。因此，通常是将append返回的结果直接赋值给输入的slice变量：

```go
runes = append(runes, r)
```

更新slice变量不仅对调用append函数是必要的，实际上对应任何可能导致长度、容量或底层数组变化的操作都是必要的。要正确地使用slice，需要记住尽管底层数组的元素是间接访问的，但是slice对应结构体本身的指针、长度和容量部分是直接访问的。


## 遍历

for 循环的 range 格式可以对 slice 或者 map 进行迭代循环。

当使用 for 循环遍历一个 slice 时，每次迭代 range 将返回两个值。 第一个是当前下标（序号），第二个是该下标所对应元素的一个拷贝。

```go
import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```




