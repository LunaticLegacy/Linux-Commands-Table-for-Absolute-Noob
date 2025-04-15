## tmux 教程
**Tutorial for Terminal MUltipleXer** <br>
Auth: **月と猫 - LunaNeko** (GitHub: LunaticLegacy) <br>
Environment: **Debian 12 in VMWare WorkStation** <br>

本教程仅为新手快速入门教程，且**可能存在大量错误**。<br>
所有能被写进这个.md文件里的都是我亲自实验过，或者来自文档内容的。<br>
单位：tmux会话（最大）->tmux窗口（最小单元）<br>

## 1. 创建并启动会话
创建会话的方法有如下若干种：<br>

```bash
tmux new -s mysession
```

如果没有指定会话名则会自动创建一个会话，名字默认为0。<br>
如果以此方法创建了新的tmux会话，则该数字会继续增长。<br>
此外，一个新的会话被创建时，**必定会创建一个新窗口**。且当会话内没有窗口时，会话自动终结。<br>

```bash
tmux
```

会话创建完毕后将**默认进入该会话**中，此时屏幕下方将显示一条绿条。<br>
绿条内容相当于Windows系统窗口中的任务栏。<br>
在每一个tmux会话中，下方绿条都会不同。<br>
例如：

```
[0] 0:main  1:bash  2:stars- 3:bash  4:[tmux]* 5:bash  6:bash  
```
此时会话名称为0，有7个窗口。

## 2. 在会话内创建窗口 
请**不要**尝试在一个tmux会话中，使用`tmux`或`tmux new`创建新窗口。否则会这样：<br>
```bash
devcontainers@Device:~$ tmux
sessions should be nested with care, unset $TMUX to force
devcontainers@Device:~$ echo $TMUX
/tmp/tmux-1000/default,20647,0
```
如果要创建新窗口，请[查看常用快捷键列表的Ctrl+B系列](#31-ctrlb系列--ctrlb)。<br>
##### 小声：如果你要强行覆写掉`$tmux`的话，这相当于在一个tmux会话里叠加一个tmux会话——以此造成的会话紊乱鄙人概不负责。

## 3. 常用快捷键列表 
### 3.1 Ctrl+B系列 <br> 
{#ctrl+b}
本系列快捷键需按照以下时序使用：<br>
按下Ctrl+B -> 松开Ctrl+B -> 按下对应按键。<br>

| 快捷键 | 速记 | 功能 | 备注 |
| ------- | ------- | ------- | ------- |
| ? | ？ | 显示快捷键列表 | |
| % | percent | 按当前界面上下对半分，向右侧分屏 | 分屏窗口会共享任务栏内的tmux窗口。 |
| " | quote | 按当前界面左右对半分，向下分屏 | 同上。 |
| ! | FULL! | 将当前界面调整到全屏 | |
| c | create | 创建新窗口（相当于标签页） | 自动切到新窗口 |
| n / p | next / previous | 切换下一个 / 上一个窗口 | n=next p=previous |
| 箭头键 | arrows | 切换分屏窗口 | 当前窗口会被绿色围绕。 |
| x | x-kill | 关闭当前分屏 | 不影响其他面板 |
| z | zoom | 放大或还原当前分屏 | 类似“最大化”效果 |
| w | windows | 显示所有窗口列表 | 可以用方向键选择 |
| , | rename | 重命名当前窗口 | |
| d | detach | 脱离会话（保留后台运行） | 可用 `tmux attach` 返回 |
| [ | scroll / copy mode | 进入复制模式 | 默认复制到tmux会话内自己的buffer。 |
| ] | paste | 粘贴复制缓冲区内容 | |

### 3.2 窗口内快捷键
| 快捷键 | 速记 | 功能 | 备注 |
| ------- | ------- | ------- | ------- |
| Ctrl+D | Delete | 关闭当前窗口 | 不会影响其他窗口 |
除该快捷键外，其他用法均和正常Linux shell相同。

## 4. tmux会话管理
分离操作（将当前tmux会话和当前shell解绑）：
```bash
tmux detach
```
被解绑后的操作可被重新绑定，在回退到shell后，可使用`tmux ls`列出所有会话。<br>

重新将shell和tmux会话绑定：
```bash
tmux attach -t <session_name>
```

列出所有会话：
```bash
tmux ls
```

手动杀死会话：
```bash
tmux kill-session -t <session_name>
```
手动杀死会话的命令请在shell中使用。

## END OF FILE
