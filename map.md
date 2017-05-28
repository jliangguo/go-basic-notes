# map

map是键值对的映射，类型可以写为`map[K]V`，其中K和V分别对应key和value。key必须是支持==比较运算符的数据类型，所以map可以通过测试key是否相等来判断是否已经存在。

如何创建一个map呢？make可以创建空的map，比如：

```go
m = make(map[string]Vertex)
m["Bell Labs"] = Vertex{ 
	40.68433, -74.39967,
}
fmt.Println(m["Bell Labs"])
```
也可通过如下**map字面值的语法**方式显式初始化：

```go
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}

// or like these:
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```

## 修改map

在map中插入一个元素或修改一个元素：

```go
m[key] = elem
```

获取元素：

```go
elem = m[key]
```

删除元素：

```go
delete(m, key)
```

通过双赋值检测某个键存在：

```go
elem, ok = m[key]
```

如果 key 在 m 中， ok 为 true。否则， ok 为 false，并且 elem 是 map 的元素类型的零值。

## 关于Set

Go语言中并没有提供一个set类型，但是map中的key也是不相同的，可以用map实现类似set的功能。

下面程序通过map来表示所有的输入行所对应的set集合，以确保已经在集合存在的行不会被重复打印。

```go
func main() {
    seen := make(map[string]bool) // a set of strings
    input := bufio.NewScanner(os.Stdin)
    for input.Scan() {
        line := input.Text()
        if !seen[line] {
            seen[line] = true
            fmt.Println(line)
        }
    }

    if err := input.Err(); err != nil {
        fmt.Fprintf(os.Stderr, "dedup: %v\n", err)
        os.Exit(1)
    }
}
```



