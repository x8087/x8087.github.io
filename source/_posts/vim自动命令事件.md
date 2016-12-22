title: vim自动命令事件整理
tags:
  - vim
  - 笔记
  - 技巧
  - 帮助
date: 2016-11-12 15:09:00

---

vim参考文档内自动命令事件的翻译整理。

<!-- more -->

## 事件 ##
### 读取(Reading) ###
#### BufNewFile ####

starting to edit a file that doesn't exist
开始编辑不存在的文件。
Can be used to read in a skeleton file.
能用在一个构架文件读取。

#### BufReadPre ####

starting to edit a new buffer, before reading the file
开始编辑一个新的缓存，读取文件之前。
Not used if the file doesn't exist.
文件如果不存在不能使用

#### BufRead or BufReadPost ####

starting to edit a new buffer, after reading the file
开始编辑一个新的缓存，读取文件之后。

When starting to edit a new buffer, after reading the file into the buffer, before executing the modelines.  See |BufWinEnter|
当开始编辑一个新的缓存，读取文件进入缓存之后，执行模式行之前。参考|BufWinEnter|
for when you need to do something after processing the modelines.
为了当你需要在处理模式行之后做操作。
This does NOT work for ":r file".
变个对于":r file"不工作
Not used when the file doesn't exist.
当文件不存在时不可用。
Also used after successfully recovering a file.
也用在成功覆盖文件之后。
Also triggered for the filetypedetect group when executing ":filetype detect" and when writing an unnamed buffer in a way that the buffer gets a name.
同时当执行":filetype detect"时作为文件类型检测组和当书写未命名缓存获取名字时触发。

#### BufReadCmd ####

before starting to edit a new buffer |Cmd-event|
开始编辑新缓存之前|Cmd-event|
Should read the file into the buffer. |Cmd-event|
文件应已读入缓存。

#### FileReadPre ####

before reading a file with a ":read" command
通过":read"命令读取文件之前。

#### FileReadPost ####

after reading a file with a ":read" command
通过":read"命令读取文件之后。
Note that Vim sets the '[ and '] marks to the first and last line of the read.  This can be used to operate on the lines just read.
注意：vim在读取的首行和尾行设置'[和']标记。这个能用来在只读行上操作。

#### FileReadCmd ####

before reading a file with a ":read" command |Cmd-event|
通过":read"命令读取文件之前。
Should do the reading of the file. |Cmd-event|
应作用在文件读取中。

#### FilterReadPre ####

before reading a file from a filter command
过滤命令读取文件之前。
Vim checks the pattern against the name of the current buffer, not the name of the temporary file that is the output of the filter command.
Not triggered when 'shelltemp' is off.

#### FilterReadPost ####

after reading a file from a filter command
过滤命令读取文件之后。
After reading a file from a filter command.
Vim checks the pattern against the name of the current buffer as with FilterReadPre.
Not triggered when 'shelltemp' is off.

#### StdinReadPre ####

before reading from stdin into the buffer
从标准输入读取到缓存之前。
Before reading from stdin into the buffer.
Only used when the "-" argument was used when Vim was started |--|.

#### StdinReadPost ####

After reading from the stdin into the buffer
从标准输入读取到缓存之后。
After reading from the stdin into the buffer, before executing the modelines.  
Only used when the "-" argument was used when Vim was started |--|.

### 写入(Writing) ###

#### BufWrite ####

starting to write the whole buffer to a file
开始写整个缓存到文件。
Before writing the whole buffer to a file.

#### BufWritePre ####

starting to write the whole buffer to a file
开始写整个缓存到文件。
Before writing the whole buffer to a file.

#### BufWritePost ####

after writing the whole buffer to a file
写整个缓存到文件后。
(should undo the commands for BufWritePre).

#### BufWriteCmd ####

before writing the whole buffer to a file |Cmd-event|
写整个缓存到文件前。
Should do the writing of the file and reset 'modified' if successful, unless '+' is in 'cpo' and writing to another file |cpo-+|.
The buffer contents should not be changed.
When the command resets 'modified' the undo information is adjusted to mark older undo states as 'modified', like |:write| does. |Cmd-event|

#### FileWritePre ####

starting to write part of a buffer to a file
开始写部分缓存到文件。
Before writing to a file, when not writing the whole buffer.
Use the '[ and '] marks for the range of lines.

#### FileWritePost ####

after writing part of a buffer to a file
写部分缓存到文件后。
After writing to a file, when not writing the whole buffer.

#### FileWriteCmd ####

before writing part of a buffer to a file |Cmd-event|
写部分缓存到文件前。
Before writing to a file, when not writing the whole buffer.
Should do the writing to the file.
Should not change the buffer.
Use the '[ and '] marks for the range of lines. |Cmd-event|

#### FileAppendPre ####

starting to append to a file
开始附加到文件。
Before appending to a file.
Use the '[ and '] marks for the range of lines.

#### FileAppendPost ####

after appending to a file
附加到文件后。

#### FileAppendCmd ####

before appending to a file |Cmd-event|
附加到文件前。
Should do the appending to the file.
Use the '[ and '] marks for the range of lines.|Cmd-event|

#### FilterWritePre ####

starting to write a file for a filter command or diff
开始为过滤命令或diff写文件。
Before writing a file for a filter command or making a diff.
Vim checks the pattern against the name of the current buffer, not the name of the temporary file that is the output of the filter command.
Not triggered when 'shelltemp' is off.

#### FilterWritePost ####

after writing a file for a filter command or diff
为过滤命令或diff写文件后。
After writing a file for a filter command or making a diff.
Vim checks the pattern against the name of the current buffer as with FilterWritePre.
Not triggered when 'shelltemp' is off.

###   缓存(Buffers) ###

#### BufAdd ####

just after adding a buffer to the buffer list
添加缓存到缓存列表后。

#### BufCreate ####

just after adding a buffer to the buffer list
添加缓存到缓存列表后。
Just after creating a new buffer which is added to the buffer list, or adding a buffer to the buffer list.
Also used just after a buffer in the buffer list has been renamed.
The BufCreate event is for historic reasons.
NOTE: When this autocommand is executed, the current buffer "%" may be different from the buffer being created "<afile>".
	

#### BufDelete ####

before deleting a buffer from the buffer list
从缓存列表删除缓存前。
The BufUnload may be called first (if the buffer was loaded).
Also used just before a buffer in the buffer list is renamed.
NOTE: When this autocommand is executed, the current buffer "%" may be different from the buffer being deleted "<afile>" and "<abuf>".
Don't change to another buffer, it will cause problems. 

#### BufWipeout ####

before completely deleting a buffer
完全删除缓存前。
The BufUnload and BufDelete events may be called first (if the buffer was loaded and was in the buffer list).
Also used just before a buffer is renamed (also when it's not in the buffer list).
NOTE: When this autocommand is executed, the current buffer "%" may be different from the buffer being deleted "<afile>".
Don't change to another buffer, it will cause problems.

#### BufFilePre ####

before changing the name of the current buffer
更改当前缓存名字前。
with the ":file" or ":saveas" command.

#### BufFilePost ####

after changing the name of the current buffer
更改当前缓存名字后。
with the ":file" or ":saveas" command.

#### BufEnter ####

after entering a buffer
进入缓存前。
Useful for setting options for a file type.
Also executed when starting to edit a buffer, after the BufReadPost autocommands.


#### BufLeave ####

before leaving to another buffer
离开到另一个缓存前。
Also when leaving or closing the current window and the new current window is not for the same buffer.
Not used for ":qa" or ":q" when exiting Vim.

#### BufWinEnter ####

after a buffer is displayed in a window
缓存在窗口显示后。
This can be when the buffer is loaded (after processing the modelines) or when a hidden buffer is displayed in a window (and is no longer hidden).
Does not happen for |:split| without arguments, since you keep editing the same buffer, or ":split" with a file that's already open in a window, because it re-uses an existing buffer.
But it does happen for a ":split" with the name of the current buffer, since it reloads that buffer.

#### BufWinLeave ####

before a buffer is removed from a window
缓存从窗口移除前。
Not when it's still visible in another window.
Also triggered when exiting.
It's triggered before BufUnload or BufHidden.
NOTE: When this autocommand is executed, the current buffer "%" may be different from the buffer being unloaded "<afile>".
When exiting and v:dying is 2 or more this event is not triggered.

#### BufUnload ####

before unloading a buffer
卸下缓存前。
This is when the text in the buffer is going to be freed.
This may be after a BufWritePost and before a BufDelete.
Also used for all buffers that are loaded when Vim is going to exit.
NOTE: When this autocommand is executed, the current buffer "%" may be different from the buffer being unloaded "<afile>".
Don't change to another buffer, it will cause problems.
When exiting and v:dying is 2 or more this event is not triggered.

#### BufHidden ####

just after a buffer has become hidden
缓存变成隐藏后。
That is, when there are no longer windows that show the buffer, but the buffer is not unloaded or
deleted.
Not used for ":qa" or ":q" when exiting Vim.
NOTE: When this autocommand is executed, the current buffer "%" may be different from the buffer being unloaded "<afile>".

#### BufNew ####

just after creating a new buffer
创建新的缓存后。
Also used just after a buffer has been renamed.
When the buffer is added to the buffer list BufAdd will be triggered too.
NOTE: When this autocommand is executed, the current buffer "%" may be different from the buffer being created "<afile>".

#### SwapExists ####

detected an existing swap file
检测存在交换文件。
Detected an existing swap file when starting to edit a file.  
Only when it is possible to select a way to handle the situation, when Vim would ask the user what to do. 
The |v:swapname| variable holds the name of the swap file found, <afile> the file being edited.  |v:swapcommand| may contain a command to be executed in the opened file. 
The commands should set the |v:swapchoice| variable to a string with one character to tell Vim what should be done next: 'o'	open read-only 'e'	edit the file anyway 'r'	recover 'd'	delete the swap file 'q'	quit, don't edit the file 'a'	abort, like hitting CTRL-C When set to an empty string the user will be asked, as if there was no SwapExists autocmd. 
*E812* It is not allowed to change to another buffer, change a buffer name or change directory here.

### 选项(Options) ###

#### FileType ####

when the 'filetype' option has been set
当设置了'filetype'选项时。
The pattern is matched against the filetype.
<afile> can be used for the name of the file where this option was set, and <amatch> for the new value of 'filetype'. See |filetypes|.

#### Syntax ####

when the 'syntax' option has been set
当设置了'syntax'选项时。
When the 'syntax' option has been set.  
The pattern is matched against the syntax name. <afile> can be used for the name of the file where this option was set, and <amatch> for the new value of 'syntax'.
See |:syn-on|.

#### EncodingChanged ####

after the 'encoding' option has been changed
当'encoding'选项改变时。
Fires off after the 'encoding' option has been changed.
Useful to set up fonts, for example.
FileEncoding
Obsolete.  It still works and is equivalent to |EncodingChanged|.


#### TermChanged ####

after the value of 'term' has changed
当'term'选项值改变时。
After the value of 'term' has changed.  
Useful for re-loading the syntax file to update the colors, fonts and other terminal-dependent settings.  
Executed for all loaded buffers.

#### OptionSet ####

after setting any option
设置任何选项后。
The pattern is matched against the long option name. 
The |v:option_old| variable indicates the old option value, |v:option_new| variable indicates the newly set value, the |v:option_type| variable indicates whether it's global or local scoped and |<amatch>| indicates what option has been set. 
Is not triggered on startup and for the 'key' option for obvious reasons. 
Usage example: Check for the existence of the directory in the 'backupdir' and 'undodir' options, create the directory if it doesn't exist yet. 
Note: It's a bad idea to reset an option during this autocommand, this may break a plugin. 
You can always use `:noa` to prevent triggering this autocommand.


### 启动与退出(Startup and exit) ###

#### VimEnter ####

after doing all the startup stuff
完成所有启动操作后。
After doing all the startup stuff, including loading .vimrc files, executing the "-c cmd" arguments, creating all windows and loading the buffers in them.
Just before this event is triggered the |v:vim_did_enter| variable is set, so that you can do: >
```
if v:vim_did_enter
call s:init()
else
au VimEnter * call s:init()
endif
```

#### GUIEnter ####

after starting the GUI successfully
成功打开GUI后。
After starting the GUI successfully, and after opening the window.  
It is triggered before VimEnter when using gvim.  
Can be used to position the window from a .gvimrc file: >
:autocmd GUIEnter * winpos 100 50

#### GUIFailed ####

after starting the GUI failed
打开GUI失败后。
After starting the GUI failed.  
Vim may continue to run in the terminal, if possible (only on Unix and alikes, when connecting the X server fails).  
You may want to quit Vim: > :autocmd GUIFailed * qall

#### TermResponse ####

after the terminal response to |t_RV| is received
终端对|t_RV|的答复收到后。
After the response to |t_RV| is received from the terminal.  
The value of |v:termresponse| can be used to do things depending on the terminal version.  
Note that this event may be triggered halfway executing another event, especially if file I/O, a shell command or anything else that takes time is involved.

#### QuitPre ####

when using `:quit`, before deciding whether to quit
当使用':quit'，决定是否退出前。
When using `:quit`, `:wq` or `:qall`, before deciding whether it closes the current window or quits Vim.  
Can be used to close any non-essential window if the current window is the last ordinary window.

#### VimLeavePre ####

before exiting Vim, before writing the viminfo file
退出Vim前，写viminfo文件前。
Before exiting Vim, just before writing the .viminfo file.  
This is executed only once, if there is a match with the name of what happens to be the current buffer when exiting.
Mostly useful with a "*" pattern. 
` :autocmd VimLeavePre * call CleanupStuff() `
To detect an abnormal exit use |v:dying|.
When v:dying is 2 or more this event is not triggered.

#### VimLeave ####

before exiting Vim, after writing the viminfo file
退出Vim前，写viminfo文件前。
Before exiting Vim, just after writing the .viminfo file.  
Executed only once, like VimLeavePre.
To detect an abnormal exit use |v:dying|.
When v:dying is 2 or more this event is not triggered.

### 其他(Various) ###

#### FileChangedShell ####

Vim notices that a file changed since editing started
Vim通知文件自从编辑开始时的改变。
When Vim notices that the modification time of a file has changed since editing started.
Also when the file attributes of the file change or when the size of the file changes.
|timestamp|
Mostly triggered after executing a shell command, but also with a |:checktime| command or when Gvim regains input focus.
This autocommand is triggered for each changed file.
It is not used when 'autoread' is set and the buffer was not changed.
If a FileChangedShell autocommand is present the warning message and prompt is not given.
The |v:fcs_reason| variable is set to indicate what happened and |v:fcs_choice| can be used to tell Vim what to do next.
NOTE: When this autocommand is executed, the current buffer "%" may be different from the buffer that was changed "<afile>".
NOTE: The commands must not change the current buffer, jump to another buffer or delete a buffer.
*E246* *E811*
NOTE: This event never nests, to avoid an endless loop.
This means that while executing commands for the FileChangedShell event no other FileChangedShell event will be triggered.

#### FileChangedShellPost ####

After handling a file changed since editing started
处理文件自从编辑开始时的改变后。
After handling a file that was changed outside of Vim.
Can be used to update the statusline.

#### FileChangedRO ####

before making the first change to a read-only file
对只读文件做出首个改变前。
Before making the first change to a read-only file.
Can be used to check-out the file from a source control system.
Not triggered when the change was caused by an autocommand.
This event is triggered when making the first change in a buffer or the first change after 'readonly' was set, just before the change is applied to the text.
WARNING: If the autocommand moves the cursor the effect of the change is undefined.
*E788*
It is not allowed to change to another buffer here.
You can reload the buffer but not edit another one.
*E881*
If the number of lines changes saving for undo may fail and the change will be aborted.

#### ShellCmdPost ####

after executing a shell command
执行shell命令后。
After executing a shell command with |:!cmd|, |:shell|, |:make| and |:grep|.  
Can be used to check for any changed files.

#### ShellFilterPost ####

after filtering with a shell command
用shell命令过滤后。
After executing a shell command with ":{range}!cmd", ":w !cmd" or ":r !cmd".
Can be used to check for any changed files.

#### CmdUndefined ####

a user command is used but it isn't defined
使用未定义的用户命令时。
Useful for defining a command only when it's used.
The pattern is matched against the command name.  Both <amatch> and <afile> are set to the name of the command.
NOTE: Autocompletion won't work until the command is defined.
An alternative is to always define the user command and have it invoke an autoloaded function.
See |autoload|.

#### FuncUndefined ####

a user function is used but it isn't defined
使用未定义的用户函数时。
When a user function is used but it isn't defined.
Useful for defining a function only when it's used.
The pattern is matched against the function name.
Both <amatch> and <afile> are set to the name of the function.
NOTE: When writing Vim scripts a better alternative is to use an autoloaded function.
See |autoload-functions|.

#### SpellFileMissing ####

a spell file is used but it can't be found
使用一个拼写文件，但是找不到。
When trying to load a spell checking file and it can't be found.
The pattern is matched against the language.  <amatch> is the language, 'encoding' also matters.
See |spell-SpellFileMissing|.

#### SourcePre ####

before sourcing a Vim script
sourcing Vim脚本前。
|:source| <afile> is the name of the file being sourced.

#### SourceCmd ####

before sourcing a Vim script |Cmd-event|
sourcing Vim脚本前。
When sourcing a Vim script. |:source|
<afile> is the name of the file being sourced.
The autocommand must source this file.
|Cmd-event|

#### VimResized ####

after the Vim window size changed
Vim窗口尺寸改变后。
After the Vim window was resized, thus 'lines' and/or 'columns' changed.  
Not when starting up though.

#### FocusGained ####

Vim got input focus
Vim得到输入焦点时。
When Vim got input focus.
Only for the GUI version and a few console versions where this can be detected.

#### FocusLost ####

Vim lost input focus
Vim失去输入焦点时。
When Vim lost input focus.  Only for the GUI version and a few console versions where this can be detected.
May also happen when a dialog pops up.

#### CursorHold ####

the user doesn't press a key for a while
用户不按按键一会儿后。
When the user doesn't press a key for the time specified with 'updatetime'.
Not re-triggered until the user has pressed a key (i.e. doesn't fire every 'updatetime' ms if you leave Vim to make some coffee. :)  See |CursorHold-example| for previewing tags.
This event is only triggered in Normal mode.
It is not triggered when waiting for a command argument to be typed, or a movement after an operator.
While recording the CursorHold event is not triggered. |q| *<CursorHold>*
Internally the autocommand is triggered by the <CursorHold> key.
In an expression mapping |getchar()| may see this character.

Note: Interactive commands cannot be used for this event.  There is no hit-enter prompt, the screen is updated directly (when needed).
Note: In the future there will probably be another option to set the time.
Hint: to force an update of the status lines use: ` :let &ro = &ro `
{only on Amiga, Unix, Win32, MSDOS and all GUI versions}

#### CursorHoldI ####

the user doesn't press a key for a while in Insert mode
用户在插入模式不按按键一会儿后。
Just like CursorHold, but in Insert mode.
Not triggered when waiting for another key, e.g. after CTRL-V, and not when in CTRL-X mode |insert_expand|.

#### CursorMoved ####

the cursor was moved in Normal mode
在普通模式光标移动。
After the cursor was moved in Normal or Visual mode.
Also when the text of the cursor line has been changed, e.g., with "x", "rx" or "p".
Not triggered when there is typeahead or when an operator is pending.
For an example see |match-parens|.
Careful: This is triggered very often, don't do anything that the user does not expect or that is slow.

#### CursorMovedI ####

the cursor was moved in Insert mode
在插入模式光标移动。
After the cursor was moved in Insert mode.
Not triggered when the popup menu is visible.
Otherwise the same as CursorMoved.

#### WinNew ####

after creating a new window
创建一个新窗口后。
When a new window was created.  
Not done for the fist window, when Vim has just started.
Before a WinEnter event.

#### TabNew ####

after creating a new tab page
创建一个新标签页后。
When a tab page was created. |tab-page|
A WinEnter event will have been triggered first, TabEnter follows.

#### TabClosed ####

after closing a tab page
关闭一个标签页后。

#### WinEnter ####

after entering another window
进入另一个窗口后。
After entering another window.  
Not done for the first window, when Vim has just started.
Useful for setting the window height.
If the window is for another buffer, Vim executes the BufEnter autocommands after the WinEnter autocommands.
Note: When using ":split fname" the WinEnter event is triggered after the split but before the file "fname" is loaded.

#### WinLeave ####

before leaving a window
离开一个窗口前。
Before leaving a window.  
If the window to be entered next is for a different buffer, Vim executes the BufLeave autocommands before the WinLeave autocommands (but not for ":new").
Not used for ":qa" or ":q" when exiting Vim.


#### TabEnter ####

after entering another tab page
进入另一个标签页后。
Just after entering a tab page. |tab-page|
After triggering the WinEnter and before triggering the BufEnter event.

#### TabLeave ####

before leaving a tab page
离开一个标签页前。
Just before leaving a tab page. |tab-page|
A WinLeave event will have been triggered first.

#### CmdwinEnter ####

after entering the command-line window
进入命令行窗口后。
Useful for setting options specifically for this special type of window.
This is triggered _instead_ of BufEnter and WinEnter.
<afile> is set to a single character, indicating the type of command-line. |cmdwin-char|

#### CmdwinLeave ####

before leaving the command-line window
离开命令行窗口前。
Useful to clean up any global setting done with CmdwinEnter.
This is triggered _instead_ of BufLeave and WinLeave.
<afile> is set to a single character, indicating the type of command-line. |cmdwin-char|

#### InsertEnter ####

starting Insert mode
开始插入模式。
Just before starting Insert mode.  
Also for Replace mode and Virtual Replace mode.  
The |v:insertmode| variable indicates the mode.
Be careful not to do anything else that the user does not expect.
The cursor is restored afterwards.  
If you do not want that set |v:char| to a non-empty string.

#### InsertChange ####

when typing <Insert> while in Insert or Replace mode
当在插入或替换模式时键入<Insert>。
The |v:insertmode| variable indicates the new mode.
Be careful not to move the cursor or do anything else that the user does not expect.

#### InsertLeave ####

when leaving Insert mode
当离开插入模式时。
When leaving Insert mode.  
Also when using CTRL-O |i_CTRL-O|.  
But not for |i_CTRL-C|.

#### InsertCharPre ####

when a character was typed in Insert mode, before inserting it
当在插入模式键入字符时，插入它之前。
When a character is typed in Insert mode, before inserting the char.
The |v:char| variable indicates the char typed and can be changed during the event to insert a different character.  
When |v:char| is set to more than one character this text is inserted literally.
It is not allowed to change the text |textlock|.
The event is not triggered when 'paste' is set.

#### TextChanged ####

after a change was made to the text in Normal mode
在普通模式更改文本后。
After a change was made to the text in the current buffer in Normal mode.  
That is when |b:changedtick| has changed. 
Not triggered when there is typeahead or when an operator is pending. 
Careful: This is triggered very often, don't do anything that the user does not expect or that is slow.

#### TextChangedI ####

after a change was made to the text in Insert mode
在插入模式更改文本后。
After a change was made to the text in the current buffer in Insert mode.
Not triggered when the popup menu is visible.
Otherwise the same as TextChanged.

#### ColorScheme ####

after loading a color scheme
加载一个颜色方案后。
|:colorscheme|
The pattern is matched against the colorscheme name.
<afile> can be used for the name of the actual file where this option was set, and <amatch> for the new colorscheme name.

#### RemoteReply ####

a reply from a server Vim was received
Vim收到来自服务器的回应时。
When a reply from a Vim that functions as server was received |server2client()|.
The pattern is matched against the {serverid}. <amatch> is equal to the {serverid} from which the reply was sent, and <afile> is the actual reply string. 
Note that even if an autocommand is defined, the reply should be read with |remote_read()| to consume it.

#### QuickFixCmdPre ####

before a quickfix command is run
quickfix命令运行前。
Before a quickfix command is run (|:make|, |:lmake|, |:grep|, |:lgrep|, |:grepadd|, |:lgrepadd|, |:vimgrep|, |:lvimgrep|, |:vimgrepadd|, |:lvimgrepadd|, |:cscope|, |:cfile|, |:cgetfile|, |:caddfile|, |:lfile|, |:lgetfile|, |:laddfile|, |:helpgrep|, |:lhelpgrep|). 
The pattern is matched against the command being run.  
When |:grep| is used but 'grepprg' is set to "internal" it still matches "grep". 
This command cannot be used to set the 'makeprg' and 'grepprg' variables. 
If this command causes an error, the quickfix command is not executed.

#### QuickFixCmdPost ####

after a quickfix command is run
quickfix命令运行后。
Like QuickFixCmdPre, but after a quickfix command is run, before jumping to the first location. 
For |:cfile| and |:lfile| commands it is run after error file is read and before moving to the first error.
See |QuickFixCmdPost-example|.

#### SessionLoadPost ####

after loading a session file
加载一个会话文件后。
After loading the session file created using the |:mksession| command.

#### MenuPopup ####

just before showing the popup menu
显示弹出菜单前。
Just before showing the popup menu (under the right mouse button).  
Useful for adjusting the menu for what is under the cursor or mouse pointer.
The pattern is matched against a single character representing the mode:
n	Normal
v	Visual
o	Operator-pending
i	Insert
c	Command line

#### CompleteDone ####

after Insert mode completion is done
插入模式自动补全作用后。
Either when something was completed or abandoning completion. |ins-completion|
The |v:completed_item| variable contains information about the completed item.

#### User ####

to be used in combination with ":doautocmd"
用":doautocmd"使用组合。
Never executed automatically.  
To be used for autocommands that are only executed with ":doautocmd".


#### UserGettingBored ####
When the user presses the same key 42 times. Just kidding! :-)


