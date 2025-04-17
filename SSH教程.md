# SSH教程：
**Tutorial for Secure SHell** <br>
Auth: **月と猫 - LunaNeko** (GitHub: LunaticLegacy) <br>
Environment: **Debian 12 in VMWare WorkStation and Windows 11** <br>

本文作为个人笔记，可作为新手快速入门教程食用，但**可能存在错误**。<br>
所有能被写进这个.md文件里的都是我亲自实验过或者来自文档内容的。<br>

## 1. SSH连接流程：
建立**TCP连接** - 检查身份 - 会话建立 - 成功！<br>
SSH**默认使用端口22**。<br>
连接时远程服务器端必须有对应用户上传的公钥，而你手里必须有对应的私钥。<br>
可以使用如下方法创建你的密钥对（公钥+私钥）：<br>
```bash
$ ssh-keygen
```
## 2. SSH连接方法：
ssh [options] user@host
其中，options的值将在下文中介绍。<br>
例如：
```
$ ssh -T git@github.com
```

## 3. SSH断开方法：
```
$ exit
```
或者：ctrl+D <br>

## 4. options的可用值列表
| 选项名 | 速记 | 作用 | 用法 | 示例 |
|----------|------------|----------|----------|--------------|
| -p | Port | 指定连接端口 | -p \<port\> | ssh -p 1024 git@github.com |
| -i | Identify File | 指定私钥路径 | -i \<target\> | ssh -i ~/.ssh/custom_key user@host |
| -J | JumpHost | 通过跳板机连接 | -J <跳板机用户@跳板机> | ssh -J user@jump user@target |
| -v | Verbose | 显示详细连接过程 | -v（可叠加-vv或-vvv以获得更详细信息） | ssh -vv user@host |
| -C | Compression | 启用传输压缩 | -C | ssh -C user@host |
| -X | X11Forwarding | 启用X11图形界面转发 | -X | ssh -X user@host firefox |
| -L | LocalForward | 本地端口转发（隧道） | -L <本地端口:目标地址:目标端口> | ssh -L 8080:localhost:80 user@gateway
| -R | RemoteForward | 远程端口转发（反向隧道） | -R <远程端口:目标地址:目标端口> | ssh -R 9000:localhost:3000 user@public-server
| -N | None | 不执行远程命令 | -N（常与端口转发配合） | ssh -N -L 3306:localhost:3306 user@host |
| -T | Terminal | 禁用终端分配 | -T | ssh -T git@github.com
| -t | terminal | 强制启用终端分配 | -t | ssh -t git@github.com

## 6. 常见问题

### 6.1  WARNING: UNPROTECTED PRIVATE KEY FILE!
```bash
PS E:\Codes\SSH\small_laptop_keys> ssh -i ./keys git@github.com
Bad permissions. Try removing permissions for user: NT AUTHORITY\\Authenticated Users (S-1-5-11) on file E:/Codes/SSH/small_laptop_keys/keys.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions for './keys' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "./keys": bad permissions
git@github.com: Permission denied (publickey).
```
一般而言，SSH的私钥文件权限必须保证**仅文件所有者具有读写权限**。（在Linux上，权限组为600或700）<br>
如果私钥的读写权限过于宽泛，以至于可被拥有者之外的用户读写，则SSH会因此拒绝使用本私钥内容。<br>
如果使用Windows内置的文件复制-粘贴功能迁移私钥，请注意：**一定要在迁移完毕后修改私钥的权限组。**<br>
在Linux系统中，可使用`chmod 600 ./keys`更改权限。<br>
在Windows系统中，右键单击文件夹，然后按照`属性->安全->权限->高级`找到权限组，并修改。


### 6.2 忘记密码（Passphrase）
```bash
PS E:\Codes\SSH\small_laptop_keys> ssh -i ./keys git@github.com
Enter passphrase for key './keys':
Enter passphrase for key './keys':
Enter passphrase for key './keys':
git@github.com: Permission denied (publickey).
```
无解，重新做一份key并重新上传一份吧。<br>

## 7. 杂项

### 7.1 身份检查流程
服务端需要一份由客户端上传的公钥，才可与客户端进行SSH身份认证。<br>
以ED27759为例，流程如下：

1. 服务器生成一段随机的 **质询字符串（Challenge）** A，在求哈希值A'=Hash(A)后，将A'发送至客户端。<br>
2. 客户端收到A'后使用私钥加密A'，即C'=Encrypt(A')，随后将A'和C'返回给服务器。
3. 接下来服务器会对A'执行E'=Hash(A)，然后解密C'，得A''=Decrypt(C')，并将结果求哈希得E''=Hash(A'')。<br>
4. 比对E'和E''，如果两者相等则签名验证通过，身份验证成功。否则身份验证失败。<br>

私钥的本地密码（Passphrase）会被作为**以对称加密算法对私钥进行加密**的密钥。<br>
*签名过程：生成字符串 -> Hash -> 发送给客户端 -> 使用客户端私钥加密 -> 发送给服务器*

### 7.2 连接到用户后，如果远程服务器对该用户也有密码，还需要输入用户密码吗？
如果在服务器文件`/etc/ssh/sshd_config`中，字段`AuthenticationMethods`未显式要求密码，<br>
则只要公钥验证通过，就不会再提示输入用户密码，可直接登入。<br>
如果管理员在`sshd_config`中设置了：
```text
AuthenticationMethods publickey,password
```
则意味着“必须同时通过公钥签名验证和用户密码验证”才算完整认证流程。这时，在公钥阶段后你还会看到类似：
```bash
$ user@server's password:
```
提示你输入该账户在服务器上的登录密码。<br>
但如果设置了：
```
AuthenticationMethods publickey
```
则意味着“仅通过公钥签名验证”即可（禁用密码）。这时公钥验证结束后，身份认证直接通过，不会再看到类似提示。<br>
*小声：sshd_config文件不一定存在。它作为服务端行为，而ssh_config为客户端行为*

## 最后更新日期：2025/4/17 18:35
