title: hexo使用publish发布草稿
date: 2016-11-13 14:38:02
tags: [hexo,bug,问题,publish]
---
## 问题 ##

hexo使用publish发布草稿时报错FATAL Something's wrong.

<!-- more -->

## 解决 ##

```
Error: Draft "笔记-vim自动命令事件-md" does not exist.
    at /Users/user/Documents/work/hexo/node_modules/hexo/lib/hexo/post.js:197:22
    at tryCatcher (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/util.js:16:23)
    at Promise._settlePromiseFromHandler (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:497:31)
    at Promise._settlePromise (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:555:18)
    at Promise._settlePromise0 (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:600:10)
    at Promise._settlePromises (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:683:18)
    at Promise._fulfill (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:624:18)
    at Promise._resolveCallback (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:424:57)
    at Promise._settlePromiseFromHandler (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:510:17)
    at Promise._settlePromise (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:555:18)
    at Promise._settlePromise0 (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:600:10)
    at Promise._settlePromises (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:683:18)
    at Promise._fulfill (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:624:18)
    at Promise._resolveCallback (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:424:57)
    at ReductionPromiseArray._resolve (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/reduce.js:51:19)
    at Promise.completed [as _fulfillmentHandler0] (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/reduce.js:112:15)
    at Promise._settlePromise (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:552:21)
    at Promise._settlePromise0 (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:600:10)
    at Promise._settlePromises (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/promise.js:683:18)
    at Async._drainQueue (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/async.js:125:16)
    at Async._drainQueues (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/async.js:135:10)
    at Immediate.Async.drainQueues [as _onImmediate] (/Users/user/Documents/work/hexo/node_modules/hexo-fs/node_modules/bluebird/js/release/async.js:16:14)
    at tryOnImmediate (timers.js:534:15)
    at processImmediate [as _immediateCallback] (timers.js:514:5
```

根据报错堆栈可以确认问题是由于找不到文件导致。仔细检查文件名发现后面加了**-md**的文件类型后缀，调用时去掉后缀改成`hexo publish "笔记-vim自动命令事件"`成功发布。
