---
title: pm2 senior (一)
date: 2016-01-25 21:53:18
tags:
- pm2
---
搞了一周线上环境...慢慢要变成OP了...  
pm2还是很好很强大的，用到的就慢慢写下来吧，当笔记了，等有空了专门研究下...像个奢望...像我这么懒的人...

## 内存阀值
给pm2设置一个内存阀值，若占用内存达到阀值，pm2将自动重启该服务。

```bash
pm2 start app.js --max-memory-restart 20M  # 50K | 50M 1G
```
值得注意的是，这只是一种容错机制，而不能当做功能用。当你真的发现线上服务有重启现象时，就应该去挖挖自己程序的bug了。所以每次重启应该关注一下服务的重启次数额。  
要生效需delete掉先前的server再start，直接restart不会生效额。

## 传递Node启动参数

```bash
pm2 start app.js --node-args="--harmony"
```

## 日志分文件
官方推荐插件[pm2-logrotate](https://github.com/pm2-hive/pm2-logrotate)，虽然star少，但它是一个独立的服务，不影响pm2运行，可以放心使用。

```bash
pm2 install pm2-logrotate
pm2 set pm2-logrotate:interval 1            # 默认为1，可以不设置
pm2 set pm2-logrotate:interval_unit 'DD'    # 设置分文件的时间单位(默认天)，DD：天；MM：月；mm：分
pm2 set pm2-logrotate:max_size 100M           # 设置分文件的大小单位(默认10MB)，接受G,M,K
pm2 set pm2-logrotate:retain all            # 设置保存的日志数，超过将自动删除，接受 all | number
```