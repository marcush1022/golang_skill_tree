# **FUNCTION**

## **I. Overview**

> **"在 golang 中，函数是 first-class，函数的类型也是一等的数据类型."**  
函数不仅可以用于封装代码、分割功能、解耦逻辑，还可以化身为普通的值在其他的函数之间进行传递、赋予变量、做类型判断和转换等，就像切片和字典一样.  

> **而更深层的含义就是：函数值可以由此成为能够随机传播的独立逻辑组件.**    

> 对于函数类型来说，它是一种对一组输入和输出进行模板化的工具，比接口更加灵活，它的值也因此成为了可被热替换的逻辑组件.    

```
import "fmt"

// 函数签名, 参数列表的左边不是函数名称, 而是 func 关键字
type Printer func(contents string) (n int, err error) 

func printToStd(contents string) (bytesNum int, err error) {
    return fmt.Println(contents)
}

func main() {
    // 先声明一个函数再将其赋值给另一个变量
    var p Printer
    p = printToStd
    p("something")
}
```

> **函数签名其实就是函数的参数列表和结果列表的统称，它定义了可以用来鉴别不同函数的那些特征，也定义了与函数的交互的方式.**    

> **只要两个函数的参数列表和结果列表中的元素的顺序及其类型是一致的，则就可以说他们是一样的函数，或者是实现了同一个函数类型的函数.**  

> **printToStd 的签名与 Printer 是一致的，因此前者是后者的一个实现，即使他们的函数名称及部分结果名称不同.**  

**函数是 first-class 是函数式编程（function programming）的重要特征，golang 在语言层面支持了函数式编程.**  

## **II. 高阶函数**

### **什么是高阶函数**

高阶函数满足以下条件：  
1. 接受其他的函数作为输入；
2. 把其他的函数作为结果返回。 

满足其中任意一个条件的函数即为高阶函数。    
**高阶函数也是函数式编程的重要概念和特征。**  

### **a. 接收其他函数作为传入**

**EXP: 通过 calculate 函数来实现两个整数之间的加减乘除运算，但是希望两个整数和具体的操作都由函数的调用方给出.**

```
// 可以先声明一个函数
type operate func(x, y int) int
```

```
// 传入操作函数 operate 作为参数
func calculate(x int, y int, op operate) (int, error) {
    // 卫述语句
    if op == nil {
        return 0, errors.New("Invalid operate")
    }
    return op(x, y), nil
}
```

> 注：卫述语句是指用来检查函数的关键语句的合法性，并在检查未通过的情况下终止函数运行，GO 中通常使用 if 作为卫述语句。

```
// 直接实现一个 operate 类型的匿名函数
op := func(x, y int) int {
    return x + y
}
```

上述 calculate 函数就是一个高阶函数，展示了高阶函数的一个特点：**接收其他的函数作为参数**。  

### **b. 把其他的函数作为结果返回**

**EXP:**

```
func genCalculator(op operate) calculateFunc {
    return func (x int, y int) (int, error) {
        if op == nil {
            return 0, errors.New("invalid operate")
        }
        return op(x, y), nil
    }
}
```

genCalculator 定义了一个匿名的 calculateFunc 类型的函数并将其作为结果返回，**而这个匿名的函数就是一个闭包函数，它使用的变量 op 既代表他的任何参数或结果也不是他自己声明的，而是定义它的 genCalculator 的参数，所以是一个自由变量**。   
这个自由变量代表了什么并不是在定义闭包函数时确定的，而是在 genCalculator 被调用时确定的，**只有给定了该函数的参数 op，才知道其返回的闭包函数可以进行什么运算**。  
