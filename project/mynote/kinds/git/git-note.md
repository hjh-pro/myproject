[TOC]

# myself-note

## 1、git命令

1、git init； -- 初始化git版本控制工具，初始仓库

2、touch readme.txt  -- 创建文件

3、vi readme.txt  -- 编辑文件

4、 cat readme.txt -- 读取文件里面的内容

5、git status  -- 查看文件状态

6、git add readme.txt  -- 将文件提交到暂存区

7、git commit -m '第一次提交'  -- 将暂存区的内容提交到master

8、rm -rf hh.txt  -- 删除txt文件

9、git diff 文件名  --查看当前文件和当前分支的文件内容的区别 如果没有区别就不显示任何东西

10、git log 文件名 -- 显示当前分支的文件的提交记录

11、git checkout 文件名 -- 撤销文件(文件在工作区)

12、git reset --hard HEAD^  --回退上一个版本

13、git reset --hard HEAD^^  --回退上2个版本

14、git reset --hard 版本号   --回退到对应版本,版本号可以只写前面一部分 

15、git rm -rf 文件名   -- 删除分支里面的文件

16、(HEAD -> master): 表示当前所处的文件

17、git add *  -- 将所有文件提交到暂存区

18、git branch  查看当前仓库的所有分支

19、git branch dev 在当前仓库创建一个名为dev的分支

20、git checkout -b dev 在当前仓库创建一个名为dev的分支并切换到dev分支

21、git checkout dev 切换到dev分支

22、git branch -d v1  删除v1分支   【先离开这个分支】

23、git merge 分支名

24、git stash -- 存储当前工作环境

25、git stash list -- 列出当前分支存储的所有工作环境

26、git stash pop --恢复工作环境

27、git remote add origin [github/gitee上面的ssh/http] --关联github上创建的仓库

28、git push -u origin master  --把本的仓库的数据推送到github

29、git clone [github/gitee上面的ssh/http] --从远程仓库克隆代码

30、git checkout -b dev origin/dev  --创建分支并关联远程仓库的分支

31、git remote --查看远程库的信息

32、git remote -v --查看远程库的详细信息

33、git pull origin master   --拉取分支

安装git后执行的命令：

34、git config --global user.name "Your Name"

35、git config --global user.email "email@example.com"

## 2、git相关操作

**03【掌握】文件管理-添加文件**

相关命令

git init  初始仓库

git status 查看工作区的状态  如果出现working tree is clean 说明工作区里面的数据和分支的数据是一样的

git add  文件1  文件2  把文件提交到暂存区

git commit -m ‘批注信息’ 把当前分支的暂存的所有数据提交到当前分支

git diff 文件名  查看当前文件和当前分支的文件内容的区别 如果没有区别就不显示任何东西

```
$ git diff readme.txt

warning: LF will be replaced by CRLF in readme.txt.

The file will have its original line endings in your working directory

diff --git a/readme.txt b/readme.txt

index a55bb8e..10681c7 100644

--- a/readme.txt

+++ b/readme.txt

@@ -1,2 +1,3 @@

 添加第一行数据

 hjh

+添加第二行数据
```

**04【掌握】文件管理-撤销及版本回退**

分三个区: 工作区  暂存区   分支

**相关命令**

git log 文件名

![img](data\qqAC860E386EC9643055C01F91E84D8F92\93573fbb96854e9488098620fab9bda3\wps1.jpeg)

显示当前分支的文件的提交记录

 

 

**文件只是在工作区****[撤销]**

git checkout 文件名

**文件到分支里面****[回退]**

![img](data\qqAC860E386EC9643055C01F91E84D8F92\8e57d1068eed44f4a1d85262fb63daec\wps2.jpeg)

 

 

Git reset –hard HEAD^  回退上一个版本

![img](data\qqAC860E386EC9643055C01F91E84D8F92\e94d86f754ce48eb8db44c64492c89e1\wps3.jpeg)

 

 

Git reset --hard 版本号

版本号可以使用git log 文件名查看

**05【掌握】工作区和暂存区**

![img](data\qqAC860E386EC9643055C01F91E84D8F92\7e433b0b0ac343c08d6d1a4e9ca0bed2\wps5.jpeg)

 

如果在仓库添加一个文件，默认在工作区

如果使用add 之到暂存区

如果使用commit 之后  是提交到当前分支 

**06【掌握】文件管理-删除文件**

![img](data\qqAC860E386EC9643055C01F91E84D8F92\ff6d8d12b121427a828c3a3c71f20477\wps6.jpeg)

 

先使用git rm -rf 文件名  删除

 

再提交 一次

 

**08【掌握】****分支管理**

**相关命令**

git branch  查看当前仓库的所有分支

git branch dev 在当前仓库创建一个名为dev的分支

git checkout -b dev 在当前仓库创建一个名为dev的分支并切换到dev分支

git checkout dev 切换到dev分支

git branch -d v1  删除v1分支  【先离开这个分支】

 

 

将dev合并到master里面去:（在master分支里面操作）

git merge 分支名  

 

 

冲突问题？

先合并

再手动解决

再添加

再提交

**10【了解】分支管理-Bug分支**

![img](data\qqAC860E386EC9643055C01F91E84D8F92\5d0eb2f9358941a58f5f80212a5c2d02\wps7.jpeg)

 

存储当前工作环境

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\d7f0ccc9c39345bea087f84c4b65e78d\wps8.jpeg)

 

列出当前分支存储的所有工作环境

![img](data\qqAC860E386EC9643055C01F91E84D8F92\ea868fa1e7f94c9eb9bd1547c8b7c5d9\wps9.jpeg)

 

恢复工作环境

## 3、git关联github

**Git****是分布式版本控制系统，同一个****Git****仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以****“****克隆****”****这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。**

**1****，远程库存配置**

**第****1****步：创建****SSH Key；**

**第****2****步：登陆****GitHub****，打开****“Account****à****settings”****，****“SSH Keys”**

**2****，添加远程仓库**

**在****github****上创建仓库**

![img](data\qqAC860E386EC9643055C01F91E84D8F92\6639aee6b2664061a37661abd0dc5893\wps10.jpeg)

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\b88cfe0d467f451dbe6cbaaf8bea3290\wps11.jpeg)

**3****，从自己远程仓库克隆代码**

git clone [git@github.com:leijhArvin/my0520test.git](mailto:git@github.com:leijhArvin/my0520test.git)

默认的clone是把远程仓库里面的所有分支全部克隆下来 但是本地只会有master分支

 

如何把其它分支关联起来

 

创建分支并关联远程仓库的分支

 

git checkout -b dev origin/dev

 

 

**4****，从其它远程仓库克隆代码**

只在推送才需要ssh  克隆不用

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\7955d14210ae40d086a34690c020ec86\wps12.jpeg)

**13【掌握】分支管理-多人协作**

**要查看远程库的信息，用****git remote：**

![img](data\qqAC860E386EC9643055C01F91E84D8F92\3dd7caed0b8149e3b426527432ef86e4\wps13.jpeg)

 

**要查看远程库的详细信息，用****git remote -v：**

![img](data\qqAC860E386EC9643055C01F91E84D8F92\cb0430250d3d45cc8e7d6859b8698589\wps14.jpeg)

 

Fetch  代表有拉取文件的权限

Push 代表有退送文件的权限

 

**推送分支**

git push origin master

git push origin dev

 

**拉取分支**

git pull origin master

 

**冲突问题**

在不同的目录(A,B)下分别拉取同一个远程库

在A里面操作文件并提交

在B里面操作文件并提交

 

此时B提交时会出现

![img](data\qqAC860E386EC9643055C01F91E84D8F92\7db58adcc5644f31b78ef195c6fe8590\wps15.jpeg)

 

就是因为本地的基本仓库和远程库不一致

先更新再推送:

先进行git pull origin master ,

再进行手动合并（包括add,commit）,

最后git push origin master.

## 4、git关联gitee

**1，概述**     

​       使用GitHub时，国内的用户经常遇到的问题是访问速度太慢，有时候还会出现无法连接的情况（原因你懂的）。 如果我们希望体验Git飞一般的速度，可以使用国内的Git托管服务——码云（https://gitee.com）。

​    和GitHub相比，码云也提供免费的Git仓库。此外，还集成了代码质量检测、项目演示等功能。对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务，5人以下小团队免费。

 **码云的免费版本也提供私有库功能，只是有5人的成员上限。**

**2，设置公匙**

使用码云和使用GitHub类似，我们在码云上注册账号并登录后，需要先上传自己的SSH公钥。选择右上角用户头像 -> 菜单“修改资料”，然后选择“SSH公钥”，填写一个便于识别的标题，然后把用户主目录下的.ssh/id_rsa.pub文件的内容粘贴进去：

**3，创建项目**

在码云上创建一个新的项目，选择右上角用户头像 -> 菜单“控制面板”，然后点击“创建项目”：

![img](data\qqAC860E386EC9643055C01F91E84D8F92\3fd0823a026a4bfb83e5cdaaeb4e48a5\wps16.jpeg)

 

项目名称最好与本地库保持一致：

然后，我们在本地库上使用命令git remote add把它和码云的远程库关联：

 

git remote add origin https://gitee.com/leijharvin/repository.git

**4，拉取远程代码库** 

由于在创建远程仓库时会初始化一个README.md文件，而本地仓库里没有，所以需要先执行pull操作将远程仓库拉取合并到本地仓库，否则会出错。执行代码：

 

git pull origin master

**15【掌握】eclipse里面使用gitee**

之前使用bash连接github时做了哪些事

1， 生成ras.pub  公钥

2， 配置sshkey

3， 创建仓库

4， 关联远程库

5， 向仓库推送数据

6， 拉取数据

 

**一，把项目分享到****gitee**

**1****、准备工作**

1、一个注册好的码云帐号 https://gitee.com/

2、安装好的Eclipse （本示例用的版本 sts ，已经集成Git）

 

**2****、****Eclipse** **配置****Git** **相关帐号资料**

1、路径： Window --- Preferences --- Team--- Git --- Configuration --- User Settings --- Add Entry --- 如下图：

a. user.email ： “ 您的码云帐号”

b. user.name： "提交时显示的名字，自定义。"

c. 等价于git命令： git config --global user.name/user.email "xxxx"

![img](data\qqAC860E386EC9643055C01F91E84D8F92\bf5284a298214356b877f6bf5d995eb5\wps17.jpeg)

 

 

**3****、****Eclipse** **配置** **publickKey**

1、生成 key 路径： Window --- Preferences --- General --- Network Connections --- SSH2 --- Key Management --- Generate RSA Key ---

![img](data\qqAC860E386EC9643055C01F91E84D8F92\a4a6dfc78ed946a58d7f15f2df6e3e44\wps18.jpeg)

 

​    2、获取 key 路径： Window --- Preferences --- General --- Network Connections --- SSH2 --- General

![img](data\qqAC860E386EC9643055C01F91E84D8F92\745110949e6644eaa54d1392208d549d\wps19.jpeg)

 

**4****、在码云中配置 用户** **ssh key**

1、步骤： 登录码云 --- 进入 个人设置中心 --- SSH公钥 （即： 用户 ssh key）

![img](data\qqAC860E386EC9643055C01F91E84D8F92\b0eb455ffce648e9abbc372f5a076cb7\wps20.jpeg)

 

  2、补充：为什么要配置用户 ssh key 而非 项目 ssh key？

项目的 sshkey 只针对项目，且我们仅对项目提供了部署公钥，即项目下的公钥仅能拉取项目，这通常用于生产服务器拉取仓库的代码。

用户的 key 则是针对用户的，用户添加了 key 就对用户名下的项目和用户参加了的项目具有权限，一般而言，用户的key具有推送和拉取的权限，而项目的 key 则只具有拉取权限。

 

3、补充：验证添加的 key 是否生效。

在 Git Bash 中输入命令： ssh -T git@gitee.com

返回： Welcome to Gitee.com, xxx ! ---- 则表示添加key成功！如下

![img](data\qqAC860E386EC9643055C01F91E84D8F92\42c3d67f02a94f2dad1a32e6f603961d\wps21.png)

 

**5****、****Eclipse****项目分享到码云**

1、码云上创建一个项目，名字：helloworld 注意不要创建README.md这个文件

![img](data\qqAC860E386EC9643055C01F91E84D8F92\715c18887204489183b375f1ee2f58a1\wps22.jpeg)

 

 

2、Eclipse上创建一个项目，名字：helloworld

![img](data\qqAC860E386EC9643055C01F91E84D8F92\74a3e851c4c040e9a619760605ee9723\wps23.jpeg)

 

 

3、Eclipse项目分享到码云： 选中项目gitee --- 右键 --- Team --- Share Project --- Git --- 如下

![img](data\qqAC860E386EC9643055C01F91E84D8F92\a64020b37b7446d9871e9b38e5e16430\wps24.jpeg)

 

4、提交项目： 选中项目helloworld--- 右键 --- Team --- Commit ---

选择中要提交的文件添加到暂存区

![img](data\qqAC860E386EC9643055C01F91E84D8F92\9a9d8a07a6bb4311b913531145c4cd41\wps25.jpeg)

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\4f4713e45de04db3af7c1429a5a4b9f4\wps26.jpeg)

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\4f1717d70fb84c31a3e2af4ce9652352\wps27.jpeg)

 

以上的两个按钮，第一个是提交到本地仓库并push到gitee

第二个按钮是提交到本地仓库

push到gitee

右键项目--team-->push Branch master

![img](data\qqAC860E386EC9643055C01F91E84D8F92\91247b51171a4c7c890268931f42f1a1\wps28.jpeg)

 

输入密码找回的问题

![img](data\qqAC860E386EC9643055C01F91E84D8F92\94fdae42ae8c4d6392169b411a57dcec\wps29.jpeg)

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\cc273f6d2c2d4e54919723269d0db655\wps30.jpeg)

 

之后再看gitee里的代码就成功了

 

 

 

当gitee里面的项目内容发生改变时，如何更新到本地的eclipse当中？

右击项目-》选择Team-》点击pull，即可(或者选择第二个pull，参数是master)。

![img](data\qqAC860E386EC9643055C01F91E84D8F92\8bb2166baa34494f95f6cb997bd433b5\wps31.png)

 

创建分支：

![img](data\qqAC860E386EC9643055C01F91E84D8F92\99f4584ab4a545b58d75955b0e96caa5\wps32.png)

 

 

 

删除分支：

![img](data\qqAC860E386EC9643055C01F91E84D8F92\b5e3780466944f92954feb26f6d190f8\wps33.png)

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\6d61778e1b084f40bc12ee5e3907e3cc\wps34.jpeg)

 

 

 

在eclipse中修改了代码，撤回到前面的版本(在工作区，还未提交到gitee)

右键->Team->Show Local History->右键回退的版本->get contents,即可

**二，从****gitee****把项目拉取下来**

1，找到项目地址

![img](data\qqAC860E386EC9643055C01F91E84D8F92\11d2e6a54e9740b1bd756c644f17b1e1\wps35.jpeg)

 

2，项目区域右键import  选择git

![img](data\qqAC860E386EC9643055C01F91E84D8F92\785acf271bb14876a464d75cf7d99676\wps36.png)

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\df92a6eea87141429f2dfe67e33cbf37\wps37.png)

 

![img](data\qqAC860E386EC9643055C01F91E84D8F92\dd597d33b7f144f1a80f547f14820d2c\wps38.jpeg)

 

选择保存路径完事

![img](data\qqAC860E386EC9643055C01F91E84D8F92\ab9a11e14e054493b426bcf81537658c\wps39.jpeg)

 

 