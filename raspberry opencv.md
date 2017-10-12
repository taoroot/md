## 在树莓派上安装 opencv

###  开发工具包

    sudo apt-get install build-essential cmake pkg-config

### 图像库(解码JPEG、PNG等格式)

    sudo apt-get install libjpeg8-dev libtiff4-dev libjasper-dev libpng12-dev

### GTK开发库(opencv的hughgui的依赖库)
    
    sudo apt-get install libgtk2.0-dev

### 视频库(opencv加载视频)

    sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev

### opencv优化库
 
    sudo apt-get install libatlas-base-dev gfortran

### python2.7 安装
    
    sudo apt-get install python2.7-dev

### 下载opencv源代码

    mkdir ~/opencv && cd ~/opencv
    wget https://github.com/opencv/opencv/archive/3.2.0.zip
    unzip 3.2.0.zip
    cd opencv-3.2.0
    mkdir release
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_NEW_PYTHON_SUPPORT=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON ..
    make -j4 && sudo make install

### 测试opencv是否安装完成

代码

```cpp
    #include <iostream>
    #include <opencv2/core/core.hpp>
    using namespace cv;
    using namespace std;
    int main(int argc, const char * argv[])
    {
        Mat M(2, 2, CV_8UC3, Scalar(0, 0, 255));
        cout << M << endl;
        return 0;
    }
```   

配置环境

```shell
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib/

    g++ -w `pkg-config --cflags --libs opencv` test.cpp

    ./a.out
```
