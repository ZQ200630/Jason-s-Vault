---
date created: 2021-11-18 16:45
date updated: 2021-11-18 16:46
tags: Linux Bash
---

# Bash

### Crontab 定期执行程序

```bash
crontab -l
# 显示所有程序
crontab -e
# 编辑执行程序文件

f1 f2 f3 f4 f5 program
分钟 小时 月份中日期  月份  星期中第几天  程序
*表示每隔多少都要执行
a-b表示从a - b执行
*/n 表示没n个时间间隔执行一次
a, b, c 表示第 a, b, c 时间点要执行

*    *    *    *    *
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- 星期中星期几 (0 - 7) (星期天 为0)
|    |    |    +---------- 月份 (1 - 12) 
|    |    +--------------- 一个月中的第几天 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)

every_minute.sh >> /tmp/cron_every_minute.log 2>&1
```

### Alias 别名

```bash
alias alias_name="command_to_alias"

mcd () {
    mkdir -p $1
    cd $1
}
```

### 后台进程

```bash
fg (%n)
#后台进程切换到前台,可以选择切换哪一个进程到前台
bg
#前台进程切换到后台
ctrl+z
[[暂停前台进程，之后可以使用bg]]在后台继续进行
jobs -l
#列出当前所有进程
nohup
#关闭终端依旧执行
&
#在指令后面加&可以让其默认后台运行
```

### 杀死进程

```bash
kill -l
#现实所有信号
kill -STOP PID
[[暂停指定PID]]进程
kill -TERM PID
[[正常停止PID]]进程
kill -KILL PID
#强制杀死进程
kill %n
[[删除第n]]个任务
kill -9 PID
#彻底杀死进程
```

### 显示进程

```bash
ps

# 选项：
a
# 显示所有用户进程，包含每个程序完整路径
-x
# 显示所有系统程序，包含没有终端的程序
-f
# 显示UID，PPIP，C与STIME栏位
-c
# 只显示进程名称，不显示进程完整路径
-e
# 将除了内核进程意外所有进程信息写到标准输出

[[UID]]： 程序被该 UID 所拥有
[[PID]]： 就是这个程序的 ID
[[PPID]] 则是其上级父程序的ID
[[CPU]]： 使用的资源百分比
[[STIME]] ：系统启动时间
[[TTY]]： 登入者的终端机位置
[[TIME]]：使用掉的 CPU 时间。
[[CMD]]： 所下达的指令为何

ps -ef | grep nginx
ps aux | grep nginx
```

^1d322e

### Tmux

```bash
ctrl+b
#命令前缀
ctrl+b ?
[[帮助，按ESC或Q]]退出
tmux new -s name
#新建窗口
tmux detach
#退出当前窗口，但后台程序仍然回继续进行
tmux ls 
[[列出所有Tmux]]会话
tmux attach -t name
#接入某个已经存在但会话
tmux -a
#重新连接最后一个会话
tmux kill-session -t name
#杀死会话
tmux switch -t name 
#切换会话
tmux rename-session -t name newname
#重命名会话

#快捷键
Ctrl+b d: 分离当前会话，此会话会永久小时，如果暂时退出使用tmux attach
Ctrl+b s: 列出所有会话
Ctrl+b w: 列出所有窗口
Ctrl+b $: 重命名当前会话

tmux split-window
#划分上下窗格
tmux split-window -h
#划分左右窗格

tmux select-pane [-UDLR]
#移动光标位置上下左右

tmux swap-pane [-UD]
#交换窗格位置

#快捷键
Ctrl+b %: 划分左右窗格
Ctrl+b ": 划分上下窗格
Ctrl+b <arrow key>: 光标切换
Ctrl+b x: 关闭当前窗格
Ctrl+b !: 将窗口拆分为一个独立窗口
Ctrl+b z: 全屏显示某个窗口，在使用一次变回原来大小
Ctrl+b Ctrl+<arrow key>: 按箭头方向调整窗格大小
Ctrl+b q: 显示窗口编号

tmux new-window -n window-name
#创建新窗口
tmux select-window -t window-name
#切换到指定窗口

tmux rename-window
#重命名窗口

#快捷键
Ctrl+b c: 创建一个新窗口
Ctrl+b p: 切换到上一个窗口
Ctrl+b n: 切换到下一个窗口
Ctrl+b ,: 窗口重命名

tmux list-keys
#列出所有快捷键
tmux list-commands
#列出所有命令及参数
tmux info
#列出会话到信息
tmux source-file ~/.tmux.conf
[[重新加载tmux]]配置
```

## 文本清洗

### awk

```bash
[[awk会读入每一列数据，按照分隔符进行分列，若不需要分割则不需要加-F]]

awk -F' '
# -F后面接分隔符
awk -F':' '{print $1}'
#{}内为一个匿名函数, $1为第一列值, $0为整行
awk -F':' 'BEGIN{}{}END{}'
[[BEGIN]] 与 END 表示在开头和结尾加上一定的字符串

```

### grep

```bash
-E
# 开启扩展正则表达式
-i
# 忽视大小写
-v
# 打印不匹配的内容，invert反向
-n
# 显示行号
-w
# 全单词匹配，不能是某一部分
-c
# 显示有多少行被匹配到了， -cv显示多少行没有被匹配到
-o
# 只显示被匹配到到字符串
-color
# 高亮显示被匹配到到内容
-A n
# 显示匹配到的行及后面n行的内容， after
-B n
# 显示匹配到的行及前面n行的内容， before
-C n
# 显示匹配到的行及前后n行内容

[[:space:]]
# 匹配空白字符
[[:punct:]]
# 匹配所有标点
^
# 开头
$
# 结尾
\<
# 词头
\>
# 词尾
```
