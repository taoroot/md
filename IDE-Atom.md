# Atom 设置和取消代理
## 设置Shadowsocks代理
```
apm config set http-proxy socks5:127.0.0.1:1080
apm config set https-proxy socks5:127.0.0.1:1080
```
## 取消ssl
```
apm config set strict-ssl false
```
## 取消代理
```
apm config set http-proxy null
apm config set https-proxy null
```
## 查看代理设置
```
apm config get http-proxy
apm config get https-proxy
```
# Sync Settings 同步备份Atom配置
## 安装 Sync Settings
```
apm install Sync Settings
```
## 创建 github personal access token
```
https://github.com/settings/tokens
# 1. 创建tokens
# 2. 勾选权限 Gist
```
## 创建 gist
```
https://gist.github.com/
```
## 在 Sync Settings 配置中添加token, 和 gist ID
 
