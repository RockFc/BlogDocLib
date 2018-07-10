##### 11. 数组

（1）数组初始化时，如果初始化数字个数超过存储区个数，就忽略多余数字。如果初始化数字个数少于存储区个数，则后面的存储区自动被初始化为0。

（2）数组名称可以代表数组里第一个存储区的地址。可以对数组名称进行sizeof计算，结果是数组所包含的总字节数。

（3）变长数组：C99规范允许声明数组时使用变量表示数组里包含的存储区个数。但是变长数组不能初始化。

（4）一维、二维少数族名称不可被赋值。对二维数组名进行sizeof计算可以得到数组里面所有存储区的总大小。可以在二维数组名称后加一个下标，这个写法中的下标作为组下标使用。它表示组下标对应组中第一个存储区的地址。

```c
int arr[3][4];     //arr[1] 表示二维数组中 arr[1][0]的地址， 这个写法有时候可以代表一组中的所有存储区（看做一个一维数组）
```

（5）产生7个1~36之间的整数

```c
#include <stdio.h>
void Product7Numbers(int arr[7])
{
        int count = 0, i = 0;
        srand(time(0));
    	//改循环中，负责产生随机数
        do{
        arr[count] = rand()%36 + 1;
        //该循环中，用于检查新产生的随机数是否和之前产生的所有随机数重复
        for(i=0; i<=count-1; ++i)
        {
                if(arr[count] == arr[i])
                        break;
        }
        //如果新产生的随机数和之前的所有随机数都不重复，则count加1，表示成功产生一个随机数
        if(i == count)
        {
                count++;
        }
        }while(count<7);
}
void PrintNumbers(int arr[7])
{
        int i = 0;
        for(i=0; i<7; i++)
        {
                printf("arr[%d]: %d\n",i, arr[i]);
        }
}
int main()
{
        int arr[7] = {0};
        PrintNumbers(arr);
        Product7Numbers(arr);
        PrintNumbers(arr);
        return 0;
}
```

```shell
[root@localhost 0701]# vim RandomNum.c
[root@localhost 0701]# gcc RandomNum.c
[root@localhost 0701]# ./a.out
arr[0]: 0
arr[1]: 0
arr[2]: 0
arr[3]: 0
arr[4]: 0
arr[5]: 0
arr[6]: 0
arr[0]: 17
arr[1]: 1
arr[2]: 2
arr[3]: 8
arr[4]: 24
arr[5]: 6
arr[6]: 31
```

##### 12. 函数

（1）多函数程序执行的模式如下：

```
a. 整个程序的执行时间被话费成多个端，每个段分给一个函数使用；
b. 任何两个时间段不能相互重叠，并且所用时间段必须连续；
c. 如果函数A把自己的时间分给函数B使用，则函数B结束后必须把后面的时间还给函数A
```

（2）变量不可以跨函数使用，不用函数内部的变量可以重名。

（3）如果函数多次执行，则每次变量对应的存储区可能不同，volative 声明变量的存储区可以在多个程序中使用。

（4）用于实现数据传递的存储区必须由调用函数提供。

（5）调用函数只有一次机会获得返回值，得到后必须立刻使用或者转存到某个存储区里。

（6）函数名前什么都不写，表示函数有一个整数类型的返回值。（C99中不支持）

（7）只要能当数字使用的内容都可以作为实际参数。

（8）如果小括号里什么都不写，表示函数形参的个数和类型是任意的。

（9）数组是可以作为形参使用的。

（10）使用数组形参可以实现双向数据传递，这种参数叫做输入输出参数。

（11）数组形参声明中可以省略存储区个数。

##### 12. 静态变量

（1）静态变量初始化只在程序开始的时候执行一次。

（2）可以跨函数使用静态变量的存储区。

（3）静态全局变量的作用域只包含声明它的文件内部的所有语句，其生命周期为程序执行期间。

##### 13. 指针

（1）指针变量可以用来记录地址数据，也可以根据记录的地址数据找到来源的存储区。

（2）指针和存储区的捆绑关系在程序执行过程中可能不断变化。

（3）可以把指针看做变量的某种身份，可以使用指针实现针对身份的编程。

```c
#include <stdio.h>
void Sort3Numbers(int* p_a, int* p_b, int* p_c)
{
        if(NULL == p_a |NULL == p_b | NULL == p_c)
        {
                return;
        }
        int t = 0;
        if(*p_c < *p_b)
        {
                t = *p_c; *p_c = *p_b; *p_b = t;
        }
        if(*p_b < *p_a)
        {
                t = *p_b; *p_b = *p_a; *p_a = t;
        }
        printf("%d %d %d\n", *p_a, *p_b, *p_c);
}
int main()
{
        int a=3, b=20, c=1;
        Sort3Numbers(&a, &b, &c);
        return 0;
}
```

```shell
[root@localhost 0701]# vim Sort3Numbers.c
[root@localhost 0701]# gcc Sort3Numbers.c
[root@localhost 0701]# ./a.out
1 3 20
```

```c
#include <stdio.h>

void Sort3Numbers1(int** pp_a, int** pp_b, int** pp_c)
{
        if(NULL == pp_a |NULL == pp_b | NULL == pp_c)
        {
                return;
        }
        int* p_t = NULL;
        if(**pp_c < **pp_b)
        {
                p_t = *pp_c; *pp_c = *pp_b; *pp_b = p_t;
        }
        if(**pp_b < **pp_a)
        {
                p_t = *pp_b; *pp_b = *pp_a; *pp_a = p_t;
        }
        printf("%d %d %d\n", **pp_a, **pp_b, **pp_c);
}
int main()
{
        int a=3, b=20, c=1;
        int *p_a = &a;
        int *p_b = &b;
        int *p_c = &c;
        Sort3Numbers1(&p_a, &p_b, &p_c);
        printf("the mininum is %d\n", *p_a);
        return 0;
}
```

```shell
[root@localhost 0701]# vim Sort3Numbers1.c
[root@localhost 0701]# gcc Sort3Numbers1.c
[root@localhost 0701]# ./a.out
1 3 20
the mininum is 1
```

##### 13. 函数声明 

（1）C语言中，函数形参的个数可以不固定，这种参数叫做变长参数（例如 printf 和 scanf ）。

（2）函数的隐式声明：如果编译器首先编译函数调用的语句，就会猜测函数的格式，猜测结果里认为函数有一个整数类型存储区记录返回值，有任意多个不确定类型的形参。

（3）隐式声明中形式参数的类型只能是整数类型或者双精度浮点类型。

（4）如果隐式声明的格式和函数真实的格式不一致则编译时会出错。

##### 14. 递归函数

（1）C语言中一个函数可以调用自己，这种函数叫递归函数。如果一个问题可以拆分成 n 个小问题，其中至少有一个小问题和原来的问题本质是一样的，这种问题就可以采用递归函数解决。

（2）递归函数编写步骤

```
a. 编写语句描述问题的分解方式（假设递归函数已经完成）
b. 在递归函数的开头编写分支处理无法分解的情况（这个分支必须保证函数可以结束）
```

（3）采用递归函数解决问题的思路叫递归，采用循环解决同样问题的思路叫递推。

（4）没有初始化的全局变量自耦东被初始化为 0，没有初始化的静态变量被自动初始化为 0。

（5）递归算1+2+3+...+n = ?

```c
#include <stdio.h>
int RecursionAdd(int num)
{
        if(num == 1)
        {
                return 1;
        }
        return RecursionAdd(num-1) + num;
}
int main(int argc, char** argv)
{
        int num = atoi(*(argv+1));
        printf("Sun is %d\n", RecursionAdd(num));
        return 0;
}
```

```
[root@localhost 0701]# vim RecursionAdd.c
[root@localhost 0701]# gcc RecursionAdd.c
[root@localhost 0701]# ./a.out 2
Sun is 3
```

##### 15. 指针与字符串

（1）地址数据只能参与如下计算过程：

```
a. 地址 +/- 整数    实际上加减的是 n 个捆绑存储区的大小
b. 地址 - 地址      结果是一个整数，这个整数表示两个地址之间包含的捆绑存储区个数
```

（2）计算机里对数组下标的处理：

```
arr[num]    ------>      *(arr+num)
```

（3）所有跨函数使用存储区都必须通过指针实现。

（4）函数可以把一个存储区里的地址作为返回值使用，这个时候它需要提供一个指针类型的存储区记录这个返回值。

（5）不可以把非静态局部变量的地址作为返回值使用。

（6）前置 const 和后置 const

```c
const int* p_num;    //表示 p_num 所指的存储区中的数字不能修改 
int const *p_num1;	 //表示 p_num1 这个指针的值不能修改	
```

（7）无类型指针通常作为函数的形式 参数使用，可以通过它把任意类型的存储区传递给被调用函数。

（8）字符串 —— C语言中所有的文字信息必须记录在一组连续的字符类型存储区里，所有文件信息必须以 '\0'（ASCII是整数0）字符作为结尾。

（9）所有的字符串都可以采用一个字符类型指针表示。（一般使用字符串字面值或字符数组表示）

（10）编译器在编译时会自动在字符串字面值的末尾增加一个 '\0' 字符。编译器会把字符串字面值替换成第一个字符所在存储区的地址。（程序中内容一样的字符串字面值只有 1 个，多个并列的字符串会被合并成一个）。

（11）只有包含 '\0' 的字符数组才能当做字符串使用。

（12）可以使用字符串字面值对字符数组进行初始化，这个时候 '\0' 字符也会被初始化到字符数组里。

（13）可以使用 %s 作占位符把字符串内容显示在屏幕上。

（14）将一个字符颠倒

```c
#include <stdio.h>
#include <string.h>
char* ReversalCharString(char* p_char, int size)
{
        char* p_start = p_char, *p_end = p_char + size - 1;
        char temp = 0;
        while(p_start < p_end)
        {
                temp = *p_start;
                *p_start = *p_end;
                *p_end = temp;
                p_start++;
                p_end--;
        }
        return p_char;
}
int main(int argc, char** argv)
{
        char* p_test = *(argv+1);
        ReversalCharString(p_test, strlen(p_test));
        printf("%s\n", p_test);
        return 0;
}
```

```shell
[root@localhost 0701]# vim ReversalCharString.c
[root@localhost 0701]# gcc ReversalCharString.c
[root@localhost 0701]# ./a.out  ILoveYou
uoYevoLI
```

（15）指针数组里每个存储区都是一个指针类型的存储区，字符指针数组包含多个字符指针，每个字符指针可以代表一个字符串。所以可以采用字符指针数组带保镖多个相关字符串。例如 int main(int argc, char* argv[])。

##### 16. 字符串相关处理

（1）处理字符串的函数

```
strlen		统计有效字符个数（'\0'之前的字符），和sizeof不同。
strcat		合并两个字符串（把参数2中的内容追加到参数1的数组中），如果参数1所指的存储区不够大，则会造成严重的错误
strncat		功能和 strcat 一样，它比 strcat 多了一个整数类型参数表示数组里空余存储区的个数
strcmp		比较两个字符串的大小（长度），根据 ASCII 码比较，ASCII 码大则这个字符大。
			返回值： 1  —— 前一个字符串大 
				   -1  —— 后一个字符串大
				   	0  —— 相等
strncmp		只比较前 n 个字符
strcpy		把一个字符串复制到字符串数组里（替换原有内容）
memset		把字符数组里多个存储区填充成同一个字符，无返回值。
strstr		在大字符串里查找小字符串的位置，返回值为找到字符串的指针，找不到的时候返回值是NULL。
```

（2）字符串输入输出函数（#include<stdio.h>）

```
sprintf			把数字按照格式放在字符数组里形成字符串
例如： sprintf(str, "%d %c %g\n", num, ch, fnum);	//把num、ch、fnum 按照 "%d %c %g\n"的geshi8输出到字符数组 str 中。
sscanf			从字符串里获得数字并记录在缓存区里。
例如： sscanf(str, "%d %c %g", &num, %ch, &fnum); 	//把str字符串中的数字按 "%d %c %g"格式分别保存到 num、ch、fnum 中。
```

（3）字符串转换成数字

```
atoi		将字符串中不带小数点的数字字符 转换成 整数类型
atof		将字符串中带小数点的数字字符	 转换成 双精度浮点型
```

##### 17. 宏与程序编译

（1）编译器会在预编译阶段把程序中所有宏名称替换成它所代表的数字。

（2）可以在编译命令中使用 -D 选项指定宏名称所代表的的数字。

```
gcc -DPI = 3.14f   01macro.c    //只有在编译时才知道的数字就应该用宏表示
```

（3）宏不可以使用自己的存储区和函数进行数据传递。宏也没有形式参数和返回值。

（4）能到数字使用的宏必须写成表达式

```
#define  ABS(n)   n >= 0 ? n | 0 - n   //宏的参数可以直接代表函数的存储区
```

（5）宏没有形式参数，所以不能保证优先计算参数内部的操作符。宏里面所有能当数字使用的参数都应该写在小括号里。

（6）条件编译可以让编译器在编译时从 n 组语句中选择一组编译而或略其他组。

```c
#ifdef / #ifndef   ...    #else    ...    #endif
#if   ...  #elif(任意多次)  ...  #else ...   #endif
```

（7）多文件编译步骤

```
a. 把所有函数分散在多个不同的源文件里（主函数通常单独占一个文件夹）。 
b. 为每个源文件编写配对的头文件（主函数所在的源文件不需要头文件）。 
	- 所有不分配内存的内容都可以写在头文件里
	- 头文件里至少应该包含配对源文件里所有函数的声明
c. 在每个源文件里使用 #include 预处理指令包含所有必要的头文件
	- 配对头文件是必需头文件
	- 如果源文件里使用了头文件里声明的函数，则这个头文件也是必要头文件
```

（8）头文件中采用的宏名称应该根据文件路径变化得到，例如 \__ADD_H__。

（9）如果希望在一个源文件里使用另一个源文件里声明的全局变量，就需要使用 extern 关键字再次声明这个全局变量。

​	- 使用 extern 关键字声明变量的语句不会分配内存，所以通常放在头文件里。

（10）不可以在一个源文件里使用另一个源文件中的静态全局变量

##### 18. 结构体

（1）结构体声明中包含的变量声明语句不会分配内存。

（2）结构体变量会分配存储区，它可以用来记录数字。

（3）结构体中，在捆绑过的结构体指针后使用 -> 再加上某个子存储区名称，这个写法可以表示那个子存储区。

```
p_Person->age 		等价于 	（*p_person）.age
```

（4）我们的计算机中，所有的指针都占 4 个字节。

##### 19. 枚举（enum）和联合（union）

（1）计算机把从 0 开始的一组连续的整数分配给枚举类型中的所有名称。

（2）声明枚举时，可以指定某个名称代表的整数，后面的整数也会发生变化。

（3）联合存储区可以当做多种不同类型存储区使用，联合的每个子存储区代表联合存储区一种可写的类型。

（4）联合的所有子存储区开始地址一样，他们所占的位置互相重叠。

（5）联合存储区的大小是最大子存储区的大小

##### 20. 函数指针

（1）二级指针通常作为函数的形式参数使用，这个时候可以让被调用函数使用调用函数的一级指针存储区。

（2）C 语言里，函数也有地址，函数名称可以代表函数的地址。

（3）函数指针可以用来记录函数的地址，函数指针也需要先声明，然后才能使用

​	- 函数指针声明可以根据函数声明变化得到

​	- 例如： int add(int, int)    --> 	int (*p_func)(int, int)

（4）函数指针也分类型，不同类型的函数指针适合于不同类型的函数捆绑。

（5）函数指针一旦和函数捆绑，就可以用来调用那个函数。

（6）函数指针可以用作函数的形式参数，会作为实际参数使用的函数叫做回调函数。

##### 21. 动态内存分配

（1）malloc 函数可以动态分配一组连续的字节

​	它的返回值表示分配好的第一个字节的地址

​	如果分配失败则返回值为 NULL

​	malloc 函数用一个无类型指针存储区记录返回值，需要首先强制类型转换成有类型指针然后才可以使用

​	动态分配内存使用完成后必须释放   free(p_num);

​	一个函数动态分配内存可以给任何其他函数使用（即延长了动态内存的生命周期）。

##### 22. 文件操作

（1）所有 文件都采用二进制方式记录数字

​	如果文件里的所有二进制内容都对应字符，则这种文件叫文本文件

​	除了文本文件以外的所有文件叫做二进制文件

​	文本文件可以当做二进制文件使用

（2）文件操作的基本步骤

```
a. 打开文件 fopen("a.txt", "w");
b. 操作文件    fread/fwrite   (以二进制方式操作)
c. 关闭文件 fclose(p_file);
```

（3） fopen 函数

```
fopen 函数有两个参数，第一个为文件名字符串，第二个为打开文件方式标识
打开文件的方式：
"r"		只能查看文件内容，只能从文件头开始查看。如果文件不存在，打开会失败
"r+"	比"r"多了修改功能
"w"		只能修改文件内容不能查看，只能从文件头开始修改，如果文件不存在就创建文件，如果已存在就删除文件内容
"w+"	比"w"多了查看功能
"a"		只能修改文件内容不能查看，在文件原有内容后面追加新内容，如果我呢间不存在就创建文件，如果文件存在不许修改文件原有内容
"a+"	比"a"多了查看功能
"b"		也是一种文件打开方式，它可以和上面任何一种打开方式混用，这个打开方式表示程序中只能采用二进制方式操作文件
```

（4）fopen 函数的返回值是一个地址，这个地址应该记录在文件指针里。

​	程序中只能使用文件指针代表打开的文件

​	fopen 函数如果打开文件失败则返回值为 NULL

（5）一旦完成对文件的操作之后就必须使用 fclose 函数关闭文件。

​	fclose 函数需要文件指针作为参数

​	关闭文件后文件指针成为野指针，必须恢复成空指针

（6）文件操作分两种：	

​	a. 把内存中一组连续存储区的内容拷贝到文件里（写文件）

​	b. 把文件中一组连续字节的内容拷贝到内存里（读文件）

（7）fread 函数采用二进制方式读文件内容，fwrite 函数采用二进制方式写文件内容。这两个函数都需要四个参数

```
第一个参数： 内存中第一个存储区的地址
第二个参数： 内存中单个存储区的大小
第三个参数： 希望操作的存储区个数
第四个参数： 文件指针
```

这两个函数的返回值都表示实际操作的存储区个数。

（8）fprintf 函数可以把数据按照格式记录在文本文件里。这个函数的参数就是在 printf 函数参数的前面加一个文件指针。

（9）fscanf 函数可以按照格式从文本文件里落去数字并记录在存储区里。该函数的参数就是在 scanf 函数参数的前面增加一个文件指针。

（10）ftell 函数可以得到位置指针的数值。

（11）计算机里为每个打开的文件保留一个整数，这个整数表示下一次读写操作的开始位置。这个位置一定在两个相邻的字节之间。这个正式表示文件头开始到这个位置之间包含的字节个数。这个整数叫做**文件的位置指针**。

（12）每当从文件中获得 N 个字节或者向文件里写入 N 个字节后，位置指针向后移动 N 个位置。

（13）rewind 函数可以把位置指针设置成文件开头。

（14）fseek 函数可以把位置指针设置成文件里的任何位置。如果目标位置在基准位置后则用非负数表示，否则用负数表示。

（15）示例：以结构体为单位读写文件

```c
[root@rockman 0706]# cat filewrite.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
typedef struct{
        int id;
        float salary;
        char name[10];
}Person;
void filewrite(char filepath[16])
{
        FILE* pfile = fopen(filepath, "ab");
        int i=0, cnt=0;
        Person ps;
        if(pfile)
        {
                for(i=0; i<2; i++)
                {
                        printf("Please input your ID:");
                        scanf("%d", &ps.id);
                        printf("Please input your name:");
                        scanf("%s", &ps.name);
                        printf("Please input your salary:");
                        scanf("%f", &ps.salary);
                        cnt = fwrite(&ps, sizeof(ps), 1, pfile);
                        if(cnt)
                        {
                                printf("the record was writen successfully!\n");
                        }
                        else{
                                printf("fwrite error!\n");
                        }
                }
        }
        fclose(pfile);
        pfile = NULL;
}
int main()
{
        filewrite("person.bin");
        return 0;
}
```

```shell
[root@rockman 0706]# ./write.out
Please input your ID:1
Please input your name:rock
Please input your salary:1111
the record was writen successfully!
Please input your ID:2
Please input your name:mary
Please input your salary:2222
the record was writen successfully!
```

```c
[root@rockman 0706]# cat fileread.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
typedef struct{
        int id;
        float salary;
        char name[10];
}Person;
void fileread(char filepath[16])
{
        FILE* pfile = fopen(filepath, "rb");
        int cnt=0;
        Person ps;
        if(pfile)
        {
                while(1){
                        cnt = fread(&ps, sizeof(ps), 1, pfile);
                        //printf("cnt:%d\n", cnt);
                        if(cnt)
                        {
                                printf("ID:%d, name:%s, salary:%f\n",ps.id, ps.name, ps.salary);
                        }
                        else{
                                break;
                        }
                }
        }
        fclose(pfile);
        pfile = NULL;
}
int main()
{
        fileread("person.bin");
        return 0;
}
```

```shell
[root@rockman 0706]# ./read.out
ID:1, name:rock, salary:1111.000000
ID:2, name:mary, salary:2222.000000
```

（16）编程实现文件拷贝功能，类似 cp 命令

```c
[root@rockman 0706]# cat mycp.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
void mycp(char* psrc, char* pdest)
{
        char buf[100] = {0};
        int size = 0;
        FILE* pfsrc = NULL, *pfdest = NULL;
        pfsrc = fopen(psrc, "rb");
        if(!pfsrc)
        {
                printf("resouse file open error!\n");
                fclose(pfsrc);
                pfsrc = NULL;
                return;
        }
        pfdest = fopen(pdest, "wb");
        if(!pfdest)
        {
                printf("destination file open error!\n");
                fclose(pfdest);
                pfdest = NULL;
                return;
        }
        while(1)
        {
                size = fread(buf, sizeof(char), 100, pfsrc);
                if(!size)
                {
                        break;
                }
                fwrite(buf, sizeof(char), size, pfdest);
        }
        fclose(pfdest);
        pfdest = NULL;
        fclose(pfsrc);
        pfsrc = NULL;
        return;
}
int main(int argc, char** argv)
{
        mycp(*(argv+1), *(argv+2));
        return 0;
}
```

```shell
[root@rockman 0706]# gcc mycp.c -o cp.out
[root@rockman 0706]# ./cp.out person.bin person_copy.bin
[root@rockman 0706]# ls -l per*
-rw-r--r--. 1 root root 40 Jul  6 10:45 person.bin
-rw-r--r--. 1 root root 40 Jul  6 11:08 person_copy.bin
```







