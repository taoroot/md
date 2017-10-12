--- 
title: alpha 叠加两张图片
tags:  ["opencv"]
notebook: Knowledge 
---

## alpha 叠加两张图片


```cpp
	//  main.cpp
	//  opencv-test
	//
	//  线性混合
	//  将两张图片叠加在一起

	//  使用格式：./a.out x1.jpg x2.jpg alpha-value
	//  注意：这里的两张图片都得相同的大小的图片才可以。

	//  Created by taoroot on 17/3/26.
	//  Copyright © 2017年 taoroot. All rights reserved.
	//

	#include <iostream>
	#include <opencv2/core/core.hpp>
	#include <opencv2/imgproc.hpp>
	#include <opencv2/highgui/highgui.hpp>
	using namespace std;
	using namespace cv;


	/*
	    g(x) = (1 - a)f0(x) + af1(x)
	 */

	void harpen(const Mat& myImages, Mat& Result);

	int main(int argc, const char * argv[])
	{
	    if(argc < 4)
	    {
	        cout << "Not enough parameters" << endl;
	        return -1;
	    }
	//    存储图片1、图片2、最后合成图
	    Mat I,J,K;
	    
	    I = imread(argv[1],CV_LOAD_IMAGE_COLOR);        //图片1
	    J = imread(argv[2],CV_LOAD_IMAGE_COLOR);        //图片2
	    


	//    判断图片是否被加载
	    if(!I.data)
	    {
	        cout << "The image1 " << argv[1] << " could not be loaded." << endl;
	        return -1;
	    }
	    else
	        cout << "The image1 " << argv[1] << " has loaded." << endl;
	    if(!J.data)
	    {
	        cout << "The image2 " << argv[2] << " could not be loaded." << endl;
	        return -1;
	    }
	    else
	        cout << "The image2 " << argv[2] << " has loaded." << endl;
	    
	//    获取alpha值
	    double alpha;
	    stringstream s;
	    s << argv[3];
	    s >> alpha;
	    if(!s || alpha < 0 || alpha > 1)
	    {
	        cout << "Invaild alpha number " << endl;
	        return -1;
	    }
	    else
	        cout << "The alpha " << alpha << " has loaded." << endl;
	        
	//    创建窗口
	    namedWindow("Linear Blend", 1);
	//    合成图片
	    addWeighted(I, alpha, J, 1-alpha, 0.0, K);
	//    显示图片
	    imshow("Linear Blend", K);
	    waitKey(0);
	    return 0;
	}
```





