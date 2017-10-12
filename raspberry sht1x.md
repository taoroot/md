# 温湿度传感器的使用

## 传感器
1. sht1x 温湿度传感器

## 驱动安装
    sudo pip3 install pi-sht1x

## 获取数据
```python3
# 文件名 sht1x.py
# 这个文件请用Python3运行

from time import sleep
import RPi.GPIO as GPIO
from pi_sht1x import SHT1x

DATA_PIN = 21
SCK_PIN = 23

with SHT1x(DATA_PIN, SCK_PIN, gpio_mode=GPIO.BOARD) as sensor:
    temp = sensor.read_temperature()
    humidity = sensor.read_humidity(temp)
    sensor.calculate_dew_point(temp, humidity)
    print (sensor)
    output = open('/tmp/data_sht1x.txt','w')
    print (sensor, file=output)
#    sleep(2)
```
## 定时启动
    sudo crontab -e
    # 在打开的文件最后面添加如下代码
    # 每隔十秒检查一次
    * * * * * sleep 10; python3 /home/pi/sht1x/sht1x.py
    # 每隔十秒检查一次
    * * * * * sleep 20; sh /home/pi/yeelink/yeelink.sh

## 生成json文件
```python3
#打开数据文件
tfile = open("/tmp/data_sht1x.txt")
#读取文件所有内容
text  = tfile.read()
#关闭文件
tfile.close()
temperaturedata = text.split("\n")[0]
temperaturedata = temperaturedata.split(": ")[1]
temperaturedata = float(temperaturedata[0:4])
res = '{"value":%.1f}' %temperaturedata
# 生成json文件
# print(temperaturedata)
# print(res)
output = open('temper_data.txt', 'w')
output.write(res)
output.close
```
## 上传数据
```shell
sudo python3 /home/pi/yeelink/gettemp.py
curl --request POST --data-binary @temper_data.txt --header "U-ApiKey: f1572cd7dc7fcd2e4452d3d567bbc5a8" http://api.yeelink.net/v1.0/device/356669/sensor/404292/datapoints
echo "yeelink upload finished"

```

