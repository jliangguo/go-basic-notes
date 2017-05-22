# Slice

一个slice会指向一个序列的值，并且包含了长度信息。

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



