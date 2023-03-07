# 设计模式


<!--more-->

## Day6 C语言

### 输入输出画家音乐家和

#### -getchar()\putchar()

>  getchar()会接受一切字符，包括空格、回车

#### printf()\scanf()

> %d 整形
>
> %c 字符
>
> %f 单精度
>
> %d 双精度
>
> %p 地址
>
> %s 字符串
>
> --- scanf()
>
> * 解决垃圾字符
>* 抑制符 指定输入项后的内容不输入![1677466757754](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/1677466757754.png)
> 
> 2. 用getchar()处理
> 3. 在连续输入字符的中间加上空格
>
> * 遇到空格，就相当于另外一个字符串

#### gets()\puts()

> gets()
>
> * 功能：从键盘输入一个指定的字符串
>
> * gets(<font style="color:red">输入字符串的首地址</font>)
>
> * ```c
>   char a[10]="HELLO";
>   gets(a);
>   ```
> ```
> 
> ```
>
> * ## <font style="color:#922999">要注意栈溢出</font>
>
> puts()
>
> * 功能：向终端输出一个指定的字符串
>
> * puts(<font style="color:red">输出内容首地址</font>);
>
> * ```c
>   char a[10]="HELLO";
>   puts(a);
>   ```
> ```
> 
> * ##### 自带换行 不用加"\n"
> 
> 
> ```

> 获取随机的时间种子
>
> ```c
> srand((unsigned)time(NULL));
> rand();
> ```
>
> 

##  Day6 Linux

> man命令（手册）
>
> ![1677485741404](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/1677485741404.png)
>
> ![1677485724006](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/1677485724006.png)

## Day7  C语言

#### 数组初始化

>* 完全初始化
>  * int a[5]={1,2,3,4,5}; 
>* 不完全初始化，其余初始化的元素默认为0
>  * int b[5]={1,2,3};
>* 不指定元素个数，根据给定的来分配
>  * int c[]={1,2,3,4,5}

寻找素数的方法---https://zhuanlan.zhihu.com/p/104314640

```c
方法3：普通筛法——埃拉托斯特尼(Eratosthenes)筛法
思路:
我们的想法是，创建一个比范围上限大1的数组，我们只关注下标为 1 ~ N（要求的上限） 的数组元素与数组下标（一一对应）。
将数组初始化为1。然后用for循环，遍历范围为：【2 ~ sqrt(N)】。如果数组元素为1，则说明这个数组元素的下标所对应的数是素数。
随后我们将这个下标（除1以外）的整数倍所对应的数组元素全部置为0，也就是判断其为非素数。
这样，我们就知道了范围内（1 ~ 范围上限N）所有数是素数（下标对应的数组元素值为1）或不是素数（下标对应的数组元素值为0）

用百度百科对埃拉托斯特尼筛法简单描述：要得到自然数n以内的全部素数，必须把不大于 的所有素数的倍数剔除，剩下的就是素数。
核心代码：
//                 判断素数的数组    范围上限N
void Eratprime(int* isprime, int upper_board) {

    int i = 0;
    int j = 0;
    //初始化isprime
    for (i = 2; i <= upper_board; i++)
        isprime[i] = 1;


    for (i = 2; i < (int)sqrt(upper_board); i++) {
        if (isprime[i]) {
            isprime[i] = 1;
        }
        for (j = 2; i * j <= upper_board; j++) {//素数的n倍（n >= 2）不是素数
            isprime[i * j] = 0;
        }
    }

}
方法4：线性筛法——欧拉筛法
思路:
我们再思考一下上面的埃拉托斯特尼筛法，会发现，在“剔除“非素数时，有些合数会重复赋值。这样就增加里复杂度，降低了效率。
比如：范围上限N = 16时
2是素数，剔除”2 的倍数“，他们是：4，6， 8，10， 12， 14， 16
3是素数，剔除”3 的倍数”，他们时，6，9，12，15
6，12是重复的。如何减少重复呢？

核心代码：

void PrimeList(int* Prime, bool* isPrime, int n) {

    int i = 0;
    int j = 0;
    int count = 0;

    if (isPrime != NULL) {//确保isPrime不是空指针
        //将isPrime数组初始化为 1
        for (i = 2; i <= N; i++) {
            isPrime[i] = true;
        }
    }
    if (isPrime != NULL && Prime != NULL) {
        //从2遍历到范围上限N
        for (i = 2; i <= N; i++) {
            if (isPrime[i])//如果下标（下标对应着1 ~ 范围上限N）对应的isPrime值没有被置为false，说明这个数是素数，将下标放入素数数组
                Prime[count++] = i;
            //循环控制表达式的意义：j小于等于素数数组的个数 或 素数数组中的每一个素数与 i 的积小于范围上限N
            for (j = 0; (j < count) && (Prime[j] * (long long)i) <= N; j++)//将i强制转换是因为vs上有warning，要求转换为宽类型防止算术溢出。数据上不产生影响
            {
                isPrime[i * Prime[j]] = false;//每一个素数的 i 倍（i >= 2）都不是素数，置为false

                //这个是欧拉筛法的核心，它可以减少非素数置false的重复率
                //意义是将每一个合数（非素数）拆成 2（最小因数）与最大因数 的乘积
                if (i % Prime[j] == 0)
                    break;
            }
        }
    }
}
```

## Day8 C语言

>/x 表示16进制
>
>```c
>printf("%c\n","/x69");
>output---> i
>```
>
>结果为i ->the reason is 十六进制 69 对应的十进制为105，从ASCLL表中可以查到105对于为i

#### fflush(stdout);

##### 刷新缓冲区，![image-20230301095123749](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/image-20230301095123749.png)

如果不加fflush(),则程序不会输出，需要等程序结束后才会输出。原因是以为缓冲区。

>\r表示光标移动到改行的首位
>
>\n表示移动到下一行
>
>写程序时通常只写\n，程序回自动变成\r\n
>
>%s 表示输入整个字符串

![虚拟内存图](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/%E8%99%9A%E6%8B%9F%E5%86%85%E5%AD%98%E5%9B%BE.png)

>存储类型---->变量的存储类型关系到变量的存储位置，C语言中定义了4种存储属性，即 **自动变量 (auto)、外部变量 (extern)、静态变量 (static)和寄存器变量 (register)** ，它关系到变量在内存中的存放位置，由此决定了变量的保留时间和变量的作用范围。

## Day 09 C语言

#### #if 条件编译

>**<font style="color:red">这种能够根据不同情况编译不同代码、产生不同目标文件的机制，称为条件编译。</font>**条件-编译是预处理程序的功能，不是编译器的功能。
>
>这些操作都是在预处理阶段完成的，多余的代码以及所有的宏都不会参与编译，不仅保证了代码的正确性，还减小了编译后文件的体积。
>
>e.g.
>
>```c 
>#include <stdio.h>
>int main(){
>    #if _WIN32
>        system("color 0c");
>        printf("http://c.biancheng.net\n");
>    #elif __linux__
>        printf("\033[22;31mhttp://c.biancheng.net\n\033[22;30m");
>    #else
>        printf("http://c.biancheng.net\n");
>    #endif
>    return 0;
>}
>```
>
>\#if、#elif、#else 和 #endif 都是预处理命令，整段代码的意思是：如果宏 _WIN32 的值为真，就保留第 4、5 行代码，删除第 7、9 行代码；如果宏 __linux__ 的值为真，就保留第 7 行代码；如果所有的宏都为假，就保留第 9 行代码。
>
>用法**<font style="color:red">（需要注意的是，#if 命令要求判断条件为“整型常量表达式”）</font>**
>
>``` c
>#if 整型常量表达式1
>    程序段1
>#elif 整型常量表达式2
>    程序段2
>#elif 整型常量表达式3
>    程序段3
>#else
>    程序段4
>#endif
>```
>
>

### Day10 C语言

#### 地址存放问题

>在c语言中，局部变量存放在栈中其地址是从低到高，即先声明的在内存的低位，后声明的在内存的高位。<font style="color:red">类似于栈的操作，先进后出</font>
>
>其他类型的变量都是正常存放，即照顺序存放。

### 第二周周末 C语言

#### 同样地址问题,多字节数据，高位存放在高地址，地位存放在地址。例

>![image-20230304234240708](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/image-20230304234240708.png)
>
>![image-20230304234443044](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/image-20230304234443044.png)
>
>other: 
>
>![image-20230304235153304](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/image-20230304235153304.png)
>
>![image-20230304235209162](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/image-20230304235209162.png)
>
>![image-20230304235241342](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/image-20230304235241342.png)

## Day11 C语言

#### 野指针

>野指针：定义一个指针，没有明确指向，不能访问野指针，且程序不会报错。
>
>其地址位**0000000000000000**
>
>![image-20230306103615011](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/image-20230306103615011.png)
>
>* 错误用法
>
>![](../../AppData/Roaming/Typora/typora-user-images/image-20230306102835519.png)
>
>* 正确

#### 指针运算

> 指针运算中，没有指针加指针，指针乘指针，指针除指针
>
> 但是有指针减指针
>
> * 作用： 表示相同类型之间的两个指针之间相差多少个元素，结果的正负，仅代表地址高低

####  指针与二维数组

> 本质：数组指针是一个指针，存放的是内容是地址，指向的内容是一个以为数组
>
> 二维数组的数组名是一个行指针，指向二维数组额首地址，行指针+n相当于地址偏离n行元素
>
> 行指针的定义的一般形式
>
> * <存储类型> <数据类型> (*指针变量名)[<font style="color:red">列数</font>]
>
> * ```c
>   int a[2][3]={1,2,3,4,5,6};
>   int(*p)[3]=a;
>   ```
>
> * 例如
>
>   ```c
>   #include <stdio.h>
>     
>   int main()
>   {
>       int a[2][3]={1,2,3,4,5,6};
>       int(*p)[3]=a;
>       printf("**p %d\n",**p);
>       printf("**p+1 %d\n",**p+1);
>       printf("**(p+1) %d\n",*(*(p)+1));
>       printf("**(p+1) %d\n",*(*(p)+2));
>       printf("**(p+1) %d\n",**(p+1));
>       return 0;
>   }
>   ```
>
>   ![image-20230306215849152](https://typora-1304577690.cos.ap-chengdu.myqcloud.com/image-20230306215849152.png)

#### 大端序和小端序

> 所有x86的计算机都是小端序，arm架构和网络编程中都是大端序

