git是一个开源的分布式版本控制系统，原理和结构都较为复杂，本章节旨在说明简易的上手教程方便项目组后续的版本迭代更新。更为深入的介绍和解释可以详见**Pro Git**：  
```
https://gitee.com/progit/index.html
```
***   
***
***
# 1.安装git
## 1.1 windows
下载地址
```
https://www.git-scm.com/download/win（官方）
http://www.wmzhe.com/soft-38801.html（国内，下载快，但是版本好像不是最新）
```
下载好后,直接默认安装即可，GUI没尝试过，不用的话可以取消
![安装界面](https://images2017.cnblogs.com/blog/1182576/201801/1182576-20180125193721506-1476751842.png)
桌面使用方式如下
![命令行界面](https://images2017.cnblogs.com/blog/1182576/201801/1182576-20180125193737944-1885732206.png)
## 1.2 linux
直接**apt-get**即可
```
apt-get install git
```
(linux下载时,若出现error:403,可以试试vi /etc/resolv.conf,将nameserver地址改为: 114.114.114.114)**还没试过Linux，先把这个可能的报错贴上**
## 1.3 配置git
```
git config --global user.name "lifeyx"               //个人账号
git config --global user.email 123456@qq.com         //个人邮箱地址
```
 上面的--global选项,表示以后管理git库时,默认使用上面的用户信息,也可以通过git config -l 来查看配置信息
 ## 1.4 git bash 基本命令
1. cd : 切换到哪个目录下， 如 cd e:\fff  切换 E 盘下面的 fff 目录。当我们用cd 进入文件夹时,我们可以使用 通配符*, cd f*, 如果E盘下只有一个f开头的文件夹,它就会进入到这个文件夹.  
2. cd .. 回退到上一个目录， 注意，cd 和两个点点..之间有一个空格。
3. pwd : 显示当前目录路径。
4. ls(ll): 都是列出当前目录中的所有文件，只不过ll(两个ll)列出的内容更为详细。
5. touch : 新建一个文件 如 touch index.js 就会在当前目录下新建一个index.js文件。
6. rm:  删除一个文件, rm index.js 就会把index.js文件删除.
7. mkdir: 新建一个目录,就是新建一个文件夹. 如mkdir src 新建src 文件夹.
8. rm -r : 删除一个文件夹,  rm -r src 删除src目录， 好像不能用通配符。
9. mv 移动文件, mv index.html src   index.html 是我们要移动的文件, src 是目标文件夹,当然, 这样写,必须保证文件和目标文件夹在同一目录下.
10. reset 清屏，把git bash命令窗口中的所有内容清空。
 ***
 ***
 ***
# 2.仓库、分支和工作区域的概念
**作为版本管理工具，git的仓库分为本地和云端（如github），二者组织形式一致，不同开发者可以很方便的在本地和github之间更新和上传。与之有关的git命令会在解释的过程中插入介绍**
---
## 2.1 仓库
仓库就是项目，既可以在本地创建也可以从云端clone。本地和云端可以互相更新。管理方式如下：
### 2.1.1 创建操作
#### 本地
利用cd命令打开指定文件夹，如
```
cd D:\practice2\git-study
```
使用git命令
```
git init   
```
![文件夹](https://s3.ax1x.com/2020/12/16/rlDEf1.png)
即可使这个文件夹成为工作区，并生成一个".git"隐藏文件,这个文件夹的主目录成为**工作区**  
.git里会保存git所需要的数据和资源,也就是git**仓库**和**暂存区**都会保存在.git里  
这三者的区别会在如下进行介绍
#### github
在github创建仓库如图，  
![github](https://images2017.cnblogs.com/blog/1182576/201801/1182576-20180125193203225-291967992.png)
**个人认为直接从github上创建仓库，然后clone到本地进行编辑——上传——下载——更细 这个流程更流畅**  
github端的clone方法如下
```
git clone https://github.com/zhaoNF/git-study.git
```
即可得到仓库文件夹
![github——clone](https://s3.ax1x.com/2020/12/16/rl6lKP.png)
cd 进入此文件夹,即可对该仓库进行管理，**分支**默认为main
![github——clone](https://s3.ax1x.com/2020/12/16/rl6sVU.png)
之后对此仓库进行编辑，之后上传到云端即可，上传更新方式之后介绍
## 2.2 分支
### 分支的必要性
小伙伴们都知道，我们在完成一个项目时，不可能是“单线程”开发的，很多时候任务是并行的，举个栗子：项目2.0版本上线了，现在要着手开发3.0版本，同时2.0版本可能还有一些bug需要修复，这些bug修复之后我们可能还会发2.1，2.2，2.3这些版本，我们不可能等所有bug都修复完了再去开发3.0版本，修复2.0的bug和开发3.0的新功能是两个并行的任务，这个时候我们3.0的功能开发直接在master分支上进行肯定不合适，我们要保证有一个稳定，可以随时发版本的分支存在（一般情况下这个角色由master分支来扮演），此时我们就可以灵活的使用Git中的分支管理功能：

1.创建一个长期分支用来开发3.0功能，假设这个分支的名字就叫v3，我们在v3上添加新功能，并不断测试，当v3稳定后，将v3合并到master分支上。  
2.创建一个特性分支用来修复2.0的bug，一旦bug修复成功，就将该分支合并到master上，一旦发现新bug，就立马再创建分支进行修复，修复成功之后再合并。

以上两个步骤同步进行，这在Svn中简直是不可想象的，因为Svn的分支管理太low，而Git能够让我们做到随心所欲的创建、合并和删除分支。
### 查看分支
我们可以通过git branch命令来查看当前仓库有哪些分支，而我们处于哪一个分支中，如下：   
![image](https://segmentfault.com/img/bVYc69?w=319&h=56)
```
git branch
```
这里显示当前仓库只有一个master分支，这是git默认创建出来的，master前面的*表示我们当前处于这一个分支中。  
若要查看远程分支（github）：
```
git branch -r
```
查看所有分支（本地加远程）:
```
git branch -a
```
PS: 若git branch -r 无法获取远程分支，ui可以看见分支但是git 命令无法查看
原因 git branch -a 这条命令并没有每一次都从远程更新仓库信息，我们可以手动更新一下(远程相关下一节介绍）:
```
git fetch origin 
git branch -a
```

### 分支的创建和切换
我们可以利用
```
git branch <分支名>
```
命令来创建一个分支，然后利用
```
git checkout <分支名>
```
来切换分支，如下：  
![image](https://segmentfault.com/img/bVYc7k?w=341&h=199)  
如果觉得这样太麻烦，可以通过
```
git checkout -b <分支名>
```
来一步到位，创建并切换分支，如下：  
![image](https://segmentfault.com/img/bVYc7A?w=301&h=190)  
也可以通过
```
git checkout -
```
命令来切换回上一个分支，如下：  
![image](https://segmentfault.com/img/bVYc7D?w=307&h=125)  
### 分支合并
现在我切换到fa分支中，由于fa分支是从master分支中创建出来的，所以此时**fa分支的内容和master分支的内容是一致的**，然后我在fa分支中向git01.txt文件添加一行内容并提交，此时fa分支中的git01.txt和master分支中git01.txt的内容就不相同了，具体操作如下：
![image](https://segmentfault.com/img/bVYc7N?w=564&h=380)  
上图展示了此时master分支和fa分支的不同，现在我通过
```
git merge --no-ff <分支名>
```
命令将fa分支合并到master分支上。其中--no-ff表示强行关闭fast-forward方式，fast-forward方式表示当条件允许时，git直接把HEAD指针指向合并分支的头，完成合并，这种方式合并速度快，但是在整个过程中没有创建commit，所以如果当我们删除掉这个分支时就再也找不回来了，因此在这里我们将之关闭。
想要合并分支，我们先切换到master分支上，然后执行
```
git merge --no-ff fa
```
命令即可完成分支合并，如下图：
![image](https://segmentfault.com/img/bVYc8f?w=564&h=380)  
合并成功后，我们看到master分支上的git01.txt上已经有了fa分支中的内容了。  
### 以图表方式查看分支
我们可以通过
```
git log --graph
```
命令来直观的查看分支的创建和合并等操作，如下图：  
![image](https://segmentfault.com/img/bVYc8m?w=543&h=635)  
### 分支衍合（这一节剩下的简单了解，更复杂的可以看Pro Git）
所谓的分支衍合其实也是分支合并的一种方式，下面我们就来看看这个分支衍合到底是什么样的。现在我的master分支的内容和fa分支的内容是保持一致的，fa是从master中创建出来的，如下图：  
![image](https://segmentfault.com/img/bVYc8v?w=189&h=182)  
现在我向fa和master中各自做一次提交，如下图：  
![image](https://segmentfault.com/img/bVYc8z?w=256&h=265)  
此时我们执行如下两条命令将两个分支合并：
```
$ git checkout fa
$ git rebase master
```
rebase命令在执行的过程中会首先把fa中的每个commit取消，并且将之保存为临时patch，再将fa分支更新为最新的master分支，然后再把那些临时的patch应用到fa上，此时fa分支将指向新创建的commit上，那些老的commit将会被丢弃，这些被丢弃的commit在执行git gc命令时会被删除。合并后的分支如下图：  
![image](https://segmentfault.com/img/bVYc8A?w=398&h=256)  
上面的git rebase master命令在执行的过程中有可能会发生冲突，发生冲突时我们有两种方案，一种直接退回到之前的状态，另一种就是解决冲突继续提交。
### 退回之前的状态
我们可以通过如下命令来回到之前的状态：
```
$ git rebase --abort
```
### 解决冲突
不过大多数情况下我们都是要解决冲突的，解决之后继续提交。此时我们用编辑器打开冲突的文件，看到的内容可能是这样的：  
![image](https://segmentfault.com/img/bVYc8P?w=322&h=128)  
上面的是HEAD中的内容，下面的是要合并的内容，根据自己的需求编辑文件，编辑完成之后，通过如下两条命令继续完成合并：  
```
$ git add git01.txt
$ git rebase --continue
```
如下图：  
![image](https://segmentfault.com/img/bVYc8V?w=516&h=130)  
### 冲突解决
我们前面提到了在分支衍合时出现冲突的解决方案，其实普通的合并也有可能出冲突，出现冲突很正常，解决就是了，git merge合并分支时如果出现冲突还是先重新编辑冲突文件，编辑完成之后，再执行git add 和git commit即可。
## 2.3 仓库、暂存区和工作区（重要）
在编辑本地文件时，git可分为如下图所示的三个区域：
![image](https://images2017.cnblogs.com/blog/1182576/201801/1182576-20180125193813131-573734094.png)  
上图的git仓库,是指本地仓库,不会更新到远程仓库(github网页上的仓库),需要使用git push -u origin master命令才行（下一章节介绍）  
文件先在工作区编辑，编辑好的文件通过add命令上传至暂存区，确定无误后可通过commit命令确认至本地仓库。此仓库可以和github端仓库互相更新。
### Git add命令
add命令将文件上传至暂存区，命令具体为：  
1. git add 添加多个文件，文件之间以空格隔开
```
git add file1 file2 file3
```
2. 多次git add
```
git add file1
git add file2
git add file2
```
3. 添加指定目录下的文件
```
config目录下及子目录下所有文件，home目录下的所有.php文件

git config/*
git home/*.php
```
4. git add . 添加所有的文件， 或者 git add --all 添加所有的文件
```
git add .
git add --all
```
5. git add 文件夹
```
git add <文件夹名>
```
add 命令使用后，可以使用
```
git status
```
命令来查看工作区及暂存区文件的状态，如图示：
![image](https://images2017.cnblogs.com/blog/1182576/201801/1182576-20180125193845444-1578601254.png)  
若要删除暂存区的文件，可以使用命令：
```
git rm file 
```
将暂存区的文件删除掉,若工作区文件存在,则需要使用
```
git rm -f file
```
来强制删除掉
### Git commit 命令
git commit 主要是将暂存区里的改动给提交到本地的版本库。每次使用git commit 命令我们都会在本地版本库生成一个40位的哈希值，这个哈希值也叫commit-id，commit-id在版本回退的时候是非常有用的，它相当于一个快照,可以在未来的任何时候通过与git reset的组合命令回到这里.
1. 
```
git commit -m "message"
```
这种是比较常见的用法，表示将暂存区全部数据录入仓库中，"message"是说明信息  
-m 参数表示可以直接输入后面的“message”，如果不加-m参数，那么是不能直接输入message的，而是会调用一个编辑器  
还有另外一种方法，当我们想要提交的message很长或者我们想描述的更清楚更简洁明了一点，我们可以使用这样的格式，如下：
```
 git commit -m '
 message1
 message2
 message3
'
```
2. 
```
git commit -a -m “massage”
```
-a:跳过暂存区,git自动将工作区里记录的所有文件添加到暂存区并一起提交,从而跳过git add步骤  
其他功能如-m参数，加的-a参数可以将所有已跟踪文件中的执行修改或删除操作的文件都提交到本地仓库，即使它们没有经过git add添加到暂存区，  
注意，新加的文件（即没有被git系统管理的文件）是不能被提交到本地仓库的。建议一般不要使用-a参数，正常的提交还是使用git add先将要改动的文件添加到暂存区，再用git commit 提交到本地版本库。
3. (简单了解)
```
git commit --amend
```
如果我们不小心提交了一版我们不满意的代码，并且给它推送到服务器了，在代码没被merge之前我们希望再修改一版满意的，而如果我们不想在服务器上abondon，那么我们怎么做呢？
git commit --amend //也叫追加提交，它可以在不增加一个新的commit-id的情况下将新修改的代码追加到前一次的commit-id中，  
（1） 假如现在版本库里最近的一版正是我们想要追加进去的那版，此时是最简单的，直接修改工作区代码，然后git add，之后就可以直接进行git push到服务器，中间不需要进行其他的操作如git pull等  
（2） 如果现在版本库里最近的一版不是我们想要追加进去的那版，那么此时我们需要将版本库里的版本回退到我们想要追加的那一版，想要将版本回退到我们想要的哪一版有好几种方法   
1） 第一种即是我们从服务器上选取我们需要的版本，直接进行挑拣，在服务器的提交管理页面上右上方一般会有一个Download按钮，点击会弹出一个下拉框，选择其中的cherry-pick，复制命令，
之后在我们版本仓库对应的目录下运行这个命令，执行完后，使用git log -1 命令，可以查看到现在版本库里最近的一版变成了我们刚才挑拣的这版，此时再在工作区直接修改代码，
改完之后进行git add，再执行本git commit --amend命令，之后git push.  
2） 使用gitk或其他的图形界面化工具，在终端输入 gitk，回车，会弹出gitk的图形界面，在界面的左侧部分陈列着版本库中的一条条commit-id，此时选中我们需要的那一版，右键点击之后会弹出一个选择菜单，如果是在master  分支上，那么其中会有一项是 Reset master branch here，点击这项，会弹出一个名为confirm reset的确认box，选择reset type 中的hard项，再点击OK，关闭gitk图形界面，回到终端，运行git log-1命令，发现现在版本库里最近的一次提交已经是我们希望的那一版了，此时再在工作区直接修改代码，改完之后进行git add，再执行本git commit --amend命令，之后git push.  
3） 如果我们知道我们需要的版本与现在最近的版本中间隔着 n 个提交，那么我们可以直接使用git reset --hard HEAD～n命令，关于git reset 命令有详解，此时这个命令执行完后，运行git log -1 命令我们会发现现在版本库里最近的一版就是我们需要的那版，此时再在工作区直接修改代码，改完之后进行git add，再执行本git commit --amend命令，之后git push.  
4） 如果我们不知道我们需要的版本与现在最近的版本中间隔着 n 个提交，那么我们可以使用git log来查看版本库中的commit-id，找到我们需要的commit-id后，在终端中执行git reset --hard commit-id，时这个命令执行完后，运行git log -1 命令我们会发现现在版本库里最近的一版就是我们需要的那版，此时再在工作区直接修改代码，改完之后进行git add，再执行本git commit --amend命令，之后git push.
4. 
```
git commit --help
```
查看帮助，还有许多参数有其他效果，一般来说了解上述三种即可满足我们工作中的日常开发了
5. 
```
git reset HEAD
```
撤销commit,如果想修改commit时的文件,则使用上面命令撤销
### 工作区和分支
1.所有git的切换分支，添加到暂存区，提交文件都是针对本地仓库，只有push是对云端仓库进行更新。  
2.当切换分支时，本地文件会跟随当前分支内容一起改变。而还没来及提交的某个分支的文件，不管怎么切换分支都会存在。举例：  
（1）在login分支下添加一个text.txt文件。切换到master分支下，text.txt文件在本地文件夹内仍然能看到。

（2）在login分支下添加一个text.txt文件，并添加到暂存区，然后切换到master分支下，text.txt文件在本地文件夹内仍然能看到。

（3）在login分支下添加一个text.txt文件添加到暂存区，通过commit命令提交到当前分支。此时text.txt文件只存在于login分支，切换到master分支就看不到text.txt文件。  
3.被修改的文件通过add命令添加到暂存区，再通过commit命令被提交到本地仓库，本地push分支到云端仓库，**云端仓库只和本地仓库单线联系**，和本地文件，暂存区没有直接联系。  
4.分支的意义相当于分工合作，也相当于对工程的检查。一个项目由多个分支组成，一个分支对应一个模块，或者一个功能，可以做到多人合作开发，最后确认功能无误在整合到master主分支。
***
***
***
# 3.远程remote
git的重点在于本地和云端的互相更新，所以和远程仓库的联系和交互**非常重要**。
基本命令(remote 、push、fetch 、pull)
## 3.1 获取远程仓库(remote)
为了便于管理，Git要求每个远程仓库都必须指定一个仓库名。  
添加远程仓库的基本命令为
```
git remote add <name> <url>
```
例如将我的远程git仓库命名为 origin （origin 为远程仓库的默认名称，也可以起其他名字）：
```
git remote add origin https://github.com/zhaoNF/git-study.git
```
可以通过如下命令来检查已加载的远程仓库：
```
git remote     (查看远程仓)
git remote -v  (查看仓及地址）
```
如图：

![image](https://s3.ax1x.com/2020/12/16/r1x0G4.png)
如下命令用于删除指定远程仓：
```
git remote remove <name>
```
查看指定远程仓详细信息：
```
git remote show <name>
```
## 3.2 从远程仓pull、fetch
### fetch
fetch 是从远程获取最新版本到本地仓库，而不是工作目录,  并且不会自动merge  
以下内容简单理解：
```
理解fetch 的关键，是理解FETCH_HEAD。FETCH_HEAD指的是: 某个branch在服务器上的最新状态'.
每一个执行过fetch操作的项目'都会存在一个FETCH_HEAD列表, 这个列表保存在 .Git/FETCH_HEAD 文件中, 其中每一行对应于远程服务器的一个分支.当前分支指向的FETCH_HEAD, 就是这个文件第一行对应的那个分支.
一般来说, 存在两种情况:  
（1）如果没有显式的指定远程分支, 则远程分支的master将作为默认的FETCH_HEAD.  
（2）如果指定了远程分支, 就将这个远程分支作为FETCH_HEAD.  
```
命令：
```
git fetch <远程仓库名>
```
将指定远程仓库的所有分支都拉取到本地。这一步其实是执行了2个关键操作：  
（1）创建并更新所有远程分支的本地分支；  
（2）设定当前分支的FETCH_HEAD为远程服务器的master分支（上面说的第一种情况)  
**需要注意的是，和push不同，fetch会自动获取远程分支“新加入”的分支**

fetch 后不带分支名，则是**所有仓库**里的分支  ：
```
git fetch 
```
注意：所取得的远程分支在本地仓库上要用"远程仓库名/分支名"的形式读取。比如origin仓库的master，就要用origin/master读取。

命令：
```
git fetch <远程仓库名> <分支名>
```
这个命令可以用来测试远程仓库的远程分支branch1是否存在, 如果存在, 返回0, 如果不存在, 返回128, 抛出一个异常。  

命令：
```
git fetch <远程仓库名> <远程分支名>：<本地分支名>
```
使用远程<远程分支名>分支在本地创建<本地分支名>(但不会切换到该分支), 如果本地不存在<本地分支名>分支, 则会自动创建一个新的<本地分支名>分支, 如果本地存在<远程分支名>分支, 并且是fast forward, 则自动合并两个分支, 否则, 会阻止以上操作。

<本地分支名>和<远程分支名>同名，则等价于
```
git fetch origin :<远程分支名>
```
取得远程分支后有两种处理方式  
（1）基于远程分支直接在本地创建一个新分支：
```
git checkout -b <本地分支名> origin/<远程分支名>
```
（2）git merge命令或者git rebase 命令，在本地分支上合并远程分支。
```
 git merge origin/master
 
 git rebase origin/master
 ```

### pull
pull是从远程获取最新版本到本地，并低程度的自动merge；但大部分还需要程序员自己判断合并（fetch+merge，建议优先使用fetch）。pull和fetch关系为：  
![image](https://pic2.zhimg.com/80/v2-af3bf6fee935820d481853e452ed2d55_720w.jpg?source=1940ef5c)
 
即：
```
git fetch orgin master //将远程仓库的master分支下载到本地当前branch中

git log -p master  ..origin/master //比较本地的master分支和origin/master分支的差别

git merge origin/master //进行合并
```
或：
```
git fetch origin master:tmp //从远程仓库master分支获取最新，在本地建立tmp分支

git diff tmp //將當前分支和tmp進行對比

git merge tmp //合并tmp分支到当前分支
```
等价于：
```
git pull origin master
```
(PS:这部分详细理解比较复杂，涉及到树结构指针的转换。深入理解可以看  
https://blog.csdn.net/a19881029/article/details/42245955  
https://www.jianshu.com/p/58a166f24c81  
咱们编辑时严格保证github和本地的仓库的追踪关系，采用fetch、merge更新本地库即可。如果本地想直接套用github版本直接pull)  


如果当前分支与远程分支存在追踪关系，git pull就可以省略远程分支名:
```
git pull origin
```
上面命令表示，本地的当前分支自动与对应的origin仓库"追踪分支"（remote-tracking branch）进行合并。

如果当前分支只有一个追踪分支，连远程仓库名都可以省略:
```
 git pull
```
## 3.3 push到远程端
git push命令用于将本地分支的更新，推送到远程仓库。它的格式与git pull命令相仿。

git分支推送/拉取顺序的写法是<来源地>:<目的地>所以push和pull肯定是相反的,push来源地是本机，pull的来源地是远程。
1. 完整写法 (老老实实用这个吧，其他的看看就好)  
命令：
```
git push <远程仓库名> <本地分支名>:<远程分支名>
```
注意，分支推送顺序的写法是<来源地>:<目的地>，所以git pull是<远程分支>:<本地分支>，而git push是<本地分支>:<远程分支>。

如：
```
git push origin master:master
```
将本地的master分支推送到远程库的master分支，如果远程分支不存在将会被创建。

2. 省略本地分支名
如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。注意第一个分支（origin/master）是不可以被删除的，

命令：删除远程分支
```
git push origin :远程分支名   # origin后面有一个空格
```
或 
```
git push origin --delete :远程分支名
```
或
```
git push origin :远程分支名 --delete
```
3. 省略远程分支名  
如果省略远程分支名，则表示将本地分支推送到与之存在Tracking（追踪关系)的远程分支，通常两分支同名。如果远程分支不存在，则会自动创建分支。

如：
```
git push origin master
```
上面命令表示，将本地的master分支推送到origin仓库的master分支。如果后者不存在，则会被新建。

 
以上当前分支与远程分支没存在追踪关系，就是谁也不是谁的upstream/downstream。

4. 全部省略  
如果当前分支只有一个追踪分支，那么仓库名都可以省略。
```
git push
```
如果当前分支与多个仓库存在追踪关系，则可以使用-u选项指定一个默认仓库，这样后面就可以不加任何参数使用git push。
```
git push -u origin master
```
上面命令将本地的master分支推送到origin，同时指定origin为默认仓库，后面就可以不加任何参数使用git push了。

还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程仓库，这时需要使用--all选项。
```
$ git push --all origin
```
上面命令表示，将所有本地分支都推送到origin仓库。

如果远程仓库的版本比本地版本更新，推送时Git会报错，要求先在本地做git pull合并差异，然后再推送到远程仓库。这时，如果你一定要推送，可以使用--force选项。
```
$ git push --force origin
```
上面命令使用--force选项，结果导致远程仓库上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用--force选项。

最后，git push不会推送标签（tag），除非使用--tags选项。
```
$ git push origin --tags
```
## 3.4 手动建立追踪
在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。比如，在git clone的时候，所有本地分支默认与远程仓库的同名分支，建立追踪关系，也就是说，本地的master分支自动"追踪"origin/master分支。**我们用clone建立联系的话默认就是跟踪，以下高端操作看看就好**  

指定master分支追踪origin/next分支:
```
git branch - -set-upstream master origin/next
```
## 3.5 操作示例
```
git clone https://github.com/lifeyx/test2.git   //下载test2仓库

cd test2                                       //进入仓库

vi 1.txt                                       //创建1.txt

git add 1.txt                                  //添加1.txt

git commit -m "第二天提交文件"                   //提交到本地仓库

git push origin master                     //上传到远程仓库地址,并输入账号密码
```
***这样github上就有新的文件了，咱们操作按这个来就行，其实也没有那么复杂。***
***
***
***
# 最后一点补充
其实不用每次push都输入账号密码的，但是方法比较麻烦，咱们的更新频率就输入密码好了，还能防忘（QWQ）。   
ssh key方法如下：https://www.cnblogs.com/lifexy/p/8353040.html

git命令基本的操作方法就差不多了，以咱们的开发强度估计够用了……吧  
水平比较有限更多高玩的设置和理解就看Pro Git吧………………希望用不到  
欢迎遇到问题继续在本文里补充哈
