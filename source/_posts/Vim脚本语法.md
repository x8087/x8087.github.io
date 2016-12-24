title: Vim脚本语法
date: 2016-12-15 00:13:14
tags: 
  - 笔记
  - vim

---
vim7.4脚本相关帮助笔记
<!-- more -->
# 定义变量 #
```vimscript
:let {变量} = {表达式}

```
# 流程控制 #
## 循环 ##
### while ###
```vimscript
:while {条件}
:  {语句}
:endwhile
```
### for in ###
```vimscript
:for {变量} in {列表}
:  {语句}
:endfo[r]

:for [{变量1}, {变量2}, ...] in {二维列表}
:  {语句}
:endfo[r]
```

# 变量 #
变量名包含ASCII字符，数字和下划线，不能以数字开头。
`:let`查看当前定义变量列表。
`:let {变量} = {表达式}`    定义变量
`:unlet {变量}`             删除变量
`:unlet! {变量}`            删除变量，不管变量是否存在，不需要报错信息。
`exist("{变量}")`           检测变量是否存在，参数是检测的变量名。
true为非0，false为0
vim在查找数字时自动转换字符串为数字，当字符串不以0开头时，结果为0。
`type()`获取变量类型
特殊变量：
- 'viminfo'选项包含'!'标志时，以大写字符开头且不包含小写字符的全局变量保存在viminfo-file
- 'sessionoptions'选项包含"global"时，以大写字符开头且至少包含一个小写字符的全局变量保存在session-file
## 变量类型 ##
Vim支持的基本类型有数字和字符串。变量类型是动态的，每次使用`:let`给变量赋值时设置类型。

### 数字 ###
- *十进制* 不以"0"开头数字。
- *十六进制* 以"0x"或者"0X"开头。
- *八进制* 以"0"开头。
- 负数在前面加负号 
注:**表达式忽略空格**

### 字符串 ###
#### 字符串常量 ####
##### 使用双引号 #####
- 字符串内部包含双引号时使用反斜杠转义`\"`
- 特殊字符使用反斜杠转义

A string constant accepts these special characters:
\...	three-digit octal number (e.g., "\316")
\..	two-digit octal number (must be followed by non-digit)
\.	one-digit octal number (must be followed by non-digit)
\x..	byte specified with two hex numbers (e.g., "\x1f")
\x.	byte specified with one hex number (must be followed by non-hex char)
\X..	same as \x..
\X.	same as \x.
\u....	character specified with up to 4 hex numbers, stored according to the
	current value of 'encoding' (e.g., "\u02a4")
\U....	same as \u but allows up to 8 hex numbers.
\b	backspace <BS>
\e	escape <Esc>
\f	formfeed <FF>
\n	newline <NL>
\r	return <CR>
\t	tab <Tab>
\\	backslash
\"	double quote
\<xxx>	Special key named "xxx".  e.g. "\<C-W>" for CTRL-W.  This is for use
	in mappings, the 0x80 byte is escaped.
	To use the double quote character it must be escaped: "<M-\">".
	Don't use <Char-xxxx> to get a utf-8 character, use \uxxxx as
	mentioned above.

##### 使用单引号 #####
- 所有字符除单引号外都不需要转义，使用两个单引号表示自己`''`
### 列表 ###
- 有序序列
- 序列项可以为任何类型，甚至混合使用
- *创建列表* `:let alist = [{item}, {item}, {item}....]`
- *添加项* `add({list}, {item})`
- *连接或扩展列表* `{list}+{list2}` 或者 `call extend({list}, {list2})`
- *拷贝* `copy()` 或者 `[:]`
- *深拷贝* `deepcopy()`
- *判断是否同一个list* `is`和`isnot`
- *判断两个列表是否包含相同值* `==`，当长度相同且所有项使用`==`对比后相等时为真。
注意：字符串和数字比较时不会自动类型转换，视为不相等项。
- 列表比较比数字与字符串比较要严格
- *列表解包* `:let [var1, var2] = mylist`（当列表长度与变量数不对应时会报错），使用`:let [var1, var2; rest] = mylist`可以将额外项放到rest，避免报错。
- *列表修改* `:let list[4] = "four"`,`:let listlist[0][3] = item`
- *列表部分修改* `:let list[3:5] = [3, 4, 5]
- 从列表添加和移除项：
examples: >
```
	:call insert(list, 'a')		" prepend item 'a'
	:call insert(list, 'a', 3)	" insert item 'a' before list[3]
	:call add(list, "new")		" append String item
	:call add(list, [1, 2])		" append a List as one new item
	:call extend(list, [1, 2])	" extend the list with two more items
	:let i = remove(list, 3)	" remove item 3
	:unlet list[3]			" idem
	:let l = remove(list, 3, -1)	" remove items 3 to last item
	:unlet list[3 : ]		" idem
	:call filter(list, 'v:val !~ "x"')  " remove items with an 'x'
```

Changing the order of items in a list: >
```
	:call sort(list)		" sort a list alphabetically
	:call reverse(list)		" reverse the order of items
	:call uniq(sort(list))		" sort and remove duplicates
```

- 列表方法：
```
	:let r = call(funcname, list)	" call a function with an argument list
	:if empty(list)			" check if list is empty
	:let l = len(list)		" number of items in list
	:let big = max(list)		" maximum value in list
	:let small = min(list)		" minimum value in list
	:let xs = count(list, 'x')	" count nr of times 'x' appears in list
	:let i = index(list, 'x')	" index of first 'x' in list
	:let lines = getline(1, 10)	" get ten text lines from buffer
	:call append('$', lines)	" append text lines in buffer
	:let list = split("a b c")	" create list from items in a string
	:let string = join(list, ', ')	" create string from list items
	:let s = string(list)		" String representation of list
	:call map(list, '">> " . v:val')  " prepend ">> " to each item
```

### 字典 ###
- 无序序列，键值对
- *创建列表* `:let {varname} = {<key> : <value>, ...}`
- *访问字典项* `:echo {varname}[{key}]`或者`:echo {varname}.{key}`
- *字典方法* `:function {dictname}.{funcname}({param}) dict`方法内部可以通过`self`访问字典本身
- `keys()` `values()` `items()`
- 字典相关方法
	:if has_key(dict, 'foo')	" TRUE if dict has entry with key "foo"
	:if empty(dict)			" TRUE if dict is empty
	:let l = len(dict)		" number of items in dict
	:let big = max(dict)		" maximum value in dict
	:let small = min(dict)		" minimum value in dict
	:let xs = count(dict, 'x')	" count nr of times 'x' appears in dict
	:let s = string(dict)		" String representation of dict
	:call map(dict, '">> " . v:val')  " prepend ">> " to each item
## 变量范围 ##
`name`     在函数内为函数本地变量，否则为全局变量
`s:name`   脚本变量
`b:name`   本地缓存变量。其中b:changedtick为预定义变量，表示当前缓存改变总数。
`w:name`   本地窗口变量
`t:name`   本地标签变量
`g:name`   全局变量
`l:name`   函数本地变量
`a:name`   函数参数（只在函数内有效）
`v:name`   vim预定义变量

变量范围名可以当作字典使用。例如：
```
for k in keys(s:)
  unlet s:[k]
endfor
```
## 花括号名称 ##
在大多数可以使用变量的地方，可以使用”花括号各称”变量。这是具有一个或者多个表达式的常规变量名包裹在一对花括号内，如：`my_{varname}_variable`
当vim遇到这种情况时，会计算括号内的表达式，代替表达式位置，并将整个重新解释为一个变量名。
可以使用选项值，如：`my_{&background}_message`
可以使用多对花括号。
可以嵌套使用。
注：最终解悉后的变量名必须为有效的变量名。
# 表达式 #
- 表达式的基本项有数字，字符串，变量，$Name(环境变量), &name(选项), @r(寄存器)
|expr1|   expr2
	expr2 ? expr1 : expr1	if-then-else

|expr2|	expr3
	expr3 || expr3 ..	logical OR

|expr3|	expr4
	expr4 && expr4 ..	logical AND

|expr4|	expr5
	expr5 == expr5		equal
	expr5 != expr5		not equal
	expr5 >	 expr5		greater than
	expr5 >= expr5		greater than or equal
	expr5 <	 expr5		smaller than
	expr5 <= expr5		smaller than or equal
	expr5 =~ expr5		regexp matches
	expr5 !~ expr5		regexp doesn't match

	expr5 ==? expr5		equal, ignoring case
	expr5 ==# expr5		equal, match case
	etc.			As above, append ? for ignoring case, # for
				matching case

	expr5 is expr5		same |List| instance
	expr5 isnot expr5	different |List| instance

|expr5|	expr6
	expr6 +	 expr6 ..	number addition or list concatenation
	expr6 -	 expr6 ..	number subtraction
	expr6 .	 expr6 ..	string concatenation

|expr6|	expr7
	expr7 *	 expr7 ..	number multiplication
	expr7 /	 expr7 ..	number division
	expr7 %	 expr7 ..	number modulo

|expr7|	expr8
	! expr7			logical NOT
	- expr7			unary minus
	+ expr7			unary plus

|expr8|	expr9
	expr8[expr1]		byte of a String or item of a |List|
	expr8[expr1 : expr1]	substring of a String or sublist of a |List|
	expr8.name		entry in a |Dictionary|
	expr8(expr1, ...)	function call with |Funcref| variable

|expr9|   number		number constant
	"string"		string constant, backslash is special
	'string'		string constant, ' is doubled
	[expr1, ...]		|List|
	{expr1: expr1, ...}	|Dictionary|
	&option			option value
	(expr1)			nested expression
	variable		internal variable
	va{ria}ble		internal variable with curly braces
	$VAR			environment variable
	@r			contents of register 'r'
	function(expr1, ...)	function call
	func{ti}on(expr1, ...)	function call with curly braces
	{args -> expr1}		lambda expression


".." 表示此级别的操作可连接
Example: >
	&nu || &list && &shell == "csh"

一个级别内的所有表达式从左到右解析
# 条件 #
## 三种条件语句 ##
```
:if {condition}
  {statements}
:endif

if {condition}
  {statements}
:else
  {statements}
:endif

:if {condition}
  {statements}
:elseif {condition}
  {statements}
:else
  {statements}
:endif
```
## 逻辑操作 ##
### 数字和字符串 ###
a == b等于
a != b不等于
a >  b大于
a >= b大于或等于
a < b 小于
a <= b小于或等于

- 如果条件满足结果为1，否则为0
- 逻辑运算符同时用于数字和字符串。 
- 比较两个字符串时，使用数学差异。 这是比较字节值，可能不适合某些语言。
- 当将字符串与数字进行比较时，字符串首先转换为数字。 这有点棘手，因为当一个字符串不像一个数字时，使用数字零。

### 字符串 ###
a =~ b 匹配
a !~ b 不匹配
`#` 匹配大小写
`?` 忽略大小写

### 更多循环 ###
`:continue` 跳转回到循环开始继续循环
`:break` 跳转到`:endwihile`，不继续循环

# 执行表达式 #
- `:execute`命令允许执行表达式结果，这是构建并执行命令的一种非常强大的方法。
- `:execute`命令只能执行冒号命令。
- `:normal`命令执行普通模式命令，但参数不能为表达式。
- `:execute "normal " . normal_commands`实现允许执行表达式结果普通模式命令。
- `eval()`函数只计算获取表达式的值而不执行。

# 函数 #

## 调用函数 ##

- 使用`:call`命令调用函数。 在括号之间传入参数用逗号分隔。
- 函数能在表达式里调用

## 删除函数 ##
- `:delfunction {name}`函数不存在时报错

## 函数引用 ##
- `function()`这个函数可以把函数名转化成函数引用
- 注意：为了与内置函数区分开来，函数引用的变量首字母也要大写
- `call({funcRef}, {paramlist})`通过这个函数可以调用函数引用

## 定义函数 ##

```
:function {name}({var1}, {var2}, ...)
:  {body}
:endfunction
```
- 函数名必须首字符大写
- 函数执行到`:endfunction`或者`:return`没有参数时，返回0
- `:function!`重定义已存在的函数

### 使用范围 ###
- `:call`命令可以给一个行范围。
- 当函数用`range`关键字定义时，它会只关心行范围本身。
- 函数传递变量`a:firstline`和`a:lastline`，拥有函数调用范围的行数字。
- 另一种使用行范围的方法是定义不带`range`关键字的函数。函数对范围内的每行调用一次，光标在该行。
```
:function Number()
: echo "line " . line(".") . " contains: " . getline(".")
:endfunction
```

### 可变数量参数 ###
- Vim允许你定义带可变数量参数的函数
- `a:1`包含第一个可选参数, `a:2`第二个，以此类推。`a:0`包含额外参数数量。
- `a:000`所有`...`参数的列表

### 列出函数 ###
- `:function`列出所有用户定义函数的名字和参数
- `:function {funcname}`查看函数实现

### 调试 ###
#### 进入调试模式 ####
- `vim -D file.txt` 使用 `-D`参数打开vim
- 调用带`:debug`前缀的命令。调试只有当这个命令执行时有效。用于调试特定脚本或者用户函数，以及自动命令使用的脚本和函数。
- 在源文件或者用户函数中设置断点。你可以在命令行里这样做：`vim -c "breakadd file */explorer.vim"。
- 在调试模式，每条执行命令在它执行前显示。跳过空行和没有执行的行。当一行内包含两条命令，由`|`分开，每条命令分别显示。
- 在调试模式下可以使用平常的Ex命令。
#### 调试模式下的命令 ####
- `cont`
- `quit`
- `next`
- `step`
- `interrupt`
- `finish`
- `backtrace``bt``where`
- `frame N`
- `up`
- `down`
- 不会自动补全，自动补全只对Ex命令有效
- 可以缩短输入的字符，除了相同字符开始的命令需要输入足够字符区分。f=finish,fr=frame
- 点击<CR>重复上个命令，当运行另一个命令时，重置。
- 当需要使用相同名字的Ex命令时，前面添加冒号。
#### 断点 ####
##### 添加断点 #####
- `:breaka[dd] func [lnum] {name}` 在函数里设置断点，不会验证函数名有效性，因此可以在函数定义前添加断点。
- `:breaka[dd] file [lnum] {name}` 在源文件设置断点
- `:breaka[dd] here` 在当前文件当前行设置断点。
- `[lnum]`是断点的行数字，缺省为1
- `{name}`匹配文件或者函数名的模式。不指定目录则使用当前目录。
##### 删除断点 #####
- `:breakd[el] {nr}`删除断点{nr}， 使用`:breaklist`查看每个断点的数值
- `:breakd[el] \*`删除所有断点
- `:breakd[el] func [lnum] {name}`删除函数内断点
- `:breakd[el] file [lnum] {name}`删除源文件断点
- `:breakd[el] here`删除当前文件当前行断点
- `[lnum]`缺省为第一个断点位置
##### 列出断点 #####
- `:breakl[ist]` 列出所有断点
## 函数列表 ##

### 字符串操作：###
nr2char（）通过其ASCII值获取字符
char2nr（）获取字符的ASCII值
str2nr（）将字符串转换为数字
str2float（）将字符串转换为Float
printf（）根据％项格式化一个字符串
escape（）转义一个字符串中带有'\'的字符
shellescape（）转义一个用于shell命令的字符串
fnameescape（）转义用于Vim命令的文件名
tr（）将字符从一个集翻译成另一个集
strtrans（）翻译一个字符串，使其可打印
tolower（）将字符串转换为小写
toupper（）将字符串转换为大写
match（）位置，其中模式在字符串中匹配
matchend（）位置，其中模式匹配以字符串结尾
matchstr（）匹配字符串中的模式
matchstrpos（）匹配和字符串中模式的位置
matchlist（）like matchstr（）并返回子匹配
stridx（）长字符串中的短字符串的第一个索引
strridx（）一个长字符串中的一个短字符串的最后一个索引
strlen（）字符串的长度（以字节为单位）
strchars（）字符串的长度
strwidth（）显示时字符串的大小
strdisplaywidth（）显示时字符串的大小，处理制表符
substitute（）用一个字符串替换模式匹配
submatch（）在“：s”和substitute（）中获取特定的匹配项，
strpart（）使用字节索引获取字符串的一部分
strcharpart（）使用char索引获取字符串的一部分
strgetchar（）使用char索引从字符串中获取字符
expand（）展开特殊关键字
iconv（）将文本从一个编码转换为另一个
byteidx（）字符串中字符的字节索引
byteidxcomp（）like byteidx（）但计算构成字符
repeat（）重复一个字符串多次
eval（）计算字符串表达式
execute（）执行Ex命令并获取输出
### 列表操作： ###
get（）得到一个没有错误的项目，错误的索引
len（）列表中的项目数
empty（）检查List是否为空
insert（）在List中的某个位置插入一个项目
add（）将一个项目附加到List
extend（）将列表附加到列表
remove（）从列表中删除一个或多个项目
copy（）创建一个List的浅拷贝
deepcopy（）创建一个列表的完整副本
filter（）从列表中删除所选项目
map（）更改每个List项
sort（）排序列表
reverse（）反转List的顺序
uniq（）删除重复的邻近项目的副本
split（）将一个String拆分成一个List
join（）将列表项目转换为String
range（）返回一个包含数字序列的List
string（）List的字符串表示形式
call（）调用List作为参数的函数
index（）列表中值的索引
max（）最大值
min（）最小值
count（）count一个值出现在列表中的次数
repeat（）重复一个List多次
### 字典操作： ###
get（）获取一个条目，没有错误的错误键
len（）在Dictionary中的条目数
has_key（）检查一个键是否出现在Dictionary中
empty（）检查Dictionary是否为空
remove（）从Dictionary中删除一个条目
extend（）将一个字典中的条目添加到另一个
filter（）从Dictionary中删除选定的条目
map（）更改每个Dictionary条目
keys（）获取字典键列表
values（）获取字典值列表
items（）获取字典键值对列表
copy（）做一个字典的浅拷贝
deepcopy（）创建一个字典的完整副本
string（）字典的字符串表示形式
max（）在Dictionary中的最大值
min（）最小值
count（）count一个值出现的次数
### 浮点计算 ###
float2nr（）将Float转换为Number
abs（）绝对值（也适用于Number）
round（）舍入
ceil（）向上舍入
floor（）向下舍入
trunc（）删除小数点后的值
fmod（）除法的余数
exp（）指数
log（）自然对数（以e为底的对数）
log10（）对数以10为底
pow（）值x到指数y
sqrt（）平方根
sin（）正弦
cos（）余弦
tan（）切线
asin（）反正弦
acos（）反余弦
atan（）反正切
atan2（）反正切
sinh（）双曲正弦
cosh（）双曲余弦
tanh（）双曲正切
isnan（）检查不是一个数字
### 其它计算 ###
and（）按位与
invert（）按位反转
or（）按位或
xor（）按位异或
sha256（）SHA-256哈希
### 变量 ###
type（）类型的变量
islocked（）检查变量是否被锁定
function（）获取函数名的Funcref
getbufvar（）从特定缓冲区获取变量值
setbufvar（）在一个特定的缓冲区中设置一个变量
getwinvar（）从特定窗口获取变量
gettabvar（）从特定标签页获取一个变量
gettabwinvar（）从特定窗口和标签页获取一个变量
setwinvar（）在一个特定的窗口中设置一个变量
settabvar（）在一个特定的标签页中设置一个变量
settabwinvar（）在一个特定的窗口和标签页中设置一个变量
garbagecollect（）尽可能释放内存

### 光标和标志位置 ###

col（）列号或一个标记
virtcol（）屏幕列的光标或标记
line（）行号的光标或标记
wincol（）窗口列的光标号
winline（）窗口中光标的行号
cursor（）将光标定位在一行/列
screencol（）获取屏幕列的光标
screenrow（）获取光标的屏幕行
getcurpos（）获取光标的位置
getpos（）获取光标，标记等的位置。
setpos（）设置光标，标记等的位置
byte2line（）获取特定字节计数的行号
line2byte（）在特定行的字节计数
diff_filler（）获取行之上的填充行数
screenattr（）获取屏幕线/行的属性
screenchar（）在屏幕行/行获取字符代码

### 使用当前缓冲区的文本 ###

getline（）从缓冲区中获取行或行的列表
setline（）替换缓冲区中的一行
append（）在缓冲区中追加行或行列表
indent（）缩进特定行
cindent（）缩进根据C缩进
lispindent（）缩进根据Lisp缩进
nextnonblank（）找到下一个非空行
prevnonblank（）找到上一个非空行
search（）找到一个模式的匹配
searchpos（）找到一个模式的匹配
searchpair（）找到另一端的开始/跳过/结束
searchpairpos（）找到开始/跳过/结束的另一端
searchdecl（）搜索名称的声明
getcharsearch（）返回字符搜索信息
setcharsearch（）设置字符搜索信息

### 系统功能和文件操作 ###

glob（）展开通配符
globpath（）在许多目录中展开通配符
glob2regpat（）将glob模式转换为搜索模式
findfile（）在目录列表中找到一个文件
finddir（）在目录列表中找到一个目录
resolve（）找出快捷方式指向的位置
fnamemodify（）修改文件名
pathshorten（）缩短路径中的目录名
simplify（）简化路径而不改变其含义
executable（）检查可执行程序是否存在
exepath（）可执行程序的完整路径
filereadable（）检查是否可以读取文件
filewritable（）检查是否可以写入文件
getfperm（）获取文件的权限
setfperm（）设置文件的权限
getftype（）获取文件的种类
isdirectory（）检查目录是否存在
getfsize（）获取文件的大小
getcwd（）获取当前工作目录
haslocaldir（）检查当前窗口是否使用|：lcd |
tempname（）获取临时文件的名称
mkdir（）创建一个新目录
delete（）删除文件
rename（）重命名文件
system（）获取shell命令的结果作为字符串
systemlist（）获取shell命令的结果作为列表
hostname（）系统的名称
readfile（）将一个文件读入一个List行
writefile（）将一行List写入一个文件

### 日期和时间 ###

getftime（）获取文件的最后修改时间
localtime（）获取当前时间（秒）
strftime（）将时间转换为字符串
reltime（）准确地获取当前或逝去的时间
reltimestr（）将reltime（）的结果转换为字符串
reltimefloat（）将reltime（）结果转换为Float

### 缓冲区，窗口和参数列表 ###

argc（）参数列表中的条目数
argidx（）当前位置在参数列表中
arglistid（）获取参数列表的id
argv（）从参数列表中获取一个条目
bufexists（）检查是否存在缓冲区
buflisted（）检查是否存在并列出了缓冲区
bufloaded（）检查缓冲区是否存在并且已加载
bufname（）获取特定缓冲区的名称
bufnr（）获取特定缓冲区的缓冲区号
tabpagebuflist（）return标签页中的缓冲区列表
tabpagenr（）获取标签页的编号
tabpagewinnr（）像 winner（）用于特定标签页
winner（）获取当前窗口的窗口编号
bufwinid（）获取特定缓冲区的窗口ID
bufwinnr（）获取特定缓冲区的窗口编号
winbufnr（）获取特定窗口的缓冲区号
getbufline（）从指定的缓冲区中获取行的列表
win_findbuf（）查找包含缓冲区的窗口
win_getid（）获取窗口的窗口ID
win_gotoid（）转到带有ID的窗口
win_id2tabwin（）从窗口ID获取选项卡和窗口nr
win_id2win（）从窗口ID获取窗口nr
getbufinfo（）获取包含缓冲区信息的列表
gettabinfo（）获取带有标签页信息的列表
getwininfo（）获取带有窗口信息的列表

### 命令行 ###

getcmdline（）获取当前命令行
getcmdpos（）获取光标在命令行中的位置
setcmdpos（）设置光标在命令行中的位置
getcmdtype（）返回当前的命令行类型
getcmdwintype（）返回当前命令行窗口类型
getcompletion（）命令行完成匹配列表

### 快速修复和位置列表 ###

getqflist（）quickfix错误列表
setqflist（）修改quickfix列表
getloclist（）列出位置列表项
setloclist（）修改位置列表

### 插入模式补全 ###

complete（）设置找到的匹配项
complete_add（）添加找到的匹配项
complete_check（）检查是否应该中止完成
pumvisible（）检查是否显示弹出菜单

### 折叠 ###

foldclosed（）检查特定行处的折叠
foldclosedend（）像 foldclosed（）但返回最后一行
foldlevel（）检查特定行的折叠级别
foldtext（）生成为封闭折叠显示的线
foldtextresult（）获取关闭折叠显示的文本

### 语法和高亮 ###

clearmatches（）清除由| matchadd（）|定义的所有匹配 和 |：match | 命令
getmatches（）获取由| matchadd（）|定义的所有匹配 和 |：match | 命令
hlexists（）检查高亮组是否存在
hlID（）获取高亮组的ID
synID（）获取特定位置的语法ID
syndICate（）获取语法ID的特定属性
synIDtrans（）获取翻译的语法ID
synstack（）获取特定位置的语法ID列表
synconcealed（）获取有关隐藏的信息
diff_hlID（）获取位置处diff模式的高亮ID
mathcad（）定义一个模式来突出显示（“匹配”）
matchaddpos（）定义要突出显示的位置列表
matcharg（）获取有关|：match |的信息 参数
matchdelete（）删除由| matchadd（）|定义的匹配 或 |：match | 命令
setmatches（）恢复由保存的匹配列表 | getmatches（）|

### 拼写 ###

spellbadword（）定位在光标处或之后的拼写错误的单词
spellsuggest（）返回建议的拼写更正
soundfold（）返回一个词的声音a类似的等价物

### 历史 ###

histadd（）向历史添加项目
histdel（）从历史记录中删除项目
histget（）从历史记录中获取项目
histnr（）获取历史列表的最高索引

### 互动 ###

browse（）放置一个文件请求者
browsedir（）放了一个目录请求者
confirm（）让用户做出选择
getchar（）从用户获取一个字符
getcharmod（）获取最后一个输入字符的修饰符
feedkeys（）将字符放在预先排队的队列中
input（）从用户获取一行
inputlist（）让用户从列表中选择一个条目
inputsecret（）从用户获取一行而不显示它
inputdialog（）在对话框中从用户获取一行
inputsave（）保存和清除typeahead
inputrestore（）恢复typeahead

### GUI ###

getfontname（）获取当前正在使用的字体的名称
getwinposx（）GUI Vim窗口的X位置
getwinposy（）Y GUI的Vim窗口的位置

### VIM服务器 ###

serverlist（）返回服务器名称的列表
remote_send（）向Vim服务器发送命令字符
remote_expr（）计算Vim服务器中的表达式
server2client（）向Vim服务器的客户端发送回复
remote_peek（）检查是否有来自Vim服务器的回复
remote_read（）从Vim服务器读取回复
foreground（）将Vim窗口移动到前台
remote_foreground（）将Vim服务器窗口移动到前台

### 窗口大小和位置 ###

winheight（）获取特定窗口的高度
winwidth（）获取特定窗口的宽度
winrestcmd（）return命令恢复窗口大小
winsaveview（）获取当前窗口的视图
winrestview（）恢复当前窗口的保存视图

### 映射 ###

hasmapto（）检查是否存在映射
mapcheck（）检查是否存在匹配的映射
maparg（）获取映射的rhs
wildmenumode（）检查wildmode是否处于活动状态

### 测试 ###

assert_equal（）断言两个表达式的值相等
assert_notequal（）断言两个表达式的值不相等
assert_inrange（）断言表达式在范围内
assert_match（）断言模式匹配该值
assert_notmatch（）断言模式与值不匹配
assert_false（）断言表达式为false
assert_true（）断言表达式为true
assert_exception（）断言命令抛出异常
assert_fails（）断言函数调用失败
test_alloc_fail（）使内存分配失败
test_autochdir（）在启动期间启用'autochdir'
test_disable_char_avail（）测试没有typeahead
test_garbagecollect_now（）可用内存
test_null_channel（）返回一个null通道
test_null_dict（）返回一个空Dict
test_null_job（）返回一个空作业
test_null_list（）返回一个空列表
test_null_partial（）返回一个零部分函数
test_null_string（）返回一个空字符串

### 进程间通信 ###

ch_open（）打开一个通道
ch_close（）关闭一个通道
ch_read（）从通道读取消息
ch_readraw（）从通道读取原始消息
ch_sendexpr（）通过通道发送JSON消息
ch_sendraw（）通过通道发送原始消息
ch_evalexpr（）通过通道评估表达式
ch_evalraw（）通过通道评估原始字符串
ch_status（）获取通道的状态
ch_getbufnr（）获取通道的缓冲区号
ch_getjob（）获取与通道相关联的作业
ch_info（）获取通道信息
ch_log（）在通道日志文件中写入一条消息
ch_logfile（）设置通道日志文件
ch_setoptions（）设置通道的选项
json_encode（）将表达式编码为JSON字符串
json_decode（）将JSON字符串解码为Vim类型
js_encode（）将表达式编码为JSON字符串
js_decode（）将JSON字符串解码为Vim类型

### Jobs ###

job_start（）启动作业
job_stop（）停止作业
job_status（）获取作业的状态
job_getchannel（）获取作业使用的通道
job_info（）获取有关作业的信息
job_setoptions（）设置作业的选项

### 定时器 ###

timer_start（）创建一个定时器
timer_pause（）暂停或取消暂停计时器
timer_stop（）停止定时器
timer_stopall（）停止所有计时器
timer_info（）获取有关计时器的信息

### 各种 ###

mode（）获取当前编辑模式
visualmode（）最后使用的可视模式
exists（）检查是否存在变量，函数等
has（）检查Vim中是否支持特性
changenr（）返回最近更改的数字
cscope_connection（）检查cscope连接是否存在
did_filetype（）检查是否使用了FileType自动命令
eventhandler（）检查是否由事件处理程序调用
getpid（）获取Vim的进程ID
libcall（）调用外部库中的函数
lincoln（）idem，返回一个数字
undofile（）获取撤销文件的名称
undotree（）返回撤销树的状态
getreg（）获取寄存器的内容
getregtype（）获取寄存器的类型
setreg（）设置寄存器的内容和类型
shiftwidth（）有效值'shiftwidth'
wordcount（）获取缓冲区的字节/字/ char计数
taglist（）获取匹配标签的列表
tagfiles（）获取标签文件的列表
luaeval（）评估Lua表达式
mzeval（）evaluate | MzScheme | 表达
perleval（）计算Perl表达式（| + perl |）
py3eval（）评估Python表达式（| + python3 |）
pyeval（）求值Python表达式（| + python |）

# 异常(Exceptions) #
```
:try
: read ~/templates/pascal.tmpl
:catch /E484:/
 echo "sorry, ..."
:endtry
```
- 异常是一个字符串，每个错误信息都有一个固定数值
- 通过匹配模式处理对应异常
- 匹配模式为空表示捕捉所有异常
- `:finally`机制，不管是否有错误或者退出都会执行到

# 各种设定 #
## 行结束符 ##

系统  | 符号
:-|-:
Unix | <NL>
MS-DOS,windows,OS/2 | <CR><LF>

## 空白字符 ##
- 允许并忽略空行
- 前导空白字符所有忽略
- 参数间的空白字符它缩减为一个并作为分隔字符使用
- 最后一个字符后的空白字符根据情况可能忽略也可能不忽略
- `=`前的空白忽略，但后面则不
- 选项中包含空白字符使用反斜杠转义

## 注释 ##
- 字符`"`表示注释。`"`后面包括该字符直到行尾都认为是注释并忽略，除非命令不认为是注释。
- 注释能在行的任何字符位置开始
- 以下命令需要通过`|`分隔成两个命令处理，否则注释会被认为是命令的一部分
```
:abbrev dev development|" shorthand
:map <F3> o#include|" insert include
:excute cmd         |" do it
:exe '!ls *c'       |" list C files
```

# 编写插件 #
## 插件类型 ##
- 全局插件(global) 
- 文件类型插件(filetype) 
## 插件的基本组成 ##
### 名字 ###
- 不能超过8个字符,避免在旧的windows系统上出问题
### 头部 ###
```
" Vim global plugin for correcting typing mistakes
" Last Change:  2000 Oct 15
" Maintainer: Bram Moolenaar <Bram@vim.org>
" License:	This file is placed in the public domain.
" copyright
```
## 注意 ##
### 续行与避免边界效果 ###
- `compatible`选项设置后续行机制运行会出问题，不能只是重置`compatible`，因为那样有很多边界效果。避免这个，设置`cpoptions`选项为vim默认值并稍候恢复它。
### 不加载 ###
```
if exists("g:loaded_typecorr")
  finish
endif
let g:loaded_typecorr = 1
```
### 映射 ###
```
map <unique> <Leader>a <Plug>TypecorrAdd
```
- 使用`<unique>`，如果映射已存在时报错
```
if !hasmapto('<Plug>TypecorrAdd')
  map <unique> <Leader>a <Plug>TypecorrAdd
endif
```
- 判断是否有相关的映射





