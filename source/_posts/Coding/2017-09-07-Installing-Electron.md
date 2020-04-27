---
date: 2017-09-07 00:00:00
title: "Windows 10 x64安装Electron"
category: "Coding"
tag: ["Electron"]
---

## 安装Node.js ##

从[官网](https://nodejs.org/en/download/)下载适合自己系统的安装包并安装。安装完成house可以使用`npm -v`命令查看node.js版本号，确认其是否正常安装。

使用node -v命令检查node.js版本，确认node安装；

## 安装cnpm工具 ##

从官方npm下载速度较慢，可以使用淘宝定制的命令行工具cnpm代替默认的npm。安装命令为`npm install -g cnpm --registry=https://registry.npm.taobao.org`

## 安装Electron ##

安装命令为`cnpm install -g electron`。
