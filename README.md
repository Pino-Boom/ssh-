# ssh-
通过设置公钥私钥认证来达到免密登录效果
> 通常为了方便登录Linux服务器,配置自动化脚本等需求会使用公钥私钥认证登录,以下是自己记录的步骤
## ssh 免密登录
1. 在本机上创建`公私钥`
```shell
jokechatAir:~ jokechat$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/jokechat/.ssh/id_rsa): productServer
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in productServer.
Your public key has been saved in productServer.pub.
The key fingerprint is:
SHA256:Ojya9JHdUjAv3NMc5w9fIvTwrUemF+ncPWCGhxyROMY jokechat@jokechatAir.local
The key's randomart image is:
+---[RSA 2048]----+
|        . ...    |
|         E ..    |
|        + ..+ .  |
|       . =.++B ..|
|        S =++=*o=|
|     . + + .+.+X=|
|    . B o .   o+B|
|   . + + .     o.|
|    o .          |
+----[SHA256]-----+
jokechatAir:~ jokechat$
```
2. 用`ssh-copy-id`将公钥复制或传输到远程服务器,并将身份标识文件追加到服务器的 `~/.ssh/authorized_keys`中
```shell
jokechatAir:~ jokechat$ ssh-copy-id -i productServer.pub username@domain
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "productServer.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
jokechat@ssh.jokechat.cn's password: 
Number of key(s) added:        1
Now try logging into the machine, with:   "ssh 'username@domain'"
and check to make sure that only the key(s) you wanted were added.
jokechatAir:~ jokechat$
```
3. 配置本机ssh config文件
```shell
Host jokechat
      HostName domain
      User username
      Port 22
      PubkeyAuthentication yes 
      IdentityFile ~/.ssh/productServer
```
4. 修改服务器 `/etc/ssh/sshd_config`,并重启sshd服务
```shell
RSAAuthentication yes 
PubkeyAuthentication yes 
AuthorizedKeysFile      ~/.ssh/authorized_keys
```
