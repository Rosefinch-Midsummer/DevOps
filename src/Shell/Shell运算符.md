# Shell 运算符

<!-- toc -->


## 运算符简介

Shell 和其他编程语言一样，支持多种运算符，包括：

- 算术运算符
- 关系运算符
- 布尔运算符
- 字符串运算符
- 文件测试运算符

原生 bash 不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。

expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

例如，两个数相加 (**注意使用的是反引号 \` 而不是单引号 '**)：

## 算术运算符

下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                        | 举例                        |
| --- | ------------------------- | ------------------------- |
| +   | 加法                        | `expr $a + $b` 结果为 30。    |
| -   | 减法                        | `expr $a - $b` 结果为 -10。   |
| *   | 乘法                        | `expr $a \* $b` 结果为  200。 |
| /   | 除法                        | `expr $b / $a` 结果为 2。     |
| %   | 取余                        | `expr $b % $a` 结果为 0。     |
| =   | 赋值                        | a=$b 把变量 b 的值赋给 a。        |
| ==  | 相等。用于比较两个数字，相同则返回 true。   | [ $a == $b ] 返回 false。    |
| !=  | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。     |

**注意：** 条件表达式要放在方括号之间，并且要有空格，例如: **`[$a==$b]`** 是错误的，必须写成 **`[ $a == $b ]`**。

算术运算符实例如下：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b /a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```

执行脚本，输出结果如下所示：

```
a + b : 30
a - b : -10
a * b : 200
b /a : 2
b % a : 0
a 不等于 b
```

> **注意：**
> 
> - 乘号 (\*) 前边必须加反斜杠 (\) 才能实现乘法运算；
> - if...then...fi 是条件语句，后续将会讲解。
> - 在 MAC 中 shell 的 expr 语法是：**$((表达式))**，此处表达式中的 "\*" 不需要转义符号 "\\" 。


## 三种求值方式

### **$((表达式))**

在 MAC 中 shell 的 expr 语法是：**$((表达式))**，此处表达式中的 "\*" 不需要转义符号 "\\" 。

例如计算（2+3）x 4 的值：

`$(((2+3)*4))`

### **`$[表达式]`**

例 1：计算（2+3）x 4 的值：

推荐使用 `$[(2+3)*4]` 

例 2：求出命令行的两个参数【整数】的和

`$[$1 + $2]`

### expr 简单加法实例

```shell
#!/bin/bash  
  
val=`expr 2 + 2`  
echo "两数之和为 : $val"  
```

执行脚本，输出结果如下所示：

```
两数之和为 : 4
```

注意：

- 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
- 完整的表达式要被 \`\` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。

---


## 自增和自减操作符

尽管 Shell 本身没有像 C、C++ 或 Java 那样的 ++ 和 -- 操作符，但可以通过其他方式实现相同的功能。以下是一些常见的方法：

### 使用 let 命令

let 命令允许对整数进行算术运算。

```shell
#!/bin/bash

# 初始化变量
num=5

# 自增
let num++

# 自减
let num--

echo $num
```

### 使用 $(( )) 进行算术运算

$(( )) 语法也是进行算术运算的一种方式。

```shell
#!/bin/bash

# 初始化变量
num=5

# 自增
num=$((num + 1))

# 自减
num=$((num - 1))

echo $num
```
### 使用 expr 命令

expr 命令可以用于算术运算，但不如 **let** 和 **$(( ))** 常用。

```shell
#!/bin/bash

# 初始化变量
num=5

# 自增
num=$(expr $num + 1)

# 自减
num=$(expr $num - 1)

echo $num
```

### 使用 (( )) 进行算术运算

与 $(( )) 类似，(( )) 语法也可以用于算术运算。

```shell
#!/bin/bash

# 初始化变量
num=5

# 自增
((num++))

# 自减
((num--))

echo $num
```



---

## 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明 | 举例 |
|---|---|---|
|-eq | 检测两个数是否相等，相等返回 true。|[ $a -eq $b ] 返回 false。|
|-ne | 检测两个数是否不相等，不相等返回 true。|[ $a -ne $b ] 返回 true。|
|-gt | 检测左边的数是否大于右边的，如果是，则返回 true。|[ $a -gt $b ] 返回 false。|
|-lt | 检测左边的数是否小于右边的，如果是，则返回 true。|[ $a -lt $b ] 返回 true。|
|-ge | 检测左边的数是否大于等于右边的，如果是，则返回 true。|[ $a -ge $b ] 返回 false。|
|-le | 检测左边的数是否小于等于右边的，如果是，则返回 true。|[ $a -le $b ] 返回 true。|

关系运算符实例如下：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

a=10
b=20

if [ $a -eq $b ]
then
   echo "$a -eq $b : a 等于 b"
else
   echo "$a -eq $b: a 不等于 b"
fi
if [ $a -ne $b ]
then
   echo "$a -ne $b: a 不等于 b"
else
   echo "$a -ne $b : a 等于 b"
fi
if [ $a -gt $b ]
then
   echo "$a -gt $b: a 大于 b"
else
   echo "$a -gt $b: a 不大于 b"
fi
if [ $a -lt $b ]
then
   echo "$a -lt $b: a 小于 b"
else
   echo "$a -lt $b: a 不小于 b"
fi
if [ $a -ge $b ]
then
   echo "$a -ge $b: a 大于或等于 b"
else
   echo "$a -ge $b: a 小于 b"
fi
if [ $a -le $b ]
then
   echo "$a -le $b: a 小于或等于 b"
else
   echo "$a -le $b: a 大于 b"
fi
```

执行脚本，输出结果如下所示：

```
10 -eq 20: a 不等于 b
10 -ne 20: a 不等于 b
10 -gt 20: a 不大于 b
10 -lt 20: a 小于 b
10 -ge 20: a 小于 b
10 -le 20: a 小于或等于 b
```

---

## 布尔运算符

下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明 | 举例 |
|---|---|---|
|!| 非运算，表达式为 true 则返回 false，否则返回 true。|[ ! false ] 返回 true。|
|-o | 或运算，有一个表达式为 true 则返回 true。|[ $a -lt 20 -o $b -gt 100 ] 返回 true。|
|-a | 与运算，两个表达式都为 true 才返回 true。|[ $a -lt 20 -a $b -gt 100 ] 返回 false。|

布尔运算符实例如下：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

a=10
b=20

if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a == $b: a 等于 b"
fi
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a 小于 100 且 $b 大于 15 : 返回 true"
else
   echo "$a 小于 100 且 $b 大于 15 : 返回 false"
fi
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a 小于 100 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 100 或 $b 大于 100 : 返回 false"
fi
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a 小于 5 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 5 或 $b 大于 100 : 返回 false"
fi
```

执行脚本，输出结果如下所示：

```
10 != 20 : a 不等于 b
10 小于 100 且 20 大于 15 : 返回 true
10 小于 100 或 20 大于 100 : 返回 true
10 小于 5 或 20 大于 100 : 返回 false
```

---

## 逻辑运算符

以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

| 运算符  | 说明      | 举例                                           |
| ---- | ------- | -------------------------------------------- |
| &&   | 逻辑的 AND | `[[ $a -lt 100 && $b -gt 100 ]]` 返回 false    |
| \|\| | 逻辑的 OR  | `[[ $a -lt 100` \|\| `$b -gt 100 ]]` 返回 true |

逻辑运算符实例如下：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

a=10
b=20

if [[ $a -lt 100 && $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi

if [[ $a -lt 100 || $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi
```

执行脚本，输出结果如下所示：

```
返回 false
返回 true
```

## 附录布尔运算符和逻辑运算符的关系

在 `bash shell` 中，逻辑运算符和布尔运算符其实是同一个概念，主要用来连接两个条件表达式或命令。通常逻辑运算符包括 `-o`(OR)、`-a`(AND) 和 `!`(NOT)，而布尔运算符包括 `||`（逻辑或）和 `&&`（逻辑与）。

逻辑运算符和布尔运算符的主要区别在于语法形式和用法上。在 `bash shell` 中，我们可以使用逻辑运算符和布尔运算符来连接条件表达式或命令，例如：
- 使用逻辑运算符 `-o` 和 `-a`，示例如下：
```bash
if [ "$a" -gt 10 -a "$b" -lt 20 ]; then
    echo "条件成立"
fi
```

- 使用布尔运算符 `||` 和 `&&`，示例如下：
```bash
if [ "$a" -gt 10 ] && [ "$b" -lt 20 ]; then
    echo "条件成立"
fi
```

综上所述，逻辑运算符和布尔运算符在 `bash shell` 中都可以用来连接条件表达式或命令，只是语法形式和用法略有不同。逻辑运算符更常用于 `test` 表达式中，而布尔运算符更常用于逻辑判断语句中。

---

## 字符串运算符

下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

| 运算符 | 说明 | 举例 |
|---|---|---|
|=| 检测两个字符串是否相等，相等返回 true。|[ $a = $b ] 返回 false。|
|!=| 检测两个字符串是否不相等，不相等返回 true。|[ $a != $b ] 返回 true。|
|-z | 检测字符串长度是否为 0，为 0 返回 true。|[ -z $a ] 返回 false。|
|-n | 检测字符串长度是否不为 0，不为 0 返回 true。|[ -n "$a" ] 返回 true。|
|$| 检测字符串是否不为空，不为空返回 true。|[ $a ] 返回 true。|

字符串运算符实例如下：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

a="abc"
b="efg"

if [ $a = $b ]
then
   echo "$a = $b : a 等于 b"
else
   echo "$a = $b: a 不等于 b"
fi
if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
if [ -z $a ]
then
   echo "-z $a : 字符串长度为 0"
else
   echo "-z $a : 字符串长度不为 0"
fi
if [ -n "$a" ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
if [ $a ]
then
   echo "$a : 字符串不为空"
else
   echo "$a : 字符串为空"
fi
```

执行脚本，输出结果如下所示：

```
abc = efg: a 不等于 b
abc != efg : a 不等于 b
-z abc : 字符串长度不为 0
-n abc : 字符串长度不为 0
abc : 字符串不为空
```

---

## 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

属性检测描述如下：

| 操作符     | 说明                                       | 举例                     |
| ------- | ---------------------------------------- | ---------------------- |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。               | [ -b $file ] 返回 false。 |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。              | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录，如果是，则返回 true。                  | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。           | [ -g $file ] 返回 false。 |
| -k file | 检测文件是否设置了粘着位 (Sticky Bit)，如果是，则返回 true。   | [ -k $file ] 返回 false。 |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。           | [ -u $file ] 返回 false。 |
| -r file | 检测文件是否可读，如果是，则返回 true。                   | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写，如果是，则返回 true。                   | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行，如果是，则返回 true。                  | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于 0），不为空返回 true。          | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。             | [ -e $file ] 返回 true。  |

其他检查符：

- **-S**: 判断某文件是否 socket。
- **-L**: 检测文件是否存在并且是一个符号链接。

### 文件检测实例

变量 file 表示文件 **/var/www/runoob/test.sh**，它的大小为 100 字节，具有 **rwx** 权限。下面的代码，将检测该文件的各种属性：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

file="/var/www/runoob/test.sh"
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi
if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi
if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi
if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

执行脚本，输出结果如下所示：

```
文件可读
文件可写
文件可执行
文件为普通文件
文件不是个目录
文件不为空
文件存在
```

## Shell test 命令

Shell 中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

---

### 数值测试

| 参数 | 说明 |
|---|---|
|-eq | 等于则为真 |
|-ne | 不等于则为真 |
|-gt | 大于则为真 |
|-ge | 大于等于则为真 |
|-lt | 小于则为真 |
|-le | 小于等于则为真 |

相等判断实例

```shell
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo ' 两个数相等！'
else
    echo ' 两个数不相等！'
fi
```

输出结果：

```
两个数相等！
```

代码中的 `[]` 执行基本的算数运算，如：

```shell
#!/bin/bash

a=5
b=6

result=$[a+b] # 注意等号两边不能有空格
echo "result 为： $result"
```

结果为:

```
result 为： 11
```

---

### 字符串测试

| 参数 | 说明 |
|---|---|
|=| 等于则为真 |
|!=| 不相等则为真 |
|-z 字符串 | 字符串的长度为零则为真 |
|-n 字符串 | 字符串的长度不为零则为真 |

字符串相等判断实例

```shell
num1="ru1noob"
num2="runoob"
if test $num1 = $num2
then
    echo ' 两个字符串相等！'
else
    echo ' 两个字符串不相等！'
fi
```

输出结果：

```
两个字符串不相等！
```

---

### 文件测试

| 参数     | 说明                 |
| ------ | ------------------ |
| -e 文件名 | 如果文件存在则为真          |
| -r 文件名 | 如果文件存在且可读则为真       |
| -w 文件名 | 如果文件存在且可写则为真       |
| -x 文件名 | 如果文件存在且可执行则为真      |
| -s 文件名 | 如果文件存在且至少有一个字符则为真  |
| -d 文件名 | 如果文件存在且为目录则为真      |
| -f 文件名 | 如果文件存在且为普通文件则为真    |
| -c 文件名 | 如果文件存在且为字符型特殊文件则为真 |
| -b 文件名 | 如果文件存在且为块特殊文件则为真   |

判断文件存在实例

```shell
cd /bin
if test -e ./bash
then
    echo ' 文件已存在！'
else
    echo ' 文件不存在！'
fi
```

输出结果：

```
文件已存在！
```

另外，Shell 还提供了与 ( -a )、或 ( -o )、非 ( ! ) 三个逻辑操作符用于将测试条件连接起来，其优先级为： ! 最高， -a 次之， -o 最低。例如：

```shell
cd /bin
if test -e ./notFile -o -e ./bash
then
    echo ' 至少有一个文件存在！'
else
    echo ' 两个文件都不存在 '
fi
```

输出结果：

```
至少有一个文件存在！
```

