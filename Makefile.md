# Makefile

B站链接：[20分钟速通Makefile](https://www.bilibili.com/video/BV1tyWWeeEpp/?spm_id_from=333.337.search-card.all.click&vd_source=e99e5d6d2e9c53c73a6b5ebf92bfc6e4)

## 基本用法

在软件开发中，“构建”（Build）是指将文件转化成可执行文件的过程。

需要借助自动化的构建工具完成这个过程。

对于`main.c`文件，使用`gcc main.c -o hello`可以将这个文件编译为可执行文件`Hello`（前提是一共只需要这一个文件就能进行编译）。

如果这个文件需要`message.h` `message.c` `main.c`三个文件，我们用Make工具来实现构建。

Make通过读取`Makefile`中的规则来进行编译。这个工具主要用于C和C++项目的构建中。

`Makefile`中的格式：

```Makefile
targer: dependencies
    actions
```

整个过程有三件事：要生成什么文件，文件的依赖是什么，怎么生成这个文件。

例如，上面所说的需要三个文件的文件，就可以在`Makefile`中这样写：

```Makefile
hello: main.c message.c
    gcc main.c message.c -o hello
```

首先要检查依赖文件是否更新，如有则会执行下面的命令。这时候输入`make`就会执行`Makefile`中的命令。

如果依赖文件没有更新（对比目标文件和依赖文件的时间戳），会提醒不需更新，否则会检查更新然后重新编译。

但这时，如果`main.c` `message.c`有一个更新，就会重新编译。更标准的写法是如下（把编译和链接分开）：

```Makefile
hello: main.o message.o
    gcc main.o message.o -o hello

main.o: main.c
    gcc -c main.c

message.o: message.c
    gcc -c message.c
```

提高效率。
这样就会先编译两个`.c`文件。这样有需要修改源文件时候，只需要重新编译修改了的源文件，其他不需要重新编译。

关于**伪目标**：只是一个标签，用于执行一些操作。

举个例子，在`Makefile`文件末尾加上：

```Makefile
clean:
 rm -f *.o hello
```

然后如果执行`make clean`，就会执行这里的`rm`命令。

但如果本地也有一个叫`clean`的文件，`make clean`这个命令就会失效。如果在文件首加上`.PHONY: clean`，Make就不会把clean当作文件名来处理。

另一个常用的伪目标是`all`。如果只执行`make`命令，会只按照第一条规则运行，生成第一条规则对应的文件（如果其依赖文件中包有本条Makefile描述了其规则的文件，则这个依赖文件也会被生成）。按照这个规则，如果在伪目标`all`上面加入所有需要生成的文件，执行`make all`就能生成所有文件：

```Makefile
all: hello world
    echo succeed
```

这样，执行`make all`就会生成所有文件，然后打印提示语句。同时，如果只想生成其中一个文件，执行`make <filename>`即可。

当然，中间文件`.o`也可以这样编译，根据文件名选择。只要是`Makefile`中定义的目标文件，都能按照这一系列规则进行。

如果两个目标文件的依赖文件和生成规则是一样的，把它们写在一行即可：

```Makefile
hello world: main.o meaasge.o
    gcc main.o message.o -o $@
```

这里的`$@`代表目标文件。这个变量叫做**自动变量**。后面会讲解所有的自动变量。

同时，`Makefile`中可以定义变量。如`targets = hello world`，然后需要输入`hello world`这两个目标文件的地方，直接输入`$(targets)`即可。通常会定义`targets`  `sources` `objects`这些变量。举个完整的例子：

```Makefile
.PHONY: clean all
CFLAGS = -Wall -g -O2
targets = hello world
sources = main.c message.c
objects = main.o message.o

all: $(targets)
    @echo "all done"

$(targets): $(objects)
    gcc $(CFLAGS) $(objects) -o $@

main.o: main.c
    gcc $(CFLAGS) -c main.c

message.o: message.c
    gcc $(CFLAGS) -c message.c

clean:
    rm -f *.o $(targets)
```

下面给出自动变量：

- `$@`：目标文件
- `$<`：第一个以来问及那
- `$^`：所有的依赖文件

根据自动变量，或许也能节省一下敲代码的时间。这里不再展开说。

还可使用通配符来简化文件。比如想把所有`.c`文件转换成对应的`.o`文件，就可以再`Makefile`中写道：`%.o: %.c`，这样对应了所有的`.c`为依赖文件，转化成对应的`.o`文件。

在命令行中，可以为`make`命令加上参数，除了之前说的指定文件等。如果你的`Makefile`名字不叫`Makefile`而叫`114514`，可以使用`make -f 114514`，指定需要哪个`Makefile`。在命令后面加上`-n`参数可以打印出将会执行的命令，但不会真正执行，可以在调试`Makefile`的时候使用。`-C`参数用于指定`Makefile`执行的目录。

## CMake

很多时候我们不需要手动编写`Makefile`。`CMake`这个工具更加简单好用。

建立一个`CMakelist.txt`，在第一行加上最小版本要求，然后加上project命令，设置源文件列表和生成可执行文件的命令，这样就完成了一个最简单的`CMake`文件，示意如下：

```CMake
cmake_minimum_required(VERSION 3.10)

project(HelloWorld)

set(SOURCES main.c message.c)

add_executable(hello ${SOURCES})
```

运行命令`cmake`。这样，`CMake`会帮助我们自动生成`Makefile`文件，就不用自己去写咯。

`CMake`还有很多其他功能，这里不再展开。
