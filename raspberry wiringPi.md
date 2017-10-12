### wiringPi简介

    WiringPi是应用于树莓派平台的GPIO控制库函数，WiringPi遵守GUN Lv3。
    wiringPi使用C或者C++开发并且可以被其他语言包转，例如python、ruby或者PHP等。
    WiringPi中的函数类似于Arduino的wiring系统，这使得熟悉arduino的用户使用wringPi更为方便。
    树莓派具有26个普通输入和输出引脚。在这26个引脚中具有8个普通输入和输出管脚，这8个引脚既可以作为输入管脚也可以作为输出管脚。
    除此之外，树莓派还有一个2线形式的I2C、一个4线形式的SPI和一个UART接口。
    树莓派上的I2C和SPI接口也可以作为普通端口使用。
    如果串口控制台被关闭便可以使用树莓派上的UART功能。
    如果不使用I2C，SPI和UART等复用接口，那么树莓派总共具有8+2+5+2 =17个普通IO。
    wiringPi包括一套gpio控制命令，使用gpio命令可以控制树莓派GPIO管脚。
    用户可以利用gpio命令通过shell脚本控制或查询GPIO管脚。
    wiringPi是可以扩展的，可以利用wiringPi的内部模块扩展模拟量输入芯片，可以使用MCP23x17/MCP23x08（I2C 或者SPI）扩展GPIO接口。
    另外可通过树莓派上的串口和Atmega（例如arduino等）扩展更多的GPIO功能。
    另外，用户可以自己编写扩展模块并把自定义的扩展模块集成到wiringPi中。
    WiringPi支持模拟量的读取和设置功能，不过在树莓派上并没有模拟量设备。
    但是使用WiringPi中的软件模块却可以轻松地应用AD或DA芯片。

### 安装wiringPi 

    git clone git://git.drogon.net/wiringPi
    cd wiringPi 
    sudo ./build

### 测试wiringPi安装
    
    gpio -v
    gpio -readall

### 测试代码

```cpp
    #include <wiringPi.h>  
    int main(void)  
    {  
        wiringPiSetup();  
        pinMode (0, OUTPUT) ;  
        for(;;)   
        {  
             digitalWrite(0, HIGH) ; 
             delay (500) ;  
             digitalWrite(0,  LOW) ; 
             delay (500) ;  
        }      
    }
```

### 编译&&运行

    gcc -Wall -o test test.c -lwiringPi
    ./test

