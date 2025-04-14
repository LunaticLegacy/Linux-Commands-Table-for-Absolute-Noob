# SSH相关教程：

## 1. SSH连接流程：
建立TCP连接 - 检查身份（公钥-密码） - 会话建立 - 成功！<br>
服务器端必须有上传的公钥。如果没有公钥，则必须使用如下方法创建你的公钥（和对应的私钥）：<br>
```
$ ssh-keygen
```
## 2. SSH连接方法：
```
ssh [options] user@host
$ ssh -T git@github.com
```
其中，options的值将在下文中介绍。<br>

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
| -J | JumpHost | 通过跳板机连接 | -J | <跳板机用户@跳板机> | ssh -J user@jump user@target |
| -v | Verbose | 显示详细连接过程 | -v（可叠加-vv或-vvv） | ssh -vv user@host |
| -C | Compression | 启用传输压缩 | -C | ssh -C user@host |
| -X | X11Forwarding | 启用X11图形界面转发 | -X | ssh -X user@host firefox |
| -L | LocalForward | 本地端口转发（隧道） | -L <本地端口:目标地址:目标端口> | ssh -L 8080:localhost:80 user@gateway
| -R | RemoteForward | 远程端口转发（反向隧道） | -R <远程端口:目标地址:目标端口> | ssh -R 9000:localhost:3000 user@public-server
| -N | None | 不执行远程命令 | -N（常与端口转发配合） | ssh -N -L 3306:localhost:3306 user@host |
| -T | Terminal | 禁用终端分配 | -T | ssh -T git@github.com
| -t | terminal | 强制启用终端分配 | -t | ssh -t git@github.com


## 最后更新日期：2025/4/14 17:51
