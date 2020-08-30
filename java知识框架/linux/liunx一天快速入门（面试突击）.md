# 

# liunx一天快速入门（面试突击）



# Linux 系统目录结构

```shell
 ls / 
```



树状目录结构：	

![目录结构 2020-08-20 12.04.32](/Users/mac/Desktop/linux/屏幕快照 2020-08-20 12.04.32.png)



/代表根目录

. 可以代表当前目录 也可以写 ./

..表示上级目录 也可以写../

bin:是系统的命令库，里面有许多常用命令

````shell
macdeMacBook-Air:/ mac$ cd bin
macdeMacBook-Air:bin mac$ ls
[		df		launchctl	pwd		test
bash		domainname	link		rm		unlink
cat		echo		ln		rmdir		wait4path
chmod		ed		ls		sh		zsh
cp		expr		mkdir		sleep
csh		hostname	mv		stty
date		kill		pax		sync
dd		ksh		ps		tcsh
macdeMacBook-Air:bin mac$ 

````



在 Linux 系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。

**/etc**： 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。

**/bin, /sbin, /usr/bin, /usr/sbin**: 这是系统预设的执行文件的放置目录，比如 ls 就是在/bin/ls 目录下的。

==值得提出的是==，/bin, /usr/bin 是给系统用户使用的指令（除root外的通用户），而/sbin, /usr/sbin 则是给root使用的指令。 

**/var**： 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，具体在/var/log 目录下，另外mail的预设放置也是在这里。



如果一个目录或文件名以一个点 . 开始，表示这个目录或文件是一个==隐藏目录==或文件(如：.bashrc)。即以默认方式查找时，不显示该目录或文件。





# Linux 文件与目录管理

- **绝对路径：**
  路径的写法，由根目录 / 写起，例如： /usr/share/doc 这个目录。
- **相对路径：**
  路径的写法，不是由 / 写起，例如由 /usr/share/doc 要到 /usr/share/man 底下时，可以写成： cd ../man 这就是相对路径的写法啦！



## 处理目录的常用命令

- ls: 列出目录及文件名

- cd：切换目录

- pwd：显示目前的目录

- mkdir：创建一个新的目录

- rmdir：删除一个空的目录

- ==cp==: 复制文件或目录

  - ```shell
    cp  css复习.pdf a.pdf       #将css复习.pdf复制为a.pdf
    ```

  - 

- ==rm==: 移除文件或目录

  - -r递归删除

  

- ==mv==: 移动文件与目录，或修改文件与目录的名称

  - 写法 mv [-fiu] source destination

  - ```shell
    mv a.pdf test
    ```

  

  ## Linux 文件内容查看

  - cat
    - -n ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
    - -b :空行无行号

  ## vim

  什么是 vim？

  Vim是从 vi 发展出来的一个文本编辑器

  vi 文件

  i：进入编辑模式

  esc: 退出编辑模式

  :wq 保存并退出

  后面的内容跟运维有关，不介绍了，

  ## Shell 教程

  第一个shell脚本

  ```shell
  echo "Hello World !"
  ```

  

两种执行方法：

1 cd 到相应目录

````shell
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
````

2  cd 到相应目录

```shell
sh test.sh
```



- 双引号里可以有变量

- 双引号里可以出现转义字符

  - ```shell
    a="hell0"
    echo "hel "$a" "  #用双引号拼接字符串，可以加入变量
    ```

  - 

获取字符串长度：

```shell
a="hell0"
echo ${#a}
```

提取子字符串

```shell
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```



查找字符串

```shell
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```



## 数组

定义数组

```shell
array_name=(value0 value1 value2 value3)
```

取出元素：

```shell
${数组名[下标]}
```

```shell
# 取得数组元素的个数
length=${#array_name[@]}
```



注释： #



原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用

算术运算符：

````shell
var=`expr 2 + 2 `   #注意2和运算符+必须要有空格，否则不成功
echo $var
````

## 关系运算符

- 
- -eq  检测两个数是否相等，相等返回 true。
- -ne  检测两个数是否不相等，不相等返回 true。
- -gt 检测左边的数是否大于右边的，如果是，则返回 true。
- -lt
- -ge
- -le

```shell
a=10
b=20
echo `expr $a -eq $b `
```



## 参数传递

shell：

```shell
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

运行脚本

```shell
$ chmod +x test.sh 
$ ./test.sh 1 2 3
```

