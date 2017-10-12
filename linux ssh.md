# ssh 代理

###客户端
    1.autossh
        功能: 
            使ssh断线重连功能
            将占用服务器9022(sshd)端口
        安装:
            1. 生成开机启动服务
            sudo apt-get install autossh -y
            sudo vim /lib/systemd/system/autossh.service(写完保存代码后再执行下面语句)
            sudo systemctl enable autossh.service 
            sudo systemctl enable systemd-networkd-wait-online.service
            =======================================
            [Unit]
            Description=Auto SSH Tunnel
            After=network-online.target
            [Service]
            User=root
            Type=simple
            Environment=PATH=/bin:/usr/bin:/usr/local/bin
            ExecStart=/bin/bash /home/pi/ChildSafe/Board/Client/ssh_socket.sh
            ExecReload=/bin/kill -HUP $MAINPID
            KillMode=process
            Restart=always
            [Install]
            WantedBy=multi-user.target
            WantedBy=graphical.target
            =======================================

            2. 代理脚本
            vim /home/pi/ChildSafe/Board/Client/ssh_socket.sh
            sudo chmod 777 ssh_socket.sh
            !!!!!!!!每个代理端口先要手动在命令行运行一遍,以为第一次连接的时候需要确认id_rsa,需要手动输入yes
            =======================================
            #!/bin/bash
            # 
            #端口转发
            sleep 5s
            # ssh 代理
            sudo /usr/bin/autossh -M 9023 -C -N -R 9022:localhost:22  root@47.94.80.118 -i /home/pi/.ssh/id_rsa &
            # mjpg-streamer 转发
            sudo /usr/bin/autossh -M 9033 -C -N -R 9032:localhost:2001  root@47.94.80.118 -i /home/pi/.ssh/id_rsa &
            # 开启摄像头
            /bin/sh /root/mjpg_stream/do_mjpg_streamer.sh &
            =======================================

            3. 摄像头驱动问题
            让树莓派摄像头在/dev/下面显示(如果是usb摄像头就不需要这步骤)
            解决方法
            在/etc/moudules或是modules-load.d/xx.conf里面添加 bcm2835-v4l2

            4. mjpg_streamer
            安装mjpg_streamer
            https://github.com/jacksonliam/mjpg-streamer
            网页index.html
            vim /home/pi/ChildSafe/Board/Client/mjpg_stream/index.html
            =======================================
            <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
            "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
            <html xmlns="http://www.w3.org/1999/xhtml">
            <head>
            <title>MJPEG-Streamer</title>
            </head>
            <script type="text/javascript">

            /* Copyright (C) 2007 Richard Atterer, richard©atterer.net
            This program is free software; you can redistribute it and/or modify it
            under the terms of the GNU General Public License, version 2. See the file
            COPYING for details. */

            var imageNr = 0; // Serial number of current image
            var finished = new Array(); // References to img objects which have finished downloading
            var paused = false;

            function createImageLayer() {
            var img = new Image();
            img.style.position = "absolute";
            img.style.zIndex = -1;
            img.onload = imageOnload;
            img.onclick = imageOnclick;
            img.src = "./?action=snapshot&n=" + (++imageNr);
            var webcam = document.getElementById("webcam");
            webcam.insertBefore(img, webcam.firstChild);
            }

            // Two layers are always present (except at the very beginning), to avoid flicker
            function imageOnload() {
            this.style.zIndex = imageNr; // Image finished, bring to front!
            while (1 < finished.length) {
            var del = finished.shift(); // Delete old image(s) from document
            del.parentNode.removeChild(del);
            }
            finished.push(this);
            if (!paused) createImageLayer();
            }

            function imageOnclick() { // Clicking on the image will pause the stream
            paused = !paused;
            if (!paused) createImageLayer();
            }

            </script>
            <body onload="createImageLayer();">

            <div id="webcam">
            <center>
            <noscript>
            <img src="./?action=snapshot" />
            </noscript>
            </center>
            </div>

            </body>
            </html>
            =======================================

            运行脚本
            do_mjpg_streamer.sh
			注意index.html的目录位置
            =======================================
            # sleep 10
            cd /home/pi/ChildSafe/Board/Client/mjpg_stream
            #密码登入
            # mjpg_streamer -i "input_uvc.so" -o "output_http.so -w ./ -p 2001 -c pi:123"
            #无密码登入
            mjpg_streamer -i "input_uvc.so" -o "output_http.so -w ./ -p 2001"
            =======================================
            
            开启自启动服务
            sudo vim /etc/init.d/webcam
			sudo chmod 755 /etc/init.d/webcam
			sudo update-rc.d webcam defaults
            =======================================
            #! /bin/sh
            # /etc/init.d/webcam

            # Carry out specific functions when asked to by the system
            case "$1" in
            start)
            echo "Starting webcam script"
            export LD_LIBRARY_PATH=/home/pi/cam_project/mjpg-streamer-r63
            /bin/sh /root/mjpg_stream/do_mjpg_streamer.sh &
            ;;
            stop)
            echo "Stopping webcam script"
            killall mjpg_streamer
            ;;
            *)
            echo "Usage: /etc/init.d/webcam {start|stop}"
            exit 1
            ;;
            esac
            exit 0
            =======================================


