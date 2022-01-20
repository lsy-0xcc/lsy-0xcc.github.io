---
title: Git 单独忽略某一行
date: 2022-01-22 23:34:57
tags: 
  - Git
categories: 
  - [DevOps, Git]
---


Git 提交时又是会有文件中包含敏感信息，比如密码，或者用于测试的配置不想提交。而yaml文件不支持导入另一个yaml文件，若全部忽略跟着很麻烦。

这是一个不提交某行或者不提交代码块的方法。

<!--more-->

## 步骤

1. 添加在根目录下添加 `.gitattributes` 文件

   ```properties
   *.yml filter=HashIgnore
   ```

   `*.yml` 为正则表达式，表示所有扩展名为 `yml` 的文件。可以自己修改。

   这一行表示为所有 `yml` 文件添filter `HashIgnore`。

2. 配置 filter

   有以下两种方式

   放心大胆地改，写错了可以用 `git config --global --edit`  或者 `git config --global --unset` 恢复。

   - 使用命令行

     ```bash
     git config --global filter.hashignore.clean "sed '/# *NoCommit$/d' %f| sed '/# *BeginNoCommit/,/# *EndNoCommit/d'"
     git config --global filter.hashignore.smudge "cat"
     ```

   - 编辑文本

     ```bash
     git config --global --edit
     ```

     ```properties
     [filter "HashIgnore"]
     	clean = "sed '/# *NoCommit$/d' %f| sed '/# *BeginNoCommit/,/# *EndNoCommit/d'"
     	smudge = "cat"
     ```

## 原理

### .gitattributes

.gitattributes文件可以让Git对指定位置的文件或应用一定的规则。这里定义一个filter。

filter的clean属性表示在执行 `git add` 时执行的脚本，这里设置忽略某行或某个代码块。

filter的smudge属性表示在执行 `git add` 时执行的脚本，`cat` 表示什么也不做。

#### %f

**脚本中的 `%f` 表示脚本中需要处理的文件**

### sed 命令

sed 是 Linux 中的流编辑器，用于处理文件

参考文档[GNU sed - GNU Project - Free Software Foundation](https://www.gnu.org/software/sed/)

执行脚本的意义

```bash
sed '/# *NoCommit$/d' %f
```

表示删除 `%f` 中匹配 `# *NoCommit$` 正则表达式的行

``` bash
sed '/# *BeginNoCommit/,/# *EndNoCommit/d'
```

表示删除匹配 `# *BeginNoCommit` 正则表达式和匹配 `# *EndNoCommit$` 正则表达式之间的所有行

#### 特殊转义

由于sed中正则表达式前后要使用 `/` 符号，所以也需要转义为 ` \/` 。

如果以后需要在 Java 文件这种使用 `//` 作为注释的需要注意

### “|”  管道

```bash
command1 | command2
command1 | command2 [ | commandN... ]
```

当在两个命令之间设置管道时，管道符`|`左边命令的输出就变成了右边命令的输入。只要第一个命令向标准输出写入，而第二个命令是从标准输入读取，那么这两个命令就可以形成一个管道。大部分的 Linux 命令都可以用来形成管道。

## 参考

[gitx - Can git ignore a specific line? - Stack Overflow](https://stackoverflow.com/questions/6557467/can-git-ignore-a-specific-line/22171275#22171275)

https://git-scm.com/docs/gitattributes

[GNU sed - GNU Project - Free Software Foundation](https://www.gnu.org/software/sed/)

[Linux Shell管道详解](http://c.biancheng.net/view/3131.html)
