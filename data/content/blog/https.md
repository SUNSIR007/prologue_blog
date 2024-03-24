---
title: 为网站配置证书（ssl/tls）
tags: ["Web"]
image: /static/images/https.webp
pubDate: 2024-3-24
featured: false
---

## 第一步：安装 web 服务器 nginx（可选）

Debian 12:

```shell
sudo apt-get update
sudo apt-get install nginx
```

安装完成后，nginx 就默认已经在运行了

可通过以下命令来检查 nginx 运行状态：

```shell
systemctl status nginx
```

## 第二步：登录[Let's Encrypt](https://letsencrypt.org/zh-cn/)

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240324/截屏2024-03-24-18.47.26.1jbqvrafn4yo.png" alt="截屏2024-03-24-18" />

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240324/截屏2024-03-24-18.53.07.1g6t3u16arog.png" alt="截屏2024-03-24-18" />

<img src="https://cdn.jsdelivr.net/gh/SUNSIR007/picx-images-hosting@master/20240324/截屏2024-03-24-18.54.03.3nx8bl7mzey0.png" alt="截屏2024-03-24-18" />

选择你的 web 服务器和系统

## 第三步：安装 snapd

```shell
sudo apt update
sudo apt install snapd
```

## 第四步：安装 Certbot

若之前有安装过 certbot 则需要先清理干净

```shell
sudo apt remove certbot
sudo apt purge certbot
sudo apt autoremove
```

安装 certbot：

```shell
sudo snap install --classic certbot
```

建立索引：

```shell
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

执行此条命令后须退出终端重新连接来进行刷新

## 第五步：安装证书

```shell
sudo certbot --nginx
```

这条命令会自动配置 nginx 的配置

## 第六步：自动续订

```shell
sudo certbot renew --dry-run
```

END
