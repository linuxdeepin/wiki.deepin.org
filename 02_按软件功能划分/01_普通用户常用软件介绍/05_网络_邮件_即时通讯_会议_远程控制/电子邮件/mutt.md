---
title: mutt
description: 一款TUI的邮件客户端
published: true
date: 2022-10-21T16:20:52.246Z
tags: mua, mutt, 邮件
editor: markdown
dateCreated: 2022-07-21T09:42:27.597Z
---

# 简述

mutt一款古老而强大的TUI邮件用户代理（MUA），有着非常强大的定制性。neomutt则是fork自mutt并添加了些新功能。

![79074d31-05d2-4219-bbd6-bad0b6a2d7fb.png](/软件/79074d31-05d2-4219-bbd6-bad0b6a2d7fb.png)
# 安装


## mutt
```
    apt install mutt
```

## neomutt
```
    apt install neomutt
```

# 配置

例举的都是尽量简单配置


## 收信


### POP3

POP3是把邮件下载到本地，对邮件操作（如删除）不影响服务器上的邮件
```
    set pop_authenticators = 'user'
    # 完整标准URL格式为 协议://[用户名[:密码]@]邮件服务器域名或ip[:端口]
    set pop_host = 'pops://邮件服务器:995'
    set pop_user = '用户名'
    set pop_pass = '密码'
```

### IMAP

IMAP会同步邮件的状态，对邮件操作（如删除）将会影响到服务器上的邮件
```
    # 完整标准URL格式为 协议://[用户名[:密码]@]邮件服务器域名或ip[:端口]
    set folder = 'imaps://邮件服务器:993'
    set imap_user = '用户名'
    set imap_pass = '密码'
```

## 发信


### SMTP

smtp是发送邮件用的
```
    set smtp_url = 'smtp://邮件服务器:25'
    set smtp_user = '用户名'
    set smtp_pass = '密码'
```

## 多账户

官方手册的建议：应视为上个使用的邮箱不可知，所以要清空当前连接账户，然后再连下一个账号


### 账户钩子
```
    account-hook . 'unset imap_user; unset imap_pass; unset tunnel'
    account-hook imap://host1/ 'set imap_user=me1 imap_pass=foo'
    account-hook imap://host2/ 'set tunnel="ssh host2 /usr/libexec/imapd"'
    account-hook smtp://user@host3/ 'set tunnel="ssh host3 /usr/libexec/smtpd"'
```

### 邮箱和目录钩子
```
    mailboxes imap://user@host1/INBOX
    folder-hook imap://user@host1/ 'set folder=imap://host1/ ; set record=+INBOX/Sent'
    mailboxes imap://user@host2/INBOX
    folder-hook imap://user@host2/ 'set folder=imap://host2/ ; set record=+INBOX/Sent'
```

## 外观

格式化字符

| 代码  | 意义 |
|-----|----|
| %y  | 年  |
| %m  | 月  |
| %d  | 日  |
| %H  | 时  |
| %M  | 分  |
| %S  | 秒  |



### index_format

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">字符串</th>
<th scope="col" class="org-left">意义</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">%a</td>
<td class="org-left">发件人地址</td>
</tr>


<tr>
<td class="org-left">%A</td>
<td class="org-left">回复地址（如果存在；否则：发件人地址）</td>
</tr>


<tr>
<td class="org-left">%b</td>
<td class="org-left">原始邮件文件夹的文件名（类似邮箱）</td>
</tr>


<tr>
<td class="org-left">%B</td>
<td class="org-left">等同%K</td>
</tr>


<tr>
<td class="org-left">%C</td>
<td class="org-left">当前消息编号</td>
</tr>


<tr>
<td class="org-left">%c</td>
<td class="org-left">消息正文中的字符数（字节）（请参阅 formatstrings-size）</td>
</tr>


<tr>
<td class="org-left">%cr</td>
<td class="org-left">原始消息中的字符数（字节），包括标题 (参阅formatstrings-size)</td>
</tr>


<tr>
<td class="org-left">%D</td>
<td class="org-left">使用 date_format 和本地时区来显示日期时间</td>
</tr>


<tr>
<td class="org-left">%d</td>
<td class="org-left">使用 date_format 和发件人时区来显示日期时间</td>
</tr>


<tr>
<td class="org-left">%e</td>
<td class="org-left">线程中的当前消息编号</td>
</tr>


<tr>
<td class="org-left">%E</td>
<td class="org-left">线程中当前消息数量</td>
</tr>


<tr>
<td class="org-left">%F</td>
<td class="org-left">发件人名或收件人名（如果邮件是你发的）</td>
</tr>


<tr>
<td class="org-left">%Fp</td>
<td class="org-left">类似%F，但保持原文。收件人姓名不应用上下文格式</td>
</tr>


<tr>
<td class="org-left">%f</td>
<td class="org-left">发件人（地址+真名），either From: or Return-Path:</td>
</tr>


<tr>
<td class="org-left">%g</td>
<td class="org-left">新闻组名（如果使用NNTP支持编译）</td>
</tr>


<tr>
<td class="org-left">%g</td>
<td class="org-left">消息标签</td>
</tr>


<tr>
<td class="org-left">%Gx</td>
<td class="org-left">单个消息标签</td>
</tr>


<tr>
<td class="org-left">%H</td>
<td class="org-left">此消息的垃圾邮件属性</td>
</tr>


<tr>
<td class="org-left">%I</td>
<td class="org-left">作者的首字母缩写</td>
</tr>


<tr>
<td class="org-left">%i</td>
<td class="org-left">当前消息的消息ID</td>
</tr>


<tr>
<td class="org-left">%J</td>
<td class="org-left">消息标签（如果存在，树展开，并且！=父母的标签）</td>
</tr>


<tr>
<td class="org-left">%K</td>
<td class="org-left">发送信件的列表（如果有的话；否则：空）</td>
</tr>


<tr>
<td class="org-left">%L</td>
<td class="org-left">如果“To:”或“Cc:”标题字段中的地址与用户的“subscribe”命令定义的地址匹配，则会显示“To &lt;list-name&gt;”，否则与%F相同</td>
</tr>


<tr>
<td class="org-left">%l</td>
<td class="org-left">未处理邮件中的行数（可能不适用于 maildir、mh 和 IMAP 文件夹）</td>
</tr>


<tr>
<td class="org-left">%M</td>
<td class="org-left">隐藏邮件的数量，如果线程已折叠</td>
</tr>


<tr>
<td class="org-left">%m</td>
<td class="org-left">邮箱中的邮件总数</td>
</tr>


<tr>
<td class="org-left">%N</td>
<td class="org-left">邮件分数</td>
</tr>


<tr>
<td class="org-left">%n</td>
<td class="org-left">作者的真实姓名（如果丢失，则为地址）</td>
</tr>


<tr>
<td class="org-left">%O</td>
<td class="org-left">NeoMutt以前存储消息的原始保存文件夹：列表名称或收件人名称，如果没有发送到列表</td>
</tr>


<tr>
<td class="org-left">%P</td>
<td class="org-left">内置寻呼机的进度指示器（已显示多少文件）</td>
</tr>


<tr>
<td class="org-left">%q</td>
<td class="org-left">新闻组名（如果使用NNTP支持编译）</td>
</tr>


<tr>
<td class="org-left">%R</td>
<td class="org-left">“Cc:”收件人的逗号分隔列表</td>
</tr>


<tr>
<td class="org-left">%r</td>
<td class="org-left">Comma separated list of "To:" recipients</td>
</tr>


<tr>
<td class="org-left">%S</td>
<td class="org-left">消息的单个字符状态（“N”/“O”/“D”/“d”/“！”/"R"/"*")</td>
</tr>


<tr>
<td class="org-left">%s</td>
<td class="org-left">消息的主题</td>
</tr>


<tr>
<td class="org-left">%T</td>
<td class="org-left">$to<sub>chars字符串中的相应字符</sub></td>
</tr>


<tr>
<td class="org-left">%t</td>
<td class="org-left">"To:" 字段（收件人）</td>
</tr>


<tr>
<td class="org-left">%u</td>
<td class="org-left">作者的用户（登录名）名称</td>
</tr>


<tr>
<td class="org-left">%v</td>
<td class="org-left">作者的名字，或者收件人的名字，如果消息来自你</td>
</tr>


<tr>
<td class="org-left">%W</td>
<td class="org-left">作者组织名称（“组织：”字段）</td>
</tr>


<tr>
<td class="org-left">%x</td>
<td class="org-left">"X-Comment-To:" 字段（如果存在，并在NNTP支持下编译）</td>
</tr>


<tr>
<td class="org-left">%X</td>
<td class="org-left">Number of MIME attachments (please see the " attachments" section for possible speed effects)</td>
</tr>


<tr>
<td class="org-left">%Y</td>
<td class="org-left">"X-Label:" field, if present, and (1) not at part of a thread tree, (2) at the top of a thread, or (3) "X-Label:" is different from Preceding message's "X-Label:"</td>
</tr>


<tr>
<td class="org-left">%y</td>
<td class="org-left">"X-Label:" field, if present</td>
</tr>


<tr>
<td class="org-left">%Z</td>
<td class="org-left">A three character set of message status flags. The first character is new/read/replied flags ("n"<i>"o"</i>"r"<i>"O"</i>"N"). The second is deleted or encryption flags ("D"<i>"d"</i>"S"<i>"P"</i>"s"<i>"K"). The third is either tagged/flagged ("*"</i>"!"), or one of the characters Listed in $to<sub>chars</sub>.</td>
</tr>


<tr>
<td class="org-left">%zc</td>
<td class="org-left">Message crypto flags</td>
</tr>


<tr>
<td class="org-left">%zs</td>
<td class="org-left">Message status flags</td>
</tr>


<tr>
<td class="org-left">%zt</td>
<td class="org-left">Message tag flags</td>
</tr>


<tr>
<td class="org-left">%@name@</td>
<td class="org-left">insert and evaluate format-string from the matching " index-format-hook" command</td>
</tr>


<tr>
<td class="org-left">%{fmt}</td>
<td class="org-left">the date and time of the message is converted to sender's time zone, and "fmt" is expanded by the library function strftime(3); if the first character inside the braces is a bang ("!"), the date is formatted ignoring any locale settings. Note that the sender's time zone might only be available as a numerical offset, so "%Z" behaves like "%z".</td>
</tr>


<tr>
<td class="org-left">%[fmt]</td>
<td class="org-left">the date and time of the message is converted to the local time zone, and "fmt" is expanded by the library function strftime(3); if the first character inside the brackets is a bang ("!"), the date is formatted ignoring any locale settings.</td>
</tr>


<tr>
<td class="org-left">%(fmt)</td>
<td class="org-left">the local date and time when the message was received, and "fmt" is expanded by the library function strftime(3); if the first character inside the parentheses is a bang ("!"), the date is formatted ignoring any locale settings.</td>
</tr>


<tr>
<td class="org-left">%&gt;X</td>
<td class="org-left">right justify the rest of the string and pad with character "X"</td>
</tr>


<tr>
<td class="org-left">%&vert;X</td>
<td class="org-left">pad to the end of the line with character "X"</td>
</tr>


<tr>
<td class="org-left">%*X</td>
<td class="org-left">soft-fill with character "X" as pad</td>
</tr>
</tbody>
</table>


### flag_chars

| 字符  | 默认选项  | 描述                  |
|-----|-------|---------------------|
| 1   | *     | 邮件被标记了。             |
| 2   | !     | 邮件被标记为重要。           |
| 3   | D     | 邮件被标记为删除。           |
| 4   | d     | 邮件中标有要删除的附件。        |
| 5   | r     | 邮件已回复。              |
| 6   | O     | 邮件是旧的（未读但已看到）。      |
| 7   | N     | 邮件为新邮件（未读但未看到）。     |
| 8   | o     | 邮件线程是旧的（未读但已看到）。    |
| 9   | n     | 邮件主题为“新建”（未读但未显示）。  |
| 10  | —     | 邮件已读数 - %S expando。 |
| 11  | <空格>  | 邮件已读取 - %Z expando。 |
