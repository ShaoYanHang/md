# * 什么是默认登录 Shell ？

在 Linux 操作系统，"/bin/bash" 是默认登录 Shell，是在创建用户时分配的。

使用 chsh 命令可以改变默认的 Shell 。

```
chsh syh -s /bin/sh
useradd syh -s /bin/sh
usermod syh -s /bin/sh
```



# 可以在 Shell 脚本中使用哪些类型的变量？

系统定义变量：系统变量是由系统系统自己创建的。这些变量通常由大写字母组成，可以通过 set 命令查看。

用户定义变量：用户变量由系统用户来生成和定义，变量的值可以通过命令 "echo $<变量名>" 查看。

#  Shell脚本中 $? 标记的用途是什么？

如果结束状态是 0 ，说明前一个命令执行成功。

如果结束状态不是0，说明命令执行失败。

# Bourne Shell(bash) 中有哪些特殊的变量？

$0 命令行中的脚本名字

$1 第一个命令行参数

$2 第二个命令行参数…..

…….$9 第九个命令行参数

$# 命令行参数的数量

$* 所有命令行参数，以空格隔开

# 如何取消变量或取消变量赋值？

```
unset
```



# 在 Shell 脚本中如何比较两个数字？

```
x=10
y=20
if [ $x > $y ]; then
	echo "x > y"
else 
	echo "x < y"
fi
```

```
x=10
y=20
if [ $x -gt $y ]; then
	echo "x > y"
else 
	echo "x < y"
fi
```

# [ ] 和 [[ ]]的区别

[(4条消息) shell中[ \]与[[ ]]的区别_运维@小兵的博客-CSDN博客](https://blog.csdn.net/anqixiang/article/details/111598067)


# Shell 脚本中 if 语法如何嵌套?

```
if [ 条件 ];then

else 
	if [ 条件 ]; then

	else
	fi
fi
```

#  * Shell 脚本中 case 语句的语法?

```
case $i in
	1)
	echo 1
	;;
	2)
	echo 2
	;;
	*)
	echo "other"
esac
```

# Shell 脚本中 for 循环语法？

```
for $i in arr
do
	
done
```

# Shell 脚本中 while 循环语法？

```
while [  ]
do
	
done
```

# 在 Shell 脚本如何定义函数呢？

```
name() 
{

}
name
```

# 如何使脚本可执行?

1. chmod
2. sh test.sh

# * 如何调试 Shell脚本？

```
sh -x test.sh
sh -nv test.sh
```

# * 如何将标准输出和错误输出同时重定向到同一位置?

```
2>&1
ls /usr/share/doc > out.txt 2>&1 
&>
ls /usr/share/doc &> out.txt
```



# * 如何执行算术运算？

```
let a + b
(($a+$b))
sum = $[1 + 2]
expr 5 + 2
```

# “#!/bin/bash”的作用 ?

答：#!/bin/bash是shell脚本的第一行，称为释伴（shebang）行。这里#符号叫做hash，而! 叫做 bang。它的意思是命令通过 /bin/bash 来执行。

# $# $*

# 如何使用Linux命令来移除文件头？

```
sed -i '1d' file
```

# 你怎么检查一个文本文件中某一行的长度？

```
sed -n '5p' file | wc -c
```



# 编程题

## **判断一文件是不是字符设备文件，如果是将其拷贝到** **/dev** 目录下？

```
#!/bin/bash
read -p "input filename please" FileName
if [ -c $FileName ]; then
	cp $FileName /dev
fi

# read 变量放置位置
# [ ]怎么判断
```

## 添加一个新组为 class1 ，然后添加属于这个组的 30 个用户，用户名的形式为 stdxx ，其中 xx 从 01 到 30 ？

```
#!/bin/bash
groupadd class1
for((i=1;i<31;i++))
do
	if [ $i -lt 10 ]; then
		useradd -g class1 std0$i
	else 
		useradd -g class1 std$i
	fi
done
```

## 编写 Shell 程序，实现自动删除 50 个账号的功能，账号名为stud1 至 stud50 ？

```
#!/bin/bash
for((i=1;i<51;i++))
do
	userdel -r stud$i
done
```

## * 写一个 sed 命令，修改 /tmp/input.txt 文件的内容？
要求：
· 删除所有空行。
· 一行中，如果包含 “11111”，则在 “11111” 前面插入 “AAA”，在 “11111” 后面插入 “BBB” 。比如：将内
容为 0000111112222 的一行改为 0000AAA11111BBB2222 。

```
sed -i '/^$/d'  /tmp/input.txt 
```

```
sed 's#\(11111\)#AAA\1BBB#g' /tmp/input.txt 
```

## 磁盘使用率检测

```
#!/bin/bash
SPACE=`df -Ph | awk {print int($5)}`
for $i in SPACE
do
	if [[ $i -gt 80 ]]; then
		echo "超标"
	fi
done
```

## 写一个脚本，实现判断192.168.1.0/24网络里，当前在线的IP有哪些，能ping通则认为在线

```
#!/bin/bash
for ip in `seq 1 255`
do
	ping -c 1 192.168.1.$ip > /dev/null 2>&1
	if [[ $0 -eq 0 ]]; then
		echo "192.168.1.$ip up"
	else
		echo "192.168.1.$ip down"
	fi
done
```

## shell下32位随机密码生成

```
cat /dev/urandom | head -1 | md5sum | cut -c 32 >> pass
```

