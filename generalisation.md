# Generalisation - 泛型

## interface - 接口

> An **interface** defines it in terms of a set of method signatures which it must implement.

**接口类型**是一种抽象的类型。它不会暴露出它所代表的对象的内部值的结构和这个对象支持的基础操作的集合；它们只会展示出它们自己的**方法**。也就是说当你有看到一个接口类型的值时，你不知道它是什么，唯一知道的就是可以通过它的方法来做什么。

## type assertions

A **type assertion** provides access to an interface value's underlying concrete value.

```go
t := i.(T)
```

This statement asserts that the interface value `i` holds the concrete type `T` and assigns the underlying `T` value to the variable `t`.

If `i` does not hold a `T`, the statement will trigger a panic.




