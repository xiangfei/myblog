---
title: nodejs hexo  deploy error
date: 2020-07-16 09:43:51
author: 相飞
comments:
- true
tags:
- nodejs
categories:
- nodejs

---



### node js deploy 报错


```bash
[root@autotest-ruby-agent myblog]# hexo d
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
TypeError [ERR_INVALID_ARG_TYPE]: The "mode" argument must be integer. Received an instance of Object
    at copyFile (fs.js:1895:10)
    at tryCatcher (/service/myblog/node_modules/bluebird/js/release/util.js:16:23)
    at ret (eval at makeNodePromisifiedEval (/usr/lib/node_modules/hexo-cli/node_modules/bluebird/js/release/promisify.js:184:12), <anonymous>:13:39)
    at /service/myblog/node_modules/hexo-deployer-git/node_modules/hexo-fs/lib/fs.js:144:39
    at tryCatcher (/service/myblog/node_modules/bluebird/js/release/util.js:16:23)
    at Promise._settlePromiseFromHandler (/service/myblog/node_modules/bluebird/js/release/promise.js:512:31)
    at Promise._settlePromise (/service/myblog/node_modules/bluebird/js/release/promise.js:569:18)
    at Promise._settlePromise0 (/service/myblog/node_modules/bluebird/js/release/promise.js:614:10)
    at Promise._settlePromises (/service/myblog/node_modules/bluebird/js/release/promise.js:694:18)
    at Promise._fulfill (/service/myblog/node_modules/bluebird/js/release/promise.js:638:18)
    at Promise._resolveCallback (/service/myblog/node_modules/bluebird/js/release/promise.js:432:57)
    at Promise._settlePromiseFromHandler (/service/myblog/node_modules/bluebird/js/release/promise.js:524:17)
    at Promise._settlePromise (/service/myblog/node_modules/bluebird/js/release/promise.js:569:18)
    at Promise._settlePromise0 (/service/myblog/node_modules/bluebird/js/release/promise.js:614:10)
    at Promise._settlePromises (/service/myblog/node_modules/bluebird/js/release/promise.js:694:18)
    at Promise._fulfill (/service/myblog/node_modules/bluebird/js/release/promise.js:638:18)
    at Promise._resolveCallback (/service/myblog/node_modules/bluebird/js/release/promise.js:432:57)
    at Promise._settlePromiseFromHandler (/service/myblog/node_modules/bluebird/js/release/promise.js:524:17)
    at Promise._settlePromise (/service/myblog/node_modules/bluebird/js/release/promise.js:569:18)
    at Promise._settlePromise0 (/service/myblog/node_modules/bluebird/js/release/promise.js:614:10)
    at Promise._settlePromises (/service/myblog/node_modules/bluebird/js/release/promise.js:694:18)
    at Promise._fulfill (/service/myblog/node_modules/bluebird/js/release/promise.js:638:18)



```


> 原因是node 升级到14.x版本导致的, 将node 版本回滚的10.x可以
