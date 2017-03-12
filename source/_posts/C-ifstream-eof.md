---
title: C++ ifstream.eof()的一个小问题
date: 2016-12-30 17:40:24
tags: [C++]
catogory: [C++]
---

# 代码

```
while(!in.eof()){
    in >> tmp; //in是ifstream的对象
    cout << "temp:" << tmp << endl;
    if(count % 2 != 0){
        w[wIndex] = tmp;
        wIndex++;
    }else{
        v[vIndex] = tmp;
        vIndex++;
    }
    count++;
}
```

这里出来的结果是这样的

```
temp:26
...
...
temp:83
temp:17
temp:48  //这个是文件结尾最后一个数
temp:48  //但是这里却出现了两次，证明有错（有些编译器会报错）
```

# 解决方法

1. 在循环之前先读入一个数据
2. 把读取数据的操作放在循环的最后

```
in >> tmp;
while(!in.eof()){
    cout << "temp:" << tmp << endl;
    if(count % 2 != 0){
        w[wIndex] = tmp;
        wIndex++;
    }else{
        v[vIndex] = tmp;
        vIndex++;
    }
    count++;
    in >> tmp;
}
```

# 原因

这是因为判断文件是否结束的eof()函数是根据读入的字符判断的

比如：假定文件后面是`17 48 文件结束标志` 3个数据（假设是最后的三个数据），现在已经读到48（`in >> tmp`已经执行），到`!in.eof()`的时候，因为读入的不是文件结束标志，所以循环继续，再一次执行`in >> tmp`，读取的时候文件结束标记，如果后面还有一些操作包含这个**tmp**的话，有可能会报错。

```
while(!in.eof()){
    in >> tmp;
    //这里下面还有各种操作blabla
}
```

优化为：

```
in >> tmp;
while(!in.eof()){
    //这里下面还有各种操作blabla
    in >> tmp;
}
```

这样最后执行`in >> tmp`的时候就会读到文件结束标记，然后刚好有`!in.eof()`判断。
