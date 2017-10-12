### 创建用户

####1. 新建用户组
创建一个用户组,组号为313,组名为matesgroup

    sudo addgroup -g 313 matesgroup

####2. 新建用户名lm

更改默认参数中的用户组(使用组名或组号),将用户组改为313
    
    sudo adduser -g 313 用户名 

添加密码

    sudo passwd lm 

添加SSH KEY

    cat ~/.ssh/id_rsa.pub | ssh root@[your.ip.address.here] "cat >> ~/.ssh/authorized_keys"
