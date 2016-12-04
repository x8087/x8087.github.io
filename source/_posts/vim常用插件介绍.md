title: vim常用插件介绍
date: 2015-08-17 11:45:18
categories: 技术
tags: [vim, 技术, 插件]
---
主要介绍Spf13vim配置里用到以及常用的插件。涉及插件的用途以及插件的一般使用方法。
<!-- more -->
### Deps ###

#### Plugin 'gmarik/vundle'
Vundle是Vim bundle的缩写， 它是一个Vim插件管理器。
功能：

- 直接在.vimrc中跟踪和配置你的插件。
- 安装已配置的脚本（a.k.a. bundle）
- 更新已配置的脚本
- 按名称搜索所有可用的Vim脚本
- 清理未使用的插件
- 使用交互模式在单个按键中运行上述操作


- 管理已安装脚本的运行时路径
- 在安装和更新后重新生成帮助标签

Vundle的搜索使用http://vim-scripts.org提供所有可用的Vim脚本列表。

##### 配置插件 #####

Vundle跟踪你想要通过`Plugin`命令在*.vimrc*里配置的插件。每个`Plugin`命令告诉Vundle在启动时激活脚本将它添加到你的**runtimepath**。注释或者移除行将禁用插件。
每个`Plugin`命令都有一个指向脚本的URI。没有注释跟在命令的同一行。
```
Plugin 'git_URI'
```
`Plugin`命令可以选择在URI之后采用第二个参数。它必须是一个字典，用逗号与URI隔开。每个键值对在字典中是一个配置选项。

以下脚本配置选项可用。

| 选项 | 介绍 | 例子 |
|:-|:-|:-|
| rtp | 指定在存储库里Vim插件所在位置的目录（从存储库的根目录相对路径）。它确定将要添加到**runtimepath**的路径。 | Plug 'git_URI',{'rtp': 'some/subdir/'} |
| name | 所配置脚本即将保存的本地克隆目录名称 | Plgin 'git_URI',{'name':'newPluginName'} |
| pinned | 一个标志，当值设置为1时，告诉Vundle不要在插件上执行任何git操作，同时仍然添加`bundles`目录下现有插件到**runtimepath**  | Plugin 'mylocalplugin',{'pinned':1} |

##### 支持的URI #####

URI | 例子 | 备注
:-|:-|:-
GitHub | Plugin 'VundleVim/vundle.vim' => https://github.com/VundleVim/Vundle.vim | 当`user/repo`传给`Plugin`时使用GitHub
Vim Script | Plugin 'ctrlp.vim' => https://github.com/vim-scripts/ctrlp.vim | 不带斜杠的任意单个单词被当作来自Vim Scripts
Other Git URI | Plugin 'git://git.wincent.com/command-t.git'  | 
本地插件 | Plugin 'file:///path/from/root/to/plugin' | 

##### 安装插件 #####
```
:PluginInstall "安装.vimrc里配置的所有插件。最新安装的插件将自动启用。某些插件可能需要额外的步骤如编译或者外部程序，请参阅它们的文档。
:PluginInstall unite.vim "安装并激活插件unite.vim
:PluginInstall tpope/vim-surround tpope/vim-fugitive "安装多个插件，用空格隔开
"可以用**Tab**自动补全已知的名称
```
**Note**: 
  安装操作不是永久的。要完成，必须在.vimrc的合适位置放置`Plugin 'unite.vim'`告诉Vundle启动时加载插件。
  安装完后可以按`l`查看命令的日志。

##### 更新插件 #####
```
:PluginInstall!
:PluginUpdate "安装或者更新已配置的插件。可更新多个，用空格隔开。
```
**Note**:
  更新完成后按`u`查看所有已更新插件的改变日志。按`l`查看命令的日志。

##### 搜索插件 #####
```
:PluginSearch foo "在Vim脚本库搜索相关插件，显示在新的分隔窗口。
:PluginSearch! foo "在搜索前刷新脚本列表
:PluginSearch! "不带参数搜索所有插件
```
[Vim Scripts](http://vim-scripts.org/vim/scripts.html)
需要'curl'在系统上可用。

##### 列出插件 #####
```
:PluginList "显示已安装插件列表
```

##### 清理 #####
```
:PluginClean "请求确认删除所有.vimrc文件未配置但是存在于bundle安装目录的插件
:PluginClean! "自动确认删除没用过的插件
```

##### 交互模式按键映射 #####

KEY | DESCRIPTION
----|-------------------------- 
 i  |  run :PluginInstall with name taken from line cursor is positioned on
 I  |  same as i, but runs :PluginInstall! to update bundle
 D  |  delete selected bundle (be careful not to remove local modifications)
 c  |  run :PluginClean
 s  |  run :PluginSearch
 R  |  fetch fresh script list from server

##### 选项 #####
```
let g:vundle_default_git_proto = 'git' "使Vundle使用git替代https
```

##### 旧版本接口改变 #####

  Deprecated Names  | New Names
  ------------------|----------
  Bundle            | Plugin
  BundleInstall(!)  | PluginInstall(!), VundleInstall(!)
  BundleUpdate      | PluginUpdate, VundleUpdate
  BundleSearch(!)   | PluginSearch(!), VundleSearch(!)
  BundleClean       | PluginClean(!), VundleClean(!)
  BundleList        | PluginList

#### Plugin 'MarcWeber/vim-addon-mw-utils'
#### Plugin 'tomtom/tlib_vim'
#### Plugin 'mileszs/ack.vim'
##### 介绍 #####
Ack全局搜索插件
```
:Ack[!] [options] {pattern} [{directory}]

```
> 用`{pattern}`递归搜索目录`{directory}`(默认为当前目录)并打开*quickfix*窗口显示匹配项。如果没有`[!]`自动跳转到第一个匹配项。

```
:AckAdd [options] {pattern} [{directory}]

```
> 与`:Ack`类似，不过搜索到的匹配项添加到当前的*quickfix*列表。

```
:AckFromSearch [{directory}]

```
> 与`:Ack`类似，不过使用上一次搜索的模式。

```
:LAck [options] {pattern} [{directory}]

```
> 与`:Ack`类似，不过匹配项放到*location-list*。

```
:LAckAdd [options] {pattern} [{directory}]
```
> 与`:AckAdd`类似，不过匹配项放到*location-list*。

```
:AckFile [options] {pattern} [{directory}]
```
> 与`:Ack`类似，不过只搜索文件名。

```
:AckHelp[!] [options] {pattern}
```
> 与`:Ack`类似，不过只搜索vim帮助文档。

```
:LAckHelp [options] {pattern}
```
> 与`:AckHelp`类似，不过匹配项放到*location-list*。

```
:AckWindow[!] [options] {pattern}
```
> 使用`{pattern}`模式对屏幕中当前标签页可见的所有缓存搜索。

```
:LAckWindow [options] {pattern}
```
> 与`:AckWindow`类似，不过匹配项放到*location-list*。

##### 配置 #####
选项 | 介绍 | 默认值 | 例子
:-|:-|:-|:-
g:ackprg | 指定搜索命令 | ack | let g:ackprg="ag --vimgrep"
`g:ack_default_options` | 指定传递的默认参数。只用在`g:ackprg`没有自定义配置时。 | -s -H --nocolor --column | let g:ack_default_options = " -s -H --nocolor --nogroup --column --smart-case --follow"
`g:ack_apply_qmappings` | 在*quickfix*窗口启用映射 | 1 | 
`g:ack_apply_lmappings` | 在*location-list*窗口启用映射 | 1 | 
`g:ack_mappings` | 列出在*quickfix*/*location-list*窗口创建的所有映射 | {"t": "<C-W><CR><C-W>T","T": "<C-W><CR><C-W>TgT<C-W>j","o": "<CR>","O": "<CR><C-W><C-W>:ccl<CR>","go": "<CR><C-W>j","h": "<C-W><CR><C-W>K","H": "<C-W><CR><C-W>K<C-W>b","v": "<C-W><CR><C-W>H<C-W>b<C-W>J<C-W>t","gv": "<C-W><CR><C-W>H<C-W>b<C-W>J" } | let g:ack_mappings={"o": "<CR>zz"}
`g:ack_qhandler` | 打开*quickfix*窗口的命令 | "botright copen" | letg:ack_qhandler = "botright copen 30"
`g:ack_lhandler` | 打开*location-list*窗口的命令 | "botright lopen" | `let g:ack_lhandler = "botright lopen 30"`
`g:ackhightlight` | 使用此选项突出显示搜索的字词 | 0 | `let g:ackhightlight = 1`
`g:ack_autoclose` | 使用此选项指定是否在使用快捷键后关闭*quickfix*窗口 | 0 | `let g:ack_autoclose = 1`
`g:ack_autofold_results` | 使用此选项通过名字折叠在*quickfix*里的结果。默认只有当前折叠打开且当按‘j'和’k‘在结果之间移动时，如果碰到折叠的最后一项时关闭并打开下一个折叠。| 0 | `let g:ack_qutofold_results = 1`
`g:ackpreview` | 使用此选项用’j'或者‘k’自动打开文件 | 0 | `let g:ackpreview = 1`
`g:ack_use_dispatch` | 使用此选项用*vim-dispatch*在后台运行搜索，并为不同系统执行不同的后端。由于此时在*Dispatch*的限制，不支持*location-list*且结果窗口将出现在结果准备好之前。但是，这些可能对于那些搜索缓慢的大型项目是可接受的折衷方案。 | 0 | `let g:ack_use_dispatch = 1`
`g:ack_use_cword_for_empty_search` | 使用此选项启用空白搜索针对光标下单词运行。当此选项未设置，空白搜索将只输入错误信息。 | 1 | `let g:ack_use_cword_for_empty_search = 0`

##### 映射 #####
> 在*quickfix*与*location-list*窗口的快捷键

按键 | 说明
:-|:-
？ | 显示这些映射的快速摘要
o | 打开文件（如`Enter`）
O | 打开文件且关闭*quickfix*窗口
go | 预览文件（打开但保持焦点在ack.vim结果上）
t | 在新的标签打开
T | 在新的标签打开但不移动到新标签
h | 在水平分屏打开
H | 在水平分屏打开，保持焦点在结果上
v | 在垂直分屏打开
gv | 在垂直分屏打开，保持焦点在结果上
q | 关闭*quickfix*窗口

##### 忽略文件 #####
- .gitignore 和 .hgignore
- .agignore


### General ###
 
#### Plugin 'scrooloose/nerdtree'
#### Plugin 'spf13/vim-colors'
#### Plugin 'tpope/vim-surround'
##### 介绍 #####
移除，修改和添加成对符号的工具，包括括号，引号和标签。

##### 映射 #####
```
ds {TARGET} //删除。

cs {TARGET} {REPLACEMENT} //修改。
cS {TARGET} {REPLACEMENT} //修改，把围绕文本放在单独的行。

ys {MOTION or TEXTOBJECT} {REPLACEMENT} //添加。
yss {REPLACEMENT} //当前行添加，忽略前导空白。
ySS {REPLACEMENT} //当前行添加，缩进并将围绕文本放在单独的行。
yS {MOTION or TEXTOBJECT} {REPLACEMENT} //添加，缩进并将围绕文本放在单独的行。

vS {REPLACEMENT} //可视模式下添加
//行可视模式，围绕放在分开的行并缩进。
//块可视模式，每一行分开围绕。

vgS {REPLACEMENT} //可视模式下的"gS“
//行可视模式，阻止自动缩进。
//块可视模式，允许围绕根据'virtualedit'设置超过行的结束

//插入模式下
//插入成对符号，并把光标放在中间
<C-G>s {REPLACEMENT}
<C-G>S {REPLACEMENT}
<C-S> {REPLACEMENT}
```

##### 目标对象 #####
单个字符，基于Vim提供的`text-object`。
- 8个括号标记。`(` `)` `{` `}` `[` `]` `<` `>`对于`ds`和`cs`使用开括号时，包含的空白也会除去。`b` `B` `r` `a`作为`)` `}` `]` `>`的别名。
- 3个引号标记。`'` `"` `\`` 只在当前行搜索。
- `t`是一对*HTML*或者*XML*标签。默认对光标所在最内层标签处理，指定数字可以针对其他标签。
- `w` `W` `s`相当于`word` `WORD` `sentence`。这些没有什么可以删除，且使用`ds`没有操作。对于`cs`可以当作轻量的`ysi`(`cswb` == `ysiwb`, 多或者少)。
- `p`表示`paragraph`。类似于`w` `W`和`s`；不过会添加或者删除新行。

##### 替换对象 #####
单个字符。未定义的字符默认直接使用该字符。
- 使用`)` `}` `]` `>`添加适当的成对符号。`(` `{` `[`与其类似，不过会添加额外的空白字符在里面。`b` `B` `r` `a`对应`)` `}` `]` `>`。为实现在C风格语言对于代码块的通用需要，`<C-}>`添加花括号到与内容隔开的行(貌似按键冲突)。
- 使用`t`或者`<`， Vim提示作为一个HTML/XML标签插入。可以指定属性，标签会自动闭合。如果替换标签，属性会保留在新的标签里。用`>`结束你的输入抛弃那些属性。如果使用`<C-T>`，标签将自己显示在行上。
- 使用`s`， 添加一个前导但不包括尾部的空白。这对于来自csbs的方法调用移除圆括号是有用的。

##### 自定义 #####
- 针对特定文件，`autocmd FileType php let b:surround_45 = "<?php \r ?>"`
- 全局变量`let g:surround_45 = "<% \r %>"`，其中的`\r`会替换被围绕的字符，`45`为`-`键。
- 通过提示输入替换字符串，`let g:surround_108 = "\\begin{\1environment: \1}\r\\end{\1\1}"`，其中`\1`指定替换位置，`environment:`为提示字符，`108`为`l`键。
- 使用正则表达式应用替换，`let g:surround_108 = "\\begin{\environment: \1}\r\\end{\1\r}.\*\r\1}"`，其中`\1`中间的`\r`为正则表达式。
- 使用正则表达式应用替换，`let g:surround_{char2nr("d")} = "<div\1id: \r..*\r id=\"&\"\1>\r</div>"`，其中`char2nr`将字符转换成ascII码，提示输入id将匹配`\r..\*\r`并替换`&`所在位置。
- 插入模式下添加`let g:surround_insert_tail = "<++>"`。

#### Plugin 'tpope/vim-repeat'
#### Plugin 'jiangmiao/auto-pairs'
#### Plugin 'ctrlpvim/ctrlp.vim'
#### Plugin 'tacahiroy/ctrlp-funky'
#### Plugin 'kristijanhusak/vim-multiple-cursors'
多光标操作支持

#### Plugin 'vim-scripts/sessionman.vim'
窗口会话管理

#### Plugin 'matchit.zip'
 %匹配扩展支持不只单字符,支持更多语言Ada, ASP with VBS, Csh, DTD, Essbase, Fortran, HTML, JSP (same as HTML), LaTeX, Lua, Pascal, SGML, Shell, Tcsh, Vim, XML.

#### Plugin 'bling/vim-airline'
状态条  

#### Plugin 'powerline/fonts'
状态条字体

#### Plugin 'bling/vim-bufferline'
状态栏显示缓存文件名

#### Plugin 'Lokaltog/vim-easymotion'
快速跳转

#### Plugin 'jistr/vim-nerdtree-tabs'
nerdtree标签显示优化

#### Plugin 'flazz/vim-colorschemes'
#### Plugin 'mbbill/undotree'
#### Plugin 'nathanaelkane/vim-indent-guides'
#### Plugin 'vim-scripts/restore_view.vim'
#### Plugin 'mhinz/vim-signify'
#### Plugin 'tpope/vim-abolish.git'
#### Plugin 'osyo-manga/vim-over'
#### Plugin 'kana/vim-textobj-user'
#### Plugin 'kana/vim-textobj-indent'
#### Plugin 'gcmt/wildfire.vim'
### Writing ###

#### Plugin 'reedes/vim-litecorrect'
#### Plugin 'reedes/vim-textobj-sentence'
#### Plugin 'reedes/vim-textobj-quote'
#### Plugin 'reedes/vim-wordy'
### Programming ###

#### Plugin 'scrooloose/syntastic'
#### Plugin 'tpope/vim-fugitive'
#### Plugin 'mattn/webapi-vim'
#### Plugin 'mattn/gist-vim'
#### Plugin 'scrooloose/nerdcommenter'
#### Plugin 'tpope/vim-commentary'
#### Plugin 'godlygeek/tabular'
#### Plugin 'majutsushi/tagbar'
### Snippets & autoComplete ###

#### Plugin 'Valloric/YouCompleteMe'
#### Plugin 'SirVer/ultisnips'
#### Plugin 'honza/vim-snippets'
### Php ###

#### Plugin 'spf13/PIV'
#### Plugin 'arnaud-lb/vim-php-namespace'
#### Plugin 'beyondwords/vim-twig'
### Python ###

#### Plugin 'klen/python-mode'
#### Plugin 'yssource/python.vim'
#### Plugin 'python_match.vim'
#### Plugin 'pythoncomplete'
### Javascript ###

#### Plugin 'elzr/vim-json'
#### Plugin 'groenewege/vim-less'
#### Plugin 'pangloss/vim-javascript'
#### Plugin 'briancollins/vim-jst'
#### Plugin 'kchmck/vim-coffee-script'
### Html ###

#### Plugin 'amirh/HTML-AutoCloseTag'
#### Plugin 'hail2u/vim-css3-syntax'
#### Plugin 'gorodinskiy/vim-coloresque'
#### Plugin 'tpope/vim-haml'
#### Plugin 'mattn/emmet-vim'
### Ruby ###

#### Plugin 'tpope/vim-rails'
### Go ###

#### Plugin 'fatih/vim-go'
### Misc ###

#### Plugin 'rust-lang/rust.vim'
#### Plugin 'tpope/vim-markdown'
#### Plugin 'spf13/vim-preview'
#### Plugin 'tpope/vim-cucumber'
#### Plugin 'cespare/vim-toml'
#### Plugin 'quentindecock/vim-cucumber-align-pipes'
#### Plugin 'saltstack/salt-vim'
#### Plugin 'desertEx'

