# 一： 版本控制工具应该具备的功能

- 协同修改

  - 多人并行不悖的修改服务器端的同一个文件。

- 数据备份

  - 不仅保存目录和文件的当前状态，还能够保存每一个提交过的历史状态。

- 版本管理

  - 在保存每一个版本的文件信息的时候要做到不保存重复数据，以节约存储空间，提高运行效率。这方面
    SVN 采用的是增量式管理的方式，而 Git 采取了文件系统快照的方式。

- 权限控制

  - 对团队中参与开发的人员进行权限控制。

  - 对团队外开发者贡献的代码进行审核——Git 独有。

- 历史记录

  - 查看修改人、修改时间、修改内容、日志信息。

  - 将本地文件恢复到某一个历史状态。

- 分支管理

  - 允许开发团队在工作过程中多条生产线同时推进任务，进一步提高效率。

# 二： 版本控制简介

## 2.1 版本控制

工程设计领域中使用版本控制管理工程蓝图的设计过程。在 IT 开发过程中也可以使用版本控制思想管理代码的版本迭代。

## 2.2 版本控制工具

版本控制工具集中式版本控制工具：CVS、**SVN**、VSS……

![](media/7c16de1db872a3f12d2fa3af8fa3100d.jpg)

分布式版本控制工具：Git、Mercurial、Bazaar、Darcs……

![](media/2ec04fe48f2c5902c34baeda67618cb6.jpg)

# 三：Git 简介

## 3.1Git 简史

![](media/54fac158442b44e3b78de0081e2d2f11.png)

## 3.2Git 官网和 Logo

官网地址：https://git-scm.com/

## 3.3Git 的优势

- 大部分操作在本地完成，不需要联网

- 完整性保证

- 尽可能添加数据而不是删除或修改数据

- 分支操作非常快捷流畅

- 与 Linux 命令全面兼容

## 3.4Git 安装

![](media/97b8c3fab4fa3b10cf0f274ba1754c0e.jpg)

![](media/b960743284db633d85cc4b90e4cdfa6f.jpg)

![](media/95f5f78213990abff0b09449e31ed729.jpg)

![](media/ad49e4b0b539b2dae36071ef9d3ef2e7.jpg)

![](media/3951ba70f0d67a53ac7b6a724b837440.jpg)

## 3.5Git 结构

![](media/ea7d69e15bc86ef52fe36bbad6b1d8df.png)

## 3.6Git 和代码托管中心

代码托管中心的任务：维护远程库

- 局域网环境下

  - GitLab 服务器

- 外网环境下

  - GitHub

  - 码云

## 3.7 本地库和远程库

### 3.7.1 团队内部协作

![](media/c49fa766caedfb7c5e86ca1fd7919c89.png)

### 3.7.2 跨团队协作

![](media/1f64470a06c4a3b17a1b9f6654bd944b.png)

# 四: Git 命令行操作

## 4.1 本地库初始化

- 命令：git add

- 效果

  ![](media/4e3165338d91c4b2f0be26e2a415f80a.jpg)

- 注意：.git
  目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改。

## 4.2 设置签名

- 形式

  用户名：tom

  Email 地址：goodMorning\@atguigu.com

- 作用：区分不同开发人员的身份

- 辨析：这里设置的签名和登录远程库(代码托管中心)的账号、密码没有任何关系。

- 命令

  - 项目级别/仓库级别：仅在当前本地库范围内有效

    - git **config** user.name tom_pro

    - git **config** user.email goodMorning_pro\@atguigu.com

    - 信息保存位置：./.git/config 文件

      ![](media/4e6c5bc9d9bd0821ca726985afe192a1.jpg)

  - 系统用户级别：登录当前操作系统的用户范围

    - git config **--global** user.name tom_glb

    - git config **--global** goodMorning_pro\@atguigu.com

    - 信息保存位置：\~/.gitconfig 文件

      ![](media/6c229418c4f491c7ba13823543b9ebc8.jpg)

  - 级别优先级

    - 就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别的签名

    - 如果只有系统用户级别的签名，就以系统用户级别的签名为准

    - 二者都没有不允许

## 4.3 基本操作

### 查看

查看工作区、暂存区状态

git status

添加：将工作区的“新建/修改”添加到暂存区

git add [file name]

提交：将暂存区的内容提交到本地库

git commit -m "commit message" [file name]

查看历史记录

git log

多屏显示控制方式：

空格向下翻页

b 向上翻页

q 退出

git log --pretty=oneline

![](media/7eceb0d37a6e0c96dd14b5821489e039.jpg)

git log --oneline

![](media/1dba0b9ba6946f842e8e906b293e4971.jpg)

git reflog

![](media/fda56192a405866a3b5cbe641f417330.jpg)

HEAD\@{移动到当前版本需要多少步}

### 4.3.5 前进后退

- 本质

  ![](media/f6a565ab239d36a6f17e3967b444f009.png)

- 基于索 引值操作[推荐]

  - git reset --hard [局部索引值]

  - git reset --hard a6ace91

- 使用\^符号：只能后退

  - git reset --hard HEAD\^

  - 注：一个\^表示后退一步，n 个表示后退 n 步

- 使用\~符号：只能后退

  - git reset --hard HEAD\~n

  - 注：表示后退 n 步

### 4.3.6 reset 命令的三个参数对比

- \--soft 参数

  - 仅仅在本地库移动 HEAD 指针

    ![](media/132df47f6c74b0afd34b82fa6b7280d4.png)

- \--mixed 参数

  - 在本地库移动 HEAD 指针  重置暂存区

    ![](media/fb9f6fc18e3d20334018170f6953606b.png)

- \--hard 参数

  - 在本地库移动 HEAD 指针

  - 重置暂存区

  - 重置工作区

### **4.3.7** 删除文件并找回

- 前提：删除前，文件存在时的状态提交到了本地库。

- 操作：git reset --hard [指针位置]

  - 删除操作已经提交到本地库：指针位置指向历史记录

    - 删除操作尚未提交到本地库：指针位置使用 HEAD

### **4.3.8** 比较文件差异

- git diff [文件名]

  将工作区中的文件和暂存区进行比较

- git diff [本地库中历史版本] [文件名]

  将工作区中的文件和本地库历史记录比较

- 不带文件名比较多个文件

## 4.4 分支管理

### **4.4.1** 什么是分支？

> 在版本控制过程中，使用多条线同时推进多个任务。

![](media/19521be94e20684d6d08a5c94a3f6e97.png)

### **4.4.2** 分支的好处？

- 同时并行推进多个功能开发，提高开发效率

- 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

### 4.4.3 分支操作

- 创建分支

  git branch [分支名]

- 查看分支 git branch -v

- 切换分支

  git checkout [分支名]

- 合并分支

  - 第一步：切换到接受修改的分支（被合并，增加新内容）上

  git checkout [被合并分支名]

  - 第二步：执行 merge 命令

    git merge [有新内容分支名]

- 解决冲突

  - 冲突的表现

    ![](media/f9957d731cf3e2357c55db8e8ba8d76c.png)

    - 冲突的解决

      - 第一步：编辑文件，删除特殊符号

      - 第二步：把文件修改到满意的程度，保存退出

      - 第三步：git add [文件名]

      - 第四步：git commit -m "日志信息"

         注意：此时 commit 一定不能带具体文件名

### rebase

采用 merger 和 rebase 后，git log 的区别：

**merge 命令不会保留 merge 的分支的 commit**：

处理冲突的方式：

- （一股脑）使用`merge`命令合并分支，解决完冲突，执行`git add .`和`git commit -m'fix conflict'`。这个时候会产生一个 commit。
- （交互式）使用`rebase`命令合并分支，解决完冲突，执行`git add .`和`git rebase --continue`，不会产生额外的 commit。这样的好处是，‘干净’，分支上不会有无意义的解决分支的 commit；坏处，如果合并的分支中存在多个`commit`，需要重复处理多次冲突。

## 其他操作

1：没有 commit 就回退的，导致暂存区的数据丢失。恢复方法：

```shell
# 列出最近的60个提交的文件
find .git/objects -type f | xargs ls -lt | sed 60q
输出：
-r--r--r-- 1 xiao4er xiao4er      848 Jun 14 11:40 .git/objects/e2/a9e0a491c21b06f029fdbe96a8f40934de6ff9
……

# 获取响应编号的文件内容输出
git cat-file -p  e2a9e0a491c21b06f029fdbe96a8f40934de6ff9 > ResultFile.md
其中ID为文件编号后边的两个去掉中间的 /
```

2：删除没有使用的大文件

```shell
# 查出已删除的大文件前3名的commit SHA值
git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -n | tail -3

# SHA值查出文件路径
git rev-list --objects --all |grep a283535d282b3437773cd1a7a16e1ae8cca3c498
git rev-list --objects --all |grep 0c53c451a47688c9bfa65e1cd856f2083828f7fb

# 查出文件提交commit记录
git log --pretty=oneline --branches -- 0.0数据结构与算法/数据结构与算法.docx

# 遍历所有提交： commit多了会比较慢
git filter-branch --force --prune-empty --index-filter 'git rm -rf --cached --ignore-unmatch 0.0数据结构与算法/数据结构与算法.docx' --tag-name-filter cat -- --all
git filter-branch --force --prune-empty --index-filter 'git rm -rf --cached --ignore-unmatch 1.1版本控制/1.Git/Git/media/137ab5ef17d3a9b579bc777612860f31.png' --tag-name-filter cat -- --all

# 如果没出问题可以不执行
# 储藏
git stash

# 回收内存
rm -rf .git/refs/original/
   #  修剪早于指定时间的条目。
git reflog expire --expire=now --all
  #   清理不必要的文件并优化本地存储库
git gc --prune=now

# 强制提交的远程
git push --force --all
```

其中：

```shell
 # 强制
 -f 或则 --force

```

# 五：Git 基本原理

## 5.1 哈希

![](media/350068a52e3f2ae0e4cacdbf664f8df8.png)

哈希是一个系列的加密算法，各个不同的哈希算法虽然加密强度不同，但是有以下几个共同点：

① 不管输入数据的数据量有多大，输入同一个哈希算法，得到的加密结果长度固定。

② 哈希算法确定，输入数据确定，输出数据能够保证不变

③ 哈希算法确定，输入数据有变化，输出数据一定有变化，而且通常变化很大

④ 哈希算法不可逆

Git 底层采用的是 SHA-1 算法。

哈希算法可以被用来验证文件。原理如下图所示：

![](media/a63c98b124dad701fcf4253984344565.png)

Git 就是靠这种机制来从根本上保证数据完整性的。

## 5.2Git 保存版本的机制

### **5.2.1** 集中式版本控制工具的文件管理机制

以文件变更列表的方式存储信息。这类系统将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。

![](media/442cc182b3b63c5f305413169c0c25e2.jpg)

### 5.2.2 Git 的文件管理机制

Git 把数据看作是小型文件系统的一组快照。每次提交更新时 Git 都会对当前的全部文件制作一个快照并保存这个快照的索引。为了高效，如果文件没有修改，
Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。所以 Git
的工作方式可以称之为快照流。

![](media/35dd2fd0764def1a724c0d8c76f9f351.jpg)

### 5.2.3 Git 文件管理机制细节

- Git 的“提交对象”

![](media/6160c8fbdde4f32351af59dd382a0376.jpg)

- 提交对象及其父对象形成的链条

![](media/4a1a4e7bd509bddb5d54600a7af9e5d5.jpg)

## 5.3Git 分支管理机制

### 5.3.1 分支的创建

![](media/e127818be6f250d01cdd7619998a3717.jpg)

### 5.3.2 分支的切换

![](media/c584e9d67d7498a6f43a84a374b02ace.jpg)

![](media/1c4e36134e9855f83ea05a48d9524cf9.jpg)

# 六：GitHub

## 6.2 创建远程库

![](media/7fbfe2c491edefa355ea41fd10894202.jpg)

![](media/677c23a660878d7b0294eb65641455a4.jpg)

## 6.3 创建远程库地址别名

git remote -v 查看当前所有远程地址别名 git remote add [别名] [远程地址]

![](media/aac3745d4b6ea3e954b277f2db3e4492.jpg)

## 6.4 推送

git push [别名] [分支名]

![](media/6dc755589193831e029f8a45370a05cf.jpg)

## 6.5 克隆

- 命令

  - git origin [远程地址]

    ![](media/f21a3e2824070b64039d6f6140b91a54.jpg)

- 效果

  - 完整的把远程库下载到本地

  - 创建 origin 远程地址别名

  - 初始化本地库

## 6.6 团队成员邀请

![](media/983d33889aff9f288c2c38addb1cee77.png)

![](media/0d81e599d41e4ad3fc070b084da76eae.jpg)

![](media/b5669df6f421d777f0f07e89e0cf6e21.png)

“岳不群”其他方式把邀请链接发送给“令狐冲”，“令狐冲”登录自己的 GitHub 账号，访问邀请链接。

![](media/53a313785821e046178ef7cd142cffe9.jpg)

## 6.7 拉取

- pull=fetch+merge

- git fetch [远程库地址别名] [远程分支名]

- git merge [远程库地址别名/远程分支名]

- git pull [远程库地址别名] [远程分支名]

## 6.8 解决冲突

- 要点

  - 如果不是基于 GitHub 远程库的最新版所做的修改，不能推送，必须先拉取。

  - 拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作解决即可。

- 类比

  - 债权人：老王

  - 债务人：小刘

  - 老王说：10 天后归还。小刘接受，双方达成一致。

  - 老王媳妇说：5 天后归还。小刘不能接受。老王媳妇需要找老王确认后再执行。

## 6.9 跨团队协作

- Fork

![](media/adc1eed51bd36a841424b703236204ed.png)

![](media/ed775d2abeb78a05ac6f9e0c27869661.png)

- 本地修改，然后推送到远程

- Pull Request

![](media/cbdfbf8cc195156a25453d22de11ea03.jpg)

![](media/ba88eeeef4068e22cb0fc24fa3ea1fc7.png)

- 对话

![](media/4de48172551642ca229a4925ae80bcab.jpg)

![](media/45d43d496828c4e5cdb861e04a396674.jpg)

- 审核代码

![](media/1c9f5b1fe3fa11d2c2b797f591aea0a8.jpg)

- 合并代码

  ![](media/ea04b07f03bfbfb3c38cd9ee127e8a57.png)

- 将远程库修改拉取到本地

## 6.10SSH 登录

- 进入当前用户的家目录

  \$ cd \~

- 删除.ssh 目录

  \$ rm -rvf .ssh

- 运行命令生成.ssh 密钥目录

  \$ ssh-keygen -t rsa -C atguigu2018ybuq\@aliyun.com

  [注意：这里**-C** 这个参数是大写的 **C**]

- 进入.ssh 目录查看文件列表

  \$ cd .ssh

  \$ ls -lF

- 查看 id_rsa.pub 文件内容

  \$ cat id_rsa.pub

- 复制 id_rsa.pub 文件内容，登录 GitHub，点击用户头像 →Settings→SSH and GPG
  keys

- New SSH Key

- 输入复制的密钥信息

- 回到 Git bash 创建远程地址别名

  git remote add origin_ssh git\@github.com:atguigu2018ybuq/huashan.git

- 推送文件进行测试

# 七：Eclipse 操作

## 7.1 工程初始化为本地库

- 工程 → 右键 →Team→Share Project→Git

  ![](media/6c524a9f1582bd92288c6d98aa715b1d.png)

- Create Repository

  ![](media/522a67e5610fd8fc253e344d2dead439.png)

- Finish

## 7.2Eclipse 中忽略文件

- 概念：Eclipse 特定文件

  这些都是 Eclipse
  为了管理我们创建的工程而维护的文件，和开发的代码没有直接关系。最好不要在 Git
  中进行追踪，也就是把它们忽略。

  .classpath 文件

  .project 文件

  .settings 目录下所有文件

- 为什么要忽略 Eclipse 特定文件呢？

  同一个团队中很难保证大家使用相同的 IDE 工具，而 IDE 工具不同时，相关工程特定文件就有可能不同。如果这些文件加入版本控制，那么开发时很可能需要为了这些文件解决冲突。

![](media/eb863231dc92ee99fabc78bbdc0df61d.png)

- GitHub 官网样例文件

  https://github.com/github/gitignore

  https://github.com/github/gitignore/blob/master/Java.gitignore

- 编辑本地忽略配置文件，文件名任意

- 在\~/.gitconfig 文件中引入上述文件

  [core] excludesfile = C:/Users/Lenovo/Java.gitignore

[注意：这里路径中一定要使用“**/**”，不能使用“**\\**”]

## 7.3 推送到远程库

![](media/bd944b0cbf4222f2744ba2cd9a2a8e6e.jpg)

![](media/c5f1ca856a1506960a46e396c96b6d7b.jpg)

![](media/65ccf8889b6b2b84884e5f718d26c1fd.jpg)

![](media/547539688719987d61fb596b49784a79.jpg)

## 7.6 解决冲突

冲突文件 → 右键 →Team→Merge Tool 修改完成后正常执行 add/commit 操作即可

# 八：Git 工作流

## 8.1 概念

在项目开发过程中使用 Git 的方式，流程并不是唯一的适合自己的就是最好的。

## 8.2 分类

### 集中式工作流

像 SVN 一样，集中式工作流以中央仓库作为项目所有修改的单点实体。所有修改都提交到 Master 这个分支上。这种方式与 SVN 的主要区别就是开发人员有本地库。Git 很多特性并没有用到。这样就是当人多了就会很容易冲突；

![](media/7e85a8463a89ea8692e42a3d2032272b.png)

### 功能分支工作流

每个功能开一个分支，每个人开发自己的功能在自己的分支上，提交到自己的分支上，别人也可以切换到这个分支查看代码，在合并到master之前，开发者发起一个Pull Request让团队其他人知道功能已经完成；

### GitFlow 工作流

Gitflow 工作流通过为功能开发、发布准备和维护设立了独立的分支，让发布迭代过程更流畅。严格的分支模型也为大型项目提供了一些非常必要的结构。主干上只维护版本的迭代；

![](media/33fc9540104ae3882bd6bf55ce6723a5.png)

###  Forking工作流

Forking 工作流是在 GitFlow 基础上，充分利用了 Git 的 Fork 和 pull request 的功能以达到代码审核的目的。更适合安全可靠地管理大团队的开发者，而且能接受不信任贡献者的提交。

![](media/75ed8e4ce5f0ae886ca3e64118cd2e3f.png)

## 8.3GitFlow 工作流详解

### **8.3.1** 分支种类

- 主干分支 master

  主要负责管理正在运行的生产环境代码。永远保持与正在运行的生产环境完全一致。

- 开发分支 develop

  主要负责管理正在开发过程中的代码。一般情况下应该是最新的代码。

- bug 修理分支 hotfix

  主要负责管理生产环境下出现的紧急修复的代码。从主干分支分出，修理完毕并测试上线后，并回主干分支。并回后，视情况可以删除该分支。

- 准生产分支（预发布分支） release

  较大的版本上线前，会从开发分支中分出准生产分支，进行最后阶段的集成测试。该版本上线后，会合并到主干分支。生产环境运行一段阶段较稳定后可以视情况删除。

- 功能分支 feature

  为了不影响较短周期的开发工作，一般把中长期开发模块，会从开发分支中独立出来。开发完成后会合并到开发分支。

### 8.3.2 GitFlow 工作流举例

### 8.3.3 分支实战

![image-20210209134600237](media/image-20210209134600237.png)

### 8.3.4 具体操作



### 学习网站

https://github.com/pcottle/learnGitBranching



## 九： Gitlab 服务器搭建过程

### **9.1**官网地址

首页：https://about.gitlab.com/

安装说明：https://about.gitlab.com/installation/

### **9.2**安装命令摘录

# 十：小乌龟

<https://tortoisegit.org/download/>

**1.先下载 git，按照相应的系统，**[https://git-scm.com/downloads](https://links.jianshu.com/go?to=https%3A%2F%2Fgit-scm.com%2Fdownloads)**，然后，一直 next 即可完成安装**

**2.安装 git 小乌龟，**[https://tortoisegit.org/download/](https://links.jianshu.com/go?to=https%3A%2F%2Ftortoisegit.org%2Fdownload%2F)**，同样的，一直 next 即可完成安装，但是，需要注意的是必须先安装 git，在安装 git 小乌龟**

**3.安装语言包，同样是**[https://tortoisegit.org/download/](https://links.jianshu.com/go?to=https%3A%2F%2Ftortoisegit.org%2Fdownload%2F)**，然后一直 next 即可，要先装完小乌龟在安装语言包。**

4.右键 →tortogit→setting，把 language 项改为中文，点击确定就可以了

![](media/27b45712cf05fdfc1d04c6ae32031e06.png)

## 1：常见操作

**1:从远程更新**

**拉取**

![](media/aecab8fb92ac20491f9b0a47bc407b42.png)

2：提交到本地库

提交——添加注解

![](media/62ea2218faa90a35b9dc8981349e30fb.png)

3：推送到远程库

# 十二：问题

清理历史大文件

方法一：使用 `git filter-branch`

```shell
git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -n | tail -3

git rev-list --objects --all |grep eeb2730bd72a6f48e5880a9705f0eb32083b5bc2
git rev-list --objects --all |grep 0c53c451a47688c9bfa65e1cd856f2083828f7fb

git log --pretty=oneline --branches -- 1.1版本控制/1.Git与GitHub/Git与GitHub/media/c247fd2525be0cfdacbe2a77fcc05b22.png
git reflog --pretty=oneline --branches -- 1.1版本控制/1.Git与GitHub/Git与GitHub/media/c247fd2525be0cfdacbe2a77fcc05b22.png

git filter-branch --force --prune-empty --index-filter 'git rm -rf --cached --ignore-unmatch 1.1版本控制/1.Git/Git/media/c247fd2525be0cfdacbe2a77fcc05b22.png' --tag-name-filter cat -- --all
git filter-branch --force --prune-empty --index-filter 'git rm -rf --cached --ignore-unmatch 1.1版本控制/1.Git/Git/media/137ab5ef17d3a9b579bc777612860f31.png' --tag-name-filter cat -- --all

git stash

rm -rf .git/refs/original/

git reflog expire --expire=now --all

git gc --prune=now

git push --force --all
```

方法二：使用工具 `BFG.jar`

```shell

先克隆
git clone --mirror https://github.com/username/-Note.git

先将bfg.jar拷贝到项目-Note.git下
使用cmd
java -jar bfg.jar --delete-files c247fd2525be0cfdacbe2a77fcc05b22.png

再用Git Bash
git reflog expire --expire=now --all && git gc --prune=now --aggressive
git push

https://www.jianshu.com/p/a9caf4b3100e
```
