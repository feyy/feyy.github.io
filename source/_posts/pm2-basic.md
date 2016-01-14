---
title: pm2基础命令
date: 2016-01-14 01:39:54
tags:
- pm2
- node
---

他跟PM2.5扯不上任何关系，也不是Project Management，[pm2](https://github.com/Unitech/pm2)是一个带有负载均衡功能的Node应用的进程管理器。可轻松根据服务器CPU fork出多个进程，并保证永远存活，自动重启。目前已在生产环境被普遍使用。

## 安装

```bash
npm install -g pm2
```

## 常用命令

```bash
pm2 start app.js [-i 4]         # 托管运行app.js, 可通过-i指定进程数，也可传递'-i max'
pm2 start app.js --name app     # 命名启动进程
pm2 list                        # 显示所有进程状态
pm2 monit                       # 监视所有进程的资源使用状况
pm2 logs                        # 显示所有进程日志
pm2 stop [all | name | id]      # 停止全部或者指定name或id的进程
pm2 restart [all | name | id]   # 重启全部或者指定name或id的进程
pm2 reload [all | name |id]     # 0秒停机重载进程
pm2 delete [all | name |id]     # 删除全部或者指定name或id的进程
```

## 日志

pm2会接管项目日志默认存放于用户目录下

```
~/.pm2/logs/
```

日志文件明构成：

* 错误日志：{appName}-error-{id}.log
* 其他日志：{appName}-out-{id}.log

注意：使用 `pm2 logs` 可实时查看所有进程日志。