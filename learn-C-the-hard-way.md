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

一开始没有报错，后来发现是编译选项中的`-O2`导致的。`-O`系列参数是编译器优化，0代表无优化，1代表基本优化，2代表中等优化，3代表最高优化。而其他参数中，`-Wall`是开启所有警告信息，`-g`是生成调试信息，`-o`是指定输出文件名称。

## 练习5：一个C程序的结构

运行代码，不再赘述：

```C
#include <stdio.h>

/* This is a comment. */
int main(int argc, char *argv[])
{
    int distance = 100;

    // this is also a comment
    printf("You are %d miles away.\n", distance);

    return 0;
}
```

## 练习6：变量类型

再次运行代码：

```C
include <stdio.h>

int main(int argc, char *argv[])
{
    int distance = 100;
    float power = 2.345f;
    double super_power = 56789.4532;
    char initial = 'A';
    char first_name[] = "Zed";
    char last_name[] = "Shaw";

    printf("You are %d miles away.\n", distance);
    printf("You have %f levels of power.\n", power);
    printf("You have %f awesome super powers.\n", super_power);
    printf("I have an initial %c.\n", initial);
    printf("I have a first name %s.\n", first_name);
    printf("I have a last name %s.\n", last_name);
    printf("My whole name is %s %c. %s.\n",
            first_name, initial, last_name);

    return 0;
}
```

其中包含了整数、浮点、字符和字符串的打印方法，注意区分字符`char`和字符串`char[]`。

修改代码让程序出现错误，观察报错。

## 练习7：更多变量和一些算术

继续运行代码：

```C
int main(int argc, char *argv[])
{
    int bugs = 100;
    double bug_rate = 1.2;

    printf("You have %d bugs at the imaginary rate of %f.\n",
            bugs, bug_rate);

    long universe_of_defects = 1L * 1024L * 1024L * 1024L;
    printf("The entire universe has %ld bugs.\n",
            universe_of_defects);

    double expected_bugs = bugs * bug_rate;
    printf("You are expected to have %f bugs.\n",
            expected_bugs);

    double part_of_universe = expected_bugs / universe_of_defects;
    printf("That is only a %e portion of the universe.\n",
            part_of_universe);

    // this makes no sense, just a demo of something weird
    char nul_byte = '\0';
    int care_percentage = bugs * nul_byte;
    printf("Which means you should care %d%%.\n",
            care_percentage);

    return 0;
}
```

注意，最后一句用`%%`来打印一个百分号。最后一句printf还包含了`char`与`int`相乘，经过试验应该是拿字符的ASCII与整数相乘。

## 练习8：大小和数组

先运行一段代码：

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
    int areas[] = {10, 12, 13, 14, 20};
    char name[] = "Zed";
    char full_name[] = {
        'Z', 'e', 'd',
         ' ', 'A', '.', ' ',
         'S', 'h', 'a', 'w', '\0'
    };

    // WARNING: On some systems you may have to change the
    // %ld in this code to a %u since it will use unsigned ints
    printf("The size of an int: %ld\n", sizeof(int));
    printf("The size of areas (int[]): %ld\n",
            sizeof(areas));
    printf("The number of ints in areas: %ld\n",
            sizeof(areas) / sizeof(int));
    printf("The first area is %d, the 2nd %d.\n",
            areas[0], areas[1]);

    printf("The size of a char: %ld\n", sizeof(char));
    printf("The size of name (char[]): %ld\n",
            sizeof(name));
    printf("The number of chars: %ld\n",
            sizeof(name) / sizeof(char));

    printf("The size of full_name (char[]): %ld\n",
            sizeof(full_name));
    printf("The number of chars: %ld\n",
            sizeof(full_name) / sizeof(char));

    printf("name=\"%s\" and full_name=\"%s\"\n",
            name, full_name);

    return 0;
}
```

其中包含了不同类型变量占用的字节数（`sizeof`实现）。

观察输出，看到`name`的尺寸是4，因为C语言字符串默认以`\0`结尾。在`fullname`中我们手动加入了`\0`，因此没有出现令人困惑的现象。通过访问`name[3]`即可发现这个问题。

如果我们在定义`name`时直接写出`char[3]`而不是`char[]`，那么`name`大小就会变为3，也就没有默认的空字符结尾。如果访问超出数组范围的值，比如修改后的`name[3]`，由于下面正好初始化了`full_name`，这时候访问到的就是`full_name[0]`，序号增加则以此类推。但是如果访问`areas[10]`，可能是因为附近没有定义`int`，得到的是一个随机的值。

## 练习9：数组和字符串

运行代码：

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
    int numbers[4] = {0};
    char name[4] = {'a'};

    // first, print them out raw
    printf("numbers: %d %d %d %d\n",
            numbers[0], numbers[1],
            numbers[2], numbers[3]);

    printf("name each: %c %c %c %c\n",
            name[0], name[1],
            name[2], name[3]);

    printf("name: %s\n", name);

    // setup the numbers
    numbers[0] = 1;
    numbers[1] = 2;
    numbers[2] = 3;
    numbers[3] = 4;

    // setup the name
    name[0] = 'Z';
    name[1] = 'e';
    name[2] = 'd';
    name[3] = '\0';

    // then print them out initialized
    printf("numbers: %d %d %d %d\n",
            numbers[0], numbers[1],
            numbers[2], numbers[3]);

    printf("name each: %c %c %c %c\n",
            name[0], name[1],
            name[2], name[3]);

    // print the name like a string
    printf("name: %s\n", name);

    // another way to use name
    char *another = "Zed";

    printf("another: %s\n", another);

    printf("another each: %c %c %c %c\n",
            another[0], another[1],
            another[2], another[3]);

    return 0;
}
```

输出：

```bash
numbers: 0 0 0 0
name each: a   
name: a
numbers: 1 2 3 4
name each: Z e d
name: Zed
another: Zed
another each: Z e d
```

对int数组，赋初值的，自动给0；而对于字符串，未赋初值的则是`\0`。

另外，使用字符串时，使用上面代码中的`char *another = "some characters"`更省事且更符合语言习惯。

## 练习10：字符串数组和循环

运行代码：

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
    int i = 0;

    // go through each string in argv
    // why am I skipping argv[0]?
    for(i = 1; i < argc; i++) {
        printf("arg %d: %s\n", i, argv[i]);
    }

    // let's make our own array of strings
    char *states[] = {
        "California", "Oregon",
        "Washington", "Texas"
    };
    int num_states = 4;

    for(i = 0; i < num_states; i++) {
        printf("state %d: %s\n", i, states[i]);
    }

    return 0;
}
```

这段代码有两个需要注意的地方。

### 1.main函数的参数

`argc`是从命令行中获取的参数的个数；`argv`是参数的字符串数组。

需要注意的是，当我们使用命令`./ex10 hello world`，`./ex10`本身也会当作参数存储到`argv[0]`中。不要忘记这一点。

### 2.字符串数组

这里的`argv[]`是一个字符串数组，`states`同理。这是一个二维数组。举个例子，`states[0][0]`对应"California"中的"C"，这样就好理解了。

很好理解的一点是，`states[]`和`argv[]`可以互相赋值。

如果把`NULL`赋值给`states[1]`，打印出来是`(null)`。

### 另：for循环

不做赘述，已经太熟悉了。

## 练习11：While循环和布尔表达式

运行代码：

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
    // go through each string in argv

    int i = 0;
    while(i < argc) {
        printf("arg %d: %s\n", i, argv[i]);
        i++;
    }

    // let's make our own array of strings
    char *states[] = {
        "California", "Oregon",
        "Washington", "Texas"
    };

    int num_states = 4;
    i = 0;  // watch for this
    while(i < num_states) {
        printf("state %d: %s\n", i, states[i]);
        i++;
    }

    return 0;
}
```

C语言中整数替代了布尔类型，其中0代表`false`，其他值代表`true`。至于`while`循环，很熟悉了，不必多言。

令人疑惑的是，将`argv[]`中的元素赋值给`states[]`，没有报错，但是输出后发现没有成功修改，这是为何？

## 练习12：If,Else If,Else

`if else`语句很熟悉了，运行下面这个程序来温习一下吧：

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
    int i = 0;

    if(argc == 1) {
        printf("You only have one argument. You suck.\n");
    } else if(argc > 1 && argc < 4) {
        printf("Here's your arguments:\n");

        for(i = 0; i < argc; i++) {
            printf("%s ", argv[i]);
        }
        printf("\n");
    } else {
        printf("You have too many arguments. You suck.\n");
    }

    return 0;
}
```

## 练习13：Switch语句

通过运行下面代码复习：

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
    if(argc != 2) {
        printf("ERROR: You need one argument.\n");
        // this is how you abort a program
        return 1;
    }

    int i = 0;
    for(i = 0; argv[1][i] != '\0'; i++) {
        char letter = argv[1][i];

        switch(letter) {
            case 'a':
            case 'A':
                printf("%d: 'A'\n", i);
                break;

            case 'e':
            case 'E':
                printf("%d: 'E'\n", i);
                break;

            case 'i':
            case 'I':
                printf("%d: 'I'\n", i);
                break;

            case 'o':
            case 'O':
                printf("%d: 'O'\n", i);
                break;

            case 'u':
            case 'U':
                printf("%d: 'U'\n", i);
                break;

            case 'y':
            case 'Y':
                if(i > 2) {
                    // it's only sometimes Y
                    printf("%d: 'Y'\n", i);
                }
                break;

            default:
                printf("%d: %c is not a vowel\n", i, letter);
        }
    }

    return 0;
}
```

C中的switch语句是一个“跳转表”，只能放置结果为整数的表达式。

上面这个代码能够识别输入的参数中的元音字符并大写输出。这段代码只能接受一个参数（除去执行文件本身对应的那个参数）。

程序根据`letter`的值进行跳转。需要注意的是，由于小写字母`a e i o u y`对应的代码不包含`break`，程序执行完后会继续**贯穿**执行，也就是执行它们下面`A E I O U Y`对应的代码。

注意在Y的判断出，如果把`break`放在`if`里面，效果是一样的。

注意，一定要先写`case`和`break`在写代码，不要忘记`default`！！！！！

## 练习14：编写并使用函数

>你该写自己的函数了。

运行下面这段代码：

```C
#include <stdio.h>
#include <ctype.h>

// forward declarations
int can_print_it(char ch);
void print_letters(char arg[]);

void print_arguments(int argc, char *argv[])
{
    int i = 0;

    for(i = 0; i < argc; i++) {
        print_letters(argv[i]);
    }
}

void print_letters(char arg[])
{
    int i = 0;

    for(i = 0; arg[i] != '\0'; i++) {
        char ch = arg[i];

        if(can_print_it(ch)) {
            printf("'%c' == %d ", ch, ch);
        }
    }

    printf("\n");
}

int can_print_it(char ch)
{
    return isalpha(ch) || isblank(ch);
}


int main(int argc, char *argv[])
{
    print_arguments(argc, argv);
    return 0;
}
```

其中，第二个头文件有我们需要的新的函数。

这段代码将参数中的每一个**字母**输出了出来并输出对应的ASCII值。

注意，如果要使用当前文件中没碰到过的函数，记得**前向声明**。代码中有体现这点。尝试去掉前向声明后，程序无法编译。

**关于K&R语法**：
教程中告诉我们不要使用这种语法。K&R语法的特点是：声明函数时仅指定函数名和返回类型，参数列表为空，或者只有参数名而没有参数类型。

```C
int add(); // 声明时不指定参数类型
```

定义函数时，在小括号中写上参数名，然后在大括号前面把参数定义齐全。

```C
int add(a, b) 
int a; 
int b; 
{ 
    return a + b; 
}
```

现在常用的语法叫做ANSI C语法。

## 练习15：指针，可怕的指针

先运行代码：

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
    // create two arrays we care about
    int ages[] = {23, 43, 12, 89, 2};
    char *names[] = {
        "Alan", "Frank",
        "Mary", "John", "Lisa"
    };

    // safely get the size of ages
    int count = sizeof(ages) / sizeof(int);
    int i = 0;

    // first way using indexing
    for(i = 0; i < count; i++) {
        printf("%s has %d years alive.\n",
                names[i], ages[i]);
    }

    printf("---\n");

    // setup the pointers to the start of the arrays
    int *cur_age = ages;
    char **cur_name = names;

    // second way using pointers
    for(i = 0; i < count; i++) {
        printf("%s is %d years old.\n",
                *(cur_name+i), *(cur_age+i));
    }

    printf("---\n");

    // third way, pointers are just arrays
    for(i = 0; i < count; i++) {
        printf("%s is %d years old again.\n",
                cur_name[i], cur_age[i]);
    }

    printf("---\n");

    // fourth way with pointers in a stupid complex way
    for(cur_name = names, cur_age = ages;
            (cur_age - ages) < count;
            cur_name++, cur_age++)
    {
        printf("%s lived %d years so far.\n",
                *cur_name, *cur_age);
    }

    return 0;
}
```

这段代码定义了整数数组和字符串数组，其中字符串数组是用`char *names[]={xxx}`方法定义的。

首先使用索引打印，然后创建指针，通过指针取值和打印。

创建指针：

```C
    int *cur_age = ages;
    char **cur_name = names;
```

第一句创建了指向`ages`的指针；第二句中，由于`names`已经是指向字符的指针，故需要二重指针。这里隐式地表示了取地址`&`，因为是数组。但如果是单独的变量，需要加上取地址符`&`。

我们有了指针`cur_ages`和`cur_names`，怎么使用它们？`*(cur_names+i)`和`name[i]`效果一样，与`cur_name[i]`也有一样的效果。

而对于第四种方法，通过指针自增的方式取元素。需要注意的是，`cur_age-ages`比较的是当前地址与矩阵首地址之间的距离（因为`cur_age`不出意外一定是`ages[]`对应元素的地址）。

>**文档中说**：
对于你看到的其它所有情况，实际上应当使用数组。在早期，由于编译器不擅长优化数组，人们使用指针来加速它们的程序。然而，现在访问数组和指针的语法都会翻译成相同的机器码，并且表现一致。由此，你应该每次尽可能使用数组，并且按需将指针用作提升性能的手段。

## 练习16：结构体和指向它们的指针

本节课学习`struct`，并学习用`malloc`从原始内存中构造它们。

```C
#include <stdio.h>
#include <assert.h>
#include <stdlib.h>
#include <string.h>

struct Person {
    char *name;
    int age;
    int height;
    int weight;
};

struct Person *Person_create(char *name, int age, int height, int weight)
{
    struct Person *who = malloc(sizeof(struct Person));
    assert(who != NULL);

    who->name = strdup(name);
    who->age = age;
    who->height = height;
    who->weight = weight;

    return who;
}

void Person_destroy(struct Person *who)
{
    assert(who != NULL);

    free(who->name);
    free(who);
}

void Person_print(struct Person *who)
{
    printf("Name: %s\n", who->name);
    printf("\tAge: %d\n", who->age);// \t means TAB
    printf("\tHeight: %d\n", who->height);
    printf("\tWeight: %d\n", who->weight);
}

int main(int argc, char *argv[])
{
    // make two people structures
    struct Person *joe = Person_create(
            "Joe Alex", 32, 64, 140);

    struct Person *frank = Person_create(
            "Frank Blank", 20, 72, 180);

    // print them out and where they are in memory
    printf("Joe is at memory location %p:\n", joe);
    Person_print(joe);

    printf("Frank is at memory location %p:\n", frank);
    Person_print(frank);

    // make everyone age 20 years and print them again
    joe->age += 20;
    joe->height -= 2;
    joe->weight += 40;
    Person_print(joe);

    frank->age += 20;
    frank->weight += 20;
    Person_print(frank);

    // destroy them both so we clean up
    Person_destroy(joe);
    Person_destroy(frank);

    return 0;
}
```

先关注头文件：

- `<assert.h>`提供了`assert()`，用于判断真假，如果括号中表达式为假，会打印错误信息并停止执行。
- `<stdlib.h>`提供了内存管理、程序控制、转换、随机数相关的函数。本代码中的`malloc()`和`free()`都是它提供的。
- `<string.h>`是字符串相关，不再赘述。

在代码的一开始先定义结构体，然后通过`struct Person *Person_create()`函数来创建一个具体的结构体。注意这里`*`的意思是，返回值是`struct Person`的指针，不要搞混。

其中`malloc`用于申请一块原始的内存。在我们定义了`who`指针后，可以使用`who->name`语句访问结构体中的`name`元素。其中`who`是指针，而`who->name`是`(*who).name`的简写。我们使用指针而不是名称来访问结构体的时候，都需要这么干。

而在`Person_destroy()`函数中，使用`free()`释放内存。注意在创建和清空之前都要进行`assert(who != NULL)`的判断。

如果想按照创建一个普通的整型变量那样**在栈上创建一个结构体变量**，可以使用下面的命令：

```C
struct Person p = {"Alice", 30, 165, 55};//main函数结束时会自动销毁，内存自动释放
```

按照附加题中所要求，在栈上创建结构体变量，并通过`x.y`方式需改其中内容，同时写了一个函数，直接传递结构体而非指针。

## 练习17：堆和栈的内存分配
