#### tar 命令用于对文件进行打包压缩或解压，格式为"tar [选项]\[文件]"。

​	在Linux 系统中，常见的文件格式比较多，其中主要使用的是 .tar 或者 .tar.gz 或者 tar.bz2格式，我们不用担心格式太多而记不住，其实这些格式大部分都是由 tar 命令来生成的。

###### 1. 有且只能有一个的参数

```
-c     创建压缩文件                                       
-x     解开压缩文件
-t     查看压缩包内有哪些文件   
```

###### 2. 可选参数                    

```c
-z    用Gzip压缩或解压
-j    用 bzip2 压缩或解压                                 
-v    显示压缩或解压过程
-f    目标文件名                                              
-p    保留原始的权限与属性 
-P    使用绝对路径来压缩（大写P）            
-C    指定解压到的目录（大写C）
```

首先，-c 参数用于创建压缩文件， -x 参数用于解压文件，因此这两个参数不能同时使用。

其次，-z 参数指定使用 Gzip 格式来压缩或解压文件， -j 参数指定使用 bzip2 格式来压缩或解压文件。用户使用时根据文件的后缀决定使用 何种格式参数进行解压。

另外，对于 -v 参数，在执行某些压缩或者解压操作时，可能需要花费数个小时，如果屏幕一直没有输出，您一方面不好判断打包的进度情况，令一方面也会怀疑电脑死机了，因此非常推荐使用 -v 参数向用户不断显示压缩或解压的过程。

最后， -f 参数特别重要，它必须放到参数的最后一位，代表要压缩或解压的软件包名称。

```shell
[root@bogon 0608]# ls
560_file  aaa.txt  test
//1.打包（将test文件夹中的内容打包到test.tar包中，大小基本没变）
[root@bogon 0608]# tar -cvf test.tar test/
test/
test/560_file
test/aaa.txt
[root@bogon 0608]# ls -l
total 1146896
-rw-r--r--. 1 root root 587202560 Jun  8 09:38 560_file
-rw-r--r--. 1 root root        21 Jun  8 10:14 aaa.txt
drwxr-xr-x. 2 root root        37 Jun  8 11:09 test
-rw-r--r--. 1 root root 587212800 Jun  8 11:20 test.tar
//2.解包
[root@bogon 0608]# tar -xvf test.tar
test/
test/560_file
test/aaa.txt
[root@bogon 0608]# ls
560_file  aaa.txt  test  test.tar
[root@bogon 0608]# ls -l
total 1146896
-rw-r--r--. 1 root root 587202560 Jun  8 09:38 560_file
-rw-r--r--. 1 root root        21 Jun  8 10:14 aaa.txt
drwxr-xr-x. 2 root root        37 Jun  8 11:09 test
-rw-r--r--. 1 root root 587212800 Jun  8 11:27 test.tar
[root@bogon 0608]# ls

//3.打包并使用Gzip压缩
[root@bogon 0608]# tar -czvf test.tar.gz ./test/
./test/
./test/560_file
./test/aaa.txt
[root@bogon 0608]# ls -l
total 574004
-rw-r--r--. 1 root root 587202560 Jun  8 09:38 560_file
-rw-r--r--. 1 root root        21 Jun  8 10:14 aaa.txt
drwxr-xr-x. 2 root root        37 Jun  8 11:09 test
-rw-r--r--. 1 root root    570109 Jun  8 11:36 test.tar.gz
//4.解压
[root@bogon 0608]# tar -xzvf test.tar.gz
./test/
./test/560_file
./test/aaa.txt
[root@bogon test]# ls -l
total 573444
-rw-r--r--. 1 root root 587202560 Jun  8 11:09 560_file
-rw-r--r--. 1 root root        21 Jun  8 11:09 aaa.txt
//5.打包并使用bzip2压缩
[root@bogon 0608]# tar -cjvf test.tar.bz2 ./test/
./test/
./test/560_file
./test/aaa.txt
[root@bogon 0608]# ls -l
total 574008
-rw-r--r--. 1 root root 587202560 Jun  8 09:38 560_file
-rw-r--r--. 1 root root        21 Jun  8 10:14 aaa.txt
drwxr-xr-x. 2 root root        37 Jun  8 11:09 test
-rw-r--r--. 1 root root       661 Jun  8 12:34 test.tar.bz2
-rw-r--r--. 1 root root    570109 Jun  8 11:36 test.tar.gz
//6.解压
[root@bogon 0608]# tar -xjvf test.tar.bz2
./test/
./test/560_file
./test/aaa.txt
[root@bogon test]# ls -l
total 573444
-rw-r--r--. 1 root root 587202560 Jun  8 11:09 560_file
-rw-r--r--. 1 root root        21 Jun  8 11:09 aaa.txt

//7.查看压缩包中的内容
[root@bogon 0608]# tar -tf test.tar.gz
./test/
./test/560_file
./test/aaa.txt

//8.解压到指定文件夹（使用-C参数）
[root@bogon 0608]# tar -xzvf teat.tar.gz -C /home/hk/
./test/
./test/560_file
./test/aaa.txt
[root@bogon 0608]# cd ..
[root@bogon hk]# ls
0607  0608  c  test
```








