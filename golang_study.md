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
`LABLE_NAME`
```go
func main() {
LABLE1:
	for i := 1; i <= 5; i++ {
		for j := 2; j <= 4; j++ {
			fmt.Printf("i=%v, j=%v\n", i, j)
			if i == 2 && j == 2 {
				break LABLE1
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
LABLE1:
func main() {
LABLE1:
	for i := 0; i < 3; i++ {
		for j := 0; j < 3; j++ {
			if i == 1 && j == 1 {
				fmt.Println("Skip this line")
				continue LABLE1
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
同样的goto可以使用`LABLE`控制
```go
func main() {
	fmt.Print(1)
	fmt.Print(2)
	goto LABLE_GOTO4
	fmt.Print(3)
LABLE_GOTO4:
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
还可以给返回值命名，这样在接受多个返回值时可以不用考虑顺序，推荐定义命名返回值，而不是非命名返回值
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
new() return a pointer of the type
new()只能用于分配值类型(int float bool string struct)
new() 返回一个它创建的内存空间起始地址，参数为具体要创建的什么类型
```go
new(int)
```
make()用于引用类型的分配(slice channle map interface)

### Go异常
```go
func main() {
	test()
	fmt.Println("Function test is called successfully")
	fmt.Println("Execution instruction normally")
}

func test() {
	num1 := 10
	num2 := 0
	result := num1 / num2
	fmt.Println(result)
}
```
Output:
```
panic: runtime error: integer divide by zero

goroutine 1 [running]:
main.test()
        D:/projects/Golang/GoDemo1/main/main.go:14 +0x9
main.main()
        D:/projects/Golang/GoDemo1/main/main.go:6 +0x17
exit status 2
```
程序发生错误(panic)之后，中断执行
#### 错误处理机制
Go 追求代码的简洁优雅，引入`defer`+`recover`机制处理错误
内建(built-in)函数 `func recover() interface{}` 用于捕获panic引起的异常
如果recover在defer函数中调用，并且当前的goroutine发生了panic，`recover会捕获到panic的值并返回它，终止panic的传播`，使得程序可以继续运行。
```go
func main() {
	test()
	fmt.Println("Function test is called successfully")
	fmt.Println("Execute instruction normally")
}

func test() {
	// 利用defer + recover 捕获错误, defer后加上你们函数的调用
	defer func() {
		// 调用recover内建函数，可以捕获错误
		err := recover()
		// 如果没有捕获错误，返回值为0值，也就是nil
		if err != nil {
			fmt.Println("错误已经捕获")
			fmt.Println("err : ", err)
		}
	}()
	num1 := 10
	num2 := 0
	result := num1 / num2
	fmt.Println(result)
}
```
Output:
```
错误已经捕获
err :  runtime error: integer divide by zero
Function test is called successfully
Execute instruction normally
```

#### 抛出自定义异常
调用errors包下的New函数
```go
import (
	"errors"
	"fmt"
)

func main() {
	err := test()
	if err != nil {
		fmt.Println("Custom error: ", err)
	}
	fmt.Println("Function test is called successfully")
	fmt.Println("Execute instruction normally")
}

func test() (err error) {
	num1 := 10
	num2 := 2

	if num2 == 0 {
		// 抛出自定义错误
		return errors.New("除数不能为零")
	} else {
		result := num1 / num2
		fmt.Println(result)
		return nil
	}
}

```
Output:
```
Custom error:  除数不能为零
Function test is called successfully
Execute instruction normally
```

如果想让程序立即中断，可以调用panic()函数, 如同goruntine发生的panic一样，
recover()会捕获panic的值
`panic() built-in function`
```go
func main() {
	defer func() {
		err := recover()
		if err != nil {
			fmt.Println("Execute instruction normally")
		}
	}()
	err := test()
	if err != nil {
		fmt.Println("Custom error: ", err)
		panic(err)
	}
}

func test() (err error) {
	num1 := 10
	num2 := 0

	if num2 == 0 {
		// 抛出自定义错误
		return errors.New("除数不能为零")
	} else {
		result := num1 / num2
		fmt.Println(result)
		return nil
	}
}
```
Output:
```
Custom error:  除数不能为零
Execute instruction normally
```
if `num2 = 5` or other values here, we only got output:`2`

### 数组
```go
func main() {
	// 输入一组数，求它们的和与平均值,并显示所有输入的内容
	var scores [5]int32
	for i := 0; i < len(scores); i++ {
		fmt.Printf("Please enter the No.%v's value: ", i)
		fmt.Scanln(&scores[i])
	}

	for i := 0; i < len(scores); i++ {
		fmt.Printf("No.%v's value: %v", i, scores[i])
	}

	var sum int32
	for i := 0; i < len((scores)); i++ {
		sum += scores[i]
	}

	var avg float32 = float32(sum) / 5.0
	fmt.Printf("sum = %v, avg = %.3f\n", sum, avg)
}

```
Output:
```
Please enter the No.0's value: 79
Please enter the No.1's value: 89
Please enter the No.2's value: 80
Please enter the No.3's value: 76
Please enter the No.4's value: 88
No.0's value: 79
No.1's value: 89
No.2's value: 80
No.3's value: 76
No.4's value: 88
sum = 412, avg = 82.400
sum = 413, avg = 82.600
```

使用 for-range 循环，切片
```go
	for key, value := range scores {
		fmt.Printf("No.%v's value: %v\n", key, value)
	}
```

#### 数组的初始化方式
```go
var arr1 [3]int = [3]int{3, 6, 9}
var arr2 = [3]int{1, 2, 3}
var arr3 = [...]int{4, 5, 6, 7, 8}
var arr4 = [...]int{2: 66, 0: 33, 1: 99, 3: 88}
arr5 := [3]int{1, 2, 3}
arr6 := [...]int{4, 5, 6}
arr7 := [...]int{3: 22, 1: 55, 0: 34, 2: 0}
```
数组的数据类型是[3]int , [4]int等等，所以这两个或其他长度的数组类型并不相等，
可以把数组类型当作parameter
`func test(arr [3]int) {}`

通过引用的方式传递(指针)
`func test(arr *[3]int) {}`
嵌套使用取地址和解引用
```go
func main() {
	arr5 := [3]int{1, 2, 3}
	arr6 := [...]int{4, 5, 6}
	arr7 := [...]int{3: 22, 1: 55, 0: 34, 2: 1}
	fmt.Println(arr5)
	fmt.Println(arr6)
	fmt.Println(arr7)
	fmt.Println(test1(&arr6))
	fmt.Printf("(*(arr7))[0] = %v", (*(&arr7))[0])
}

func test1(arr *[3]int) (length int) {
	// 这里arr 存储的是地址
	return len(arr)
}
```
Output:
```
[1 2 3]
[4 5 6]
[34 55 1 22]
3
(*(arr7))[0] = 34
```

使用`new(Type) *Type`获取分配的空间进行数组的定义和初始化，注意new()返回的是指向这片空间的指针
```go
// The new built-in function allocates memory. The first argument is a type,
// not a value, and the value returned is a pointer to a newly
// allocated zero value of that type.
func main() {
	arr := new([3]int)
	fmt.Println(*arr)
}
```
Output:`[0 0 0]`

使用make

#### 多维数组
```go
func main() {
	// 省略初始化元素
	arr := [2][3]int{}
	fmt.Printf("&arr = %p, arr = %v\n", &arr, arr)
	fmt.Printf("&arr[0] = %p, &arr[1] = %p\n", &arr[0], &arr[1])
	fmt.Printf("arr[0] = %v, arr[1] = %v\n", arr[0], arr[1])
}
```
Output:`&arr = 0xc00000e2d0, arr = [[0 0 0] [0 0 0]]`

多维数组的遍历
```go
func main() {
	arr := [3][3]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
	fmt.Println(arr)
	// for 循环遍历
	for i := 0; i < len(arr); i++ {
		for j := 0; j < len(arr[i]); j++ {
			fmt.Printf("[%v] ", arr[i][j])
		}
		fmt.Println()
	}
	// for-range
	for key, value := range arr {
		for i, k := range value {
			fmt.Printf("[%v][%v]=%v ", key, i, k)
		}
		fmt.Println()
	}
}
```
Output:
```
[[1 2 3] [4 5 6] [7 8 9]]
[1] [2] [3]
[4] [5] [6]
[7] [8] [9]
[0][0]=1 [0][1]=2 [0][2]=3
[1][0]=4 [1][1]=5 [1][2]=6
[2][0]=7 [2][1]=8 [2][2]=9
```

#### slice 切片
slice是golang中特殊的数据类型，切片是建立在数组类型之上的抽象，切片是对数组的一个连续片段的引用
```go
func main() {
	arr := [6]int{1, 3, 5, 7, 9, 11}
	var slice []int = arr[1:3]
	fmt.Println(slice)

	arr1 := [3][3]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
	slice1 := arr1[0:1]
	fmt.Println(slice1)
	fmt.Println(len(arr1[0:2]))
}
```
Output
```
[3 5]
[[1 2 3]]
2
```
len(slice)获取切片或者数组的元素个数，{{1,2,3},{4,5,6}} 像这样的切片或者数组len()返回的值为`2`，因为它只能看见有两个类型为`[3]int`的元素

获取切片的容量`cap()``
```go
func main() {
	arr := [4][4][4]int16{}
	m := int16(1)
	for i := 0; i < 4; i++ {
		for j := 0; j < 4; j++ {
			for k := 0; k < 4; k++ {
				arr[i][j][k] = m
				m++
			}
		}
	}
	fmt.Println("arr:")
	for _, j := range arr {
		for _, v := range j {
			fmt.Printf("%2v", v)
		}
		fmt.Println()
	}
	fmt.Println("cap(arr[1:3]):", cap(arr[1:3]))
	fmt.Println("arr[0:0]:", arr[0:0])
}
```
Output:
```
arr:
[ 1  2  3  4][ 5  6  7  8][ 9 10 11 12][13 14 15 16]
[17 18 19 20][21 22 23 24][25 26 27 28][29 30 31 32]
[33 34 35 36][37 38 39 40][41 42 43 44][45 46 47 48]
[49 50 51 52][53 54 55 56][57 58 59 60][61 62 63 64]
cap(arr[1:3]): 3
arr[0:0]: []
```
`arr[0:0]`这样的切片返回的结果为[]而不是nil
```go
	if arr[0:0] != nil {
		fmt.Println("arr[0:0] is not nil")
	}
```
Problem:`this nil check is always true (SA4031)`

##### 通过make来创建切片
make(type, len, cap) 切片类型，切片长度，切片容量
```go
func main() {
	slice := make([]int, 4, 20)
	fmt.Println(slice)
	fmt.Println(len(slice))
	fmt.Println(cap(slice))
}
```
make在底层创建一个长度为20的array，基于这个数组创建一个长度为4的slice，但无法访问到这个数组，只能访问到这个切片，数组对外不可见，只能通过切片间接的访问到数组
Output:
```
[0 0 0 0]
4
20
```

doc:
```go
//	Slice: The size specifies the length. The capacity of the slice is
//	equal to its length. A second integer argument may be provided to
//	specify a different capacity; it must be no smaller than the
//	length. For example, make([]int, 0, 10) allocates an underlying array
//	of size 10 and returns a slice of length 0 and capacity 10 that is
//	backed by this underlying array.
```
或者说说如果不指定第二个integer参数，make(Type, size ...int), 创建的slice的 capacity和length一样大

##### 向切片中加入元素
```go
func main() {
	slice := make([]int, 0, 5)
	fmt.Printf("slice: %v\nlen(slice): %v\ncap(slice): %v\n--------\n", slice, len(slice), cap(slice))
	slice = append(slice, 1, 2)
	fmt.Printf("slice: %v\nlen(slice): %v\ncap(slice): %v\n--------\n", slice, len(slice), cap(slice))
	slice = append(slice, 3, 4, 5)
	fmt.Printf("slice: %v\nlen(slice): %v\ncap(slice): %v\n--------\n", slice, len(slice), cap(slice))
	slice = append(slice, 6)
	fmt.Printf("slice: %v\nlen(slice): %v\ncap(slice): %v\n--------\n", slice, len(slice), cap(slice))
}
```
Output:
```
slice: []
len(slice): 0
cap(slice): 5
--------
slice: [1 2]
len(slice): 2
cap(slice): 5
--------
slice: [1 2 3 4 5]
len(slice): 5
cap(slice): 5
--------
slice: [1 2 3 4 5 6]
len(slice): 6
cap(slice): 10
--------
```
可以看到，超出容量后的值，append会增加一倍的容量(不一定),append会创建一个容量更大的新数组，将原有内容复制到新数组，
##### copy
```go
func main() {
	a := []int{1, 2, 3, 4, 5, 6}
	b := make([]int, 12)
	copy(b, a)
	fmt.Println(b)
}
```
Output:
```
[1 2 3 4 5 6 0 0 0 0 0 0]
```
会按照顺序将a的内容复制到b替换，从起始位置开始替换

### 映射 map
`map的key-value`是无序的
```go
func main() {
	// 只声明map内存是没有分配空间的,必须通过make()进行初始化
	var a map[int]string = make(map[int]string, 10)
	fmt.Println(a)
	a[10001] = "张三"
	a[10002] = "李四"
	a[10003] = "王五"
	fmt.Println(a)
}

```
Output:
```
map[]
map[10001:张三 10002:李四 10003:王五]
```

假如有重名的键
```go
func main() {
	// 只声明map内存是没有分配空间的,必须通过make()进行初始化
	var a map[int]string = make(map[int]string, 10)
	a[10001] = "张三"
	a[10001] = "李四"
	fmt.Println(a)
}

```
Output:
```
map[10001:李四]
```
同名键存在后，使得`"李四"`替换了`"张三"`
key不能重复，但value可以
`假如通过任意方式定义了map后，又再次重新使用make等方式分配内容，原有的内容将会消失，等于clear()了map`

map定义方式
```go
	a := make(map[int]string, 10)
	b := make(map[int]string)
	c := map[int]string{
		10001: "张三",
		10002: "李四",
	}
```
#### 删除一个键值对 delete()
```go
func main() {
	a := map[int]string{
		10001: "张三",
		10002: "李四",
	}
	delete(a, 10001)
	fmt.Println(a)
}
```
Output:
```
map[10002:李四]
```

#### 删除所有键值对 clear()
删除不存在的键值对不会报错
- 删除map中的所有内容:
遍历手动删除，或者make一个新的map，让原来的内容被GC当作垃圾回收，使用clear()函数
```
// The clear built-in function clears maps and slices.
// For maps, clear deletes all entries, resulting in an empty map.
```

#### 查找
`value, bool = map[key]`
```go
func main() {
	a := map[int]string{
		10001: "张三",
		10002: "李四",
	}

	value, flag := a[10003]
	fmt.Println(value, " ", flag)
}

```
Output:
`   false`

#### map的长度
```go
func main() {
	a := map[int]string{
		10001: "张三",
		10002: "李四",
		10003: "王五",
		123:   "xxx",
	}
	fmt.Println(len(a))
}
```
Output:
`4`
#### map遍历
```go
func main() {
	a := map[int]string{
		101: "张三",
		102: "李四",
		123: "xxx",
		103: "王五",
	}
	for k, v := range a {
		fmt.Printf("%v:\"%v\"\n", k, v)
	}
}
```
Output:
```
103:"王五"
101:"张三"
102:"李四"
123:"xxx"
```
#### map嵌套
```go
func main() {
	a := make(map[string]map[int]string)
	a["班级1"] = make(map[int]string)
	a["班级1"][1001] = "张三"
	a["班级1"][1002] = "李四"
	a["班级2"] = map[int]string{
		1001: "asd",
		1008: "fas",
	}
	fmt.Println(a)
}

```
Output:
`map[班级1:map[1001:张三 1002:李四] 班级2:map[1001:asd 1008:fas]]`

### 面向对象(OOP)
Golang支持OOP，但和传统的面向对象有区别，并不是传统的面向对象语言，或者说Golang有着支持面向对象编程的特性
Golang没有class，结构体(struct)和其他编程语言的类(class)有同等的地位，可以理解为是基于struct来实现的OOP
Golang非常简洁，去掉了传统OOP语言的方法重载，构造方法，析构函数，this等
Golang仍然有OOP的继承，封装和多态等特性，只是实现的方式和其他额OOP语言不同。
#### 结构体定义strcut
```go
type Teacher struct {
	Name   string
	Age    int
	School string
}

func main() {
	teaher1 := Teacher{
		Name:   "张三",
		Age:    30,
		School: "xx Collage",
	}
	fmt.Println(teaher1)
	teaher1.Name = "李四"
	teaher1.Age = 26
	teaher1.School = "yy Collage"
	fmt.Println(teaher1)
}
```
和正常的数据类型一样的进行初始化，访问改变成员变量使用 `.` 表示
Output:
```
{张三 30 xx Collage}
{李四 26 yy Collage}
```
#### 使用指针引用结构体
```go
func main() {
	var t *Teacher = new(Teacher)
	t1 := new(Teacher)
	(*t).Name = "John"
	(*t).Age = 22
	(*t).School = "xx Collage"
	fmt.Println(*t)
	fmt.Println(t)
	// Golang提供了简洁的赋值方式，可以直接：
	t1.Name = "Jack"
	t1.Age = 20
	t1.School = "Night City Collage"
	fmt.Println(*t1)
	fmt.Println(t1)
}
```
Output:
```
{John 22 xx Collage}
&{John 22 xx Collage}
&{Jack 20 Night City Collage}
{Jack 20 Night City Collage}
```
同样的，可以使用`t := *Teacher{}`的方式创建结构体指针

#### 结构体之间的转换
结构体之间的转换需要拥有相同的字段
显式转换
```go
type Student struct {
	Age int
}

type Person struct {
	Age int
}

func main() {
	s := Student{10}
	p := Person{10}
	s = Student(p)
	fmt.Println(s)
	fmt.Println(p)
}
```
Output:
```
{10}
{10}
```

定义类型别名(Alias)也不能直接赋值，同样需要显式转换
```go
type Student struct {
	Age int
}

type Stu Student

func main() {
	s1 := Student{Age: 19}
	var s2 Stu
	s2 = s1
	fmt.Println(s2)
}
```
Problems:
```
cannot use s1 (variable of type Student) as Stu value in assignment
```
##### 方法
方法作用在指定的数据类型上，和指定的数据类型绑定
`func (a A) test() {}` 体现方法`test()`和结构体A的绑定关系

```go
type Person struct {
	Name string
}

// 绑定到Person
func (person Person) test() {
	fmt.Println(person.Name)
}

func main() {
	p := Person{}
	p.Name = "Jack"
	p.test()
}
```
Output:
`Jack`

绑定到自定义类型的函数(方法)，依然遵循值传递机制(值拷贝)

#### 封装
就像Java类似地封装代码，根据go的规则，identifier首字母大写表示public，小写表示private
In GoDemo1/person.go
```go
package person

import "fmt"

type person struct {
	Name string
	age  int
}

func NewPerson(name string) (p *person) {
	return &person{
		Name: name,
	}
}

func (p *person) SetAge(age int) {
	if age >= 0 && age <= 150 {
		p.age = age
	} else {
		fmt.Println("传入的年龄范围有误")
	}
}

func (p *person) GetAge() (age int) {
	return p.age
}

```
In GoDemo1/main.go
```go
func main() {
	p := person.NewPerson("张三")
	p.SetAge(22)
	fmt.Println(p.Name)
	fmt.Println(*p)
}
```
Output:
```
张三
{张三 22}
```

#### 类似的继承
当多个结构体存在相同的属性(字段)和方法时，可以从这些结构体中抽象出结构体，在该结构体中定义这些相同的属性和方法，其他的结构体不需要重新定义这些属性和方法，只需要嵌套一个匿名结构体即可。
```go
package main

import (
	"fmt"
)

type Animal struct {
	Age    int
	Weight float32
}

func (animal *Animal) Shout() {
	fmt.Println("Animal can shout")
}

func (animal *Animal) ShowInfo() {
	fmt.Printf("Animal's age is: %v, weight is: %v\n", animal.Age, animal.Weight)
}

type Cat struct {
	// Use inheritance thought
	Animal
}

// Binding specific method to Cat
func (c *Cat) scratch() {
	fmt.Println("Cat can scratch people")
}

func main() {
	cat := &Cat{}
	cat.Animal.Weight = 5.3
	cat.Animal.Shout()
	cat.Animal.ShowInfo()
	cat.scratch()
}

```
Output:
```go
Animal can shout
Animal's age is: 0, weight is: 5.3
Cat can scratch people
```

结构体可以使用嵌套匿名结构体所有的字段和方法，即，首字母大小写的字段都可以用

```go

package main

import (
	"fmt"
)

type Animal struct {
	Age    int
	weight float32
}

func (animal *Animal) shout() {
	fmt.Println("Animal can shout")
}

func (animal *Animal) showInfo() {
	fmt.Printf("Animal's age is: %v, weight is: %v\n", animal.Age, animal.weight)
}

type Cat struct {
	// Use inheritance thought
	Animal
}

// Binding specific method to Cat
func (c *Cat) scratch() {
	fmt.Println("Cat can scratch people")
}

func main() {
	cat := &Cat{}
	cat.Age = 2
	cat.weight = 3.4
	cat.shout()
	cat.showInfo()
	cat.scratch()
}
```
Output
```
Animal can shout
Animal's age is: 2, weight is: 3.4
Cat can scratch people
```
##### 就近原则
假如继承的子类(结构体)拥有和父级同样的字段，调用或赋值时未指明是哪一个字段，默认调用或赋值最近的一级,或者说掉用自己本身
```go
type X struct {
	a int
	b int
}

type Y struct {
	X
	a int
}

func main() {
	y := Y{}
	y.a = 10
	fmt.Println(y)
}
```
Output:
`{{0 0} 10}`

##### 字段歧义
继承多个父结构体的情况时，假如结构体A，B都含有相同的字段，此时省略匿名结构体访问父类结构体字段时会报错，必须指明访问的是哪一个结构体的字段

```go
type X struct {
	a int
	b string
}

type Y struct {
	c int
	d string
	a int
}

type Z struct {
	X
	Y
}

func main() {
	z := Z{X{10, "aaa"}, Y{20, "bbb", 30}}
	fmt.Println(z)
	fmt.Println(z.a)
}
```
Problem:
`ambiguous selector z.a`
改为`z.X.a` 或 `z.Y.a`才不会产生歧义

##### 匿名数据类型

甚至，结构体成员变量也可以是匿名的
```go
type X struct {
	a int
	b string
	float32
}

func main() {
	x := X{15, "xxx", 12.3}
	fmt.Println(x)
}

```
Output:
`{15 xxx 12.3}`

##### 创建结构体实例同时对匿名结构体初始化
##### 嵌入匿名结构体指针
如上一系列代码已经展示，需要注意的是，对含有匿名字段的结构体初始化，要么全部指定实参对应的形参名，要么全部不指定，匿名字段的形参绑定使用字段所使用的匿名数据类型
访问匿名结构体指针需要使用`*`解引用
```go
type A struct {
	a int
	string
}

type B struct {
	*A
	string
}

func main() {
	b := B{&A{a: 30, string: "aaaa"}, "bbbb"}
	fmt.Println(b)
	fmt.Println(*b.A)
}
```
Output:
```
{0xc000008048 bbbb}
{30 aaaa}
```
字段A是匿名结构体指针，需要解引用才能获取其中的值，而明面上它只是一个地址

##### 成员字段为结构体(组合模式)
```go
type A struct {
	a int
}

type B struct {
	a int
	b string
	c A
}

func main() {
	b := B{a: 10, b: "abc", c: A{12}}
	fmt.Println(b)
}
```
Output:
`{10 abc {12}}`

#### interface{} 接口
接口：定义规则，定义规范，定义某种能力
Golang不需要显示的实现接口，没有implement关键字

Golang中实现接口是基于方法的，而不是基于接口的

接口的目的是定义规范，由别人来实现接口
```go
package main

import (
	"fmt"
)

// Define interface
type SayHello interface {
	sayHello()
}

// implement interface
type Chinese struct {
}

func (person Chinese) sayHello() {
	fmt.Println("你好")
}

type American struct {
}

func (person American) sayHello() {
	fmt.Println("Hello")
}

func greet(s SayHello) {
	s.sayHello()
}

func main() {
	p1 := Chinese{}
	greet(p1)
	p2 := American{}
	greet(p2)
}

```
Output:
```
你好
Hello
```

接口本身不能创建任何实例，但可以指向一个已经创建的实例
```go
func main() {
	p1 := Chinese{}
	greet(p1)
	p2 := American{}
	greet(p2)
	var s SayHello = p1
	s.sayHello()
}
```

只要是自定义数据类型，就可以实现接口
一个接口可以继承多个别的接口
```go
type CInterface interface {
	c()
}

type BInterface interface {
	b()
}

type AInterface interface {
	BInterface
	CInterface
	a()
}

type Student struct {
}

func (s Student) a() {
	fmt.Println("a")
}

func (s Student) b() {
	fmt.Println("b")
}

func (s Student) c() {
	fmt.Println("c")
}

func main() {
	var s Student
	var a AInterface = s
	a.a()
	a.b()
	a.c()
}
```
Output:
```
a
b
c
```

interface类型默认是一个指针(引用类型),如果没有对interface初始化就使用，那么会输出nul


空接口没有任何方法，所以可以理解为所有类型都实现了空接口
```go
	var num float64 = 9.3
	var e2 interface{} = num
	fmt.Println(e2)
```
Output:
`9.3`

#### 多态
多态是通过接口实现的
如上文的SayHello 中，可以通过上下文来识别具体是什么类型，具体是什么类型的实例，就体现多态

### 断言
```go
package main

import (
	"fmt"
)

type SayHello interface {
	sayHello()
}

type Chinese struct {
	name string
}

type American struct {
	name string
}

func (person Chinese) sayHello() {
	fmt.Println("你好")
}

func (person Chinese) sayChinese() {
	fmt.Println("其他的话")
}

func (person American) sayHello() {
	fmt.Println("hello")
}

func (person American) sayAmerican() {
	fmt.Println("Something")
}

func greet(s SayHello) {
	s.sayHello()
	switch s.(type) {
	case Chinese:
		ch := s.(Chinese)
		ch.sayChinese()
	case American:
		ch := s.(American)
		ch.sayAmerican()
	}
}

func main() {
	c := Chinese{name: "Chinese"}
	a := American{name: "American"}

	greet(c)
	greet(a)
}
```
Output:
```
你好
其他的话
hello
Something
```

### 文件读写 File

#### 打开文件
`Open(name string) (*File, error)`
```go
func main() {
	// 打开文件
	file, err := os.Open("D:/projects/Golang/test.txt")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Printf("file: %v", file)
	}
}
```
If successful, Output:
`file: &{0xc000076a00}`
failure, Output:
`open D:/projects/Golang/test2.txt: The system cannot find the file specified.`

#### 关闭文件
File.Close()
```go
	// 关闭文件
	err2 := file.Close()
	if err2 != nil {
		fmt.Println("File closing failure")
	}
```
Output:
`File closed successfully.`
如果读取失败，文件会关闭异常

#### 读取文件内容 io流(io, io/ioutil)

```go
func main() {
	// 读取文件
	content, err := ioutil.ReadFile("D:/projects/Golang/test.txt")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(content)
	}
}
```
在对应路径的文件内容:
```
Line1: context
ABCDEFG
中文
```
Output:
`[76 105 110 101 49 58 32 99 111 110 116 101 120 116 13 10 65 66 67 68 69 70 71 13 10 228 184 173 230 150 135]`
fmt输出file内容为[]byte形式,格式化输出内容为10进制的UTF-8编码
可以看到换行操作是由 `13 10`完成的, 对应`回车 换行`操作
回车:光标定位到行开头

需要注意的是
```
Package ioutil implements some I/O utility functions.

Deprecated: As of Go 1.16, the same functionality is now provided by package io or package os, and those implementations should be preferred in new code. See the specific function documentation for details.
```

ioutil.Readfile():
```go
func ReadFile(filename string) ([]byte, error) {
	return os.ReadFile(filename)
}
```

带有缓冲区的读取文件内容的方式 4096字节
```go
func main() {
	file, err := os.Open("d:/projects/Golang/test.txt")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(file)
	} 

	defer file.Close()

	// Create a stream

	reader := bufio.NewReader(file)

	for {
		str, err := reader.ReadString('\n')
		fmt.Print(str)
		if err == io.EOF {
			break
		}
	}
}
```
Output:
```
&{0xc000076a00}
Line1: context
ABCDEFG
中文
```
