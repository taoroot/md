--- 
title: 时钟中断 控制8位LED数码管组
tags:  ["msp430"]
notebook: Knowledge 
---


### 利用时钟中断 控制8位LED数码管组

```cpp
#include "msp430g2553.h"
#include "src/Write7219.h"

int Count = 0;
int i 	  = 0;
void main(void) 
{
	WDTCTL=WDTPW+WDTHOLD; 	//关狗
	P2DIR |=BIT3+BIT4+BIT5;
	P1DIR &=~BIT3;		 	//输入
	P1REN |=BIT3;			//调用内部上（下）拉电阻
//	P1OUT |=BIT3;			//决定最终为上拉电阻
//	P1IES |=BIT3;			//决定上升沿为中断
//	P1IE  |=BIT3;			//开启总中断（I/O中断）
	TACTL |=TASSEL_2 + MC_1;//选择时钟源、工作模式
	TACCR0 =50000;			//设置计时值
	TACCTL0 |=CCIE;			//开启时钟中断
	_EINT();				//打开总中断

	Write7219(0x09FF);//初始化
	Write7219(0x0a08);//初始化
	Write7219(0x0b07);//初始化
	Write7219(0x0c01);//初始化
	Write7219(0x0f00);//初始化
	while(1)
	{
		Write7219((1<<8)|(i%10));
		Write7219((2<<8)|(i/10%6));
		Write7219((3<<8)|10);
		Write7219((4<<8)|(i/60%10));
		Write7219((5<<8)|(i/60/10%6));
		Write7219((6<<8)|10);
		Write7219((7<<8)|(i/60/60%10));
		Write7219((8<<8)|(i/60/60/10%3));
	}
}
//	//中断服务函数
//	#pragma vector = PORT1_VECTOR
//	__interrupt void PORT1_ISR(void)
//	{
//		MATH3210();
//		P1IFG = 0;				//清除I/O中断标志位
//	}

#pragma vector = TIMER0_A0_VECTOR
__interrupt void Timer_A(void)
{
	Count++;
	if(Count == 1)
	{
		i++;
		Count = 0;
	}
}

```

### 添加一个4*4的矩阵按键调整时间

```cpp
#include "msp430g2553.h"
#include "src/Write7219.h"

int i;
int Sec=0;
int Min=0;
int Hour=0;
int Count = 0;
int Count_Button = 0;
void Init_Button();
void Init_GPIO();
void Init_Time();
void Init_7219();
void Time_Dis();
void Time_Count();


void main(void) {
	WDTCTL=WDTPW+WDTHOLD; 	//关狗
	Init_GPIO();
	Init_Time();
	Init_7219();
	Init_Button();
	_EINT();				//打开总中断
}


void Init_GPIO(){
	P2DIR |=BIT3+BIT4+BIT5;
}


void Init_Time(){
		TACTL |=TASSEL_2 + MC_1;//选择时钟源、工作模式
		TACCR0 =50000;			//设置计时值
		TACCTL0 |=CCIE;			//开启时钟中断
}

void Init_Button(){
	P1DIR &=~(BIT3+BIT4+BIT5);		 	//输入
	P1REN |=(BIT3+BIT4+BIT5);			//调用内部上（下）拉电阻
	P1OUT |=(BIT3+BIT4+BIT5);			//决定最终为上拉电阻
	P1IES |=(BIT3+BIT4+BIT5);			//决定上升沿为中断
	P1IE  |=(BIT3+BIT4+BIT5);			//开启总中断（I/O中断）
}
void Init_7219(){
	Write7219(0x09FF);//初始化
	Write7219(0x0a08);//初始化
	Write7219(0x0b07);//初始化
	Write7219(0x0c01);//初始化
	Write7219(0x0f00);//初始化
}


void Time_Dis(){
	Write7219((1<<8)|(Sec%10));
	Write7219((2<<8)|(Sec/10));
	Write7219((3<<8)|0x0a);
	Write7219((4<<8)|(Min%10));
	Write7219((5<<8)|(Min/10));
	Write7219((6<<8)|0x0a);
	Write7219((7<<8)|(Hour%10));
	Write7219((8<<8)|(Hour/10));
}


void Time_Count(){
	Sec++;
	if(Sec==60){
		Min++;
		Sec=0;
	}
	if(Min==60){
		Hour++;
		Min=0;
	}
	if(Hour==24){
		Hour=0;
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




#pragma vector = TIMER0_A0_VECTOR
__interrupt void Timer_A(void){
	Count++;
	if(Count==10&&Count_Button%4==1){
			Write7219((1<<8)|(0x0f));
			Write7219((2<<8)|(0x0f));
		}
		if(Count==10&&Count_Button%4==2){
				Write7219((4<<8)|(0x0f));
				Write7219((5<<8)|(0x0f));
			}
		if(Count==10&&Count_Button%4==3){
				Write7219((7<<8)|(0x0f));
				Write7219((8<<8)|(0x0f));
			}
	if(Count == 20){
		Time_Count();
		Time_Dis();
		Count = 0;
	}
}



//中断服务函数
#pragma vector = PORT1_VECTOR
__interrupt void PORT1_ISR(void)
{
	if(P1IFG &BIT3){
		_delay_cycles(10000);
		if((P1IN&BIT3) == 0){
			Count_Button++;
		}
	}
	if(P1IFG&BIT4){
		_delay_cycles(1000000);
		if((P1IN&BIT4)==0){
			if(Count_Button%4==1){
				Sec++;
				if(Sec>60){
					Sec=0;
				}
			}
			if(Count_Button%4==2){
				Min++;
				if(Min>60){
					Min=0;
				}
			}
			if(Count_Button%4==3){
				Hour++;
				if(Hour>24){
					Hour=0;
				}
			}
		}

	}
	if(P1IFG&BIT5){
		_delay_cycles(1000000);
		if((P1IN&BIT5)==0){
			if(Count_Button%4==1){
				Sec--;
				if(Sec<0){
					Sec=59;
				}
			}
			if(Count_Button%4==2){
				Min--;
				if(Min<0){
					Min=59;
				}
			}
			if(Count_Button%4==3){
				Hour--;
				if(Hour<0){
						Hour=23;
				}
			}
		}
	}
	P1IFG = 0;				//清除I/O中断标志位
}
```

