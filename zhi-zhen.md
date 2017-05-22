# 指针

Go保留了C/C++中值和指针的区别，但是对于指针繁琐用法进行了大量的简化，引入引用的概念。当指针作为参数传递时比较轻量级（8 bytes），即只传递内存地址，可以通过引用机制改变指针的指向值，不需要copy对象，在传递体积较大的结构体时较为快速。

Go中的指针和C/C++相同的地方有：

- 指针`*T`
- 指针的指针`**T`
- 通过operator`*`访问指针指向的对象，`&`获取对象的内存地址

不同的地方有：

- 指针的默认值（零值，zero value）为`nil`，而不是`NULL`
- 指针所指对象的成员使用`.`访问而非`->`
- 使用`unsafe.Pointer`可以对指向不同类型对象的指针进行转换
- 指针不能进行+、-运算，以保证安全性。



## References

1. [Go's unsafe.Pointer Pointer Type](http://learngowith.me/gos-pointer-pointer-type/)，关于unsafe.Pointer的总结很好。