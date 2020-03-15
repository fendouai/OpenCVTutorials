# 写在前面的话

**OpenCV**是计算机视觉中经典的专用库，其支持多语言、跨平台，功能强大。**OpenCV-Python**为OpenCV提供了Python接口，使得使用者在Python中能够调用C/C++，在保证易读性和运行效率的前提下，实现所需的功能。

**OpenCV-Python Tutorials**是官方提供的文档，其内容全面、简单易懂，使得初学者能够快速上手使用。2014年段力辉在当时已翻译过OpenCV3.0，但时隔五年，如今的**OpenCV4.1**中许多函数和内容已经有所更新，因此有必要对该官方文档再进行一次翻译。

翻译过程中难免有所疏漏，如发现错误，希望大家指出，谢谢支持。

OpenCV-Python Tutorials官方文档：https://docs.opencv.org/4.1.2/d6/d00/tutorial_py_root.html

# 目录

### [OpenCV中文官方文档](http://www.woshicver.com/.)

*   <span class="caption-text">OpenCV简介</span>

    * [0_OpenCV-Python Tutorials](http://www.woshicver.com/FirstSection/0_OpenCV-Python%20Tutorials/)
    
*   <span class="caption-text">OpenCV安装</span>

    *   [1_1_OpenCV-Python教程简介](http://www.woshicver.com/SecondSection/1_1_OpenCV-Python教程简介/)
    *   [1_2_在Windows中安装OpenCV-Python](http://www.woshicver.com/SecondSection/1_2_在Windows中安装OpenCV-Python/)
    *   [1_3_在Fedora中安装OpenCV-Python](http://www.woshicver.com/SecondSection/1_3_在Fedora中安装OpenCV-Python/)
    *   [1_4_在Ubuntu中安装OpenCV-Python](http://www.woshicver.com/SecondSection/1_4_在Ubuntu中安装OpenCV-Python/)
    
*   <span class="caption-text">OpenCV中的GUI特性</span>

    *   [2_1_图像入门](http://www.woshicver.com/ThirdSection/2_1_图像入门/)
    *   [2_2_视频入门](http://www.woshicver.com/ThirdSection/2_2_视频入门/)
    *   [2_3_OpenCV中的绘图功能](http://www.woshicver.com/ThirdSection/2_3_OpenCV中的绘图功能/)
    *   [2_4_鼠标作为画笔](http://www.woshicver.com/ThirdSection/2_4_鼠标作为画笔/)
    *   [2_5_轨迹栏作为调色板](http://www.woshicver.com/ThirdSection/2_5_轨迹栏作为调色板/)
    
*   <span class="caption-text">核心操作</span>

    *   [3_1_图像的基本操作](http://www.woshicver.com/FourthSection/3_1_图像的基本操作/)
    *   [3_2_图像上的算法运算](http://www.woshicver.com/FourthSection/3_2_图像上的算法运算/)
    *   [3_3_性能衡量和提升技术](http://www.woshicver.com/FourthSection/3_3_性能衡量和提升技术/)
    
*   <span class="caption-text">OpenCV中的图像处理</span>

    *   [4_1_改变颜色空间](http://www.woshicver.com/FifthSection/4_1_改变颜色空间/)
    *   [4_2_图像几何变换](http://www.woshicver.com/FifthSection/4_2_图像几何变换/)
    *   [4_3_图像阈值](http://www.woshicver.com/FifthSection/4_3_图像阈值/)
    *   [4_4_图像平滑](http://www.woshicver.com/FifthSection/4_4_图像平滑/)
    *   [4_5_形态转换](http://www.woshicver.com/FifthSection/4_5_形态转换/)
    *   [4_6_图像梯度](http://www.woshicver.com/FifthSection/4_6_图像梯度/)
    *   [4_7_Canny边缘检测](http://www.woshicver.com/FifthSection/4_7_Canny边缘检测/)
    *   [4_8_图像金字塔](http://www.woshicver.com/FifthSection/4_8_图像金字塔/)
    *   [4_9_1_OpenCV中的轮廓](http://www.woshicver.com/FifthSection/4_9_1_OpenCV中的轮廓/)
    *   [4_9_2_轮廓特征](http://www.woshicver.com/FifthSection/4_9_2_轮廓特征/)
    *   [4_9_3_轮廓属性](http://www.woshicver.com/FifthSection/4_9_3_轮廓属性/)
    *   [4_9_4_轮廓：更多属性](http://www.woshicver.com/FifthSection/4_9_4_轮廓：更多属性/)
    *   [4_9_5_轮廓分层](http://www.woshicver.com/FifthSection/4_9_5_轮廓分层/)
    *   [4_10_1_直方图-1：查找，绘制，分析](http://www.woshicver.com/FifthSection/4_10_1_直方图-1：查找，绘制，分析/)
    *   [4_10_2_直方图-2：直方图均衡](http://www.woshicver.com/FifthSection/4_10_2_直方图-2：直方图均衡/)
    *   [4_10_3_直方图3：二维直方图](http://www.woshicver.com/FifthSection/4_10_3_直方图3：二维直方图/)
    *   [4_10_4_直方图-4：直方图反投影](http://www.woshicver.com/FifthSection/4_10_4_直方图-4：直方图反投影/)
    *   [4_11_傅里叶变换](http://www.woshicver.com/FifthSection/4_11_傅里叶变换/)
    *   [4_12_模板匹配](http://www.woshicver.com/FifthSection/4_12_模板匹配/)
    *   [4_13_霍夫线变换](http://www.woshicver.com/FifthSection/4_13_霍夫线变换/)
    *   [4_14_霍夫圈变换](http://www.woshicver.com/FifthSection/4_14_霍夫圈变换/)
    *   [4_15_图像分割与分水岭算法](http://www.woshicver.com/FifthSection/4_15_图像分割与分水岭算法/)
    *   [4_16_交互式前景提取使用GrabCut算法](http://www.woshicver.com/FifthSection/4_16_交互式前景提取使用GrabCut算法/)
    
*   <span class="caption-text">特征检测与描述</span>

    *   [5_1_理解特征](http://www.woshicver.com/Sixth/5_1_理解特征/)
    *   [5_2_哈里斯角检测](http://www.woshicver.com/Sixth/5_2_哈里斯角检测/)
    *   [5_3_Shi-Tomasi拐角探测器和良好的跟踪功能](http://www.woshicver.com/Sixth/5_3_Shi-Tomasi拐角探测器和良好的跟踪功能/)
    *   [5_4_SIFT（尺度不变特征变换）简介](http://www.woshicver.com/Sixth/5_4_SIFT（尺度不变特征变换）简介/)
    *   [5_5_SURF简介（加速的强大功能）](http://www.woshicver.com/Sixth/5_5_SURF简介（加速的强大功能）/)
    *   [5_6_用于角点检测的FAST算法](http://www.woshicver.com/Sixth/5_6_用于角点检测的FAST算法/)
    *   [5_7_BRIEF（二进制的鲁棒独立基本特征）](http://www.woshicver.com/Sixth/5_7_BRIEF（二进制的鲁棒独立基本特征）/)
    *   [5_8_ORB（定向快速和旋转简要）](http://www.woshicver.com/Sixth/5_8_ORB（定向快速和旋转简要）/)
    *   [5_9_特征匹配](http://www.woshicver.com/Sixth/5_9_特征匹配/)
    *   [5_10_特征匹配+单应性查找对象](http://www.woshicver.com/Sixth/5_10_特征匹配+单应性查找对象/)
    
*   <span class="caption-text">视频分析</span>

    *   [6_1_如何使用背景分离方法](http://www.woshicver.com/Seventh/6_1_如何使用背景分离方法/)
    *   [6_2_Meanshift和Camshift](http://www.woshicver.com/Seventh/6_2_Meanshift和Camshift/)
    *   [6_3_光流](http://www.woshicver.com/Seventh/6_3_光流/)
    
*   <span class="caption-text">相机校准和3D重建</span>

    *   [7_1_相机校准](http://www.woshicver.com/Eighth/7_1_相机校准/)
    *   [7_2_姿态估计](http://www.woshicver.com/Eighth/7_2_姿态估计/)
    *   [7_3_对极几何](http://www.woshicver.com/Eighth/7_3_对极几何/)
    *   [7_4_立体图像的深度图](http://www.woshicver.com/Eighth/7_4_立体图像的深度图/)
    
*   <span class="caption-text">机器学习</span>

    *   [8_1_理解KNN](http://www.woshicver.com/Ninth/8_1_理解KNN/)
    *   [8_2_使用OCR手写数据集运行KNN](http://www.woshicver.com/Ninth/8_2_使用OCR手写数据集运行KNN/)
    *   [8_3_理解SVM](http://www.woshicver.com/Ninth/8_3_理解SVM/)
    *   [8_4_使用OCR手写数据集运行SVM](http://www.woshicver.com/Ninth/8_4_使用OCR手写数据集运行SVM/)
    *   [8_5_理解K均值聚类](http://www.woshicver.com/Ninth/8_5_理解K均值聚类/)
    *   [8_6_OpenCV中的K均值](http://www.woshicver.com/Ninth/8_6_OpenCV中的K均值/)
    
*   <span class="caption-text">计算摄影学</span>

    *   [9_1_图像去噪](http://www.woshicver.com/Tenth/9_1_图像去噪/)
    *   [9_2_图像修补](http://www.woshicver.com/Tenth/9_2_图像修补/)
    *   [9_3_高动态范围](http://www.woshicver.com/Tenth/9_3_高动态范围/)
    
*   <span class="caption-text">目标检测</span>

    *   [10_1_级联分类器](http://www.woshicver.com/Eleventh/10_1_级联分类器/)
    *   [10_2_级联分类器训练](http://www.woshicver.com/Eleventh/10_2_级联分类器训练/)
    
*   <span class="caption-text">OpenCV-Python Binding</span>

    *   [11_1_OpenCV-Python Bindings](http://www.woshicver.com/Twelfth/11_1_OpenCV-Python%20Bindings/)

# 关于

OpenCV 中文官方文档

[http://woshicver.com/](http://woshicver.com/)
