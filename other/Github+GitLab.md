在一台电脑上同时配置 Github 和 GitLab
--

> 公司Git 仓库托管在 GitLab 上，而我个人研究的小玩意在GitHub 上，为了能同时满足双线 update 需求，决定通过Config 分离两个账号。


** 1. 生成单独的 SSH Key **

	//GitLab 生成方法一样， 或者默认先生成Gitlab， 以公司账户为主
	ssh-keygen -t rsa -f ~/.ssh/id_rsa_github -C "zhukejin@msn.com"

** 2. 添加 SSH Key 到Github **

1. Github-> Setting -> SSH and GPG Keys -> 右上角 New SSH Key
2. cat ~/.ssh/id_rsa_github.pub 
3. 将其中的 Public Key 添加到 Github 中

** 3. 添加 Config **

1. vim ~/.ssh/config
2. 加入以下配置

	
		# gitlab
		Host gitlab.com
    	HostName gitlab.com
    	PreferredAuthentications publickey
    	IdentityFile ~/.ssh/id_rsa

		# github
		Host github.com
	    HostName github.com
    	PreferredAuthentications publickey
	    IdentityFile ~/.ssh/id_rsa_github
	    

** 4. 测试 **

	ssh -T git@github.com
	
如果出现:

	Hi zhukejin1223! You've successfully authenticated, but GitHub does not provide shell access.
	
这样.. 就说明github 副Key 配置成功了，然后再去试一下 GitLab 能不能提交了...


	