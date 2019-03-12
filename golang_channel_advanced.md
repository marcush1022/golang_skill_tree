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

1. select 在所有的 case 都阻塞或者没有满足条件时，default 分支就会执行，没有 default 时 select 会阻塞.  
2. select 语句的分支若阻塞相当于未满足选择条件.   
3. 仅当 select 的分支都求值完成之后，才会开始选择候选分支。这时会选择满足条件的候选分支执行，若所有的候选分支都不满足条件，则会选择默认分支执行。若没有默认分支，则 select 就会阻塞，直到至少有一个候选分支满足条件。一旦有一个候选分支满足条件，select语句（或者它所在的 goroutine）就会被唤醒，这个候选分支就会被执行.  
4. 若 select 语句发现同时有多个候选分支满足选择条件，则会使用一种伪随机算法在这些分支中选择一个执行.  
5. select 只能有一个默认分支，并且只有在没有满足条件的候选分支时才会执行，与默认分支的位置无关.  


## **RECAP**

1. 可以使用 range 和 for 从通道中获取数据，也可以使用 select 来操纵通道.  
2. select 是专门为通道设计的，可以包含多个候选分支，每个分支的 case 表达式中都会包含针对某个通道的发送或接收操作.  
3. 发送和接收的阻塞是 select 进行分支选择的重要依据.

## **QUS**

如果在 select 语句中发现某个通道已经关闭，则怎么屏蔽掉该通道所在的 select 分支？  

```
for {
    select {
        case _, ok := <-=ch1:
            if !ok {
                ch1 = make(chan int)
            }
        case something:
            do something
        default:
            do something
    }
}
```




