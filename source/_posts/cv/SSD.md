---
title: 我和 SSD 的亲密接触
date: 2018-05-21 08:22:22
tags: [cv]
---

> 官方源码：https://github.com/weiliu89/caffe/tree/ssd

## 装 caffe

实验室的服务器（Ubuntu 16.04）已经装好了 CUDA, cuDNN, OpenCV 的环境，所以现在只需要编译一下 Caffe 就可以跑。

> 官方安装教程：https://github.com/weiliu89/caffe/tree/ssd#installation

跟着这个教程走完 **Installation** 和 **Preparation**。

> 我的 [Makefile.config](Makefile.config) & [Makefile](Makefile)

PS: 由于在编译的时候遇到了一个关于 `hdf5` 和 `undefined reference to「boost::re_detail::raise_runtime_error(std::runtime_error const&)」` 的错误，需要修改下 [Makefile](Makefile):

```sh
# 原来版本：LIBRARIES += glog gflags protobuf boost_system boost_filesystem boost_regex m hdf5_hl hdf5
LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial boost_thread stdc++ boost_regex
```

我这边 `make py` 的结果：

![make py](images/make_py.png)

## 训练 & 评估

### 1. Train & Evaluate(顺便)

```sh
# time: 08:48
python examples/ssd/ssd_pascal.py
```

开始跑：

![train_1](images/train_1.png)

![train_1](images/train_2.png)

以及 GPU 的状态如下（zhengzhao 是我）：

![gpustat](images/gpustat.png)

刚开始用 `tmux`，不小心关掉了窗口，看不到输出，只能等它跑完了...这是 `ps` 查到的进程状态（先是通过 ps）：

```sh
# 找到相应进程的 ID（其实已经包含线程状态了）
ps aux
...

# 查看进程的状态
ps <pid>
  PID TTY      STAT   TIME COMMAND
31938 ?        Sl   385:59 ./build/tools/caffe train --solver=models/VGGNet/VOC0712/SSD_300x300/solver.prototxt --weights=models/VGGNet/VGG_ILSVRC_16_layers_fc_reduced.caffemodel --gpu 0,1,2,3
```

这里顺便复习下 Linux 的进程状态吧 hhh：

+ `D` 不可中断 Uninterruptible sleep(usually IO)
+ `R` 正在运行，或在队列中的进程
+ `S` 处于休眠状态
+ `T` 停止或被追踪
+ `Z` 僵尸进程
+ `W` 进入内存交换（从内核 2.6 开始无效）
+ `X` 死掉的进程

进程状态的修饰：

+ `<` 高优先级
+ `N` 低优先级
+ `L` 有些页被锁进内存
+ `s` 包含子进程
+ `+` 位于后台的进程组
+ `l` 多线程，克隆线程

再顺便贴上 `tmux` 的常用操作：

+ `tmux new -s <name>` 新建会话并取个名字。
+ `tmux ls` 查看所有会话。
+ `tmux a -t <name>` 进入某个会话。
+ `tmux kill-session -t <name>` 终止某个会话。
+ `Ctrl + B + D` 退出某个会话（仍在后台）。
+ `Ctrl + B + S` 切换到另外的会话。

我观察了一下，跑 200 次迭代需要 123 秒。这次训练需要 120k 次迭代，大概要跑 20 个小时......漫长等待之后，可以在 `$HOME/data/VOCdevkit/results/VOC2007/SSD_300x300/` 看到运行的结果：

![训练结果](images/results.png)

训练出来的模型在 `$CAFFE_ROOT/models/VGGNet/VOC0712/SSD_300x300/`:

![训练模型](images/models.png)

还有这次训练的其它 job file, log file, python script 在 `$CAFFE_ROOT/jobs/VGGNet/VOC0712/SSD_300x300/`:

![训练 stuff](images/stuff.png)

### 2. Evaluate

> It should reach 77.* mAP at 120k iterations.

官方说能达到 77 的 mAP，现在来测试一下。

```sh
python examples/ssd/score_ssd_pascal.py
```

输出如下：

![mAP](images/mAP.png)

可以看到是有 76.99 的 mAP，没有 77 有点小失望。

### 3. Test Using a Webcam

这一步就算了，我这里没有网络摄像头。

### 4. Test

> Check out examples/ssd_detect.ipynb or examples/ssd/ssd_detect.cpp on how to detect objects using a SSD model. Check out examples/ssd/plot_detections.py on how to plot detection results output by ssd_detect.cpp.

这里是在说有 Jupyter 文档说明如何用 SSD 来做目标检测以及如何圈出检测结果。

### 5. Train on Other Dataset

> To train on other dataset, please refer to data/OTHERDATASET for more details. We currently add support for COCO and ILSVRC2016. We recommend using examples/ssd.ipynb to check whether the new dataset is prepared correctly.
