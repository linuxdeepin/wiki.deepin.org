---
title: fish脚本编程
description: 
published: true
date: 2022-10-25T02:12:07.811Z
tags: fish
editor: markdown
dateCreated: 2022-06-28T08:21:45.363Z
---

# fish 脚本编程

## 前言

fish shell 是和 bash shell 类似的产物，它比 linux 系统默认的 bash 命令行更优胜的地方在于它的人性化，很多智能体验，或者文字样式不需要经过任何配置，就能够使用。而 zsh 之类的需要自己去配置，非常麻烦。

另一方面，fish shell 有自己的脚本语言，这个和 bash 不兼容，所以一旦你使用了 fish，那么就要考虑到这一层面的问题。很多第三方软件，都是基于 bash 的，如果在使用的时候出现什么状况，可以分析一下是不是 fish 的兼容性问题。

在日常使用中，也要学会编写 fish 特有的脚本。不过幸运的是，fish 脚本比 bash 脚本更加易于普通人理解，它和其他现代编程语言更加接近。

## 简要使用说明

### 帮助

```fish
man set
```

### 颜色提示

1. 存在时文字蓝色，不存在时红色。

### 通配符

1. `*`: 匹配文件名的全部或者一部分
2. `**`: 匹配当前目录下的所有路径的全部或一部分

### 管道和重定向

1. `|`： 左命令通过标准输出(/dev/stdout)重定向到右命令的标准输入(/dev/stdin)
2. `>`、`<`： 重定向输出到右侧文件、读取右侧文件。`>`左侧默认是命令的标准输出，`2>`指定标准错误输出(/dev/stderr)。`>&1`表示输出到标准输出。

### 自动完成词语

1. 当你敲击命令时，fish 会提示补充完整的词，在出现提示词时，按 ctrl + f 完成单词（或按 -> 右箭头)。局部完成：alt + ->
2. tab 键激活提示，包括命令、参数、路径。

### 目录操作

1. atl + 左右箭头： 进入上一个目录或下一个目录

## 脚本编程

### 变量

1. `$变量名`: 读取变量值
2. `set 变量名 变量值`: 设置变量值
3. 双引号内可以使用变量，单引号内不能。他们都代表字符串。
4. `set -e 变量名`： 撤销变量
5. `set -x 变量名 变量值`： 设置导出变量。即子脚本可以读取的变量。
6. `set -u 变量名`： 撤销导出变量
7. 变量是一个列表，`set a $a b` 往 $a 添加新的列表项 b
8. `count $a`: 列表长度
9. `$a[1]`: 列表第 1 项，`$a[-1]`: 列表末尾项
10. `$a[1..2]`: 列表的第 1 项到第 2 项（包括）的范围切片
11. `$a"x"$a`: 列表将和相连的内容计算笛卡尔积，如$a = 1 2 3，那么结果是：1x1 2x1 3x1 1x2 2x2 3x2 1x3 2x3 3x3。当在双引号内不会计算。
12. `set -g 变量名 变量值`： 设置全局变量，即当前脚本有效
13. `set -l 变量名 变量值`： 设置局部变量
14. `set -U 变量名 变量值`： 设置通用变量，在所有 fish 脚本内有效
15. `string join \n $变量`： 变量转化为字符串，通常是用空格连接，string join 命令可以指定连接的字符(\n 即换行)。

### 执行命令

1. `(命令)`: 执行括号内的命令，输出作为参数内容插入，如 `echo (uname)` 等价于 `echo Linux`
   1. `set a (命令|string split0)`: 命令替换通常会根据换行来建立变量的多个表项，用 `string split0` 根据空字符划分表项，即最多只有一个项。
2. `命令;命令`： 在同一行可以用分号并列两个命令
3. `$status`: 上一个命令的返回值，0 为成功，非 0 为失败
4. `&&`、`||`、`!`、`and`、`or`、`not` ： 与，或，非，如 `true && false` 根据上一个命令(true)执行状态执行下一个命令(false)。与，是上个命令成功，执行下个命令，两个命令都成功，结果成功。或，是上个命令失败，执行下个命令，其中一个成功，结果成功。非，只有一个命令，如果成功，结果失败，如果失败，结果成功。and 等版本作用于上一条语句（而不是同一行的命令）。
5. 数字运算： 例 `math -s0 1+1`，math 函数支持整数和小数的加(+)、减(-)、乘(x)、除(/)、幂（^）、模(%)等数学运算。`-s0`保留 0 位小数，即保留整数。
6. `begin ... end`: 包含在 begin 和 end 间的语句，形成一个是局部段落，可以设置局部变量，可以将内部语句的输出一起重定向。
7. `builtin 命令`：强制使用内部命令，而不使用同名的自定义函数
8. `command 命令`：调用外部命令，而不使用内部命令和自定义函数
9. `eval "命令"`： 将参数转化为命令执行
10. `exec 命令`： 以指定命令替换掉当前的shell，当前shell关闭，即该命令永不返回。
11. `命令 (命令 | psub)`： 进程替换。命令替换和管道的结合，和普通命令替换的区别是最终以命名管道（文件）的方式作为参数输入，而不是文本

### 控制流程

程序是从上往下一次执行的，但是，如果只是顺序执行，程序就缺乏对环境变化的理解，因此任何成熟的程序语言，都带有控制流程的语句。如，判断语句，循环语句，函数等。

#### 判断语句

```fish
# 将 false 替换条件语句
if false
else if false
else
end
```

if ... else ... end 语句段可以添加多个 else if 判断条件 ，条件成立时执行if 和 else 之间的语句，条件不成立时，执行 else 和 end 之间的语句。也可以省略 else 段。

命令执行有成功和失败，它就是一个条件语句。

同时，fish 使用 test 命令来提供强大的判断能力。如：`test "$number" -gt 5`，判断变量 number 是否大于(-gt) 5。

数字判断：

1. -gt: >
2. -lt: <
3. -ge: >=
4. -le: <=
5. -eq: =
6. -ne: !=

文件判断：

1. -b：设备文件
2. -c: 字符设备
3. -d： 目录
4. -e： 文件存在
5. -f: 常规文件
6. -g: 文件设置了组id
7. -G: 文件属于当前用户的组
8. -k：文件设置了粘滞位
9. -L: 符号链接文件
10. -O： 文件属于当前用户
11. -p： 管道文件
12. -r： 可读
13. -w： 可写
14. -x： 可执行
15. -s： 非零文件
16. -S： 套接字文件
17. -t： 终端
18. -u： 设置用户位

字符串判断：

1. =: 两字符串是否相等
2. !=: 两字符串是否不等
3. -n: 非空字符串
4. -z: 空字符串

条件组合：

1. -a: and
2. -o: or
3. !: !

```fish
# 选项分支
switch $a
case a
    echo '$a == a'
case b
    echo '$a == b'
case c d
    echo '$a == c || $a == d'
case '*'
    echo '其他，默认选项'
end
```

switch ...case 语句，根据内容的值（$a）是否等于选项（case）来控制分支。

#### 循环语句

循环语句根据条件是否成立，重复的执行循环内部的代码。

```fish
# 死循环，不停的输出 loop
while true
    echo "loop"
end

# *.txt 匹配当前目录下的所有txt后缀的文件，组成一个列表
# for file in ... 依次读取列表中的一项，保存到 $file
# more $file 显示当前文件内容
for file in *.txt
    more $file
end
```


#### 函数

当遇到函数调用时，从当前流程跳转到函数内部执行。函数的作用等于将代码分割成若干功能的小片段。函数能够接受参数，并且能够返回值，很像数学上的函数概念，因此得名。

```fish
# 函数 say
# 参数 argv
function say
    echo hello $argv
end

# 使用函数
say jim # 输出 hello jim
echo $status # 返回值输出 0
```

定义了一个函数，就可以随意调用了。之后想获得该函数的信息：

```fish
functions say # 输出该函数的定义

functions # 输出所有已经定义的函数名
```

##### 参数

函数和脚本，都可以接受参数，对参数格式的处理，可以用 argparse 函数：

```fish
function say
# 支持参数 -h --help -n -name -s --sooo
# 其中 -n jim --name ming 可以多次输入参数
# -s9 -9 --sooo=9 都是有效参数
    argparse --name=myfunc 'h/help' 'n/name=+' 's#sooo' --$argv
    if set -q _flag_h
        echo 'say -n yourname'
        echo 'say -h help'
        echo 'say -9'
    end
    if set -q _flag_n
        set -l once 1
        echo -n 'hello '
        for var in $_flag_n
            if test $once -eq 0
                echo -n ','
            end
            set once 0
            echo -n $var
        end
        echo '!'
    end
    if set -q _flag_s
        set -l i 1
        echo -n "I'm S"
        while test $i -le $_flag_s
            echo -n 'o'
            set i (math $i+1)
        end
        echo ' happy!'
    end
end

# 使用例子
say -99 --name htqx -n xiaoming -h
```

##### 事件

函数可以处理事件。即当事件产生时，将自动调用函数。

```fish
# 处理事件
# --on-event 表示要处理的事件
function event_say --on-event say_event
    echo poker face:"$argv"
end

function say
    # 引发事件
    emit say_event hello,$argv
end
```

### 字符串处理

脚本编程的一个大对象，就是针对字符串（文本）的处理。

string 命令：

1. collect: 不拆分多行字符串，-N 不修剪多余的换行
   1. `echo a\nb\nc | string collect -N` 
      ![](https://gitee.com/deepinwiki/wiki/raw/master/pics/string%20collect.png)
2. escape: 转义字符串。 -n 不带引号
   1. --type=: var 变量转义，url 网址转义，regex 正则转义
   2. `echo "a\nb\nc" | escape ...` 分别输出：var: a_5C_nb_5C_nc; url:a%5Cnb%5Cnc; regex:`a\\nb\\nc`
3. unescape：转义还原。 --type=var,url
4. join：联接字符串，指定联接符
   1. `echo a\nb\nc | string jion ' '`：输出 a b c
5. join0:联接字符串
   1. `echo a\nb\nc | string jion0`：输出abc
6. split:拆分字符串，指定字符结尾。-r 从右到左，-n 过滤空串，-m 最多拆分次数，-f 指定列。
7. split0:拆分字符串，拆分符 \0 （但不要使用 split \0 代替）。
8. length：长度，-q 只判断
9. lower：小写, -q 只判断
10. upper：大写，-q 只判断
11. pad：填充。-r 右侧填充，-c 填充字符，-w 总宽度。
    1.  `echo ab\nc |string pad -c x`
        ![](https://gitee.com/deepinwiki/wiki/raw/master/pics/string%20pad.png)
12. repeat：重复字符串。-n 重复次数，-m 最大字数，-N 最后不换行。
13. replace：替换匹配内容。用 $1 捕获相应组。 
14. trim：修剪空格，-l 左空格，-r 右空格，-c 修剪符。
15. sub： 字串。-s 开始位置，-e 结束位置，-l 长度。
16. match：多组字符串进行匹配，默认是完全匹配某一项才输出对应项。
    1.  -a：全部测试匹配性，而不是匹配一个就结束
    2.  -e: 某一项匹配一部分，输出整个项。
    3.  -i：忽略大小写
    4.  -n：输出匹配项的匹配位置的范围，如输出：1 6
    5.  -r：正则匹配，输出匹配的部分,而不是整项。正则支持 perl 模式。
    6.  -v：反转匹配
        ![](https://gitee.com/deepinwiki/wiki/raw/master/pics/2021-04-09-13-16-19.png)

### 内置函数

1. `abbr -ag ll ls -l`: 将 ls -l 用 ll 缩写代替
2. `alias ll="ls -l"`： 用 ll 函数包装 ls -l
3. argparse: 对参数文本进行解析，包括三部分，选项，规范，参数。
   1. -n： 函数名（用于错误输出）
   2. -x： 独占
   3. -N： 最小参数个数
   4. -X： 最大参数个数
   5. -i： 忽略未知参数
   6. -s： 遇到未知参数立即停止
   7. 规范：从参数中搜索，找到并生成规范对应的局部变量_flag_x。
      1. 'n/name': 生成局部变量_flag_n 和 _flag_name，能处理选项 -n 和 --name
      2. 'n/name=': 接受选项值，如 -n9 和 --name 9 和 --name=9
      3. 'n/name=?': 选项值可选
      4. ‘n/name=+': 可多次使用选项，保存多个值
      5. `'n#max'`: -99 等
      6. 'n/name=!验证函数`：对参数值进行验证
         1. _argparse_cmd : 被验证的函数名
         2. _flag_name: 当前验证的选项
         3. _flag_value: 当前参数值
4. bg、fg、jobs、disown： 任务放后台、转到前台、任务状态、分离任务
5. bind：快捷键绑定，ctrl(\c),alt(Meta),esc(\e)等
6. block：屏蔽事件
7. breakpoint: 调试断点
8. `cd` 改变目录、`cdh` 通过最近访问目录列表改变目录、`dirh` 目录记录、`prevd` 上一个目录、`netxd` 下一个目录、`pushd` 入栈当前目录并进去指定目录、`popd` 出栈目录、`pwd` 显示当前目录
9. complete： 定义自动完成词（补全）
   1.  -c / -p: 命令或者路径，没有其他参数时返回当前补全定义
   2.  -s / -l: 短选项和长选项
   3.  -a: 参数
   4.  -F / -f: 文件名参数 或 非文件名
   5.  -r： 需要参数
   6.  -w： 别名
   7.  -n ： 判断条件
   8.  -C ： 搜索相关补全
   9.  -d ： 说明描述
   10.  -e ： 删除补全
   11.  -x : -r -f 的组合
10. `contains key I have key`：查询列表中（I have key）是否包含指定项（key）
11. count: 列表（即变量）的项总数
12. exit: 退出当前 shell
13. fish_add_path: 添加搜索路径，即 $PATH
14. fish_config: 启动 fish 配置网页
15. fish_is_root_user: 检查当前用户是否 root
16. fish_key_reader: 交互式输出按键的 bind 绑定形式
17. fish_update_completions: 解析系统自带的补全
18. funcsave: 保存自定义函数 ~/.config/fish/functions/xxx.fish
19. isatty: 文件描述符是否终端
20. open: 类似 xdg-open ，用关联的程序打开不同类型的文件
21. printf： 打印格式化文本
    1.  %d、%s：数字、文本
    2.  %o、%x：八进制、十六进制
    3.  %f、%g、%e: 六位小数浮点、浮点、科学计数
22. `echo -n -e “a\nb”`： 输出文本，-n 结束不换行，-e 识别转义(\n)
23. random: 0~32767 的随机数
24. read : 读取用户输入
    1.  -g、-l、-u、-x、-U：存储的变量的范围
        2〔方案選單〕 .  -d、-t、-a、-L：指定分隔符、按toke、列表、多行列表
    2.  -s、-S：密码模式、语法高亮
    3.  -n、-z：行标记、空字符行标记
25. `realpath -s txt`：将相对路径转换为绝对路径，-s 保留符号链接文件
26. set：显示和设置变量，变量是列表，可以多个值
    1.  -a、-p：添加值到末尾、添加到头部
    2.  -l：局部变量（可屏蔽更大范围的同名变量）
    3.  -g：全局变量
    4.  -U: 通用变量（多个 fish shell 共享）
    5.  -x：导出变量（环境变量）
    6.  -u：取消导出
    7.  --path：路径变量（以:分隔列表项，而不是空格）
    8.  --unpath：取消路径模式
    9.  -e：取消变量
    10.  -q: 判断是否定义了变量
    11.  -n：所有已经定义的变量名的列表
    12.  -S: 显示变量的定义信息
    13.  `变量[2]`:可以使用下标对变量的表项进行操作
27. set_color:设置颜色
    1.  -b：背景色
    2.  -c：输出安全颜色名
    3.  -o：粗体
    4.  -d：黯淡
    5.  -i：斜体
    6.  -r：反转前景和背景色
    7.  -u：下划线
28. source:导入脚本
29. status： 查询运行时状态
30. suspend: 挂起 shell
31. time：命令运行需要的时间，$CMD_DURATION 结果。
32. trap: 收到外部信号响应
33. type: 返回名称的类型
34. ulimit：限制资源
35. umask：文件创建掩码
36. vared:交互式修改变量
37. wait：等待后台任务完成

