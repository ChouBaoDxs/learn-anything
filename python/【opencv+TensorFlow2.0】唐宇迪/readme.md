---
title: 【opencv+TensorFlow2.0】这才是我想要的实战教程！
date: 2022-08-14 00:00:00

tags: 
- Python
- 机器学习  

categories: 
- Python
- 机器学习
---

:::warning
视频名称：【opencv+TensorFlow2.0】这才是我想要的实战教程！  
视频地址：https://www.bilibili.com/video/BV1234y1j7D1  
作者：[MIC博士带你学AI](https://space.bilibili.com/691348450)  
我的评价：★★★☆☆  
备注：图片和各种原始数据不放到 git 仓库里
:::

<!-- more -->
[TOC]

## P1 1-OpenCV简介 02:37
课程安排
- 通俗讲解知识点，项目实战驱动
- 零基础到进阶，熟悉Python即可
- 环境配置: Jupyter notebook+IDE, Opencv-Python
- 提供所有数据与代码，追随热点持续更新

## P2 2-Python与Opencv配置安装 10:12
Python： 推荐直接 Anaconda

opencv： pip install opencv-python==3.4.1.15

opencv 扩展： pip install opencv-contrib-python==3.4.1.15

## P3 2-Notebook与IDE环境 11:44
第三方 python 依赖库 whl 下载网站（适用于 windows）：https://www.lfd.uci.edu/~gohlke/pythonlibs/

浏览器启动 jupyter： 终端执行 jupyter notebook，访问 http://localhost:8888/

## P4 1-计算机眼中的图像 09:20
- cv2.IMREAD_COLOR：彩色图像
- cv2.IMREAD_GRAYSCALE：灰度图像
```python
import cv2 # opencv读取的格式是 BGR
import matplotlib.pyplot as plt
import numpy as np 
# jupyter 语法，省略掉plt.show()，直接内嵌图片
%matplotlib inline

img = cv2.imread('cat.jpg')

#图像的显示,也可以创建多个窗口
cv2.imshow('image',img) 
# 等待时间，毫秒级，0表示任意键终止
cv2.waitKey(0) 
cv2.destroyAllWindows()

def cv_show(name,img): # 封装函数
    cv2.imshow(name,img) 
    cv2.waitKey(0) 
    cv2.destroyAllWindows()

img = cv2.imread('cat.jpg',cv2.IMREAD_GRAYSCALE) # 读取灰度图像
img.shape # (414, 500, 3)

cv2.imwrite('mycat.png',img) # 保存
```
## P5 2-视频的读取与处理 10:57
```python
import cv2

vc = cv2.VideoCapture('test.mp4')

# 检查是否打开正确
if vc.isOpened(): 
    open, frame = vc.read() # frame 为一帧图像
else:
    open = False

while open:
    ret, frame = vc.read()
    if frame is None:
        break
    if ret == True:
        gray = cv2.cvtColor(frame,  cv2.COLOR_BGR2GRAY) # 灰度图
        cv2.imshow('result', gray)
        if cv2.waitKey(100) & 0xFF == 27: # 按下 ESC 退出键就退出循环
            break
vc.release()
cv2.destroyAllWindows()
```

## P6 3-ROI区域 04:19
截取部分图像数据
```python
img = cv2.imread('cat.jpg')
cat = img[0:50, 0:200] 
cv_show('cat',cat)
```

颜色通道提取
```python
b,g,r = cv2.split(img)
r.shape # (414, 500)
img = cv2.merge((b,g,r))
img.shape # (414, 500, 3)

# 只保留R
cur_img = img.copy()
cur_img[:,:,0] = 0
cur_img[:,:,1] = 0
cv_show('R',cur_img)

# 只保留G
cur_img = img.copy()
cur_img[:,:,0] = 0
cur_img[:,:,2] = 0
cv_show('G',cur_img)

# 只保留B
cur_img = img.copy()
cur_img[:,:,1] = 0
cur_img[:,:,2] = 0
cv_show('B',cur_img)
```

## P7 4-边界填充 05:08
```python
top_size,bottom_size,left_size,right_size = (50,50,50,50)

replicate = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, borderType=cv2.BORDER_REPLICATE)
reflect = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size,cv2.BORDER_REFLECT)
reflect101 = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, cv2.BORDER_REFLECT_101)
wrap = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, cv2.BORDER_WRAP)
constant = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size,cv2.BORDER_CONSTANT, value=0)

import matplotlib.pyplot as plt
plt.subplot(231), plt.imshow(img, 'gray'), plt.title('ORIGINAL')
plt.subplot(232), plt.imshow(replicate, 'gray'), plt.title('REPLICATE')
plt.subplot(233), plt.imshow(reflect, 'gray'), plt.title('REFLECT')
plt.subplot(234), plt.imshow(reflect101, 'gray'), plt.title('REFLECT_101')
plt.subplot(235), plt.imshow(wrap, 'gray'), plt.title('WRAP')
plt.subplot(236), plt.imshow(constant, 'gray'), plt.title('CONSTANT')

plt.show()
```
- BORDER_REPLICATE：复制法，也就是复制最边缘像素。
- BORDER_REFLECT：反射法，对感兴趣的图像中的像素在两边进行复制例如：fedcba|abcdefgh|hgfedcb
- BORDER_REFLECT_101：反射法，也就是以最边缘像素为轴，对称，gfedcb|abcdefgh|gfedcba
- BORDER_WRAP：外包装法cdefgh|abcdefgh|abcdefg
- BORDER_CONSTANT：常量法，常数值填充。

## P8 5-数值计算 09:18
数值计算
```python
img_cat=cv2.imread('cat.jpg')
img_dog=cv2.imread('dog.jpg')

img_cat2= img_cat + 10 # 每个颜色值都 + 10
img_cat[:5,:,0]

#相当于 % 256
(img_cat + img_cat2)[:5,:,0] # 超过 256 的话会直接减掉 256

cv2.add(img_cat,img_cat2)[:5,:,0] # 上限为 255
```

图像融合：尺寸必须相同
```python
img_dog = cv2.resize(img_dog, (500, 414))

res = cv2.addWeighted(img_cat, 0.4, img_dog, 0.6, 0)
plt.imshow(res)
```

图像拉伸：
```python
res = cv2.resize(img, (0, 0), fx=4, fy=4) # 宽高各 4 倍
plt.imshow(res)

res = cv2.resize(img, (0, 0), fx=1, fy=3) # 宽 1 倍，高 3 倍
plt.imshow(res)
```

## P9 图像阈值 07:52
## P10 1-图像平滑处理 07:55
## P11 2-高斯与中值滤波 06:15
## P12 1-腐蚀操作 06:50
## P13 2-膨胀操作 03:06
## P14 3-开运算与闭运算 02:56
## P15 4-梯度计算 02:45
## P16 5-礼帽与黑帽 03:22
## P17 1-Sobel算子 09:34
## P18 2-梯度计算方法 08:33
## P19 3-scharr与lapkacian算子 06:42
## P20 1-Canny边缘检测流程 05:39
## P21 2-非极大值抑制 05:25
## P22 3-边缘检测效果 08:10
## P23 1-图像金字塔定义 06:38
## P24 2-金字塔制作方法 07:26
## P25 1-轮廓检测方法 06:01
## P26 2-轮廓检测结果 07:49
## P27 3-轮廓特征与近似 11:58
## P28 1-模板匹配方法 11:13
## P29 2-匹配效果展示 05:48
## P30 1-直方图定义 07:19
## P31 2-均衡化原理 09:39
## P32 3-均衡化效果 06:52
## P33 1-傅里叶概述 07:20
## P34 2-频域变换结果 07:09
## P35 3-低通与高通滤波 06:48
## P36 总体流程与方法讲解 09:15
## P37 2-环境配置与预处理 08:27
## P38 3-模板处理方法 06:56
## P39 4-输入数据处理方法 08:54
## P40 5-模板匹配得出识别结果 10:59
## P41 1-整体流程演示 05:34
## P42 2-文档轮廓提取 08:43
## P43 3-原始与变换坐标计算 07:45
## P44 4-透视变换结果 08:40
## P45 5-tesseract-ocr安装配置 07:08
## P46 6-文档扫描识别效果 05:22
## P47 1-角点检测基本原理 05:45
## P48 2-基本数学原理 10:14
## P49 3-求解化简 10:10
## P50 4-特征归属划分 10:54
## P51 5-opencv角点检测效果 06:11
## P52 1-尺度空间定义 05:58
## P53 2-高斯差分金字塔 06:22
## P54 3-特征关键点定位 14:08
## P55 4-生成特征描述 07:03
## P56 5-特征向量生成 09:22
## P57 6-opencv中sift函数使用 08:11
## P58 1-特征匹配方法 08:32
## P59 2-RANSAC算法 09:41
## P60 2-图像拼接方法 10:03
## P61 4-流程解读 05:15
## P62 1-任务整体流程 07:24
## P63 2-所需数据介绍 05:50
## P64 3-图像数据预处理 08:55
## P65 4-车位直线检测 12:01
## P66 5-按列划分区域 11:04
## P67 6-车位区域划分 11:03
## P68 7-识别模型构建 06:41
## P69 8-基于视频的车位检测 09:24
## P70 1-整体流程与效果概述 06:56
## P71 2-预处理操作 07:04
## P72 3-填涂轮廓检测 07:39
## P73 4-选项判断识别 10:19
## P74 1-背景消除-帧差法 07:08
## P75 2-混合高斯模型 06:44
## P76 3-学习步骤 07:23
## P77 4-背景建模实战 06:51
## P78 0-课程简介 01:49
## P79 1-Tensorflow2版本简介与心得 10:29
## P80 2-Tensorflow2版本安装方法 10:06
## P81 3-tf基础操作 07:55
## P82 1-深度学习要解决的问题 07:56
## P83 2-深度学习应用领域 14:07
## P84 3-计算机视觉任务 05:49
## P85 4-视觉任务中遇到的问题 10:02
## P86 5-得分函数 07:15
## P87 6-损失函数的作用 10:43
## P88 7-前向传播整体流程 13:46
## P89 8-返向传播计算方法 09:34
## P90 9-神经网络整体架构 10:53
## P91 10-神经网络架构细节 10:55
## P92 11-神经元个数对结果的影响 07:12
## P93 12-正则化与激活函数 08:50
## P94 13-神经网络过拟合解决方法 11:07
## P95 1-任务目标与数据集简介 07:25
## P96 2-建模流程与API文档 06:49
## P97 3-网络模型训练 07:49
## P98 4-模型超参数调节与预测结果展示 11:12
## P99 5-分类模型构建 09:29
## P100 6-tf.data模块解读 08:15
## P101 7-模型保存与读取实例 10:45
## P102 1-卷积神经网络应用领域 07:26
## P103 2-卷积的作用 09:24
## P104 3-卷积特征值计算方法 08:08
## P105 4-得到特征图表示 07:00
## P106 5-步长与卷积核大小对结果的影响 08:12
## P107 6-边缘填充方法 06:31
## P108 7-特征图尺寸计算与参数共享 07:03
## P109 8-池化层的作用 05:39
## P110 9-整体网络架构 06:21
## P111 10-VGG网络架构 06:17
## P112 11-残差网络Resnet 07:42
## P113 12-感受野的作用 05:47
## P114 1-猫狗识别任务与数据简介 05:39
## P115 2-卷积网络涉及参数解读 06:33
## P116 3-网络架构配置 08:28
## P117 4-卷积模型训练与识别效果展示 11:37
## P118 1-数据增强概述 11:42
## P119 2-图像数据变换 15:34
## P120 3-猫狗识别任务数据增强实例 05:47
## P121 1-迁移学习的目标 05:33
## P122 2-迁移学习策略 07:12
## P123 3-Resnet原理 11:55
## P124 3-加载训练好的经典网络模型 09:17
## P125 4-Callback模块与迁移学习实例 11:18
## P126 1-tfrecords数据源制作方法 10:39
## P127 2-图像数据处理实例 10:01
## P128 1-RNN网络架构解读 11:28
## P129 1-词向量模型通俗解释 08:15
## P130 2-模型整体框架 10:10
## P131 3-训练数据构建 05:11
## P132 4-CBOW与Skip-gram模型 08:21
## P133 5-负采样方案 07:41
## P134 1-任务流程解读 06:30
## P135 2-模型定义参数设置 06:06
## P136 3-文本词预处理操作 05:30
## P137 4-训练batch数据制作 11:16
## P138 5-损失函数定义与训练结果展示 08:22
## P139 1-任务目标与数据介绍 05:07
## P140 2-RNN模型输入数据维度解读 07:00
## P141 3-数据映射表制作 10:12
## P142 4-embedding层向量制作 10:27
## P143 5-数据生成器构造 09:56
## P144 6-双向RNN模型定义 06:26
## P145 7-自定义网络模型架构 11:00
## P146 8-训练策略指定 06:07
## P147 9-训练文本分类模型 07:42
## P148 1-CNN应用于文本任务原理解析 10:47
## P149 2-整体流程解读 07:11
## P150 3-网络架构设计与训练 11:20
## P151 1-任务目标与数据源 06:21
## P152 2-构建时间序列数据 07:16
## P153 3-训练时间序列数据预测结果 09:52
## P154 4-多特征预测结果 07:41
## P155 5-序列结果预测 04:31
## P156 13-额外补充-Resnet论文解读 11:48
## P157 14-额外补充-Resnet网络架构解读 08:27
## P158 3-项目结构概述 04:32
## P159 4-数据集处理方法 05:57
## P160 5-训练数据构建 05:52
## P161 6-网络架构层次解读 08:42
## P162 7-前向传播配置 08:10
## P163 8-训练resnet模型 07:27