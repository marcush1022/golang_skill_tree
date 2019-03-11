# **ADVANCED CHANNEL**

## **I. 单向通道**

只能接收的通道。  
```
var uselessChan = make(chan<- int, 1)
```
> "通道的作用是用来传递数据，而声明一个只有一端的通道没有任何意义。"

## **II. 单向通道的作用**

> "单向通道的主要的用途就是约束其他代码的行为"

单向通道，ch 只能接收元素.
```
func SendInt(ch chan<- int) {
    ch <- rand.Intn(1000)
}

type Notifier Interface {
    SendInt(ch chan<- int)
}
```

调用 SendInt 只需要将一个元素类型匹配的双向通道传入即可，golang 会自动将双向通道转化为函数所需的单向通道.

```
intChan1 := make(chan int, 3)
SendInt(intChan1)
```

在函数的返回值中使用单项通道，该函数的调用方只能从函数的返回值中接收元素，即对函数调用方的一种约束.

```
func getIntChan() <-chan int {
    num := 5
    ch := make(chan int, num)
    for i := 0; i < num; i++ {
        ch <- i
    }
    close(ch)
    return ch
}
```

调用 getIntChan()：

```
intChan2 := getIntChan()
for elem := range intChan2 {
    fmt.Println(elem)
}
```

- for 循环打印会不断地从 intChan2 中取出元素值，即使通道关闭，仍然会继续取出剩余的元素值之后再结束执行.  
- 当 intChan2 中元素可取时，for 所在的那一行会被阻塞，直至有新的元素可取.
- 假设 intChan2 的值为 nil，则 for 所在的那一行会一直被阻塞.

## **III. select 与通道**

select 只能与通道连用，  
select 语句是专为通道设计的，因此其每一个 case 中只能包含通道操作表达式.  

```
intChannels := [3]chan int{
    make(chan int, 1),
    make(chan int, 1),
    make(chan int, 1),
}
index := rand.Intn(3)
intChannels[index] <- index
select {
    case <- intChannels[0]:
        doSomething1
    case <- intChannels[1]:
        doSomething2
    case <- intChannels[2]:
        doSomething3
    default:
        doSomething4
}
```

select 在所有的 case 都阻塞或者没有满足条件时，default 分支就会执行，没有 default 时 select 会阻塞.  


