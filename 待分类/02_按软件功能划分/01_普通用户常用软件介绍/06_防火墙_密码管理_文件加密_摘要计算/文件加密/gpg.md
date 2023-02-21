---
title: GPG
description: 
published: true
date: 2022-10-25T02:45:12.035Z
tags: 
editor: markdown
dateCreated: 2022-07-18T06:36:14.323Z
---

# GPG 介绍 

提到 `GPG` 不得不提一下 `PGP(Pretty Good Privacy)` ， `PGP` 最开始是由 `Phil Zimmermann` 开发，开发的目的是为了躲避监视，如果文件在网络上明文传输，那是多么危险。`PGP` 虽然受很多人喜爱，但是是个商业软件，不能自由使用。所以自由基金会决定自己开发一个取名叫 `GPG` ，这就是 `GPG` 的由来。`GPG` 和 `PGP` 都遵循 `OpenPGP` 加解密标准，可以相互兼容。现在的 `PGP` 被赛门铁克公司收购了。

对于我们普通用户来说，平时登录各个网站都会用到密码，如果这些网站的密码都用一个，那风险就很高，如果每个网站都不一样，那也记不住。通常的做法是把这些密码存到一个文件里，但是这个文件本身的安全我们没法保证，所以我们需要对这个文件进行加密。这就要用到我们今天的主角 `GPG` ，这样就算被人拿到了这个文件也看不懂。

## 安装

安装就非常简单，没什么好说的，执行命令就完成了。

### Mac

```bash
brew install gpg
```

### Arch / Manjaro

```bash
sudo pacman -S gnupg
```

### Debian

``````bash
sudo apt install gnupg
``````



## 生成密钥

在使用 `GPG` 之前，先要生成加密的公私钥。`GPG` 是支持对称加密的，如果你只需要使用对称加密，也就没有必要生成密钥了。

现在我们通过 `gpg --full-gen-key` 来生成密钥

```bash
➜  ~ gpg --full-gen-key
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
(1) RSA and RSA (default)
(2) DSA and Elgamal
(3) DSA (sign only)
(4) RSA (sign only)
(14) Existing key from card
# 选择要使用的加密算法，加解密都使用 RSA
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
# 选择加密算法的长度，默认是2048，我选了最长的 4096
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
0 = key does not expire
<n>  = key expires in n days
<n>w = key expires in n weeks
<n>m = key expires in n months
<n>y = key expires in n years
# 如果是自己使用的话，可以选择永不过期
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

# 输入用户名，不能少于 5 个字符
Real name: test1
# 输入邮箱
Email address: test@test.com
# comment 可以不写
Comment: test
You selected this USER-ID:
"test1 (test) <test@test.com>"

# 如果确认无误，输入 O
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
                   disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
                   disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key 0E827A8AE76FFEB0 marked as ultimately trusted
gpg: revocation certificate stored as '/home/test/.gnupg/openpgp-revocs.d/32BB424E02712F45B9E1A1CA0E827A8AE76FFEB0.rev'
public and secret key created and signed.

pub   rsa4096 2020-01-05 [SC]
32BB424E02712F45B9E1A1CA0E827A8AE76FFEB0
uid                      test1 (test) <test@test.com>
sub   rsa4096 2020-01-05 [E]
```

通过上面的操作，生成了一个名为 `test1` ，使用 `RAS` 加密算法，并且永不过期的账户。`32BB424E02712F45B9E1A1CA0E827A8AE76FFEB0` 是账户的 ID，可以用来表示账户。

你如果不想选择加密算法和长度信息，可以使用 `gpg --gen-key` 来快速生成。这样生成的默认算法是 `RAS` ，有效时间是一年， comment 为空。

## 生成撤销密钥

生成完密钥之后，为了保险起见，我们需要生成一个撤销证书，以便在日后我们不使用这个密钥的时候可以撤销。

这个撤销只有你把公钥上传到公钥服务器才有效，如果你不上传到公钥服务器其实没多大作用。即便申请撤销了，它还是会存在，只是不建议使用了。

生成撤销的命令如下

```bash
➜  ~ gpg --gen-revoke test1
```

## 加密

有了密钥之后，我们来看看怎么用它来加密一个文件

通过 `gpg -r [uid] -o file -e file` 来加密一个文件，其中 `-r` 指定用哪个用户加密， `-o` 表示加密后输出的文件， `-e` 表示要加密的文件。

如果不用 `-o` 来指定输出的文件，默认会在原来的文件后面添加 `.gpg` 的后缀，用来表示加密文件。

举个例子：假设我们有一个 `test.txt` 的文件，内容如下

```bash
This is gpg test
```

我们对其进行加密

```bash
➜  ~ gpg -r test1 -e test.txt# 可以使用 gpg -r test1 -o test.txt.gpg -e test.txt 效果是一样的
```

执行完成后会得到一个名为 `test.txt.gpg` 的文件，这个文件是一个二进制文件。

如果我们想要显示 ASCII ，可以通过添加 `-a` 来显示，这样会得到 `test.txt.asc` 的文件，后缀和二进制的不一样。

```bash
➜  ~ gpg -r test1 -a -e test.txt
➜  ~ cat test.txt.asc
-----BEGIN PGP MESSAGE-----

hQIMAwICWpdGn8f1ARAAhnUD+TEv4siDsS5jO/w9eO6pmhxJU1uSobVc4O4gzVs1
9tNT+3cb4VjEjhPokVyx0US4wQfwYQwc6H4jP9I3w5GhdH8kC8bwMcpVIRkryjj1
J/7WftBPW7sbZJYVzS2mt0qDGb6wxosiVWsAbnCR9ofBzZD5L1jfp9iJTpLzAXy3
yVVwI3XKuOBmlj/fA/kbetECgZQzEVkNYu9p35jYHs4qz6U8OKVOLS1O4h5jpLYp
oVSK0uGeg5fMymNmu0A1WPYIQ0Eor2E8MBVNc+vR3jKfn5cBT5vA10w1j9AHVNSX
MndRh5KhRmutx0TTx6nxPzRlSaFt6+XhXTBwaZ32cnj6H0AT0nFlMzT0X3GoXqET
7ts9cc7cwFNWBJk3iiNCohTLYMHoaxZok8eQzkMO9WXsddaje9WfjE/VisuqxscS
WJvFx7rFZUZew5xNdPI8gyBh9gEgIljcnofGAWB5Pts2CXb6+JCfd5vvRGvjr5CQ
ze0Qpa012Y982JmCy1eFtl6t3QFpb6yIWDK5B6oFyfv9yW41kqON7CNstvizv6t2
1A3pCIqETGwiFNSdpJB7X3KD/yImgeEReREYokpNLIp0TZS2fe029ibYJXLrwFel
HeNwGEaedGZpjmib6WeeddI0S0AQKY6+7OLh2R10Y4maEkduuxsXqfFdxlQIUV3S
UAG6DZHSUVvhOZyQjWxOe098nFir80BXXc9cw39luMOxjw3d4kaID/6xwSD0HJc5
6xgDoK0FLsbAAv4zfgttrNdPbxmOM9eoKMVUyPZJGX1f
=2XkE
-----END PGP MESSAGE-----
```

## 解密

加密完之后，我们来看如何解密

通过 `gpg -o file -d file` 来对文件进行解密，其中 `-o` 表示解密后输出的文件，要放在前面， `-d` 表示要解密的文件。

我们就拿之前加密的文件来进行解密，把解密后的文件保存到 `decrypt.txt` 中。

```bash
# 解密二进制文件
➜  ~ gpg -o decrypt.txt -d test.txt.gpg
➜  ~ cat decrypt.txt
This is gpg test
# 解密 ASCII 文件
➜  ~ gpg -o decrypt.txt -d test.txt.asc
➜  ~ cat decrypt.txt
This is gpg test
```

可以看到解密前后内容是一样的，并且二进制和 ASCII 解密出来是一样的。

## 签名

`GPG` 除了可以用来加密，还可以用来签名，比如在 `Github` 上可以添加公钥来表示这个代码确实是你提交的，而不是同名的其他人提交的。

签名的作用就是用来表示这个文件是你写的，而不是别人写的，不然别人可以随便冒充你，有了这个签名别人就没办法冒充你了。

### 二进制签名

使用 `gpg --sign test.txt` 对 `test.txt` 进行签名，生成 `test.txt.gpg` ，里面的内容是二进制。

### ASCII 签名

除了二进制的方式还可以输出 ASCII 的签名，用 `gpg --clearsign test.txt` 对 `test.txt` 进行 ASCII 签名

```bash
➜  ~ gpg --clearsign test.txt
➜  ~ cat test.txt.asc
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

This is gpg test
-----BEGIN PGP SIGNATURE-----

iQIzBAEBCAAdFiEEiPHq0DgY+8QbgkHmOxWoUusKglEFAl41jDIACgkQOxWoUusK
glFEKhAAqwzhd4lexf1KyMTuCd5jAftPGB6AC1igmUx0gjpnMoZHv7gJkfFr8jTa
iD958jXVY4fawsBQ+LEEkpJarBdZfY2No5Dw1PSlRlaUzCQgMkOPgLTdqGHuTNB0
/ENsIiweXv5kUXiSU1Paxedwi7yux5L34WWdi9Mg9KoLhJN84HEdzIKYZjJNyFtB
WYP/Nqqqs01SBa3writrdVaj/ZaDqMtpzM9DSxfMT4P6NVDR3RGSTkQpnatEEa67
11i+oQXQcQT6a+O1vZ8GwbfoQ/EmPGT4nURs1HSEbAXj2aInquU5UjIvkvDp3q2x
j9UbDBcQaQWQdAsuJItIfLeyY1BhejqVIjhPPFMhplp5XrHMMR+JJXOUJleQkkZV
q35RitRlnRCjm+Z26aACS1vSS4Y23jaNV+MWv23OF570+iQ1lVotYLeLeMbuW4s9
oWQIqJydaePbL87wR3tTWm7OjlgZNrrSXh9aH366Oz5tjZVrZoJwNDupOj5uAgsf
T58N0St0OlrPO2csUQw5Kkd7Ls1lKCkh+drPQEPHJJODzNo4WxjtPGwgwhIhLJcw
o1TQxd3h5YhVInCs0XFzSx3Ty/Ds0Lm0zFa4AyINl0QiHkQy2OoD4rsFO7n6LdOk
zJKrFEkSwuvTOeIBUOGulwZiZEFrxb9KpaCjhMLZgQfjSYAnyQ8=
=lmwE
-----END PGP SIGNATURE-----
```

这种方式会在源文件末尾加上签名信息。

### 签名与加密

我们除了进行签名，也支持在进行签名的同时进行加密。

`gpg -u test1 -r test1 -se test.txt` 其中 `-u` 表示 `--local-user` 也就是用谁来签名， `-r` 同前面一样表示用谁来加密。

为什么签名和加密不是同一个人呢？其实很好理解，我们签名表示的是这份文件确实是我写的，而加密要用对方的公钥来加密，这样对方才能解密。

```bash

➜  ~ gpg -u test1 -r test1 -se -a test.txt
➜  ~ cat test.txt.asc
-----BEGIN PGP MESSAGE-----

hQIMAwICWpdGn8f1ARAAtcowWglddI5V75WYb/xLCzYPVDc5GNHqny+Gl7W6WRWd
504Ks0b70L2NNjFcX+bEqiHv2oT+UkPKrD+7B3nl7DbIikMkCCPyl6keD9DS6oQa
UWyh8qkzPn3isuVpvRts4mL1n+q0NLx4G5uhqFkxKjavoYPS1dyCv3iQ6gV4+Nkn
ZEtpiDYK5fv71CQSae5jhN1z45xV58g+oyFWQkVkouyeP8U5hUEjZKeoxq2krrL+
7/VtPAQ2Q3hZ5/s7BxJi9yvU4rSG/APjJ+1ukczSEJ2IJdD71W0x7TglUbuhSmq0
AQlCLg6Ph1mLKBY/1PeQFnwhzaGAG3h5rjN7Y9MJogh4RdBQKxa8/WiAnH3AKciZ
IE4r7bLh03mR+lX4R7ZBt+lEqr36G5TjjO6qoAyuPs9Vf0kXG4EcAU5LPXbS5eDl
MXLvjUBraWWVuiTB4ULfLGnJXfLNBzVQt7fdD617jJD44uPIKDe6z6mDtwZhNQFv
ViYoRaCq33MmzsDPTmruoMTPrHcyD7Rua10d66AQ2PwpajjHyWMSmMlifVStsgLY
8RH0vwAJ2ZneNYIxRAPUqMX6YZs9YMC0FBtR1TV5MT4jnmlPcn/KfrQjHu5EcBVy
vj4ziWT4ZHqszqK7r2PP7LZBQ9PBXL497VNmJyvcc/B4YeAVk8a9RjiE8c+iqqfS
6QFDW9TIwNd0K0DuNA0DjfJTkV1Zg0PLNmaaJPUEpyvlvrf5BjbXPnho6FrSU4Jt
GuspawIYSk+CSF8y+LM6nvi15T9GHt610KcQ3s280NWD4WfLHin9WWSw3IdOXIfG
KINoC8dlp+6XHnru4rUMsO6jEiQ7Seoc4sBRZF7BB+YUy1tmYwM5sMQoY+PfQDTs
Pw0sQkP2RSO3XDGrAUIvM2Ub185voPM0J6r91xqytWHVJn9hSwYaWELLQ9NEACKd
BDJBxuEToryNmIRMJ8ufjH8EFo1UU2X0sS8TRPbYsyh/4cD12ejkePjyQSpaQp8y
La4eG0+amxvbucwBO9inj3nIV8sNrYFw5Vz9uvIFLZIEvPQFFI6ziEbidQhhYi4H
u1xZ2I6lR/697GRpWA2Dy1q5BRcHBQAjpS5r4STCR2b0FKMlGCDzfDAoH06mbENK
McvlIdnjSL/+oXPR4AgRDfB6FETS/vxnFXSod4pXu6cKV28TmBIk2/jSgt9Ny1zI
3WmEZyNaabwqGBOk4sRmVIvJcFdwBPDeumJlbfU2MTs+75+qx0nlR1QCq/+G+d3a
D9dkQbuPYScX3p+ihzcDi6+TWT+uvwqRyVL4xDA8e38/QmDRLnvQfCdZX8JM1Xzk
QLiEfACM6BNmLAMBBEEJCEOOdva4DGvSwjz4VfiFY15orRANYfzeOk/4d0BkYScz
YpCqdBd/6iCNRWNTMzYOi4W/UWj0lwPO7zUKwEdYuggzpOmVJ9LpMaeHRUQUN7KH
ASl2ZWXoCZJWbj3YUmJISlTjpXJ9/Rsdiz0ZqQwojn573L1Cas65eiWyiBjlmmCO
SjxOzmSJyX5xlQSNfe8vaxrY0gyh/mJ9mnQtspUFIKvV68lJPy3J2ujFl3RyZNSf
DlE2OtHLOqFKq/U4aJU5
=5rfp
-----END PGP MESSAGE-----
```

### 分离签名文件

之前的方式会把加密和签名放在同一个文件里，如果想要分开需要加上 `-b` 它表示 `detach-sign`

使用 `gpg -b test.txt` 会得到 `test.txt.sig` 这个单独的签名文件

```bash
➜  ~ gpg -b test.txt➜  ~ ls |grep testtest.txttest.txt.sig
```

### 验证签名

对文件加上签名后，我们需要验证这个签名看看对不对，验证也很简单加上 `--verify` 就行

```bash
➜  ~ gpg --verify test.txt.sig
gpg: assuming signed data in 'test.txt'
gpg: Signature made Sat Jan 05 22:54:09 2020 CST
gpg:                using RSA key 32BB424E02712F45B9E1A1CA0E827A8AE76FFEB0
gpg: Good signature from "test1 (test) <test@test.com>" [ultimate]
```



本文来源于 https://devbins.github.io/ ,部分内容有进行删改

### 相关链接
[gpg命令 – 加密工具](https://www.xtuos.com/1434.html)
