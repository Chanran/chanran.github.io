---
title: Complex类，运算符重载
date: 2016-12-16 15:53:07
tags: [C++]
categories: [C++]
---

## 题目描述

复数形式是a+bi，其中a和b是实数，i是。a和b分别被称为复数的实部和虚部。可以使用下列公式实现复数的加、减、乘、除：

a + bi + c + di = (a + c) + (b + d) i

a + bi - (c + di) = (a - c) + (b - d) i

(a + bi) * (c + di) = (ac - bd) + (bc + ad) i

(a + bi) / (c + di) = (ac + bd) / (c2 + d2) + (bc - ad) i /
(c2 + d2)

使用下面公式也可以获得复数的绝对值：

|a+bi|=√(a^2+b^2)

设计一个名为Complex的复数类，它可以用函数add、subtract、multiply、divide和abs实现复数的加、减、乘、除和取绝对值。toString函数实现以字符串形式表示的复数a+bi。如果b是0，只返回a。

该类有三个构造函数Complex (a, b)、Complex (a)和Complex ( )。Complex ( )生成一个表示原点的复数对象，Complex (a)生成一个b值为0的复数对象。函数getRealPart ( )和getImaginaryPart ( )分别返回复数的实部和虚部。

重载运算符+，-，*，/，+=，-=，*=，/=，[ ]，一元+和-，前缀++和--，后缀++和--，<<，>>。

以非成员函数形式重载+，-，*，/。重载[ ]，使得[0]返回a，[1]返回b。
编写一个测试程序：当用户输入两个复数后，程序显示它们的加、减、乘、除操作的结果。样例输出如下：

```
Enter the first complex number:  3.5  5.5  ~Enter
Enter the second complex number: -3.5  1  ~Enter
(3.5 + 5.5i) + (-3.5 + 1.0i) = 0.0 + 6.5i
(3.5 + 5.5i) - (-3.5 + 1.0i) = 7.0 + 4.5i
(3.5 + 5.5i) * (-3.5 + 1.0i) = -17.75 + -15.75i
(3.5 + 5.5i) / (-3.5 + 1.0i) = -0.5094 + -1.7i
|3.5 + 5.5i| = 6.519202405202649
```

## C++代码

### Complex.h

```
//
// Created by blue on 16-12-9.
//

#ifndef COMPLEXCLASS_COMPLEX_H
#define COMPLEXCLASS_COMPLEX_H

#include "iostream"
#include "sstream"
#include <string>

class Complex {
private:
    double realPart;
    double virtualPart;

public:
    Complex add(const Complex &c);
    Complex subtract(const Complex &c);
    Complex multiply(const Complex &c);
    Complex divide(const Complex &c);
    double abs();
    std::string toString();

public:
    Complex operator+(const Complex &c);
    Complex operator-(const Complex &c);
    Complex operator*(const Complex &c);
    Complex operator/(const Complex &c);
    Complex operator+=(const Complex &c);
    Complex operator-=(const Complex &c);
    Complex operator*=(const Complex &c);
    Complex operator/=(const Complex &c);
    double operator[](const int &index);
    Complex operator+();
    Complex operator-();
    Complex operator++(); //前置版本
    Complex operator--(); //前置版本
    Complex operator++(int t); //后置版本
    Complex operator--(int t); //后置版本
    friend std::ostream& operator<<(std::ostream &os,const Complex &c); //输出运算附
    friend std::istream& operator>>(std::istream &is,Complex c); //输入运算符

public:
    Complex();
    Complex(double realPart);

    Complex(double realPart, double virtualPart);

    double getRealPart() const;

    void setRealPart(double realPart);

    double getVirtualPart() const;

    void setVirtualPart(double virtualPart);

};


#endif //COMPLEXCLASS_COMPLEX_H

```

### Complex.cpp

```
//
// Created by blue on 16-12-9.
//

#include "iostream"
#include "sstream"
#include "Complex.h"
#include <math.h>

/*方法*/
Complex Complex:: add(const Complex &c){
    Complex temp;
    temp.setRealPart(Complex::getRealPart()+c.getRealPart());
    temp.setVirtualPart(Complex::getVirtualPart()+c.getVirtualPart());
    return temp;
}
Complex Complex::subtract(const Complex &c){
    Complex temp;
    temp.setRealPart(Complex::getRealPart()-c.getRealPart());
    temp.setVirtualPart(Complex::getVirtualPart()-c.getVirtualPart());
    return temp;
}
Complex Complex::multiply(const Complex &c){
    Complex temp;
    temp.setRealPart(Complex::getRealPart()*c.getRealPart()-Complex::getVirtualPart()*c.getVirtualPart());
    temp.setVirtualPart(Complex::getVirtualPart()*c.getRealPart()+Complex::getRealPart()*c.getVirtualPart());
    return temp;
}
Complex Complex::divide(const Complex &c){
    Complex temp;
    temp.setRealPart((Complex::getRealPart()*c.getRealPart()+Complex::getVirtualPart()*c.getVirtualPart())/(pow(c.getRealPart(),2)+pow(c.getVirtualPart(),2)));
    temp.setVirtualPart((Complex::getVirtualPart()*c.getRealPart()-Complex::getRealPart()*c.getVirtualPart())/(pow(c.getRealPart(),2)+pow(c.getVirtualPart(),2)));
    return temp;
}
double Complex::abs(){
    return sqrt(pow(Complex::getRealPart(),2)+pow(Complex::getVirtualPart(),2));
}
std::string Complex::toString(){
    std::ostringstream ostring;

    if (Complex::getVirtualPart() == 0){
        ostring << Complex::getRealPart();
    }else if (Complex::getVirtualPart() < 0){
        ostring << Complex::getRealPart() << "-" << fabs(Complex::getVirtualPart()) << "i";
    }else{
        ostring << Complex::getRealPart() << "+" << Complex::getVirtualPart() << "i";
    }

    return ostring.str();
}

/*运算符重载*/
Complex Complex::operator+(const Complex &c){
    Complex tmp;
    tmp.setRealPart(Complex::getRealPart()+c.getRealPart());
    tmp.setVirtualPart(Complex::getVirtualPart()+c.getVirtualPart());
    return tmp;
}
Complex Complex::operator-(const Complex &c){
    Complex tmp;
    tmp.setRealPart(Complex::getRealPart()-c.getRealPart());
    tmp.setVirtualPart(Complex::getVirtualPart()-c.getVirtualPart());
    return tmp;
}
Complex Complex::operator*(const Complex &c){
    Complex tmp(Complex::getRealPart(),Complex::getVirtualPart());
    tmp.setRealPart(Complex::getRealPart()*c.getRealPart()-Complex::getVirtualPart()*c.getVirtualPart());
    tmp.setVirtualPart(Complex::getVirtualPart()*c.getRealPart()+Complex::getRealPart()*c.getVirtualPart());
    return tmp;
}
Complex Complex::operator/(const Complex &c){
    Complex temp;
    temp.setRealPart((Complex::getRealPart()*c.getRealPart()+Complex::getVirtualPart()*c.getVirtualPart())/(pow(c.getRealPart(),2)+pow(c.getVirtualPart(),2)));
    temp.setVirtualPart((Complex::getVirtualPart()*c.getRealPart()-Complex::getRealPart()*c.getVirtualPart())/(pow(c.getRealPart(),2)+pow(c.getVirtualPart(),2)));
    return temp;
}
Complex Complex::operator+=(const Complex &c){
    double a = Complex::getRealPart();
    double b = Complex::getVirtualPart();
    Complex::setRealPart((a*c.getRealPart()+b*c.getVirtualPart())/(pow(c.getRealPart(),2)+pow(c.getVirtualPart(),2)));
    Complex::setVirtualPart((b*c.getRealPart()-a*c.getVirtualPart())/(pow(c.getRealPart(),2)+pow(c.getVirtualPart(),2)));
    return *this;
}
Complex Complex::operator-=(const Complex &c){
    double a = Complex::getRealPart();
    double b = Complex::getVirtualPart();
    Complex::setRealPart(a-c.getRealPart());
    Complex::setVirtualPart(b-c.getVirtualPart());
    return *this;
}
Complex Complex::operator*=(const Complex &c){
    double a = Complex::getRealPart();
    double b = Complex::getVirtualPart();
    Complex::setRealPart(a*c.getRealPart()-b*c.getVirtualPart());
    Complex::setVirtualPart(b*c.getRealPart()+a*c.getVirtualPart());
    return *this;
}
Complex Complex::operator/=(const Complex &c){
    double a = Complex::getRealPart();
    double b = Complex::getVirtualPart();
    Complex::setRealPart((a*c.getRealPart()+b*c.getVirtualPart())/(pow(c.getRealPart(),2)+pow(c.getVirtualPart(),2)));
    Complex::setVirtualPart((a*c.getRealPart()-b*c.getVirtualPart())/(pow(c.getRealPart(),2)+pow(c.getVirtualPart(),2)));
    return *this;
}
double Complex::operator[](const int &index){
    if (index == 0){
        return Complex::getRealPart();
    }else if(index == 1){
        return Complex::getVirtualPart();
    }else{
        std::cout << "Invalid Index." << std::endl;
    }
}
Complex Complex::operator+(){
    Complex::setRealPart(fabs(Complex::getRealPart()));
    Complex::setVirtualPart(fabs(Complex::getVirtualPart()));
    return *this;
}
Complex Complex::operator-(){
    Complex::setRealPart(-Complex::getRealPart());
    Complex::setVirtualPart(-Complex::getVirtualPart());
    return *this;
}
Complex Complex::operator++(){ //前置版本
    Complex::setRealPart(Complex::getRealPart()+1);
    Complex::setVirtualPart(Complex::getRealPart()+1);
    return *this;
}
Complex Complex::operator--(){ //前置版本
    Complex::setRealPart(Complex::getRealPart()-1);
    Complex::setVirtualPart(Complex::getRealPart()-1);
    return *this;
}
Complex Complex::operator++(int t){ //后置版本
    Complex tmp(Complex::getRealPart(),Complex::getVirtualPart());
    this->setRealPart(this->getRealPart()+1);
    this->setVirtualPart(this->getVirtualPart()+1);
    return tmp;
}
Complex Complex::operator--(int t){ //后置版本
    Complex tmp(Complex::getRealPart(),Complex::getVirtualPart());
    this->setRealPart(this->getRealPart()-1);
    this->setVirtualPart(this->getVirtualPart()-1);
    return tmp;
}

/*输入输出重载*/
std::ostream& operator<<(std::ostream &os,const Complex &c){ //输出运算符
    if (c.getVirtualPart() > 0){
        os << c.getRealPart() << "+" << c.getVirtualPart() << "i";
    }else if(c.getVirtualPart() == 0){
        os << c.getRealPart();
    }else{
        os << c.getRealPart() << "-" << fabs(c.getVirtualPart()) << "i";
    }
    return os;
}
std::istream& operator>>(std::istream &is,Complex c){ //输入运算符
    double t1,t2;
    is >> t1 >> t2;
    c.setRealPart(t1);
    c.setVirtualPart(t2);
    return is;
}

/*构造函数,getter,setter*/
Complex::Complex() {
    Complex::realPart = 0;
    Complex::virtualPart = 0;
}

Complex::Complex(double realPart) : realPart(realPart) {
    Complex::realPart = realPart;
    Complex::virtualPart = 0;
}

Complex::Complex(double realPart, double virtualPart) : realPart(realPart), virtualPart(virtualPart) {
    Complex::realPart = realPart;
    Complex::virtualPart = virtualPart;
}


double Complex::getRealPart() const {
    return realPart;
}

void Complex::setRealPart(double realPart) {
    Complex::realPart = realPart;
}

double Complex::getVirtualPart() const {
    return virtualPart;
}

void Complex::setVirtualPart(double virtualPart) {
    Complex::virtualPart = virtualPart;
}
```

### main.cpp

```
#include <iostream>
#include "Complex.h"

using namespace std;

int main(){

    double a0,a1;
    double b0,b1;
    cout << "Enter the first complex number: " << endl;
    cin >> a0 >> a1;
    cout << "Enter the second complex number:" << endl;
    cin >> b0 >> b1;

    Complex a(a0,a1);
    Complex b(b0,b1);


    cout << "(" << a.toString() << ")" << " + " << "(" << b.toString() << ")" << " = " << (a+b).toString() << endl;
    cout << "(" << a.toString() << ")" << " - " << "(" << b.toString() << ")" << " = " << (a-b).toString() << endl;
    cout << "(" << a.toString() << ")" << " * " << "(" << b.toString() << ")" << " = " << (a*b).toString() << endl;
    cout << "(" << a.toString() << ")" << " / " << "(" << b.toString() << ")" << " = " << (a/b).toString() << endl;
    cout << "|" << a.toString() << "|" << " = " << a.abs() << endl;

    return 0;
}
```
