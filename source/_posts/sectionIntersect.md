---
title: 8602 区间相交问题
date: 2016-12-23 15:30:10
tags: [算法,C++]
catogory: [算法]
---

## 题目描述

### Description

给定x轴上n个闭区间，去掉尽可能少的闭区间，使剩下的闭区间都不相交。

注意：这里，若区间与另一区间之间仅有端点是相同的，不算做区间相交。

例如，[1，2]和[2，3]算是不相交区间。

### 输入格式

第一行一个正整数n(n<=50)，表示闭区间数。

接下来n行中，每行2个整数，表示闭区间的2个整数端点。


### 输出格式

输出去掉的最少的闭区间数。


### 输入样例
3

10 20

10 15

12 15

### 输出样例
2

### 提示

这个问题基本等同于书本的活动安排问题。


## 代码

```
#include <iostream>

using namespace std;

typedef struct section{
    int start;
    int end;
} Section;

int greedySelector(Section* sectionArr,int n){
    int i,j;
    int count = 1;
    int current = 0;
    Section tmp;
    int length = n;
    for (i = 0; i < length-1; i++){
        for(j = i+1; j < length; j++){
            if (sectionArr[i].end >  sectionArr[j].end){
                tmp = sectionArr[i];
                sectionArr[i] = sectionArr[j];
                sectionArr[j] = tmp;
            }
        }
    }

    for(i = 1; i < length; i++){
        if (sectionArr[current].end <= sectionArr[i].start){
            count++;
            current = i;
        }
    }
    return count;
}

int main() {

    int n;
    cin >> n;
    Section* sectionArr = new Section[n];

    for (int i = 0; i < n; i++){
        cin >> sectionArr[i].start;
        cin >> sectionArr[i].end;
    }

    cout << n-greedySelector(sectionArr,n) << endl;

    delete []sectionArr;
    return 0;
}
```
