# SSH：连接时间超时



> ssh: connect to host github.com port 22: Connection timed out

> 昨天在使用 [VsCode](https://so.csdn.net/so/search?q=VsCode&spm=1001.2101.3001.7020) 向 GitHub 上传代码时，出现错误：（此前已经使用 SSH 的方式连接了 GitHub 仓库）

```
ssh: connect to host github.com port 22: Connection timed out
Please make sure you have the correct access rights
and the repository exists.
```

> 首先输入以下命令检查 [SSH](https://so.csdn.net/so/search?q=SSH&spm=1001.2101.3001.7020) 是否能够连接成功（ssh 后面有空格）

```
ssh -T git@github.com
```

*   发现报错：

```
ssh: connect to host github.com port 22: Connection timed out
```

*   端口连接超时。

查阅资料后，找到解决办法:
-------------

> 解决方案（亲测有效）  
> 在 C 盘——用户——你的主机名文件夹中找到. ssh 文件夹；（此前配置 SSH 时会生成该文件夹）  
> 在. ssh 文件夹中新建文件 config, 不带后缀（可以新建文本文档，去掉. txt 后缀）  
> 使用 notepad++（或其他方式）打开 config 文件，输入以下内容，保存后即可

```
Host github.com
User YourEmail（你的邮箱）
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```

*   再次执行

```
ssh -T git@github.com
```

*   会出现以下提示，输入 yes 回车即可

```
The authenticity of host '[ssh.github.com]:443 ([192.30.255.123]:443)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[ssh.github.com]:443,[192.30.255.123]:443' (RSA) to the list of known hosts.
```

*   再次执行`ssh -T git@github.com`，发现验证通过

```
Hi Clare! You've successfully authenticated, but GitHub does not provide shell access.
```

*   完成以上步骤后，再次上传代码到 [GitHub](https://so.csdn.net/so/search?q=GitHub&spm=1001.2101.3001.7020) 就不会报错了
    
*   需要注意的是，**假如你的电脑上拥有多个 github 账户**，你可能事先修改过 ssh 的配置文件，你可能会将配置文件修改成这样子。
    

```
# For the account1
Host github_account1
HostName ssh.github.com
PreferredAuthentications publickey
Port 443
User git
IdentityFile ~/.ssh/id_rsa_account1

# For the account2
Host github_account2
HostName ssh.github.com
PreferredAuthentications publickey
Port 443
User git
IdentityFile ~/.ssh/id_rsa_account2
```

*   相比于普通只配置一个账户的用户，多账户用户在设置远程仓库连接时，往往设置成这个样子

```
[remote "origin"]
    url = git@github_account1:account1/my_repo.git
```

你将通过配置文件中设置的`Host`别名`github_account1`代替`github.com`访问 github，这意味着，如果你想要进行进行 git clone 操作，也应该通过别名访问，否则无法走 443 端口。假如你想 clone `git@github.com:torvalds/linux.git`，你应该修改为`git@你设置的别名:torvalds/linux.git`。或者是，额外在配置文件里加上。

```
# Just for clone
Host github.com
HostName ssh.github.com
Port 443
User git
```

参考
--

*   [1] [Using SSH over the HTTPS port](https://link.segmentfault.com/?enc=z9b3h93Lh6e7XuMMZbnX8w%3D%3D.BNc6Dzxn%2FB4BKZ9%2FOWaqEikyqmD1ZI7WSG71dBMUn8LrEv2Ibwf6rn5stTbfQNvInvh7gJDkx1541UwIwg%2Bls9uWswDaw%2Bv8rTzdtLasJmD5xPDLxgkzOX1Tq74v5AwH)
*   [2] [ssh: connect to host github.com port 22: Connection timed out](https://link.segmentfault.com/?enc=dsrH9JJOkvkFMJvfXsi6BA%3D%3D.KEvR8cgbOasreIOrfiKdDPTErhWnoyDtX5DSFTOWXQv3WORSquRoLb3e2e4vNGGggRkVjsWZu1JZnlYrwVEAeiola3iNNY2XipXMMKVmIup%2BQQ6xpojDgYLEDw32LKJKNbGHSQL6abscs2dbd9Al3g%3D%3D)
*   [3] [https://segmentfault.com/a/1190000040896781](https://segmentfault.com/a/1190000040896781)
*   [4] [https://blog.csdn.net/hdm314/article/details/119947761](https://blog.csdn.net/hdm314/article/details/119947761)
