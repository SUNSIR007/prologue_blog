---
title: 递归-全排列
tags: ["C++"]
image: /static/images/digui.png
pubDate: 2024-2-23
featured: false
---

```c++
#include <stdio.h>


int n,p[10];//p 用来存放排列的序列
bool hashTable[10]={false}; //hashTable用来表示数字是否已经被使用过，例如：若 1 被使用过了，则 hashTable[1]=true

void pailie(int index)
{
    if(index>=n+1)//该函数是一位一位地进行确认，当所有位都确认完成时，即 index>=n+1，此时即可将该序列输出
    {
        for(int i=1;i<=n;i++)
        {
            printf("%d",p[i]);//将排列好的序列输出
        }
        printf("\n");
    }
    for(int j=1;j<=n;j++)
    {
        if(hashTable[j]==false)//如果j这个数还没有被使用
        {
            p[index]=j;//令当前这个位数为 j
            hashTable[j]=true;//将 j 标记为已使用
            pailie(index+1);//使用递归产生下一位数
            hashTable[j]=false;//这个位数为 j 的序列已经全部生成，换成其他序列进行尝试
        }
    }
}


int main()
{
    scanf("%d",&n);
    pailie(1);//从第一位开始，先确定好第一位
    return 0;
}
```

算法思想：先确定一位数，然后使用递归生成下一位
