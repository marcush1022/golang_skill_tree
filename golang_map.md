# **DIRECTORY (MAP)**

**map(映射)的键可以是任何相等性操作支持的数据类型, 整型, 浮点, 字符串, 指针, 接口, 结构, 数组等.**  

**映射也是引用类型, 将映射传入函数, 函数内改变了映射的内容会影响函数外.**

映射赋值和值的获取类似数组
```
offset:=timeZone["EST"]
```

## **在值为 nil 的 map 上进行读操作**
**通过映射中不存在的元素取值时, 返回的是与该 键 类型一致的零值(0、"" 等).**  
**除了添加键值对，在一个值为 nil 的 map 上进行的任何操作都不会报错(删除 delete() 也不报错)**

判断元素是否存在:
```
func offset(tz string) int {
	if second, ok:=timeZone[tz]; ok {
		return seconds
	}
	return 0
}
```
若tz存在, second 会赋予相应的值, ok 为 true, 若不存在, second 赋予相应的零值, ok 为 false.

仅判断元素是否存在, _ 代替一般变量:

```
_ , present:=timeZone[t]
```

内建函数 delete 删除映射的某项.
```
delete(timeZone, "est")
```

## **二维 map**

```
top := make(map[int]map[int]string)
button := make(map[int]string)
top[0]=button
button[0]="a"

// print:
top[0][0] = "a"
top[0][1] = ""

button2 := make(map[int]string)
button2[1]="b"
top[0]=button2

// print:
top[0][1] = "b"
top[0][0] = ""
```
top[0][0] 的数据丢失，取而代之的是 top[0][1] 的数据，Golang 直接把 top[0] 的数据由 button 替换成了 button2，不会递归地向 map 中添加缺失的数据.

增加额外的判断
```
if _, exist := m["a"]; exist{
    m["a"]["c"] = 2
}else{
    tmp := make(map[string]int)
    tmp["c"] = 2
    m["a"] = tmp
}
```
每次创建一个一维 map 都要 make() 一次，不然就会 panic. 多维 map 每加一层都要多 make() 好几次