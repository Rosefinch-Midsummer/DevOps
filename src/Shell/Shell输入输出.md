# Shell 输入输出

<!-- toc -->

## Shell read 命令

基本语法

read（选项）（参数）选项：

read 读取控制台输入

-p：指定读取值时的提示符；

-t：指定读取值时等待的时间（秒），如果没有在指定的时间内输入，就不再等待了。。・参数

变量：指定读取值的变量名应用实例 testRead.sh

案例 1：读取控制台输入一个 num 值

`read -p"Please input a number: NUM1`

`echo $NUM1`

案例 2：读取控制台输入一个 num 值，在 10 秒内输入。

`read -t 10 -p"Please input a number: NUM1`

`echo $NUM1`

## Shell echo 命令

Shell 的 echo 指令与 PHP 的 echo 指令类似，都是用于字符串的输出。命令格式：

```
echo string
```

您可以使用 echo 实现更复杂的输出格式控制。

### 1. 显示普通字符串:

```
echo "It is a test"
```

这里的双引号完全可以省略，以下命令与上面实例效果一致：

```
echo It is a test
```
### 2. 显示转义字符

```
echo "\"It is a test\""
```

结果将是:

```
"It is a test"
```

同样，双引号也可以省略

### 3. 显示变量

read 命令从标准输入中读取一行，并把输入行的每个字段的值指定给 shell 变量

```shell
#!/bin/sh
read name 
echo "$name It is a test"
```

以上代码保存为 test.sh，name 接收标准输入的变量，结果将是:

```
[root@www ~]# sh test.sh
OK                     #标准输入
OK It is a test        #输出
```
### 4. 显示换行

```shell
echo -e "OK! \n" # -e 开启转义
echo "It is a test"
```

输出结果：

```
OK!

It is a test
```

### 5. 显示不换行

#!/bin/sh
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"

输出结果：

OK! It is a test

### 6. 显示结果定向至文件

```
echo "It is a test" > myfile
```

### 7. 原样输出字符串，不进行转义或取变量 (用单引号)

```shell
echo '$name\"'
```

输出结果：

```
$name\"
```

### 8. 显示命令执行结果

```shell
echo `date`
```

**注意：** 这里使用的是反引号 \`, 而不是单引号 '。

结果将显示当前日期

```
Wed Sep 18 04:11:38 AM UTC 2024
```


## Shell printf 命令

上一章节我们学习了 Shell 的 echo 命令，本章节我们来学习 Shell 的另一个输出命令 printf。

printf 命令模仿 C 程序库（library）里的 printf () 程序。

printf 由 POSIX 标准所定义，因此使用 printf 的脚本比使用 echo 移植性好。

printf 使用引用文本或空格分隔的参数，外面可以在 **printf** 中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。默认的 printf 不会像 echo 自动添加换行符，我们可以手动添加 \n。

printf 命令的语法：

```
printf  format-string  [arguments...]
```

**参数说明：**

- **format-string:** 一个格式字符串，它包含普通文本和格式说明符。
- **arguments:** 用于填充格式说明符的参数列表。。

格式说明符由 % 字符开始，后跟一个或多个字符，用于指定输出的格式。常用的格式说明符包括：

- `% s`：字符串
- `% d`：十进制整数
- `% f`：浮点数
- `% c`：字符
- `% x`：十六进制数
- `% o`：八进制数
- `% b`：二进制数
- `% e`：科学计数法表示的浮点数

### printf 输出实例

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com
 
printf "%-10s %-8s %-4s\n" 姓名 性别 体重 kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876
```

执行脚本，输出结果如下所示：

```
姓名     性别   体重 kg
郭靖     男      66.12
杨过     男      48.65
郭芙     女      47.99
```

% s % c % d % f 都是格式替代符，**％s** 输出一个字符串，**％d** 整型输出，**％c** 输出一个字符，**％f** 输出实数，以小数形式输出。

%-10s 指一个宽度为 10 个字符（**-** 表示左对齐，没有则表示右对齐），任何字符都会被显示在 10 个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。

%-4.2f 指格式化为小数，其中 **.2** 指保留 2 位小数。

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com
 
# format-string 为双引号
printf "% d % s\n" 1 "abc"

# 单引号与双引号效果一样 
printf '% d % s\n' 1 "abc" 

# 没有引号也可以输出
printf % s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf % s abc def

printf "% s\n" abc def

printf "% s % s % s\n" a b c d e f g h i j

# 如果没有 arguments，那么 % s 用 NULL 代替，% d 用 0 代替
printf "% s and % d \n"
```

执行脚本，输出结果如下所示：

```
1 abc
1 abc
abcdefabcdefabc
def
a b c
d e f
g h i
j  
 and 0
```

### printf 的转义序列

| 序列 | 说明 |
|---|---|
|\a | 警告字符，通常为 ASCII 的 BEL 字符 |
|\b | 后退 |
|\c | 抑制（不显示）输出结果中任何结尾的换行字符（只在 % b 格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略 |
|\f | 换页（formfeed）|
|\n | 换行 |
|\r | 回车（Carriage return）|
|\t | 水平制表符 |
|\v | 垂直制表符 |
|\\| 一个字面上的反斜杠字符 |
|\ddd | 表示 1 到 3 位数八进制值的字符。仅在格式字符串中有效 |
|\0ddd | 表示 1 到 3 位的八进制值字符 |

实例：

```shell
$ printf "a string, no processing:<% s>\n" "A\nB"
a string, no processing:<A\nB>

$ printf "a string, no processing:<% b>\n" "A\nB"
a string, no processing:<A
B>

$ printf "www.runoob.com \a"
www.runoob.com $                  #不换行
```

