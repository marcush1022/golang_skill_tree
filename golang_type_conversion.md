# **TYPE CONVERSION**
**Golang 类型转换整理**

## **I. 整型到字符串**

```
var numberInt int = 123
var numberString string
numberString, err := strconv.Itoa(numberInt)
或者
numberString = FormatInt(numberInt)
```

## **II. 字符串到整型**

```
var numberString string = "123"
var numberInt int
numberInt, err := strconv.Atoi(numberString)
或
numberInt = ParseInt(numberString)
```

## **III. 字符串到 float**

```
var floatString string = "3.14"
var floatNumber float32
floatNumber, err := ParseFloat(32) 
```

## **IV. int 与 float 互转**

```
float(i) 或 int(f)
```