--- 
title: 矩阵掩码的操作
tags:  ["opencv"]
notebook: Knowledge 
---

## 矩阵掩码的操作
	
```cpp
	//  main.cpp
	//  opencv-test
	//  矩阵掩码的操作
	//  增加对比度
	//  通过对中心点周围的像素点设置不同权值，凸显出中心的色彩
	//  使用格式：./a.out x1.jpg 
	//  Created by taoroot on 17/3/26.
	//  Copyright © 2017年 taoroot. All rights reserved.
	//

	#include <iostream>
	#include <opencv2/core/core.hpp>
	#include <opencv2/imgproc.hpp>
	#include <opencv2/highgui/highgui.hpp>
	using namespace std;
	using namespace cv;


	/*                           i\j
	                                |-----------|
	                            -1  | 0  -1   0 |
	   I(i,j) * M, where M =     0  |-1   5  -1 |
	                            +1  | 0  -1   0 |
	                                |-----------|
	 */

	void harpen(const Mat& myImages, Mat& Result);

	int main(int argc, const char * argv[])
	{
	    if(argc < 1)
	    {
	        cout << "Not enough parameters" << endl;
	        return -1;
	    }
	    
	//  存储前后对比图
	    Mat I,J,K;
	//    可选择参数判断
	    if(argc == 3 && !strcmp(argv[2],"G"))
	    {
	        I = imread(argv[1],CV_LOAD_IMAGE_GRAYSCALE);
	    }
	    else
	    {
	        I = imread(argv[1],CV_LOAD_IMAGE_COLOR);
	        cout << "No Optional parameters." << endl;
	    }
	//    判断图片是否被加载
	    if(!I.data)
	    {
	        cout << "The image " << argv[1] << " could not be loaded." << endl;
	        return -1;
	    }
	    else
	    {
	        cout << "The image " << argv[1] << " has loaded." << endl;
	    }
	    
	    cout << "The original image load." << endl;
	    imshow("original", I);
	    double t;
	    int times = 1000;
	    /* 通过自定义函数实现对比图 */
	    t = (double)getTickCount();
	    for(int i = 0; i < times; i++)
	    {
	        harpen(I, J);
	    }
	    t = 1000*((double)getTickCount() - t) / getTickFrequency() / times; //(计数差/频率/times = 运行一次所需的时间);
	    cout << "Time of reducing                      " << times << " runs: " << t << " millisecode." << endl;
	    imshow("reduced1", J);
	    
	    /* 通过内部函数实现对比图  */
	    t = (double)getTickCount();
	    Mat kern = (Mat_<char>(3,3) << 0, -1,  0,
	                                  -1,  5, -1,
	                                   0, -1,  0);
	    for(int i = 0; i < times; i++)
	    {
	        filter2D(I, K, I.depth(), kern);
	    }
	    t = 1000*((double)getTickCount() - t) / getTickFrequency() / times; //(计数差/频率/times = 运行一次所需的时间);
	    cout << "Time of reducing                      " << times << " runs: " << t << " millisecode." << endl;
	    
	    cout << "The reduced image load." << endl;
	    imshow("reduced2", K);
	    waitKey(0);
	    return 0;
	}

	void harpen(const Mat& myImages, Mat& Result)
	{
	    //只接受uchar图像
	    CV_Assert(myImages.depth() == CV_8U);
	    Result.create(myImages.size(),myImages.type());
	    const int nChannels = myImages.channels();
	    for(int j = 1; j < myImages.rows-1; j++)
	    {
	        const uchar* previous   = myImages.ptr<uchar>(j - 1);
	        const uchar* current    = myImages.ptr<uchar>(j);
	        const uchar* next       = myImages.ptr<uchar>(j + 1);
	        
	        uchar * output = Result.ptr<uchar>(j);
	        for (int i = nChannels; i < nChannels*(myImages.cols-1); ++i) {
	            *output++ = saturate_cast<uchar>(5*current[i] - current[i-nChannels] - current[i+nChannels] - previous[i] - next[i]);
	        }
	    }
	//    最外围的一行像素点没有设置其像素
	    Result.row(0).setTo(Scalar(0));                 //顶端
	    Result.row(Result.rows-1).setTo(Scalar(0));     //底端
	    Result.col(0).setTo(Scalar(0));                 //左端
	    Result.col(Result.cols-1).setTo(Scalar(0));     //右端
	    
	}
```




