---
title: Commit message 和 Change log
date: 2016-01-23 03:23:08
tags:
---
此处不得不赞叹一下阮老师的文笔和高产，膜拜中...🚬  
浏览过不少开源项目了，也算是github的深水用户，但还真没有太注意人家的提交注释(commit message)有什么不一样，以及每次发版时长长的chang logs。感谢小伟哥的指导，技术视野上的差距还是蛮大的，刚把得💪

先来张[angular-ui-bootstrap](https://github.com/angular-ui/bootstrap)库的commit截图  
![ui-bootstrap commit message](http://i.niupic.com/images/2016/01/23/TF2n76.png)

不管多少人提交代码，总是那么清晰爽快。
<!-- more -->
## commit message 格式

```bash
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```
其中，Header 是必需的，Body 和 Footer 可以省略。 
Header部分：type（必需）、scope（可选）和subject（必需） 
不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。

### type

```bash
feat            # 新功能（feature）
fix             # 修补bug
docs            # 文档（documentation）
style           # 格式（不影响代码运行的变动）
refactor        # 重构（即不是新增功能，也不是修改bug的代码变动）
test            # 增加测试
chore           # 构建过程或辅助工具的变动
```
7种类型基本覆盖了所有需求，好开心。

如果type为feat和fix，则该 commit 将肯定出现在 Change log 之中。其他情况（docs、chore、style、refactor、test）由你决定，要不要放入 Change log，建议是不要。

### scope
scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

### subject
subject是 commit 目的的简短描述，不超过50个字符。

### body
Body 部分是对本次 commit 的详细描述，可以分成多行

* 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
* 第一个字母小写
* 结尾不加句号（.）

### {% raw %}footer{% endraw %}

* 不兼容变动

```bash
BREAKING CHANGE:
```
* 关闭 Issue

```bash
Closes #234
```

## Commitizen

[Commitizen](https://github.com/commitizen/cz-cli)是一个撰写合格 Commit message 的工具。

```bash
npm install -g commitizen
cd {target}
commitizen init cz-conventional-changelog --save --save-exact
```

## validate-commit-msg

[validate-commit-msg](https://github.com/kentcdodds/validate-commit-msg) 用于检查 Node 项目的 Commit message 是否符合格式。

## Change log
[conventional-changelog](https://github.com/ajoslin/conventional-changelog) 就是生成 Change log 的工具，运行下面的命令即可。

```bash
npm install -g conventional-changelog
cd my-project
conventional-changelog -p angular -i CHANGELOG.md -w
```

## 参考文献
* [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)