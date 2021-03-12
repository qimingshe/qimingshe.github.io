---
layout:     post
title:      Android CTS问题分析思路

date:       2021-03-12
author:     白夜
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - CTS问题
---

1、CTS（兼容性测试套件Compatibility Test Suite）
https://source.android.com/compatibility
CTS是一套自动化测试套件，其目的是尽早发现不兼容性，确保软件在整个开发过程中保持兼容性。测试内容包括：签名测试、平台API测试（核心库和Android 应用框架）、Dalvik测试、平台Intent、平台权限、平台资源。
测试套件下载地址：
https://source.android.com/compatibility/cts/downloads
测试命令：
https://source.android.com/compatibility/cts/command-console-v2

注意事项
1.1、安装Cts Verifier后，请手动授予CtsVerifier所有的权限，Android R请安装JDK 11

2、CDD（兼容性定义文档），代表兼容性的“政策”方面
本文档列举了设备必须满足哪些要求才能与最新版本的Android兼容
https://source.android.com/compatibility/cdd

3、GTS（GMS测试套件Google Mobile Service Test Suite）
Google移动服务（GMS）是Google提供的应用程序和服务的集合，它运行在Android应用程序框架之上。GMS测试套件（GTS）是一个自动化测试套件，用于验证GMS应用程序是否已正确集成，同意的合同条款保持是否与google一致。GTS使用Tradefed测试工具，类似于兼容性测试套件（CTS）一样。

注意事项
3.1 、GMS包和对应GTS测试工具包都是google不开源，无源码的。GMS测试fail主要对比Google原生机器的测试结果参考，看fail的log提示，或者反编译测试apk进行分析。

4、BTS（构建测试套件Build Test Suite）
MBA（Mobile Bundle Apps）安全漏洞政策
对于违反MBA安全漏洞政策的应用，构建测试套件（BTS）发出WARN（警告）。合作伙伴必须在披露之日起90天内解决此问题。如果问题仍未解决，则状态会在90天后自动变为ALERT（警报），并导致构建批准被阻止。当BTS发出WARN（警告）时，除了Android合作伙伴批准（APA）中的消息外，还会为您分配一个错误，以通知您违规行为。

5、VTS（供应商测试套件 Vendor Test Suite）
供应商测试套件（VTS）会自动执行HAL和操作系统内核测试。要使用VTS测试Android原生系统实现，请设置一个测试环境，然后使用VTS方案来测试相应补丁程序。

6、GSI（Generic System Image）
GSI可视为一种“纯Android”实现，采用未经修改的Android开源项目（AOSP）代码，在任何运行Android8.1或以上版本的Android设备上都可以顺利运行。GSI用于运行VTS和CTS-on-GSI测试。为确保运行最新版Android的设备正确实现供应商接口，您需要将Android设备的系统映像替换为GSI，然后使用供应商测试套件（VTS）和兼容性测试套件（CTS）来测试设备。R上的GSI测试是在CTS工具下测试，Q上是用VTS工具测试，GSI测试需要刷google GSI,VTS跟GSI的区别是VTS需要刷boot-debug.img,需要root权限。

7、CTS测试结果分析
7.1 一份报告一般有result和log目录，根据报告的result目录，查看test_result.html、test_result_failures_suite.html，查看测试fail项，搜索项目源码，查看fail项报告的原因。如果测试工具更新了，对应的测试项也更新了，行号对应不上，此时可以查看源码网站：
https://android.googlesource.com/platform/cts
https://cs.android.com/
例如：https://android.googlesource.com/platform/cts/+/refs/tags/android-cts-11.0_r2/tests/tests/permission/src/android/permission/cts/RemovePermissionTest.java

7.2 如果缺少log，请环境编译，CTS测试源码添加log，编译apk替换原有APK进行分析

7.3 实在搞不定，请确认google原生机器pixel是否也会fail，如果也有问题可以找google寻求帮助。如果定位是google测试工具问题或是GMS包的问题，也可以找google寻求帮助。

8. CTS运行小技巧

加快执行速度
run cts-dev 或者 加 -d-o
run gts  -d - o

涉及到gms问题最常见方式卸载更新或者更新到最新版本来排除问题
手动更新 google services
adb shell am start -a android.intent.action.VIEW -d 'market://details?id=com.google.android.gms' -p 'com.android.vending'
