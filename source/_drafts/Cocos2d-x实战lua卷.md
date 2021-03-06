title: Cocos2d-x实战lua卷
date: 2015-08-17 21:42:25
categories: 笔记
tags: [笔记, cocos2d-x, lua, 技术]
---
### 基础篇

#### 1. 准备开始

##### 1.1 本书学习路径图
##### 1.2 使用实例代码

#### 2. lua语言基础

##### 2.1 环境搭建

###### 2.1.1 lua编辑工具
- [Eclipse 的lua Development Tools(LDT)插件](http://www.eclipse.org/koneki/ldt/)
- [在线安装插件地址](http://download.eclipse.org/koneki/releases/stable)
- 需要JRE（Java运行环境）和JDK（Java开发工具包）支持

###### 2.1.2 HelloLua实例测试

##### 2.2 标识符和保留字

###### 2.2.1 标识符
- 区分大小写
- 首字母必须是下划线（_）、美元符（$）或者字母，不能是数字
- 其他字符可以是下划线、美元符、字母或数字组成的
- 采用ASCCII编码

###### 2.2.2 保留字
and\break\do\else\elseif\end\false\for\function\if\in\local\nil\not\or\repeat\return\then\true\until\while

##### 2.3 常量和变量

###### 2.3.1 常量
无

###### 2.3.2 变量
local关键字修饰的变量是局部变量
变量赋值之前值为nil

###### 2.3.3 命名规范
1. 常量名：全大写，多个单词用下划线隔开
2. 变量名：驼峰式，变量和函数首字母小写，对象首字母大写

##### 2.4 注释
1. 单行注释：--
2. 多行注释：--[[ .... --]]可嵌套

##### 2.5 Lua数据类型

###### 2.5.1 数据类型
1. 数值类型：包括整数和浮点数
2. 布尔类型
3. 字符串类型
4. 自定义类型：与C进行交互
5. 函数类型
6. 线程类型
7. 表类型
8. nil

###### 2.5.2 type函数
返回变量或数值的类型

###### 2.5.3 数据类型转换
1. 转换成字符串：tostring(t)
2. 转换成数字：tonumber(t)

##### 2.6 运算符

###### 2.6.1 算术运算符
+-*/%^

###### 2.6.2 关系运算符
==,~=,>,<,>=,<=

###### 2.6.3 逻辑运算符
and,or,not

###### 2.6.4 运算优先级
1. ^
2. * /
3. + -
4. < > <= >= ~= ==
5. and
6. or

##### 2.7 控制语句

###### 2.7.1 分支语句 ######
```
if 条件表达式 then
  语句组
end

if 条件表达式 then
  语句组
else
  语句组
end

if 条件表达式 then
  语句组
else if 条件表达式 then
  语句组
eles if 条件表达式 then
  语句组
else
  语句组
end
```

###### 2.7.2 循环语句 ######
1. while语句

###### 2.7.3 跳转语句 ######

##### 2.8 表类型 #####

###### 2.8.1 字典 ######

###### 2.8.2 数组 ######

##### 2.9 字符串类型 #####

###### 2.9.1 字符串截取 ######

###### 2.9.2 字符串转换 ######

###### 2.9.3 字符串查询 ######

###### 2.9.4 字符串格式化 ######

##### 2.10 函数 #####

###### 2.10.1 使用函数 ######

###### 2.10.2 变得作用域 ######

###### 2.10.3 多重返回值 ######

##### 2.11 闭包函数 #####

###### 2.11.1 嵌套函数 ######

###### 2.11.2 返回函数 ######

###### 2.11.3 使用闭包表达式 ######

##### 2.12 Lua中的面向对象 #####

###### 2.12.1 Lua中的对象 ######

###### 2.12.2 类的实现 ######

#### 3. Hello Cocos2d-x Lua ####

##### 3.1 移动平台游戏引擎介绍  #####

##### 3.2 Cocos2d 游戏引擎 #####

###### 3.2.1 Cocos2d游戏引擎家谱 ######

###### 3.2.2 Cocos2d-x引擎 ######

###### 3.2.3 JavaScript和Lua绑定 ######

##### 3.3 搭建Coco2d-x Lua开发环境 #####

###### 3.3.1 搭建Cocos Code IDE开发环境 ######

###### 3.3.2 下载和使用Cocos2d-x Lua官方案例 ######

##### 3.4 第一个Cocos2d-x Lua游戏 #####

###### 3.4.1 创建工程 ######

###### 3.4.2 Cocos Code IDE 运行 ######

###### 3.4.3 工程文件结构 ######

###### 3.4.4 代码解释 ######

##### 3.5 重构HelloLua #####

##### 3.6 Cocos2d-x Lua 核心概念 #####

###### 3.6.1 导演 ######

###### 3.6.2 场景 ######

###### 3.6.3 层 ######

###### 3.6.4 精灵 ######

###### 3.6.5 菜单 ######

##### 3.7 Node与Node层级架构 #####

###### 3.7.1 Node中重要的操作 ######

###### 3.7.2 Node中重要的属性 ######

###### 3.7.3 游戏循环与调度 ######

##### 3.8 Cocos2d-x Lua坐标系 #####

###### 3.8.1 UI坐标 ######

###### 3.8.2 OpenGL坐标 ######

###### 3.8.3 世界坐标和模型坐标 ######

#### 4. 标签和菜单 ####

##### 4.1 使用标签 #####

###### 4.1.1 LabelTTF ######

###### 4.1.2 LabelAtlas ######

###### 4.1.3 LabelBMFont ######

###### 4.1.4 Cocos2d-x 3.x 标签类Label ######

##### 4.2 使用菜单 #####

###### 4.2.1 文本菜单 ######

###### 4.2.2 精灵菜单和图片菜单 ######

###### 4.2.3 开关菜单 ######

#### 5. 精灵 ####

##### 5.1 Sprite精灵类 #####

###### 5.1.1 创建Sprite精灵对象 ######

###### 5.1.2 实例：使用纹理对象创建Sprite ######

##### 5.2 精灵的性能优化 #####

###### 5.2.1 使用纹理图集 ######

###### 5.2.2 使用精灵帧缓存 ######

#### 6. 场景与层 ####

##### 6.1 场景与层的关系 #####

##### 6.2 场景切换 #####

###### 6.2.1 场景切换相关函数 ######

###### 6.2.2 场景过渡动画 ######

##### 6.3 场景的生命周期 #####

###### 6.3.1 生命周期函数 ######

###### 6.3.2 多场景切换生命周期 ######

#### 7. 动作、特效和动画 ####

##### 7.1 动作 #####

###### 7.1.1 瞬时动作 ######

###### 7.1.2 间隔动作 ######

###### 7.1.3 组合动作 ######

###### 7.1.4 动作速度控制 ######

###### 7.1.5 函数调用 ######

##### 7.2 特效 #####

###### 7.2.1 网格动作 ######

###### 7.2.2 实例特效演示 ######

##### 7.3 动画 #####

###### 7.3.1 帧动画 ######

###### 7.3.2 实例：帧动画的使用 ######

#### 8. 用户事件 ####

##### 8.1 事件处理机制 #####

###### 8.1.1 事件分发器 ######

###### 8.1.2 触摸事件 ######

###### 8.1.3 实例：单点触摸事件 ######

###### 8.1.4 实例：多点触摸事件 ######

###### 8.1.5 键盘事件 ######

##### 8.2 加速度计与加速度事件 #####

###### 8.2.1 加速度计 ######

###### 8.2.2 加速度计事件 ######

###### 8.2.3 实例：运动的小球 ######

### 进阶篇 ###

#### 9. 游戏背景音乐与音效 ####

##### 9.1 Cocos2d-x Lua中音频文件 #####

###### 9.1.1 音频文件介绍 ######

###### 9.1.2 Cocos2d-x Lua跨平台音频支持 ######

##### 9.2 使用Audio Engine引擎 #####

###### 9.2.1 音频文件的预处理 ######

###### 9.2.2 播放背景音乐 ######

###### 9.2.3 停止播放背景音乐 ######

##### 9.3 实例：设置背景音乐与音效 #####

###### 9.3.1 GameScene场景实现 ######

###### 9.3.2 SettingScene场景实现 ######

#### 10. 粒子系统 ####

##### 10.1 问题的提出 #####

##### 10.2 粒子系统的基本概念 ######

###### 10.2.1 实例：打火机 ######

###### 10.2.2 粒子发射模式 ######

###### 10.2.3 粒子系统属性 ######

##### 10.3 Cocos2d-x内置粒子系统 ######

###### 10.3.1 内置粒子系统 ######

###### 10.3.2 实例：内置粒子系统 ######

##### 10.4 自定义粒子系统 #####

###### 10.4.1 代码创建 ######

###### 10.4.2 plist文件创建 ######

#### 11. 瓦片地图 ####

##### 11.1 地图的性能问题 #####

##### 11.2 Cocos2d-x Lua中瓦片地图API #####

##### 11.3 实例：忍者无敌 #####

###### 11.3.1 设计地图 ######

###### 11.3.2 程序中加载地图 ######

###### 11.3.3 移动精灵 ######

###### 11.3.4 检测碰撞 ######

###### 11.3.5 滚动地图 ######

#### 12. 物理引擎 ####

##### 12.1 使用物理引擎 #####

###### 12.1.1 物理引擎核心概念 ######

###### 12.1.2 物理引擎与精灵关系 ######

##### 12.2 Cocos2d-x Lua中物理引擎 #####

###### 12.2.1 Cocos2d-x Lua物理引擎API ######

###### 12.2.2 实例：HelloPhysicsWorld ######

###### 12.2.3 实例：碰撞检测 ######

###### 12.2.4 实例：使用关节 ######

### 数据与网络篇 ###

#### 13. 数据持久化 ####

##### 13.1 使用FileUtils访问文件 #####

###### 13.1.1 Cocos2d-x Lua中的目录 ######

###### 13.1.2 实例：读取文件 ######

###### 13.1.3 实例：路径搜索 ######

##### 13.2 持久化概述 #####

##### 13.3 UserDefault数据持久化 #####

###### 13.3.1 UserDefaultAPI ######

###### 13.3.2 实例：保存背景音乐和音效设置 ######

##### 13.4 属性列表数据持久化#####

###### 13.4.1 属性列表概述 ######

###### 13.4.2 实例：访问根为字典列表结构的属性列表文件 ######

###### 13.4.3 实例：访问根为列表结构的属性列表文件 ######

#### 14. 基于HTTP的网络编程 ####

##### 14.1 网络结构 #####

###### 14.1.1 客户端服务器结构网络 ######

###### 14.1.2 点对点结构网络 ######

##### 14.2 HTTP与HTTPS协议 #####

##### 14.3 使用XMLHttpRequest对象开发客户端 #####

###### 14.3.1 使用XMLHttpRequest对象 ######

###### 14.3.2 实例：MyNotes ######

##### 14.4 数据交换格式 #####

##### 14.5 JSON数据交换格式 #####

###### 14.5.1 文档结构 ######

###### 14.5.2 JSON解码与编码 ######

###### 14.5.3 实例：完善MyNotes ######

#### 15 Node.js与WebSocket网络通信 ####

##### 15.1 Node.js #####

###### 15.1.1 Node.js安装 ######

###### 15.1.2 Node.js测试 ######

##### 15.2 使用WebSocket #####

###### 15.2.1 使用Node.js开发WebSocket服务端服务 ######

###### 15.2.2 Cocos2d-x Lua客户端 ######

##### 15.3 实例：WebSocket重构MyNotes #####

###### 15.3.1 WebSocket服务端开发 ######

###### 15.3.2 服务器端Node.js访问SQLLite数据库 ######

###### 15.3.3 Cocos2d-x Lua客户端开发 ######

### 优化篇 ###

#### 16. 性能优化 ####

##### 16.1 合理使用缓存 #####

###### 16.1.1 场景与资源 ######

###### 16.1.2 缓存创建和清除的时机 ######

##### 16.2 图片与纹理优化 #####

###### 16.2.1 选择图片格式 ######

###### 16.2.2 拼图 ######

###### 16.2.3 纹理像素格式 ######

###### 16.2.4 纹理缓存异步加载 ######

###### 16.2.5 背景图片优化 ######

##### 16.3 声音优化 #####

###### 16.3.1 声音格式优化 ######

###### 16.3.2 声音预处理与清除 ######

### 跨平台移值篇 ###

#### 17. 移植到Android平台 ####

##### 17.1 搭建交叉编译和打包环境 #####

###### 17.1.1 Android SDK安装 ######

###### 17.1.2 管理Android SDK ######

###### 17.1.3 管理Android 开发模拟器 ######

###### 17.1.4 Android NDK安装 ######

##### 17.2 创建Cocos2d-x Lua工程 #####

##### 17.3 交叉编译 #####

##### 17.4 打包运行 #####

##### 17.5 移植问题汇总 #####

###### 17.5.1 Lua文件编译问题 ######

###### 17.5.2 横屏与竖屏设置问题 ######

#### 18. 移植到IOS平台 ####

##### 18.1 IOS开发环境搭建 #####

###### 18.1.1 Xcode安装和卸载 ######

###### 18.1.2 Xcode操作界面 ######

##### 18.2 创建Cocos2d-x Lua工程 #####

##### 18.3 编译与发布 #####

##### 18.4 移植问题汇总 #####

###### 18.4.1 IOS平台声音移植问题 ######

###### 18.4.2 使用PVR纹理格式 ######

###### 18.4.3 横屏与竖屏设置问题 ######

##### 18.5 多分辨率屏幕适配 #####

###### 18.5.1 问题的提出 ######

###### 18.5.2 Cocos2d-x Lua屏幕适配 ######

###### 18.5.3 分辨率策略 ######

###### 18.5.4 纹理图集资源适配 ######

###### 18.5.5 瓦片地图资源适配 ######

### 实战篇 ###

#### 19. 使用Git管理程序代码版本 ####

##### 19.1 代码版本管理工具——Git #####

###### 19.1.1 版本控制历史 ######

###### 19.1.2 术语与基本概念 ######

###### 19.1.3 Git环境配置 ######

###### 19.1.4 Git常用命令 ######

##### 19.2 代码托管服务——GitHub #####

###### 19.2.1 创建和配置GitHub账号 ######

###### 19.2.2 创建代码库 ######

###### 19.2.3 删除代码库 ######

###### 19.2.4 派生代码库 ######

###### 19.2.5 GitHub协同开发 ######

##### 19.3 实例：Cocos2d-x Lua游戏项目协同开发 #####

###### 19.3.1 提交到GitHub代码库 ######

###### 19.3.2 克隆GitHub代码库 ######

###### 19.3.3 重新获得GitHub代码库 ######

#### 20. Cocos2d-x Lua 敏捷开发项目实战——迷失航线手机游戏 ####

##### 20.1 迷失航线游戏分析与设计 #####

###### 20.1.1 迷失航线故事背景 ######

###### 20.1.2 需求分析 ######

###### 20.1.3 原型设计 ######

###### 20.1.4 游戏脚本 ######

##### 20.2 任务1:游戏工程的创建与初始化 #####

###### 20.2.1 迭代1.1:创建工程 ######

###### 20.2.2 迭代1.2:添加资源文件 ######

###### 20.2.3 迭代1.3:添加常量文件SystemConst.lua ######

###### 20.2.4 迭代1.4:多分辨率支持 ######

###### 20.2.5 迭代1.5:发布到GitHub ######

##### 20.3 任务2:创建Loading场景 #####

###### 20.3.1 迭代2.1:添加场景和层 ######

###### 20.3.2 迭代2.2:Loading动画 ######

###### 20.3.3 迭代2.3:异步加载纹理缓存 ######

##### 20.4 任务3:创建Home场景 #####

###### 20.4.1 迭代3.1:添加场景和层 ######

###### 20.4.2 迭代3.2:添加菜单 ######

##### 20.5 任务4:创建设置场景 #####

##### 20.6 任务5:创建帮助场景 #####

##### 20.7 任务6:游戏场景实现 #####

###### 20.7.1 迭代6.1:创建敌人精灵 ######

###### 20.7.2 迭代6.2:创建玩家飞机精灵 ######

###### 20.7.3 迭代6.3:创建炮弹精灵 ######

###### 20.7.4 迭代6.4:初始化游戏场景 ######

###### 20.7.5 迭代6.5:游戏场景菜单实现 ######

###### 20.7.6 迭代6.6:玩家飞机发射炮弹 ######

###### 20.7.7 迭代6.7:炮弹与敌人的碰撞检测 ######

###### 20.7.8 迭代6.8:玩家飞机与敌人的碰撞检测 ######

###### 20.7.9 迭代6.9:玩家飞机生命值显示 ######

###### 20.7.10 迭代6.10:显示玩家得分情况 ######

##### 20.8 任务7:游戏结束场景 #####

#### 21. 为迷失航线游戏添加广告 ####

##### 21.1 使用谷歌AdMob广告 #####

###### 21.1.1 注册AdMob账号 ######

###### 21.1.2 管理AdMob广告 ######

###### 21.1.3 AdMob广告类型 ######

###### 21.1.4 下载谷歌AdMob Ads SDK ######

##### 21.2 为迷失航线游戏Android平台添加AdMob广告 #####

###### 21.2.1 Google Play服务下载与配置 ######

###### 21.2.2 导入libcocos2dx类库工程到Eclipse ######

###### 21.2.3 导入LostRoutes工程到Eclipse ######

###### 21.2.4 编写AdMod相关代码 ######

###### 21.2.5 交叉编译、打包和运行 ######

##### 21.3 为迷失航线游戏IOS平台添加AdMob广告 #####

###### 21.3.1 Cocos2d-x引擎IOS平台下AdMod开发环境搭建 ######

###### 21.3.2 编写AdMob相关代码 ######

#### 22. 把迷失航线游戏发布到Google play应用商店 ####

##### 22.1 谷歌Android应用商店Google Play #####

##### 22.2 还有“最后一公里” #####

###### 22.2.1 Lua文件编译 ######

###### 22.2.2 添加图标 ######

###### 22.2.3 应用程序打包 ######

##### 22.3 发布产品 #####

###### 22.3.1 上传APK ######

###### 22.3.2 填写商品详细信息 ######

###### 22.3.3 定价和发布范围 ######

#### 23. 把迷失航线游戏发布到苹果的App Store ####

##### 23.1 苹果的App Store #####

##### 23.2 IOS设备测试 #####

###### 23.2.1 创建开发者证书 ######

###### 23.2.2 设备注册 ######

###### 23.2.3 创建App ID ######

###### 23.2.4 创建配置概要文件 ######

###### 23.2.5 设备上运行 ######

##### 23.3 还有“最后一公里” #####

###### 23.3.1 添加图标 ######

###### 23.3.2 添加启动界面 ######

###### 23.3.3 修改发布产品属性 ######

###### 23.3.4 为发布进行编译 ######

###### 23.3.5 应用打包 ######

##### 23.4 发布产品 #####

###### 23.4.1 创建应用及基本信息 ######

###### 23.4.2 应用定价信息 ######

###### 23.4.3 基本信息输入 ######

###### 23.4.4 上传应用前的准备 ######

###### 23.4.5 上传应用 ######

##### 23.5 常见审核不通过的原因 #####

###### 23.5.1 功能问题 ######

###### 23.5.2 用户界面问题 ######

###### 23.5.3 商业问题 ######

###### 23.5.4 不当内容 ######

###### 23.5.5 其他问题 ######
