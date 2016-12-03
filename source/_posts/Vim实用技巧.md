title: 实用技巧
date: 2015-08-07 09:40:52
categories: 笔记
tags: [vim,笔记,技巧]
---

## 0. Vim解决问题的方式
### 结识`.` [^hpoint] 命令
**功能** 重复上次修改
**注意** 

- 插入模式下,重复从进入到结束的所有操作(*移动光标会重置修改状态*)
- `.`命令事实上是一个微型的宏

| 操作   | 功能                             |
| :----: | --------                         |
| `x`    | 删除光标下字符                   |
| `dd`   | 删除整行                         |
| `>G`   | 增加当前行到文档末尾处的缩进层级 |

<!-- more -->
### 不要自我重复
#### 减少无关移动
<font color='green'>**example1:**在行末添加分号</font>
方法1

```vim
$
a;<Esc>
j$.
```

方法2:

```vim
A;<Esc>
j.
```
#### 复合命令
>执行动作的同时自动从普通模式切换到插入模式

#### 常用复合命令

| 复合命令 | 普通操作 |
| :-:      | :-----   |
| `C`      | `c$`     |
| `s`      | `cl`     |
| `S`      | `^c`     |
| `I`      | `^i`     |
| `A`      | `$a`     |
| `o`      | `A<CR>`  |
| `O`      | `ko`     |

### 以退为进
使移动可重复`f{char}`[^f] `;`
```bash
f{char}
{operation}<Esc>
;.
```

### 执行、重复、回退

| 操作 | 功能                        |
| :-:  | -----                       |
| `@:` | 重复ex命令                  |
| `&`  | 重复上次的替换命令          |
| `,`  | 回退到上次`f{char}`字符位置 |
| `u`  | 撤消                        |

| 目的                   | 操作                        | 重复          | 回退 |
| ----                   | :-:                         | :-:           | :-:  |
| 做出一个修改           | `{edit}`                    | `.`           | `u`  |
| 在行内查找下一指定字符 | `f{char}`/`t{char}`         | `;`           | `,`  |
| 在行内查找上一指定字符 | `F{char}`/`T{char}`         | `;`           | `,`  |
| 在文档内查找下一匹配项 | `/{pattern}<CR>`            | `n`           | `N`  |
| 在文档内查找上一匹配项 | `?{pattern}<CR>`            | `n`           | `N`  |
| 执行替换               | `s/{pattern}/{replacement}` | `&`           | `u`  |
| 执行一系列修改         | `q{register}{changes}q`     | `@{register}` | `u`  |

### 查找并手动替换

| 功能                 | 操作           |
| -----                | :-:            |
| 替换                 | `:substitute`  |
| 查找当前光标下的单词 | ` * ` [^hstar] |
| 设置高亮查找         | `set hls`      |

### 结识`.`范式
>**用一键移动,另一键执行**

## 2. 模式
### 2.1. 普通
#### 2.1.1. 把撤消单元切成块，普通模式和插入模式之间切换的粒度,
#### 2.1.2. 构造可重复的修改:

| 操作  | 执行重复命令的操作 |
| -     | -                  |
| `dbx` | `x`                |
| `bdw` | `dw`               |
| `daw` | `daw`[^haw]        |

#### 2.1.3. 用次数做简单的算术运算

- **执行次数**[^hcount] 大部分普通模式命令都可以带一个次数前缀
- **八进制** 以0开头的数字

|操作|功能|
|-|-|
|`<C-a>`|对当前光标下或光标后数字执行加1|
|`<C-x>`|对当前光标下或光标后数字执行减1|
|`yyp`|复制行|
|`cw`|修改单词|
|`set nrformats=`|所有数字当作十进制|

#### 2.1.4. 能够重复，就别用次数
> **执行，重复，回退，在必要时使用次数**

#### 2.1.5. 双剑合璧，天下无敌
- 操作符[^operator] + 动作命令 = 操作
- 自定义操作符[^mapoperator]，参考已有插件commentary.vim[^pcommentary]
- 自定义动作命令与已有操作符协同工作[^omapinfo]，参考已有插件textobj-entire[^ptextobjentire]
- 操作符待决模式Operator-Pending mode，**只接受动作命令状态**:
    
|命令|用途|
|-|-|
|`c`|修改|
|`d`|删除|
|`y`|复制到寄存器|
|`g~`|反转大小写|
|`gu`|转换为小写|
|`gU`|转换为大写|
|` > `|增加缩进|
|``<``|减少缩进|
|` = `|自动缩进|
|`!`|使用外部程序过滤{motion}所跨越的行|

- 多按键操作命令:

|操作符|类型|
|-|-|
|`dd`,`>>`,`gUgU`(`gUU`)|一个操作命令连续调用再次，相当于作用于当前行|
|`g~`,`gu`,`gU`[^gU],`g`[^g],`z`[^z],`ctrl-w`[^ctrlw],``[`` [^[]|两个以上按键调用，头一个按键只是第二个按键的前缀，不会激活操作符待决模式，可以把它们当成命名空间|
|`cw`,`de`,`yb`|操作符后带动作命令，激活操作符待决模式|
### 2.2. 插入
#### 2.2.1. 在插入模式中即时更正错误

|按键操作|用途|
|-|-|
|`<C-h>`|删除前一个字符（同退格键）|
|`<C-w>`|删除前一个单词|
|`<C-u>`|删至行首|

#### 2.2.2. 返回普通模式

|按键操作|用途|
|-|-|
|`<Esc>`|切换到普通模式|
|`<C-[>`|切换到普通模式|
|`<C-o>`[^ictrlo]|切换到插入-普通模式|
|`zz`|重绘屏幕，并把当前行显示在窗口正中|

#### 2.2.3. 不离开插入模式，粘贴寄存器中的文本
- **插入模式下粘贴大量文本会有延时，因此相当于逐个输入**
- **插入模式下小心操作**`K`[^K],`J`[^J]

|按键操作|用途|
|-|-|
|`<C-r>{register}`[^ictrlr]|插入寄存器内的文本|
|`<C-r><C-p>{register}`[^ictrlrp]|按原义插入寄存器内的文本|

#### 2.2.4. 随时随地做运算
> 表达式寄存器<C-r>=

#### 2.2.5. 用字符编码插入非常用字符

|按键操作|用途|
|-|-|
|`<C-v>{nnn}`|以十进制字符编码插入字符|
|`<C-v>u{nnnn}`|以十六进制字符编码插入字符|
|`ga`|查看字符编码|
|`<C-v>{nodigit}`|按原义插入非数字字符|
|`<C-k>{c1}{c2}`|插入以二合字母{c1}{c2}表示的字符

#### 2.2.6. 用二合字母[^digraphsdefault]插入非常用字符
> `:digraphs`[^digraphtable] 查看可用的二合字母表

#### 2.2.7. 用替换模式替换已有文本

|按键操作|用途|
|-|-|
|`R`|由普通模式进入替换模式，<Esc>返回普通模式|
|`gR`|虚拟替换模式，按屏幕实际显示的宽度来替换字符|
|`r{char}`[^r]|单次替换|
|`gr{char}`|单次虚拟替换|

### 2.3. 可视
#### 2.3.1. 深入理解可视模式
> 选择模式[^selectmode]
> `<C-g>` —*VISUAL*—  —*SELECT*—

#### 2.3.2. 选择高亮选区
- 面向字符的模式
- 面向行的模式
- 面向列块的模式

|按键操作|用途|
|-|-|
|`v`|激活面向字符的可视模式|
|`V`|激活面向行的可视模式|
|`<C-v>`|激活面向列块的可视模式|
|`gv`|重复上次的高亮选区|

> 在可视模式间切换

|按键操作|用途|
|-|-|
|`<Esc>` `<C-[>`|回到普通模式|
|`v` `V` `<C-[>`|切换到普通模式|
|`v`|切换到面向字符的可视模式|
|`V`|切换到面向行的可视模式|
|`<C-v>`|切换到面向列块的可视模式|
|`o`|切换高亮选区的活动端|

#### 2.3.3. 重复执行面向行的可视命令
> 缩进:
- `shiftwidth`
- `softtabstop`
- `expandtab`
- Tabs and Spaces

> 面向行的可视命令适合`.`重复操作

#### 2.3.4. 只要可能，最好用操作符命令，而不是可视命令
> 可视模式操作，不适合重复[^vU]

#### 2.3.5. 用面向列块的可视模式编辑表格数据

```vim

<C-v>3j
x...
gv
r|
yyp
Vr-

```

#### 2.3.6. 修改列文本

```vim
<C-v>jje
c
components
<Esc>
```

#### 2.3.7. 在长短不一的高亮块后添加文本

```vim
<C-v>jj$
A;
<Esc>
```

### 2.4 命令行
#### 2.4.1 结识vim的命令行模式
- vim历史：**ed** —> **ex** —> **vi** —> **vim**
- 命令行模式类型
  1. :ex命令[^excmdindex]
  2. /查找匹配
  3. <C-r>=表达式
- 命令行模式快捷键

|按键操作|用途|
|-|-|
|`<c-w>`|删除至上个单词|
|`<c-u>`|删除至开头|
|`<c-v>` `<c-k>`|插入键盘找不到的字符|
|`<c-r>{register}`|插入寄存器内容|

- 常用命令行模式命令

|按键操作|用途|
|-----|-|
|``:[range]delete [x]``|删除指定范围内的行[到寄存器x中]|
|``:[range]yank [x]``|复制指定范围的行[到寄存器x中]|
|``:[line]put [x]``|在指定行后粘贴寄存器x中的内容|
|``:[range]copy {address}``|把指定范围内的行拷贝到{address}所指定的行之下|
|``:[range]move {address}``|把指定范围内的行移动到{address}所指定的行之下|
|``:[range]join``|连接指定范围内的行|
|``:[range]normal {commands}``|对指定范围内的每一行执行普通模式命令{commands}|
|``:[range]substitute/{pattern}/{string}/[flags]``|把指定范围内出现{pattern}的地方替换为{string}|
|``:[range]global/{pattern}/[cmd]``|对指定范围内匹配{pattern}的所有行，在其上执行Ex命令{cmd}|

#### 2.4.2 在一行或多个连续行上执行命令
- 可以用**行号**，**位置标记**或**查找模式**来指定范围开始结束位置，[range]
- `:print`打印命令
- 行号指定范围：
  - `:n`
  - `:n,m`
  - `:.` 当前行
  - `:$` 最后一行
  - `:%` 所有行
- 高亮选区指定范围:`'<,'>`
- 用模式指定范围:`/{pattern}/,/{pattern}/`
- 用偏移对地址进行修正:`{address}+n`

|符号|地址|
|-|-|
|`1`|文件第一行|
|`$`|文件最后一行|
|`0`|虚拟行，位于文件第一行上方|
|`.`|光标所在行|
|``'m``|包含位置标记m的行|
|`'<`|高亮选区的起始行|
|`'>`|高亮选区的结束行|
|`%`|整个文件（`:1,$`的简写形式）|

#### 2.4.3 使用`:t`和`:m`命令复制和移动行
- `` :[range]copy {address} ``（``:[range]t {address}``）
- ``:[range]move {address}``
- **Ex命令影响范围广且距离远**

#### 2.4.4 在指定范围上执行普通模式命令
- ``:[range]normal {commands}``

#### 2.4.5 重复上次的Ex命令
- `@:`[^@:]
- `:`[^hquote:]寄存器总是保存着最后执行的命令行命令
- `@@`调用上次执行的寄存器宏
- `<C-o>`回退
- ``:bn[ext]``缓冲区正向移动
- ``bp[revious]``缓冲区反向移动

#### 2.4.6 自动补全Ex命令[^commandcomplete]
- `<Tab>`，`<S-Tab>`自动补全
- `<C-d>`[^cctrld]显示可用补全列表
- 自定义补全行为[^wildmode]
- **bash shell** `set wildmode=longest,list`
- **zsh** `set wildmenu wildmode=full`
- **wildmenu**启用时`<Tab>` `<C-n>` `<Right>`与`<S-Tab>` `<C-p>` `<Left>`遍历

#### 2.4.7 把当前单词插入到命令行
- `<C-r><C-w>`复制光标下单词并插入命令行
- `* `命令等效于输入`/\<<C-r><C-w>\><CR>`
- `<C-r><C-a>`[^cctrlrctrlw]插入光标下的字符串到命令行

#### 2.4.8 回溯历史命令
- `:` —> `<Up>` or `<Down>`遍历命令历史，可加字符过滤
- `/` —> `<Up>` or `<Down>`遍历查找历史，可加字符过滤
- `history`选项，设置历史记录保存上限
- 历史记录退出重启后仍旧存在~！[^viminfo]
- 回溯时可以使用`<C-p>`或`<C-n>`遍历，只不过不会自动过滤
- 通过 `cnoremap <C-p> <Up> cnoremap <-n> <Down>`可以解决过滤的问题[^phistoryscrollers]
- 命令行窗口[^cmdwin]

|命令|动作|
|-|-|
|`q/`|打开查找命令历史的命令行窗口|
|`q:`|打开Ex命令历史的命令行窗口|
|`<C-f>`|从命令行模式切换到命令行窗口|
|`:q`|退出命令行窗口|
|`<CR>`|退出命令行窗口，并执行命令|

#### 2.4.9 运行Shell命令
- `%`代表当前文件名[^cmdlinespecial]
- 文件名修饰符[^filenamemodifiers]
- `<C-z>`在shell的vim中转入后台
- `fg`恢复
- `jobs`显示挂起的作业文件（man bash 作业控制job control）

|按键操作|用途|
|-|-|
|`:shell`[^hshell]|启动一个shell(exit返回vim)|
|`:!{cmd}`[^:!]|在shell中执行{cmd}|
|`:read !{cmd}`[^read!]|在shell中执行{cmd},并把其标准输出插入到光标下方|
|``:[range]write !{cmd}``[^writec]|在shell中执行{cmd},以[range]作为其标准输入|
|``:write !sh``[^renamefiles]|在shell中执行缓冲区的每行内容|
|``:[range]!{filter}``[^range!]|使用外部程序{filter}过滤指定的[range]|
|`!{motion}`[^!]|切换到命令行，并设置{motion}范围|
|`:2,$!sort -t ',' -k2`|/|

## 3. 管理多个文件
### 3.1 用缓冲区列表管理打开的文件
#### 3.1.1 `ls`列出缓冲区列表[^ls]
- `%`当前窗口可见
- `#`轮换文件
- `<C-^>`轮换文件间快速切换

#### 3.1.2 使用缓冲区列表：
- `:bprev`
- `:bnext`
- `:bfirst`
- `:blast`
- `:buffer N`[^:b]
- `:buffer {bufname}` 只需要提供足够标识的字符
- `:bufdo`[^:bufdo] 所有缓冲区执行Ex命令
- 映射缓冲区列表操作快捷键[^punimpaired]
 - ``nnoremap <silent> [b :bprevious<CR>``
 - ``nnoremap <silent> ]b :bnext<CR>``
 - ``nnoremap <silent> [B :bfirst<CR>``
 - ``nnoremap <silent> ]B :blast<CR>``


#### 3.1.3 删除缓冲区
- `:bdelete N1 N2 N3`
- `:N,M bdelete`


### 3.2 用参数列表将缓冲区分组
- `:args`显示参数列表
- `:args {arglist}`[^argsf]接填充参数列表
- ``:args *``用Glob(通配符)``*``[^hwildcard] ``**``[^hstarstarwildcard]指定文件
- `` :args `{shell commands}` ``用反引号结构指定文件[^backtick-expansion]
- 使用参数列表`:next`，`:prev`，`:argdo`

### 3.3 管理隐藏缓冲区

|按键操作|用途|
|-|-|
|``:w[rite]``|把缓冲区内容写入磁盘|
|``:e[dit]!``|把磁盘文件内容读入缓冲区|
|``:qa[ll]!``|关闭所有窗口，摒弃修改而无需警告|
|``:bn[ext]!``|隐藏当前缓冲区并切换到下一个|
|``:bp[revious]!``|隐藏当前缓冲区并切换到上一个|
|``set hidden``[^hidden]|启用默认自动隐藏缓冲区|
|``wn``|逐个保存|

### 3.4 将工作区切分成窗口

|按键操作|用途|
|-|-|
|`<C-w>s`|水平切分当前窗口，新窗口仍显示当前缓冲区|
|`<C-w>v`|垂直切分当前窗口，新窗口仍显示当前缓冲区|
|``sp[lit] {file}``|水平切分当前窗口，并在新窗口中载入{file}|
|``vsp[lit] {file}``|垂直切分当前窗口，并在新窗口中载入{file}|
|`<C-w>w`[^windowmovecursor]|在窗口间循环切换|
|`<C-w>h`|切换到左边的窗口[^mouse]|
|`<C-w>j`|切换到下边的窗口|
|`<C-w>k`|切换到上边的窗口|
|`<C-w>l`|切换到右边的窗口|
|``:clo[se]`` ``<C-w>c``|关闭活动窗口|
|``:on[ly]`` ``<C-w>o``|只保留活动窗口，关闭其他所有窗口|
|`:h window-resize`|\|
|`<C-w>=`|使所有窗口等宽、等高|
|``<C-w>_ ``|最大化活动窗口的高度|
|``<C-w>｜``|最大化活动窗口的宽度|
|``[N]<C-w>_ ``|把活动窗口的高度设为[N]行|
|``[N]<C-w>｜``|把活动窗口的宽度设为[N]列|
|`:h window-moving`|\|

### 3.5 用标签页将窗口分组[^tabpage]

|按键操作|用途|
|-|-|
|`:lcd {path}`|设置当前窗口的本地工作目录|
|`:windo lcd {path}`|所有窗口设置本地工作目录|
|``:tabe[dit] {filename}``|在新标签页中打开{filename}|
|`<C-w>T`|把当前窗口移到一个新标签页[^ctrlwT]|
|``:tabc[lose]``|关闭当前标签页及其中的所有窗口|
|``tabo[nly]``|只保留活动标签页，关闭所有其他标签页|
|``:tabn[ext] {N}`` ``{N}gt``|切换到下一标签页|
|``:tabn[ext]`` `gt`|切换到下一标签页|
|``:tabp[revious]`` `gT`|切换到下一标签页|
|``:tabmove [N]``|移动标签页，0移到开头，省略[N]移到结尾|

## 4. 打开和保存文件
### 4.1 用edit命令打开文件
- `:pwd`显示当前目录print working directory
- **tab**键展开
- `:edit %:h<Tab>`相对于活动目录打开文件
> `cnoremap <expr> %% getcmdtype() == ':' ? expand('%:h').'/' : '%%'`

### 4.2 使用find打开文件
- `:find`允许通过文件名打开文件
- ``set path+=app/**``配置’path’路径[^hpath][^filesearching]
- rails的智能路径管理[^prails]

### 4.3 使用netrw管理文件系统
- 准备工作：`set nocompatible  filetype plugin on`
- `-`返回上级目录
- 打开文件管理目录
 - `:edit {path}`，`:edit .`，`:edit %:h`，`:e`
 - `:Explore`[^explore] `:E`
 - `:Sexplore`，`:Vexplore`
     
- 与分割窗口协同工作`<C-^>`
- 使用netrw完成更多功能
 - 创建新文件[^netrw%]
 - 创建新目录[^netrwd]
 - 重命名已有的文件及目录[^netrwrename]
 - 删除[^netrwdel]
 - 通过网络读写文件scp,ftp,curl,wget[^netrwref]

### 4.4 把文件保存到不存在的目录中
- `<C-g>`显示当前文件的文件名及状态[^ctrlG]（
- `:!mkdir -p %:h`
- `:write`

### 4.5 以超级用户权限保存文件
- `ls -al /etc/|grep hosts`，`whoami`
- `:w !sudo tee % > /dev/null`[^writec][^:%]

---

## 5. 移动跳转[^motion]
### 5.1 用动作命令在文档中移动
#### 5.1.1 让手指保持在本位行上
> hjkl;

#### 5.1.2 区分实际行与屏幕行
- `'wrap'`，`'number'`
- `j`，`k`，`0`，`^`，`$`，`gj`，`gk`，`g0`，`g^`，`g$`

#### 5.1.3 基于单词移动[^wordmotions]
- `w`，`b`，`e`，`g`e面向单词移动，词首词尾，正向反向
- `W`,`B`,`E`,`gE`面向字串移动
- 理解单词(word)[^word]与字串(WORD)[^WORD]

#### 5.1.4 对字符进行查找
- `f`[^f] `;`[^;] `,`[^,]
- `noremap <Leader>n nzz`
- `noremap <Leader>N Nzz`
  - ``<Leader>``默认``\ ``[^mapleader]
  - ``let mapleader=","`` 
  - normal \ ,

- `f{motion}`，`F{motion}`，`t{motion}`，`T{motion}`，`;`，`,`

#### 5.1.5 通过查找进行移动
- `/{char}<CR>`
- `n`
- `N`
- `'hlsearch'`
- `'incsearch'`
- 用查找动作操作文本[^exclusive]
 - `v/{char}<CR>hd`
 - `d/{char}<CR>`


#### 5.1.6 用精确的文本对象选择选区[^textobjects]

|文本对象|选择区域|
|-|-|
|`a)` `ab`|一对圆括号(parentheses)|
|`i)` `ib`|圆括号内部|
|`a}` `aB`|一对花括号{braces}|
|`i}` `iB`|花括号内部|
|`a]`|一对方括号[brackets]|
|`i]`|方括号内部|
|`a>`|一对尖括号<angle brackets>|
|`i>`|尖括号内部|
|`a'`|一对单引号'single quotes'|
|`i'`|单引号内部|
|`a"`|一对双引号"double quotes"|
|`i"`|双引号内部|
|`` a` ``|一对反引号\`backticks\`|
|`` i` ``|反引号内部|
|`at`|一对XML标签tags|
|`it`|标签内部|

#### 5.1.7 删除周边，修改内部
- 分隔符文本对象，块对象
- 范围文本对象，非块对象
- d配a,c配i


|文本对象|选择范围|
|-|-|
|`iw`|当前单词|
|`aw`|当前单词及一个空格|
|`iW`|当前字串|
|`aW`|当前字串及一个空格|
|`is`|当前句子|
|`as`|当前句子及一个空格|
|`ip`|当前段落|
|`ap`|当前段落及一个空行|

#### 5.1.8 设置位置标记[^markmotion]，以便快速跳回
- `m{a-zA-Z}`[^m]小写缓冲区局部可见，大写全局可见
- `` `{mark} ``跳转到位置标记所在行，首个非空字符上
- ``'{mark}``跳转到位置标记所在位置
- 自动位置标记


|位置标记|跳转到|
|-|-|
|`"`|当前文件中上次跳转动作之前的位置|
|`.`|上次修改的地方|
|`^`|上次插入的地方|
|` [ `|上次修改或复制的起始位置|
|`]`|上次修改或复制的结束位置|
|` < `|上次高亮选区的起始位置|
|`>`|上次高亮选区的结束位置|

#### 5.1.9 在匹配括号间跳转
- **matchit.vim**插件，对tags的跳转[^matchitinstall]

```vim
set nocompatible
filetype plugin on
runtime macros/matchit.vim
```

- %对一组开、闭括号间跳转[^%]
- Surround.vim，给选中的文本加分隔符`S"`

### 5.2 在文件间跳转
#### 5.2.1 遍历跳转列表
- `:jumps`显示跳转列表
- **大范围动作命令会被当成跳转，小范围的动作命令只算移动**
- 每个单独窗口拥有一份跳转列表
- <font color='red'>注意</font>**`<C-i>`和`<Tab>`被当成同一个东西**
- `<C-o>` `<C-i>`
    

|按键操作|用途|
|-|-|
|`{count}G`|跳转到指定行号|
|`/pattern<CR>/?pattern<CR>` `n` `N`|跳转到下一个/上一个模式出现之处|
|`%`|跳转到匹配括号所在之处|
|`(` `)`|跳转到上一个/下一句的开头|
|`{` `}`|跳转到上一段/下一段的开头|
|`H` `M` `L`|跳到屏幕最上方/正中间/最下方|
|`gf`|跳转到光标下的文件名|
|`<C-]>`|跳转到光标下关键字的定义之处|
|`` `{mark} `` `'{mark}`|跳转到一个位置标记|

#### 5.2.2 遍历改变列表
- `:changes`[^changelist]显示编辑会话期间维护的改变列表
- `g;`和`g,`遍历改变列表，类似`u<C-r>`
- `:h '.` `:h '^` 
- `gi`[^gi]回到退出的地方继续编辑
- 每个缓冲区单独维护一张表
    
#### 5.2.3 跳转到光标下的文件[^gf]
- 指定文件扩展名`set suffixesadd+=.rb`[^suffixesadd]
- 指定要搜寻的目录[^path]`set path?`显示值
- bundler.vim 使用工程的Gemfile自动生成"path"设置
- `vim -u NONE -N {file}`不加载任何插件启动vim
#### 5.2.4 用全局位置标记在文件间快速跳转

> 在浏览代码前先设置一个全局标记`:h '` `:h m` `:h 'viminfo'`

---

## 6. 寄存器
### 6.1 复制与粘贴
#### 6.1.1 用无名寄存器实现删除、复制与粘贴操作
- `xp`
- `ddp`
- `yyp`

#### 6.1.2 深入理解Vim寄存器
- 引用一个寄存器`"{register}`，缺省使用无名寄存器`""`
- vim术语对照剪切(cut)=delete,复制(copy)=yank,粘贴(paste)=put
- 无名寄存器（`""`）[^quotequote]
> `x`,`s`,`d{motion}`,`c{motion}`与`y{motion}`以及它们的大写命令都会覆盖无名寄存器

- 复制专用寄存器（`"0`）[^quote0]
> `y{motion}`命令时才会使用

- `:reg "{register}`显示寄存器内容
- 有名寄存器（`"a`-`"z`）[^quotealpha]，小写重写，大写追加
- 黑洞寄存器（`"_ `）[^quote_]，有去无回
- 系统剪切板（`"+`）[^quote+]
- 选择专用寄存器（`"* `）[^quotestar]
- 表达式寄存器（`"=`）[^quote=]，计算并返回值
- 其他寄存器，只读，(`".`)[^quote.]
- `"%`，`"#`，`".`，`":`，`"/`

#### 6.1.3 用寄存器中的内容替换高亮选区的文本
> `:h v_p`

#### 6.1.4 把寄存器的内容粘贴出来[^p]
- 面向行还是面向字符[^linewiseregister]
- `<C-r>{register}`总插入到光标之前
- `gp`，`gP`，粘贴后光标在文本之后，更适合操作

#### 6.1.5 与系统剪贴板进行交互[^paste]
- `:set pastetoggle=<f5>`使用f5切换"paste"选项
- 使用加号寄存器进行粘贴可以避免

---

### 6.2 宏
#### 6.2.1 宏的读取与执行
- `q{register}`,按`q`停止
- `@{register}`执行[^@],`@@`重复最近调用过的宏
- 串行，并行

#### 6.2.2 规范光标位置、直达目标以及中止宏
- 确保每条命令都可被重复执行
- 规范光标的位置，更容易正确执行
- 用可重复的动作命令直达目标
- 当动作命令失败时，宏将中止执行[^visualbell]

#### 6.2.3 加次数回放宏
> `n@{register}`

#### 6.2.4 在连续的文件行上重复修改
- 串行：`n@{register}`
- 并行：`:’<,’>normal @{register}`

#### 6.2.5 给宏追加命令
> 使用大写字母

#### 6.2.6 在一组文件中执行宏
- 准备工作

```vim
set nocompatible
filetype plugin indent on
set hidden
if has("autocmd")
autocmd fileType ruby setlocal ts=2 sts=2 sw=2 expandtab
endif
```

- 启用"hidden"
- 建立目标文件列表``:args *.rb``
- 录制宏`:first`，`:e!`
- `:h :argdo` `:h :edit!`
- `:argdo normal @a`并行执行宏
- `qA :next q 22@a`串行执行宏
- 保存所有文件改动`:h` `:wa` `:argdo write` `:h :wn`

#### 6.2.7 用迭代求值的方式给列表编号

```vim
:let i=1
qa
I<C-r>=i<CR>)<Esc>
:let i+= 1
q
jVG
:normal @a
```

#### 6.2.8 编辑宏的内容
- `:put a`（put 总会将内容粘贴到下一行)
- `"ay$`（dd会留下^J）
- `let @a=substitute(@a, '\~', 'vU', 'g')`（:h substitute()）（:h function-list）

---

## 7. 匹配
### 7.1 按模式匹配及按原义匹配
#### 7.1.1 调整查找模式的大小写敏感性
- 全局设置大小写敏感性"`ignorecase`"（会影响关键字自动补全行为）
- 每次查找时设置大小写敏感（可以出现在元字符的任何位置）
 - `\c`忽略大小写
 - `\C`强制区分大小写

- 启用更智能的大小写敏感设置"`smartcase`"(:h /ignorecase)

#### 7.1.2 按正则表达式查找时，使用`\v`模式开关
- **magic**模式下，方括号具有特殊含义，不用转义，圆括号开闭都要转义，花括号只需开括号转义
- **very magic**模式除_ ，大小写字母以及数字0到9之外的所有字符都具有特殊意义（:h \v）
- `:h /character-classes`
- `:h /\\`任何还未具有特殊含义的字符都被保留以被将来扩展时使用
- `\v` **very magic** `\V`原义 `\m` magic(`.`，`* `,方括号具有特殊含义） `\M` nomadic（一些字符具有特殊含义，`^`与`$`）

#### 7.1.3 按原义查找文本时，使用`\V`原义开关
> `:h /\V`

#### 7.1.4 使用圆括号捕获子匹配
- `\0`全匹配，`\1`-`\9`匹配捕获的文本
- `:h /\_` `:h 27.8`

#### 7.1.5 界定单词的边界
- `<`与`>`符号表示单词定界符
- 使用圆括号但不捕获内容在括号前加`%`
- `\W\ze\w`模拟元字符`<`，`\w\ze\W`表示元字符`>`
- `<`与`>`只在very magic下不用转义`:h /\< `
- ` * `与`#`查找光标下单词，有用到单词定界符
- `g* `与`g#`同上，但不使用单词定界符
- 零宽度元字符，本身不匹配字符，表示单词与围绕此单词的空白字符之间的边界

#### 7.1.6 界定匹配的边界
- `\zs`匹配开始`:h /\zs`
- `\ze`匹配结束

#### 7.1.7 转义问题字符
- `\V`下正向查找需转义`/`与`\`
- `\V`下反向查找需转义`?`与`\`
- 用编程的方式转义字符`:h escape()` `:h getcmdtype()`

```vim
/\V<C-r>=
=escape(@u, getcmdtype().’\’)
```

- `/vim/e<CR>`
- GVim `:promptfind`（:h :promptfind）

### 7.2 查找
#### 7.2.1 结识查找命令
"`wrapscan`"  `:h 'wrapscan'`

|按键操作|用途|
|-|-|
|`n`|跳至下一处匹配，保持查找方向与偏移不变|
|`N`|跳至上一处匹配，保持查找方向与偏移不变|
|`/<CR>`|正向跳转至相同模式的下一处匹配|
|`?<CR>`|反向跳转至相同模式的上一处匹配|

#### 7.2.2 高亮查找匹配
- `:h 'hlsearch'` 启用匹配高亮
- `:set nohlsearch`(`:se hls!`) 禁用匹配高亮
- `:nohlsearch` `:h noh` 临时关闭匹配高亮
- `nnoremap <silent> <C-l> :<C-u>nohlsearch<CR><C-l>(:h CTRL-L)`

#### 7.2.3 在执行查找前预览第一处匹配
- `:h 'incsearch'` 增量匹配
- `<C-r><C-w>`查找域自动补全

#### 7.2.4 统计当前模式的匹配个数
> `:%s///gn`

#### 7.2.5 将光标偏移到查找匹配结尾
- `:h search-offset`
- `/{pattern}/e<CR>`

#### 7.2.6 对完整的查找匹配进行操作
```vim
/\vX(ht)?ml\C<CR>
gU//e<CR>
//<CR>
.
//<CR>.
```

- `//<CR>`,`n`会重复`//e<CR>`所以不适合点范式
- Natsuno textobj-laastpat插件`gUi/`解决了上面问题

#### 7.2.7 利用查找历史，迭代完成复杂的模式
- `/<UP>`遍历查找历史
- `q/`在命令行窗口修改
- `c%(<C-r>")<Esc>`
- `:%s//"\1"/g`

#### 7.2.8 查找当前高亮选区中的文本
- `:h visual-search`
- `:vmap X y/<C-R>"<CR>`（某些特殊字符"`.`"或"`*`"通常会引起问题）
- 解决方案：

```vim
xnoremap *:<C-u>call <SID>VSetSearch()<CR>/<C-R>=@/<CR><CR>
xnoremap #:<C-u>call <SID>VSetSearch()<CR>?<C-R>=@/<CR><CR>
function! s:VsetSearch()
   let temp = @s
   norm! gv"sy
   let @/ = '\V' .substitute(escape(@s, '/\'), '\n', '\\n', 'g')
   let @s = temp
endfunction
```

- **visual star search**插件
- `xnoremap`关键字指明映射项只在可视模式下有效，不包括选择模式
- `:h mapmode-x`


### 7.3 替换
#### 7.3.1 结识substitute命令
- `:[range]s[ubstitute]/{pattern}/{string}/[flags]`
- 标志位`:h s_flags`
 - `g`，全局执行
 - `c`，确认或拒绝
 - `n`，抑制正常替换行为，即不执行替换操作，只报告匹配次数
 - `e`，屏蔽错误提示
 - `&`，重用上一次的标志位

- 特殊字符`:h sub-replace-special`

|符号|描述|
|-|-|
|`\r`|插入一个换行符|
|`\t`|插入一个制表符|
|`\\`|插入一个反斜杠|
|`\n`|插入第n个子匹配n为1-9|
|`\0`|插入匹配模式的所有内容|
|`&`|插入匹配模式的所有内容|
|`={vim script}`|执行{vim script}表达式，并将返回的结果作为替换{string}|
|`~`|使用上一次调用时的{string}|

#### 7.3.2 在文件范围内查找并替换每一处匹配
- 标志位`g`

#### 7.3.3 手动控制每一次替换操作
- 标志位`c`   `:h :s_c`

|答案|用途|
|-|-|
|`y`|替换此处匹配|
|`n`|忽略此处匹配|
|`q`|退出替换过程|
|`l`|"last"--替换此处匹配后退出|
|`a`|"all"--替换此处与之后所有的匹配|
|`<C-e>`|向上滚动屏幕|
|`<C-y>`|向下滚动屏幕|

#### 7.3.4 重用上次的查找模式
- 查找域留空
- `:h cmdline-history`
- `<C-r>/`在命令历史中创建完整的记录

#### 7.3.5 用寄存器的内容替换
- 传值：通过`<C-r>{register}`插入寄存器内容到替换域
- 引用：通过`\=@{register}`引用寄存器内容到替换域
- 比较：`:let @/='……'`  `:let @a='……..'`  `:%s//\=@a/g`

#### 7.3.6 重复上一次Substitute命令
- `g&`整个文件范围内执行上一次`substitute`命令 `:h g&`
- `:&` 重复上一次`substitute`命令  `:h :&`
- `:&&` 后面的`&`重复上一次的标志位
- `gv`  激活可视模式并选中上次高亮位置
- 修正`&`命令

```vim
nnoremap & :&&<CR>
xnoremap & :&&<CR>
```

#### 7.3.7 使用子匹配重排CSV文件的字段
- `\n`

#### 7.3.8 在替换过程中执行算术运算

```vim
/\v\<\/?h\zs\d
:%s//\=submatch(0) - 1/g
```

#### 7.3.9 交换两个或更多的单词

```vim
/\v(<man>|<dog>)
:%s//\={"dog":"man","man":"dog"}[submatch(1)]/g
```

- `:h submatch()`
- **Abolish.vim**:超级`substitute`命令
  - 由Tim pope开发的插件
  - `:%S/{man,dog}/{dog,man}/g`
      

#### 7.3.10 在多个文件中执行查找与替换
- 散射法

```vim
/Pragmatic\ze Vim
:%s//Practical/g
:args **/*.txt
:set hidden
:argdo %s//Practical/ge
```


- 创建只包含目标的文件列表

```vim
/Pragmatic\ze Vim
:vimgrep /<C-r>// **/*.txt
```

 - `:copen` 打开quickfix列表
 - **substitution/qargs.vim**

```vim
command! -nargs=0 -bar Qargs execute 'args' QuickfixFileName()
function! QuickfixFilenames()
   let buffer_numbers = {}
   for quickfix_item in getqflist()
       let buffer_numbers[quickfix_item['bufnr']] = bufname(quickfix_item['bufnr'])
   endfor
   return join(map(values(buffer_numbers), 'fnameescape(v:val)'))
endfunction
```

 - `:Qargs` 把quickfix列表中的文件加至参数列表
 - `:argdo %s//Practical/g`
 - `:argdo update` `:h update`
 - `:Qargs | argdo %s//Practical/g | update`
 - `:h :bar`



### 7.4 global命令
#### 7.4.1 结识global命令
- `:h :g`
- `:[range]global[!] /{pattern}/ [cmd]`
- `:global!  :vglobal`

#### 7.4.2 删除所有包含模式的文本行
- `:g/re/d`
 - `/\v\<\/?\w+>`
 - `:g//d`

- `:v/re/d`
 - `:v/href/d`

#### 7.4.3 将ToDo项收集至寄存器

```vim
qaq
:g/TODO/yank A
:g/TODO/t$
```

#### 7.4.4 将CSS文件中所有规则的属性按照字母排序
- `:h :sort`
- `vi{ | :’<,’>sort`
- `:g/{/ .+1,/}/-1 sort`
- `:g/{pattern}/[range][cmd]`
- `:g/{start}/ .,{finish} [cmd]`
- `:g/{/ .+1,/}/-1 >`
- `:h >` `:h` `:sil` `:slient`
- `:g/{/sil .+1,/}/-1 >`

---


## 8. 工具

### 8.1 过ctags建立索引，并用其浏览源代码 ###

#### 8.1.1 结识ctags ####

##### 8.1.1.1 安装Exuberant Ctags  #####
- `sudo apt-get install exuberant-ctags`
- osx自带的是ctags的BSD
- `brew install ctags`
- 修改$PATH使/usr/local/bin比/usr/bin优先
- Doctor JS,jsctags

##### 8.1.1.2 创建代码库索引 #####
- `ctags *.rb`

##### 8.1.1.3 详解标签文件 #####
- 前几行由元数据组成
- 此后的每一行由关键字、文件名以及关键字在源代码中的位置构成
- 关键字按字母顺序排列
##### 8.1.1.4 用模式定位关键字，而不是行号 #####
- 关键字的地址可以是任意的Ex命令

##### 8.1.1.5 用元数据标记关键字 #####
- 传统由制表符分隔3组字段构成：关键字、文件名以及定位符
- 扩展格式：末尾添加字段，为关键字提供元数据，c类，f函数

#### 8.1.2 配置Vim使用ctags ####
- `:h 'tags'`指定标签文件查找路径
- `:h tags-option`
- `set tags?`查看选项内容

##### 8.1.2.1 生成标签文件 #####

```
!ctags -R
-exclude=.git // or -languages=-sql
:nnoremap <f5> :!ctags -R<CR>  //映射快捷键
```

##### 8.1.2.2 在每次保存文件时自动执行ctags #####
- `autocommand`允许事件发生时调用一条命令
- `:autocmd BufWritePost * call system("ctags -R")`

##### 8.1.2.3 通过版本控制工具的回调机制自动执行ctags #####
>Tim Pope的<<Effortless Ctags with Git>>讲解了如何为post-commit、post-merge、以及post-checkout等事件建立回调机制

#### 8.1.3 使用Vim的标签中转命令，浏览关键字的定义 ####
- `<C-]>`跳转到
- `<C-t>`跳回
- `g<C-]>`可选跳转
- `:h tag-stack`、`:h tag-priority`
- `:tselect` `:tprev` `:tnext` `:tfirst` `:tlast`
- `:tag{keyword}`等同于`<C-]>`
- `:tjump{keyword}`等同于`g<C-]>`
- `:h :tag` `:h :tjump`
- `:tag /{pattern}` `:tjump /{pattern}`
- `:pop`等同于`<C-t>`

### 8.2 编译代码，并通过Quickfix列表浏览错误信息 ###

#### 8.2.1 不用离开Vim也能编译代码 ####

- 准备工作
- 在Shell中编译工程`make`
- 在Vim中编译工程`:make`
  - `:h quickfix`加快 编辑-编译-编辑 循环
  - `:cnext`跳转到下一次quickfix
  - `:make!`编译不自动跳转
  
#### 8.2.2 浏览Quickfix列表 ####

##### 8.2.2.1 位置列表以|开头，quickfix只有一份，位置列表有多份 #####

- `:lnext` `:lprev` `:ll N` `:lmake` `:lgrep` `:lvimgrep`

|命令|用途|
|--|--|
|`:cnext`|跳转到下一项|
|`:cprev`|跳转到上一项|
|`:cfirst`|跳转到第一项|
|`:clast`|跳转到最后一项|
|`:cnfile`|跳转到下一个文件中的第一项|
|`:cpfile`|跳转到上一个文件中的最后一项|
|`:cc N`|跳转到第n项|
|`:copen`|打开quickfix窗口|
|`:cclose`|关闭quickfix窗口|

#### 8.2.3 回溯以前的Quickfix列表 ####

- `:colder` `:cnewer`

#### 8.2.4 定制外部编译器 ####

- `:h :compiler` `:h :make`
- 配置`:make`命令，使调用JSLint来验证JavaScript文件
  - nodelint依赖Node.js，可以通过NPM命令安装`npm install nodelint -g`
  - `:h 'makeprg'`允许指定运行:make时所调用的程序
  - `:setlocal makeprg=NODE_DISABLE_COLORS=1\nodelint\%`
    - 相当于在命令行执行` export NODE_DISABLE_COLORS=1 nodelint 文件路径`
  - 用Nodelint的输出结果填充Quickfix列表
    - `errorformat`选项允许我们指导Vim如何解析由`:make`产生的输出结果`:h 'errorformat'`
    - 查看缺省选项`:setglobal errorformat?`
    - `%f`文件名， `%l`行号，`%m`错误信息，`:h errorformat`
    - `setlocal efm=%A%f\,\ line\ %l\,\ character\ %c:%m,%Z%.%#,%-G%.%#`
  - 一条命令设置`makeprg`与`errorformat`
    - `:h :compiler`
    - `:compiler nodelint`
    - `:args $VIMRUNTIME/compiler/*.vim`

### 8.3 通过grep,vimgrep以及其他工具对整个工程进行查找 ###

#### 8.3.1 不必离开Vim也能调用grep ####

- `:h :grep`
- `-i`不区分大小写

#### 8.3.2 定制grep程序 ####

- `:h 'grepprg'` `$*`代表占位符，代替参数
- `:h 'grepformat'`可以逗号分隔多组格式
- `:h errorformat`
- `grepprg="grep -n $* /dev/null"`
- `grepformat="%f:%l:%m,%f:%l%m,%f %l%m"`
- 通过`:grep`调用`ack`
  - 安装`ack`
    - ubuntu
      - `sudo apt-get install ack-prep`
      - `sudo ln -s /usr/bin/ack-grep /usr/local/bin/ack`
    - OSX
      - `brew install ack`
  - `:set grepprg=ack\ -nogroup -column\ $*`
  - `:set grepformat=%f:%l:%c:%m`
- 其他grep插件
  - ack.vim
  - fugitive.vim `:Ggrep git-grep`
- 使用Vim内部的Grep
  - `:vim[grep][!]/{pattern}/[g][j] {file} ...`
  - `g` 允许单行多匹配
  - `j` 允许不自动跳转
  - `:h :_##`

#### 8.3.3 自动补全 ####

- 结识Vim关键字自动补全
  - `:h 'ignorecase'`会影响自动补全
  - `:h 'inercase'`避免`ignorecase`对自动补全的影响
  - `<C-p>`和`<C-n>`调用普通关键字自动补全
  - `<C-x>` `:h ins-completion`
- 与自动补全的弹出式菜单进行交互
  - `:h popupmenu-completion`
- 掌握关键字的来龙去脉
  - `:h compl-current <C-x><C-n>`当前关键字补全
  - `:h compl-keyword <C-x><C-i>`
  - `:h 'include'`
  - `:h compl-tag <C-x><C-]>`
  - `:h 'complete'`
- 使用字典中的单词进行自动补全
  - `:h compl-dictionary <C-x><C-k>`
  - `:set spell`
  - `:h 'dictionary'`
- 自动补全整行文本
  - `:h compl-whole-line <C-x><C-l>`
- 自动补全文件名
  - `:h compl-filename <C-x><C-f>`
  - `:pwd`
  - `:cd {path}`
  - `:cd -` `:h :cd-`切换回原来的工作目录
- 根据上下文自动补全
  - `:h compl-ommi <C-x><C-o>`
  - `:h compl-omni-filetypes`
  - `:h complete-function`

|命令|补全类型|
|--|--|
|<C-n>|普通关键字|
|<C-x><C-n>|当前缓冲区关键字|
|<C-x><C-i>|包含文件关键字|
|<C-x><C-]>|标签文件关键字|
|<C-x><C-k>|字典查找|
|<C-x><C-l>|整行补全|
|<C-x><C-f>|文件名补全|
|<C-x><C-o>|全能补全|

|按键操作|作用|
|--|--|
|<C-n>|使用来自补全列表的下一个匹配项|
|<C-p>|使用来自补全列表的上一个匹配项|
|<Down>|选择来自补全列表的下一个匹配项|
|<Up>|选择来自补全列表的上一个匹配项|
|<C-y>|确认使用当前选中的匹配项|
|<C-e>|还原最早输入的文本|
|<C-h>或<BS>|从当前匹配项中删除一个字符|
|<C-l>|从当前匹配项中增加一个字符|
|{char}|中止自动补全并插入字符|

#### 8.3.4 利用Vim的拼写检查器，查找并更正拼写错误 ####

- 对你工作进行拼写检查
  - `:set spell`
  - `[s`和`]s`遍历拼写错误项`:h ]s`
  - `z=`获取建议项`:h z=, 1z=`
  - `zg`将当前单词添加到拼写文件中
  - `zw`把当前单词从拼写文件中删除
  - `zug`撤消针对当前单词的`zg`或`zw`命令
- 使用其他拼写字典
  - `:h 'spelllang'`
  - `:h spell-remarks`
  - `:h spelllfile.vim`
- 将单词添加到拼写文件中`:h 'spellfile'`
- 在插入模式下更正拼写错误`<C-x>s` `:h compl-spelling`

#### 8.3.5 接下来干什么 ####

- 继续练习
- 定制你自己的vim
  - essential.vim
  - vimcasts.org
  - moolenaar.net/habits.html
  - github.com/nelstrom/dotfiles
  - iccf-holland.org
- 欲善其事，先利其器

### 8.4 根据个人喜好定制vim ###

#### 8.4.1 动态改变vim的设置项 ####

- `:h option-list`
- 选项前面加no,关闭功能
- 选项之后加!,反转设置
- 选项之后加?,获取选项当前状态
- 选项之后加&,重置为默认值
- 可以set多组选项
- setlocal只对当前缓冲区设置

#### 8.4.2 将配置信息存至vimrc文件 ####

- `:h :source`
- `:source {file}`应用文件设置项
- `:h vimrc`
- `:edit $MYVIMRC`

#### 8.4.3 为特定类型的文件应用个性化设置 ####

```
if has("autocmd")
  filetype on
  autocmd FileType ruby setlocal ts=2 sts=2 sw=2 et
  autocmd FileType javascript setlocal ts=4 sts=4 sw=4 noet
endif
```

- `:h :autocmd`
- `:h ftplugin-name`
- ~/.vim/after/ftplugin/javascript.vim
- `filetype plugin on`
