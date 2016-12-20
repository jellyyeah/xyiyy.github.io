---
title: hexo genarate的一种问题
date: 2016-06-12 01:00:00
categories: 杂七杂八
---

generate的时候出现了一些奇怪的问题

<!--more-->
```
ERROR Process failed: source/_css/layout/_sidebar.scss
TypeError: Cannot read property 'compile' of undefined
    at View._precompile (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/lib/theme/view.js:104:22)
    at View (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/lib/theme/view.js:13:8)
    at new Theme._View.View (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/lib/theme/index.js:37:10)
    at Theme.setView (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/lib/theme/index.js:71:20)
    at /Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/lib/theme/processors/view.js:14:14
    at tryCatcher (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/util.js:16:23)
    at Promise._settlePromiseFromHandler (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:503:31)
    at Promise._settlePromise (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:560:18)
    at Promise._settlePromise0 (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:605:10)
    at Promise._settlePromises (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:684:18)
    at Promise._fulfill (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:629:18)
    at Promise._resolveCallback (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:424:57)
    at Promise._settlePromiseFromHandler (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:515:17)
    at Promise._settlePromise (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:560:18)
    at Promise._settlePromise0 (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:605:10)
    at Promise._settlePromises (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:684:18)
    at Promise._fulfill (/Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/promise.js:629:18)
    at /Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/bluebird/js/release/nodeback.js:42:21
    at /Users/louis/Versionning/Personnal/hexo-blog/node_modules/hexo/node_modules/hexo-fs/node_modules/graceful-fs/graceful-fs.js:78:16
    at FSReqWrap.readFileAfterClose [as oncomplete] (fs.js:380:3)
```


最终得知这个问题的一个原因时因为Hexo 3.2.0版本的问题，在出现这种问题的时候，一个比较简单可行的方法是将版本回退到3.1.1
```bash
npm uninstall hexo
npm install hexo@3.1.1 --save
```
然后在对应的项目下重新执行npm install，而后再执行之后的操作即可
