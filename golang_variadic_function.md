# **VARIADIC FUNCTION**
**可变参数函数**  
**accepts variable number of parameters and use three dots in front of an input type makes it variadic.**

```
func foo(names ...string)
// string will passed as a slice
```


## **I. A SIMPLE VARIADIC FUNCTIOIN**

```
func foo(names ...string) returnvalue {
    // do something;
}
```

**can pass zero or more parameters**

```
foo("param1", "param2")
foo("param")
foo()
```

## **II. WHEN USE VF?**

**1. 避免传入仅作为临时参数的切片**  
**2. 参数的数量未知**  
**3. 增加可读性**  

## **III. EXAMPLE**

```
// fmt.Println(), 通过可变参数函数接收不定数量的参数;
func Println(a ...interface{})

// 若不使用可变参数函数;
func Println(params []interface{})

// 通过传入切片来使用;
fmt.Println([]interface{} {"foo", "bar"})

// 使用变参函数的形式;
fmt.Println("foobar")
fmt.Println("foo", "bar")
fmt.Println()
```

## **IV. 切片和可变参数**

**变参函数会在其内部生成一个新的切片，可变参数简化了切片参数传递**

**1. 不传参数**  
不传参数时，可变参数会成为一个空值切片  
所有的切片都有内建数组，空值切片没有  

**2. 已有切片传入变参函数**  
在已有切片后加...将其传入变参函数  
```
names:= []string{"foo", "bar"}
foobar(names)
// 同: foobar("foo", "bar")
```
差异: 传入的参数会直接使用而不是创建一个新的

**3. 传入已有切片在函数内对其进行改变会影响源切片**  
传入切片和函数内使用的切片使用同一个底层数组  

**4. 传入连接的切片**  
```
foobar := []string{"bbb", "ccc"}
// 在foobar前加"aaa":
somefunction(append([]string{"aaa"}, foobar...)...)
// output: "aaa bbb ccc"
// 同: names = append([]string{"aaa"}, "bbb", "ccc")
somefunction(names...)
```

**5. 接收多种类型**
例: Printf()可接受多种类型的参数, 实现是通过将类型声明为空接口类型(interface type), 因此函数可以接受类型和参数都不确定的参数  
```
func Printf(format string, args ...interface{}) (n int, err error) {
    return Fprintf(os.Stdout, format, a...)
}

fmt.Printf("%d, %s", 1, "aaa")
```






