---
title: 服务端部署jenkins
description: 
published: true
date: 2022-06-20T02:48:27.477Z
tags: 服务端, jenkins
editor: markdown
dateCreated: 2022-06-20T02:48:27.477Z
---

# 服务端部署jenkins
1.安装tomcat及相关组件

`apt-get install tomcat9 tomcat9-docs tomcat9-examples tomcat9-admin -y`

`service tomcat9 start`

2.配置管理员权限

`sudo vim /var/lib/tomcat9/conf/tomcat-users.xml`

```
<role rolename="manager-gui"/>

<role rolename="admin-gui"/>

<user username="root" password="123456" roles="manager-gui,admin-gui"/>
```

3.重启服务

`service tomcat9 restart`

4.下载jenkins官方软件仓库密钥

`wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key`

5.添加密钥到本地

`apt-key add jenkins.io.key`

6.添加源

> deb https://pkg.jenkins.io/debian-stable binary/
{.is-info}


7.更新并安装jenkins

`apt-get update`

`apt-get install jenkins`

8.启动jenkins

`/etc/init.d/jenkins start`

9.打开浏览器输入地址，端口可能会被tomcat占用，进入配置文件修改端口并重启服务

```
vim /etc/default/jenkins

HTTP_PORT=8085
```
`sudo /etc/init.d/jenkins restart`

10.首次进入jenkins如要输入密码，密码位置：/var/lib/jenkins/secrets/initialAdminPassword

等待提示点击“Install suggested plugins”