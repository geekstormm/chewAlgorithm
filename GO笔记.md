# GO笔记

## 数组array和切片slice

### 数组的声明

```go
var a [3]int	//声明并初始化为0
a[0] = 1

b := [3]int{1, 2, 3}//声明的同时初始化
c := [2][2]int{{1,2}, {3,4}}//多维数组初始化
```

### 数组的分片

和python类似

```go
arr := [...]int{1, 2, 3, 4, 5}
arr_sec := arr[1:3]
t.Log(arr_sec)	//2,3
t.Log(arr[:3])	//1,2,3
t.Log(arr[3:])	//4,5
```

### 切片slice

slice就是C++里面的vector

```go

```

**切片的内部结构**

![截屏2022-07-20 17.17.38](/Users/lgs/Desktop/截屏2022-07-20 17.17.38.png)



