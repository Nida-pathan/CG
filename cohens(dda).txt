/*
Problem Statement: Write C++ program to implement Cohen Sutherland line clipping algorithm.
Code from Computer Graphics (SPPU - Second Year - Computer Engineering - Content) repository on KSKA Git: https://git.kska.io/sppu-se-comp-codes/CG
*/

// BEGINNING OF CODE
#include<iostream>
#include<graphics.h>
#include<math.h>
#include<cstdlib>
using namespace std;
static int LEFT = 1, RIGHT = 2, BOTTOM = 4, TOP = 8, xmax,ymax,xmin,ymin;

int getcode(int x,int y)
{
	int code = 0;
	if(y>ymax) code |= TOP;
	if(y<ymin) code |= BOTTOM;
	if(x<xmin) code |= LEFT;
	if(x>xmax) code |= RIGHT;
	return code;
}

void line1(int x1,int y1,int x2,int y2)
{
	float len;
	float dx = x2-x1;
	float dy = y2-y1;
	if(abs(dx) >= abs(dy))
	{
		len = abs(dx);
	} else {
		len = abs(dy);
	}
	float xin = dx/len;
	float yin = dy/len;
	float x = x1;
	float y = y1;
	int i = 0;
	while (i<=len)
	{
		putpixel(round(x),round(y),RED);
			x = x + xin;
			y = y + yin;
		i++;
	}
}

int main()
{
	int gd = DETECT,gm;
	int x1,y1,x2,y2;
	cout<<"Enter top left and bottom right coordinates: ";
	cin>>xmin>>ymin>>xmax>>ymax;
	cout<<"Enter endpoints of line: ";
	cin>>x1>>y1>>x2>>y2;
	initgraph(&gd,&gm,NULL);
	//outtext("Before clipping");
	rectangle(xmin,ymin,xmax,ymax);
	line1(x1,y1,x2,y2);
	delay(10000);

	int outcode1 = getcode(x1,y1);
	int outcode2 = getcode(x2,y2);
	int accept = 0;
	while(1)
	{
		float m = (float)(y2-y1)/(x2-x1);
		if(outcode1==0 && outcode2==0)
		{
			accept = 1;
			break;
		}
		else if((outcode1 & outcode2)!= 0)
		{
			break;
		}
		else
		{
			int x,y;
			int temp;
			if(outcode1 == 0)
			{
				temp = outcode2;
			}
			else
			{
				temp = outcode1;
			}
			if(temp & TOP)
			{
				x = x1 + (ymax - y1)/m;
				y = ymax;
			}
			else if(temp & BOTTOM)
			{
				x = x1 + (ymin - y1)/m;
				y = ymin;
			}
			else if(temp & LEFT)
			{
				x = xmin;
				y = y1 + m*(xmin - x1);
			}
			else if(temp & RIGHT)
			{
				x = xmax;
				y = y1 + m*(xmax - x1);
			}
			if(temp == outcode1)
			{
				x1 = x;
				y1 = y;
				outcode1 = getcode(x1,y1);
			}
			else
			{
				x2 = x;
				y2 = y;
				outcode2 = getcode(x2,y2);
			}
		}
	}
	setcolor(BROWN);
	if(accept)
	{
		cleardevice();
		//outtext("After clipping");
		rectangle(xmin,ymin,xmax,ymax);
		line1(x1,y1,x2,y2);
	}
	else
	{
		cleardevice();
		//outtext("After clipping");
		rectangle(xmin,ymin,xmax,ymax);
	}
	getch();
	closegraph();
	return 0;
}
// END OF CODE
