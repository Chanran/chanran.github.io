---
title: 寻找最大子方阵
date: 2016-12-16 15:46:00
tags: [C++]
categories: [C++]
---

## 题目描述

给定一个元素为0或1的方阵，编写一个程序，找出其中最大的子方阵，使得该子方阵的元素都是1。程序先提示用户输入矩阵的行数，然后提示用户输入矩阵内容，打印输出最大子方阵的第一个元素的位置以及最大子方阵的行数。假定矩阵最多有100行。下面是样例运行：

```
Enter the number of rows for the matrix: 5  ~Enter
Enter the matrix row by row:
1  0  1  0  1  ~Enter
1  1  1  0  1  ~Enter
1  0  1  1  1  ~Enter
1  0  1  1  1  ~Enter
1  0  1  1  1  ~Enter
The maximum square submatrix is at (2，2) with size 3
```

程序中应实现下面的函数来寻找最大子方阵：
vector <int> findLargestBlock (const vector <vector <int>> & m)
返回值是一个向量，包含3个值，前两个值代表该最大子方阵第一个元素的行标和列标，第三个值表示该最大子方阵的行数。

## C++代码


### main.cpp

> 没用什么优化算法，暴力解题。

```
#include <iostream>
#include <vector>

using namespace std;

vector <int> findLargestBlock (const vector <vector <int> > &m,int n){
    int t = 1;
    int i = 0,j = 0,l = 0;
    int x = 0,y = 0;
    vector<int> result(3,0);

    for(l=n;l>=1;l--){ //矩阵维数，从最大开始
        for(i=0;i<=n-l;i++){
            for(j=0;j<=n-l;j++){
                t=1;
                for(x=i;x<i+l;x++){
                    for(y=j;y<j+l;y++){
                        if(m[x][y]!=1){    //不为1退出此轮循环
                            t=0;
                            break;
                        }
                    }
                    if(t==0) break;
                }
                if(t==1) break;
            }
            if(t==1) break;
        }
        if(t==1) break;
    }

    result[0] = i;
    result[1] = j;
    result[2] = l;

    return result;
}

int main() {

    int n;
    vector<vector <int> > m(100,vector<int>(100,0));
    cout << "Enter the number of rows for the matrix:";
    cin >> n;
    cout << "Enter the matrix row by row:" << endl;
    int numOfOne = 0;
    for (int i = 0; i < n; i++){
        for (int j = 0; j < n; j++){
            cin >> m[i][j];
        }
    }
    vector<int> result =  findLargestBlock(m,n);

    cout << "横坐标：" << result[0] << " 纵坐标：" << result[1] << " 最大子方阵的行数：" << result[2] << endl;
    return 0;
}
```
