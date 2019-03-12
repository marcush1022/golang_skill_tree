# **FUNCTION**

> **"在 golang 中，函数是 first-class，函数的类型也是一等的数据类型."**  
函数不仅可以用于封装代码、分割功能、解耦逻辑，还可以化身为普通的值在其他的函数之间进行传递、赋予变量、做类型判断和转换等，就像切片和字典一样.  

> **而更深层的含义就是：函数值可以由此成为能够随机传播的独立逻辑组件.**    

> 对于函数类型来说，它是一种对一组输入和输出进行模板化的工具，比接口更加灵活，它的值也因此成为了可被热替换的逻辑组件.    

```
import "fmt"

// 函数签名, 参数列表的左边不是函数名称, 而是 func 关键字
type Printer func(contents string) (n int, err error) 

// 
func printToStd(contents string) (bytesNum int, err error) {
    return fmt.Println(contents)
}

func main() {
    var p Printer
    p = printToStd
    p("something")
}
```

> **函数签名其实就是函数的参数列表和结果列表的统称，它定义了可以用来鉴别不同函数的那些特征，也定义了与函数的交互的方式.**    

> **只要两个函数的参数列表和结果列表中的元素的顺序及其类型是一致的，则就可以说他们是一样的函数，或者是实现了同一个函数类型的函数.**  

> **printToStd 的签名与 Printer 是一致的，因此前者是后者的一个实现，即使他们的函数名称及部分结果名称不同.**  

**函数是 first-class 是函数式编程（function programming）的重要特征，golang 在语言层面支持了函数式编程.**  