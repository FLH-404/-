# Shell 脚本基础

> vim 编辑  一般使用  .sh 为后缀 例 hello.sh 



### 一、shell脚本编辑

#### 1、 编写脚本

````shell
#!/bin/bash //第一句不是注释
# 一些自定义注释
shell 脚本
... ....

````

#### 2、 运行方式

##### 方式一:

* 先添加用户权限

chomd 755 hello.sh

* 执行

> 1.绝对路径 /root/xx/xx/hello.sh

> 2.相对路径./hello.sh

##### 方式二：

```shell
bash hello.sh
```

#### 3、编码转换

```` shell
dos2unix  +文件名  win->unix
unix2dos  +文件名  unix->win
````





### 二、Bash的基本功能

##### 1、历史命令

```shell
history[选项][历史命令保存文件文件]
-c:清空历史
-w:把缓存命令写入历史命令保存文件文件~/.bash_history

#注：~/.bash_history默认保存10000条 可以在/etc/profile中修改

历史命令的使用
*使用上、下箭头调用以前的历史命令
*使用“!n”重复执行第n条历史命令
*使用“!!”重复执行上一条命令
*使用“!字串”重复执行最后一条以该字 串开头的命令 

```



##### 2、bash中使用tab键

````shell
tab键 输入命令或文件名自动补全

````

##### 3、命令别名

````shell
alias 命令 = ‘别名’

alias vi='vim'

alias 查询

例子:
alias cp='cp -i'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'

1 第一顺位执行用绝对路径或相对路径执行的命令。
2 第二顺位执行别名。
3 第三顺位执行Bash的内部命令。
4 第四顺位执行按照$PATH环境变量定义的 目录查找顺序找到的第一个命令

别名永久生效
vi /root/.bashrc 

删除别名
unalias + 别名


````



##### 4、bash常用快捷键

| 快捷键 | 作用                                                         |
| ------ | ------------------------------------------------------------ |
| ctrl+A | 把光标移动到命令行开头。如果我们输入的命令过长，想要把光标移 动到命令行开头时使用。 |
| ctrl+E | 把光标移动到命令行结尾。                                     |
| ctrl+C | 强制终止当前的命令。                                         |
| ctrl+L | 清屏，相当于clear命令。                                      |
| ctrl+U | 删除或剪切光标之前的命令。我输入了一行很长的命令，不用使用退 格键一个一个字符的删除，使用这个快捷键会更加方便 ctrl+K 删除或剪切光标之后的内容。 |
| ctrl+Y | 粘贴ctrl+U或ctrl+K剪切的内容。                               |
| ctrl+R | 在历史命令中搜索，按下ctrl+R之后，就会出现搜索界面，只要输入 搜索内容，就会从历史命令中搜索。 |
| ctrl+D | 退出当前终端。                                               |
| ctrl+Z | 暂停，并放入后台。这个快捷键牵扯工作管理的内容，我们在系统管 理章节详细介绍。 |
| ctrl+Q | 恢复屏幕输出。                                               |



##### 5、输入输出重定向

* 标准输入输出

  

# 有个表格未写

* 输出重定向

# 有个表格未写



````shell
 > 覆盖 >>追加

#例1：命令正确结果输入到指定的文件

ls -l >abc 覆盖
ls -l >>abc 追加
#文件如果不存在会自动创建，错误不会输入

#例2：保存错误输出

ls -l 2> abc
ls -l 2>> abc


#例3：正确错误保存在同一个文件中 2>&1

ls -l > abc  2>&1
ls -l >> abc 2>&1

标准格式
命令  2&> 文件名

ls &>/dev/null 将数据放入垃圾箱

#例4：把正确的输出追加到文件 1中，把错误的输出追加 到文件2中

ls >>def(正确输出) 2>>efg(错误输出)


````

* 输入重定向

````shell
wc [选项][文件名]

-c 统计字节数
-w 统计单词数
-l 统计行数

wc <<结束符
#例1：
wc <<ssd
>asdsad
>dasd
>ssd
结束
````

##### 6、多命令顺序执行和管道符

* 1、多命令顺序执行

# 有个表格未写

````shell
dd if = 输入文件 of=输出文件 bs=字节数 count=个数

选项：  
if=输入文件  指定源文件或源设备  
of=输出文件  指定目标文件或目标设备  
bs=字节数    指定一次输入/输出多少字节，即把这些字节看做一个数据块  
count=个数  指定输入/输出多少个数据块 

#例：
date ; dd if=/dev/zero of=/testfiles bs=1k count = 100000; date

&&符号

ls && echo yes
前面正确执行->后面执行 否则 后面不执行
#例2:安装命令
。/configure && make && make install

||符号

ls || 前面正确执行->后面执行 否则 不执行

前面正确执行->后面不执行 否则 后面执行
#例3：判断命令的正确与否
ls && echo yes || echo no

````

* 管道符

  

```shell
| 管道符
命令1 | 命令2 

命令1的正确输出 作为命令2执行对象

#注：命令1的结果正确才行

ll -a /etc/ | more

grep [选项] 搜索内容

-i   忽略大小写
-n   输出行号
-v   反向查找
--color=auto 搜索出的关键字用颜色显示

grep "test" /etc/passwd

grep "test" -n --color=auto /etc/passwd




```

##### 7、通配符和其他特殊符号

* 通配符

  

# 有个表格未写



* bash中的其他特殊符号

#  有个表格未写



````shell
#例1：单引号与双引号对比

[root@bogon tmp]# name=sc
[root@bogon tmp]# echo $name
sc
[root@bogon tmp]# echo '$name'
$name
[root@bogon tmp]# echo "$name"
sc
[root@bogon tmp]# 

#例2：$() 和`` 调用系统命令

[root@bogon tmp]# echo date
date
[root@bogon tmp]# echo $(date)
2020年 05月 19日 星期二 13:19:47 CST

#例3：$ 引用变量
[root@bogon tmp]# name=sc
[root@bogon tmp]# echo $name
sc
#例4： \ 转义字符

[root@bogon tmp]# name=sc
[root@bogon tmp]# echo \$name
$name
[root@bogon tmp]# 




````



##### 8、用户自定义变量

* 什么是变量

​       变量是计算机内存的单元，其中存放的值 可以改变。

​        当Shell脚本需要保存一些信息 时，如一个文件名或是一     个数字，就把它 存放在一个变量中。每个变量有                                		一个名字 ，所以很容易引用它。使用变量可以保存 有用信息，使 系统获知用户相关设置，变 量也可以用于		保存暂时信息。 

* 变量设置规则

  变量名称可以由字母、数字和下划线组成 ，但是不能以数字开头。如果变量名是 “2name”则是错误的。 
  
  在Bash中，变量的默认类型都是字符串型 ，如果要进行数值运算，则必修指定变量 类型为数值型。 
  变量用等号连接值，等号左右两侧不能有 空
  
  变量的值如果有空格，需要使用单引号或 双引号包括。
  
  在变量的值中，可以使用“\”转义符。
  
  如果需要增加变量的值，那么可以进行变 量值的叠加。不过变量需要用双引号包含 “$变量名”或用${变量名}包含。 
  如果是把命令的结果作为变量值赋予变量 ，则需要使用反引号或$()包含命令。
  
  环境变量名建议大写，便于区分。

* 变量分类

  用户自定义变量。

  环境变量：这种变量中主要保存的是和系统操 作环境相关的数据。 

  位置参数变量：这种变量主要是用来向脚本当 中传递参数或数据的，变量名不能自定义，变 量作用是固定的。

  预定义变量：是Bash中已经定义好的变量，变 量名不能自定义，变量作用也是固定的。 

```shell
用户自定义变量

*变量定义 
[root@localhost ~]# name="CE DE"

*变量叠加 
[root@localhost ~]# aa=123 
[root@localhost ~]# aa="$aa"456 
[root@localhost ~]# aa=${aa}789  

*变量调用
[root@localhost ~]# echo $name 
 
*变量查看 
[root@localhost ~]# set 
 
*变量删除
[root@localhost ~]# unset name 

```



````shell
#知识点

pstree 查询继承树 shell
````



##### 9、环境变量

环境变量：这种变量中主要保存的是和系统操 作环境相关的数据。

用户自定义变量只在当前的Shell中生效， 而环境变量会在当前Shell和这个Shell的所 有子Shell当中生效。如果把环境变量写入 相应的配置文件，那么这个环境变量就会 在所有的Shell中生效 

````
定义
export 变量名 = 值

查询
env

删除
unset 变量名


````



* 常见的系统环境变量

  

````

PATH：系统查找命令的路径
[root@localhost ~]# echo $PATH /usr/lib/qt-3.3/bin:/usr/local/sbin:/usr/local/bin: /sbin:/bin:/usr/sbin:/usr/bin:/root/bin 
PATH="$PATH":/root/sh 

#PATH 变量叠加
PS1：
定义系统提示符的变量 
\d：显示日期，格式为“星期 月 日” 
\h：显示简写主机名。如默认主机名“localhost” 
\t：显示24小时制时间，格式为“HH:MM:SS” 
\T：显示12小时制时间，格式为“HH:MM:SS” 
\A：显示24小时制时间，格式为“HH:MM” 
\u：显示当前用户名 
\w：显示当前所在目录的完整名称 
\W：显示当前所在目录的最后一个目录 
\#：执行的第几个命令 
\$：提示符。如果是root用户会显示提示符为“#”，如果是普通用户 会显示提示符为“$” 
 
举例： 
[root@localhost ~]# PS1='[\u@\t \w]\$ ' 
 
[root@04:50:08 /usr/local/src]#PS1='[\u@\@ \h \# \W]\$‘ 
 
[root@04:53 上午 localhost 31 src]#PS1='[\u@\h \W]\$ ' 
````













