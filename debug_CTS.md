---
title: Use CTS
category: AndroidDebug
tags: CTS
---

CTS
===

## Overview

CTS就是兼容性测试为了确保Android应用能够在所有兼容Android的设备上正确运行，并且保持相似的用户体验，在每个版本发布之时，Android提供了一套兼容性测试用例集合（Compatibility Test Suite, CTS）来认证运行Android系统的设备是否完全兼容Android规范，并附带有相关的兼容性标准文档（Compatibility Definition Document, CDD）。  

从 http://source.android.com/compatibility/downloads.html（ 网络需要能进google）处下载最新的兼容性测试用例集合，并解压。大部分是基于Junit和仪表盘技术编写的。还扩展了自动化测试过程，可以自动执行用例，自动收集和汇总测试结果。CTS采用XML配置文件的方式将这些测试用例分组成多个测试计划（plan）,第三方也可以创建自己的plan。

## Achieve CTS Package
1. google官方下载  
Link: http://source.android.com/compatibility/downloads.html
2. 编译源码  
```
CTS的代码在Android源码CTS目录下。  
make cts -j4  
编译好cts后生成的文件位置： /out/host/linux-x86/  
在该目录下包含tools和rtestcases两个目录。
```

## 环境配置
1. Android 调试桥 (adb)
2. 手机不能休眠
```
设置->开发人员选项：
打开Settings->Accessibility->Developer options->USB debugging(USB 调试)
打开Settings->Accessibility->Developer options->Stay Awake（保持唤醒）
```
3. 系统语言最好保证英语
```
设置手机语言为英语：Setting->Language&input->language->English(United States)。
```

## 测试命令
1. 在CTS/tools目录下执行  
> ./cts-tradefed  进入cts-tf,进入的打印信息会提示连接到手机设备！
```
run  cts   --help   查看帮助
help list
help run
```
2. 测试模块  
> run cts -s DeviceID -m CtsMediaTestCases  （在testcases目录下找apk）  
> 测试模块下的一小模块： cts-tf > run cts -m CtsMediaTestCases -a  armeabi-v7a

3. 测试模块某个testcase
> run cts -s DeviceID -m <模块名> -t <具体fail项> 
```
run  cts   -
兼容性（compatibility）选项：
--include-filter     	请求包含的模块过滤器

--exclude-filter        请求排出的模块过滤器（默认一大堆）

--subplan  				运行子计划

--serial/-s <deviceID>	在特定设备上运行 CTS

-d/--device   <devicedID> 		设备

-r /--retry			重新尝试当前的session

--skip-system-status-check    +  ...Check     跳过系统状态检查

-–skip-preconditions/-o	 跳过一些预置条件的检查，可以减少测试的时间

–-skip-device-info 跳过手机信息的收集，可以减少测试的时间

--continue-session  <sessionId>    继续被中断的任务
```
4. 查看测试结果
> l r

5. 查看等待执行的任务
> l c

6. 查看所有执行的任务
> l i
```
l  -
d	设备
r	结果
c	当前被等待执行的命令
p	所有可获得的plan
m	所有可获得的module
i	 所有调用的线程（查看任务是否结束）
config	所有可知的配置
```
7. 全局测试
> run cts –plan CTS

8. 继续被中断的测试
> l r  
> run cts –continue-session sessionID

9. 重新跑测某个任务的失败项（节约时间）
> l r  
> run cts retry -s DeviceID --retry sessionID

## 测试结果
测试结束后在/results文件夹中，会看到以日期和时间命名的文件夹用于保存执行过的测试结果。还有一个同名的zip文件保存同样的内容。

测试过程中的自动录log，测试结束后log自动保存在/logs里边以日期和时间命名的文件夹中。

在测试结果文件夹中，所有的测试结果是以XML的形式保存的。  
通常测试结果网页分成“Device Information”、“Test Summary”、“Test Summary by Package”、“Test Failures(xx)”和“Detailed Test Report”等四个区域。