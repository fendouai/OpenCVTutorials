# BRIEF(二进制的鲁棒独立基本特征)

###### 作者|OpenCV-Python Tutorials
###### 编译|Vincent
###### 来源|OpenCV-Python Tutorials 

### 目标

在本章中，
- 我们将看到BRIEF算法的基础知识

### 理论

我们知道SIFT使用128维矢量作为描述符。由于它使用浮点数，因此基本上需要512个字节。同样，SURF最少也需要256个字节（用于64像素）。为数千个功能部件创建这样的向量会占用大量内存，这对于资源受限的应用程序尤其是嵌入式系统而言是不可行的。内存越大，匹配所需的时间越长。

但是实际匹配可能不需要所有这些尺寸。我们可以使用PCA，LDA等几种方法对其进行压缩。甚至使用LSH（局部敏感哈希）进行哈希的其他方法也可以将这些SIFT描述符中的浮点数转换为二进制字符串。这些二进制字符串用于使用汉明距离匹配要素。这提供了更快的速度，因为查找汉明距离仅是应用XOR和位数，这在具有SSE指令的现代CPU中非常快。但是在这里，我们需要先找到描述符，然后才可以应用散列，这不能解决我们最初的内存问题。

现在介绍BRIEF。它提供了一种直接查找二进制字符串而无需查找描述符的快捷方式。它需要平滑的图像补丁，并以独特的方式（在纸上展示）选择一组$n_d(x，y)$位置对。然后，在这些位置对上进行一些像素强度比较。例如，令第一位置对为$p$和$q$。如果$I(p)<I(q)$，则结果为1，否则为0。将其应用于所有$n_d$个位置对以获得$n_d$维位串。

该$n_d$可以是128、256或512。OpenCV支持所有这些，但默认情况下将是256（OpenCV以字节为单位表示，因此值将为16、32和64）。因此，一旦获得此信息，就可以使用汉明距离来匹配这些描述符。

重要的一点是，BRIEF是特征描述符，它不提供任何查找特征的方法。因此，您将不得不使用任何其他特征检测器，例如SIFT，SURF等。本文建议使用CenSurE，它是一种快速检测器，并且BIM对于CenSurE点的工作原理甚至比对SURF点的工作要好一些。

简而言之，BRIEF是一种更快的方法特征描述符计算和匹配。除了平面内旋转较大的情况，它将提供很高的识别率。

### OpenCV中的BRIEF

下面的代码显示了借助CenSurE检测器对Brief描述符的计算。（在OpenCV中，CenSurE检测器称为STAR检测器）注意，您需要使用opencv contrib）才能使用它。

```python
import numpy as np
import cv2 as cv
from matplotlib import pyplot as plt
img = cv.imread('simple.jpg',0)
# 初始化FAST检测器
star = cv.xfeatures2d.StarDetector_create()
# 初始化BRIEF提取器
brief = cv.xfeatures2d.BriefDescriptorExtractor_create()
# 找到STAR的关键点
kp = star.detect(img,None)
# 计算BRIEF的描述符
kp, des = brief.compute(img, kp)
print( brief.descriptorSize() )
print( des.shape )
```

函数`brief.getDescriptorSize()`给出以字节为单位的$n_d$大小。默认情况下为32。下一个是匹配项，这将在另一章中进行。

### 附加资源

1. Michael Calonder, Vincent Lepetit, Christoph Strecha, and Pascal Fua, "BRIEF: Binary Robust Independent Elementary Features", 11th European Conference on Computer Vision (ECCV), Heraklion, Crete. LNCS Springer, September 2010.
2. [LSH (Locality Sensitive Hashing)](https://en.wikipedia.org/wiki/Locality-sensitive_hashing) at wikipedia.