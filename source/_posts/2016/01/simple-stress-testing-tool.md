---
title: simple stress testing tool
date: 2016-01-21 01:48:10
tags:
- test
- tool
---

介绍两款简单的压力测试软件[webbench](http://home.tiscali.cz/~cz210552/webbench.html)和[siege](https://www.joedog.org/)，使用起来都非常方便。

## siege
这还是2BB的找了半天，然后在自己机器上编译安装的产物...结果后来发现mac自带webbenc，欲哭无泪啊。
### 安装

```bash
curl -O http://download.joedog.org/siege/siege-latest.tar.gz
tar -zxvf siege-latest.tar.gz
cd siege-*
./configure
make && make install
```
### 使用实例

```bash
siege -c 10 -t 10M https://www.baidu.com        # 并发数10持续时间10min的请求百度
siege -t 100s -f url.txt                        # 持续请求url.txt中的url列表100s,每次取一个url
```
### 参数详解

```bash
 -C | –config               # 在屏幕上打印显示出当前的配置,配置是包括在他的配置文件$HOME/.siegerc中
                            #    可以编辑里面的参数,这样每次siege 都会按照它运行
 -v                         # 运行时能看到详细的运行信息
 -c n | –concurrent=n       # 模拟有n个用户在同时访问,n不要设得太大,因为越大,siege 消耗本地机器的资源越多
 -i | –internet             # 随机访问urls.txt中的url列表项,以此模拟真实的访问情况(随机性),当urls.txt存在是有效
 -d n | –delay=n            # hit每个url之间的延迟,在0-n之间
 -r n | –reps=n             # 重复运行测试n次,不能与 -t同时存在
 -t n | –time=n             # 持续运行siege ‘n’秒(如10S),分钟(10M),小时(10H)
 -l                         # 运行结束,将统计数据保存到日志文件中siege.log
                            #    一般位于/usr/local/var/siege .log中,也可在.siegerc中自定义
 -R SIEGERC | –rc=SIEGERC   # 指定用特定的siege 配置文件来运行,默认的为$HOME/.siegerc
 -f FILE | –file=FILE       # 指定用特定的urls文件运行siege ,默认为urls.txt,位于siege 安装目录下的etc/urls.txt
 -u URL | –url=URL          # 测试指定的一个URL,对它进行”siege “,此选项会忽略有关urls文件的设定
 ```
<!-- more -->
### 结果说明

```bash
Transactions: 30000 hits                # 完成30000次处理
Availability: 100.00 %                  # 100.00 % 成功率
Elapsed time: 68.59 secs                # 总共使用时间
Data transferred: 817.76 MB             # 共数据传输 817.76 MB
Response time: 0.04 secs                # 响应时间，显示网络连接的速度
Transaction rate: 437.38 trans/sec      # 平均每秒完成 437.38 次处理
Throughput: 11.92 MB/sec                # 平均每秒传送数据
Concurrency: 17.53                      # 实际最高并发连接数
Successful transactions: 30000          # 成功处理次数
Failed transactions: 0                  # 失败处理次数
Longest transaction: 3.12               # 每次传输所花最长时间
Shortest transaction: 0.00              # 每次传输所花最短时间
```

## webbench
webbench是一枚强大得可以的压力测试工具，它最多可以模拟3万个并发连接去测试网站的负载能力。使用方法跟siege大致相同，只是在参数上有些许差别。

### 使用实例

```bash
webbench -c 10 -t 10 https://www.baidu.com        # 并发数10持续时间10s的请求百度
webbench -t 100 -f https://www.baidu.com          # 强制请求100s百度，不等待返回结果
```
### 参数详解

```bash
 -f | --force                   # 不等待服务器返回结果继续发起请求
 -r | --reload                  # 请求头部信息带no-cahe参数：Pragma: no-cache.
 -t | --time <sec>              # 请求持续时间，默认30s
 -p | --proxy <server:port>     # 使用代理服务器请求
 -c | --clients <n>             # 一次发起n次请求，即请求并发
 -9 | --http09                  # 使用HTTP/0.9协议请求
 -1 | --http10                  # 使用HTTP/1.0协议请求
 -2 | --http11                  # 使用HTTP/1.1协议请求
 --get                          # 使用get方法请求
 --head                         # 使用head方法请求
 --options                      # 使用options方法请求
 --trace                        # 使用trace方法请求
 -? | -h | --help               # 帮助信息
 -V | --version                 # 查看版本信息
```

## YY
要是有几台机器一致请求一下别人的服务器应该还是蛮好玩的😱