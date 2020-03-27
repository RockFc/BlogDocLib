#### 1. 概述

##### Shell脚本命令的工作方式有两种：交互式和批处理。

交互式（Interrctive）: 用户每输入一条命令就立即执行。

批处理（Batch）: 由用户事先编写好一个完整的 Shell 脚本， Shell 会一次性执行脚本中诸多的命令。

##### Shell脚本文件的名称可以任意。但为了避免被误以为是普通文件，建议将 .sh 后缀加上，以表示是一个脚本文件。

#### 2. 第一个简单的shell脚本

```shell
[root@rockman 0614]# vi example.sh
#!/bin/bash
#For example by rock
pwd
ls -l
#第一种执行方法   bash 脚本文件名及其参数 或者 sh 脚本文件名及其参数
[root@rockman 0614]# bash example.sh
/home/hk/0614
total 20
-rw-r--r--. 1 root root   65 Jun 14 17:32 aaa.txt
-rw-r--r--. 1 root root    0 Jun 14 16:17 abc.txt
-rw-r--r--. 1 root root    0 Jun 14 16:23 bbb.txt
-rw-r--r--. 1 root root    0 Jun 14 16:27 ccc.txt
-rw-r--r--. 1 root root   43 Jun 14 17:51 example.sh
-rw-rw-r--. 1 hk   hk   4731 Jun 14 09:10 openman.txt
-rw-rw-r--. 1 hk   hk     22 Jun 14 09:11 practice.txt
#第二种执行方法   先让Shell文件权限加上可执行，然后直接运行
[root@rockman 0614]# chmod u+x example.sh
[root@rockman 0614]# ./example.sh
/home/hk/0614
total 20
-rw-r--r--. 1 root root   65 Jun 14 17:32 aaa.txt
-rw-r--r--. 1 root root    0 Jun 14 16:17 abc.txt
-rw-r--r--. 1 root root    0 Jun 14 16:23 bbb.txt
-rw-r--r--. 1 root root    0 Jun 14 16:27 ccc.txt
-rwxr--r--. 1 root root   43 Jun 14 17:51 example.sh
-rw-rw-r--. 1 hk   hk   4731 Jun 14 09:10 openman.txt
-rw-rw-r--. 1 hk   hk     22 Jun 14 09:11 practice.txt
#第三种方法  source 脚本文件名及其参数 或者 . 脚本文件名及其参数
[root@rockman 0614]# source example.sh
/home/hk/0614
total 20
-rw-r--r--. 1 root root   65 Jun 14 17:32 aaa.txt
-rw-r--r--. 1 root root    0 Jun 14 16:17 abc.txt
-rw-r--r--. 1 root root    0 Jun 14 16:23 bbb.txt
-rw-r--r--. 1 root root    0 Jun 14 16:27 ccc.txt
-rw-r--r--. 1 root root   43 Jun 14 17:51 example.sh
-rw-rw-r--. 1 hk   hk   4731 Jun 14 09:10 openman.txt
-rw-rw-r--. 1 hk   hk     22 Jun 14 09:11 practice.txt
```

#### 3. 可以接受用户参数的脚本文件

Shell内置可用于接受参数的变量，变量之间可以使用空格间隔。

```
$0		当前shell脚本程序的名称
$#		总共有几个参数
$*		所有位置的参数值
$?		显示上一次命令执行的返回值
$1		第一个位置的参数值
$N		第N个位置的参数值
```

```shell
[root@rockman 0615]# cat example.sh
#!/bin/bash
echo "当前脚本名称为$0"
echo "总共有$#个参数，分别是$*。"
echo "第1个参数为$1, 第二个参数为$2。"

[root@rockman 0615]# sh example.sh one two three four five six
当前脚本名称为example.sh
总共有6个参数，分别是one two three four five six。
第1个参数为one, 第二个参数为two。
```