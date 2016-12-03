title: 疯狂Vim折腾之插件
date: 2015-08-07 20:20:57
categories: 工具
tags: [vim,插件,YouCompleteMe,Eclim]
---
介绍vim语法补全插件YouCompleteMe

<!-- more -->
##### Mac上快速安装[YouCompleteMe][]
1. 安装[MacVim][]
 - 通过brew快速安装`brew install macvim`
 - 保证正确运行可复制从MacVim下载的mvim脚本文件到本地二进制输出文件夹并添加软链接
 - 控制台vim
    - 推荐使用MacVim.app包内的vim二进制文件(MacVim.app/Contents/MacOS/Vim)
    - 由于homebrew的配置编译方法有问题，不推荐使用homebrew的vim
    - 使用homebrew的python以于MacVim可能出现编译问题或者编译后启动不了，需要运行`brew rm python; brew install python`更新到最新
2. 通过Vundle安装YouCompleteMe
  - 注意更新后要重新调用install.sh编译
  - C-family completion需要最新的CLT和Xcode
3. 安装CMake
4. `./install.sh`可选项
 - C语言支持:`--clang-completer`
 - C#支持:`--omnisharp-completer`
 - Go支持:`--gocode-completer`

5. C语言的语法补全还需要添加配置flags

##### [YouCompleteMe][]使用方法
1.基础用法
- 使入字符，自动提示。输入的字符越多，可选项越准
- 智能大小写，大小写敏感。输入大写过滤只包含该大写字符选项，小写则包含该字符大小写选项
- Tab选取或遍历选项，shit-Tab反方向选取。*(注意：控制台下的shift-Tab按键可能无法使用，需另外关联)*
- 补全列表排列时，会将符合子字符项的选项排在前面。每个子字符串首字母大写，首字符为下划线时忽略下划线。   
- Ctrl-space快捷键可直接调出补全列表，而不需要输入任何字符。

2.c语言补全
- YCM会自动在文件打开的目录及子目录下查找.ycm_extra_conf.py文件
- 也可以配置全局的.ycm_extra_conf.py文件
- 范例[.ycm_extra_conf.py](https://github.com/Valloric/ycmd/blob/master/cpp/ycm/.ycm_extra_conf.py)，仅供参考，不同项目可能不一样，大部分项目都只需要使用文件中的部分标志
- [Clang's CompilationDatabase system](http://clang.llvm.org/docs/JSONCompilationDatabase.html)通过CMake生成文件只需在项目的CMakeList.txt文件里加入`set(CMAKE_EXPORT_COMPILE_COMMANDS 1)`
- [YCM-Generator](https://github.com/rdnetto/YCM-Generator)ycm_extra_conf.py文件生成工具
- 如果编译错误或者文件包含的头文件有错误，会使生成补全列表的时间比较久且生成的列表项出现很无关补全项，调用`:YcmDiags`查看错误

3.其他语言的补全
- Pyhon,C#和Go语言各自通过[Jedi](https://github.com/davidhalter/jedi),[Omnisharp](https://github.com/nosami/OmniSharpServer)和[GoCode](https://github.com/nsf/gocode)语言引擎提供补全，需要安装编译时添加相应选项
- 对于没有本地语法引擎支持的文件类型，可以使用`omnifunc`(`:h omnifunc`)作为语法补全源，Vim为多种语言自带omnifuncs，类似Ruby,PHP
- 通过[Eclim][]可以为Java和Ruby提供非常好的omnifuncs
    - 安装完[Eclim][]后需要在Vim里输入`:ProjectCreate <path-to-your-project> -n ruby` (or `-n java`)创建新的Eclip项目并在.vimrc文件内加入`let g:EclimCompletionMethod = 'omnifunc'`使Vim与Eclim更好运行 

4.编写新的语法补全
- 通过编写Vim的`omnifunc`实现
- 通过自定义YCM补全功能实现
- 两者的不同：
    - omnifunc通过VimScript编写，另一个则使用Python根据YCM的API编写
    - YCM API是一种更强大的方法和YCM集成，且提供更宽泛的功能设置
    - YCM方案执行效率会更高
- vim参考`:h complete-functions`，YCM参考[API文档](https://github.com/Valloric/ycmd/blob/master/ycmd/completers/completer.py)

##### [Eclim](http://eclim.org)
提供通过命令行或网络连接调用Eclipse代码编辑的功能，允许集成到自己喜欢的编辑器上。
[macvim]: https://github.com/macvim-dev/macvim/releases
[youcompleteme]: https://github.com/Valloric/YouCompleteMe#general-usage
