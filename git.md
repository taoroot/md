# Git
## 走ss代理
git config --global http.proxy 'socks5://127.0.0.1:1080' 
git config --global https.proxy 'socks5://127.0.0.1:1080'

## 
cd existing_folder
git init
git remote add origin http://taoroot@git.foreseer.com.cn/taoroot/covers_mina.git
git add .
git checkout -b dev
git push --set-upstream origin dev
