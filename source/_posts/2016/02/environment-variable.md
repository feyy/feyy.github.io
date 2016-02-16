---
title: environment variable
date: 2016-02-16 23:56:33
tags:
- bash
---

windows的环境变量的设置都应该很熟悉了，这儿不记录了，不知道的自己gg去。

换成mac后一直不太清除环境变量设置的方法和原理，此处稍作抄袭记录了。

## SHELL

```bash
echo $SHELL
```
使用上面的命令可以知道使用的shell类型。  
如果输出的是：csh或者是tcsh，那么你用的就是C Shell。  
如果输出的是：bash，sh，zsh，那么你的用的可能就是Bourne Shell的一个变种。  
Mac OS X 10.2之前默认的是C Shell。  
Mac OS X 10.3之后默认的是Bourne Shell。  

如果是Bourne Shell，我是这个...
那么你可以把你要添加的环境变量添加到你主目录下面的.profile或者.  bash_profile，如果存在没有关系添加进去即可，如果没有生成一个。  

## 如何配置
Mac配置环境变量的地方
1./etc/profile   （建议不修改这个文件 ）
全局（公有）配置，不管是哪个用户，登录时都会读取该文件。
 
2./etc/bashrc    （一般在这个文件中添加系统级环境变量）
全局（公有）配置，bash shell执行时，不管是何种方式，都会读取此文件。
 
3.~/.bash_profile  （一般在这个文件中添加用户级环境变量）
每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!

4.~/.bashrc  
文件会在bash shell调用另一个bash shell时读取，也就是在shell中再键入bash命令启动一个新shell时就会去读该文件。这样可有效分离登录和子shell所需的环境
。但一般 来说都会在.bash_profile里调用.bashrc脚本以便统一配置用户环境。然而我用的mac并没有...

linux下环境变量用冒号隔开，所以配置的方式为：

```bash
export PATH=<your path>:$PATH
```

## 赠送
* ~/.bash_logout在退出shell时被读取。所以我们可把一些清理工作的命令放到这文件中。
* 修改host: `sudo vi /etc/hosts`
* 查看系统变量用 `$echo`

## YY

```bash
vim /etc/profile
# 文档末尾追加
export PATH="<your path>:$PATH"
# 想立即生效
source /etc/profile
```
其他常用变量通常修改~/.bash_profile

## 参考文献
* [Mac 可设置环境变量的位置、查看和添加PATH环境变量](http://elf8848.iteye.com/blog/1582137)
* [.bash_profile和.bashrc的区别(如何设置生效)](http://blog.163.com/wang_hai_fei/blog/static/309020312008728333912)