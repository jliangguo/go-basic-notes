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

append函数就理解为往slice中加入一个值，如果未达到容量（len<cap）那么就直接往数组中加值，如果达到容量（len = cap）那么就增加一个新的元素空间，将值放在里面


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




