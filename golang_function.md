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

// 使用闭包函数的代码
x, y = 56, 78
add := genCalculator(op)
resutl, err := add(x, y)
fmt.Println("result = ", result)
```

### **Q1: 如何实现闭包**

**闭包是什么:**  

在一个函数中存在对外来标识符的引用，外来标识符既不代表当前函数的任何参数和结果，也是函数内部声明的.  
有个专门的术语：**自由变量**.  

genCalculator 定义了一个**匿名的 calculateFunc 类型**的函数并将其作为结果返回.  
**这个匿名的函数就是一个闭包函数，它使用的变量 op 既不是他的任何参数或结果，也不是他自己声明的，而是定义它的 genCalculator 函数的参数，所以是一个自由变量**。   
这个自由变量代表了什么并不是在定义闭包函数时确定的，而是在 genCalculator 被调用时确定的，**只有给定了该函数的参数 op，才知道其返回的闭包函数可以进行什么运算**。  

当执行到 op == nil 时，go 编译器就会去试图寻找 op 代表的东西，然后发现 op 代表的是 getCalculator 函数的参数，然后将这两者联系起来.  

当程序运行到这里时，op 就是那个参数确定了，这个闭包函数的状态就由不确定变成了确定，或者说转到了闭合的状态，至此也就真正形成了一个闭包.  

**用高阶函数实现闭包是高阶函数的一个作用.**     

**实现闭包的意义:**  

动态生成部分程序逻辑，可以在程序的运行过程中根据需要的生成不同的函数，与 GoF 设计模式中的 "模板方法" 设计模式类似.  

### **Q2: 传入函数的参数怎么样了**

```
package main

import "fmt"

func main() {
    array1 := [3]string{"a", "b", "c"}
    fmt.Println("original array1 = ", array1)
    array2 := modifyArray(array1)
    fmt.Println("modified array1 = ", array1)
    fmt.Println("modified array2 = ", array2)
}

func modifyArray(arr [3]string) [3]string {
    a[1] = "x"
    return a
}
```

原数组 array1 没有改变，任何传入函数的参数都会被复制，函数在其内部使用的参数是一个副本，并不是参数的原值.  
由于数组是值类型，每一次复制都会拷贝它，以及它所有的元素值.  

对于引用类型，slice, map, channel 像上面那样复制它们的值只会拷贝他们本身而已，并不会复制底层的数据，只是浅拷贝不是深拷贝.  

以切片值为例，如此复制的时候只是会复制指向其底层数组的指针，以及他的长度和容量值，底层数组并不会被拷贝.  

注意：即使传入函数的参数是一个值类型的参数值，但如果这个参数中的某个值是引用类型的: 

```
complexArray1 := [3][]string{
    []string{"d", "e", "f"},
    []string{"g", "h", "i"},
    []string{"a", "b", "c"},
}
```
