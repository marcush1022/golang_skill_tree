# **CHANNEL**

**"通道类型的值本身就是并发安全的，这是 Golang 自带的唯一一个可以满足并发安全的类型."**

## **I. Channel 3种类型**

```
chan T // 可用于接收或发送 T 类型的数据
chan<- float64 // 只可用来发送 float64 类型的数据
<-chan int // 只可用来接收 int 类型的数据
```

使用 make 初始化 channel，并且可以设定容量。容量为 channel 可以容纳的最多的元素数量，代表 channel 缓存的大小。  
如果没有设置容量，或者容量设置为0，则说明 channel 没有缓存，只有 sender 和 receiver 都准备好了才可以通讯。  
如果设置了缓存，就有可能不发生阻塞，只有 buffer 满了 send 才会阻塞，只有缓存空了之后 receive 才会阻塞。  
一个 nil channel 不会通信。  

```
make(chan int, 100)
```

可以通过内建方法 close 关闭 channel。  
可以从多个 goroutine 向一个 channel 发送或接收数据，不需要额外考虑同步措施。  
channel 可以作为一个 FIFO 队列，接收数据的顺序和发送数据的顺序是一致的。  
channel 的 receive 支持 multiple assignment:  

```
v, ok := <-cha // 可以用来检查通道是否关闭
```

## **II. channel 发送和接收操作的基本特性**

### **A. 对于同一个 channel，发送操作时间是互斥的，接收操作之间也是互斥的**

### **B. 发送操作和接收操作中对元素的处理都是不可分割的**

### **C. 发送操作在完成之前会被阻塞，接收操作也是**


