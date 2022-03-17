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

	rmdir # 删除空文件夹
	mv 
	cp
	cp -r
	#切换工作目录/进入上一个工作目录/进入当前用户主目录
	cd [path] & cd - & cd ~ 

	# 查看所有目录/按时间排序/翻页显示/为每个文件加上序号
	ls -al & ls -lrt & ls -al|more & ls |cat -n

	# 查找文件， find path -name word   
	#[-mtime/-atime/-ctime -n/+n] 查找n天内或n天前更改/访问过/创建的文件
	find ./ -name "a*" 

	# 文件索引数据库查找,linux每天都会记录文件位置到db中，使用updatedb强制更新记录
	locate test.txt

	# 查找执行文件 which/whereis
	which passwd		# 从系统PATH变量定义的目录中查找可执行文件的绝对路径
	whereis passwd		# 与which不同是，还能找到相关man文件


	# 压缩处理
	gzip test.txt
	gunzip test.txt.gz
	# tar 可以将整个目录中 的文件整合成一个包并压缩
	tar -zcvf test.tgz /zcf  # z表示gzip压缩，c表示创建压缩文件，v表示显示被压缩的文件，f使用文件名（手动命名）

	tar -zxcf test.tgz	[-C /tmp]	#z表示解压缩，-C表示指定目录

```

2. 文件处理

> cat vi vim tail head more  

``` bash
	# 创建文件
	touch test.txt
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
	# 格式转换,用来把windows文件格式转换为unix
	dos2unix test.txt

	# 修改文件权限，r(4)w(2)x(1)，读写执行，+-表示增减权限
	chmod +x test.txt		
	chmod 754 test.txt		
	# 改变文件拥有者
	chown zcf test.txt
	chown :zcf test.txt  		#改变用户分组
	chown zcf:zcf test.txt		#同时改变用户和分组	
```


3. 文件链接

``` bash
	# 硬链接，又称实际连接，不能连接目录，即使删除一个，另一个也能用，直到所有硬链接删除完才会被删除
	ln cc ccagain 
	# 软连接，类似快捷方式
	ln -s cc ccto

```


4. 管道，重定向,清空

> 管道的意义是把多个进程之间自动连接起来，把前者的输出作为后者的输入

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

5. grep文本行内容搜索

``` bash
	# -i 不区分大小写（默认区分）
	# -c 统计包含匹配的行数
	# -n 输出行号
	# -v 反向匹配，不包含
	grep [-ivnc] word test.txt

	# 可以与其他命令一起使用
	cat test.txt|grep 'hello' 

```



6. bash快捷删除

>	Ctl-U   删除光标到行首的所有字符,在某些设置下,删除全行
	
>	Ctl-W   删除当前光标到前边的最近一个空格之间的字符
	
>	Ctl-H   backspace,删除光标前边的字符
	
>	Ctl-R   匹配最相近的一个文件，然后输出



##### 例行任务管理
	
1. at 定时执行命令

``` bash
	at now + 30 minutes 				// 指定时间开启定时任务,也可指定具体时间（00:00 2021-03-15）
	at> /sbin/shutdown-h now			// 开启配置，设置执行的命令
	at> <EOF>							// ctrl+d退出定时配置

	atq									// 查看at任务队列
	atrm	id							// 按标号删除任务

```

2. cron	周期性执行任务

> 通过cron表达式周期性执行，但是是以分钟为最小量级

``` bash
crontab -e #进入编辑任务
crontab -l #查看当前设置的所有任务
crontab -r #删除所有任务

* * * * * command
| | | | | |
| | | | | 需要执行的命令
| | | | |
| | | | 星期几，0~6,0表示周日
| | | 月份
| |	日期
| |
| 小时，23-3/1 23点到1点每小时执行一次
|
表示分钟，每分钟表示为*或*/1
```

3. /etc/crontab 管理

> 系统级的任务管理，通过在/etc/crontab 文件下编辑添加cron命令创建定时任务

``` bash

# 与crontab命令不同，多了两级角色和运行方式

* * * * * root echo "hello"

* * * * * root run-parts /etc/cron.daily	# 运行cron.daily 文件下的所有命令


```


#### 进程管理


``` bash
	#ps 查看瞬时进程
 	ps -ef|grep java 



 	#kill 杀掉进程 常用信号量 1(重启)9(强制结束)15(正常结束)
 	kill -9  PID

 	#killall 按进程名杀掉,不用先查pid，也确保误杀了pid
 	killall httpd

```



#### shell脚本

``` bash

#!/bin/bash
#一般第一行标注使用什么解析器还执行这个脚本

unset name   		#取消变量

echo ${#Array[@]} 	# 输出全部元素，以空格分隔
echo ${#Array[*]}	# 输出全部元素，没有分隔符

echo ${Array[@:1:3]}	# 从1开始，截取三位元素

Coon=(${Array[@]}${Name[@]})	#拼接数组

Array=(${Array[@]/word/newWord})	#把数组中的word替换成newWord

readonly ro=100				#只读变量，常量


# 双引号，单引号，反引号(`)，转义符四种引用符

# 双引号是部分引用，其中的单引号，反义号，转移符，$都不会被直译

a=100
echo "$a" 		#会输出100


# 单引号是全引用，包含的内容都会被直译

echo '$a' 		#会输出$a

# 命令替换,当需要获取命令执行结果作为变量时，反引号和$()一样的效果

time=`date`		
echo $time 		#输出当前时间


data=$(ls -al)
echo data		#输出文件列表信息


# 运算符

## 算数运算符，包含加减乘除等常用运算符，但是仅支持整数计算，会舍弃小数部分

## 位运算，左移(<<)右移(>>)，与或非等


## 自增++自减--


## $[] 做算数运算

echo $[1+1]		# 2

## expr 做运算,操作符与操作数之间必须有空格

expr 1 + 1 


## 判断结构

if test 判断条件;then
	command
fi

#必须左右各一个空格
if [ 判断条件 ];then
	command
elif [ 判断条件 ];then
	command
else
	command
fi



case var in 
case1) command;;
case2) command;;
case3) command;;
*) command;;
esac
			

## for循环

for data in ${items}
# for data in  ("a" "b" "c")
# for data in 1 2 3 4 5
# for data in `seq 1 2 100`		从1到100递进，步长为2，就是1,3,5递增
# for ((i=1;i<=100;i++))		类C的for
do
	command
done



## while循环


while [[ condition ]]; do
	#statements
done

## until 当判断为假值的时候才进入循环

until [[ condition ]]; do
	#statements
done






```



#### 重定向

``` bash
# 把命令结果存到文件中
ls -al > test.txt

# 追加到文件内容
ls -al >> test.txt

# >&讲错误也输出到文件
ls -al > test.txt 2>&1 

```