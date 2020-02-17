# 在Windows中安装OpenCV-Python

###### 作者|OpenCV-Python Tutorials 
###### 编译|Vincent
###### 来源|OpenCV-Python Tutorials 

### 目标

在本教程中

- 我们将学习在你的Windows系统中设置OpenCV-Python。

下面的步骤在装有Visual Studio 2010和Visual Studio 2012的Windows 7-64位计算机上进行了测试。屏幕截图展示的是VS2012。

### 从预编译的二进制文件安装OpenCV

1. 下面的Python软件包将被下载并安装到其默认位置。

   1. Python的3.X(3.4+)或Python 2.7.x从这里下载：https://www.python.org/downloads/。
   2. Numpy包(例如使用`pip install numpy`命令下载)。
   3. Matplotlib( `pip install matplotlib`)(*Matplotlib是可选的，但推荐它，因为我们使用了很多在我们的教程*)。

2. 将所有软件包安装到其默认位置。`C:/Python27/`如果使用Python 2.7，将安装Python。

3. 安装后，打开Python IDLE。输入**import numpy**并确保Numpy运行正常。

4. 从GitHub：https://github.com/opencv/opencv/releases 或SourceForge网站：https://sourceforge.net/projects/opencvlibrary/files/ 下载最新的OpenCV版本，然后双击将其解压缩。

5. 转到**opencv/build/python/2.7**文件夹。

6. 将**cv2.pyd**复制到**C:/Python27/lib/site-packages**。

7. 打开Python IDLE，然后在Python终端中键入以下代码。

```python
>>> import cv2 as cv
>>> print( cv.__version__ )
```

如果打印出来的结果没有任何错误，那就恭喜！你已经成功安装了OpenCV-Python。

### 从源代码构建OpenCV

1. 下载并安装Visual Studio和CMake。

   1. Visual Studio 2012:http://go.microsoft.com/?linkid=9816768
   2. CMake:https://cmake.org/download/

2. 将必要的Python软件包下载并安装到其默认位置

   1. Python
   2. Numpy

   > 注意
     在这种情况下，我们使用的是32位Python软件包二进制文件。但是，如果要将OpenCV用于x64，则将安装Python软件包的64位二进制文件。问题在于，没有Numpy的官方64位二进制文件。你必须自行构建。为此，你必须使用与构建Python相同的编译器。启动Python IDLE时，它会显示编译器详细信息。你可以在此处：http://stackoverflow.com/q/2676763/1134940 获得更多信息。因此，你的系统必须具有相同的Visual Studio版本并从源代码构建Numpy。

     拥有64位Python软件包的另一种方法是使用来自第三方(如Anaconda：http://www.continuum.io/downloads、 Enthought：https://www.enthought.com/downloads/)等现成Python发行版。它的大小会更大，但可以满足你的所有需求。一切都在一个外壳中。你也可以下载32位版本。

3. 确保Python和Numpy正常运行。

4. 下载OpenCV源代码。它可以来自Sourceforge:http://sourceforge.net/projects/opencvlibrary/(官方发行版)或来自Github:https://github.com/opencv/opencv (最新源)。

5. 将其解压缩到一个文件夹中，在opencv中创建一个新的文件夹。

6. 打开CMake-gui(*Start>All Programs> CMake-gui*)

7. 如下填写字段(请参见下图)：

   a. 单击**Browse Source**然后找到opencv文件夹。

   b. 单击**Browse Build**然后找到我们创建的构建文件夹。

   c. 点击**Configure**。

    ![ ](http://qiniu.aihubs.net/Capture1.jpg)

   d. 它将打开一个新窗口以选择编译器。选择适当的编译器(此处为Visual Studio 11)，然后单击**Finish**。
   ![ ](http://qiniu.aihubs.net/why.png)
   e. 等待分析完成。

8. 你将看到所有字段都标记为红色。单击**WITH**字段将其展开。它决定了你需要哪些额外的功能。因此，请标记适当的字段。见下图：
  
    ![ ](http://qiniu.aihubs.net/Capture3.png)

9. 现在，单击**BUILD**字段以将其展开。前几个字段配置构建方法。见下图：

    ![ ](http://qiniu.aihubs.net/Capture5.png)

10. 其余字段指定要构建的模块。由于OpenCV-Python尚不支持GPU模块，因此可以完全避免使用它以节省时间(但是如果使用它们，则将其保留在此处)。见下图：

    ![ ](http://qiniu.aihubs.net/Capture6.png)

11. 现在单击 **ENABLE**字段将其展开。确保未选中**ENABLE_SOLUTION_FOLDERS**(Visual Studio Express版本不支持解决方案文件夹)。见下图：

    ![ ](http://qiniu.aihubs.net/Capture7.png)

12. 还要确保在**PYTHON**字段中，所有内容都已填充。(忽略PYTHON_DEBUG_LIBRARY)。见下图：

    ![ ](http://qiniu.aihubs.net/Capture80.png)

13. 最后，单击**Generate**按钮。

14. 现在转到我们的**opencv / build**文件夹。在那里你将找到**OpenCV.sln**文件。用Visual Studio打开它。

15. 将构建模式检查为**Release**而不是**Debug**。

16. 在解决方案资源管理器中，右键单击**Solution**(或**ALL_BUILD**)并进行构建。需要一些时间才能完成。

17. 再次，右键单击**INSTALL**并进行构建。现在将安装OpenCV-Python。

    ![ ](http://qiniu.aihubs.net/Capture8.png)

18. 打开Python IDLE，然后输入`import cv2 as cv`。如果没有错误，则说明已正确安装。

> 注意
  我们没有安装其他支持如TBB、Eigen、Qt、Documentation等。在这里很难解释清楚。我们将添加更详细的视频，或者你可以随意修改。

### 其他资源

### 练习题

如果你有Windows计算机，请从源代码编译OpenCV。做各种各样极客。如果遇到任何问题，请访问OpenCV论坛并解释你的问题。