--- 
title: 8*8 LED 矩阵
tags:  ["msp430"]
notebook: Knowledge 
---

### 8*8LED矩阵 显示数字

```cpp
#include "msp430g2553.h"
void Write7219(int data);
void main(void) 
{
	WDTCTL=WDTPW+WDTHOLD; 		//关狗
	P2DIR |=BIT3+BIT4+BIT5;
		Write7219(0x0900);		//初始化
		Write7219(0x0a08);		//初始化
		Write7219(0x0b07);		//初始化
		Write7219(0x0c01);		//初始化
		Write7219(0x0f00);		//初始化
    while(1)
    {
    	//显示2
		Write7219(0x013C);
		Write7219(0x0204);
		Write7219(0x0304);
		Write7219(0x043C);
		Write7219(0x0520);
		Write7219(0x0620);
		Write7219(0x073C);
		Write7219(0x0800);
		_delay_cycles(1000000);
		//显示1
		Write7219(0x0108);
		Write7219(0x0218);
		Write7219(0x0308);
		Write7219(0x0408);
		Write7219(0x0508);
		Write7219(0x0608);
		Write7219(0x073E);
		Write7219(0x0800);
		_delay_cycles(1000000);
    }
}

/*控制led矩阵显示*/
#define din0  P2OUT &= ~BIT3
#define din1  P2OUT |= BIT3
#define load0 P2OUT &= ~BIT4
#define load1 P2OUT |= BIT4
#define clk0  P2OUT &= ~BIT5
#define clk1  P2OUT |= BIT5
void Write7219(int data)
{
  int i;
  load0;
  for(i=0;i<16;i++)
  {
    clk0;
    if(data&0x8000)
      din1;
    else
      din0;
    data<<=1;
    clk1;
  }
  load1;
}
```

### 使用按键中断控制  全灭，红亮绿灭，红灭绿亮，全亮

```cpp
#include "msp430g2553.h"
void MATH3210();
unsigned char Num=0;
void main(void) {
	WDTCTL=WDTPW+WDTHOLD;
	P1DIR |=BIT6+BIT0;   	//输出
	P1DIR &=~BIT3;		 	//输入
	P1REN |=BIT3;			//调用内部上（下）拉电阻
	P1OUT |=BIT3;			//决定最终为上拉电阻
	P1IES |=BIT3;			//决定上升沿为中断
	P1IE  |=BIT3;			//开启总中断（I/O中断）
	_EINT();				//跳转中断服务函数
}
//中断服务函数
#pragma vector = PORT1_VECTOR
__interrupt void PORT1_ISR(void)
{
	MATH3210();
	P1IFG = 0;				//清除I/O中断标志位
}

/* 利用8*8矩阵显示0-9数字 */
unsigned char Num=0;
unsigned char a[10][8]={
		{0x3C,0x24,0x24,0x24,0x24,0x24,0x3C,0x00},//0
		{0x08,0x18,0x08,0x08,0x08,0x08,0x3E,0x00},//1
		{0x3C,0x04,0x04,0x3C,0x20,0x20,0x3C,0x00},//2
		{0x3C,0x04,0x04,0x3C,0x04,0x04,0x3C,0x00},//3
		{0x24,0x24,0x24,0x3C,0x04,0x04,0x04,0x00},//4
		{0x3C,0x20,0x20,0x3C,0x04,0x04,0x3C,0x00},//5
		{0x3C,0x20,0x20,0x3C,0x24,0x24,0x3C,0x00},//6
		{0x3C,0x24,0x24,0x04,0x04,0x04,0x04,0x00},//7
		{0x3C,0x24,0x24,0x3C,0x24,0x24,0x3C,0x00},//8
		{0x3C,0x24,0x24,0x3C,0x04,0x04,0x3C,0x00}//9
};

void MATH3210()
{
	Write7219(0x0900);//初始化
	Write7219(0x0a08);//初始化
	Write7219(0x0b07);//初始化
	Write7219(0x0c01);//初始化
	Write7219(0x0f00);//初始化
	Num++;
    switch((Num-1)%10)
    {
      case 0:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[0][i]);
				_delay_cycles(10000);
		     break;
      case 1:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[1][i]);
				_delay_cycles(10000);
			    break;
      case 2:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[2][i]);
				_delay_cycles(10000);
	   	        break;
      case 3:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[3][i]);
				_delay_cycles(10000);
                break;
      case 4:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[4][i]);
				_delay_cycles(10000);
                break;
      case 5:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[5][i]);
				_delay_cycles(10000);
                break;
      case 6:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[6][i]);
				_delay_cycles(10000);
                break;
      case 7:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[7][i]);
				_delay_cycles(10000);
                break;
      case 8:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[8][i]);
				_delay_cycles(10000);
                break;
      case 9:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[9][i]);
				_delay_cycles(10000);
                break;
	}
}

```


### 同时控制四个8*8LED矩阵

```cpp
#include "msp430g2553.h"
#define din0  P2OUT &= ~BIT3
#define din1  P2OUT |= BIT3
#define load0 P2OUT &= ~BIT4
#define load1 P2OUT |= BIT4
#define clk0  P2OUT &= ~BIT5
#define clk1  P2OUT |= BIT5
int i;
void Write7219(int data0, int data1, int data2, int data3)
{
  int i;
  load0;
  for(i=0;i<16;i++)
  {
    clk0;
    if(data0&0x8000)
      din1;
    else
      din0;
    data0<<=1;
    clk1;
  }
  for(i=0;i<16;i++)
    {
      clk0;
      if(data1&0x8000)
        din1;
      else
        din0;
      data1<<=1;
      clk1;
    }
  for(i=0;i<16;i++)
    {
      clk0;
      if(data2&0x8000)
        din1;
      else
        din0;
      data2<<=1;
      clk1;
    }
  for(i=0;i<16;i++)
    {
      clk0;
      if(data3&0x8000)
        din1;
      else
        din0;
      data3<<=1;
      clk1;
    }
   load1;
}

unsigned char Num=0;
unsigned char a[10][8]=
{
		{0x3C,0x24,0x24,0x24,0x24,0x24,0x3C,0x00},//0
		{0x08,0x18,0x08,0x08,0x08,0x08,0x3E,0x00},//1
		{0x3C,0x04,0x04,0x3C,0x20,0x20,0x3C,0x00},//2
		{0x3C,0x04,0x04,0x3C,0x04,0x04,0x3C,0x00},//3
		{0x24,0x24,0x24,0x3C,0x04,0x04,0x04,0x00},//4
		{0x3C,0x20,0x20,0x3C,0x04,0x04,0x3C,0x00},//5
		{0x3C,0x20,0x20,0x3C,0x24,0x24,0x3C,0x00},//6
		{0x3C,0x24,0x24,0x04,0x04,0x04,0x04,0x00},//7
		{0x3C,0x24,0x24,0x3C,0x24,0x24,0x3C,0x00},//8
		{0x3C,0x24,0x24,0x3C,0x04,0x04,0x3C,0x00}//9
};

void MATH3210()
{
	Write7219(0x0900,0x0900,0x0900,0x0900);//初始化
	Write7219(0x0a08,0x0a08,0x0a08,0x0a08);//初始化
	Write7219(0x0b07,0x0b07,0x0b07,0x0b07);//初始化
	Write7219(0x0c01,0x0c01,0x0c01,0x0c01);//初始化
	Write7219(0x0f00,0x0f00,0x0f00,0x0f00);//初始化
	Num++;
    switch((Num-1)%2)
    {
      case 0:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[0][i],((i+1)<<8)|a[1][i],((i+1)<<8)|a[2][i],((i+1)<<8)|a[3][i]);
				_delay_cycles(10000);
		     break;

      case 1:
		     for(i=0;i<8;i++)
	             Write7219(((i+1)<<8)|a[4][i],((i+1)<<8)|a[5][i],((i+1)<<8)|a[6][i],((i+1)<<8)|a[8][i]);
				_delay_cycles(10000);
                break;
	}
}

```

