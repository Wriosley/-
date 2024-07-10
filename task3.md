# 有关Git的使用

## git下载

官网下载 https://git-scm.com/

安装完以后 应该有一个git bash的软件可以打开。

在下面我们使用的都是这个bash

> Q：为什么不用windows自带的命令行？
>
> A：都是可以的，bash只是让大家更快速的熟悉linux的命令行

## git和github和gitee的关系

git是管理代码的工具，可以记录源码的历史，本地

github和gitee都是托管仓库，在线

使用git可以方便的推送代码到远程仓库，也可以方便的进行**多人协作**

很多代码仓库的逻辑都是一样的，感兴趣的可以自己部署一个[gitea](https://docs.gitea.com/category/installation)

但在这里，我们使用[github](https://github.com)

## Git的配置

首先，要配置电脑的ssh key

打开bash/cmd/powershell

```bash
ssh-keygen -t rsa -C "<email>"
```

这个email应该对应你的github/gitee邮箱，事实上两个托管平台用一样的就行（`-C`参数可以不添加）

敲三次回车一路到底就行（可以输入文件名/密码，但不是必要的）

这时候会在你的用户目录下的.ssh文件夹里生成一个id_rsa （私钥） id_rsa.pub（公钥），我们使用公钥

有关什么是公钥、私钥参见[文章链接](https://www.ssldragon.com/zh/blog/public-key-cryptography/)

```bash
cat .ssh/id_rsa.pub
```

获取命令行输出的公钥内容，全部复制

> Q：用户目录在哪里？
>
> A：打开你的bash，输入pwd，得到的内容就是你的当前用户目录s

登陆github，进入到个人的Settings

添加sshkey，直接粘贴刚刚的公钥就行，确认。没什么问题的话一封邮件会发到你的邮箱告知你添加了这个公钥。现在，理论上你这台电脑就能连接到github了

```bash
ssh -T git@github.com
```

![](https://cdn.jsdelivr.net/gh/cyan4run/cdn_img/img/202404222337770.png)

> 如果换台电脑，同样的流程也得走一遍

接下来配置git

还是在命令行（推荐用git的bash）

```bash
git config --global user.name "your_github_username" 
git config --global user.email "your_email"
```

其中用户名是你注册github的**账号名字**

邮箱是注册使用的**邮箱**，没什么问题就和上面公钥用的一样

```bash
git config --global --list
```

可以看到自己的配置

## 如何连接远程

### 从本地项目到远程

在github中新建一个仓库，什么都不添加

```bash
git init #初始化git 生成一个.git文件夹在当前目录,默认创建master分支
git add . #添加文件到暂存区，.代表所有文件
git commit -m "your commit"
git remote add origin <your_ssh_url> #建议使用ssh url,原因下面有讲
git push -u origin master #推送远程，以后用git push就行
```

![](https://cdn.jsdelivr.net/gh/cyan4run/cdn_img/img/202404222338011.png)

![](https://cdn.jsdelivr.net/gh/cyan4run/cdn_img/img/202404222338838.png)

这样在github页面上你应该能看到你的添加的文件了

### 远程项目到本地工作

我们可以先在github创建一个仓库

```bash
git clone <http_url OR ssh_url>
cd example #进入项目文件夹
git pull #每次开启你的工作前，检查是否是最新的
git checkout master #切换到master分支
git add . #添加文件到暂存区，.代表所有文件
git commit -m "your commit"
```

以上操作都是正常操作

但是这时候推送，可能会遇到这样的问题：

鉴权失败：github移除了密码验证

![](https://cdn.jsdelivr.net/gh/cyan4run/cdn_img/img/202404222338241.png)

这个时候实际上就是上文要使用ssh的问题

解决办法如下

```bash
git remote -v #可以看到的确git remote现在是http url 因为clone用的http
git remote rm origin
git reamote add origin git@github.com:cyan4run/example.git #添加ssh的远程
git remote -v #检查一下
```

![](https://cdn.jsdelivr.net/gh/cyan4run/cdn_img/img/202404222338887.png)

然后愉快的推送到仓库

```bash
git push -u origin master
git push #第一次推送以后以后可以用这个
```

![](https://cdn.jsdelivr.net/gh/cyan4run/cdn_img/img/202404222339907.png)

另外的解决办法是，用token替代你的密码，不介绍了，去setting添加就行

## 关于多人协作

将多人协作fork到自己的账号仓库下以后，操作和 **远程项目到本地工作** 相同，参见上文

![1](https://cdn.jsdelivr.net/gh/wandering-the-earth/blog-img//blog/202404221056918.png)

推送到自己的仓库以后，请提交pull request等待多人协作仓库管理员的审核，即可合并到主分支完成一次贡献

# 本章总结性练习

**目标：**搭建一个基于hexo或者mkdocs的个人博客，要求是至少在本地能够跑起来，发布一篇本章有关的笔记（几句话即可）

**截止时间：**在你看到这行字以后的72个小时之内

**你可以参考的内容**：

为了新手友好，进行一个提示，你可以在**CSDN**上找到**非常友好的教程**

同时，本章的教程已经提供了一些你可能在部署途中遇到的坑

当然，更加要说明的是：请不要将搜索局限于csdn !!!

**考核方式：**

单人制考核，每个人都需要在自己的主机上跑起来这个博客项目

- 优秀：完整部署一个博客，在公网上能被访问（无需域名）
- 良好：完整部署一个博客，本机能够运行
- 不及格：跑不起来项目，无法部署

没有及格这一档，因为跑不起来就是不及格

**解决问题的办法**

如果你有技术问题，多问老师or小组成员or大群交流

问问题方式参考task1.md

**对本练习的问题直接大群里问，很可能其他人也有这个问题**

