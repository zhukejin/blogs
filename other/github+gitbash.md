通过 git bash 配置和使用 github
--

>

>本来在window 电脑使用的是 github for windows ， 大概是一年前安装的，也没用他提供的 gui ， 只是使用自带的 gitshell， 使用起来也是蛮流畅的，但是自从换了台电脑后， 发现在 windows.github.com 中下载的 github for windows 客户端不是那么当好用了， github shell 也是各种配置不好， 于是就另谋他法， 两年前使用 github 的时候， 使用的是 mysgit 工具， 虽然配置起来很繁琐， 但是也只能这样了。

>本文整理了一些互联网资料加上个人理解

> 这里创建的名字是我自己的名字， 到时候换成个子名字即可


_如果是github 新手， 那么先去自行创建账号and 仓库， 下面仅仅是解释如何用 git 命令行配置 github_

### 1. 本地配置

打开 git 客户端创建 ssh key, 键入指令

	ssh-keygen -t rsa -C "zhukejin@zhukejin.com"

这里-C后面是邮箱地址， 换成自己的邮箱

然后配置个人信息，键入指令

	git config --global user.name "primo" //这里是使用github的用户名
	git config --global user.email "zhukejin@zhukejin.com" //这里是github的邮箱

我个人的理解是 这一段非必须的， 因为接下来配置完 public key 后， clone 一个仓库， 就会自动生成这些东西。

### 2. 服务端配置

进入自己的github 主页， setting-> ssh keys （**这里注明一下， 有很多教程中描述的位置是老版的github, Account Settings-sshkeys**）， 然后点击 Add SSH key 按钮， 新增一个 ssh key

titel可以理解为这个key的注释， 进入 C:\Users\zhukejin\.ssh 中， 打开 id rsa.pub , 类似这样的一串：

	ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA3pt+juqyo1vN0nySyIZCjlEQQ50kxq/p86dsh56ybSfgbVEaoOygJXwo0CWHpYKsADeyeI3vrMasdgqr68dasduEtRrFZpIjaRbkLUCI+kUb7wOcedEjYD8/Uxintuc/uUmwRvE0afJf9ABMqJNNYjKOHqXG7cOgDL2sDRoasdsdun9vPJfzL/fgNwt75vEGoPm9IRQss/jF/wE9GLJrSAvZGCRB4G4W2z1q6N6xo2fe6NoChgGasdGo45DeRSo8asdfNwwVmFTM7hvX6xDx2lXPEjbcY1zuw== zhukejin@zhukejin.com

把这一段复制进 github 页面上的 key 中， 点击  add key。

这样 sshkeys中就多了一个刚刚添加的 key。

然后去 客户端验证一下， 键入：

	ssh -T git@github.com


如果是第一次执行这个命令， 会出现确认指令 xxxxxxxxxxxxxxxx(yes/no)? 

键入yes， 会提示， 已经认证成功， 但是github不提供 shell 通道巴拉巴拉(you've successfully auth..... but github does not provide shell access), 这个不用理他就行了。


### 3. 使用 git 提交代码

首先得在 github 有个仓库， 如果没有仓库先去创建一个， 我这里有个现成的仓库 "WebLabs",
那么我在本地的一个文件夹中键入：

	git clone git@github.com:zhukejin1223/WebLabs.git

如果之前配置没问题， 就会将此仓库克隆到本地，如果有问题， 就事论事再解决。

修改本地代码，提交到master 分支：

	git add *
	git commit * //这里需要记录提交信息
	git push origin master

静待完成即可。

具体的 git 分支啊， 合并啊的命令， 网上有很多。


如有不对，请指正。

Email ： zhukejin@msn.com	