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
