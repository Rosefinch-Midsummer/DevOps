# Shell 函数

<!-- toc -->

## 系统函数

### basename

功能：返回完整路径最后 / 的部分，常用于获取文件名

```
basename [pathname] [suffix] 

basename [string] [suffix]
```

选项：suffix 为后缀，如果 suffix 被指定了，basename 会将 pathname 或 string 中的 suffix 去掉。

案例：输出 /home/aaa/test.txt 的 “test.txt" 部分

`basename /home/aaa/test.txt` 得到 `test.txt`

`basename /home/aaa/test.txt .txt` 得到 `test`

### dirname

功能：返回完整路径最后 / 的前面的部分，常用于返回路径部分

dirname 文件绝对路径（功能描述：从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分））


案例：输出 /home/aaa/test.txt 的 /home/aaa

`dirname /home/aaa/test.txt` 得到 `/home/aaa`



## 自定义函数

linux shell 可以用户定义函数，然后在 shell 脚本中可以随便调用。

shell 中函数的定义格式如下：

```shell
[ function ] funname [()]

{

    action;

    [return int;]

}
```

说明：

- 1、可以带 function fun () 定义，也可以直接 fun () 定义，不带任何参数。
- 2、参数返回，可以显示加：**return** 返回，如果不加，将以最后一条命令运行结果，作为返回值。 **return** 后跟数值 n (0-255).

注意：return 语句只能返回一个介于 0 到 255 之间的整数
### 无返回值

下面的例子定义了一个函数并进行调用：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

demoFun (){
    echo "这是我的第一个 shell 函数！"
}
echo "----- 函数开始执行 -----"
demoFun
echo "----- 函数执行完毕 -----"
```

输出结果：

```
----- 函数开始执行 -----
这是我的第一个 shell 函数！
----- 函数执行完毕 -----
```

### 有返回值

下面定义一个带有 return 语句的函数：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

funWithReturn (){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字:"
    read aNum
    echo "输入第二个数字:"
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

输出类似下面：

```
这个函数会对输入的两个数字进行相加运算...
输入第一个数字: 
1
输入第二个数字: 
2
两个数字分别为 1 和 2 !
输入的两个数字之和为 3 !
```

函数返回值在调用该函数后通过 $? 来获得。

**注意：** 所有函数在使用前必须定义。这意味着必须将函数放在脚本开始部分，直至 shell 解释器首次发现它时，才可以使用。调用函数仅使用其函数名即可。

**注意：** return 语句只能返回一个介于 0 到 255 之间的整数，而两个输入数字的和可能超过这个范围。

要解决这个问题，您可以修改 return 语句，直接使用 echo 输出和而不是使用 return：

```shell
funWithReturn (){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字:"
    read aNum
    echo "输入第二个数字:"
    read anotherNum
    sum=$(($aNum + $anotherNum))
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    echo $sum  # 输出两个数字的和
}
```

---

### 函数参数

在 Shell 中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1 表示第一个参数，$2 表示第二个参数...

带参数的函数示例：

```shell
#!/bin/bash
# author: 菜鸟教程
# url:www.runoob.com

funWithParam (){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个！"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

输出结果：

```
第一个参数为 1 !
第二个参数为 2 !
第十个参数为 10 !
第十个参数为 34 !
第十一个参数为 73 !
参数总数有 11 个！
作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
```

注意，`$10` 不能获取第十个参数，获取第十个参数需要 `${10}`。当 n>=10 时，需要使用 `${n}` 来获取参数。

### 特殊参数字符

另外，还有几个特殊字符用来处理参数：

| 参数处理 | 说明 |
|---|---|
|$#| 传递到脚本或函数的参数个数 |
|$*| 以一个单字符串显示所有向脚本传递的参数 |
|$$| 脚本运行的当前进程 ID 号 |
|$!| 后台运行的最后一个进程的 ID 号 |
|$@| 与 $*相同，但是使用时加引号，并在引号中返回每个参数。|
|$-| 显示 Shell 使用的当前选项，与 set 命令功能相同。|
|$?| 显示最后命令的退出状态。0 表示没有错误，其他任何值表明有错误。|

$? 仅对其上一条指令负责，一旦函数返回后其返回值没有立即保存入参数，那么其返回值将不再能通过 $? 获得。

测试代码：

```shell
#!/bin/bash
function demoFun1 (){
    echo "这是我的第一个 shell 函数！"
    return `expr 1 + 1`
}

demoFun1
echo $?

function demoFun2 (){
 echo "这是我的第二个 shell 函数！"
 expr 1 + 1
}

demoFun2
echo $?
demoFun1
echo 在这里插入命令！
echo $?
```

输出结果：

```
这是我的第一个 shell 函数！
2
这是我的第二个 shell 函数！
2
0
这是我的第一个 shell 函数！
在这里插入命令！
0
```

调用 demoFun2 后，函数最后一条命令 expr 1 + 1 得到的返回值（$? 值）为 0，意思是这个命令没有出错。所有的命令的返回值仅表示其是否出错，而不会有其他有含义的结果。

第二次调用 demoFun1 后，没有立即查看 $? 的值，而是先插入了一条别的 echo 命令，最后再查看 $? 的值得到的是 0，也就是上一条 echo 命令的结果，而 demoFun1 的返回值被覆盖了。

下面这个测试，连续使用两次 **echo $?**，得到的结果不同，更为直观：

```shell
#!/bin/bash

function demoFun1 (){
    echo "这是我的第一个 shell 函数！"
    return `expr 1 + 1`
}

demoFun1
echo $?
echo $?
```

输出结果：

```
这是我的第一个 shell 函数！
2
0
```
