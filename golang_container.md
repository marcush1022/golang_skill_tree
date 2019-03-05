# **I. CONTAINER**

## **container/list**

list 为双向链表.  
MoveBefore / MoveAfter: 将一个给定元素移动到另一个元素的前面和后面。
```
func (l *List) MoveBefore(e, point *Elemet) // *Elemet 为元素类型的指针
func (l *List) MoveAfter(e, point * Elemet)
```
MoveToFront / MoveToBack: 将链表中的元素移动到链表的最前端和最后端。  
```
func (l *List) MoveToFront(point *Elemet) // *Elemet 为元素类型的指针
func (l *List) MoveToBack(point *Elemet)
```


EXP:
```
import (
	"fmt"
	"container/list"	
)

func main(){

	//创建一个双向链表
	ls := list.New()

	//向双向链表中插入26个小写字母
	for i := 97; i < 123; i++ {
		ls.PushFront(i)     //PushFront()代表从头部插入，同样PushBack()代表从尾部插入
	}

	//遍历双向链表ls中的所有字母
	for it := ls.Front(); it != nil; it = it.Next() {
		fmt.Printf("%c ", it.Value)
	}
	fmt.Println()
}
```

## **container/heap**

```
import (
	"fmt"
	"container/heap"	
)

//heap提供了接口，需要自己实现如下方法
type Heap []int

//构造的是小顶堆，大顶堆只需要改一下下面的符号
func (h *Heap) Less(i, j int) bool {
	return (*h)[i] < (*h)[j]
}

func (h *Heap) Swap(i, j int) {
	(*h)[i], (*h)[j] = (*h)[j], (*h)[i]
}

func (h *Heap) Len() int {
	return len(*h)
}

func (h *Heap) Pop() interface{} {
	x := (*h)[h.Len() - 1]
	*h = (*h)[: h.Len() - 1]
	return x
}

func (h *Heap) Push(x interface{}) {
	*h = append(*h, x.(int))
}

func (h *Heap) Remove(idx int) interface{} {
	h.Swap(idx, h.Len() - 1)
	return h.Pop()
}

func main(){
	
	//创建一个heap
	h := &Heap{}
	
    heap.Init(h)
	//向heap中插入元素
	h.Push(5)
	h.Push(2)
	h.Push(1)
	h.Push(8)
	h.Push(4)
	h.Push(6)
	h.Push(2)

	//输出heap中的元素，相当于一个数组，原始数组
	fmt.Println(h)

	//这里必须要reheapify，建立好堆了
	heap.Init(h)

	//小顶堆对应的元素在数组中的位置
	fmt.Println(h)

	//移除下标为5的元素，下标从0开始
	h.Remove(5)

	//按照堆的形式输出
	for h.Len() > 0 {
		fmt.Printf("%d ", heap.Pop(h))
	}
	fmt.Println()
}
```

## **container/ring**

ring是一个环形链表.  