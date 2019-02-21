# **BUILD IN (METHODS AND TYPES)**

## **I. FUNC**

### **1. func append(slice []type, elems ...type) []type**  

appends elements to the end of slice; if it has sufficient place, the destination is resliced to accommodate the 
new elements; if it does not, a new underlying array will be allocated; append returns the updated slice;

### **2. func cap(v type) int**

**cap returns the capacity of v**, according its type:   
array: the number of elements in v, same as len(v);  
pointer to array: the number of elements in * v, same as len(*v);  
slice: the max capacity of slice, if v is nil, cap(v) is 0;  
channel: the channel buffer capacity;  

### **3. func close(c chan<- type)**  

close a channel;

### **4. func complex(r, i floatType) ComplexType**

returns a complex values of given float value;

### **5. func delete(m map[type]type, key type)**

**delete the element with a given key from a map**;  
if the map is nil or given key not found, delete func is a no-op(no operation);

### **6. func len(v type) int**

returns the len of v, according v's type:  
array: the number of elements in v;  
pointer to array: the number of elements in *v;  
string: the number of bytes in v;  
channel: **the number of elements queued(unread) in channel buffer**, if v is nil. len(v) is 0;   

### **7. func make(t type, size ...integerType) type**
allocate and initialized an object of type in slice, map, or channel;  
make's return type is same as its arguement(unlike new);  

### **8. func new(type) * type**
returns a pointer to a newly allocated zero value of that type;

### **9. func panic(v interface{})**
stops normal execution of the current goroutine immediately;

### **10. func print(args ...type)**
formats its arguements in a implementation-specific way;

### **11. func println(args ...type)**
formats its arguements in a implementation-specific way and it will always starts at newline;

### **12. func recover() interface{}**
**a handle function of a panic;**

## **II. TYPE**

- bool  
- byte  
- complex128  
- complex64  
- error   
  (
      a built in interface type, is the conventional inteface for representing an error condition, with nil value representing no error;
      type error inteface {
          Error() string 
      }
  )
- float32
- float64
- int
- int16
- int32
- int64
- int8
- rune (rune is an alias for int32 and is equivalent in all ways; convention use it to distinguish charater values   from integers)
- string
- uint
- uint16
- uint32
- uint64
- uint8
- uintptr (an integer type that is large enough to hold the bit pattern of any pointer)



