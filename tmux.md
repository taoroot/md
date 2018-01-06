# tmux
# Centos 6.9 安装
## 安装文档
https://github.com/tmux/tmux

## 依赖 libevent 2.x. 下载
```
wget https://github.com/libevent/libevent/releases/download/release-2.0.22-stable/libevent-2.0.22-stable.tar.gz
# 解压
tar -xvf libevent-2.0.22-stable.tar.gz
# 编译-安装
./configure && make
sudo make install
```

## 依赖 ncurses-devel 下载
```
yum install ncurses-devel -y
```

## 编译工具
```
yum install gcc automake
```

## 遇到问题: libevent-2.0.so.5: cannot open shared object file: No such file or directory
```
ln -s /usr/local/lib/libevent-2.0.so.5 /usr/lib/libevent-2.0.so.5 # 32位系统
ln -s /usr/local/lib/libevent-2.0.so.5 /usr/lib64/libevent-2.0.so.5 # 64位系统
```


## 解决tmux下vim颜色异常
```
alias tmux='tmux -2',source ~/.bashrc. -> ~/.bashrc  
set -g default-terminal "screen-256color" -> ~/.tmux.conf
source ~/.bashrc 
# 重启 tmux
```

