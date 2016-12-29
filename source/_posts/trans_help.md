title: trans命令帮助
categories: shell
date: 2016-12-29 23:13
tags: [shell, trans, mac, 翻译]
---

TRANS(1)							      TRANS(1)



NAME
       trans  -  Command-line translator using Google Translate, Bing Translator, Yandex.Translate, etc.
       trans  - 使用Google翻译，Bing翻译器，Yandex.Translate等的命令行翻译器。

SYNOPSIS
       trans [OPTIONS] [SOURCE]:[TARGETS] [TEXT]...

DESCRIPTION
 This tool translates text into any language from the command-line,  using  a translation engine such as Google Translate, Bing Translator and Yandex.Translate.
 此工具使用翻译引擎（例如Google翻译，Bing翻译器和Yandex.Translate）从命令行将文本翻译为任何语言。

 Each argument which is not a valid option is  treated  as  TEXT	to  be translated.
 每个不是有效选项的参数被视为要翻译的TEXT。

 If  neither  TEXT nor the input file is specified by command-line arguments, the program will read and translate from standard input.
 如果命令行参数既不指定TEXT也不指定输入文件，程序将从标准输入读取并转换。
<!-- more -->
```
    OPTIONS
   Information options
       -V, -version
	      Print version and exit.
        打印版本并退出。

       -H, -help
	      Print help message and exit.
        打印帮助消息并退出。

       -M, -man
	      Show man page and exit.
        显示手册页并退出。

       -T, -reference
	      Print reference table of all supported languages and codes,  and exit.   Names of languages are displayed in their endonyms (language name in the language itself).
        打印所有支持的语言和代码的参考表，然后退出。 语言名称显示在其内部名称（语言本身的语言名称）中。

       -R, -reference-english
	      Print reference table of all supported languages and codes,  and exit.  Names of languages are displayed in English.
        打印所有支持的语言和代码的参考表，然后退出。 语言名称以英语显示。

       -L CODES, -list CODES
	      Print  details  of  languages  and exit.	When specifying two or more language codes, concatenate them by plus sign "+".
        打印语言的详细信息并退出。 指定两个或多个语言代码时，请用加号+连接。

       -S, -list-engines
	      List available translation engines and exit.
        列出可用的翻译引擎和退出。

       -U, -upgrade
	      Check for upgrade of this program.
        检查此程序的升级。

   Translator options
       -e ENGINE, -engine ENGINE
	      Specify the translation engine to use.  (default: google)
        指定要使用的转换引擎。  （默认：google）

   Display options
       -verbose
	      Verbose mode.
        详细模式。

	      Show the original text and its most relevant  translation,  then its  phonetic  notation  (if any), then its alternative translations (if any) or its definition in the dictionary (if it  is  a word).
        显示原始文本及其最相关的翻译，然后是其语音符号（如果有），然后是其替代翻译（如果有）或其在字典中的定义（如果它是一个词）。

	      This  option  is unnecessary in most cases since verbose mode is enabled by default.
        在大多数情况下，此选项不是必需的，因为默认情况下启用详细模式。

       -b, -brief
	      Brief mode.
        简要模式。

	      Show the most relevant translation or its phonetic notation  only.
        只显示最相关的翻译或其语音符号。

       -d, -dictionary
	      Dictionary mode.
        字典模式。

	      Show the definition of the original word in the dictionary.
        在字典中显示原始单词的定义。

       -identify
	      Language identification.
        语言识别。

	      Show the identified language of the original text.
        显示原始文本的标识语言。

       -show-original Y/n
	      Show original text or not.  (default: yes)
        是否显示原文。  （默认值：yes）

       -show-original-phonetics Y/n
	      Show phonetic notation of original text or not.  (default: yes)
        是否显示原始文本的语音符号。  （默认值：yes）

       -show-translation Y/n
	      Show translation or not.	(default: yes)
        是否显示翻译。  （默认值：yes）

       -show-translation-phonetics Y/n
	      Show phonetic notation of translation or not.  (default: yes)
        是否显示翻译的语音符号。  （默认值：yes）

       -show-prompt-message Y/n
	      Show prompt message or not.  (default: yes)
        是否显示提示消息。  （默认值：yes）

       -show-languages Y/n
	      Show source and target languages or not.	(default: yes)
        是否显示源语言和目标语言。  （默认值：yes）

       -show-original-dictionary y/N
	      Show dictionary entry of original text or not.  (default: no)
        是否显示原始文本的字典条目。  （默认值：no）

	      This option is enabled in dictionary mode.
        此选项在字典模式中启用。

       -show-dictionary Y/n
	      Show dictionary entry of translation or not.  (default: yes)
        是否显示翻译的字典条目。  （默认值：yes）

       -show-alternatives Y/n
	      Show alternative translations or not.  (default: yes)
        是否显示可供选择的翻译。  （默认值：yes）

       -w NUM, -width NUM
	      Specify the screen width for padding.
        指定填充的屏幕宽度。

	      This  option overrides the setting of environment variable $COLUMNS.
        此选项覆盖环境变量$COLUMNS的设置。

       -indent NUM
	      Specify the size of indent (number of spaces).  (default: 4)
        指定缩进的大小（空格数）。  （默认值：4）

       -theme FILENAME
	      Specify the theme to use.  (default: default)
        指定要使用的主题。  （默认：默认）

       -no-theme
	      Do not use any other theme than default.
      不要使用除默认值以外的任何其他主题。

       -no-ansi
	      Do not use ANSI escape codes.
        不要使用ANSI转义代码。

       -no-bidi
	      Do not convert bidirectional texts.
        不要转换双向文本。

   Audio options
       -p, -play
	      Listen to the translation.
        听听翻译。

	      You must have at	least  one  of	the  supported	audio  players (mplayer,  mpv  or  mpg123)  installed  to  stream  from	Google Text-to-Speech engine.  Otherwise, a  local  speech  synthesizer may  be  used instead (say on Mac OS X, espeak on Linux or other platforms).
        您必须至少安装一个受支持的音频播放器（mplayer，mpv或mpg123）才能从Google文字转语音引擎流式传输。 否则，可以使用本地语音合成器（例如在Mac OS X上，espeak在Linux或其他平台上）。

       -speak Listen to the original text.
       -speak收听原文。

       -n VOICE, -narrator VOICE
	      Specify the narrator, and listen to the translation.
        指定叙述者，并听取翻译。

	      Common values for this option are male and female.
        此选项的公共值是男性和女性。

       -player PROGRAM
	      Specify the audio player to use, and listen to the  translation.
        指定要使用的音频播放器，并听取翻译。

	      Option  -play will try to use mplayer, mpv or mpg123 by default, since these players are known to work for streaming  URLs.   Not all  command-line audio players can work this way.  Use this option only when you have your own preference.
        选项 -play将试图使用mplayer，mpv或mpg123默认情况下，因为这些播放器已知的工作流URL。 并不是所有的命令行音频播放器都可以这样工作。 仅当您有自己的偏好时才使用此选项。

	      This option overrides the setting of environment variable $PLAYER.
        此选项覆盖环境变量$PLAYER的设置。

       -no-play
	      Do not listen to the translation.
        不要听翻译。

   Terminal paging and browsing options
   终端分页和浏览选项
       -v, -view
	      View the translation in a terminal pager (less, more or most).
        查看在终端分页中的翻译（更少，更多或最多）。

       -pager PROGRAM
	      Specify the terminal pager to use, and view the translation.
        指定要使用的终端分页器，并查看翻译。

	      This  option  overrides  the  setting  of  environment  variable $PAGER.
        此选项覆盖环境变量$PAGER的设置。

       -no-view
	      Do not view the translation in a terminal pager.
        不在终端分页中查看翻译。

       -browser PROGRAM
	      Specify the web browser to use.
        指定要使用的Web浏览器。

	      This  option  overrides  the  setting  of  environment  variable $BROWSER.
        此选项覆盖环境变量$BROWSER的设置。

   Networking options
       -x HOST:PORT, -proxy HOST:PORT
	      Use HTTP proxy on given port.
        在给定端口上使用HTTP代理。

	      This  option  overrides  the  setting  of  environment variables
        此选项覆盖环境变量的设置
	      $HTTP_PROXY and $http_proxy.

       -u STRING, -user-agent STRING
	      Specify the User-Agent to identify as.
        指定要标识为的用户代理。

	      This option overrides the setting of environment variables  $USER_AGENT.
        此选项覆盖环境变量的设置。

   Interactive shell options
   交互式shell选项
       -I, -interactive, -shell
	      Start  an  interactive  shell, invoking rlwrap whenever possible (unless -no-rlwrap is specified).
        启动交互式shell，尽可能调用rlwrap（除非指定了-no-rlwrap）。

       -E, -emacs
	      Start the GNU Emacs front-end for an interactive shell.
        启动交互式shell的GNU Emacs前端。

	      This option does not need to, and cannot be used along  with  -I or -no-rlwrap.
        此选项不需要，不能与-I或-no-rlwrap一起使用。

       -no-rlwrap
	      Do not invoke rlwrap when starting an interactive shell.
        启动交互式shell时不要调用rlwrap。

	      This  option  is useful when your terminal type is not supported by rlwrap (e.g.  emacs).
        当您的终端类型不受rlwrap（例如emacs）支持时，此选项很有用。

   I/O options
       -i FILENAME, -input FILENAME
	      Specify the input file.
        指定输入文件。

	      Source text to be translated will be read from the  input  file, instead of standard input.
        要翻译的源文本将从输入文件读取，而不是标准输入。

       -o FILENAME, -output FILENAME
	      Specify the output file.
        指定输出文件。

	      Translations  will  be  written  to  the output file, instead of standard output.
        翻译将被写入输出文件，而不是标准输出。

   Language preference options
   语言首选项选项
       -l CODE, -hl CODE, -lang CODE
	      Specify your home language (the language you would like  to  see for displaying prompt messages in the translation).
        指定您的首页语言（您希望在翻译中显示提示消息时使用的语言）。

	      This  option  affects only the display in verbose mode (anything other than source language and target language will be displayed in  your	home  language).   This  option has no effect in brief mode.
        此选项仅影响详细模式中的显示（除了源语言和目标语言之外的任何其他语言都将以您的母语显示）。 此选项在简要模式下无效。

	      This option is optional.	When its setting is  omitted,  English will be used.
        此选项是可选的。 当省略设置时，将使用英语。

	      This option overrides the setting of environment variables $LANGUAGE, $LC_ALL, $LANG and $HOME_LANG.
        此选项覆盖环境变量的设置，$LANGUAGE, $LC_ALL, $LANG and $HOME_LANG.

       -s CODE, -sl CODE, -source CODE, -from CODE
	      Specify the source language (the language of original text).
        指定源语言（原文的语言）。

	      This option is optional.	When its setting is omitted, the  language  of original text will be identified automatically (with a possibility of misidentification).
        此选项是可选的。 当其设置被省略时，将自动识别原始文本的语言（具有误识别的可能性）。

	      This  option  overrides  the  setting  of  environment  variable
	      $SOURCE_LANG.

       -t CODES, -tl CODE, -target CODES, -to CODES
	      Specify  the  target  language(s) (the language(s) of translated text).  When specifying two or more language codes,  concatenate them by plus sign "+".
        指定目标语言（翻译文本的语言）。 指定两个或多个语言代码时，请用加号+连接。

	      This  option  is	optional.  When its setting is omitted, everything will be translated into English.
        此选项是可选的。 当它的设置被省略时，一切都将被翻译成英语。

	      This option overrides the setting of environment variables $LAN-
	      GUAGE, $LC_ALL, $LANG and $TARGET_LANG.

       [SOURCE]:[TARGETS]
	      A  simpler,  alternative	way to specify the source language and target language(s) is to use a shortcut formatted string:
        指定源语言和目标语言的一种更简单的替代方法是使用快捷键格式化的字符串：

	      o SOURCE-CODE:TARGET-CODE

	      o SOURCE-CODE:TARGET-CODE1+TARGET-CODE2+...

	      o SOURCE-CODE=TARGET-CODE

	      o SOURCE-CODE=TARGET-CODE1+TARGET-CODE2+...

	      Delimiter ":" and "=" can be used interchangeably.

	      Either SOURCE or TARGETS may be omitted, but the delimiter character must be kept.
        可以省略SOURCE或TARGETS，但必须保留分隔符。

   Other options
       -no-init
	      Do not load any initialization script.
        不要加载任何初始化脚本。

       --     End-of-options.

	      All arguments after this option are treated as TEXT to be translated.
        此选项之后的所有参数都被视为要翻译的TEXT。

EXIT STATUS
       0      Successful translation.

       1      Error.

ENVIRONMENT
       PAGER  Equivalent to option setting -pager.

       BROWSER
	      Equivalent to option setting -browser.

       PLAYER Equivalent to option setting -player.

       HTTP_PROXY
	      Equivalent to option setting -proxy.

       USER_AGENT
	      Equivalent to option setting -user-agent.

       HOME_LANG
	      Equivalent to option setting -lang.

       SOURCE_LANG
	      Equivalent to option setting -source.

       TARGET_LANG
	      Equivalent to option setting -target.

FILES
       /etc/translate-shell
	      Initialization script.  (system-wide)

       $HOME/.translate-shell/init.trans
	      Initialization script.  (user-specific)

       ./.trans
	      Initialization script.  (current directory)

REPORTING BUGS
       <https://github.com/soimort/translate-shell/issues>

AUTHORS
       Mort Yao <soi@mort.ninja>.



0.9.4				  2016-05-18			      TRANS(1)
```
