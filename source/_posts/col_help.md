title: col命令帮助
categories: shell
date: 2016-12-30 23:38
tags: [shell, col, mac, 翻译]
---
当把man帮助文本输出到文件后多了很多不需要显示的控制字符，可以通过col过滤解决。
```
man {command}|col -b>{output_file}
```
下面是col命令的帮助：
COL(1)			  BSD General Commands Manual			COL(1)

NAME
     col -- filter reverse line feeds from input
     col -- 从输入过滤反向换行

SYNOPSIS
     col [-bfhpx] [-l num]
<!-- more -->
> DESCRIPTION
>   The col utility filters out reverse (and half reverse) line feeds so that the output is in the correct order with only forward and half forward line feeds, and replaces white-space characters with tabs where possible.
>   col实用程序过滤掉反向（和一半反向）换行，使得输出以正确的顺序，只有正向和半向前的换行，并在可能的情况下用制表符替换空格字符。
>   This can be useful in processing the output of nroff(1) and tbl(1).
>   这在处理nroff(1)和tbl(1)的输出时很有用。
> 
>   The col utility reads from the standard input and writes to the standard output.
>   col实用程序从标准输入读取并写入标准输出。
> 
>   The options are as follows:
>   选项如下：
> 
>   -b      Do not output any backspaces, printing only the last character written to each column position.
>   -b      不输出任何后退格，只打印写入每个列位置的最后一个字符。
> 
>   -f      Forward half line feeds are permitted (``fine'' mode).  Normally characters printed on a half line boundary are printed on the following line.
>   -f      允许前半换行（"fine"模式）。 通常，在半行边界上打印的字符打印在下一行上。
> 
>   -h      Do not output multiple spaces instead of tabs (default).
>   -h      不要输出多个空格，而是用制表符替代（默认）。
> 
>   -l num  Buffer at least num lines in memory.  By default, 128 lines are buffered.
>   -l num  至少缓冲内存中的num行。 默认情况下，128行被缓冲。
> 
>   -p      Force unknown control sequences to be passed through unchanged. Normally, col will filter out any control sequences from the input other than those recognized and interpreted by itself, which are listed below.
>   -p      强制未知控制序列不变地传递。 通常，col将从输入中过滤出除本身识别和解释的以外的任何控制序列，如下所列。
> 
>   -x      Output multiple spaces instead of tabs.
>   -x      输出多个空格而不是制表符。
> 
>   The control sequences for carriage motion that col understands and their decimal values are listed in the following table:
>   col所了解的转运动作的控制序列及其十进制值列在下表中：
> 
>   ESC-7	      reverse line feed (escape then 7)
>   ESC-8	      half reverse line feed (escape then 8)
>   ESC-9	      half forward line feed (escape then 9)
>   backspace	      moves back one column (8); ignored in the first column
>   carriage return  (13)
>   newline	      forward line feed (10); also does carriage return
>   shift in	      shift to normal character set (15)
>   shift out	      shift to alternate character set (14)
>   space	      moves forward one column (32)
>   tab	      moves forward to next tab stop (9)
>   vertical tab     reverse line feed (11)
> 
>   All unrecognized control characters and escape sequences are discarded.
>   丢弃所有无法识别的控制字符和转义序列。
> 
>   The col utility keeps track of the character set as characters are read and makes sure the character set is correct when they are output.
>   col实用程序在读取字符时跟踪字符集，并确保在输出时字符集是正确的。
> 
>   If the input attempts to back up to the last flushed line, col will display a warning message.
>   如果输入尝试备份到最后一个刷新行，col将显示警告消息。
> 
> ENVIRONMENT
>   The LANG, LC_ALL and LC_CTYPE environment variables affect the execution of col as described in environ(7).
>   LANG，LC_ALL和LC_CTYPE环境变量影响col的执行，如environ(7)中所述。
> 
> EXIT STATUS
>   The col utility exits 0 on success, and >0 if an error occurs.
>   col实用程序成功退出0，如果发生错误，则返回>0。
> 
> SEE ALSO
>   colcrt(1), expand(1), nroff(1), tbl(1)
> 
> STANDARDS
>   The col utility conforms to Version 2 of the Single UNIX Specification
>   col实用程序符合单UNIX规范的版本2
>   (``SUSv2'').
> 
> HISTORY
>   A col command appeared in Version 6 AT&T UNIX.
>   AT命令出现在版本6 AT＆T UNIX中。
> 
> BSD				August 4, 2004				   BSD
