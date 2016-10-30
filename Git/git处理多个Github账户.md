# git命令行处理多个github账户

## 问题引入
新建了一个github账户，用SSH克隆仓库，本地提交免密操作报了如下的错
```
ERROR: Permission to edwardcqj/freecodelearning.git denied to caoqianjie.
fatal: Could not read from remote repository.
```
## 分析
- 经检查是因为我创建了俩个ssh秘钥
```
id_rsa
id_rsa.pub
edward
edward.pub
known_hosts
config
```
- config文件冲突了    
- 原来的config文件
```
#caoqianjie
Host github.com
User 785186047@qq.com
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
#edwardcqj
HOST github.com
User caoqianjie008@163.com
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/edward
Port 443
```
> 其中Host的内容为自定义名称，在后续操作中会用到此内容，User内容为自己的Github用户名，IdentityFile为相应Github账户使用的SSH key的文件路径。

来个Host 重名了，因为我第二个github账户的一个仓库地址是  
`git@github.com:edwardcqj/freecodelearning.git`    
当用ssh提交的时候会默认到.ssh文件下找Host 为github.com的 HostName 来请求  
但是由于俩个一样默认找到第一个Host为github.com的下的User caoqianjie去访问   
所以会有`ERROR: Permission to edwardcqj/freecodelearning.git denied to caoqianjie.` 
## 解决方法
1. 更改 config文件   
把第二个的配置的Host 改为github1.com 
2. 然后把第二个github账户的仓库地址  
`git@github.com:caoqianjie/freecodelearning.git`
更改为`git@github1.com:edwardcqj/freecodelearning.git`   
get over~~
