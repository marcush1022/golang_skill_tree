# **STRUCT**

## **I. 函数与方法**

函数的独立的程序实体，可以声明有名字和无名字的函数，也可以将其当作值传递。方法不同，需要有名字，不能当作值来进行传递，而且必须隶属于某一种类型，方法所属的类型可以通过其声明中的**接收者**体现出来。

接收者类型就是当前方法的所属类型，而接收者的名称就是在当前方法中引用他的所属类型的当前值。  

```
type AnimalCategory strcut {
    kingdom string
    phylum string
    class string
    order string
    family string 
    geuns string
    species string
}

func (ac AnimalCategory) String() string {
    return fmt.Sprintf("%s%s%s%s%s%s%s", ac.kingdom, ac.phylum, ac.class, ac.order, ac.family, ac.geuns, ac.species)
}
```

引用结构体的嵌入字段:  

```
type Animal struct {
    scientificName string
    AnimalCategory // 代表了 Animal 类型的一个嵌入字段，也可以成为匿名字段
}

func (a Animal) Category() string {
    return a.AnimalCategory.String()
}
```