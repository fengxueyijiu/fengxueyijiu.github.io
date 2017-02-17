---
layout:     post
title:      "nginx服务器搭建https"
subtitle:   "Nginx server to build https"
date:       2017-2-17 12:00:00
author:     "Strom"
header-img: "img/home-bg-art.jpg"
---

#### 阿里云管理后台获取免费的数字证书

首先你得购买一台阿里云服务器，然后登录阿里云服务器管理控制台，进入用户主页，在页面的左侧工具栏中选择【安全(云盾)】下拉菜单，在展开的菜单栏中，选择【证书服务】条目，进入【我的证书】页面，在页面右上角处点击【购买证书】按钮，打开【云盾证书服务】页面，选择【证书类型】为【免费型DV SSL】，其它项无需设置，然后点击右侧的【立即购买】按钮，完成支付之后，还需要填写一些信息。

再次进入【我的证书】页面，这时刚才购买证书的订单已经展示出来了，在订单右侧有一些操作，点击【补全】，绑定域名，然后填写证书申请人信息表单，申请人的邮箱地址一定不能写错。对于域名不是在阿里云注册的用户，阿里云会给你发送邮件，告诉你在你的域名管理系统中添加 CNAME 解析记录。

申请人的个人信息提交之后，然后等待审核，审核通过之后，证书购买订单的右侧会出现一个【下载】操作，进入证书下载页面，不同的 http server 需要不同的数字证书，好奇猫网站使用的是 Nginx 服务器。

#### 配置 Nginx 服务器

把从阿里云获取的免费证书下载到本地，文件解压缩之后，重新命名为 cert，这是一个目录文件，里面包含两个文件，文件后缀分别为 .key 和 .perm。然后把文件 cert 上传到你的服务器上，命令如下：

```
scp -r cert [你的服务器公网ip或域名]:
　例：scp -r cert haoqicat:

```
然后登录到你的服务器，把刚才上传的 cert 目录文件放置到 Nginx 的安装目录下，设置 cert 文件夹的权限：

```
sudo mv ~/cert /etc/nginx
cd /etc/nginx
sudo chown root:root cert
sudo chmod 755 cert

```
然后，设置 cert 目录中每个文件的权限:

```
cd /etc/nginx
sudo chown root:root xxx.key xxx.perm
sudo chmod 644 xxx.key xxx.perm

```
最后，修改好奇猫域名对应的 Nginx 配置文件，添加与 ssl 有关的语句，如下：

```
server {
  listen       443 ssl default_server;
  server_name  haoqicat.com;

  ssl on;
  ssl_certificate   /etc/nginx/cert/xxx.pem;
  ssl_certificate_key  /etc/nginx/cert/xxx.key;
  ssl_session_timeout 5m;
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;
}

```
修改了配置文件之后，需要重新加载一下，修改才能生效：

```
sudo service nginx reload

```
所以工作完成之后，就可以访问 HTTPS 协议的网址了 https://haoqicat.com，说明数字证书已经生效了。不过，这会儿直接在浏览器地址栏中输入 haoqicat.com，网站会报告502错误。

### HTTP 重定向到 HTTPS

直接访问网址 haoqicat.com 使用的是 HTTP 协议，用的是80端口，现在我们的 Nginx 服务器支持的是 HTTPS 协议，监听的是443端口，所以需要把 HTTP 协议的请求重定向到 HTTPS 协议，继续修改好奇猫域名对应的配置，在文件的开头添加下面几行语句：

```
server {
  listen         80;
  server_name    haoqicat.com;
  return         301 https://$server_name$request_uri;
}

```
保存文件，运行下面命令，重新加载一下配置文件：

```
sudo service nginx reload

```
现在，再到浏览器中访问 haoqicat.com，会重定向到 https://haoqicat.com，一切工作正常。
