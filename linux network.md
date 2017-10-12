# 端口转发

## 服务器端

### 1. 替换防火墙

    #下载iptables替换firewalld
    yum -y install iptables-services
    systemctl stop firewalld.service
    systemctl disable firewalld.service 


### 2. 添加开放端口

    sudo vim /etc/sysconfig/iptables
    # 添加语句
    -A INPUT -p tcp -m state --state NEW -m tcp --dport 2333 -j ACCEPT

### 3. sshd配置修改
    sudo vim /etc/ssh/sshd_config
    # 修改这行(允许端口映射外网访问)
    GatewayPorts yes

### 3. 重启防火墙
    sudo systemctl restart iptables.service
    # 不行的就重启试试

## 客户端
    # 将这条命令放入到开机启动项中

