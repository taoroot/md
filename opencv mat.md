--- 
title: Mat 的使用
tags:  ["opencv"]
notebook: Knowledge 
---

## Mat 的使用

```cpp
    //  
    //  main.cpp
    //  opencv-test
    //
    //  Created by taoroot on 17/3/26.
    //  Copyright © 2017年 taoroot. All rights reserved.
    //
    #include <iostream>
    #include <opencv2/core/core.hpp>
    using namespace std;
    using namespace cv;


    int main(int argc, const char * argv[]) {

    //    创建一个存储2*2的rgb像素矩阵，
    //    每个矩阵坐标值采用无符号的char类型存储，
    //    每个矩阵初始化颜色为（0，0，255）。
    Mat M(2, 2,CV_8UC3, Scalar(0,0,255));
    //    输出该矩阵
    //    cout << M << endl;

    //    定义一个三维像素点
    Point3f p(5,1,1);
    //    cout << p << endl;

    //    利用verctor，定义一条直线
    //    像素点是二维的，每个像素点用坐标值用float存储值
    vector<Point2f> vpoints(20);
    for(size_t E = 0; E < vpoints.size(); E++)
    vpoints[E] = Point2f((float)1,(float)1);
    cout << vpoints << endl;

    return 0;
    }
```
