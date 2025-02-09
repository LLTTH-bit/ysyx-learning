
# Shell

## Shell脚本简介

shell脚本是一种为shell编写的脚本程序。本次学习的“shell编程”都指的是shell脚本编程，而不是开发shell自身。

打开文本编辑器后，新建一个文件`test.sh`，这里的后缀不影响脚本执行，sh代表shell。比如下面这个：

```bash
#! /bin/bash
echo "Hello World!"
```

其中`#!`是一个约定的标记，告诉系统这个脚本使用了哪一种shell。

有两种运行方式：

- 保存为`test.sh`，cd到相应的目录，执行：

```bash
chmod +x ./test.sh #使脚本具有执行权限
./test/sh #执行脚本
```

一定需要写成`./test.sh`，意思是直接在当前目录找。

- 作为解释器参数运行 直接运行解释器

```bash
/bin/sh test.sh
```

这种不需要在第一行指定解释器，写了也没用。

## Shell变量

使用例如下命令可以定义变量：

```bash
name="lltth"
```

注意变量名与等号中间不要有空格，变量中也不要有空格。避开`if,else,while`这些关键字。
