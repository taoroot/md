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
## personal access token [创建](https://github.com/settings/tokens)
```
1. 创建tokens
2. 勾选权限 Gist
3. token 不通用,只能和一台设备绑定
```
## gist Id [创建](https://gist.github.com/)
## 配置中添加token, 和 Gist ID
## 使用 Sync Settings
```
Command --> sync
backup 备份
restore 同步
```
