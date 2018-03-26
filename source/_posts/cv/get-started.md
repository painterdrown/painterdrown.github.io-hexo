---
title: 计算机视觉学习笔记：起步
date: 2018-03-27 00:18:00
tags: [cv]
---

## 前言

今晚实验室老师开会，说打算开始带我和另外一位同学做 CV 以及神经网络。为了更系统地学习和记忆，接下来我打以博客的形式记录下学习过程及心得等等。

## 参考资料

> [吴恩达 (Andrew Ng) 关于机器学习的公开课](http://open.163.com/special/opencourse/machinelearning.html)

> [王晓刚（香港中文大学）教授关于深度学习的课程](http://www.ee.cuhk.edu.hk/~xgwang/)

## 安装 OpenCV

师兄提到玩计算机视觉必定要碰 OpenCV，所以这里讲一下我的安装过程。由于我用的是 macOS，比较方便的做法是：Xcode + OpenCV 的环境。

1. 使用 brew 安装：`brew install opencv`

2. 安装完之后，可以看到以下目录及文件：

  + /usr/local/Cellar/opencv
  + /usr/local/include
  + /usr/local/lib

3. Xcode 新建命令行程序项目，在工程文件下进行以下设置：

  + Build Phases -> Link Binary With Libraries 添加 /usr/local/Cellar/opencv/<版本>/lib 中的所有 .dylib 文件

  ![Build Phases](images/setting-1.png)

  + Build Settings -> Search Paths 添加 /usr/local/include 和 /usr/local/lib

  ![Build Settings](images/setting-2.png)

4. 配置完毕，在 cpp 文件直接：`#include "opencv2/opencv.hpp"`，测试代码：

  ```C++
  #include <iostream>
  #include "opencv2/opencv.hpp"
  using namespace cv;

  int main(int argc, const char * argv[]) {
      Mat image;
      image = imread("test.jpeg");  // 这里换成图片的绝对路径
      namedWindow("Hello OpenCV!", WINDOW_AUTOSIZE);
      imshow("Hello OpenCV!", image);
      waitKey(0);
      return 0;
  }
  ```
