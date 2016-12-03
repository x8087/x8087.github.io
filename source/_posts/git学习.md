title: git学习
date: 2015-07-30 17:46:31
categories: 技术
tags: [git,笔记,版本控制]
---
### 1. 取得项目的Git仓库
- 在工作目录中初始化新仓库
  `git init`
  `git add`
- 从现有仓库克隆
  `git clone [url]`
  `git clone [url] [addr]`

---

<!-- more -->
### 2. 记录每次更新到仓库
- 文件状态 `untracked 未跟踪`, `unmodified 未修改`, `modified 已修改`, `staged 已缓存`

##### 2.1 检查当前文件状态 `git status`
##### 2.2 跟踪新文件`git add`
##### 2.3 暂存已修改文件`git add`
##### 2.4 忽略某些文件`.gitignore`
- 忽略空行及`#`开头的行
- 后跟`/`表示目录
- 模式前加`!`表示取反，忽略模式以外的文件或目录
- 以标准glob模式匹配
 - 指shell所使用简化了的正则表达式
 - `*`匹配0个或多个任意字符
 - `[abc]`可选匹配
 - `?`匹配一个任意字符
 - `[0-9]`匹配指定范围字符
 - `**/`递归匹配子目录（1.8.2以上版本）

##### 2.5 查看已暂存和未暂存的文件
 - `git diff`比较工作目录中当前文件与暂存文件之间的差异 
 - `git diff --cached`或`git diff --staged`（1.6.1以上版本，效果一样）比较暂存文件与上次提交文件之间的差异

##### 2.6 提交更新
 - `git commit`
 - `git config --global core.editor`设定编辑器
 - `-v`显示详细差异

##### 2.7 跳过使用暂存区域
 - `git commit -a`

##### 2.8 移除文件
 - `git rm`
 - `git rm -f`强制删除包含在暂存区的文件
 - `git rm --cached`删除版本控制，保留本地文件
 - 后面可跟文件或者目录的名字，也可以使用glob模式
 - 模式前加反斜杠`\`，使用git自身的文件模式扩展匹配方式，`*`会递归匹配
 
##### 2.9 移动文件
 - `git mv file_from file_to`
