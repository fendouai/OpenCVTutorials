# OpenCV-Python教程简介

###### 作者|OpenCV-Python Tutorials 
###### 编译|Vincent
###### 来源|OpenCV-Python Tutorials 

### OpenCV

OpenCV由**Gary Bradsky**于1999年在英特尔创立，第一版于2000年问世。**Vadim Pisarevsky**加入Gary Bradsky，一起管理英特尔的俄罗斯软件OpenCV团队。2005年，OpenCV用于Stanley，该车赢得了2005年DARPA挑战赛的冠军。后来，在Willow Garage的支持下，它的积极发展得以继续，由Gary Bradsky和Vadim Pisarevsky领导了该项目。OpenCV现在支持与计算机视觉和机器学习有关的多种算法，并且正在日益扩展。

OpenCV支持多种编程语言，例如C++、Python、Java等，并且可在Windows、Linux、OS X、Android和iOS等不同平台上使用。基于CUDA和OpenCL的高速GPU操作的接口也正在积极开发中。

OpenCV-Python是用于OpenCV的Python API，结合了OpenCV C++ API和Python语言的最佳特性。

### OpenCV-Python

OpenCV-Python是旨在解决计算机视觉问题的Python专用库。

Python是由**Guido van Rossum**发起的通用编程语言，很快就非常流行，主要是因为它的简单性和代码可读性。它使程序员可以用较少的代码行表达想法，而不会降低可读性。

与C/C++之类的语言相比，Python速度较慢。也就是说，可以使用C/C++轻松扩展Python，这使我们能够用C/C++编写计算密集型代码并创建可用作Python模块的Python包装器。这给我们带来了两个好处：首先，代码与原始C/C++代码一样快（因为它是在后台运行的实际C++代码），其次，在Python中比C/C++编写代码更容易。OpenCV-Python是原始OpenCV C++实现的Python包装器。

OpenCV-Python利用了**Numpy**，这是一个高度优化的库，用于使用MATLAB样式的语法进行数值运算。所有OpenCV数组结构都与Numpy数组相互转换。这也使与使用Numpy的其他库（例如SciPy和Matplotlib）的集成变得更加容易。
 

### OpenCV-Python教程

OpenCV引入了一组新的教程，它们将指导您完成OpenCV-Python中可用的各种功能。**本指南主要针对OpenCV 3.x版本**（尽管大多数教程也适用于OpenCV 2.x）。

建议先了解Python和Numpy，因为本指南将不介绍它们。**要使用OpenCV-Python编写优化的代码，必须先明白Numpy。**

本教程最初由*Abid Rahman K.*在*Alexander Mordvintsev*的指导下作为Google Summer of Code 2013计划的一部分*启动*。

### OpenCV需要您！

由于OpenCV是开放源代码计划，因此欢迎所有人为这个库，文档和教程做出贡献。如果您在本教程中发现任何错误（从小的拼写错误到代码或概念中的严重错误），请随时通过在GitHub中:https://github.com/opencv/opencv 克隆OpenCV 并提交请求请求来更正它。OpenCV开发人员将检查您的请求请求，给您重要的反馈，并且（一旦通过审阅者的批准）它将被合并到OpenCV中。然后，您将成为开源贡献者:-)

随着新模块添加到OpenCV-Python中，本教程将不得不进行扩展。如果您熟悉特定的算法，并且可以编写一个包括算法基本理论和显示示例用法的代码的教程，欢迎你这样做。

记住，我们可以**共同**使这个项目取得巨大成功！

### 贡献者

以下是向OpenCV-Python提交了教程的贡献者列表。

1. Alexander Mordvintsev（GSoC-2013 导师）
2. Abid Rahman K.（GSoC-2013 实习生）

### 其他资源

1. Python快速指南- [一小部分Python]：http://swaroopch.com/notes/python/
2. 基本的Numpy教程：http://wiki.scipy.org/Tentative_NumPy_Tutorial
3. numpy示例列表：http://wiki.scipy.org/Numpy_Example_List
4. OpenCV文档：http://docs.opencv.org/
5. OpenCV论坛：http://answers.opencv.org/questions/