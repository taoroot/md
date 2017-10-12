--- 
title: Mat 迭代器
tags:  ["opencv"]
notebook: Knowledge 
---

## Mat 迭代器
	
```cpp
	//
	//  main.cpp
	//  opencv-test
	//  扫描图像、查表、计时。
	//  本程序功能是将图片进行压缩色彩范围，
	//  算法是讲相近颜色的值归为同一个值，
	//  采用一个数组表table[256],这里存储着原像素值对应的压缩后的数值，
	//  对每个像素值进行表对比，并替换原来的值（这里我们是新建一个图片)
	//  这里采用了三种方式，来实现，分别对应三个方法。

	//  Created by taoroot on 17/3/26.
	//  Copyright © 2017年 taoroot. All rights reserved.
	//

	#include <iostream>
	#include <opencv2/core/core.hpp>
	#include <opencv2/highgui/highgui.hpp>
	using namespace std;
	using namespace cv;


	//  采用C指针遍历
	Mat & ScanImageAndReduce(Mat& I, const uchar * const table);
	//  采用迭代器遍历
	Mat & ScanImageAndIterator(Mat& I, const uchar * const table);
	//  采用相关放回值的On-the-fly地址计算（不推荐）
	Mat & ScanImageAndReduceRandomAccess(Mat& I, const uchar * const table);

	int main(int argc, const char * argv[])
	{
	    /* 下面这一部分是对输入参数的获取和判断有效性 */
	//    argc是参入参数的个数，
	//    调用本程序格式为 ./a.out test.jpg 10 [G]
	//    因为参数G是可选择参数，所以argc只需要>=3就有效
	//    PS：a.out本身是第一个参数,也就是argv[0]
	    if(argc < 2)
	    {
	        cout << "Not enough parameters" << endl;
	        return -1;
	    }

	    Mat I,J;
	//    可选择参数判断
	    if(argc == 4 && !strcmp(argv[3],"G"))
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
	    
	//    判断传入的2个参数是否为整数
	//    刚传入进来的时候是string类型的，需要转化下。
	    int divideWith;
	    stringstream s;
	    s << argv[2];
	    s >> divideWith;
	    if(!s)
	    {
	        cout << "Invaild number entered for dividing. " << endl;
	    }
	    
	    imshow("original", I);
	//    表数组使用
	    uchar table[256];
	//    对表的初始化，根据我们传入的值想图片像素颜色压缩，0-9的压缩为同一种颜色调，以此类推。
	    for(int i = 0; i < 256; i++)
	    {
	        table[i] = divideWith * (i / divideWith);
	    }
	    
	    const int times = 100;
	    double t;
	    Mat Itemp = I.clone();
	    
	    //C指针
	    t = (double)getTickCount();
	    for(int i = 0; i < times; i++)
	    {
	        J = ScanImageAndReduce(Itemp,table);
	    }
	    
	    t = 1000*((double)getTickCount() - t) / getTickFrequency() / times; //(计数差/频率/times = 运行一次所需的时间);
	    cout << "Time of reducing with the c Operator []                     " << times << " runs: " << t << " millisecode." << endl;
	    
	    //迭代器
	    t = (double)getTickCount();
	    for(int i = 0; i < times; i++)
	    {
	        J = ScanImageAndIterator(Itemp,table);
	    }
	    t = 1000*((double)getTickCount() - t) / getTickFrequency() / times; //(计数差/频率/times = 运行一次所需的时间);
	    cout << "Time of reducing with the the iterator                      " << times << " runs: " << t << " millisecode." << endl;
	    
	    
	    t = (double)getTickCount();
	    for(int i = 0; i < times; i++)
	    {
	        J = ScanImageAndReduceRandomAccess(Itemp,table);
	    }
	    t = 1000*((double)getTickCount() - t) / getTickFrequency() / times; //(计数差/频率/times = 运行一次所需的时间);
	    cout << "Time of reducing with the the on-the-fly address generation " << times << " runs: " << t << " millisecode." << endl;

	    imshow("reduced", J);
	    waitKey(0);
	    return 0;
	}


	Mat & ScanImageAndReduce(Mat& I, const uchar * const table)
	{
	    CV_Assert(I.depth() != sizeof(uchar));
	    
	    const int channels = I.channels();
	    int nCols = I.rows * channels;
	    int nRows = I.cols;
	//    先判断下数组是否是连续存储的，如果是的话，所有元素都在一行上。
	    if(I.isContinuous())
	    {
	        nCols *= nRows;
	        nRows  = 1;
	    }
	    
	    uchar *p;
	//    根据原图数组行列数，创建对比数组
	    for(int i = 0; i < nRows; i++)
	    {
	        p = I.ptr<uchar>(i);
	        for(int j = 0; j < nCols; j++)
	        {
	            p[j] = table[p[j]];
	        }
	    }
	    return I;
	}


	Mat & ScanImageAndIterator(Mat& I, const uchar * const table)
	{
	    CV_Assert(I.depth() != sizeof(uchar));
	    const int channels = I.channels();
	    switch (channels) {
	        case 1:
	        {
	            //创建迭代器
	            MatIterator_<uchar> it, end;
	            for (it = I.begin<uchar>(),end = I.end<uchar>(); it!= end; it++)
	            {
	                *it = table[*it];
	            }
	            break;
	        }
	        case 3:
	        {
	            //创建迭代器
	            MatIterator_<Vec3b> it, end;
	            for (it = I.begin<Vec3b>(),end = I.end<Vec3b>(); it!= end; it++)
	            {
	                (*it)[0] = table[(*it)[0]];
	                (*it)[1] = table[(*it)[1]];
	                (*it)[2] = table[(*it)[2]];
	                
	            }
	            break;
	        }
	        default:
	            break;
	    }
	    return I;
	}

	Mat & ScanImageAndReduceRandomAccess(Mat& I, const uchar * const table)
	{
	    CV_Assert(I.depth() != sizeof(uchar));
	    const int channels = I.channels();
	    switch (channels) {
	        case 1:
	        {
	            for (int i = 0; i < I.rows; i++) {
	                for (int j = 0; j < I.cols; j++) {
	                    I.at<uchar>(i,j) = table[I.at<uchar>(i,j)];
	                }
	            }
	            break;
	        }
	        case 3:
	        {
	            Mat_<Vec3b> _I = I;
	            for (int i = 0; i < I.rows; i++)
	                for (int j = 0; j < I.cols; j++)
	                {
	                    _I(i,j)[0] = table[_I(i,j)[0]];
	                    _I(i,j)[1] = table[_I(i,j)[1]];
	                    _I(i,j)[2] = table[_I(i,j)[2]];
	                }
	            I = _I;
	            break;
	        }
	        default:
	            break;
	    }
	    return I;
	}
```


