# **RANGE**

**range 用来遍历 array, slice, map, string, channel**

对于 array, *array, string 返回的值分别是数据的索引和值, 遍历 map 返回的是 key 和 value，遍历 channel 时，只有一个返回数据;

| range expression | 1st value | 2nd value |
|:-----------------|:----------|:----------|
| array ([n]E, *[n]E) | index (int) | value (E[i]) |
| slice ([]E) | index (int) | value (E[i]) |
| string (abcd) | index (int) | rune (string迭代的是unicode而不是字节，所以返回的值是rune) |
| map (map[k]v) | key (k) | value (v) |
| channel | element | [none] |

## **I. EXAMPLE 01**

```
func modifySlice() {
    v := []int{1, 2, 3, 4}
    for i := range v {
        v = append(v, i)
        fmt.Printf("Modify Slice: value:%v\n", v)
    }
}
```

**输出:**
```
[1 2 3 4 0]
[1 2 3 4 0 1]
[1 2 3 4 0 1 2]
[1 2 3 4 0 1 2 3]
```
**输出不会无限循环，range在执行之前构建了range表达式的内容;**

## **II. EXAMPLE 02**

```
func modifyMap() {
    data := map[string]string{"a": "A", "b": "B", "c": "C"}
    for k, v := range data {
        data[v] = k
        fmt.Println("modify Mapping", data)
    }
}
```

**输出:**
```
map[1:a 2:b 3:c c:3]
map[1:a 2:b 3:c c:3]
map[2:b 3:c c:3 a:1 1:a]
map[1:a 2:b 3:c c:3 a:1 b:2]
```

```
map[1:a 2:b 3:c c:3]
map[1:a 2:b 3:c c:3]
map[1:a 2:b 3:c c:3 a:1]
map[a:1 b:2 1:a 2:b 3:c c:3]
map[2:b 3:c a:1 1:a]
map[b:2 1:a 2:b 3:c a:1]
map[1:a 2:b 3:c a:1 b:2 c:3]
map[1:a 2:b 3:c a:1 b:2 c:3]
map[a:1 b:2 c:3 1:a 2:b 3:c]
```

```
map[1:a 2:b 3:c c:3]
map[1:a 2:b 3:c c:3]
map[1:a 2:b 3:c c:3 a:1]
map[1:a 2:b 3:c c:3 a:1 b:2]
map[1:a 2:b 3:c a:1]
map[1:a 2:b 3:c a:1 b:2]
map[a:1 b:2 c:3 1:a 2:b 3:c]
map[a:1 b:2 c:3 1:a 2:b 3:c]
map[1:a 2:b 3:c a:1 b:2 c:3]
map[2:b 3:c c:3 1:a]
map[1:a 2:b 3:c c:3]
map[2:b 3:c c:3 a:1 1:a]
map[1:a 2:b 3:c c:3 a:1 b:2]
```

**输出不确定:**
- **the iteration order over map is not specified, and is not guaranteed to be the same from one iteration to the next;**
- **if a map entry(key) has not yet been reached is removed during the iteration, the corresponding iteration value will not be iterated;**
- **if a map entry is created during the iteratioin, the entry may be reached during the iteratioin or may be skipped;**

## **III. RANGE STRING**

```
func rangeString() {
    datas := "aAbB"
    for k, d := range datas {
        fmt.Printf("k_addr:%p, k_value:%v\nd_addr:%p, d_value:%v\n***********************\n", &k, k, &d, d)
    }
}
```

```
k_addr:0xc0420101b0, k_value:0
d_addr:0xc0420101b8, d_value:65
***********************
k_addr:0xc0420101b0, k_value:1
d_addr:0xc0420101b8, d_value:97
***********************
k_addr:0xc0420101b0, k_value:2
d_addr:0xc0420101b8, d_value:66
***********************
k_addr:0xc0420101b0, k_value:3
d_addr:0xc0420101b8, d_value:98
***********************
```

## **IV. RANGE 使用指针**

**只有array能以指针形式使用range;**

```
func rangePointer() {
    d := [5]int{1, 2, 3, 4, 5} //range successfully

    //d := "aAbBcCdD"
    //compile error: cannot range over datas (type *string)
    //d := []int{1, 2, 3, 4, 5} //compile error: cannot range over datas (type *[]int)
    //compile error: cannot range over datas (type *[]int)
    //d := make(map[string]int) //compile error: cannot range over datas (type *map[string]int)
    //compile error: cannot range over datas (type *map[string]int)

    datas := &d
    for k, d := range datas {
        fmt.Printf("k_addr:%p, k_value:%v\nd_addr:%p, d_value:%v\n----\n", &k, k, &d, d)
    }
}
```

```
k_addr:0xc042052110, k_value:0
d_addr:0xc042052118, d_value:1
---------------------
k_addr:0xc042052110, k_value:1
d_addr:0xc042052118, d_value:2
---------------------
k_addr:0xc042052110, k_value:2
d_addr:0xc042052118, d_value:3
---------------------
k_addr:0xc042052110, k_value:3
d_addr:0xc042052118, d_value:4
---------------------
k_addr:0xc042052110, k_value:4
d_addr:0xc042052118, d_value:5
---------------------
```