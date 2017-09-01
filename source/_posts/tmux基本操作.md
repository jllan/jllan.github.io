---
title: tmux基本操作
date: 2017-09-01 23:26:41
categories: IT技术
tags: linux
---
## 会话操作
### 新建会话
`tmux`
`tmux new`
`tmux new -s session-name` # 指定会话名

### 查看目前有开启的会话
`tmux ls`或`ctrl+b s`

### 接入一个已经存在的会话
`tmux ls` #查看当前有哪些会话
`tmux a`  # 接入第一个可用的会话
`tmux a -t session-name` # 接入session-name这个会话

### 临时断开会话
`ctrl+b d`或`tmux detach`

### 关闭会话
`tmux kill-session -t 1`

## 窗口操作
```
ctrl+b c    # 创建新窗口
ctrl+b n    # 下一个窗口
ctrl+b p    # 上一个窗口
ctrl+b l     # 最后一个窗口
```
