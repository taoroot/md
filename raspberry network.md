## 树莓派网络操作

### 设置静态ip
#### 使用 vi 编辑文件，增加下列配置项

    vi /etc/dhcpcd.conf

#### 指定网络接口 (eth0有线,wlan0无线)
    
    interface eth0

#### 指定静态IP，/24表示子网掩码为 255.255.255.0

    static ip_address=192.168.0.110/24

#### 路由器/网关IP地址
    
    static routers=192.168.1.1

#### 手动自定义DNS服务器

    static domain_name_servers=114.114.114.114 (尝试下)
    static domain_name_servers=127.0.0.1

#### 重启网络通信就生效

    sudo systemctl restart networkingt
    

## 无线连接账号密码输入
    
### 方法一:

    sudo vim /etc/wpa_supplicant/wpa_supplicant.conf
    在最底部添加
    network={
        ssid="315.3"
        psk="123.123.9"
        key_mgmt=WPA-PSK
    } 
    这种方法需要重启有效

### 方法二:

    wpa_cli status
    wpa_cli
    scan 
    add_network
    这里跳出来一个数字,代表你的无线网卡
    因为只有一张无线网卡,所以跳出一个数字(我跳出来的是3)
    set_network 3 ssid="313-313"
    set_network 3 psk="passwd"
    enable_network
    quit


## 将树莓派与笔记本有线直连

    树莓派:
    sudo vim /etc/dhcpcd.conf
    在后一行添加
    interface eth0
    static ip_address=192.168.22.2/24
    
    笔记本:
    有线的ip设置为192.168.22.1
    子网掩码设置为255.255.255.0


