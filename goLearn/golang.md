# Golang学习

## 一、基本语法

### 0.环境



### 1.数据类型与运算符

#### 1.1基本数据类型

-   变量

```go
package main

var a = "w3cschoolW3Cschool教程"
var b string = "w3cschool.cn"
var c bool

func main() {
	println(a, b, c)
}

```



```go
package main

import "fmt"

var x, y int
var ( //这种只能出现在全局变量中，函数体内不支持
	a int  = 4    //默认0
	b bool = true //默认false
)

var c, d int = 1, 2
var e, f = 123, "hello"
var roleName string

//这种不带声明格式的只能在函数体中出现
//g, h := 123, "hello"

func main() {
	g, h := 123, "hello"
	roleName = "123"
	fmt.Println(x, y, a, b, c, d, e, f, g, h)
	fmt.Println(roleName)
	//字符串拼接
	fmt.Println("Google" + "Runoob")
}

/*
关键字   保留字
break	default	func	interface	select
case	defer	go	map	struct
chan	else	goto	package	switch
const	fallthrough	if	range	type
continue	for	import	return	var
预定义标识符
append	bool	byte	cap	    close  complex	complex64	complex128	uint16
copy	false	float32	float64	imag	int	      int8	   int16	uint32
int32	int64	iota	len  	make	new	     nil	panic	uint64
print	println	real	recover	string	true	uint	uint8	uintptr
*/

```



-   数据类型

```go
/*
布尔
数字
	uint8  无符号 8 位整型 (0 到 255)
	uint16  无符号 16 位整型 (0 到 65535)
	uint32  无符号 32 位整型 (0 到 4294967295)
	uint64  无符号 64 位整型 (0 到 18446744073709551615)
	int8  有符号 8 位整型 (-128 到 127)
	int16  有符号 16 位整型 (-32768 到 32767)
	int32  有符号 32 位整型 (-2147483648 到 2147483647)
	int64  有符号 64 位整型 (-9223372036854775808 到 9223372036854775807)
	float32 IEEE-754 32位浮点型数
	float64  IEEE-754 64位浮点型数
	complex64  32 位实数和虚数
	complex128  64 位实数和虚数
	byte  类似 uint8
	rune  类似 int32
	uint  32 或 64 位
	int  与 uint 一样大小
	uintptr  无符号整型，用于存放一个指针
字符串
派生
	指针  Pointer
	数组
	结构化  struct
	联合体  union
	函数
	切片
	接口  interface
	Map
	Channel
*/

package main

import (
	"fmt"
	"unsafe"
)

//常量
const B string = "abc"  // 显示类型定义
const C = "abc"  //隐式类型
const A int = 5 

const D, E, F = 1, "2", 3  //多重赋值


const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)
)


//枚举
const (
    Unknown = 0  //未知性
    Female = 1  //女性
    Male = 2  //男性
)

//iota   它只能用在const的申明中，是一个从0开始的行数索引器
// const (
//     i = iota  //0
//     j = iota  //1
//     // k   //2
//     k = iota  //0
//     //l  //3
//     l  //0
// )


const (
    q = iota   //0
    w          //1
    r          //2
    t = "ha"   //独立值，iota += 1
    y          //"ha"   iota += 1
    u = 100    //iota +=1
    o          //100  iota +=1
    p = iota   //7,恢复计数
    s          //8
)



// func main() {
// 	fmt.Println("HelloWorld!")
// 	fmt.Println(B, C, D, E, F)
// 	fmt.Println(A * F)
// 	println(a, b, c)
// 	println(i, j, k, l)
// 	println(q, w, r, t, y, u, o, p, s)
// }


//==================================================================

const (
    i=1<<iota     //1
    j=3<<iota     //6
    k     //12
    l 		//24
)  
/*
iota表示从0开始自动加1，所以
i=1<<0,
j=3<<1（<<表示左移的意思），即：
i=1,j=6，
这没问题，关键在k和l，从输出结果看，
k=3<<2，l=3<<3
*/

func main() {    
	fmt.Println("i=",i)   
	fmt.Println("j=",j)   
	fmt.Println("k=",k)   
	fmt.Println("l=",l) 
} 
```

#### 1.2运算符

```go
package main

import "fmt"

/*
4算术运算符  +、- 、* 、/、 %    3
6关系运算符  ==、!=、>、>=、<、<=
逻辑运算符
	&&、与
	||、或
	！  非

5位运算符
	&  相与  参与运算的两数各对应的二进位相与  （两位均为1才为1）
	|  相或  参与运算的两数各对应的二进位相或  （两位有一个为1就为1）
	^  参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1 （两位不一样则为1）
	<<  左移n位就是乘以2的n次方。“a<<b”是把a的各二进位全部左移b位，高位丢弃，低位补0。
	>>  右移n位就是除以2的n次方。“a>>b”是把a的各二进位全部右移b位。

11赋值运算符
	=
	+=
	-=  相减后再赋值
	*=
	/=
	%=  求余后再赋值
	<<=  左移后赋值
	>>=  右移后赋值
	&=  按位与后赋值
	|=  按位或后赋值
	^=  按位异或后赋值
*/

func main() {

	var a int = 21
	var b int = 10
	var c int

	//算术运算符

	c = a + b
	fmt.Printf("第一行 - c 的值为 %d\n", c)
	c = a - b
	fmt.Printf("第二行 - c 的值为 %d\n", c)
	c = a * b
	fmt.Printf("第三行 - c 的值为 %d\n", c)
	c = a / b
	fmt.Printf("第四行 - c 的值为 %d\n", c)
	c = a % b
	fmt.Printf("第五行 - c 的值为 %d\n", c)
	a++
	fmt.Printf("第六行 - c 的值为 %d\n", a)
	a--
	fmt.Printf("第七行 - c 的值为 %d\n", a)

	//关系运算符
	if a == b {
		fmt.Printf("第一行 - a 等于 b\n")
	} else {
		fmt.Printf("第一行 - a 不等于 b\n")
	}

	if a < b {
		fmt.Printf("第二行 - a 小于 b\n")
	} else {
		fmt.Printf("第二行 - a 不小于 b\n")
	}

	if a > b {
		fmt.Printf("第三行 - a 大于 b\n")
	} else {
		fmt.Printf("第三行 - a 不大于 b\n")
	}
	/* Lets change value of a and b */
	a = 5
	b = 20
	if a <= b {
		fmt.Printf("第四行 - a 小于等于  b\n")
	}

	if b >= a {
		fmt.Printf("第五行 - b 大于等于 b\n")
	}
}

```



### 2.流程控制











# **go mod/go vender**



## 1.1 开启

```shell
# 开启
go env -w GO111MODULE=on
# 设置代理 七牛云
go env -w GOPROXY=https://goproxy.cn,https://goproxy.io,direct
```

## 1.2 使用



-   初始化一个moudle，模块名为你项目名

```text
go mod init 模块名
```

-   下载modules到本地cache

>   目前所有模块版本数据均缓存在 `$GOPATH/pkg/mod`和 `$GOPATH/pkg/sum` 下

```text
go mod download
```

-   编辑go.mod文件 选项有`-json`、`-require`和`-exclude`，可以使用帮助go help mod edit

```text
go mod edit
```

-   以文本模式打印模块需求图

```text
go mod graph
```

-   删除错误或者不使用的modules

```text
go mod tidy  # 初次使用  可以将代码当中应用的包及版本  添加到  go.mod 文件中
```

-   生成vendor目录

```text
go mod vendor  # 根据mod文件  将应用的依赖包拷贝到当前目录 /vendor 下
```

-   验证依赖是否正确

```text
go mod verify
```

-   查找依赖

```text
go mod why
```



### go get



>   使用go module之后，go get 拉取依赖的方式就发生了变化

-   下载项目依赖

```text
go get ./...
```

-   拉取最新的版本(优先择取 tag)

```text
go get golang.org/x/text@latest
```

-   拉取 master 分支的最新 commit

```text
go get golang.org/x/text@master
```

-   拉取 tag 为 v0.3.2 的 commit

```text
go get golang.org/x/text@v0.3.2
```

-   拉取 hash 为 342b231 的 commit，最终会被转换为 v0.3.2：

```text
go get golang.org/x/text@342b2e
```

-   指定版本拉取，拉取v3版本

```text
go get github.com/smartwalle/alipay/v3
```

-   更新

```text
go get -u
```



## go mod 高级操作

### 更新到最新版本

```bash
go get github.com/gogf/gf@version
```

>   如果没有指明 version 的情况下，则默认先下载打了 tag 的 release 版本，比如 v0.4.5 或者 v1.2.3；如果没有 release 版本，则下载最新的 pre release 版本，比如 v0.0.1-pre1。如果还没有则下载最新的 commit

### 更新到某个分支最新的代码

```text
go get github.com/gogf/gf@master
```

### 更新到最新的修订版（只改bug的版本）

```text
go get -u=patch github.com/gogf/gf
```

### 替代只能翻墙下载的库

```text
go mod edit -replace=golang.org/x/crypto@v0.0.0=github.com/golang/crypto@latest
go mod edit -replace=golang.org/x/sys@v0.0.0=github.com/golang/sys@latest
```

### 清理moudle 缓存

```text
go clean -modcache
```

### 查看可下载版本

```text
go list -m -versions github.com/gogf/gf
```














































