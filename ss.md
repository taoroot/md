# shadowsock 
## 添加 socket5 转 http
```
brew install privoxy
vim /usr/local/etc/privoxy/config
+++ listen-address  0.0.0.0:1081
+++ forward-socks5 / 127.0.0.1:1080 .
```
