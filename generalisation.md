# Generalisation - 泛型

## interface - 接口

> An **interface** defines it in terms of a set of method signatures which it must implement.

**接口类型**是一种抽象的类型。它不会暴露出它所代表的对象的内部值的结构和这个对象支持的基础操作的集合；它们只会展示出它们自己的**方法**。也就是说当你有看到一个接口类型的值时，你不知道它是什么，唯一知道的就是可以通过它的方法来做什么。

### empty interface

The interface type that specifies zero methods is known as the **empty interface**:

```go
interface{}
```

An empty interface may hold values of any type. (Every type implements at least zero methods.)

**Empty interfaces are used by code that handles values of unknown type**. For example, `fmt.Print` takes any number of arguments of type `interface{}`.

### type assertions

A **type assertion** provides access to an interface value's underlying concrete value.

```go
t := i.(T)
```

This statement asserts that the interface value `i` holds the concrete type `T` and assigns the underlying `T` value to the variable `t`.

If `i` does not hold a `T`, the statement will trigger a panic.

To _test_ whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.

```go
t, ok := i.(T)
```

If `i` holds a `T`, then t will be the underlying value and `ok` will be true.

If not, `ok` will be false and `t` will be the zero value of type `T`, and no panic occurs.

### type switch

A **type switch** is a construct that permits several type assertions in series.

A type switch is like a regular switch statement, but the cases in a type switch specify types (not values), and those values are compared against the type of the value held by the given interface value.

```go
switch v := i.(type) {
case T:
    // here v has type T
case S:
    // here v has type S
default:
    // no match; here v has the same type as i
}
```

The declaration in a type switch has the same syntax as a type assertion `i.(T)`, but the specific type `T` is replaced with the keyword type.

### Stringer

One of the most ubiquitous interfaces is `Stringer` defined by the `fmt` package.

```go
type Stringer interface {
    String() string
}
```

A `Stringer` is a type that can describe itself as a string. The `fmt` package (and many others) look for this interface to print values.
