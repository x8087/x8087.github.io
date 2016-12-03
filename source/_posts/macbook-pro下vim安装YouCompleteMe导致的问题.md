title: macbook pro下vim安装YouCompleteMe导致的问题
date: 2015-08-11 10:15:11
categories: 问题
tags: [问题,bug,技术,vim,macbook,YouCompleteMe]
---
##### 问题：vim下缺少一个好用的代码补全功能
**解决方案：**
>使用[YouCompleteMe][]

<!-- more -->
---
##### 问题：安装[YouCompleteMe][]后，打开报错vim版本过低
需要人更新最新版本vim
mac os10.10自带的vim版本过低
**解决方案：**
>1.本地编译最新版本Vim；<br/>
2.通过home brew安装[MacVim][]([YouCompleteMe][]与[MacVim][]协作比vim更好);

---
##### 问题：本地编译vim与[YouCompleteMe][]的python版本不一致，vim打开报错
本地编译vim源码时使用的是系统自带的python，而安装[YouCompleteMe][]与平常使用的是home brew的python
**解决方案：**
>找不到好的解决方案，放弃

---
##### 问题：将MacVim.app里的`vim`二进制文件链接到`/usr/local/bin`下使用报错
**解决方案：**
>找不到问题所在，放弃。
转而采用`alias`的方法链接

**更新：**[<font color="blue">2015-08-11 16:37:22</font>](#2015-08-11_16:37)


---
##### 问题：当采用`alias`方法链接时，shell脚本内使用vim报错
在控制台使用正常，在shell脚本使用时`alias`失效，调用的是系统自带的vim
通过在脚本中输出Path环境变量，vim版本和所使用的vim位置，可以确认并没有使用`alias`指定的vim。
**解决方案**
>由于暂时不确定什么原因导致，找不到更好的解决方法，只能在脚本中添加`alias`链接来解决。

**更新：**[<font color="blue">2015-08-11 16:37:22</font>](#2015-08-11_16:37)

---


##### 后期更新：

###### 2015-08-11_16:37
---

>从[MacVim][]下载[MacVim][] Snapshot文件，解压后把文件mvim放到`/usr/local/bin`目录下并建立软链接：(详细可查看[MacVim][]帮助`:h mvim`)


```bash
ln -s /usr/local/bin/mvim vim
ln -s /usr/local/bin/mvim view
ln -s /usr/local/bin/mvim vimdiff
ln -s /usr/local/bin/mvim vi
ln -s /usr/local/bin/mvim vimex
```

[YouCompleteMe]: https://github.com/Valloric/YouCompleteMe
[macvim]: https://github.com/macvim-dev/macvim/releases
