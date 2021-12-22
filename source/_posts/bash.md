---
title: bash
date: 2020-07-08 18:08:09
tags: linux
categories: linux
---
### linux 基础命令

#### 先学会使用帮助

> 记不得命令全名？记不得命令是干啥的？记不得要啥子参数？

1. 搜索命令
	
``` bash
	# 查看命令说明文档，用pageup/pagedn 翻页，分为9个类型
	man command
	# (1)、用户可以操作的命令或者是可执行文件
	#(2)、系统核心可调用的函数与工具等
	#(3)、一些常用的函数与数据库
	#(4)、设备文件的说明
	#(5)、设置文件或者某些文件的格式
	#(6)、游戏
	#(7)、惯例与协议等。例如Linux标准文件系统、网络协议、ASCⅡ，码等说明内容
	#(8)、系统管理员可用的管理条令
	#(9)、与内核有关的文件

	# 查询第三分类中的命令
	man 3 command
	# 不记得分类的时候匹配
	man -k command 

```

<!-- more -->

2. 查看命令信息

> 查看简要说明

``` bash
	whatis command 
	# 使用正则匹配
	whatis -w "loc*"
```
> 查看详细信息

``` bash
	info command
```

3. 查看路径

``` bash
	# 查看命令的路径
	which command
	# 搜索命令的路径，多用于查看软件的目录
	whereis command

```

#### 文件及目录处理（想必都懂，啰嗦一下）

1. 目录处理

``` bash 
	
	# 用&区分了不同的使用情况，而不是连接使用

	mkdir 
	rm & rm -rf 
	mv 
	cp
	cp -r
	#切换工作目录/进入上一个工作目录/进入当前用户主目录
	cd [path] & cd - & cd ~ 

	# 查看所有目录/按时间排序/翻页显示/为每个文件加上序号
	ls -al & ls -lrt & ls -al|more & ls |cat -n

	# 查找文件， find path -name word
	find ./ -name "a*"

```

2. 文件处理

> cat vi vim tail head more  

``` bash
	# 查看内容并显示行号
	cat -n file	
	# 查询并按页显示，回车向下一行，空格翻页
	cat file |more 
	# 编辑文件
	vi file
	# 输出最后100行信息/输出倒数第5行/持续输出内容
	tail -n 100 file & tail -5 file & tail -f file 
	# 前100行信息/第10行信息
	head -n 100 file & head -10 file 

```


3. 文件链接

``` bash
	# 硬链接，即使删除一个，另一个也能用
	ln cc ccagain 
	# 软连接，类似快捷方式
	ln -s cc ccto

```


4. 管道，重定向,清空

``` bash
	#批处理命令连接执行
	ps -ef | grep "a"

	# 前面成功执行后一句&&,前面失败执行后一句||
	ls /a && echo "success" || echo "failed"
	# 将标准输出和标准错误重定向到同一文件
	ls /home > log 2> &1
	# 等价于
	ls /home &> log
	#清空文件
	:> log


	# 统计内容 wc(world count),缺失文件参数，从标准输入读取
	# -c 统计字节数。

	#-l 统计行数。

	#-m 统计字符数。这个标志不能与 -c 标志一起使用。

	#-w 统计字数。一个字被定义为由空白、跳格或换行字符分隔的字符串
	
	wc [option] [file]

	# 统计包含AAA且不包含BBB的总行数
	cat log |grep AAA|grep -v BBB|wc -l


```

5. bash快捷删除

>	Ctl-U   删除光标到行首的所有字符,在某些设置下,删除全行
	
>	Ctl-W   删除当前光标到前边的最近一个空格之间的字符
	
>	Ctl-H   backspace,删除光标前边的字符
	
>	Ctl-R   匹配最相近的一个文件，然后输出