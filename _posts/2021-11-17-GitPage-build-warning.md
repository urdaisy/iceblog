---
layout: post
title:  Page build warning
subtitle: bug
date: 2021-11-17
tags: [Github]
comments: false
---

#### 每次push都发送一封邮件，强迫症表示很蓝瘦;(
```
The page build completed successfully, but returned the following warning for the `main` branch:

The custom domain `#填上自己的域名（如果有申请）` is not properly formatted. See https://docs.github.com/articles/troubleshooting-custom-domains/#github-repository-setup-errors for more information.
```
```
Warning: We strongly recommend not using wildcard DNS records, such as *.example.com. A wildcard DNS record will allow anyone to host a GitHub Pages site at one of your subdomains.
```
#### 参考List：
#### 1. GitPage原文档配置个人域名
##### https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages
#### 2. StackOverflow问答
##### https://stackoverflow.com/questions/9082499/custom-domain-for-github-project-pages

###  配置过程：
#### 1. 购买域名18元/年；
#### 2. 查看Github Pages在国外的IP地址；
```
ping xxx.github.io
```
#### 3. 修改DNS，参考链接：
#### https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages
#### 4. 确认DNS记录是否正确
```
$ dig WWW.EXAMPLE.COM +nostats +nocomments +nocmd
    > ;WWW.EXAMPLE.COM.                     IN      A
    > WWW.EXAMPLE.COM.              3592    IN      CNAME   YOUR-USERNAME.github.io.
    > YOUR-USERNAME.github.io.      43192   IN      CNAME    GITHUB-PAGES-SERVER .
    >  GITHUB-PAGES-SERVER .         22      IN      A       192.0.2.1
```
#### 5. 测试
```
dig NS WWW.EXAMPLE.COM
```