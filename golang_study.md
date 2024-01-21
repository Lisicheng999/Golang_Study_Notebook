# Golang Study Notebook
[go.dev 官网](https://go.dev/)
[go std](https://pkg.go.dev/std)
[golang 中文网](https://studygolang.com/)
[go 中文文档](https://studygolang.com/pkgdoc)
## 1. 注意事项
1. 源文件以go为拓展名
2. 严格区分大小写
3. 方法由一条条语句构成，每个语句后不需要分号，Go语言会在每个语句后面自动加分号
4. Go编译器是一行行进行编译的，因此一行就写一条语句
5. 定义的变量或者`import`的包如果没有使用到，代码不能编译通过
6. 缩进使用Tab，向前缩进使用`Shift+Tab`
7. `$ gofmt main.go`在控制台展示格式化后的代码，不写入源文件
8. `$ gofmt -w main.go`格式化代码并写入源文件
9. 官方推荐风格: 
	单行注释 `//`, 
	大括号:
	```go
	func main() {
		// TODO:
	}
	```
	行最大字符数: 80
10. 字符串拼接:
	```go
	fmt.Println("1111111111aaa",
	"aaaaaaaaaa")
	```
11. Go语言便准库https://golang.org
12. 不可以在赋值的时候给予不匹配的类型
	```go
	var num int = 12.56  // 错误的类型
	```
13. 导入多个依赖
	```go
	import(
		"fmt"
		"unsafe"
		// ...
	)
	```

## Golang的基本数据类型
### 整数: int / uint
1. 第一种：指定变量的类型并且赋值
   ```go
   var num int = 18
   ```	
2. 第二种：指定变量的类型但不赋值
   ```go
   var num2 int
   ```
3. 第三种：如果没有写变量的类型，那么根据后面的值进行判定变量的类型(自动类型推断)
   ```go
   var num3 = "Tom"
   ```
4. 第四种：省略var 
	```go
	num4 := "str"
	```
5. 声明多个变量
   ```go
   var n1, n2, n3 int
   var n4, n5, n6 = 10, "Jack", 7.9
   n6, n7 := 15, "str"
   ```
6. 定义全局变量
   ```go
   var(
	n8 = 9
	n9 string = "str"
	n10 float32 = 12.34
   )
   ```
7. 查看变量类型
   ```go
   num := 1
   fmt.printf("num1 type: %T", num1)
   ```
8. 查看变量占用内存大小, int类型默认大小为8Byte
   ```go
   import(
	"fmt"
	"unsafe"
   )
   //
   num1 := 1
   fmt.Println(unsave.Sizeof(num1))
   ```
9. Golang程序中整型变量在使用时遵循保小不保大的原则，在保证程序正确运行下，尽量使用占空间小的数据类型,默认的int类型数据大小为8Byte
10. byte = uint8
### 浮点数: float
1. 浮点数的定义, 不区分E/e大小写，vscode保存文件是会格式化为e
	```go
	var num1 float32 = 3.14
	var n2 float32 = -3.14
	var n3 float32 = 314E-2
	var n4 float32 = 314e-2
	var n5 float32 = 314E+2
	var n6 float32 = 314e+2
	// ctrl + s, E will become to e
	```
2. 浮点数推荐都使用float64，精度更高, golang默认浮点类型为float64
	```go
	var num1 float32 = 256.000000916
	var num2 float64 = 256.000000916
	fmt.Println(num1)
	fmt.Println(num2)
	num3 := 256.000000916
	fmt.Println(unsafe.Sizeof(num3))
	```
	输出
	```
	256
	256.000000916
	8
	```
3. 底层存储空间和操作系统无关
4. 浮点数由阶符+阶数+尾符+尾数组成(计组)
### 字符类型变量: byte
1.  字符类型变量本质上是整数(8bit(1Byte))
	```go
	var c1 byte = 'a'
	fmt.Println(c1)
	var c2 byte = '6'
	fmt.Println(c2)
	```
	输出
	```
	97
	54
	```
2. 字符类型本质是一个整数，只不过数据位宽为8bit(1Byte)，可以直接参与运算，输出字符饿时候，会将对应的码值作为输出
   字母，数字，标点等字符，按照ASCII码进行存储的
   使用转义字符 ```%c```格式化输出内容
   ```go
   var c3 byte = 'A'  // 65
   fmt.Println(c3 + 20)
   fmt.Printf("%c", c3 + 20)
   ```
   输出
   ```
   85
   U
   ```
3. 使用中文字符(Unicode)
   ```go
   var c4 byte = '中'
   fmt.Println(c4)
   ```
   报错
   ```
   cannot use '中' (untyped rune constant 20013) as byte value in variable declaration (overflows)
   ```
   '中' 编码20013超出了byte型数据的表达范围(0~255), byte类型长度为1Byte,而Unicode编码长度为4Byte
   Golang字符使用的是UTF-8， UTF-8是UniCode的其中一种编码方案(UTF-16, UTF-32)
   可以使用int类型的空间容纳
   ```go
   var c5 int = '中'
   fmt.Printf("%c", c5)
   ```
   输出
   ```
   中
   ```
   在docs_standard library_fmt中可以看到: 
-  %c the character represented	by the corresponding Unicode code point
	%c使用int类型输出内容，相应的byte类型会转换成int类型进行输出

### 转义字符
| 转义符 | 含义            | Unicode值 |
| ------ | --------------- | --------- |
| \b     | 退格(backspace) | \u0008    |
| \t     | 制表符(tab)     | \u0009    |
| \n     | 换行            | \u000a    |
| \r     | 回车            | \u000d    |
| \"     | 双引号          | \u0022    |
| \'     | 单引号          | \u0027    |
| \\\\   | 反斜杠          | \u005c    |

### 布尔类型bool
1. bool 类型长度为1Byte
   ```go
   var b1 bool = true
   var b2 bool = false
   var b3 bool = 5 < 9
   ```
 
### 字符串类型
1. 定义一个字符串
   ```go
   var str string = "Golang string.\n"
   fmt.Print(str)
   ``` 
   输出
   ```
   Golang string.
   ```
2. 字符串是不可变的
   虽然:
   ```go
   var s1 string = "abc"
   s1 = "def"
   fmt.Println(s1)
   ```
   正确，但不可指的是一旦字符串定义好了，就不可改变字符的值
   ```go
   var s1 string = "abc"
   s2[0] = 't'
   ```
   报错
   ```
   cannot assign to s2[0] (neither addressable nor a map index expression)
   ```
3. 字符串定义中会使用到转义字符， 如: ``` "abc \"def\" " ```
   当需要在多个位置使用转义字符时，可以简化为反引号括写
   ```go
   var str string = `
	package main
	import "fmt"

	func main() {
		var f1 float64 = 3.14e
		fmt.Println(f1)
	}
   `
   fmt.Println(str)
   ```
   输出
   ```
   
		package main
		import "fmt"

		func main() {
				var f1 float64 = 3.14e
				fmt.Println(f1)
		}

   ```
4. 字符串的拼接
   ```go
   var s5 string = "abc" + "def"
   fmt.Println(s5)
   s5 += "ghi"
   fmt.Println(s5)
   ```
   输出
   ```
   abcdef
   abcdefghi
   ```
5. 字符串换行拼接, 注意加号保留在上一行的最后，因为编译是按行编译，加号会让编译器不在换行时加上分号
   ```go
   var s6 = "abcdef" +
   "ghijkl"
   ```
### Golang中数据类型的默认值
| 数据类型   | 默认值 |
| ---------- | ------ |
| 整数类型   | 0      |
| 浮点类型   | 0      |
| 布尔类型   | false  |
| 字符串类型 | ""     |

### Golang中不同类型的变量之间的赋值时需要显式的类型转换
1. 表达式T(v)，T为目标类型， v为需要转换的变量
   ```go
   var n1 int = 100
   var n2 float32 = n1
   fmt.Println(n1)
   fmt.Println(n2)
   ```
   报错
   ```
   cannot use n1 (variable of type int) as float32 value in variable declaration
   ```
   手动显式转换
   ```go
   var n1 int = 100
   var n2 float32 = float32(n1)
   ```
   输出
   ```
   100
   100
   ```
2. 高类型转为低类型
   ```go
   var n3 int64 = 88888  // .... 0000 0001 0101 1011 0011 1000
   var n4 int8 = int8(n3)  // 0011 1000 截取了低8位
   fmt.Println(n4)
   n3 = 255  // .... 0000 1111 1111
   n4 = int8(n3) // 1111 1111 补码,所以会输出-1
   fmt.Println(n4)
   ```
   输出
   ```
   56
   -1
   ```
3. 一定要匹配等号左右两边的数据类型
   ```go
   var n5 int32 = 12
   var n6 int64 = n5 + 30
   fmt.Println(n6)
   ```
   报错
   ```
   cannot use n5 + 30 (value of type int32) as int64 value in variable declaration
   ```
   改为
   ```go
   var n5 int32 = 12
   var n6 int64 = int64(n5) + 30
   fmt.Println(n6)
   ```
   输出
   ```
   46
   ```
4. 编译不通过
   ```go
   var n7 int64 = 12
   var n8 int8 = int8(n7) + 127
   fmt.Println(n8)
   var n9 int8 = int8(n7) + 128
   fmt.Println(n9) 
   ```
   报错
   ```
   128 (untyped int constant) overflows int8
   ```
5. string类型转换为其他类型数据, 使用strconv.Parse
   string->bool
   ```go
   // string->bool
   var s1 string = "true"
   var b1 bool
   b1, _ = strconv.ParseBool(s1)
   fmt.Printf("b1's type is %T, b1 = %v\n", b1, b1)
   ```
   string->int64
   ```go
   var s2 string = "21"
   var i1 int64
   // strconv.ParseInt()接受三个参数, 即字符串源，目标数据进制，目标数据位宽
   i1, _ = strconv.ParseInt(s2, 10, 64)
   fmt.Printf("i1\'s type is %T, i1 = %v\n", i1, i1)
   ```
   string->float64/float32
   ```go
   var s3 = "3.14E-5"
   var f1 float64
   // strconv.ParseFloat()接受两个参数，即字符串源和目标数据位宽
   f1, _ = strconv.ParseFloat(s3, 64)
   fmt.Printf("f1\'s type is %T, f1 = %v\n", f1, f1)
   ```
   输出
   ```
   b1's type is bool, b1 = true
   i1's type is int64, i1 = 21
   f1's type is float64, f1 = 3.14e-05
   ```

## 关于package和main
1.
   包名尽量保持package的名字和目录保持一致，尽量采取有意义的包名，简短，有意义, 不要和标准库的名字冲突
##### main包是程序的入口包，所以main函数所在的包定义为main包,如果不定义为main包，那么就不能执行
   ```go
   // file_name: main.go
   package abc

import (
	"fmt"
)

func main() {
	fmt.Print(1)
}

   ```
运行go run ./main.go
   ```
package command-line-arguments is not a main package
   ```
   2. 变量名，函数名，常量名采用驼峰命名法
   3. 变量名，函数名，常量名首字母大小，则可以被其他的包访问到
   In Demo/test/test.go
   ```go
   package test

   import "fmt"

   func TestPrint(x int64) {
	   var y int64
	   fmt.Println("This is package test")
	   fmt.Println("Sum = ", x+y)
   }

   ```
   In Demo/main/main.go
   ```go
   package main

   import (
	   "Demo/test"
   )

   func main() {
	   test.TestPrint(1)
   }

   ```
   4. 如果变量名，函数名，常量名首字母小写，则只能在本包中使用
   5. 包名默认是$GOPATH/src后开始计算的，所以不用加上GoPath/src使用/进行路径分割, 若不在GoPath/src下声明包，则需要go mod init 在包的根目录下创建module声明
   
## Golang 初级
25个关键字
|          |              |        |           |        |
| :------: | :----------: | :----: | :-------: | :----: |
|  break   |   default    |  func  | interface | select |
|   case   |    defer     |   go   |    map    | struct |
|   chan   |     else     |  goto  |  package  | switch |
|  const   | fallthrought |   if   |   rango   |  type  |
| continue |     for      | import |  return   |  var   |

36个预定义标识符
|           |            |        |       |         |         |
| :-------: | :--------: | :----: | :---: | :-----: | :-----: |
|  append   |    bool    |  byte  |  cap  |  close  | complex |
| complex64 | complex128 | unit16 | copy  |  false  | float32 |
|  float64  |    imag    |  int   | int8  |  int16  | uint32  |
|  int 32   |   int64    |  iota  |  len  |  make   |   new   |
|    nil    |   panic    | uint64 | print | println |  real   |
|  recover  |   string   |  true  | uint  |  uint8  | uintprt |

> 运算符
>> 算术运算符 : +, -, *, /, %, ++, --
>> 赋值运算符 : =, +=, -=, *=， /=， %=
>> 关系运算符 : ==, !=, >, <, >=, <=
>> 逻辑运算符 : &&, ||, !
>> 位运算符 : &, |, ^
>> 其他运算符(指针) : &, *

### 指针
1. 变量的本质上内存中的字节空间, 存储一段数据, 而指针也是一个变量，它存储的一段地址,或者说一个位置
   查看int类型变量的起始地址
   ```go
   var age int = 20
   fmt.Println(&age)
   ```
   定义一个int类型变量的指针, &value, 获取变量的地址，*ptr获取地址的内容
   ```go
   var ptr *int = &age
   fmt.Println("ptr: ", ptr, ", ptr points to value of ", *ptr)
   ```
   多级指针
   ```go
   	// multilevel pointer
	var ptr2 **int = &ptr
	fmt.Println("ptr2: ", ptr2, ", ptr2 points to value of ", *ptr2)
   ```
   输出
   ```
   0xc000096068
   ptr:  0xc000096068 , ptr points to value of  18
   ptr2:  0xc0000a8020 , ptr2 points to value of  0xc000096068
   ```
2. 改变指针指向的值
   ```go
   s1 := 16
	ptr := &s1
	*ptr += 1
	fmt.Print(ptr, "->", *ptr)
   ```
   输出
   ```
   0xc00000a0d8->17
   ```

### 获取控制台输入
1.   fmt.Scanln(&x)
   Scanln以换行符结束录入
```go
   var age int 
   fmt.Println("录入数据：")
   // 传入age的地址，目的是Canln对地址的内容进行改变
   fmt.Scanln(&age)
```
2. fmt.Scanf() 指定输入的格式
   ```go
   fmt.Scanf("%d %s %t", )
   ```

### 流程控制
#### if-else
#####   在golang 里 条件表达式的()是可以省略的
   ```go
	fmt.Println("Enter value of count to compare size")
	var count int
	fmt.Scanf("%d", &count)
	if count < 30 {
		fmt.Println("count < 30")
	} else if count == 30 {
		fmt.Println("count = 30")
	} else {
		fmt.Println("count > 30")
	}
   ```
   输出
   ```
   Enter value of count to compare size
   ```
   输入```123```
   输出```count > 30```
##### 在golang中，if后面可以加入一个变量的定义
   ```go
	if count := 10; count < 20 {
		fmt.Print("count < 20 is true")
	} else {
		fmt.Print("count < 20 is false")
	}
   ```
   But after used `count` in `if-else` structure, we can not use `count` outside `if-else`, because `count` belongs to a local variable like rules of C/C++.
   ```go
   if count := 10; count < 20 {
	  ...
   } else {
	  ...
   }  // after that
   count += 20
   ```
   Problems:```undefined: count```
#### switch-case
##### 1. 基本用法
```go
switch 100 % count {
case 0:
   ...
case 1:
   ...
...
}
```

##### 2. switch 也可以进行局部变量的声明
```go
// 变量声明后需要加分号
switch a := 1; a{
   case 1: 
	  ...
   case 2:
	  ...
   ...
}
```
##### 3. 进行多个局部变量的声明
```go
switch a, b, c := 1, 2.4, "tmp"; {
case a == 1 && b == 2.4 && c == "tmp":
	fmt.Println("true")
default:
	fmt.Println("dafault")
}

```
输出 ```true```
##### 4. 使用fallthrough关键字，switch穿透, 如果在case语句后增加fallthrough关键字，则会继续执行下一句case语句
```go
switch a := 1; a {
case 1:
	fmt.Print("case a = 1 ")
	fallthrough
case 2:
	fmt.Print("case a = 2")
}
```
输出```case a = 1 case a = 2```

##### 5. case 后面可以跟多个值 like
```go
switch a := 1; a{
   case 1, 2, 3, 4 :
	  fmt.Print("case a = 1, 2, 3, 4")
}
```

##### 6. 若所有的值都不满足，但也需要有一个选择分支，则使用default关键字, like
```go
switch a := 5; a {
case 1, 2, 3, 4:
	fmt.Print("case a = 1, 2, 3, 4")
default:
	fmt.Print("default print")
}
```
输出```default print```
```default位置是随意的，同时也不是必须的```

##### 7. 将switch-case作为if使用
```go
// 省略值
switch a := 1, b := 2; {
case a == 1:
	  fmt.Println("this is a == 1")
case a != b
}
```
##### 8. 开关表达式可以是变量，也可以是函数返回值
```go
func main() {
	switch a := 1; FunctionCall(a) {
	case 1:
		// TODO：
	case 2:
		// TODO:
	}
}

func FunctionCall(x int) int {
	return x
}
```
##### 9. 不能在switch中加入了开关表达式之后再使用case进行布尔运算，要么不写入开关表达式
```go
func main() {
	switch a := 1; FunctionCall(a) {
	case a == 1:
		// TODO：
	case 2:
		// TODO:
	}
}

func FunctionCall(x int) int {
	return x
}

```
Problems:```cannot convert a == 1 (untyped bool value) to type int```
#### for循环
##### 1. 标准格式
```go
func main() {
	for i, sum := 1, 0; i < 5; i++ {
		sum += i
		fmt.Printf("i = %v, sum = %v\n", i, sum)
	}
}
```
输出
```
i = 1, sum = 1
i = 2, sum = 3
i = 3, sum = 6
i = 4, sum = 10
```

##### 2. 简写
```go
// 将变量定义和单次循环后的操作移动到for语句的外，只保留循环判断条件(布尔运算)
func main() {
	i, sum := 1, 0
	for i < 5 {
		sum += i
		fmt.Printf("i = %v, sum = %v\n", i, sum)
		i++
	}
}
```
死循环```for { // TODO: }```
等价于```for true { // TODO: }```
等价于```for ; true; { //TODO: }```, 格式化代码会自动转换为第一种最简洁的形式
##### 3. 用for循环进行字符串遍历
```go
func main() {
	var str string = "hello golang"
	for i := 0; i < len(str); i++ {
		fmt.Printf("%c|", str[i])
	}
}
```
输出```h|e|l|l|o| |g|o|l|a|n|g|```
假如加入中文，输出的内容的中文就会乱码(中文意义)，因为采用的UTF-8进行了encode
```go
func main() {
	var str string = "golang中文"
	for i := 0; i < len(str); i++ {
		fmt.Printf("%c|", str[i])
	}
}
```
输出```g|o|l|a|n|g|ä|¸|­|æ|||```  中文字符在UTF-8中编码占3个字节，所以```golang```后跟随6个空位，```str[i]```通过```%c```拆解为了单个字符进行解析
##### 4. for-range 进行遍历
for-range 可以将内容进行切片
```go
func main() {
	var str string = "golang+中文"
	for i, value := range str {
		fmt.Printf("index=%d, value=%c\n", i, value)
	}
}
```
输出
```
index=0, value=g
index=1, value=o
index=2, value=l
index=3, value=a
index=4, value=n
index=5, value=g
index=6, value=+
index=7, value=中
index=10, value=文
```
注意到index的值在7,10处产生了跳变，对应了`中文`的字符起始位置
##### 5. break
```go
func main() {
	var sum int = 0
	for i := 1; i <= 100; i++ {
		sum += i
		fmt.Println(sum)
		if sum >= 300 {
			break
		}
	}
}
```
break的作用是结束距离它最近的那个循环`jmp`
golang可以给每个上循环加上标签, 指定要break的循环
根据代码书写原则必须使用到定义的标签
`lable_name`
```go
func main() {
lable1:
	for i := 1; i <= 5; i++ {
		for j := 2; j <= 4; j++ {
			fmt.Printf("i=%v, j=%v\n", i, j)
			if i == 2 && j == 2 {
				break lable1
			}
		}
	}
}
```
输出
```
i=1, j=2
i=1, j=3
i=1, j=4
i=2, j=2
```

##### 6. continue
continue会跳过循环体中剩下的code，转而进行下一轮循环判定
例如， 输出1-100中被6整除的数
```go
func main() {
	for i := 1; i <= 100; i++ {
		if i%6 == 0 {
			fmt.Print(i, " ")
		}
	}
}
```
使用continue
```go
func main() {
	for i := 1; i <= 100; i++ {
		if i%6 != 0 {
			continue
		}
		fmt.Print(i, " ")
	}
}
```
输出```6 12 18 24 30 36 42 48 54 60 66 72 78 84 90 96 ```
同样的continue只会影响最近的循环体
同样的continue可以指定需要continue的循环体标签
```go
lable1:
func main() {
lable1:
	for i := 0; i < 3; i++ {
		for j := 0; j < 3; j++ {
			if i == 1 && j == 1 {
				fmt.Println("Skip this line")
				continue lable1
			}
			fmt.Printf("i=%v, j=%v\n", i, j)
		}
	}
}
```
输出
```
i=0, j=0
i=0, j=1
i=0, j=2
i=1, j=0
Skip this line
i=2, j=0
i=2, j=1
i=2, j=2
```

#### goto
同样的goto可以使用`lable`控制
```go
func main() {
	fmt.Print(1)
	fmt.Print(2)
	goto lable_goto4
	fmt.Print(3)
lable_goto4:
	fmt.Print(4)
	fmt.Print(5)
}
```
输出`1245`
注意到Problems:`unreachable code`

#### return
1. 结束当前函数运行
2. 返回函数值
   
### 函数
```go
func function_name(params list...) (return type list ...) {
	// TODO:
	return ...
}
```
1. 只有一个或没有返回值类型，可以省略括号
```go
func cal(num1 int, num2 int) int {
	return num1 + num2
}

func main() {
	fmt.Println(cal(10, 20))
}

```
Output:```30```
2. 函数名
	遵循标识符命名规范，使用驼峰命名，首字母不能是数字
	首字母大写该函数，该函数可以被其他的包访问到，同样的变量和常量也如此(like public)
	首字母小写，则该函数只能被本包内访问到(like private)
3. params list 形参列表，可以是一个参数，可以是n个参数，也可以没有参数
4. return type list ，返回值类型列表，可以是一个类型，可以是n个类型，也可以没有类型
```go
func cal2(num1 int, num2 int) (int, int) {
	// calculate num1 + num2, and num1 - num2
	return num1 + num2, num1 - num2
}

func main() {
	sum, sub := cal2(10, 20)
	fmt.Printf("sum = %v, sub = %v", sum, sub)
}
```
Output:`sum = 30, sub = -10`

#### memory 内存
##### 栈(存放基本数据类型), 堆(存放复杂/派生数据类型)， 代码区(存放程序本身)
main函数会栈中有自己的栈帧 存放自己的局部变量, 在运行到`cal2(10, 20)`时，会在栈中开辟出新的栈帧，存放cal中的变量
详见汇编原理

函数执行返回后，程序会销毁栈帧空间，销毁行为是编译后的程序产生的应有的行为

#### golang不支持函数重载
#### Golang支持可变参数
定义一个可变参数的函数```func test(args ...int) {}```
函数内部处理可变参数的时候，将可变参数当作切片来处理
```go
func test(args ...int) {
	for i := 0; i < len(args); i++ {
		fmt.Print(args[i], " ")
	}
}

func main() {
	test(1, 2, 3, 4, 5)
}
```
Output:`1 2 3 4 5 `
#### 基本数据类型和数组默认都是值传递，即进行值拷贝，在函数内修改不会影响原来的值
```go
func test(num int) {
	num = 30
}

func main() {
	var num = 20
	test(num)
	fmt.Printf("num = %v", num)
}
```
所以想要在函数内部改变函数外部的变量的值，可以通过指针的形式传递参数
```go
func test(num *int) {
	*num = 30
}

func main() {
	var num = 20
	test(&num)
	fmt.Printf("num = %v", num)
}
```
#### 在Go中函数也是一种数据类型，可以赋值给变量
```go
func test(num int) {
	fmt.Println("num = ", num)
}

func main() {
	a := test
	a(1)
	fmt.Printf("type(a) = %T, type(test) = %T\n", a, test)
}

```
同样，函数也可以作为形参， 例如，定义一个函数，将另一个函数作为parameter
```go
func test(num int) {
	fmt.Println("num = ", num)
}

func test02(num int, testFunc func(int)) {
	testFunc(num)
}

func test03(num int, testFunc func(int, func(int))) {
	testFunc(num, test)
}

func main() {
	num := 5
	a := test
	b := test02
	c := test03
	a(10)
	c(num, b)
}
```
Output: 
`num =  10
num =  5`

#### 为了简化数据类型定义，Go支持自定义数据类型 `type`
```go
func main() {
	// 相当于给int类型起别名
	type myInt int
	var num1 myInt = 10
	fmt.Printf("num1 = %v, type(num1) = %T", num1, num1)
}
```
Output: `num1 = 10, type(num1) = main.myInt`
可知`num1` 的类型为`myInt`，而且是在`main`包下的`myInt`
假如
```go
	num2 := 20
	num1 = num2
```

Problems:`cannot use num2 (variable of type int) as myInt value in assignment`
可以用
```go
// 相当于给int类型起别名
type myInt int

func main() {
	var num1 myInt = 10
	num2 := 20
	num1 = myInt(num2)
	fmt.Printf("num1 = %v, type(num1) = %T", num1, num1)
}
```
Output: `num1 = 20, type(num1) = main.myInt`

同样也可以使用`type`给函数类型参数起别名, 指定自定义函数类型的参数类型列表和返回值列表
```go
type func_Add func(int, int) int

func Add(x int, y int) int {
	return x + y
}

func main() {
	var a func_Add = Add
	fmt.Println("11 + 20 = ", a(11, 20))
	fmt.Printf("type(a) = %T\n", a)
}
```
Output:
```
11 + 20 =  31
type(a) = main.func_Add
```
又例如
```go
type funcAddAndSub func(float64, float64) (float64, float64)

func AddAndSub(x float64, y float64) (float64, float64) {
	return x + y, x - y
}

func main() {
	var tmpFunc funcAddAndSub = funcAddAndSub(AddAndSub)
	add, sub := tmpFunc(0.9141, 1.54e-3)
	fmt.Printf("0.9141 + 1.54e-3 = %f, 0.9141 - 1.54e-3 = %f\n", add, sub)
	fmt.Printf("type(tmpFunc) = %T\n", tmpFunc)
}
```
Output: 
```
0.9141 + 1.54e-3 = 0.915640, 0.9141 - 1.54e-3 = 0.912560
type(tmpFunc) = main.funcAddAndSub
```
需要随时注意Go是强类型语言，显示声明类型转换
#### Go 支持对函数返回值命名
```go
func AddAndSub(x float64, y float64) (float64, float64) {
	add := x + y
	sub := x - y
	return add, sub
}
```
还可以给返回值命名，这样在接受多个返回值时可以不用考虑顺序
```go
func AddAndSub(x float64, y float64) (add float64, sub float64) {
	add = x + y
	sub = x - y
	return
}
```

### 包的引入
#### 创建一个包 包和所在的目录同名
GoDemo1/
├───dbutils
└───main
```go
package dbutils

import "fmt"

func GetConnection() {
	fmt.Println("Function `GetConnection` is invoked within package `dbutils`")
}
```
在其他包中导入刚才定义的包
```go
package main
// 包定位到它所在的目录
import "GoDemo1/dbutils"

func main() {
	// 在函数调用的时候，定位到它使用的包

	dbutils.GetConnection()
}
```
#### 包的路径是从$GOPATH开始计算的， 在GOPATH之外创建的go程序，应该由go mod init project_name 生成路径定义
#### 在同一个包下不能定义同名函数
#### 在同目录下的包，需要分别被`import`到所被使用到的包中
比如定义在dbutils目录下的不同的包
```
dbutils
	├───dbutils
	└───sources
```

#### 给导入的包起一个别名并使用，原来的包名就不能使用了
```go
package main

import (
	db "GoDemo1/dbutils"
	"fmt"
)

func main() {
	db.GetConnection()
	fmt.Println("**")
}

```
#### init() 函数
在每个包都可以包含一个`init()`函数，会被默认先加载运行
```go
func init() {
	fmt.Println("function init() is invoked")
}

func main() {
	fmt.Println("function main() is invoked")
}
```
Output:
```
function init() is invoked
function main() is invoked
```
观察哥哥变量和函数的调用顺序
```go
var num int = test()

func test() int {
	fmt.Println("function test() has invoked")
	return 10
}

func init() {
	fmt.Println("function init() has invoked")
}

func main() {
	fmt.Println("function main() has invoked")
}
```
Output:
```
function test() is invoked
function init() is invoked
function main() is invoked
```

在多个包中有`init()`函数时的调用顺序
/GoDemo1/testutils/testutils.go
```go
import "fmt"

var Age int
var Sex string
var Name string

// declare funciton init()
func init() {
	fmt.Println("testutils.init() has invoked")
	Age = 19
	Sex = "女"
	Name = "小王"
}
```
/GoDemo1/main/main.go
```go
package main

import (
	"GoDemo1/testutils"
	"fmt"
)

var num int = test()

func test() int {
	fmt.Println("function test() has invoked")
	return 10
}

func init() {
	fmt.Println("function main.init() has invoked")
}

func main() {
	fmt.Println("function main() has invoked")
	fmt.Printf("Age = %v, Sex = %v, Name = %v\n", testutils.Age, testutils.Sex, testutils.Name)
	fmt.Println("num = ", num)
}

```
Output:
```
testutils.init() has invoked
function test() has invoked
function main.init() has invoked
function main() has invoked
Age = 19, Sex = 女, Name = 小王
num =  10
```
可以看出，每个包的变量首先被加载，然后是init()函数，然后才是其他函数
main包调用了fmt和testutils包，首先会加载这些包的变量和init()函数，然后才是main包自己的内容

### 匿名函数
有一个函数只用到一次，可以将函数定义为匿名函数,在匿名函数使用时就直接调用
```go
func main() {
	// 定义匿名函数
	result := func(num1 int, num2 int) int {
		return num1 + num2
	}(10, 20)

	fmt.Print(result)
}
```
将匿名函数赋值给变量，使得匿名函数变为函数变量
```go
func main() {
	// 定义匿名函数, 定义的同时调用
	result := func(num1 int, num2 int) int {
		return num1 + num2
	}(10, 20)
	fmt.Println(result)

	// 将匿名函数赋值给一个变量，这个变量实际就是函数类型变量
	sub := func(num1 int, num2 int) int {
		return num1 - num2
	}

	sub(30, 70)
	result2 := sub(20, 30)
	fmt.Println(result2)
}
```
Output：
```
30
-10
```
此时这个函数变量就可以多次使用，等价于普通的函数
上面的sub函数变成了局部函数变量，为了使得sub函数变为全局变量，可以将sub定义在main()函数外

### 闭包
闭包就是一个函数和与其相关的引用环境组合的一个整体
```go
// function get sum
// function name: getSum, parameter: null
// return type: fun(int) int, this return function's type is int
func getSum() func(int) int {
	sum := 10
	return func(num int) int {
		sum = sum + num
		return sum
	}
}

func main() {
	f := getSum()
	fmt.Println("f(26) = ", f(26))
	fmt.Println("f(-24) = ", f(-24))
}
```
Output:
```
f(26) =  36
f(-24) =  12
```
可以看到，匿名函数引用的那个变量，可以一直使用
闭包的本质依旧是一个匿名函数，只是这个匿名函数引用了外界的变量或者参数，因此这个匿名函数就和这些变量或者参数一起形成了一个整体，构成闭包
闭包可以保存结果，而不是创建新的函数

### defer
为了在函数执行完后释放资源，Go提供defer功能，defer会将需要执行的语句压栈，然后在函数返回之后弹栈，执行defer的语句，按照入栈的顺序后进先出执行，首先来看它的执行顺序
```go
func main() {
	add(10, 20)
	fmt.Print("***")
}

func add(num1 int, num2 int) int {
	// In golang, when program enconters `defer`, the code follows it does not
	// execute immediately. Instead, it is pushed onto a stack to be execute later.
	defer fmt.Println("num1 = ", num1)
	defer fmt.Println("num2 = ", num2)

	num1 += 10
	num2 += 10
	var sum int = num1 + num2
	fmt.Println("sum = ", sum)
	return sum
}
```
Output:
```
sum =  50
num2 =  20
num1 =  10
***
```
可见输出sum的操作先执行，然后按照压栈的顺序执行defer后的语句，但注意到`num1`和`num2`的值是在进行`+=10`操作前就已经被定下来了，而没有受到后续操作的影响，defer在压栈时将`num1`和`num2`的值进行了拷贝入栈
因为defer存在这种延迟执行的机制，所以可以方便的用于资源的释放

### 字符串处理函数
#### 获取字符串长度len()
 统计字符串长度，按字节计算(utf-8的中文占3字节, ASCII字符占据一个字节)
 ```go
 func main() {
	var str string = "hello golang"
	for i := 0; i < len(str); i++ {
		fmt.Printf("%c|", str[i])
	}
}
 ```
#### 对字符串进行遍历，for-range (slice)
方式一：use range slice
```go
func main() {
	str := "go语言"
	for i, value := range str {
		fmt.Printf("index = %v, value = %v, char = %c\n", i, value, value)
	}
}
```
方式二 use []rune()
```go
func main() {
	str := "go语言"
	r := []rune(str)
	for i := 0; i < len(r); i++ {
		fmt.Printf("i = %v, value = %v, char = %c\n", i, r[i], r[i])
	}
}
```
Output:
```
index = 0, value = 103, char = g
index = 1, value = 111, char = o
index = 2, value = 35821, char = 语
index = 5, value = 35328, char = 言
```

#### 字符串转整数, 整数转字符串
strconv.Atoi, strconv.Itoa
```go
	num1, _ := strconv.Atoi("-434")
	num2 := strconv.Itoa(num1)
	fmt.Printf("num1 = %v, type = %T\n", num1, num1)
	fmt.Printf("num2 = %v, type = %T", num2, num2)
```
Output:
```
num1 = -434, type = int
num2 = -434, type = string
```
#### 统计字符串中指定的字串
`func Conut(s, substr string) int`
```go
func main() {
	fmt.Println(strings.Count("golang golang gogogo", "go"))
	fmt.Println(strings.Count("中文显示中文", "中"))
	fmt.Println(strings.Count("three", ""))
}
```
Output:
```
5
2
6
```

doc: Count counts the number of non-overlapping instances of substr in s. `If substr is an empty string, Count returns 1 + the number of Unicode code points in s.`
#### 不区分大小写的字符串比较
```go
func main() {
	fmt.Println(strings.EqualFold("Go", "go"))
	fmt.Println(strings.EqualFold("AB", "ab")) // true because comparison uses simple case-folding
	fmt.Println(strings.EqualFold("ß", "ss"))  // false because comparison does not use full case-folding
}
```
Output:
```
true
true
false
```
#### 区分大小写的字符串比较
```go
func main() {
	fmt.Println("hello"=="Hello")
}
```
Output:`false`

#### 字符串拆分
```go
func main() {
	fmt.Printf("Fields are: %q", strings.Fields("  asd daun  aind  "))
}
```
Output:`Fields are: ["asd" "daun" "aind"]`

#### 返回字串第一次出现的索引值
`func Index(s, substr string) int`
Starting value of the index is 0
Index returns the index of the first instance of substr in s, or -1 if substr is not present in s.
```go
func main() {
	fmt.Println(strings.Index("crystal", "yst"))
	fmt.Println(strings.Index("corresponding", "xy"))
	fmt.Println(strings.Index("rtyuiofgh", ""))
}
```
Output:
```
2
-1
0
```

### 内建函数
无需导包直接用，但它实际上是所在的包是builtin
doc:Package builtin provides documentation for Go's predeclared identifiers.The items documented here are not actually in package builtin but their descriptions here allow godoc to present documentation for the language's special identifiers.

#### len()
#### new()
