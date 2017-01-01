title: tee命令帮助
categories: shell
date: 2017-01-01 15:29
tags: 
  - shell
  - tee
  - mac
  - 翻译
---
TEE(1)			  BSD General Commands Manual			TEE(1)

> NAME
>      tee -- pipe fitting
> 
> SYNOPSIS
>    tee [-ai] [file ...]
> 
> DESCRIPTION
>    The tee utility copies standard input to standard output, making a copy in zero or more files.  The output is unbuffered.
>    tee实用程序将标准输入复制到标准输出，从而在零个或多个文件中复制。 输出未缓冲。
> 
>    The following options are available:
>    以下选项可用：
> 
>    -a      Append the output to the files rather than overwriting them.
>    -a      将输出附加到文件，而不是覆盖它们。
> 
>    -i      Ignore the SIGINT signal.
>    -i      忽略SIGINT信号。
> 
>    The following operands are available:
>    以下操作数可用：
> 
>    file  A pathname of an output file.
>    file  输出文件的路径名。
> 
>    The tee utility takes the default action for all signals, except in the event of the -i option.
>    tee实用程序对所有信号采用默认操作，但在-i选项的情况下除外。
> 
>    The tee utility exits 0 on success, and >0 if an error occurs.
>    tee实用程序成功退出0，如果发生错误，则为>0。
> 
> STANDARDS
>    The tee function is expected to be POSIX IEEE Std 1003.2 (``POSIX.2'') compatible.
>    tee功能预期为POSIX IEEE Std 1003.2（POSIX.2“）兼容。
> 
> BSD				 June 6, 1993				   BSD
