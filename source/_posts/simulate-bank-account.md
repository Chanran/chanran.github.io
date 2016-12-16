---
title: 模拟银行账户
date: 2016-12-16 16:08:06
tags: [C++]
categories: [C++]
---

## 题目描述

前面创建了Account类来建模银行账户：

（1）一个名为id的int型数据域，表示账户的身份号。

（2）一个名为balance的double型的数据域，表示账面余额。

（3）一个名为annualInterestRate的double型数据域，保存当前年利率。

（4）一个无参的构造函数，创建一个缺省的账户，其数据域id为0，balance为0，annualInterestRate为0。

（5）id、balance和annualInterestRate的访问器和更改器函数。

一个账户有账号、余额、年利率和账户创建时间等属性，还有存款和取款函数。创建它的两个派生类——支票账户和储蓄账户，前者有一个透支额度，后者不允许透支。定义Account类的一个常量虚函数toString（），并在派生类覆盖它，用来以字符串形式返回账号的余额。

画出这些类的UML图，实现该类，并编写一个测试程序，它创建一个Account、一个SavingsAccount和一个CheckingAccount账户，并调用它们的toString（）函数。

## C++代码

### Account.h

```
//
// Created by blue on 16-12-8.
//

#ifndef CHECKINGACCOUNTCLASS_ACCOUNT_H
#define CHECKINGACCOUNTCLASS_ACCOUNT_H

#include<string>

class Account {

private:
    int id;
    double balance;
    double annualInterestRate;
public:
    Account();
    virtual std::string toString();

public:
    int getId();

    void setId(int id);

    double getBalance();

    void setBalance(double balance);

    double getAnnualInterestRate();

    void setAnnualInterestRate(double annualInterestRate);
};



#endif //CHECKINGACCOUNTCLASS_ACCOUNT_H
```

### account.cpp

```
//
// Created by blue on 16-12-8.
//

#include "iostream"
#include "sstream"
#include "Account.h"

Account::Account() {
    Account::id = 0;
    Account::balance = 0;
    Account::annualInterestRate = 0;
}

int Account::getId() {
    return id;
}

void Account::setId(int id) {
    Account::id = id;
}

double Account::getBalance() {
    return balance;
}

void Account::setBalance(double balance) {
    Account::balance = balance;
}

double Account::getAnnualInterestRate() {
    return annualInterestRate;
}

void Account::setAnnualInterestRate(double annualInterestRate) {
    Account::annualInterestRate = annualInterestRate;
}

std::string Account:: toString(){

    std::ostringstream ostring;
    ostring <<  "Account:" << Account::getId() << " balance:" << Account::getBalance();

    return ostring.str();
};

```

### SavingsAccount.h

```
//
// Created by blue on 16-12-8.
//

#ifndef CHECKINGACCOUNTCLASS_SAVINGSACCOUNT_H
#define CHECKINGACCOUNTCLASS_SAVINGSACCOUNT_H
#include <string>
#include "Account.h"
class SavingsAccount : public Account {
private:
    bool allowedOverdraft;
    std::string createAt;
public:
    std::string toString() override;
    bool deposit(int id,double money);
    bool withdrawal(int id,double money);
public:
    SavingsAccount();
    const std::string &getCreateAt() const;
    bool isAllowedOverdraft() const;
};
#endif //CHECKINGACCOUNTCLASS_SAVINGSACCOUNT_H

```

### SavingsAccount.cpp

```
//
// Created by blue on 16-12-8.
//
#include "iostream"
#include "sstream"
#include "SavingsAccount.h"
#include "Date.h"
bool SavingsAccount::deposit(int id,double money){
    if (id == SavingsAccount::getId()){
        SavingsAccount::setBalance(SavingsAccount::getBalance()+money);
        return true;
    }
    return false;
}
bool SavingsAccount::withdrawal(int id,double money){
    if (id == SavingsAccount::getId()){
        if (SavingsAccount::getBalance() < money){
            return false;
        }else{
            SavingsAccount::setBalance(SavingsAccount::getBalance()-money);
            return true;
        }
    }
    return false;
}
const std::string &SavingsAccount::getCreateAt() const {
    return createAt;
}
SavingsAccount::SavingsAccount() {
    SavingsAccount::createAt = DateTime::currentTime();
}
bool SavingsAccount::isAllowedOverdraft() const {
    return allowedOverdraft;
}
std::string SavingsAccount:: toString(){
    std::ostringstream ostring;
    ostring <<  "Account:" << SavingsAccount::getId() << " balance:" << SavingsAccount::getBalance() << " CreateAt:" << SavingsAccount::getCreateAt();
    return ostring.str();
};
```

### CheckingAccount.h

```
//
// Created by blue on 16-12-8.
//

#ifndef CHECKINGACCOUNTCLASS_CHECKINGACCOUNT_H
#define CHECKINGACCOUNTCLASS_CHECKINGACCOUNT_H
#include "Account.h"
class CheckingAccount : public Account {
private:
    bool allowedOverdraft;
    double overdraft;
    std::string createAt;
public:
    std::string toString() override;
    bool deposit(int id,double money);
    bool withdrawal(int id,double money);
public:
    CheckingAccount();
    const std::string &getCreateAt() const;
    bool isAllowedOverdraft() const;
    double getOverdraft() const;
    void setOverdraft(double overdraft);
};
#endif //CHECKINGACCOUNTCLASS_CHECKINGACCOUNT_H

```

## CheckingAccount.cpp

```
//
// Created by blue on 16-12-8.
//

#include "iostream"
#include "sstream"
#include "CheckingAccount.h"
#include "Date.h"
bool CheckingAccount::deposit(int id,double money){
    if (id == CheckingAccount::getId()){
        CheckingAccount::setBalance(CheckingAccount::getBalance()+money);
        return true;
    }
    return false;
}
bool CheckingAccount::withdrawal(int id,double money){
    if (id == CheckingAccount::getId()){
        if (CheckingAccount::getBalance()-money <= (-CheckingAccount::getOverdraft())){
            return false;
        }else{
            CheckingAccount::setBalance(CheckingAccount::getBalance()-money);
            return true;
        }
    }
    return false;
}
const std::string &CheckingAccount::getCreateAt() const {
    return createAt;
}
CheckingAccount::CheckingAccount() {
    CheckingAccount::overdraft = 10000;
    CheckingAccount::createAt = DateTime::currentTime();
}
bool CheckingAccount::isAllowedOverdraft() const {
    return allowedOverdraft;
}
double CheckingAccount::getOverdraft() const {
    return CheckingAccount::overdraft;
}
void CheckingAccount::setOverdraft(double overdraft) {
    CheckingAccount::overdraft = overdraft;
}
std::string CheckingAccount:: toString(){
    std::ostringstream ostring;
    ostring << "Account:" << CheckingAccount::getId() << " balance:" << CheckingAccount::getBalance() << " CreateAt:" << CheckingAccount::getCreateAt();
    return ostring.str();
};
```

### Date.h

```
//
// Created by blue on 16-12-8.
//

#ifndef CHECKINGACCOUNTCLASS_DATE_H
#define CHECKINGACCOUNTCLASS_DATE_H
#include <ctime>
#include <string>
#include <type_traits>
class DateTime {
public:
    template<typename T>
    static std::string convert(time_t t) {
        return time2string(t);
    }

    template<typename T>
    static time_t convert(const std::string& timeStr) {
        return string2time(timeStr);
    }
    static std::string currentTime() {
        return time2string(time(nullptr));
    }
private:
    static std::string time2string(time_t t) {
        struct tm* tmNow = localtime(&t);
        char timeStr[sizeof("yyyy-mm-dd hh:mm:ss")] = {'\0'};
        std::strftime(timeStr, sizeof(timeStr), "%Y-%m-%d %H:%M:%S", tmNow);
        return timeStr;
    }
    static time_t string2time(const std::string& timeStr) {
        struct tm stTm;
        sscanf(timeStr.c_str(), "%d-%d-%d %d:%d:%d",
               &(stTm.tm_year),
               &(stTm.tm_mon),
               &(stTm.tm_mday),
               &(stTm.tm_hour),
               &(stTm.tm_min),
               &(stTm.tm_sec));
        stTm.tm_year -= 1900;
        stTm.tm_mon--;
        stTm.tm_isdst = -1;
        return mktime(&stTm);
    }
};
#endif //CHECKINGACCOUNTCLASS_DATE_H

```

### main.cpp

```
#include <iostream>
#include "CheckingAccount.h"
#include "SavingsAccount.h"

using namespace std;

int main() {

    SavingsAccount savingsAccount;
    CheckingAccount checkingAccount;

    cout << "SavingsAccount:" << savingsAccount.toString() << endl;
    cout << "CheckingAccount" << checkingAccount.toString() << endl;

    return 0;
}

```
