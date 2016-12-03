title: ftp命令帮助
date: 2015-09-14 15:29:29
tags: [shell, zsh, ftp, mac]
---
**ftp**网络文件传输程序
ftp [-46AadefginpRtvV] [-N netrc] [-o output] [-p port] [-q quittime] [ -s srcaddr] [-r retry] [-T dir,max[,inc]] [[user@]host [port]] [[user@]host:[path][/]] [file://path] [ftp://[user[:password]@]host[:port]/path[/][;type=X]][http://[user[:password]@]host[:port]/path] [...]
ftp -u URL file [...]

<!-- more -->
### 描述：###
ftp是网络标准传输协议的用户接口。程序允许用户与远程网络站点传输文件。
选项可指定给命令行或者命令解释器。
-4 强制ftp只使用IPv4地址
-6 强制ftp只使用IPv6地址
-A 强制使用主动模式ftp。默认情况，ftp尝试使用被动模式ftp且当服务器不支持被动模式时回退到主动模式。这个选项使ftp总是使用主动连接。这只是对连接非常老可能没有实现被动模式的服务器有用。
-a 使ftp绕过普通登录程序，并替换为使用匿名登录。
-d 开启调试
-e 关闭命令行编辑。这对Emacs ange-ftp模式有用。
-f 强制经过ftp或者http代理的传输缓存重载。
-g 关闭文件名扩展。
-i 在多文件传输时关闭交互提示。
-n 阻止ftp在初始化非"auto-fetch"传输连接时尝试"auto-login"。如果允许自动登录，ftp会检查用户Home目录里的.netrc文件获取描述远程端账户的入口。如果入口不存在，ftp会提示远程端登录名（默认是用户本地机器上定义的）且如果有需要时提示登录账号的密码。覆盖"auto-fetch"传输的自动登录，指定合适的用户名（和可选的，密码）。
-N netrc 使用netrc替代~/.netrc。更多信息请参阅.netrc文件。
-o output 当自动获取文件时，保存内容到`output`。`output`根据下面的文件命名规则解析。如果`output`没有'-'或者不是以'|'开头，那只有指定的第一个文件在`output`中检索到。所有其他文件在他们远程名字的basename里检索到。
-p 启用被动模式操作，用于在连接过滤防火墙后面使用。Ftp现在默认尝试使用被动模式已不推荐使用此选项，如果服务器不支持被动连接则回退到主动模式。
-P port 设置端口号为`port`。
-q quittime 在连接停滞`quitime`秒后退出。
-r wait 如果连接失败，暂停`wait`秒尝试重新连接。
-R 重新启动所有非代理的自动提取
-s srcaddr 使用srcaddr作为所有连接的本地IP地址
-t 启用数据包跟踪
-T direction,maximum[,increment] 为`direction`设置最大传输速度到`maximum`字节/秒，如果指定，则增量为`increment`字节/秒。更多信息参考`rate`。
-u URL file [...] 在命令行上传文件到`URL`，`URL`是支持`auto-fetch`的ftp URL类型之一（对单个文件的上传附加一个可选目标文件名)，`file`是即将上传的一个或多个文件。
-v 启用详情和进度。如果输出是输出到终端这是默认的（对于进度，ftp是前台程序）。强制ftp显示所有来自远程服务端的响应，以及对数据传输的统计报告。
-V 禁用详情和进度，如果输出是输出到终端，覆盖默认的启用。

与ftp通讯的客户端主机可以在命令行中指定。如果这样的话，ftp会立即试图在主机上建立一个与ftp服务器的连接；否则，ftp会进入它的命令解释器并等待来自用户的指令。当等待来自用户的命令时，ftp提供给用户`ftp>`的提示。下面是ftp认可的命令：

! [command [args]]
在本地机器上调用一个交互式的shell。如果有参数，第一个是直接执行的命令，其余的参数作为其参数。

$ macro-name [args]
执行macdef命令定义的宏`macro-name`。参数传递到宏unglobbed。

account [passwd]
一旦登录用于访问资源的远程系统成功完成，提供所需的补充密码。如果没有包含任何参数，将提示用户在无回响输入模式输入用户密码。

append local-file [remote-file]
将本地文件附加到远程计算机上的文件。如果未指定`remote-file`，本地文件名被用来命名被任何ntrans或者nmap设置改变后的远程文件。文件传输使用当前的类型，格式，模式，结构配置。

ascii
设置文件网络传输类型ASCII。这是默认类型。

bell
设定在每个文件传输命令完成后响铃提示。

binary
设置文件传输类型支持二进制图像传输。

bye
终止与远程服务器的FTP会话并退出ftp。文件结尾也会终止会话并退出。

case
切换远程计算机文件名大小写在`get`，`mget`，和`mput`命令时的映射。当`case`开启（默认关闭），远程计算机文件名所有大写字母映射成小写写进本地目录。

cd remote-directory
将远程机器上的工作目录更改为`remote-directory`。

cdup
将远程机器上的工作目录更改为当前远程机器工作目录的父目录。

chmod mode remote-file
更改远程系统上的`remote-file`文件权限模式为`mode`。

close
终止与远程服务器的FTP会话，并返回到命令解释器。任何定义的宏被擦除。

cr
切换在ascii类型文件检索时回车提取。记录是记在ascii类型文件传输时的回车/换行序列。当`cr`开启（默认），回车从序列中去除，以符合UNIX单换行记录定界符。在非UNIX远程系统上的记录可能包含单换行符；当有一个ascii类型传输时，这些换行只有当`cr`关闭时才可能与记录定界符区分。

ftp_debug [ftp_debug-value]
切换调试模式。如果可选`ftp_debug-value`指定，可以用来设置调试等级。当调试开启时，ftp打印每条发往远程机器的命令，以`-->`字符串开始。

delete remote-file
删除在远程机器上的文件`remote-file`

dir [remote-path [local-file]]
打印远程机器上的目录内容清单。清单包括服务器指定的任何系统相关信息；例如，大多数UNIX系统通过`ls -l`产生的输出。如果没有指定`remote-path`，使用当前工作目录。如果交互提示开启，ftp会提示用户确认最后一个参数是接收`dir`输出的目标本地文件。如果没有指定本地文件，或者如果`local-file`是`-`，输出发送到终端。

disconnect
与close相同

edit
切换命令行编辑，上下文相关命令和文件补全。从终端输入时自动开启，否则禁用。

epsv4
切换在IPv4连接时使用扩展EPSV和EPRT命令。首先尝试EPSV/EPRT，然后PASV/PORT。默认开启。如果扩展命令失败，选项会在当前连接期间临时禁用，或者直到epsv4再次执行。

exit
与bye相同。

features
显示远程服务器支持什么功能（使用FEAT命令）。

fget localfile
检索`localfile`列出的文件，每个文件名占一行。

form format
将文件传输格式设置成`format`。默认（只支持）格式`non-print`。

ftp host [port]
与open相同

gate [host [post]]
切换到`gate-ftp`模式，通过`TIS FWTK`和`Gauntlet ftp proxies`连接。如果gate-ftp服务器还没设置这将不被允许（由用户显示声明，或者来自`FTPSERVER`环境变量）。如果`host`给定，那么`gate-ftp`模式开启，且`gate-ftp`服务器将设置为`host`。如果`port`也给定，将使用作为在gate-ftp服务器上连接的端口。

get remote-file [local-file]
检索`remote-file`并在本地机器上保存。如果没有指定本地文件名，给予在远程机器上相同的名字，受当前case，ntrans和nmap设置变更影响。当传输文件时使用当前类型，方式，模式和结构设置。

glob
切换对`mdelete`，`mget`，`mput`和`mreget`的文件名扩展。如果使用`glob`关闭匹配，文件名参数按字面意思理解且不扩展。`mput`的匹配和在`csh`下是一样的。对于`mdelete`，`mget`和`mreget`，每个远程文件名在远程机器上分别扩展且不合并列表。目录名扩展多半和普通文件名不相同：确切的结果取决于外来的操作系统和ftp服务器，能通过`mls remote-files -`预览。注意：`mget`，`mput`和`mreget`并不意味传输文件的整个目录树。这可以通过传输目录树的`tar`归档实现（使用二进制模式）。

hash [size]
切换`hash-sign`（`#`）对每个数据块传输的打印。默认的数据块大小为1024字节。这能通过使用字节为单位指定`size`改变。启用hash会禁用进度progress。

help [command]
打印关于`command`含义的关键信息。如果没有指定参数，ftp打印已知命令列表。

idle [seconds]
设置在远程服务器上的闲置定时为`seconds`秒。如果`seconds`省略，打印当前闲置定时。

image
与`binary`相同。

lcd [directory]
更改在本地机器上的工作目录。如果未指定`directory`，使用用户的`home`目录。

less file
与`page`相同。

lpage local-file
使用`pager`选项设置的指定程序显示`local-file`。

lpwd
打印在本地机器上的工作目录。

ls [remote-path] [local-file]
与`dir`相同。

macdef macro-name
定义一个宏。后续行存储为宏`macro-name`；一个空行（文件里的连续换行字符或者终端中输入回车）终止宏输入模式。所有定义的宏有16个宏和4096个总字符的限制。宏名称最多8个字符。宏只适用于当前它们定义的会话内（或者在外部定义的会话中定义的会话调用下一个打开的命令），并保持定义直到执行关闭命令。要调用宏，请使用`$`命令（请看上面）。
宏处理器将`$`和`\\`作为特殊字符解释。`$`后面带一个数字被替换为宏调用命令行相应的参数。`$`后面带一个`i`指示宏处理器循环执行宏。传进来的第一个`$i`被替换成宏调用命令行的第一个参数，第二个传递的替换为第二个参数，等等。`\\`后带任何字符替换为该字符。使用`\\`可以防止`$`特殊处理。

mdelete [remote-files]
删除远程机器上的`remote-files`

mdir remote-files local-file
像`dir`，除了可以指定多个远程文件。如果交互提示开启，ftp会提示用户确认最后一个参数确实是接收`mdir`输出的目标本地文件。

mget remote-files
扩展在远程机器上的`remote-files`并获取每个因此产生的文件名。在文件名扩展上的细节查看`glob`。结果文件名随后将经过`case`，`ntrans`和`nmap`设置处理。文件被转移到本地工作目录，本地工作目录能由`lcd directory`改变；新的本地目录能使用`! mkdir directory`创建。

mkdir directory-name
在远程机器上创建目录

mls remote-files local-file
像`ls`，除了可以指定多个远程文件和必须指定`local-file`。如果交互提示开启，ftp会提示用户确认最后一个参数确实是接收`mls`输出的目标本地文件。

mlsd [remote-path]
以机器解析(machine-parsable)形式，使用`MLSD`显示`remote-path`的内容（如果没给应该默认当前目录）。显示格式能用`remopts mlst ...`更改。

mlst [remote-path]
以机器解析(machine-parsable)形式，使用`MLST`显示`remote-pat`的相关详细。（如果没给应该默认当前目录）。显示格式能用`remopts mlst ...`更改。

mode mode-name
设置文件传输模式为`mode-name`。默认（只支持）模式是`stream`。

modtime remote-file
显示在远程机器上的文件最后修改时间，使用RFC2822格式。

more file
与page相同。

mput local-file
扩展搭配作为参数给定的本地文件列表并放置结果列表中的每个文件。文件名扩展详细查看`glob`。结果文件名随后经过`ntrans`和`nmap`设置处理。

mreget remote-file
当作每个`mget`，但执行`reget`替代`get`。

msend local-files
与`mput`相同。

newer remote-file [local-file]
只有当远程文件的修改时间比当前系统文件更新，才获取文件。如果文件在当前系统不存在，则该远程文件被认为更加新。否则，这个命令等同于`get`。

nlist [remote-path [local-file]]
与`ls`相同。

nmap [inpattern outpattern]
设置或撤消文件名映射机制。如果没有指定参数，文件名映射机制撤消。如果指定参数，远程文件名在`mput`命令和`put`命令未生成远程目标文件名时被映射。如果指定参数，本地文件名在`mget`命令和`get`命令未生成指定本地目标文件名时映射。这个命令在使用不同的文件命名规则或惯例连接非UNIX远程计算机时相当有用。映射遵循`inpattern`和`outpattern`的模式设置。[`Inpattern`]是传入的文件名模板（可能已经经过`ntrans`和`case`设置处理)。可变模板由包含在`inpattern`内的序列`$1`,`$2`,...`$9`实现。使用`\\`防止`$`字符特殊处理。所有其他字符按字面意思对待，并用于确定`nmap [inpattern]`变量值。例如，给定`inpattern` $1.$2且远程文件名"mydata.data"，$1将获得值"mydata"，$2获得值"data"。`outpattern`确定映射文件名结果。序列`$1`,`$2`,...`$9`替换为来自`inpattern`模板的任何结果值。序列`$0`替换为原始文件名。此外，序列`[seq1,seq2]`如果`seq1`不是空字符串替换为`[seq1]`，否则替换为`seq2`。例如，命令`nmap $1.$2.$3 [$1,$2].[$2.file]`输入文件名"myfile.data"和"myfile.data.old"产生输出文件名"myfile.data"，输入文件名"myfile"产生"myfile.file"，输入文件名".myfile"产生"myfile.myfile"。空格可以包含在`outpattern`，例如：`nmap $1 sed s/   *$// >$1`
使用`\\`防止字符'$','[',']'和','特殊处理。

ntrans [inchars [outchars]]
设置或撤消文件名字符转换机制。如果没有指定参数，文件名字符转换机制撤消。如果指定参数，远程文件名字符在`mput`命令和`put`命令未生成指定远程目标文件名时被转换。如果指定参数，本地文件名字符在`mget`命令和`get`命令未生成指定本地目标文件名时被转换。这个命令在使用不同的文件命名规则或惯例连接非UNIX远程计算机时相当有用。文件名内的字符匹配`inchars`内的字符被替换为`outchars`内相应的字符。如果`inchars`内的字符位置比`outchars`的长度长，那么字符从文件名删除。

open host [port]
与指定的`host`FTP服务器建立连接。可提供可选端口号，这种情况下，ftp将尝试在该端口联系FTP服务器。如果`auto-login`选项开启（默认），ftp同时尝试自动登录FTP服务器账户（见下文）。

page file
检索文件并用`pager`选项设定的指定程序显示。

passive [auto]
切换被动模式（如果没有给定参数）。如果给定`auto`，像`FTPMODE`设置为`auto`一样动作。如果被动模式开启（默认），ftp将为所有数据连接发送PASV命令替代PORT命令。PASV命令请求远程服务器为数据连接打开端口并返回端口地址。远程服务器监听与客户端连接的该端口。当使用更传统的的PROT命令，客户端监听端口并发送地址给远程服务器连接回来。被动模式在使用ftp经过控制交通方向的网关路由或者主机时非常有用。（注意：虽然FTP服务器需要支持RFC1123 PASV命令，但是有些没有。）

pdir [remote-path]
执行`dir [remote-path]`，并用`pager`选项指定的程序显示结果。

pls [remote-path]
执行`ls [remote-path]`，并用`pager`选项指定的程序显示结果。

pmlsd [remote-path]
执行`mlsd [remote-path]`，并用`pager`选项指定的程序显示结果。

preserve
切换在检索文件时保持修改时间。

progress
切换显示传输进度条。对于含有`-`的`local-file`或者命令以`|`开始的传送会禁用进度条。更多信息参考`文件命名规则``File NAMING CONVERTIONS`。开启`progress`会禁用`hash`。

prompt
切换交互式提示。交互式提示发生在多文件传输时允许用户选择性地检索或者存储文件。如果提示关闭（默认是开启），任何`mget`或者`mput`会传输所有文件，`mdelete`会删除所有文件。
当提示开启，下面的命令在提示可用：
a 当前文件回复`yes`，当前命令剩下的所有文件自动回复`yes`。
n 回复`no`，不传输文件。
p 当前文件回复`yes`，并关闭提示模式（类似给定`prompt off`）。
q 终止当前操作。
y 回复`yes`，传输文件。
？显示帮助消息。
任何其他回复相当于当前文件回复`yes`。

proxy ftp-command
在间接控制连接上运行ftp命令。这个命令允许同时连接两个远程ftp服务器在两个服务器之间传输文件。首先代理命令应该打开，用来建立间接控制连接。输入命令"proxy ?"查看在间接连接上其他可运行的ftp命令。下面的命令在`proxy`开端时表现不同：`open`在自动登录过程中不定义新的宏，`close`不擦除存在的宏定义，`get`和`mget`从主控制连接的主机往间接连接的主机传输文件，`put`和`mput`从间接控制连接主机往主控制连接主机附加传输文件。第三方文件传输依赖于间接控制连接服务器的FTP协议PASV命令支持。

put local-file [remote-file]
在远程机器保存本地文件。如果`remote-file`未指定，使用通过`ntrans`或者`nmap`设置处理后的本地文件名命名远程文件。文件传输使用当前的类型，格式，模式和结构设置。

pwd
打印远程机器上的当前工作目录名字。

quit
与`bye`相同。

quote arg1 arg2 ...
指定参数逐字发送到远程FTP服务器。

rate direction [maximum [increment]]
压制最大传输速率为`maximum`字节/秒。如果`maximum`为0，禁用压制。
`direction`可能是其中的：
all 两个目录。
get 输入传输。
put 输出传输。
`maximum`在飘荡中每次给定的信号被接收可改动`increment`字节（默认：1024）：
SIGUSR1 `increment`字节`maximum`增量。
SIGUSR2 `increment`字节`maximum`减量。结果必须是正整数。
如果没有提供`maximum`，显示当前压制速率。
注意：速率还没实现ascii模式的传输。

rcvbuf size
设置socket接收缓存大小为`size`。

recv remote-file [local-file]
与`get`相同。

reget remote-file [local-file]
`reget`动作像`get`，除了如果`local-file`存在并比`remote-file`小，`local-file`被假定为部分转移`remote-file`拷贝并从明显的失败点继续转移。这命令当传输非常大的文件经过容易断开连接的网络时是有用的。

remopts command [command-option]
设置`command`到`command-options`（缺省以特定命令基准处理）在远程FTP服务器上的选项。远程FTP命令所知支持选项包括：`MLST`（用于`MLSD`和`MLST`）。

rename [from [to]]
在远程机器上重命名文件`from`为文件`to`。

reset
清除应答队列。这个命令重新同步命令/应答的远程FTP服务器的排序。同步可能需要由远程服务器违反FTP协议。

restart marker
在指定的`marker`重新开始立即接着`get`或者`put`。在UNIX系统，标记通常是文件的字节偏移。

rhelp [command-name]
向远程FTP服务器请求帮助。如果指定`command-name`，则提供给服务器。

rmdir directory-name
删除远程机器上的目录。

restatus [remote-file]
不带参数，显示远程机器状态。如果指定`remote-file`，显示远程机器上的`remote-file`状态。

runique
切换在本地系统用唯一文件名存储文件。如果文件已经存在，与`get`或者`mget`命令的目标本地文件名名称相同，名称附加“.1”。如果结果名字匹配另一个存在的文件，在原本的名称附加“.2”。如果这个过程继续到“.99”，打印一个错误消息，并且不发生转移。生成的唯一文件名将被报告。注意`runique`不影响来自shell命令的本地文件生成（看下文）。默认值是关闭off。

send local-file [remote-file]
与`put`相同。

sendport
切换使用`PORT`命令。默认，ftp当建立连接时为每个数据传输尝试使用`PORT`命令。使用`PORT`命令能在执行多文件传输时防止延时。如果`PORT`命令失败，如果`PORT`命令失败，ftp使用默认数据端口。当禁用`PORT`命令，不会尝试每个数据传输使用`PORT`命令。这对于某些忽略`PORT`命令实现的FTP是有不正确，但有用的，表明他们已经接受。

set [option value]
设置`option`为`value`。如果`option`和`value`没有给定，显示所有选项和他们的值。当前支持的选项有：
anonpass 默认为$FTPANONPASS
ftp_proxy 默认为$ftp_proxy。
http_proxy 默认为$http_proxy。
no_proxy 默认为$no_proxy。
pager 默认为$PAGER。
prompt 默认为$FTPPROMPT。
rprompt 默认为$FTPRPROMPT。

site arg1 arg2 ...
作为`SITE`命令逐字发送的指定参数到远程FTP服务器。

size remote-file
返回远程机器的`remote-file`大小。

sndbuf size
设置socket发送缓存大小为`size`。

status
显示当前ftp状态。

struct struct-name
设置文件传输结构为`struct-name`。默认（只支持）结构`file`。

sunique
切换在远程机器用唯一文件名存储文件。要成功完成，远程FTP服务器必须支持FTP协议STOU命令。远程服务器会报告唯一名字。默认值是关闭off。

system
显示在远程机器上运行的保作系统类型。

tenex
设置需要告诉`TENEX`机器的文件传输类型。

throttle
与rate相同。

trace
切换包跟踪。

type [type-name]
设置文件传输类型为`type-name`。如果没有指定类型，打印当前类型。默认类型是网络ASCII。

umask [newmask]
设置在远程服务器上的默认`umask`为`newmask`。如果`newmask`省略，打印当前`umask`。

unset option
撤消`option`。更多信息参考`set`。

usage command
打印`command`的用例消息。

user user-name [password] [account]]
确定自己的远程FTP服务器。如果`password`未指定且服务请求它，ftp将向用户提示它（禁止本地回响后）。如果`account`字段未指定，且FTP服务器请求它，将向用户提示。如果`account`字段指定，如果远程服务器没有用`account`命令请求登入，登录序列完成后，将接替远程服务器。除非ftp调用`auto-login`失效，这个过程在初始化到FTP服务器的连接自动完成。

verbose
切换详情模式。在详情模式，所有来自FTP服务器的响应都显示给用户。此外，如果`verbose`开启，当文件传输完成，报告传输效率统计关系。默认为开启。

xferbuf size
设置socket发送和接收的缓存大小为`size`。

? [command]
与`help`相同。

带有嵌入空格的命令参数可以用`"`引用。
切换设置命令能采用明确的`on`或者`off`参数强制合适地设置。
以字节数作为参数的命令（例如`hash`,`rate`,`xferbuf`）支持参数可选后缀，改变参数的解释。支持的后缀有：
  b 不引起改变。（可选）
  k Kilo；参数乘以1024
  m Mega；参数乘以1048576
  g Gige；参数乘以1073741824
如果ftp接收`SIGINFO`（查看stty(1) `status`参数）或者`SIGQUIT`信号同时一个传输在进行中，当前的传输速率统计将被写到标准错误输出，以相同的标准完成消息格式。


