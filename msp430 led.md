--- 
title: led
tags:  ["msp430"]
notebook: Knowledge
---

## MSP430G2553 LED 操作
1. 红灯:P1.0
2. 绿灯:P1.6
3. 按钮:P1.3


###按下按钮红灭绿亮，松开红亮绿灭

```cpp
/*按下红灭绿亮，松开红亮绿灭*/
#include "msp430g2553.h"
void main(void) 
{
	WDTCTL=WDTPW+WDTHOLD;		//关闭关门狗
	P1DIR &=~BIT3;				//设置P1.3为输入
	P1DIR |=BIT6;				//设置P1.6为输出
	P1DIR |=BIT0;				//设置P1.0为输出
	P1REN |=BIT3;				//上拉P1.3的电阻(P1.3现在接的只有一个电阻,上拉P1.3的电阻为了防止短路)
	P1OUT |=BIT3;				//给P1.3供电
	while(1)
	{
		if(P1IN & BIT3)				//按下开关P1.3
	    {
	       	P1OUT |=BIT0;
		   P1OUT &=~BIT6;
		}
		else 						//松开开关P1.3
		{
			P1OUT |=BIT6;
		    P1OUT &=~BIT0;
		}
	}
}
```

###利用上升沿,设置按钮按下显示不同的结果

```cpp
/*P99 全灭，红亮绿灭，红灭绿亮，全亮     外部函数外部存储*/
unsigned char Num=0,key_past=0,key_now=0;
#include "msp430g2553.h"
void main(void) {
	WDTCTL=WDTPW+WDTHOLD;		//关闭关门狗
	P1DIR |=BIT6+BIT0;			//设置P1.6和P1.0输出
	P1DIR &=~BIT3;				//设置P1.3为输入
	P1REN |=BIT3;				//上拉P1.3的电阻(P1.3现在接的只有一个电阻,上拉P1.3的电阻为了防止短路)
	P1OUT |=BIT3;				//给P1.3供电
	while(1)
	{
	    Blink_LED();
	}
}

void Blink_LED()
{
    if(P1IN & BIT3)						//按下开关P1.3
      	key_now=1;			
    else 
    	key_now=0;						//松开开关P1.3
    _delay_cycles(1000);
    if((key_now==1)&&(key_past==0))		//在按钮被按下的上升沿Num加1
 	  Num++;
    key_past=key_now;					
    switch(Num%4)
    {
      case 0:P1OUT &=~(BIT6+BIT0);
             break;
      case 1:P1OUT |=BIT6;
           	 P1OUT &=~BIT0;
           	 break;
      case 2:P1OUT |=BIT0;
	   	     P1OUT &=~BIT6;
	   	     break;
      case 3:P1OUT |=BIT6+BIT0;
             break;
	}
}
```

### 利用按键中断,设置按钮按下显示不同的结果

```cpp
#include "msp430g2553.h"
void Blink_LED();
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
	Blink_LED();
	P1IFG = 0;				//清除I/O中断标志位
}

void Blink_LED()
{
	 Num++;
    switch(Num%4)
    {
      case 0:P1OUT &=~(BIT6+BIT0);
             break;
      case 1:P1OUT |=BIT6;
           	 P1OUT &=~BIT0;
           	 break;
      case 2:P1OUT |=BIT0;
	   	     P1OUT &=~BIT6;
	   	     break;
      case 3:P1OUT |=BIT6+BIT0;
             break;
	}
}
```
