# verilator

## 1.STFW veripool.org

"Fastest Verilog/SystemVerilog simulator"

快速，广泛使用，社区驱动与开放许可。

## 2.安装

从git安装`5.008`版本。按照官网的流程一步步来即可。不要在ysyx的工作目录操作。

使用`verilator --version`查看版本。

顺便，`apt-get install gtkwave`安装`GTKWave`查看波形，verilator手册中有讲相关操作。

将仓库克隆到本地后，使用`git tag`查看版本，使用`git checkout v5.008`切换到指定的版本。

然后使用`autoconf`创建`./configure`脚本

首先，进入verilator仓库所在文件夹，输入`export VERILATOR_ROOT='pwd'`，然后运行`./configure`。

然后按照官网安装，可能需要很多时间......不要着急

## 3.照着手册入门

verilator能够把verilog和system verilog硬件描述语言C++或system C，编译后可以被执行。verilator不是传统的仿真器，而是编译器。

生成的结果被输出为`.cpp`和`.h`文件。这被称为`Verilating`。过程称为`to Verilate`，输出称为`Verilated`模型。

对仿真而言，C++`wrapper`文件是必须要，这个文件中定义了`main()`，能够将`Verilated model`实例化为`C++/System C`对象。

C++`wrapper`，verilator创建的文件，verilator提供的`runtime library`，（以及如果适用的话，SystemC库），被使用C++编译器来编译，生成一个可执行的仿真。

生成的可执行文件将在`simulation runtime`运行实际的仿真。

如果适当地启用，还能生成波形。下面从例子来入门。

### 3.1 创建二进制可执行文件

新建了一个文件`our.v`：

```systemverilog
module our;
    initial begin $display("Hello World"); $finish; end
endmodule
```

然后执行命令`verilator --binary -j 0 -Wall our.v`。

其中`--binary`让verilator去做生成可执行仿真所需要的一切，`-j 0`让verilator使用尽可能多的线程，`-Wall`让verilator有更强的lint警告，最后，`our.v`是我们的SystemVerilog设计文件。

在这之后运行`obj_dir/Vour`，也就是执行这个文件，能够看到打印出`Hello World`。我们成功地执行了。

手册告诉我们，最好是用一个`Makefile`帮我们做这些步骤，这样源代码改变之后，会自动地做这些恰当的步骤。

### 3.2 C++可执行文件

我们将把这个例子编译为C++。

但这里的C++我看不懂。还是先把C部分搞完，再来数字电路部分吧！
