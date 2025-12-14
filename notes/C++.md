

## 🔍 传值 vs 传引用

现在我们来分析：

### ✅ 传值（`std::string name`）

```cpp
Player(std::string name)
```

> 表示调用者把字符串内容**复制一份**给构造函数

- **内存分配多了一次拷贝**
- 性能稍差，尤其是字符串很大时
- 安全性高（函数内部怎么改不会影响原字符串）

------

### ✅ 传引用（`const std::string& name`）

```cpp
Player(const std::string& name)
```

> 表示调用者把已有的字符串**地址传进来**（不会复制）

- **高效，不拷贝**
- 加了 `const` 表示函数内部**不能修改**这个字符串
- 推荐做法 ✅ 尤其是在类构造函数或函数参数里传大型对象（如 `std::string`, `std::vector`）

------

### 🧪 举例对比内存与速度

```C++
cpp复制编辑std::string str = "hello world";

// 传值（复制一份）
Player a(str); // 构造时创建 str 的一个拷贝，再构造 m_Name，又拷贝一次

// 传引用（直接用原来的）
Player b("hello world"); // 构造时传引用，仅构造 m_Name 一次
```

传值会造成多一次 `std::string` 的内存开销（尤其是在构造类成员的时候），所以我们更推荐用：

```C++
const std::string& 参数名
```

## const char* Name

这是一个经典的 C/C++ 语法问题，我们来**深入拆解**下面这句代码：

```cpp
const char* Name;
```

------

### 🧠 一、这句代码代表什么意思？

它定义了一个变量 `Name`，它是：

👉 “指向 `const char` 类型的**指针**”
 意思是：`Name` 这个变量的值是一个指针（即地址），这个地址指向的是一块不能修改的字符数据。

------

### 📦 二、这个变量本身占多少内存？

### 答案：**4 或 8 个字节**

具体取决于你程序运行的环境：

| 系统位数 | 指针大小（即 `Name` 变量占的内存） |
| -------- | ---------------------------------- |
| 32位系统 | 4 字节（32 bits）                  |
| 64位系统 | 8 字节（64 bits）                  |

⚠️ 注意：这只是指针变量本身占用的空间，不是它指向的字符串内容！

------

### 🧵 三、那指向的字符串数据在哪？

`const char* Name` 通常用来指向一个字符串字面量或其他字符数组，比如：

```cpp
Name = "Hello";
```

这时 `"Hello"` 是编译器放在**只读存储区（RODATA）**的一段内存，而 `Name` 是一个指针，指向 `"H"` 开始的位置。

------

### 🧷 四、为什么要用 `const char*` 而不是 `char*`？

加上 `const` 表示**你不会修改这个字符串**，这样有几个好处：

1. ✅ **保护字符串字面量**（字符串常量不能修改）

2. ✅ 编译器可以进行优化

3. ✅ 防止误操作，比如下面是**错误代码**：

   ```cpp
   char* str = "Hello";
   str[0] = 'h';  // ❌ 很可能段错误（Segfault）
   ```

   正确写法是：

   ```cpp
   const char* str = "Hello";  // 不允许改
   ```

------

### 🧪 五、内存结构图示意（64 位系统）

假设你写了：

```cpp
const char* Name = "Hello";
```

内存大致结构如下：

```
[Name指针] --> 0x600abc
                   ↓
[只读区]：   'H' 'e' 'l' 'l' 'o' '\0'
```

- `Name` 本身在栈或静态内存，占 8 字节
- `"Hello"` 存在程序的只读数据段

------

### 🧱 六、拓展：`const char*` vs `char const*` vs `char* const`

这几个容易混淆：

| 声明                               | 含义                     | 是否能修改指针 | 是否能修改指向数据 |
| ---------------------------------- | ------------------------ | -------------- | ------------------ |
| `const char* p` 或 `char const* p` | 指针指向的是**常量字符** | ✅ 可以         | ❌ 不可以           |
| `char* const p`                    | **常量指针**指向字符     | ❌ 不可以       | ✅ 可以             |
| `const char* const p`              | 指向常量的常量指针       | ❌ 不可以       | ❌ 不可以           |

------

### ✅ 总结

```cpp
const char* Name;
```

代表：

- 是一个“指向不可修改字符”的指针
- 本身在内存中占用 4（32位）或 8（64位）字节
- 常用于指向字符串字面量，保护数据不被误改

如果你还想深入了解字符串数组、动态分配字符数组、二维字符串数组（`char* argv[]` 这类），我可以继续讲～

## 类

### Constructors

（初始化方式）无返回值。系统有默认函数，但不会是0（所以我们要初始化），可以重载，可以删除（删除后不能创建类的实例），public的才可以创建类的实例

```C++
#include <iostream>
using namespace std;
class Entity {
public:
	float X, Y;

	Entity() {
		X = 0; Y = 0;
	}

	Entity(float x, float y) {//重载
		X = x;
		Y = y;
	}

	void Print(){
		cout << X << "," << Y << endl;
	}
	

};

class Log {
//private:如果是私有的就不能创建类的实例
//	Log() {}

public:
	//Log() = delete;删除构造函数就不能创建类的实例
	static void Write() {
	}
};

int main() {
	Log l;
	Entity e(10.0f,5.0f);
	e.Print();
	cin.get();
}
```

### Destructors

作用域结束后自动调用

```C++
~类名() {
		
	}
```



```C++
class Entity {
public:
	float X, Y;

	Entity() {//构造函数
		X = 0.0f;
		Y = 0.0f;
		cout << "Created Entity" << endl;
	}
    
	~Entity() {//析构函数
		cout << "Destroyed Entity" << endl;
	}

	void Print(){
		cout << X << "," << Y << endl;
	}
	

};



void Function() {
	Entity e;
	e.Print();
}//在这个函数作用域结束时调用析构函数，因为在栈上建立了一个实例，在作用域结束时要销毁
int main() {
	
	Function();
	cin.get();
}
```

### Inheritance

```C++
#include <iostream>
using namespace std;
class Entity {
public:
	float X, Y;

	void Move(float xa, float ya)
	{
		X += xa;
		Y += ya;
	}
};

class Player:public Entity
{
public:
	const char* Name;

	void PrintName() {
		cout << Name << endl;
	}
};

int main() {

	cout << sizeof(Entity) << endl;
	cout << sizeof(Player) << endl;
	
	Player player;
	cout << sizeof(player.Name) <<endl;//64位的地址为8bite
	player.Move(3, 4);
	player.X = 2;

	
	cin.get();
}
```

### Virtual Functions



## 1.18-1.24

任务：

- [x] 数学建模冬季杯（使用latex完成论文）
- [x] 完成答辩ppt修改
- [x] C++复习（基本输入输出、变量、条件语句、控制流语句、字符的基本用法、类的基础部分）手写笔记见压缩文档建议以中等图标查看
- [x] 刷题，写了15道基础题，证明见图片，和下述文档，每道题用三种语言（python、C、C++）写了一遍，附有总结，ai部分有标注（比例极小）
- [x] 杂七杂八学了一点点课内知识

------



## 终端的一些路径操作

```cmd
C:\Users\Anna>D://切换到D盘

D:\>cd D:\Qt//cd进入文件

dir//查看目录

D:\Qt>cd..//操作系统中.代指当前目录，..代指父目录，退出当前文件夹

D:\>C:

C:\Users\Anna>

start D:\Qt\example.txt//直接打开文件

```

<img src="C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250124114347792.png" alt="image-20250124114347792" style="zoom:33%;" />

## 有关指针部分的理解

问题：

指针的类型代表的是指针保存的地址的大小，还是指针只向地址的那个值的大小，我以为在64位的操作系统上地址都是8字节的？

chatgpt：你的问题很关键，涉及到指针的本质以及它在不同环境下的行为。我们逐步来解释。

------

1. **指针的本质**

指针本质上是一个变量，用于存储内存地址。它有两个关键属性：

- **指针本身的大小**：指针变量保存的地址的大小。
- **指针指向的数据类型**：指针指向的内存区域的数据类型，这决定了解释指针指向地址中的数据的方式。

------

2. **指针的大小**

在现代计算机中，指针的大小取决于操作系统的位数和编译器的实现：

- **在 32 位系统**：指针通常是 4 字节（32 位），因为地址空间是 32 位。
- **在 64 位系统**：指针通常是 8 字节（64 位），因为地址空间是 64 位。

无论指针指向的数据类型是什么（`int*`、`char*`、`float*`），指针本身的大小只取决于系统的位数。

------

3. **指针指向的数据类型**

指针的类型决定了它指向的数据的大小和如何解释数据：

- 例如

  ：

  - `int*` 表示指针指向的是一个 `int` 类型的值，通常是 4 字节（在大多数系统上）。
  - `char*` 表示指针指向的是一个 `char` 类型的值，通常是 1 字节。
  - `double*` 表示指针指向的是一个 `double` 类型的值，通常是 8 字节。

指针的类型不会影响指针本身的大小，而是影响指针解引用时读取的内存大小。

------

4. **64 位系统上的地址**

在 64 位操作系统上，内存地址的宽度是 64 位，因此指针的大小是 8 字节。然而，指针指向的数据的大小仍然由指针的类型决定：

- `int*` 指向的 `int` 通常是 4 字节，但指针本身仍然是 8 字节。
- `char*` 指向的 `char` 是 1 字节，但指针本身仍然是 8 字节。

------

5. **小结**

- **指针的大小**：由系统架构（32 位或 64 位）决定，与指针的类型无关。
- **指针指向的数据大小**：由指针的类型决定，例如 `int*` 表示 4 字节，`double*` 表示 8 字节。
- 在 64 位操作系统上，所有指针的大小通常是 8 字节，因为地址空间是 64 位。

------

6. **实践验证**

你可以通过以下代码验证指针的大小和指向数据的大小：

```c
#include <stdio.h>

int main() {
    int a = 10;
    double b = 20.0;

    int *p1 = &a;
    double *p2 = &b;

    printf("Size of int pointer: %zu bytes\n", sizeof(p1));  // 输出指针大小
    printf("Size of double pointer: %zu bytes\n", sizeof(p2));  // 输出指针大小
    printf("Size of int value: %zu bytes\n", sizeof(*p1));  // 输出指向数据大小
    printf("Size of double value: %zu bytes\n", sizeof(*p2));  // 输出指向数据大小

    return 0;
}
```

**输出结果（在 64 位系统上）**：

```
Size of int pointer: 8 bytes
Size of double pointer: 8 bytes
Size of int value: 4 bytes
Size of double value: 8 bytes
```

这说明：

- 无论指针类型如何，指针本身的大小在 64 位系统上是 8 字节。
- 指针指向的数据大小由指针的类型决定。

## 日志类（Log）代码

```C++
class Log {
public:
	const int LogLevelError = 0;
	const int LogLevelWarning = 1;
	const int LogLevelInfo = 2;//信息跟踪
private:
	int m_LogLevel=LogLevelInfo;//默认情况下设置为信息跟踪
public:
	void SetLevel(int Level) 
	{
		m_LogLevel = Level;
	}
	void Error(const char* message)
	{
		if(m_LogLevel>=LogLevelError)
			cout << "[Error]:" << message << endl;
	}
	void Warn(const char* message)
	{
		if (m_LogLevel >= LogLevelWarning)
		cout << "[Warning]:"<<message << endl;
	}
	void Info(const char* message)
	{
		if (m_LogLevel >= LogLevelInfo)
		cout << "[Info]:" << message << endl;
	}
};


int main() {

	Log log;
	log.SetLevel(log.LogLevelWarning);
	log.Warn("Hello");
	log.Error("Hello");
	log.Info("Hello");
	cin.get();
}
```



## 变量、输入输出、顺序语句

### 604 圆的面积

> [!NOTE]
>
> **C++中输出四位小数**
>
> 一般保留5位小数
>
> 首先包含一个头文件 #include<iomanip>
>
> *setprecision*：功能是控制输出流显示浮点数的**有效数字个数**，注意是有效数字。
>
> 如果和fixed合用的话，可以控制小数点后面有几位。
>
> **定义的顺序**
>
> 先输入再定义有输入变量的变量

<img src="C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250118222319872.png" alt="image-20250118222319872" style="zoom:33%;" />

```C++
#include <iostream>
#include <iomanip>
#define PI 3.14159
using namespace std;

int main(){
    double R,A;
    cin>>R;
    A=PI*R*R;
    /*
    错误案例，在计算A之前要输入R
    A=PI*R*R;
    cin>>R;
    */
    cout<<"A="<<fixed<<setprecision(4)<<A<<endl;
    /*
    需要运用头文件<iomanip>
      “fixed<<setprecision(4)”，括号中的数字表示为输出保留小数点后几位，
      具体格式为：cout<<fixed<<setprecision(保留位数)<<变量<<endl;
    */
}
```

区别

```C++
	std::cout << "保留小数位数：" << std::endl;
    std::cout << std::fixed << std::setprecision(3) << num1 << std::endl; // 输出：123.456
    std::cout << std::fixed << std::setprecision(3) << num2 << std::endl; // 输出：0.001

    std::cout << "保留有效数字：" << std::endl;
    std::cout << std::setprecision(3) << num1 << std::endl; // 输出：123
    std::cout << std::setprecision(3) << num2 << std::endl; // 输出：0.000789
```

**C语言解题**

```C
#include <stdio.h>
#define PI 3.14159

double A,R;//doble是双精度比float精度高
int main(){
    scanf("%lf",&R);//输入的时候用lf
    A=PI*R*R;
    printf("A=%.4f",A);//输出的时候用f
}
```

double是双精度（lf）输入的时候用lf，输出的时候用f  8Bite

float是单精度（f）4Bite

一般用double，在C++中用float定义变量的时候

```
float A=5.5f;//一般要加f，不然还是double类型
```



Python题解

```python
R=eval(input())
PI=3.14159
print("A={:.4f}".format(PI*R*R))
```

> [!NOTE]
>
> Python格式化输出：
>
> ```
> print("{} {}".format("Hello","Nihao"))
> print("{:.4f} {}".format(a,"Nihao"))
> ```
>
> | 数字       |                             格式                             | 输出                   | 描述                         |
> | :--------- | :----------------------------------------------------------: | :--------------------- | :--------------------------- |
> | 3.1415926  |                            {:.2f}                            | 3.14                   | 保留小数点后两位             |
> | 3.1415926  |                           {:+.2f}                            | +3.14                  | 带符号保留小数点后两位       |
> | -1         |                           {:-.2f}                            | -1.00                  | 带符号保留小数点后两位       |
> | 2.71828    |                            {:.0f}                            | 3                      | 不带小数                     |
> | 5          |                           {:0>2d}                            | 05                     | 数字补零 (填充左边, 宽度为2) |
> | 5          |                           {:x<4d}                            | 5xxx                   | 数字补x (填充右边, 宽度为4)  |
> | 10         |                           {:x<4d}                            | 10xx                   | 数字补x (填充右边, 宽度为4)  |
> | 1000000    |                             {:,}                             | 1,000,000              | 以逗号分隔的数字格式         |
> | 0.25       |                            {:.2%}                            | 25.00%                 | 百分比格式                   |
> | 1000000000 |                            {:.2e}                            | 1.00e+09               | 指数记法                     |
> | 13         |                           {:>10d}                            | 13                     | 右对齐 (默认, 宽度为10)      |
> | 13         |                           {:<10d}                            | 13                     | 左对齐 (宽度为10)            |
> | 13         |                           {:^10d}                            | 13                     | 中间对齐 (宽度为10)          |
> | 11         | `'{:b}'.format(11) '{:d}'.format(11) '{:o}'.format(11) '{:x}'.format(11) '{:#x}'.format(11) '{:#X}'.format(11)` | `1011 11 13 b 0xb 0XB` | 进制                         |

### 606 平均数1

<img src="C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250118231810987.png" alt="image-20250118231810987" style="zoom:50%;" />



> [!NOTE]
>
> 换行输入
>
> C++中
>
> ```C++
> cin>>A>>B;//C++中的换行输入
> ```
>
> Python
>
> ```python
> #输入 18
> #输入 6
> a=eval(input())//input默认输入字符串
> b=eval(input())
> #逗号间隔输出两个整数
> c,d=eval(input())
> #空格间隔输入两个整数
> e,f=input().split()#split()对字符串用以空格进行分隔，split()返回的是列表，但是可以解包
> e=int(e)
> f=int(f)
> e,f=map(int,input().split())#另一种更简洁的写法
> ```

C++解法

```C++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
    double A,B,MEDIA;
    cin>>A>>B;//C++中的换行输入
    MEDIA=(A*3.5+B*7.5)/11;//C++中‘/’一般是整除，但是如果想变成小数，首先要重新定义变量类型
    cout<<"MEDIA = "<<fixed<<setprecision(5)<<MEDIA<<endl;
    
}
```

C解法

```C
#include <stdio.h>

int main(){
    double A,B,MEDIA;
    scanf("%lf\n%lf",&A,&B);
    MEDIA=(3.5*A+7.5*B)/11;
    printf("MEDIA = %.5f",MEDIA);
}
```

python解法

```python
A=eval(input())
B=eval(input())
print("MEDIA = {:.5f}".format((A*3.5+B*7.5)/11))
```



### 609 工资



<img src="C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250119234108375.png" alt="image-20250119234108375" style="zoom:33%;" />

```C++
#include <iostream>
#include <iomanip>

using namespace std;

int main(){
    int num,work_time;
    double work_price_month,work_price;
    cin>>num>>work_time>>work_price_month;
    work_price=work_price_month*work_time;
    cout<<"NUMBER = "<<num<<endl;
    cout<<"SALARY = U$ "<<fixed<<setprecision(2)<<work_price<<endl;
    
}
```

```C
#include <stdio.h>

int main(){
    int num,work_time;
    double work_price,work_price_month;
    scanf("%d\n%d\n%lf",&num,&work_time,&work_price_month);
    work_price=work_time*work_price_month;
    printf("NUMBER = %d\nSALARY = U$ %.2f",num,work_price);
}
```

```python
num=eval(input())
work_time=eval(input())
work_price_month=eval(input())
work_price=work_price_month*work_time

print("NUMBER = {}\nSALARY = U$ {:.2f}".format(num,work_price))
```

### 615 油耗

```C++
#include <iostream>
#include <iomanip>

using namespace std;

int main(){
    int X;
    double Y;
    
    cin>>X>>Y;
    cout<<fixed<<setprecision(3)<<X/Y<<" km/l"<<endl;
}
```

```C
#include <stdio.h>

int main(){
    int X;
    double Y;
    scanf("%d\n%lf",&X,&Y);
    printf("%.3f km/l",X/Y);
}
```

```Python
X=eval(input())
Y=eval(input())

print("{:.3f} km/l".format(X/Y))
```

### 616 两点之间的距离

> [!NOTE]
>
> C++中的输入不管是换行输入还是空格输入都是一种形式
>
> C++中的数学运算：
>
> `<cmath>`这个库实际上是对 C 语言的 `<math.h>` 头文件的扩展，并将相关函数添加到 `std` 命名空间中。
>
> [`cmath`库用法](https://www.w3schools.com/cpp/cpp_ref_math.asp)
>
> ```C++
> #include <cmath>
> ```
>
> C中的数学运算（常见用法）
>
> ```C
> #include <math.h>
>  
> //平方 pow()
> int a = pow(4,2);// 4的平方=16
>  
> //开方
> int b = pow(9,0.5); // 9的平方根等于3
> int c = sqrt(9);    // 9的平方根等于3
>  
> //整数绝对值
> int c = abs(b-c);
>  
> //浮点数绝对值
> double d = fabs(b-c);
> ```

C++解法

```C++
#include <iostream>
#include <iomanip>
#include <cmath>

using namespace std;

int main(){
    double x1,x2,y1,y2;
    cin>>x1>>y1>>x2>>y2;
    cout<<fixed<<setprecision(4)<<sqrt((x2-x1)*(x2-x1)+(y2-y1)*(y2-y1));
}
```

C解法

```C
#include <stdio.h>
#include <math.h>

int main(){
    double x1,y1,x2,y2;
    scanf("%lf %lf\n%lf %lf",&x1,&y1,&x2,&y2);
    printf("%.4f",sqrt(pow(x2-x1,2)+pow(y2-y1,2)));
}
```

python解法

```python
from math import *
x1,y1=map(float,input().split())
x2,y2=map(float,input().split())
print("{:.4f}".format(sqrt(pow(x2-x1,2)+pow(y2-y1,2))))
```

### 653 钞票

```C++
#include <iostream>

using namespace std;

int main(){
    int num;
    
    cin>>num;
    cout<<num<<endl;
    cout<<num/100<<" nota(s) de R$ 100,00"<<endl;
    cout<<(num%100)/50<<" nota(s) de R$ 50,00"<<endl;
    cout<<((num%100)%50)/20<<" nota(s) de R$ 20,00"<<endl;
    cout<<(((num%100)%50)%20)/10<<" nota(s) de R$ 10,00"<<endl;
    cout<<((((num%100)%50)%20)%10)/5<<" nota(s) de R$ 5,00"<<endl;    
    cout<<(((((num%100)%50)%20)%10)%5)/2<<" nota(s) de R$ 2,00"<<endl;    
    cout<<(((((num%100)%50)%20)%10)%5)%2<<" nota(s) de R$ 1,00"<<endl;       
}
```

第二种解法：

> [!NOTE]
>
> ![image-20250120224949970](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250120224949970.png)

```C++
#include <iostream>

using namespace std;

int main(){
    int num;
    int nums[7]={100,50,20,10,5,2,1};
    cin>>num;
    
    cout<<num<<endl;
    for(int i=0;i<7;i++)
    {
        cout<<num/nums[i]<<" nota(s) de R$ "<<nums[i]<<",00"<<endl;
        num%=nums[i];
    }
}
```

C解法

```c
#include <stdio.h>

int main(){
    int num;
    int nums[7]={100,50,20,10,5,2,1};
    
    scanf("%d",&num);
    printf("%d\n",num);
    for(int i=0;i<7;i++)
    {
        printf("%d nota(s) de R$ %d,00\n",num/nums[i],nums[i]);
        num=num%nums[i];
    }
}
```

python解法

> [!NOTE]
>
> format格式 “{：}“.format()
>
> 在python中
>
> 算术[运算符](https://so.csdn.net/so/search?q=运算符&spm=1001.2101.3001.7020)‘/’表示的是除法运算，和数学中的除法规则是一样的，如：5/2=2.5；
> 算术运算符‘//’表示的是除法取整运算，即返回结果为除法运算商的整数部分，如：5//2=2；

```python
num=eval(input())
nums=[100,50,20,10,5,2,1]
print(num)
for i in range(7):
    print("{:d} nota(s) de R$ {:d},00".format(num//nums[i],nums[i]))
    num =num%nums[i]
```

### 654 时间转换

C++解法

```C++
#include <iostream>
using namespace std;

int main(){
    int time;
    cin>>time;
    cout<<time/3600<<":"<<time%3600/60<<":"<<time%60<<endl;
}
```

C解法

```C
#include <stdio.h>

int main(){
    int time,hours,minutes,seconds;
    scanf("%d",&time);
    hours=time/3600;
    minutes=time%3600/60;
    seconds=time%3600%60;//直接seconds=time%60
    printf("%d:%d:%d",hours,minutes,seconds);
}
```

python解法

```python
time=eval(input())
print("%d:%d:%d"%(time//3600,time%3600//60,time%60))
```

### 605 简单乘积

C++解法

```C++
#include <iostream>

using namespace std;

int main(){
    int a,b,PROD;
    cin>>a>>b;
    cout<<"PROD = "<<a*b;
}
```

C解法

```C
#include <stdio.h>

int main(){
    int a,b,PROD;
    scanf("%d\n%d",&a,&b);
    printf("PROD = %d",a*b);
}
```

python解法

```python
a=eval(input())
b=eval(input())

#print("PROD = ",str(a*b),sep="")
#print(f"PROD = {a*b}")
#print("PROD = %d"%(a*b))
print("PROD = {:d}".format(a*b))
```

### 606 简单计算

![image-20250121133650365](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250121133650365.png)

C++题解

```C++
#include <iostream>
#include <iomanip>

using namespace std;

int main(){
    int a,b,num1,num2;
    double price1,price2;
    cin>>a>>num1>>price1>>b>>num2>>price2;
    cout<<"VALOR A PAGAR: R$ "<<fixed<<setprecision(2)<<num1*price1+num2*price2<<endl;
}
```

C题解

```C
#include <stdio.h>

int main(){
    int a,b,num1,num2;
    double price1,price2;
    scanf("%d %d %lf\n",&a,&num1,&price1);
    scanf("%d %d %lf\n",&a,&num2,&price2);
    printf("VALOR A PAGAR: R$ %.2f",num1*price1+num2*price2);
}
```

python题解

```python
a, b, c = map(eval, input().split())
e, f, g = map(eval, input().split())

print("VALOR A PAGAR: R$ %.2f" % (b * c + f * g))
```

### 612 圆的体积

![image-20250121143310294](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250121143310294.png)

> [!NOTE]
>
> C和C++中 (4/3)无法得到 1.3333…1.3333…，建议在公式中使用 (4/3.0)。

C++解法

```C++
#include <iostream>
#include <iomanip>
#include <cmath>
#define PI 3.14159
using namespace std;

int main(){
    int R;
    cin>>R;
    cout<<"VOLUME = "<<fixed<<setprecision(3)<<(4/3.0)*PI*pow(R,3)<<endl;
}
```

C解法

```c
#include <stdio.h>
#include <math.h>
#define PI 3.14159

int main(){
    int R;
    scanf("%d",&R);
    printf("VOLUME = %.3f",(4/3.0)*PI*pow(R,3));
}
```

python题解

```python
from math import *
R=eval(input())
print("VOLUME = %.3f"%((4/3)*3.14159*pow(R,3)))
```

### 613 面积



![image-20250121144651395](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250121144651395.png)

#### C++控制精度输出

> [!NOTE]
>
> <img src="C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250121144435669.png" alt="image-20250121144435669" style="zoom:45%;" />

第一种写法和第二种写法只需要写一次，后面保持小数点后位数不变

C++解法

```C++
#include <iostream>
#include <iomanip>
#include <cmath>
#define PI 3.14159
using namespace std;

int main(){
    double A,B,C;
    cin>>A>>B>>C;
    cout<<"TRIANGULO: "<<fixed<<setprecision(3)<<A*C*0.5<<endl;
    cout<<"CIRCULO: "<<fixed<<setprecision(3)<<PI*pow(C,2)<<endl;
    cout<<"TRAPEZIO: "<<fixed<<setprecision(3)<<(A+B)*C*0.5<<endl;
    cout<<"QUADRADO: "<<fixed<<setprecision(3)<<pow(B,2)<<endl;
    cout<<"RETANGULO: "<<fixed<<setprecision(3)<<A*B<<endl;
}
```

> [!NOTE]
>
> 使用double双精度

参考别人写的

```C++
cin>>a>>b>>c;
cout.setf(ios_base::fixed);
cout.precision(3);//更加简洁
cout<<"TRIANGULO: "<<a*c*0.5<<endl;
```

C题解

```C
#include <stdio.h>
#include <math.h>
#define PI 3.14159

int main(){
    double A,B,C;
    scanf("%lf %lf %lf",&A,&B,&C);
    printf("TRIANGULO: %.3f\n",A*C*0.5);
    printf("CIRCULO: %.3f\n",PI*pow(C,2));
    printf("TRAPEZIO: %.3f\n",(A+B)*C*0.5);
    printf("QUADRADO: %.3f\n",pow(B,2));
    printf("RETANGULO: %.3f\n",A*B);
    
}
```

python题解

```python
from math import*
PI=3.14159
A,B,C=map(float,input().split())
print("TRIANGULO: %.3f"%(A*C*0.5));
print("CIRCULO: %.3f"%(PI*pow(C,2)));
print("TRAPEZIO: %.3f"%((A+B)*C*0.5));
print("QUADRADO: %.3f"%(pow(B,2)));
print("RETANGULO: %.3f"%(A*B));
```

### 607 平均数2

![image-20250121215635733](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250121215635733.png)

C++题解

```C++
#include <iostream>
#include <iomanip>

using namespace std;

int main(){
    double A,B,C;
    cin>>A>>B>>C;
    cout<<"MEDIA = "<<fixed<<setprecision(1)<<(A*2+B*3+C*5)/10<<endl;
}

```

C题解

```
#include <stdio.h>

int main(){
    double A,B,C;
    scanf("%lf\n%lf\n%lf",&A,&B,&C);
    printf("MEDIA = %.1f",(A*2+B*3+C*5)/10);
}
```

python题解

```python
A=eval(input())
B=eval(input())
C=eval(input())
print("MEDIA = %.1f"%((A*2+B*3+C*5)/10))
```

### 610 工资和奖金

![image-20250123214824259](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250123214824259.png)

##### **C++读取字符串的两种方式：**

> [!NOTE]
>
> ##### **C++读取字符串的两种方式：**
>
> 字符数组：
>
> ```C++
> char example[100];
> cin>>example;
> ```
>
> string库
>
> ```C++
> #include <string>
> 
> string name;
> cin>>name;
> ```
>
> **C中读取字符并打印**（豆包）
>
> ```C
> char str[100];  // 定义字符数组存储字符串，假设字符串长度不超99，还有1个位置存'\0'
> scanf("%s", str);  // 读取字符串存入str
> ```
>
> 比较复杂的：
>
> ```C
> char str[100];
> fgets(str, sizeof(str), stdin);  // 从标准输入读取字符串
> // fgets会读取换行符，若不需要可自行处理，比如：
> if (str[strcspn(str, "\n")] == '\n') {
>     str[strcspn(str, "\n")] = '\0';
>     }
> ```
>
> string.h
>
> ```C
> include <string.h>
> char str[] = "Hello, world!";
> size_t len = strlen(str);
> ```
>
> 在 C 语言中，`size_t`是一种无符号整数类型，用于表示内存字节数。以下是详细介绍：
>
> `size_t`是一个类型别名，在不同的 C 标准库实现中，它通常被定义为`unsigned int`、`unsigned long`或者`unsigned long long`等无符号整数类型。例如在 32 位系统中，可能被定义为`unsigned int`；在 64 位系统中，可能被定义为`unsigned long` 。它被设计用来表示内存中对象的大小、数组的长度等，确保能够容纳系统上可能出现的最大内存字节数。

C++题解

```C++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
    //错误写法没有分配内存空间const char* name;
    //正确写法const char* name=new char[100];//但是没必要
    char name[20];
    double base_salary,month_sell;
    cin>>name>>base_salary>>month_sell;
    cout<<"TOTAL = R$ "<<fixed<<setprecision(2)<<base_salary+0.15*month_sell<<endl;
    
}
```

C题解

```C
#include <stdio.h>

int main(){
    char name[20];
    double base_salary,month_price;
    scanf("%s\n%lf\n%lf",&name,&base_salary,&month_price);
    printf("TOTAL = R$ %.2f",base_salary+0.15*month_price);
    
}
```

python题解

```python
name=input()
base_salary=eval(input())
month_price=eval(input())

#print(f"TOTAL = R$ {base_salary+0.15*month_price:.2f}")
#print("TOTAL = R$ %.2f"%(base_salary+0.15*month_price))
print("TOTAL = R$ {:.2f}".format(base_salary+0.15*month_price))
```

## 1.25-1.30

- [x] 美赛数学建模
- [x] C++刷题
- [x] 过年

### 冒泡排序

两个for循环，最外层循环每次确定一个最大或最小的数的位置n-1

当内层循环经过i次之后，内层只要比较n-1-i次

```C++
#include <iostream>

using namespace std;
//降序排列
void bubblesort(int arr[],int n)
{
    for(int i=0;i<n;i++){
        for(int j=0;j<n-1-i;j++){
            if(arr[j]<arr[j+1]){
                int temp=arr[j];
                arr[j]=arr[j+1];
                arr[j+1]=temp;
            }
        }
    }
}
int main(){
    int arr[5]={6,3,9,7,1};
    bubblesort(arr,5);
    for(int i=0;i<5;i++) cout<<arr[i]<<endl;
}
```

```C++
#include <iostream>
#include <algorithm>
using namespace std;
//降序排列
/* 使用algorithm中的swap函数
    int temp=arr[j];
    arr[j]=arr[j+1];
    arr[j+1]=temp;            
*/
template <typename T>//使用模板，实现对任意类型数据的排序
void bubblesort(T arr[],int n)
{
    for(int i=0;i<n;i++){
        for(int j=0;j<n-1-i;j++){
            if(arr[j]<arr[j+1]){
                swap(arr[j],arr[j+1]);
            }
        }
    }
}
int main(){
    int arr[5]={6,3,9,7,1};
    int n = sizeof(arr) / sizeof(arr[0]);  // 计算数组长度
    bubblesort(arr,n);
    for(int i=0;i<5;i++) cout<<arr[i]<<endl;
}
```

冒泡排序的时间复杂度为 
$$
O(n^2)
$$
适合小规模数据。

大规模数据建议使用快速排序（O(nlog⁡n)）。

在标准冒泡排序中，即使数组已经提前有序，算法仍然会完整执行所有外层循环（共 n−1轮），导致大量**无效的比较操作**。
**标志位检测**的核心思想是：**如果某一轮外层循环中没有发生任何元素交换，说明数组已经完全有序，可以提前终止排序**，从而减少不必要的计算。

```C++
template <typename T>//使用模板，实现对任意类型数据的排序
void bubblesort(T arr[],int n)
{
	bool swapped;
    for(int i=0;i<n;i++){
    	swapped=false;
        for(int j=0;j<n-1-i;j++){
            if(arr[j]<arr[j+1]){
                swap(arr[j],arr[j+1]);
                swapped=true;
            }
        }
        if(!swapped) break;
    }
}
```

### python列表的有关操作

```python
mylist=[4,2,5,9,10]
#sorted()函数默认升序排列
fix_mylist=sorted(mylist)
#降序:sorted(mylist,reverse=True)
fix_mylist_rev=sorted(mylist,reverse=True)
print(fix_mylist)
print(fix_mylist_rev)
#对复杂列表排序：当列表元素是元组、自定义对象等复杂结构时，可使用key参数指定一个函数来提取用于比较的键。
#例如，对包含元组的列表，按元组中第二个元素升序排序：
complex_list = [("老刘", 40), ("老张", 60), ("老李", 47)]
sorted_complex_list_asc = sorted(complex_list, key=lambda x: x[1])
print(sorted_complex_list_asc)
sorted_complex_list_desc = sorted(complex_list, key=lambda x: x[1], reverse=True)
print(sorted_complex_list_desc)

#列表的sort默认升序
mylist.sort()
#降序
mylist.sort(reverse=True)
print(mylist)

#reversed()函数,它返回一个逆序的迭代器，需要将其转换为列表才能查看结果，原列表不会被修改
my_list = [3, 1, 4, 1, 5, 9, 2, 6]
reversed_list = list(reversed(my_list))
print(reversed_list)
#列表的reverse函数
my_list.reverse()
```



### 614 最大值

![image-20250127134439988](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250127134439988.png)

> [!NOTE]
> $$
> max(a,b)=\frac{(a+b+abs(a-b))}{2}
> $$
> 对公式的解释：当a>=b时，max(a,b)=(a+b+(a-b))/2=a;当a<b时，max(a,b)=(a+b+(b-a))/2=b
>
> 举一反三：
> $$
> min(a,b)=\frac{(a+b-abs(a-b))}{2}
> $$

> [!NOTE]
>
> C++中的<algorithm>中有max()，min(),接受两个数比较大小

C++解法：

利用公式求解

```C++
#include <iostream>
#include <cmath>
using namespace std;
int main(){
    int a,b,c,max;
    cin>>a>>b>>c;
    max=(a+b+abs(a-b))/2;
    max=(max+c+abs(max-c))/2;
    cout<<max<<" eh o maior"<<endl;
}
/*之前写的复杂多写了一个变量
int main(){
    int a,b,c,temp,max;
    cin>>a>>b>>c;
    temp=(a+b+abs(a-b))/2;
    max=(temp+c+abs(temp-c))/2;
    cout<<max<<" eh o maior"<<endl;
}
*/
```

利用if else写

```C++
#include <iostream>
#include <cmath>
using namespace std;
int main(){
    int a,b,c,max;
    cin>>a>>b>>c;
    if(a>=b) max=a;
    else max=b;
    if(max>=c) max=max;
    else max=c;
    cout<<max<<" eh o maior"<<endl;
}
```

```C++
#include <iostream>
#include <algorithm>
using namespace std;
int main(){
    int a,b,c,maxnum;
    cin>>a>>b>>c;
    maxnum=max(a,b);
    maxnum=max(maxnum,c);
    cout<<maxnum<<" eh o maior"<<endl;
}//这里变量名不能用max，代码中定义了一个名为 max 的变量，而同时又使用了标准库中的 max 函数。这会导致编译器在解析 max(a, b) 和 max(max, c) 时产生混淆，因为它无法确定你是要调用函数还是使用变量，从而引发编译错误。
```

C++解法

> [!NOTE]
>
> 三目运算符
>
> ```C
> 条件表达式? 表达式1 : 表达式2;
> ```

自己写一个max的函数

```C
#include<stdio.h>

int max(int a,int b)
{
    return (a>=b)?a:b;
}

int main(){
    int a,b,c,maxvalue;
    scanf("%d %d %d\n",&a,&b,&c);
    maxvalue=max(a,b);
    maxvalue=max(maxvalue,c);
    printf("%d eh o maior",maxvalue);
}
```

python解法

```python
a,b,c=map(int,input().split())
print("%d eh o maior"%(max(a,b,c)))
```

或者写一个函数

```python
def max_two(a,b):
	return (a+b+abs(a-b))//2
```

### 617 距离

![image-20250127144647353](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250127144647353.png)

```C++
//我的做法：
#include <iostream>
using namespace std;

int main(){
    int L;
    cin>>L;
    cout<<int((L/30.0)*60)<<" minutos"<<endl;//使用 int() 进行强制类型转换，将浮点数结果转换为整数。
    //cout<<fixed<<setprecision(0)<<(L/30.0)*60<<" minutos"<<endl;
}
```

大佬的做法：

#### 二进制输出的一个牛逼操作

```C++
#include <iostream>
using namespace std;

int main(){
    int L;
    cin>>L;
    cout<<(L<<1)<<" minutos"<<endl;
}
```

**按题目要求输出×2的结果，而在二进制运算中，对于整数而言，左移一位相当于原数乘以 2 。**

**举一反三：右移一位相当于除以二**

C解法

```C
#include <stdio.h>

int main(){
    int L;
    scanf("%d",&L);
    printf("%d minutos",L<<1);//左移一位
}
```

python解法

```python
L=eval(input())
print(f"{L<<1} minutos")
```

### 618 燃料消耗

![image-20250127190212830](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250127190212830.png)

#### long long int应用

> [!NOTE]
>
> long long int 8位，在C中输入是%lld

C++题解

```C++
#include <iostream>
#include <iomanip>

using namespace std;

int main(){
    long long int T,S;//通常为8字节，这里是防止溢出
    cin>>T>>S;
    cout<<fixed<<setprecision(3)<<T*S/12.0<<endl;
}
```

C题解

```C
#include <stdio.h>

int main(){
    long long int T,S;
    scanf("%lld\n%lld\n",&T,&S);//
    printf("%.3f",T*S/12.0);
}
```

Python题解

```python
T=eval(input())
S=eval(input())
print(f"{T*S/12:.3f}")
```

### 656 钞票和硬币

![image-20250127203330299](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250127203330299.png)

> [!NOTE]
>
> 在C++中，double类型的变量在存储小数时可能会有精度损失。例如，2.01在二进制浮点数中可能无法精确表示，导致实际存储的值接近2.009999...，当乘以100后，结果可能是200.999...，转换为int时会向下取整为200，而不是预期的201。

我写的不太优雅＞﹏＜不过跑出来没问题(●'◡'●)

```C++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
    double total;
    int nums[6]={100,50,20,10,5,2};
    cin>>total;
    int num=int(total);
    int num2=total*100-num*100;
    cout<<"NOTAS:"<<endl;
    for(int i=0;i<6;i++)
    {
        cout<<num/nums[i]<<" nota(s) de R$ "<<fixed<<setprecision(2)<<nums[i]*1.00<<endl;
        num%=nums[i];
    }
    cout<<"MOEDAS:"<<endl;
    cout<<num<<" moeda(s) de R$ "<<fixed<<setprecision(2)<<1*1.00<<endl;
    int nums2[5]={50,25,10,5,1};
    for(int i=0;i<5;i++)
    {
        cout<<num2/nums2[i]<<" moeda(s) de R$ "<<fixed<<setprecision(2)<<0.01*nums2[i]<<endl;
        num2%=nums2[i];
    }
    
}
```

这题我的思路是把整数部分和小数部分割裂开来，小数部分*100，分别用两个for循环

比较优雅的做法是整个数*100，用一个for循环，在for循环里用一个if判断整数部分和小数部分输出的结果。

```C++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
    double total;
    int nums[12]={10000,5000,2000,1000,500,200,100,50,25,10,5,1};
    cin>>total;
    //这个代码有问题int num=int(total*100);
    int num=round(total*100);
    cout<<"NOTAS:"<<endl;
    for(int i=0;i<12;i++)
    {
        if(i<6)
        cout<<num/nums[i]<<" nota(s) de R$ "<<fixed<<setprecision(2)<<nums[i]/100.0<<endl;
        else if(i==6)
        {
            cout<<"MOEDAS:"<<endl;
            cout<<num/nums[i]<<" moeda(s) de R$ "<<fixed<<setprecision(2)<<nums[i]/100.0<<endl;
        }
        else{
            cout<<num/nums[i]<<" moeda(s) de R$ "<<fixed<<setprecision(2)<<nums[i]/100.0<<endl;
        }
        num%=nums[i];
    }
}
```

C和python是一样的做法。

别人写的比较简洁的

```C
#include <iostream>
using namespace std;
int main()
{
  double n;
  cin>>n;
  int m=(int)(n*100);
  int a[12]={10000,5000,2000,1000,500,200,100,50,25,10,5,1};
  printf("NOTAS:\n");
  for(int i=0;i<12;i++)
  {
      if(i<6){printf("%d nota(s) de R$ %.2f\n",m/a[i],(float)a[i]/100);m%=a[i];}
      if(i==6)printf("MOEDAS:\n");
      if(i>=6){printf("%d moeda(s) de R$ %.2f\n",m/a[i],(float)a[i]/100);m%=a[i];}
  }

}
```

deepseek不得不说太强了，豆包瞎说一通![image-20250128002132242](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250128002132242.png)

这个代码是存在一定的问题的，在double存储小数的时候存在精度损失，2.01存储实际是2.0099999，×100后强制转化为int类型是向下取整的所以这里可以include<cmath>,再转化为整数前round四舍五入

### 655 天数转换

![image-20250128113649475](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250128113649475.png)

```C++
#include <iostream>
using namespace std;

int main(){
    int total;
    cin>>total;
    cout<<total/365<<" ano(s)"<<endl;
    total=total%365;
    cout<<total/30<<" mes(es)"<<endl;
    cout<<total%30<<" dia(s)"<<endl;
}
```

## 判断语句

### 665 倍数

![image-20250128114550364](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250128114550364.png)

```C++
#include <iostream>
using namespace std;

int main(){
    int A,B;
    cin>>A>>B;
    if(A%B==0 ||B%A==0) cout<<"Sao Multiplos"<<endl;
    else cout<<"Nao sao Multiplos"<<endl;
}
```

```c
#include <stdio.h>

int main(){
    int A,B;
    scanf("%d %d",&A,&B);
    if(A%B==0 ||B%A==0)
    printf("Sao Multiplos");
    else
    printf("Nao sao Multiplos");
}
```

```python
A,B=map(int,input().split())
if A%B==0 or B%A==0:
    print("Sao Multiplos")
else:
    print("Nao sao Multiplos")
```

### 660 零食

![image-20250128121026627](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250128121026627.png)

C++解法

```C++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
    double price[5]={4.00,4.50,5.00,2.00,1.50};
    int label,num;
    cin>>label>>num;
    cout<<"Total: R$ "<<fixed<<setprecision(2)<<price[label-1]*num<<endl;
}
```

C解法

```c
#include <stdio.h>

int main(){
    double price[5]={4.00,4.50,5.00,2.00,1.50};
    int label,num;
    scanf("%d %d",&label,&num);
    printf("Total: R$ %.2f",price[label-1]*num);
}
```

python解法

```python
price=[4.00,4.50,5.00,2.00,1.50]
x,y=map(int,input().split())
print(f"Total: R$ {price[x-1]*y:.2f}")
```

感觉还是写复杂了

其实数组可以写补一个数，让编号正好对应

int price[6]={0,4.00,4.50,5.00,2.00,1.50}

### 659 区间

![image-20250128125816560](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250128125816560.png)

```C++
#include <iostream>
using namespace std;

int main(){
    double x;
    cin>>x;
    if(x<0 ||x>100) cout<<"Fora de intervalo";
    else if(x>=0 &&x<=25) cout<<"Intervalo [0,25]"<<endl;
    else if(x>25 &&x<=50) cout<<"Intervalo (25,50]"<<endl;
    else if(x>50 &&x<=75) cout<<"Intervalo (50,75]"<<endl;
    else cout<<"Intervalo (75,100]"<<endl;
}
```

```c
#include <stdio.h>

int main(){
    double x;
    scanf("%lf",&x);
    if(x<0 ||x>100) printf("Fora de intervalo");
    else if(x>=0 &&x<=25) printf("Intervalo [0,25]");
    else if(x>25 &&x<=50) printf("Intervalo (25,50]");
    else if(x>50 &&x<=75) printf("Intervalo (50,75]");
    else printf("Intervalo (75,100]");
}
```

```python
x=eval(input())
if x<0 or x>100: 
    print("Fora de intervalo")
elif 0<=x<=25:
    print("Intervalo [0,25]")
elif 25<x<=50:
    print("Intervalo (25,50]")
elif 50<x<=75: 
    print("Intervalo (50,75]")
elif 75<x<=100:
    print("Intervalo (75,100]")
```

### 664 三角形

```C++
#include <iostream>
#include <iomanip>

using namespace std;

int main(){
    double A,B,C;
    cin>>A>>B>>C;
    if(A+B>C &&A+C>B&&B+C>A) cout<<"Perimetro = "<<fixed<<setprecision(1)<<A+B+C<<endl;
    else cout<<fixed<<setprecision(1)<<"Area = "<<(A+B)*C*0.5<<endl;
}
```

### 667 游戏时间

![image-20250129125110221](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250129125110221.png)

```c++
#include <iostream>

using namespace std;

int main(){
    int A,B;
    cin>>A>>B;
    if(A==B) cout<<"O JOGO DUROU 24 HORA(S)"<<endl;
    else if(A>B) cout<<"O JOGO DUROU "<<B+24-A<<" HORA(S)"<<endl;
    else  cout<<"O JOGO DUROU "<<B-A<<" HORA(S)"<<endl;
}
```

更简洁的写法

```C++
int main(){
    int A,B;
    cin>>A>>B;
    if(A>=B) B+=24;
    cout<<"O JOGO DUROU "<<B-A<<" HORA(S)"<<endl;
}
```

再简单一点

```
int main(){
    int A,B;
    cin>>A>>B;
    cout<<"O JOGO DUROU "<<(B-A+24)%24<<" HORA(S)"<<endl;
}
```

```C++
int main(){
    int A,B,time;
    cin>>A>>B;
    cout<<"O JOGO DUROU "<<((B-A+24)%24?(B-A+24)%24:24)<<" HORA(S)"<<endl;
}
```



### 669 加薪

![image-20250129130810738](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250129130810738.png)

```C++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
    double salary;
    cin>>salary;
    if(salary>=0.00 &&salary<=400.00)
    {
        cout<<"Novo salario: "<<fixed<<setprecision(2)<<salary*1.15<<endl;
        cout<<"Reajuste ganho: "<<fixed<<setprecision(2)<<salary*0.15<<endl;
        cout<<"Em percentual: 15 %"<<endl;
    }
    else if(salary>=400.01 &&salary<=800.00)
    {
        cout<<"Novo salario: "<<fixed<<setprecision(2)<<salary*1.12<<endl;
        cout<<"Reajuste ganho: "<<fixed<<setprecision(2)<<salary*0.12<<endl;
        cout<<"Em percentual: 12 %"<<endl;
    }
    else if(salary>=800.01 &&salary<=1200.00)
    {
        cout<<"Novo salario: "<<fixed<<setprecision(2)<<salary*1.10<<endl;
        cout<<"Reajuste ganho: "<<fixed<<setprecision(2)<<salary*0.1<<endl;
        cout<<"Em percentual: 10 %"<<endl;
    }
    else if(salary>=1200.01 &&salary<=2000.00)
    {
        cout<<"Novo salario: "<<fixed<<setprecision(2)<<salary*1.07<<endl;
        cout<<"Reajuste ganho: "<<fixed<<setprecision(2)<<salary*0.07<<endl;
        cout<<"Em percentual: 7 %"<<endl;
    }
    else{
         cout<<"Novo salario: "<<fixed<<setprecision(2)<<salary*1.04<<endl;
        cout<<"Reajuste ganho: "<<fixed<<setprecision(2)<<salary*0.04<<endl;
        cout<<"Em percentual: 4 %"<<endl;
    }
}
```

这边做的是双边判断，可以只做单边判断，同时简化代码量

```C++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
    double salary,temp;
    cin>>salary;
    if(salary<=400.00) temp=0.15;
    else if(salary<=800.00) temp=0.12;
    else if(salary<=1200.00) temp=0.10;
    else if(salary<=2000.00) temp=0.07;
    else temp=0.04;
    cout<<"Novo salario: "<<fixed<<setprecision(2)<<salary*(1+temp)<<endl;
    cout<<"Reajuste ganho: "<<fixed<<setprecision(2)<<salary*temp<<endl;
    cout<<"Em percentual: "<<fixed<<setprecision(0)<<temp*100<<" %"<<endl;
}
```

### 670 动物

![image-20250129215553892](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250129215553892.png)

思路：看成一层层的去映射输出层的序号

> [!NOTE]
>
> 注意在定义变量的时候不能数字开头

```C++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string A,B,C;
    int idx1,idx2,idx3,output_idx;
    string first_layer[2]={"vertebrado","invertebrado"};//变量命名不能以数字开头
    string second_layer[4]={"ave","mamifero","inseto","anelideo"};
    string third_layer[8]={"carnivoro","onivoro","onivoro","herbivoro",
                        "hematofago","herbivoro","hematofago","onivoro"};
    string output_layer[8]={"aguia","pomba","homem","vaca","pulga","lagarta",
                            "sanguessuga","minhoca"};
    cin>>A>>B>>C;
    for(int i=0;i<2;i++)
    {
        if(A==first_layer[i])
            idx1=i;
    }
    for(int i=0;i<4;i++)
    {
        if(B==second_layer[i])
        {
            idx2=i;
            break;
        }
    }
    for(int i=idx2*2;i<8;i++)
    {
        if(C==third_layer[i])
        {
            idx3=i;
            break;
        }
    }
    
    if(idx1==0 && idx2==0) output_idx=(idx3==0)?0:1;
    if(idx1==0 && idx2==1) output_idx=(idx3==2)?2:3;
    if(idx1==1 && idx2==2) output_idx=(idx3==4)?4:5;
    if(idx1==1 && idx2==3) output_idx=(idx3==6)?6:7;
    cout<<output_layer[output_idx]<<endl;
    
}
```

暴力解决法

```C++
#include <iostream>
#include <string>

using namespace std;

int main(){
    string A,B,C;
    cin>>A>>B>>C;
    if(A=="vertebrado"&&B=="ave"&&C=="carnivoro") cout<<"aguia"<<endl;
    if(A=="vertebrado"&&B=="ave"&&C=="onivoro") cout<<"pomba"<<endl;
    if(A=="vertebrado"&&B=="mamifero"&&C=="onivoro") cout<<"homem"<<endl;
    if(A=="vertebrado"&&B=="mamifero"&&C=="herbivoro") cout<<"vaca"<<endl;
    if(A=="invertebrado"&&B=="inseto"&&C=="hematofago") cout<<"pulga"<<endl;
    if(A=="invertebrado"&&B=="inseto"&&C=="herbivoro") cout<<"lagarta"<<endl;
    if(A=="invertebrado"&&B=="anelideo"&&C=="hematofago") cout<<"sanguessuga"<<endl;
    if(A=="invertebrado"&&B=="anelideo"&&C=="onivoro") cout<<"minhoca"<<endl;
}
```

题解中有个方法是根据三个输入字符的首字母判断

> [!NOTE]
>
> 逗号是作用于for循环内的，每次for循环都都会执行逗号前后的语句。如果没有逗号且省略了大括号，你只可以写一条循环语句。但是有了逗号以后，你就可以省略了大括号，并且写多条的循环语句

![image-20250129220233713](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250129220233713.png)

```C++
string a[3];
string ss;
for(i=0;i<3;i++)
{
	cin>>a[i];
	ss+=a[i][0];
}
```

deepseek提出的改进：

```C++
for(int i=0; i<3; i++) {
    cin >> a[i];
    if (!a[i].empty()) {
        ss += a[i][0];
    } else {
        ss += '?';  // 空输入时用占位符替代
    }
}
```

python的一个字符串操作：

```python
a=input()
b=input()
c=input()
ss=a[0]+b[0]+c[0]
```

### 657 选择练习1

![image-20250129221343414](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250129221343414.png)

```C++
#include <iostream>
using namespace std;

int main(){
    int A,B,C,D;
    cin>>A>>B>>C>>D;
    //if(B>C&&D>A&&C+D>A+B&&C*D>0&&A%2==0)//这么写是有问题的
    if(B>C&&D>A&&C+D>A+B&&C>0&&D>0&&A%2==0)
    cout<<"Valores aceitos"<<endl;
    else cout<<"Valores nao aceitos"<<endl;
}
```

### 671 DDD

![image-20250129224122465](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250129224122465.png)

建立一个序号和城市一一对应的关系

```C++
#include <iostream>
#include <string>
using namespace std;

int main(){
    int label,idx;
    int nums[8]={61,71,11,21,32,19,27,31};
    string city[9]={"Brasilia","Salvador","Sao Paulo","Rio de Janeiro","Juiz de Fora",
    "Campinas","Vitoria","Belo Horizonte","DDD nao cadastrado"};
    cin>>label;
    for(int i=0;i<8;i++)
    {
        if(nums[i]==label)
        {
            idx=i;
            break;
        }
        else
        {   
            idx=8;
        }
    }
    cout<<city[idx]<<endl;
}
```

### 662 点的坐标

![image-20250129224215727](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250129224215727.png)

```C++
#include <iostream>
 
using namespace std;

int main(){
    double x,y;
    cin>>x>>y;
    if(x==0&&y!=0) cout<<"Eixo Y"<<endl;
    if(x!=0&&y==0) cout<<"Eixo X"<<endl;
    if(x==0&&y==0) cout<<"Origem"<<endl;
    if(x>0)
    {
        if(y>0) cout<<"Q1"<<endl;
        if(y<0) cout<<"Q4"<<endl;
    }
    if(x<0)
    {
        if(y>0) cout<<"Q2"<<endl;
        if(y<0) cout<<"Q3"<<endl;
    }
}
```

更优雅的写法使用三目运算符

```C++
#include <iostream>
 
using namespace std;

int main(){
    double x,y;
    cin>>x>>y;
    if(x==0&&y==0) cout<<"Origem"<<endl;
    else if(x*y==0) cout<<(x==0?"Eixo Y":"Eixo X");
    else if(x*y>0) cout<<(x>0?"Q1":"Q3");
    else cout<<(x<0?"Q2":"Q4");
    
}
```

### 666 三角形类型

![image-20250130200753735](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250130200753735.png)

```C++
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

template <typename T>
void bubblesort(T arr[],int n)
{
    bool swapped;
    for(int i=0;i<n;i++){
        swapped=false;
        for(int j=0;j<n-1-i;j++){
            if(arr[j]<arr[j+1]){
                swap(arr[j],arr[j+1]);
                swapped=true;
            }
        }
        if(!swapped) break;
    }
}

int main(){
    double A,B,C;
    double nums[3];
    for(int i=0;i<3;i++) cin>>nums[i];
    bubblesort(nums,3);
    A=nums[0];
    B=nums[1];
    C=nums[2];
    if(A>=B+C) cout<<"NAO FORMA TRIANGULO"<<endl;
    else if(pow(A,2)==pow(B,2)+pow(C,2)) cout<<"TRIANGULO RETANGULO"<<endl;
    else if(pow(A,2)>pow(B,2)+pow(C,2)) cout<<"TRIANGULO OBTUSANGULO"<<endl;
    else if(pow(A,2)<pow(B,2)+pow(C,2)) cout<<"TRIANGULO ACUTANGULO"<<endl;
    if(!(A>=B+C))
    {
        if(A==B && B==C) cout<<"TRIANGULO EQUILATERO"<<endl;
        else if(A==B || B==C) cout<<"TRIANGULO ISOSCELES"<<endl;
    }

}
```

优化了一下主函数部分

```C++
int main(){
    double A,B,C;
    double nums[3];
    for(int i=0;i<3;i++) cin>>nums[i];
    bubblesort(nums,3);
    A=nums[0];
    B=nums[1];
    C=nums[2];
    if(A>=B+C) cout<<"NAO FORMA TRIANGULO"<<endl;
    else{
        if(pow(A,2)==pow(B,2)+pow(C,2)) cout<<"TRIANGULO RETANGULO"<<endl;
        if(pow(A,2)>pow(B,2)+pow(C,2))  cout<<"TRIANGULO OBTUSANGULO"<<endl;
        if(pow(A,2)<pow(B,2)+pow(C,2)) cout<<"TRIANGULO ACUTANGULO"<<endl;
        if(A==B && B==C) cout<<"TRIANGULO EQUILATERO"<<endl;
        else if(A==B || B==C) cout<<"TRIANGULO ISOSCELES"<<endl;
    }
}
```

更简单的方法

```C++
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

int main(){
    double A,B,C;
    cin>>A>>B>>C;
    if(A<B) swap(A,B);
    if(B<C) swap(B,C);
    if(A<B) swap(A,B);//这个其实就是冒泡排序的思想
    
    if(A>=B+C) cout<<"NAO FORMA TRIANGULO"<<endl;
    else{
        if(pow(A,2)==pow(B,2)+pow(C,2)) cout<<"TRIANGULO RETANGULO"<<endl;
        if(pow(A,2)>pow(B,2)+pow(C,2))  cout<<"TRIANGULO OBTUSANGULO"<<endl;
        if(pow(A,2)<pow(B,2)+pow(C,2)) cout<<"TRIANGULO ACUTANGULO"<<endl;
        if(A==B && B==C) cout<<"TRIANGULO EQUILATERO"<<endl;
        else if(A==B || B==C) cout<<"TRIANGULO ISOSCELES"<<endl;
    }
}
```

## 2.1-2.7

### 668 游戏时间2（计算时间差）（重要）

计算时间差

```C++
#include <iostream>

using namespace std;

int main(){
    int x1,y1,x2,y2;
    cin>>x1>>y1>>x2>>y2;
    if(x1==x2 &&y1==y2) cout<<"O JOGO DUROU 24 HORA(S) E 0 MINUTO(S)"<<endl;
    else if(x1<x2 && y1==y2) cout<<"O JOGO DUROU "<<x2-x1<<" HORA(S) E 0 MINUTO(S)"<<endl;
    else if(x2>x1 && y2==y1) cout<<"O JOGO DUROU "<<x2-x1+24<<" HORA(S) E 0 MINUTO(S)"<<endl;
    else if(x1<=x2 && y1<y2) cout<<"O JOGO DUROU "<<x2-x1<<" HORA(S) E "<<y2-y1<<" MINUTO(S)"<<endl;
    else if(x1<x2 && y1>y2)cout<<"O JOGO DUROU "<<x2-x1-1<<" HORA(S) E "<<y2+60-y1<<" MINUTO(S)"<<endl;
    else if(x1>=x2 &&y1<y2) cout<<"O JOGO DUROU "<<x2-x1+24<<" HORA(S) E "<<y2-y1<<" MINUTO(S)"<<endl;
    else if(x1>=x2 &&y1>y2) cout<<"O JOGO DUROU "<<x2-x1+23<<" HORA(S) E "<<y2-y1+60<<" MINUTO(S)"<<endl;
}
```

最主要还是看下面这个代码，将时刻都转化为分钟，用分钟来计算时间差

如果差大于0，即为时间差

如果差小于0，说明进行到了隔天，加上24*60就是时间差

这里进行一个统一都加上24*60,在对其取余，都能得到时间差就不用分情况了

```C++
#include <iostream>

using namespace std;

int main(){
    int x1,y1,x2,y2,time,hour,min;
    cin>>x1>>y1>>x2>>y2;
    y1+=x1*60;
    y2+=x2*60;
    time=(y2-y1+24*60)%(24*60);//计算时间差，单位:min
    //这里一定要注意运算的优先级
    //cout<<time<<endl;
    if(time==0)//考虑时间差为0的情况
    {
        hour=24;
        min=0;
    }else{
        hour=time/60;
        min=time%60;
    }

    cout<<"O JOGO DUROU "<<hour<<" HORA(S) E "<<min<<" MINUTO(S)"<<endl;
}
```

其实上面这个是方便阅读代码实际可以简化，如下

```C++
int x1,y1,x2,y2;
cin>>x1>>y1>>x2>>y2;
y1+=x1*60;
y2+=x2*60;
x1=(y2-y1+24*60)%(24*60);//计算时间差，单位:min
x1=(x1?x1:24+24*60);
cout<<"O JOGO DUROU "<<x1/60<<" HORA(S) E "<<x1%60<<" MINUTO(S)"<<endl;
```

### 672 税

![image-20250201224053853](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250201224053853.png)

```C++
#include <iostream>
#include <iomanip>

using namespace std;

int main(){
    double salary;
    cin>>salary;
    cout.setf(ios::fixed);
    cout<<setprecision(2);
    if(salary<=2000.00) cout<<"Isento"<<endl;
    else if(salary<=3000.00) cout<<"R$ "<<(salary-2000)*0.08<<endl;
    else if(salary<=4500.00) cout<<"R$ "<<1000*0.08+(salary-3000)*0.18<<endl;
    else cout<<"R$ "<<1000*0.08+1500*0.18+(salary-4500)*0.28<<endl;
}
```

### 663 简单排序

<img src="C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250204175437187.png" alt="image-20250204175437187" style="zoom:50%;" />

```C++
#include <iostream>
#include <algorithm>

using namespace std;
void mysort(int arr[3])
{
    if(arr[0]>arr[1]) swap(arr[0],arr[1]);
    if(arr[1]>arr[2]) swap(arr[1],arr[2]);
    if(arr[0]>arr[1]) swap(arr[0],arr[1]);
}


int main(){
    int nums[3],copy_nums[3];
    for(int i=0;i<3;i++) cin>>nums[i];
    for(int i=0;i<3;i++) copy_nums[i]=nums[i];
    mysort(nums);
    for(int i=0;i<3;i++) cout<<nums[i]<<endl;
    cout<<""<<endl;
    for(int i=0;i<3;i++) cout<<copy_nums[i]<<endl;
}
```

```C++
#include <iostream>
#include <algorithm>

using namespace std;

int main(){
    int A,B,C,a,b,sum;
    cin>>A>>B>>C;
    sum=A+B+C;
    a=min(A,min(C,B));
    b=max(A,max(B,C));
    cout<<a<<endl;
    cout<<sum-a-b<<endl;
    cout<<b<<endl;
    cout<<endl;
    cout<<A<<"\n"<<B<<"\n"<<C<<endl;
}
```

[C++四种解法](https://www.acwing.com/solution/content/82724/)

输出的一种写法

```C++
cout<<a[1]<<"\n"<<a[2]<<"\n"<<a[3]<<"\n\n"<<b[1]<<"\n"<<b[2]<<"\n"<<b[3];
```

### 658  一元二次方程公式

**![image-20250204181938825](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250204181938825.png)**

```C++
#include <iostream>
#include <cmath>
#include <iomanip>
using namespace std;
int main(){
    double a,b,c;
    cin>>a>>b>>c;
    cout.setf(ios::fixed);
    cout<<setprecision(5);
    if(pow(b,2)-4*a*c<0 ||a==0) cout<<"Impossivel calcular"<<endl;
    else{
        cout<<"R1 = "<<(sqrt(pow(b,2)-4*a*c)-b)/(2.0*a)<<endl;
        cout<<"R2 = "<<(-sqrt(pow(b,2)-4*a*c)-b)/(2.0*a)<<endl;
    }
    
}
```

### 有关动态数组的题目

#### 求和

```C++
#include <iostream>
#include <vector>
using namespace std;

int main(){
    vector<int> nums={2,5,7,9,18};
    int sum=0;
    for(int num:nums) sum+=num;
    cout<<sum<<endl;
}
```

#### 找最大值

```C++
#include <iostream>
#include <vector>
using namespace std;
int main(){
    vector<int> nums={3,5,4,7,8};
    int max=0;//一定要初始化
    for(vector<int>::iterator it=nums.begin();it!=nums.end();++it)
    {
        if(max<*it) max=*it;
    }
    cout<<max<<endl;
}
```

#### 删除目标元素

```C++
#include <iostream>
#include <algorithm>
#include <vector>
#include <sstream>
#include <string>

int main(){
    std::vector<int> v1;
    std::string line;
    std::cout<<"Please enter the wanted numbers"<<std::endl;
    //读取一整行，包括空格，遇换行符结束
    std::getline(std::cin,line);
    //使用 istringstream 将字符串流化，方便按空格拆分
    std::istringstream iss(line);
    //从输入中获取数组数据
    int num,target;
    while(iss>>num){
        v1.push_back(num);
    }
    std::cout<<"Please enter the target number"<<std::endl;
    std::cin>>target;
    //erase-remove法
    v1.erase(std::remove(v1.begin(), v1.end(), target), v1.end());
    for(int num:v1)
    {
        std::cout<<num<<std::endl;
    }

}
```

remove是算法库里的，是将目标移到最后，比如[3,5,6,7],目标为5，remove后的数组就是[3,6,7,5],再结合erase就可以删除，remove返回的是一个迭代器

#### 合并两个有序数组

```C++
#include <iostream>
#include <vector>
using namespace std;

vector<int>  mergeSortedArrays(vector<int>& v1,vector<int>& v2){//这里注意用引用
    int i=0,j=0;//这里一定要初始化
    vector<int> result;
    while(i<v1.size()&&j<v2.size()){
        if(v1[i]<v2[j]) result.push_back(v1[i++]);
        else result.push_back(v2[j++]);
    }
    while(i<v1.size()){
        result.push_back(v1[i++]);
    }
    while(j<v2.size())
    {
        result.push_back(v2[j++]);
    }
    return result;//一定要返回值
}

int main(){
    vector<int> v1={4,7,9};
    vector<int> v2={6,8,11,15,18};
    vector<int> result=mergeSortedArrays(v1,v2);
    for(int num:result){
        cout<<num<<endl;
    }

}
```

可以把这个函数传入的引用改成指针

vector和传统的数组不一样（传统的数组名会退化成指针，指向数组的第一个元素）

 **获取 `std::vector` 元素的指针**

为了获取 `std::vector` 中元素的指针，可以使用 `std::vector` 的 `data()` 成员函数。`data()` 函数返回一个指向 `std::vector` 内部存储的首元素的指针。

```C++
#include <iostream>
#include <vector>

std::vector<int> mergeSortedArrays(int* v1, int size1, int* v2, int size2) {
//这里要传入两种参数一个是数组的指针,一个是数组的大小
    std::vector<int> result;
    int i = 0, j = 0;//这里一定要初始化因为是局部变量

    // 遍历两个数组，比较指针所指向的元素大小
    while (i < size1 && j < size2) {
        if (*(v1 + i) < *(v2 + j)) {
            result.push_back(*(v1 + i));
            i++;
        } else {
            result.push_back(*(v2 + j));
            j++;
        }
    }
    while (i < size1) {
        result.push_back(*(v1 + i));
        i++;
    }
    while (j < size2) {
        result.push_back(*(v2 + j));
        j++;
    }

    return result;
}

int main() {
    std::vector<int> v1 = {1, 3, 5};
    std::vector<int> v2 = {2, 4, 6};

    // 获取向量的指针和大小
    int* ptr1 = v1.data();
    int size1 = v1.size();
    int* ptr2 = v2.data();
    int size2 = v2.size();

    std::vector<int> merged = mergeSortedArrays(ptr1, size1, ptr2, size2);
    for (int num : merged) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

#### 数组元素反转

**反向迭代**

```C++
#include<iostream>
#include <vector>
//反向迭代
std::vector<int> reverseArray(const std::vector<int>& arr) {
    std::vector<int> reversed;
    for(auto it=arr.rbegin();it!=arr.rend();++it)
    {
        reversed.push_back(*it);
    }
    return reversed;
}

int main() {
    std::vector<int> arr = {1, 2, 3, 4, 5};
    std::vector<int> reversed = reverseArray(arr);
    for (int num : reversed) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    return 0;
}

```

#### 删去重复的数

**`std::unordered_set`**

`unordered_set` 是 C++ 标准库中的一个**集合**容器，它使用哈希表来存储数据，因此查找、插入和删除操作的时间复杂度平均是 O(1)。它的特点是：

- 存储的元素是唯一的，不能重复。
- 不保证元素的顺序。

**常用方法：**

- **`find()`**：查找元素，返回一个指向该元素的迭代器（如果元素存在），或返回 `end()`（如果元素不存在）。
- **`insert()`**：插入元素。如果元素已存在，则插入失败，返回一个标记插入是否成功的值。

```C++
/*编写一个函数，接收一个 std::vector<int> 类型的动态数组，
去除数组中的重复元素，返回一个没有重复元素的新数组，且元素顺序保持不变。*/
#include <iostream>
#include <vector>
#include <unordered_set>

std::vector<int> removeDuplicates(const std::vector<int>& arr) {
    std::unordered_set<int> seen;
    std::vector<int> result;
    for(int num:arr){
        if(seen.find(num)==seen.end()){
            result.push_back(num);
            seen.insert(num);
        }
    }
	return result;
}

int main() {
    std::vector<int> arr = {1, 2, 2, 3, 4, 4, 4, 5};
    std::vector<int> result = removeDuplicates(arr);
    for (int num : result) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    return 0;
}
```

#### 合并多个数组

```C++
/*
题目描述：编写一个函数，接收一个 std::vector<std::vector<int>> 类型的二维动态数组，其中每个一维数组都是有序的，将这些有序数组合并成一个大的有序数组。
示例：
输入：[[1, 4, 7], [2, 5, 8], [3, 6, 9]]
输出：[1, 2, 3, 4, 5, 6, 7, 8, 9]
*/
#include <iostream>
#include <vector>
#include <queue>

struct Compare {
    bool operator()(const std::pair<int, std::pair<int, int>>& a, const std::pair<int, std::pair<int, int>>& b) {
        return a.first > b.first;
    }
};

std::vector<int> mergeSortedArrays(const std::vector<std::vector<int>>& arrays) {
    std::priority_queue<std::pair<int, std::pair<int, int>>, std::vector<std::pair<int, std::pair<int, int>>>, Compare> minHeap;
    std::vector<int> merged;

    // 初始化最小堆
    for (int i = 0; i < arrays.size(); ++i) {
        if (!arrays[i].empty()) {
            minHeap.push({arrays[i][0], {i, 0}});
        }
    }

    // 从最小堆中取出元素并合并
    while (!minHeap.empty()) {
        auto top = minHeap.top();
        minHeap.pop();
        int val = top.first;
        int arrayIndex = top.second.first;
        int elementIndex = top.second.second;

        merged.push_back(val);

        if (elementIndex + 1 < arrays[arrayIndex].size()) {
            minHeap.push({arrays[arrayIndex][elementIndex + 1], {arrayIndex, elementIndex + 1}});
        }
    }

    return merged;
}

int main() {
    std::vector<std::vector<int>> arrays = `{{1, 4, 7}, {2, 5, 8}, {3, 6, 9}}`;
    std::vector<int> merged = mergeSortedArrays(arrays);
    for (int num : merged) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    return 0;
}
```



### 初始化的问题

全局变量和静态变量（包括在函数内部用`static`关键字声明的变量）如果在主函数外面（即全局作用域）没有显式初始化为 0，它们会被自动初始化为 0；而局部变量如果未显式初始化，其值是未定义的。

### 重载

#### 函数重载

```C++
#include <iostream>

// 计算两个整数的和
int add(int a, int b) {
    return a + b;
}

// 计算两个双精度浮点数的和
double add(double a, double b) {
    return a + b;
}

// 计算三个整数的和
int add(int a, int b, int c) {
    return a + b + c;
}

int main() {
    int result1 = add(1, 2);
    double result2 = add(1.5, 2.5);
    int result3 = add(1, 2, 3);

    std::cout << "两个整数的和: " << result1 << std::endl;
    std::cout << "两个双精度浮点数的和: " << result2 << std::endl;
    std::cout << "三个整数的和: " << result3 << std::endl;

    return 0;
}
```

一般都是根据参数来选取函数

#### 运算符重载

### \<algorithm>

#### \<algorithm>中相关用法

##### sort

sort函数中的原型(默认是升序)

```C++
// 对 [first, last) 范围内的元素进行升序排序，使用元素类型的 < 运算符进行比较
template< class RandomIt >
void sort( RandomIt first, RandomIt last );

// 对 [first, last) 范围内的元素进行排序，使用自定义的比较函数 comp 来确定元素顺序
template< class RandomIt, class Compare >
void sort( RandomIt first, RandomIt last, Compare comp );
```

sort函数用法示例

```C++
int arr[]={3,4,6,8,10};
int size=sizeof(arr)/sizeof(arr[0]);
std::sort(arr,arr+size);
```

```C++
std::vector<int> vec={3,4,6,8,10}；
std::sort(vec.begin(),vec.end());
```

```C++
// 使用自定义比较函数进行降序排序
std::sort(vec.begin(), vec.end(), [](int a, int b) {
    return a > b;
 });
```

##### max()、min()

```C++
maxnum=max(a,max(b,c))
minnum=min(a,min(b,c))
```

##### find()

```C++
auto it =std::find(vec.begin(),vec.end(),2)//find返回的是一个迭代器
if(it!=vec.end())
	std::cout<<"Found 2"<<std::endl;
```

- 如果在 `vec` 中找到了值为 `2` 的元素，`std::find` 会返回指向该元素的迭代器；如果没有找到，它会返回 `vec.end()`，也就是范围结束的下一个位置的迭代器。

##### `auto`关键字

 是一个类型推导关键字。它的主要作用是让编译器根据初始化表达式自动推断变量的类型

```C++
auto a=10;//编译器根据10将a推断为int类型
auto str="hello"//const char*
```

##### accumulate()

```C++
int sum = std::accumulate(vec.begin(), vec.end(), 0);//初始值累加值
std::cout << "Sum: " << sum << std::endl;  
```

##### reverse()

```C++
std::reverse(vec.begin(), vec.end());
```

##### max_element()

```C++
auto maxElem = std::max_element(vec.begin(), vec.end());
```

##### 容器map

（类似python中的键值对）

```C++
std::map<int, std::string> m;
m[1] = "one";
m[2] = "two";
```

### \<string>

`<string>` 是 C++ 标准库中非常重要的一个头文件，它提供了对字符串（`std::string` 类型）进行各种操作的功能。`std::string` 是一个动态大小的字符数组，它能处理字符串的存储、操作和管理，并且比 C 风格字符串（`char[]`）更加灵活和方便。

####  **常用的字符串操作方法**

#### **构造与初始化**

- **默认构造：**

  ```cpp
  std::string s;  // 空字符串
  ```

- **通过字符串字面量初始化：**

  ```cpp
  std::string s = "Hello";
  ```

- **通过另一个 `std::string` 初始化：**

  ```cpp
  std::string s1 = "Hello";
  std::string s2 = s1;  // s2 是 s1 的副本
  ```

- **通过字符和长度初始化：**

  ```cpp
  std::string s(5, 'a');  // s = "aaaaa"
  ```

#### **大小与容量**

- **`size()` / `length()`：** 返回字符串中字符的数量（相同功能，可以互换使用）。

  ```cpp
  std::string s = "Hello";
  std::cout << s.size();  // 输出 5
  std::cout << s.length();  // 输出 5
  ```

- **`empty()`：** 判断字符串是否为空。

  ```cpp
  std::string s = "";
  std::cout << s.empty();  // 输出 true
  ```

- **`capacity()`：** 返回字符串分配的内存容量（大于等于字符串的大小），但不一定等于当前字符串的长度。

  ```cpp
  std::string s = "Hello";
  std::cout << s.capacity();  // 输出 15（可能根据实现不同会有所变化）
  ```

#### **修改字符串**

- **`append()`**： 向字符串的末尾添加内容。

  ```cpp
  std::string s = "Hello";
  s.append(" World");
  std::cout << s;  // 输出 "Hello World"
  ```

- **`push_back()`**： 在字符串末尾添加一个字符。

  ```cpp
  std::string s = "Hello";
  s.push_back('!');
  std::cout << s;  // 输出 "Hello!"
  ```

- **`insert()`**： 在指定位置插入字符串或字符。

  ```cpp
  std::string s = "Hello";
  s.insert(5, " World");
  std::cout << s;  // 输出 "Hello World"
  ```

- **`replace()`**： 替换字符串中的一部分内容。

  ```cpp
  std::string s = "Hello World";
  s.replace(6, 5, "Universe");
  std::cout << s;  // 输出 "Hello Universe"
  ```

- **`erase()`**： 删除指定位置或范围的字符。

  ```cpp
  std::string s = "Hello World";
  s.erase(5, 6);  // 删除从索引 5 开始的 6 个字符
  std::cout << s;  // 输出 "Hello"
  ```

#### **查找与查找替换**

- **`find()`**： 查找子字符串的第一次出现，返回该子字符串的起始位置，如果未找到返回 `std::string::npos`。

  ```cpp
  std::string s = "Hello World";
  size_t pos = s.find("World");
  std::cout << pos;  // 输出 6（"World" 从索引 6 开始）
  ```

- **`rfind()`**： 从右向左查找子字符串的第一次出现。

  ```cpp
  std::string s = "Hello World, Hello Universe";
  size_t pos = s.rfind("Hello");
  std::cout << pos;  // 输出 13（从右边第一个 "Hello" 的位置）
  ```

- **`substr()`**： 获取子字符串，给定起始位置和长度。

  ```cpp
  std::string s = "Hello World";
  std::string sub = s.substr(6, 5);  // 从索引 6 开始，长度为 5
  std::cout << sub;  // 输出 "World"
  ```

#### **比较字符串**

- **`compare()`**： 比较两个字符串。返回值：0 表示相等，负值表示当前字符串小，正值表示当前字符串大。

  ```cpp
  std::string s1 = "Hello";
  std::string s2 = "World";
  std::cout << s1.compare(s2);  // 输出负值，因为 "Hello" 小于 "World"
  ```

- **`==`、`!=`、`<`、`<=` 等运算符**： 可以直接使用这些运算符来比较两个字符串。

  ```cpp
  std::string s1 = "Hello";
  std::string s2 = "World";
  if (s1 == s2) {
      std::cout << "Equal";
  } else {
      std::cout << "Not equal";  // 输出 "Not equal"
  }
  ```

#### **转换操作**

- **`c_str()`**： 获取 C 风格字符串的指针（以 `\0` 结尾的字符数组）。

  ```cpp
  std::string s = "Hello";
  const char* cstr = s.c_str();
  std::cout << cstr;  // 输出 "Hello"
  ```

- **`stoi()` / `stof()` / `stod()` 等**： 将字符串转换为数字类型（`int`、`float`、`double`）。

  ```cpp
  std::string s = "12345";
  int num = std::stoi(s);
  std::cout << num;  // 输出 12345
  ```

------

####  **题目练习**

1. **题目 1：字符串反转** 编写一个函数，接受一个字符串，并将其反转。不要使用现成的库函数，只能使用 `std::string` 的成员函数。

```

```



1. **题目 2：查找子串位置** 给定一个字符串 `str` 和子串 `sub`，编写一个函数，返回 `sub` 在 `str` 中第一次出现的位置，如果没有找到返回 -1。
2. **题目 3：统计字符出现次数** 给定一个字符串，统计每个字符出现的次数，并输出字符及其出现次数（字母顺序）。
3. **题目 4：移除所有空格** 给定一个字符串，移除其中所有的空格字符，输出新的字符串。
4. **题目 5：比较字符串大小** 编写一个函数，接受两个字符串，返回它们的大小关系（`<`、`>` 或 `=`）。实现一个自定义的字符串比较，而不使用运算符直接比较。

------


