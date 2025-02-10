# verilator

## STFW veripool.org

"Fastest Verilog/SystemVerilog simulator"

快速，广泛使用，社区驱动与开放许可。

## 安装

从git安装`5.008`版本。按照官网的流程一步步来即可。不要在ysyx的工作目录操作。

使用`verilator --version`查看版本。

顺便，`apt-get install gtkwave`安装`GTKWave`查看波形，verilator手册中有讲相关操作。

将仓库克隆到本地后，使用`git tag`查看版本，使用`git checkout v5.008`切换到指定的版本。

然后使用`autoconf`创建`./configure`脚本

首先，进入verilator仓库所在文件夹，输入`export VERILATOR_ROOT='pwd'`，然后运行`./configure`。

然后按照官网安装，可能需要很多时间......不要着急
