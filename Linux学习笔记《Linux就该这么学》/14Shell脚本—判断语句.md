#### 判断语句

##### Shell脚本中的条件测试语法可以判断表达式是否成立，若条件成立则返回数字0，否则便返回其他随机数值。

##### 条件测试语法的执行格式为 [ 条件表达式 ]，切记，条件表达式两边均应有一个空格。

条件表达式也可以使用 “test 条件表达式” 格式来使用。

##### 条件测试语句可以分为4种：文件测试语句、逻辑测试语句、整数值比较语句、字符串比较语句

#### 1. 文件测试语句 

```
-d			测试文件是否为目录类型
-e			测试文件是否存在
-f			判断是否为一般文件
-r			测试当前用户是否有权限读取
-w			测试当前用户是否有权限写入
-x			测试当前用户是否有权限执行
```
使用测试语句判断testdir是否为一个目录类型的文件，然后通过Shell解释器的内设￥?变量显示上一条命令执行后的返回值。如果返回值为0，则目录存在；如果返回值为非零的值，则目录不存在。

```shell
#测试语句与$?搭配使用
[root@rockman 0615]# ls -l
total 4
-rw-r--r--. 1 root root 140 Jun 15 16:43 example.sh
drwxr-xr-x. 2 root root   6 Jun 15 17:10 testdir
[root@rockman 0615]# [ -d testdir ]
[root@rockman 0615]# echo $?
#成功返回0
0
[root@rockman 0615]# test -d testdir
[root@rockman 0615]# echo $?
#成功返回0
0
```

#### 2. 逻辑测试语句 

逻辑语句用于对测试结果进行逻辑分析，根据测试结果可实现不同的效果。

##### 2.1 Shell中的逻辑“与”  &&

表示当前面的命令执行成功后才会执行它后面的命令。

```shell
#如果testdir存在，则打印Exist
[root@rockman 0615]# [ -e testdir ] && echo "Exist"
Exist
```

##### 2.2 Shell中的逻辑“或”  ||

表示当前面的命令执行失败才会执行它后面的名令。

```shell
#如果testdir01不存在，则打印Not Exist
[root@rockman 0615]# [ -e testdir01 ] || echo "Not Exist"
Not Exist
```

##### 2.3 Shell中的逻辑“非”  ！

它表示把条件测试 中的判断结果取相反值。 

```shell
#如果[ ! $USER = root ]执行失败，则答应administrator
[root@rockman 0615]# [ ! $USER = root ] || echo "administrator"
administrator
```

##### 2.4 逻辑语句的综合示例  

```shell
#先判断当前登录用户的USER变量名称是否等于root,然后用“!”取反，效果就变成了判断当前用户是否不是root
#如果条件成立（即不是root用户），则会根据逻辑“与”运算符输出user字样
#如果条件不成立（即是root用户），则前面的[ ! $USER = root ] && echo "user"语句执行失败，此时便会执行"||"后面的语句，输出root字样
[root@rockman 0619]# [ ! $USER = root ] && echo "user" || echo "root"
root
```

#### 3. 整数值比较语句

整数比较运算符仅是对数字的操作，不能将数字与字符串、文件等内容一起操作，而且不能想当然地使用日常生活张工的等号、大于号、小于号来判断。

```
-eq			是否等于
-ne			是否不等于
-gt			是否大于
-lt			是否小于
-le			是否等于或小于
-ge			是否大于或等于
```

```shell
[root@rockman 0619]# [ 10 -eq 10 ]
[root@rockman 0619]# echo $?
#成功返回0
0
[root@rockman 0619]# [ 10 -ne 10 ]
[root@rockman 0619]# echo $?
#失败返回非零
1
```

#### 4. 字符串比较

字符串比较语句用于判断测试字符串是否为空值，或者两个字符串是否相同。它经常用来判断某个变量是否未被定义（即内容为空值）。

```
=			比较字符串内容是否相同
!=			比较字符串内容是否不同
-z			判断字符串内容是否为空
```

```shell
[root@rockman 0619]# [ -z $String ]
[root@rockman 0619]# echo $?
#成功返回零，则String为空，未定义
0
[root@rockman 0619]# [ $LANG != "en.US" ] && echo "Not en.US"
Not en.US
```



