title: "algorithm_全组合"
date: 2015-05-21 15:15:45
tags:
- algorithm
categories: skill
description: 静一静!
toc: true
---
添加标签插件[hexo-math](https://github.com/akfish/hexo-math).
以下是二种tag:
* math
&emsp;&emsp;{% math %}C{^m_n} = \dfrac{n!}{m!(n-m)!} {% endmath %}
* math_block
&emsp;&emsp;{% math %}C{^m_n} = C{^m_{n-1}} + C{^{m-1}_{n-1}}{% endmath %}

``` c
#include <stdio.h>
void recursion(int n, int m, int *data, int data_length)
{
	for(; n >= m; n--)
	{
		data[m-1] = n;
		if(m>1)
			recursion(n-1, m-1,data,data_length);
		else
		{
			for(int i=0; i<data_length; i++)
				printf("%d ",data[i]);
			printf("\n");
			recursion(n-1,1,data,data_length);
			return ;
		}
	}
}

void main(int argc, char *argv[])
{
	int data[3];
	recursion(5,3,data,3);		//1 2 3 4 5 (选择任意3个数,打印全组合 C53)
}
```
<!-- more -->
```
gcc test_recursion.c  -std=c11
```
#### 巩固基础,复习递归(尾递归),还要静静学习数学算法. ####
来自[CSDN_wumuzi520的专栏](http://blog.csdn.net/wumuzi520/article/details/8087501)
#### 非递归实现的.二进制组合算法. ####
思路是开一个数组,其下标表示1到m个数,数组元素的值为1表示其下标代表的数被选中,为0则没选中.    
首先初始化,将数组前n个元素置1,表示第一个组合为前n个数.    
然后从左到右扫描数组元素值的"10"组合,找到第一个"10"组合后将其变为"01"组合,
同时将其左边的所有"1"全部移动到数组的最左端（只有第一位变为0才需要移动,否则其左边的1本来就在最左端,无需移动）.    
当第一个"1"移动到数组的m-n的位置,即n个"1"全部移动到最右端时,就得到了最后一个组合.    
例如求5中选3的组合:
1   1   1   0   0   //1,2,3    
1   1   0   1   0   //1,2,4    
1   0   1   1   0   //1,3,4    
0   1   1   1   0   //2,3,4    
1   1   0   0   1   //1,2,5    
1   0   1   0   1   //1,3,5    
0   1   1   0   1   //2,3,5    
1   0   0   1   1   //1,4,5    
0   1   0   1   1   //2,4,5    
0   0   1   1   1   //3,4,5