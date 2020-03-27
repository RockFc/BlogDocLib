##### 1. 概述

复制内容：右键弹出快捷菜单，选择“标记（K）”,然后选中所需要的内容，然后右键即可

粘贴内容：右键弹出快捷菜单，选择“粘贴(P)”

命令参数的路径：要使用反斜杠‘\’，不要使用正斜杠‘/’   如  del d:\test2/file\my.txt

命令参数的路径：若存在空格，应使用双引号将路径引起来  如  del "d:\program file\my.txt"

文件及目录名中不能包含下列任何字符：  \ / : * ? " < > |

 rem	//在批处理文件中添加注释，其后的命令不会被执行，但会回显

::		//::也可以起到rem的注释作用，且不会有回显

任何以冒号:开头的字符行，在批处理中都被视作标记（label）,而直接忽略其后的所有内容

```
有效标号：冒号后紧跟一个以字母数字开头的字符串，goto语句可以是识别
无效标号：冒号后紧跟一个非字母数字的一个特殊符号，goto无法识别的标号，可以起到注释作用，::常被用作注释符号
```

终端命令执行： Ctrl + Z

##### 2. 常用命令

 （1）cd		切换目录

（2）dir		显示目录中的内容 —— ls

（3）ren	文件或目录重命名	—— mv

（4）md	创建目录	—— mkdir

（5）rd		删除目录	—— rm

```
rd movie             //删除当前目录下的movie空文件夹
rd /s /q d:\test     //使用安静模式删除d:\test（除目录本身外，还将删除指定目录下的所有子目录和文件）
```

（6）copy	拷贝文件 —— cp

（7）xcopy	更强大的复制命令

（8）move	移动文件

（9）del		删除文件

```
del test   //删除当前目录下的test文件夹中的所有非只读文件（子目录下的文件不删除；删除前会进行确认）
del /f test  //删除当前目录下的test文件夹汇总的所有文件
del /f /s /q  test  //删除当前目录下的test文件夹中所有文件
```

##### 3. 批处理文件

```powershell
PS F:\windows_cmd_test> cat .\date.bat
@echo off

cd.>temp.txt
echo %date% >> temp.txt
echo %time% >> temp.txt
copy temp.txt+insertfile.txt newfile.txt >nul
::强制并且不加询问的删除
del /f/q temp.txt >nul
del /f/q insertfile.txt >nul
ren newfile.txt insertfile.txt
echo 添加完毕
::将当前目录执行dir命令结果保存到dir.txt中
dir > dir.txt

pause
PS F:\windows_cmd_test> .\date.bat
添加完毕
```



