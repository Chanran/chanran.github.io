---
title: 写electron遇到的坑
date: 2016-12-08 00:37:28
tags: [electron,javascript,nodejs]
categories: [nodejs]
---

# electron进程之间的通信

## 渲染进程和主进程之间的通信

1. 渲染进程和主进程之间通过ipc通信，渲染进程使用的是ipcRenderer,而主进程使用的是ipcMain。

## 渲染进程和渲染进程之间的通信

1. 渲染进程和渲染进程之间的通信：渲染进程1->主进程->渲染进程2。

## 进程之间数据共享：IPC，HTML5 API(Storage API,IndexedDB)等

**注意不要犯的错**:比如在进程1中改变过了ES6类1的静态属性，如果在进程2中require了这个类1，那么进程2只能得到进程1改变类1的静态属性之前的值。

- IPC通信:可以使用Electron中提供的IPC系统。它在主进程中将一个对象储存为全局变量，然后可以通过remote模块操作它们
- HTML5 API
