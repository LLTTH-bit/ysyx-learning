# Learn C the hard way

笨方法学C--练习0到练习18。练习32，练习33，练习42，练习44

## 练习0：准备

这里没有讲什么，或许这里最重要的东西是，警告我们不要使用**IDE**。

## 练习1：启用编译器

写了一个文件：

```c
int main(int agrc,char *argv[]) 
{
    puts("Hello World.");

    return 0;

}
```

写入`ex1.c`并输入`make ex1`，就会得到`ex1`可执行文件。执行后输出`Hello World`。

如果使用`make`命令时改为`CFLAGS="-Wall" make ex1`，之后会正常生成`ex1`，但会报`warning`。如果在第一行加上`#include <stdio.h>`就不会报警告。

总之，这节还是初步尝试，并未讲什么知识点。

注意，这节里面给出的命令`man 3 puts`中的3指的是从`man`的第三章节中查找这个命令的手册。

## 练习2： 用Make代替Python

上一节我们用了`make ex1`命令，但没有说这是什么原理。`make`可以将C代码编译成可以运行的二进制文件。

在上节课的文件夹添加`Makefile`文件，内容如下：

```Makefile
CFLAGS=-Wall -g

clean:
    rm -f ex1
```

注意，最后一行前面必须是`TAB`，而不能是空格。

上节课运行的命令：

```bash
$ make ex1
# or this one too
$ CFLAGS="-Wall" make ex1
```

第一行创建`ex1`文件，没有指定则根据`ex1`开头的其他文件去创建。第二行命令是让`cc`编译器报告所有的警告。这里先不细究。

运行`make clean`能移除可执行文件，运行`make ex1`再次生成可执行文件。

如果我们去掉了`ex1.c`中的`#include <stdio.h>`，在运行`make ex1`时就会弹出Warning。

这里只简单讲了Makefile用法，具体的后面自己去学吧。

## 练习3：格式化输出

运行这段代码，体会格式化输出：

```C
#include <stdio.h>

int main()
{
    int age = 10;
    int height = 72;

    printf("I am %d years old.\n", age);
    printf("I am %d inches tall.\n", height);

    return 0;
}
```

删去`printf`后面的变量，看看报错；定义变量但不赋值，看看报错。

然后，找出更多让程序崩溃的方法吧。去看一看`printf`的手册。

## 练习4：Valgrind介绍

首先使用`sudo apt install valgrind`命令安装Valgrind。

如果编译了一个有问题的文件，比如不给变量赋初值就使用、`printf`中不给出匹配的变量。编译过程中会有提示。

如果在得到可执行文件后，执行`valgrind ./<executable-filename>`，就会给出详细的报错。
