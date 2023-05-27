---
title: EarlyOOM
description: 在系统 OOM 之前挽救系统
published: true
date: 2023-05-27T10:55:31.324Z
tags: 内存
editor: markdown
dateCreated: 2023-02-02T06:03:43.384Z
---

# EarlyOOM 让系统再也不会因为内存不足而卡死

许多用户讨厌 Linux 系统在内存耗尽情况下杀进程（oom-killer）的逻辑。大概也是因为这个原因，Linux 只在绝无其他选择时才调用它。系统会把桌面环境swap进虚拟内存、删除一整个分页缓存甚至清空每个缓冲区，最后才对进程下手。我们可没有耐心坐在一个无响应的桌面环境前等它救场。

> 默认情况下剩余可用物理内存小于 10% 和剩余交换内存小于 10% 时，就会触发 earlyoom
{.is-info}

## 安装代码
```bash
# 直接 apt 安装
apt install earlyoom

# 设定为开机启动，并且马上启动 earlyoom 服务
systemctl enable --now earlyoom
```

## 命令行选项

|          选项          |          说明          |
| --------------------- | --------------------- |
| `-m PERCENT[,KILL_PERCENT]` | 至少保留物理内存总量的百分之 *PERCENT*（默认百分之 10）。<br>earlyoom 在内存降至百分之 *PERCENT* 后向最占内存的进程发送 SIGTERM，如进一步降至百分之 *KILL_PERCENT*（默认为 *PERCENT* 的一半）就发送 SIGKILL。 |
| `-s PERCENT[,KILL_PERCENT]` | 至少保留虚拟内存总量的百分之 *PERCENT*（默认百分之 10）。<br>注意：仅当可用的物理内存和虚拟内存都少于设定量时才会触发 earlyoom。 |
| `-M SIZE[,KILL_SIZE]` | 至少保留 *SIZE* KiB 物理内存 |
| `-S SIZE[,KILL_SIZE]` | 至少保留 *SIZE* KiB 虚拟内存 |
| `-n`                  | 启用 d-bus 消息 |
| `-N /PATH/TO/SCRIPT`  | 当有进程因 OOM 被杀后调用脚本 |
| `-g`                  | 终止同一进程组中的所有进程 |
| `-d`                  | 启用调试信息 |
| `-v`                  | 打印版本信息后退出 |
| `-r INTERVAL`         | 每 *INTERVAL* 秒打印一次内存报告（默认为 1），设为 0 即禁用 |
| `-p`                  | 将 earlyoom 的 niceness 设为 -20、oom_score_adj 设为 -100 |
| `--prefer REGEX`      | 尽量优先终止名字与正则表达式 *REGEX* 相匹配的进程 |
| `--avoid REGEX`       | 尽量避免终止名字与正则表达式 *REGEX* 相匹配的进程 |
| `--ignore REGEX`      | 忽略名字与正则表达式 *REGEX* 相匹配的进程 |
| `--dryrun`            | 试运行（不杀任何进程） |
| `-h`, `--help`        | 显示帮助 |



项目地址: https://github.com/rfjakob/earlyoom
参考博客: [CSDN 链接](https://blog.csdn.net/ONE_SIX_MIX/article/details/124046026)
