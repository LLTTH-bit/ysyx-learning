# PA0

## Preparation

PA0是GNU/Linux环境开发配置的指南，我们将会被引导安装GNU/Linux开发环境，所有的PA和实验都会在这个环境中完成。这个文档要仔细读，如果遇到了一些文档中没提到的问题，回来重新看。机器永远是对的。

我们将安装Ubuntu 22.04。真机地稳定性会比虚拟机相对高一些，性能也更高一些。所以还是用真机！

搜索“Ubuntu 22.04安装教程”，跟着指导继续做。（不要使用UbuntuSoftware Updater。选择语言时不要选择中文）

选择虚拟机安装，真机安装还是太麻烦了，如果有两台电脑或许可以选择真机......

## First Exploration with GNU/Linux

ctrl+Alt+T 一键打开终端，显示

```shell
username@hostname:~$
```

说明现在的工作目录是`~`
GUI似乎不见了，取而代之的是终端，全部是命令行接交互（CLI Command Line Interface）.使用CLI可以做你想做的任何事情。
GUI在需要高度定义的显示的时候，比如看电影时，或许更胜一筹；其他时候或许CLI更好。

输入下面这行命令可以看看Ubuntu占用了多少空间：

```shell
df -h
```

关闭系统的时候，使用下面的命令

```shell
poweroff
```

## Installing Tools

在GUN/Linus中，可以通过一行命令下载和安装软件，这在Windows上是很困难的。这是通过package manager实现的。不同的GUN/Linux发行版有不用的package manager，在Ubuntu中，package manager叫做`apt`.

将会从网络镜像下载和安装PA需要的工具。使用网络镜像之前，需要检查一些系统能否上网。

### Checking network state

通过下面这个命令ping一下：

```shell
ping www.baidu.com -c 4
```

更改多次设置终于让虚拟机上去网了，太不容易~

### Setting APT source file

运行下面的命令来更新APT源文件：

```shell
sed -i "s/archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g" /etc/apt/sources.list"
```

但权限不够。因为APT源文件为root所有。一个解决方案是切换到root账户。为了避免切换，一个可行的解决方案是使用`sudo`。如果一个操作需要更高级用户许可，那么在操作前面加上`sudo`。在使用`sudo`之前，需要将账户加入到`sudo`group中。但首先还是要进入到root账户中。
切换到root账户，后成功加入了`sudo`群。root账户的密码是936042.
记得：使用需要权限的命令时加上`sudo`！

### Updating APT package information

输入下面命令：

```shell
sudo apt-get update
```

其中遇到了讲义中预料到的错误，额外运行一条命令后再次运行上面的命令即可。

### Installing tools for PAs

安装了一系列工具，这些工具的用途后面会解释。

### Installing Chinese input method

上网搜寻如何在Ubuntu中安装中文输入法。已经实现。

## Configuring vim

```shell
apt-get install vim
```

vim被称为编辑器之神。后面的实验和PA的代码都会在vim中编写，其他文件也是。

### Learning vim

我们将会被要求用`vim`来修改一个文件。`vim`中的操作和其他我们曾使用过的编辑器大有不同。学习`vim`需要指导，有两种方式：

- 在终端中执行`vimtutor`命令。将会推出一个`vim`的指导。这个是更推荐的
- 上网搜索“vim教程”，会搜到很多。记得使用`vim test`命令进行练习

## More Exploration

### Learning to use basic tools

看`small tutorial for GNU/Linux written by jyy`，完成三个必做题：

1. Write a "Hello World" program under GNU/Linux √
2. Write a Makefile to compile the "Hello World" program √
3. Learn to use GDB `a small tutorial for GDB`

### small tutorial for GNU/Linux

这里的很多命令在`Missing Semester`里面看过了，记录一些有用的东西。

统计代码行数：

```bash
find . | grep '\.c$\/\.h$' | xargs wc -l
```

这里需要注意，`xargs`命令的应用，搞清楚标准输入是将输入直接当作文件处理。

统计磁盘：

```bash
du -sc /usr/share/* | sort -r | less
```

`less`命令的使用，对于较长的输出可以上下翻页。

键入命令时，如果有个目录特别长，可以用`TAB`自动补全，会有奇效。

C程序编译：对于一个写好的c程序`hello.c`，运行`gcc`命令`gcc hello.c -o hello`就能将其编译成可执行文件`hello`。当然，可执行文件的名字是可以自己改的。不使用`-o`选项会默认生成`a.out`的文件。

Makefile管理工程：

```makefile
hello:hello.c
    gcc hello.c -o hello    # 注意开头的tab, 而不是空格

.PHONY: clean

clean:
    rm hello    # 注意开头的tab, 而不是空格
```

运行命令`make`执行编译，运行命令`make clean`来清除已经编译好的`hello`

GDB：
GDB是一个历史悠久且全面的Linux debug工具。
约定：`#`代表需要使用`sudo`命令来执行，`$`代表普通非特权用户执行。
费劲九牛二虎之力，终于能够直接在当前路径生成`core`文件了
编译并执行后，得到了`core`文件。这时候运行下面的命令：`gdb ./test.out ./core.xxxxxx`即可打开`core`文件。在这个界面可以进行很多交互的操作，查看不同帧时候的信息。

### Installing tmux

`tmux`:
terminal multiplexer，能够开好几个终端。这里记录一下常用命令。

- `tmux`：新建一个窗口
- `tmux new -s name`：新建一个窗口，指定它的名字
- `tmux ls`：列出所有窗口
- `tmux attach`：连接到最近访问的一个窗口
- `tmux kell-session -t name`：删除对应名称的窗口
- `tmux kill-server`：中止所有`tmux`对话

下面包含`<CTRL>+b`的命令，都是在`tmux`窗口内部进行的。省略它们前面的`<CTRL>+b`。通过修改`~`目录下的`.tmux.conf`文件，将`b`键改成了`a`：

- `%`：水平分割窗口
- `"`：垂直分割窗口
- `n/p`：切换到后一个/前一个窗口
- `d`：分离窗口，回到正常的终端而不是`tmux`
- `w`：快速切换窗口
