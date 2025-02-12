# git学习

来自bilibili视频：【GeekHour】 一小时Git教程 [视频链接](https://www.bilibili.com/video/BV1HM411377j/?spm_id_from=333.337.search-card.all.click&vd_source=e99e5d6d2e9c53c73a6b5ebf92bfc6e4)

## 课程简介

git是一种**分布式版本控制系统**。相对于集中式版本控制系统，每个人电脑上都有完整的版本库。当需要将修改内容分享给他人时，只需要互相同步仓库即可。

## 安装和初始化配置

安装好后，运行`git --version`查看当前版本。

使用命令：`git config --global user.name XXXXX`配置用户名。这里的`global`指的是全局配置，对所有仓库生效；缺省设置是`local`，只对本地仓库有效；`system`是系统配置，对所有用户生效。然后配置`user.email`，也就是用户邮箱。

运行命令：`git config --global credential.helper store`保存上面的配置，然后`git config --global --list`来查看配置的信息。

至此，配置完成！

## 新建仓库

版本库也叫仓库，简称`Repo`。其中的文件能够被git跟踪到。

创建仓库可以有两种方法：使用`git init`创建本地仓库；从远程服务器上`git clone`一个已经存在的仓库。

这里我们创建一个新目录`learn-git`，在这个路径下输入`git init`就可以创建一个仓库了。

输入`ls -a`即可看到，目录中包含`.git`目录，说明git仓库已经建立成功了。删除掉这个目录，git仓库也就删除掉了。这时如果运行`git init my-repo`，就会先在当前目录新建`my-repo`目录，然后将`my-repo`目录新建一个仓库。

通过配置`~/.bashrc`文件，进入git仓库后就会显示当前的分支。

除了`git init`创建仓库以外，还能从远程服务器克隆仓库。在`learn-git`文件夹下输入`git-clone https://github.com/geekhall-laoyang/remote-repo.git`命令即可克隆。在当前目录下又生成了一个`remote-repo`文件夹。这个`remote-repo`就是我们从服务器上克隆出的仓库。

## 工作区域与文件状态

git的工作区域分为：

1. 工作区。就是电脑上的目录，文件夹。
2. 暂存区。是临时存储区域。用于保存即将提交到git仓库的修改内容。
3. 本地仓库：使用`git init`创建的仓库。是存储代码和版本信息的主要位置。

在工作区修改完内容->`git add`->暂存区->`git commit`->本地仓库（弹幕看到一个比喻很形象，后厨-服务员-客人）

更改完后先加载到暂存区，最后一起提交到本地仓库。

文件类型分为四种，未跟踪、未修改、已修改、已暂存。

## 添加和提交文件

我们可以在之前的`remote-repo`中创建新的文件`file1.txt`，使用`git status`查看状态，会在`Untracked files`一栏看到*红色*的文件名。这时候运行命令`git add file1.txt`就可以将文件添加到暂存区。这时再次输入`git status`就会在待提交一栏看到刚刚add的文件。

这时候运行`git commit`就能提交已经加到暂存区的文件。注意，没有在暂存区的文件不会被提交。且如果直接运行`git commit`，会进入一个文件，在第一行写入一些文字，就是本次提交时候的**提交信息**。编辑后保存退出。如果使用`git commit -m commit-information`命令，就能直接提交，无需再进入文件。

如果将刚刚提交到暂存区的文件修改一下，运行`git status`，会发现已修改和待提交处都有这个文件的名字。

使用`git log`查看提交记录。像vim一样上下翻页。如果嫌太长可以使用`git log --oneline`输出简洁的提交记录。

## `git reset`回退版本

有三种模式，命令是`git reset --<xx>`，参数可以是`soft` `hard` `mixed`。

- **soft：** 回退并保留工作区和暂存区的所有内容
- **hard：** 回退并丢弃工作区和暂存区的所有内容
- **mixed：** 回退并保留工作区的所有内容、丢弃暂存区的所有内容

默认参数是`mixed`。

注意，这里说的“保留”与“丢弃”对应“区域”的文件，说的是回退后当时的文件所处的区域。

举个例子。新建三个文件`file{1..3}.txt`，然后每`add`一个文件就`commit`一次。这时候我们提交了三次，有三个版本。从第三个版本回退到第二个版本时，`file3.txt`处于暂存区。这时候如果使用`soft`，`file3.txt`就不会被删除；使用`hard`和`mixed`，则会被删除，因为这个文件在**第二个版本**处于暂存区。关注的是一个文件**回退后**所处于的区域。

使用`git ls-files`能够查看**已经被跟踪的文件**。

谨慎使用`hard`参数哦。如果不小心误操作，使用`git reflog`查看操作的历史记录，找到误操作之前的历史记录，再回溯即可。所有的提交都是可以回溯的。

## `git diff`查看差异

可以查看文件在工作、暂存和本地仓库之间的差异，也可以查看文件在两个版本之间的差异，或者文件在两个分支之间的差异。

使用`git diff`命令，默认比较工作区与暂存区之间的差异。比如修改一个文件后，先不添加到暂存区，运行`git diff`就会展示差异。`git add`之后差异不存在，使用`git diff`就不会输出任何内容。

`git diff HEAD`命令比较工作区与版本库之间的差异。这时候修改了文件，添加到暂存区，但运行`git diff HEAD`后仍然会有输出。因为工作区和版本库内容并不一致。

使用命令`git diff --cached`比较暂存区与版本库之间的差异。

在命令后加上：**两个版本的提交ID**，就能比较两个版本之间的文件差异。但注意，这里的输出可以理解为：前一个版本到后一个版本 发生了什么变化。如果对换两个版本号的位置，输出结果会是截然相反的。同时，`HEAD`指向版本的最新提交节点，想要输入最新ID直接输入`HEAD`即可。

最常用的是：比较当前版本和上一个版本之间的差异。其对应的快捷命令：`git diff HEAD~ HEAD`，其中`HEAD~`表示上一个版本。将`~`替换为`^`也是可行的。如果加上数字，`git diff HEAD^2 HEAD`，表示与当前版本与之前的第二个版本之间的差异。灵活调节数字。

`git diff`命令的最后还可以加上文件名字，这样就只会查看指定文件的差异。

该命令还可以比较分支之间的差异，后面学习了分支再细说。

## `git rm`删除文件

如果想删除一个文件，应该怎么做？

直接删除的话，查看`git status`，会显示文件删除这个操作没有被添加到暂存区，或者说暂存区中的文件还没有删除。这时候`git add .`一下，暂存区中的文件也删除掉了，再提交即可。

想要更快地操作的话，使用`git rm file-name`即可。这时会直接在工作区和暂存区中删除对应的文件，然后手动提交即可在版本库也删除该文件。

另外的命令：`git rm --cached <file>` 在暂存区删除，在工作区保留；`git rm -r*`递归删除某个目录下的所有子目录和文件。

## `gitignore`忽略文件

git中有一个特殊文件`.gitignore`，让我们忽略掉不应该加入到版本库中的文件。

哪些文件不应该加入？：

- 系统自动生成的文件
- 编译生成的中间文件和可执行文件
- 系统运行自动生成的文件，日志、缓存、临时文件
- 设计身份、命令、口令、密钥等敏感信息文件

在仓库中新建文件`.gitignore`，如果想屏蔽某个特定文件，就直接输入文件名；如果想输入某一后缀的文件，就使用通配符。这里自己探索吧。

需要注意的是，如果在将某一文件屏蔽前，已经追踪了这个文件，那么现在再修改这个文件，git还是会继续追踪；只有使用`git rm <file>  (--cached)`命令后，停止追踪，才能屏蔽掉这个文件。（--cached后缀可有可无，取决于想不想在本地删除）

另外，git是不会追踪空的文件夹的。

如果想忽略文件夹，在`.gitignore`中请以斜线结尾。如忽略`temp`文件夹，在`.gitignore`中加入`temp/`即可。

关于`.gitignore`文件的匹配规则：

- 从上到下逐行匹配，每行代表一个忽略模式。
- 使用blob模式匹配，`*`通配任意个字符，`?`通配一个字符，`[]`表示匹配括号中的单个字符
- `**`表示匹配任意的中间目录
- `[0-9]` `[a-z]`表示匹配范围内的字符
- `!`表示取反。比如`!*.log`，表示只跟踪所有的`log`文件，即使前面忽略过`log`文件。

这里的规则太复杂了......github上有常用语言的忽略模板。

## 注册github账号

github是一个非常流行的代码托管平台。注册很简单，不再赘述。之前已经注册过了。

## SSH配置和远程仓库

进入主页，按照提示可以简单地创建一个新的仓库。进入仓库后会提示我们如何关联或者创建，这里后面再说。

在`git clone`时有两种方式：http和SSH。这里我们配置SSH后使用SSH方法。我们不妨先在github创建一个仓库，便于后面克隆到本地。

进入`~/.ssh`，不存在这个目录的话，运行下面的命令会自动生成。保险起见，还是手动创建一个吧。

运行命令`ssh-keygen -t rsa -b 4096`，即可生成名为`id_rsa`文件。如果是第一次生成密钥，直接回车；如果不是，直接回车后新的密钥会覆盖掉旧的。在这里不直接回车，输入一个名字（不必带后缀），就会生成新的密钥而不是`id_rsa`。然后输入两次密码。

这里会生成两个文件，`id_rsa`和`id_rsa.pub`，没有扩展名的是私钥，谁也不要给！！！！`.pub`后缀的是公钥，复制添加到gitub。复制时需要先将`vim`中的内容复制到系统剪切板，方法是进入可视模式后，选中所有文本，依次按下`"+y`；然后再按`ctrl+shift+v`即可复制出来。

如果上面使用的是默认的`id_rsa`密钥，那么就配置好了；如果更改了名字，那么运行下面这个命令（更改后名字为test）：

```bash
touch config # create "config"
# 在config中添加下面的内容
Host github.com
HostName github.com
PreferredAuthentication publickey
IdentityFile ~/.ssh/test
```

这样就配置好了！

这时使用SSH克隆命令，然后输入密钥对应的密码，即可克隆。这里记得关掉vpn，或者改成direct。

创建新文件后，添加提交。但远程仓库这时候看不到新的文件，因为本地仓库和远程仓库是两个文件。只有使用`git push`命令后，远程仓库才会被同步。`push`指把本地仓库推送给远程，`pull`是拉取到本地。

在我们从远程仓库克隆的时候，就相当于把本地仓库和远程仓库关联了起来。对于有权限的仓库，使用推送和拉取命令就能更新本地或远程仓库。

## 关联本地仓库和远程仓库

如何把本地仓库放到远程仓库里面？

使用`git remote add`命令将已经存在的仓库加进远程仓库。具体命令在github上有提示（先运行第一行）：

```bash
git remote add origin git@github.com:LLTTH-bit/My-second-repo.git #origin是远程仓库默认别名 后面是仓库链接
git branch -M main #指定分支的名称为main，默认是main就不必运行
git push -u origin main:main #把本地仓库和远程仓库关联起来 本地仓库的main推送给远程的main 我们的名字可能是master，注意修改
```

如果我们的分支是`master`，可以使用命令`git branch -M main`来强制重命名为`main`（如果有其他分支名为`main`则覆盖）

运行第一行代码之后，使用`git remote -v`查看对应的远程仓库的别名和地址，别名默认是`origin`。

从远程仓库更新本地仓库时，运行命令`git pull <远程仓库名> <远程分支名>:<本地分支名>`，注意本地分支可能是`master`。如果省略仓库和分支的名称，默认拉取`origin`的`main`分支。

关于合并时发生冲突、`git fetch`、分支 等内容，后面再说~

## gitee gitlab的使用

`gitee`是国内平台，速度相对快；`gitlab`的特点是私有化部署，可以搭建属于自己的私有服务器，适用于安全性较高的情况。

### gitee

注册很简单，设置SSH公钥过程与github一致。

但gitee在创建仓库时只能选择私有，之后在设置里面才能把它公开。

克隆等操作与github相似，不再赘述。

### gitlab

注册、添加SSH、创建仓库都与github相同，不再赘述。

重点是gitlab的**私有化部署**。部署过程在这里不再展开，自己下去研究。

在**私有化服务器**上创建仓库，过程同github类似。

一个本地仓库可以关联到多个远程仓库。使用`git remote add`命令。

## GUI工具

github desktop，只管理github项目则基本够用，windows mac支持

sourcetree，支持windows mac

gitKraken，商用的图形化工具，免费的功能有限，支持windows和mac

## 在vscode中使用git

vscode支持直接从命令行启动 输入`code .`即可在vscode打开当前目录。如何在vscode打开终端？

按`ctrl+shift+p`后输入`setting`可以进入`json`文件，存储了所有vscode的设置内容。

在源码管理器可以看到仓库的状态。

在更改列表中可以看到未暂存的内容。可以放弃、保留更改，以及添加到暂存区。也可以进行提交。也就是图形化操作。

`??`表示为跟踪的文件，`M`表示已修改的文件，`A`表示已添加暂存的文件，`D`表示已删除的文件，`R`表示重命名的文件，`U`表示已更新未合并的文件。

在提交时记得加上消息，否则可能会崩溃。

## 分支简介和基本操作

分支（Branch）是一个重要操作。

分支适合团队协作和开发管理。多个开发人员可以在自己的分支上工作，最后再合并。

视频中提到可以用可视化工具进行分支操作。但是我在虚拟机上还没有安装可视化工具，所以就只用命令行了。

使用`git branch`查看当前仓库的所有分支。使用`git branch <branch name>`来创建一个新的分支。新的分支会保存创建分支前当前分支的文件，新分支的起点相当于在那里。

使用`git checkout <branch name>`切换到不同的分支。但该命令也可以恢复文件，当分支名与文件名相同时可能会出现歧义。因此可以使用`git switch <branch name>`切换分支。

如果在`dev`分支修改、新建文件并提交，切换到`main`分支后是看不到`dev`分支下修改和新建的文件的，因为切换分支时文件会发生变动。

使用`git merge <branch name>`可以将输入的分支与**当前的分支**合并。

合并后，被合并的分支仍然的存在；要删除分支时，使用`git branch -d <branch name>`删除分支。如果想要删除的分支没有被合并，需要使用`-D`参数。

当然，分支合并时可能会有冲突，后面再讲。

## 解决合并冲突

上一节只学习了如何合并，貌似并没有对合并的原则进行讲解。如果不深究这一块内容，是会出现一些问题的。

如果两个分支的修改内容并不重合，那么git会自动完成合并。

如果两个分支修改了同一个文件的同一行代码呢？冲突时会自动报错，需要手动修改冲突的文件，选择留下的内容。然后提交就自动合并了。使用`git status`后会发现提示，可以使用命令来终止合并。

## 回退和rebase

`Rebase`，中文意思是变基。

可以在任意分支上执行变基操作，命令是`git rebase <branch name>`。会找到当前分支与目标分支中的公共祖先，将当前分支从共同祖先到最新提交处 全部移动到 目标分支的最新提交后面。

这里太复杂了，后面有需要再细究吧。

## 分支管理和工作流模型

工作流模型：好的规范，让工作更有条理。

gitflow模型：

- 分支分为五种类型
- `main`分支叫做主线/基线分支。主线分支的代码会部署到生产环境中，只能通过合并分支的方法来修改。
- `hotfix`分支，用于解决线上问题，修复完成后合并回`main`分支。问题修复分支会更新小版本号。
- `release`分支，版本发布分支，也叫预发布分支。从开发分支中分离出来，稳定后合并到主分支和开发分支中。
- `develop`分支，开发分支，从主线中分离出来，用于开发和测试。是项目的核心分支，长期存在。
- `feature`分支，功能分支，用于开发新功能，从开发分支中分离出来，会合并回开发分支。

按照生命周期分为：**主要分支**和**辅助分支**。主要分支包括`main`和`develop`分支。其他都是辅助分支，完成功能后可以删除。

前面的gitflow模型有点复杂，可以看一看github flow模型。

github flow模型：

只有一个长期存在的主分支，主分支可以直接部署在生产环境，禁止直接在主分支上提交。

成员可以在自己的分支开发，最后发起`Pull Request`发布合并到主分支。

关于**分支命名**：带有意义的描述性名称来命名分支

关于**分支管理**：定时合并已经成功验证的分支，及时删除已经合并的分支；保持合适的分支数量；为分支设置合适的管理权限。

## cherry pick

这个视频中没有讲，有时间自己补上。
