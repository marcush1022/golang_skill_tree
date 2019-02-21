# **GENERIC PROGRAMMING**
**泛型编程(generic programming): 代码不是针对特定的类型(例如适用 int 不适用 string)有效, 而是对大部分类型的参数都是有效的**

**golang 不支持泛型, 但是可以用 interface 实现泛型**

> **duckTyping**: 判断一个对象的类型不是通过其类型定义来判断, 而是根据其是否满足某些特定的方法和属性判断  

## **I. golang 中的 interface**

- golang 中的 interface 是一组方法的集合, 可视为一种定义内部方法的动态数据类型, 任意实现了这些方法的类型都可视为该动态数据类型  
- 不关心数据, 只关心行为
- golang 中有两种 interface, 一种是类型, 一种是定义

## **II. interface{} 作为任意值**

```
func foo(v interface{}) {}
```
此时 interface 可以传入任意类型的值(int, string, 结构体等)

### **1. 类型断言(Type Assertion)**  
传入的参数为 interface 类型, 通过类型断言来获得参数类型

```
func foo(value interface{}) {
	switch v:=value.(type) {
		case string :
			fmt.Println("is string!")
		case int :
			fmt.Println("is int!")
	}
}
```

### **2. 指定类型**
通过 value.(类型) 将 interface 传入的参数转化为指定类型

```
func foo(value interface{}) {
	fmt.Println("value is a string, read as: "+value.(string))
}
```

## **III. interface 作为接口**

```
type Database interface {
	Read()
	Write()
}
```

**Mysql 结构体实现了 Database 接口:**

```
type Mysql struct {}

func (m Mysql) Read() {}
func (m Mysql) Write() {}
```

## **IV. 使用interface的优点**

### **1. Write generic algorithm(泛型编程)**    
例: 泛型编程, 标准库的 sort  

```
type Interface interface {
	Len() int
	Less(i, j int) bool
	Swap(i, j int)
}

func sort(data Interface) {
	n:=data.Len()
	maxDepth:=0
	for i:=n; i>0; i>>1 {
		maxDepth++
	}
	maxDepth*=2
	quickSort(data, 0, n, maxDepth)
}
```
sort 函数的形参是 Interface, 含有3个方法, 任何实现了这3个方法的元素类型都可以使用 sort 函数

```
type Person struct {
	Name string
	Age int
}
```

Person 实现了 Interface 的3个方法

```
type ByAge []Person

func (a ByAge) Len() int { return len(a) }
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }
func (a ByAge) Swap(i, j int) { a[i], [j]=a[j], a[i] }

//package sort
func main() {
	people:=[]Person {
		{"a", 1},
		{"b", 2},
		{"c", 3},
	}
	//使用interface Sort
	sort.Sort(ByAge([people]))
}
```

### **2. Hiding Implement Detail(隐藏实现细节)**
...


## **V. golang的方法与函数**

函数:
```
func foo() { fmt.Println("this is a function") }
```

方法:
```
type Foo struct {
	name string
	id int
}
func (f Foo) foobar() { fmt.Println("this is method") }
```