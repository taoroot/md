## 安装文档地址
http://nginx.org/en/linux_packages.html#mainline

Cetnos 6.9 添加源
vim /etc/yum.repos.d/nginx.repo
```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/6/$basearch/
gpgcheck=0
enabled=1
```
