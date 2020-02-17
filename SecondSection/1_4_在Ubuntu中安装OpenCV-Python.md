# 在Ubuntu中安装OpenCV-Python

###### 作者|OpenCV-Python Tutorials 
###### 编译|Vincent
###### 来源|OpenCV-Python Tutorials 
在本教程中，我们将学习在Ubuntu System中设置OpenCV-Python。以下步骤针对Ubuntu 16.04和18.04（均为64位）进行了测试。

可以通过两种方式在Ubuntu中安装OpenCV-Python：

- 从Ubuntu存储库中可用的预构建二进制文件安装
- 从源代码编译。在本节中，我们将同时看到两者。

另一个重要的事情是所需的其他库。OpenCV-Python仅需要**Numpy**（除了其他依赖关系，我们将在后面看到）。但是在本教程中，我们还使用**Matplotlib**进行一些简单而又漂亮的绘图目的（与OpenCV相比，我感觉好多了）。Matplotlib是可选的，但强烈建议使用。同样，我们还将看到**IPython**，这是一个强烈推荐的交互式Python终端。

### 从预构建的二进制文件安装OpenCV-Python

仅用于编程和开发OpenCV应用程序时，此方法最有效。

在终端（以root用户身份）中使用以下命令安装python-opencv:https://packages.ubuntu.com/trusty/python-opencv软件包。

`$ sudo apt-get install python-opencv`

打开Python IDLE（或IPython），然后在Python终端中键入以下代码。

```
import cv2 as cv
print(cv.__version__)
```

如果打印出来的结果没有任何错误，那就恭喜！你已经成功安装了OpenCV-Python。

这看起很容易，但也可能出现问题。Apt存储库不一定总是包含最新版本的OpenCV。例如，在编写本教程时，apt存储库包含2.4.8，而最新的OpenCV版本是3.x。关于Python API，最新版本将始终包含更好的支持和最新的错误修复。

因此，要获取最新的源代码，首选方法是从源代码进行编译。同样在某个时间点，如果你想为OpenCV做出贡献，则将通过这种方式。

### 从源代码构建OpenCV

首先，从源代码进行编译似乎有些复杂，但是一旦成功完成，就没有什么复杂的了。

首先，我们将安装一些依赖项。有些是必需的，有些是可选的。如果不想，可以跳过可选的依赖项。

#### 所需的构建依赖项

我们需要**CMake**来配置安装，需要**GCC**进行编译，需要**Python-devel**和**Numpy**来构建Python依赖项等。

```
sudo apt-get install cmake
sudo apt-get install gcc g++
```

支持python2：
`sudo apt-get install python-dev python-numpy`

支持python3：
`sudo apt-get install python3-dev python3-numpy`

接下来，我们需要GUI功能的**GTK**支持，相机支持（v4l），媒体支持（ffmpeg，gstreamer）等。

```
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
```

支持gtk2：
`sudo apt-get install libgtk2.0-dev`

支持gtk3：
`sudo apt-get install libgtk-3-dev`

#### 可选依赖项
以上依赖关系足以在你的Ubuntu计算机中安装OpenCV。但是根据你的需求，你可能需要一些额外的依赖项。此类可选依赖项的列表如下。你可以跳过或安装它，取决于你:)

OpenCV附带了用于图像格式（例如PNG，JPEG，JPEG2000，TIFF，WebP等）的支持文件。但是它可能有些旧。如果要获取最新的库，可以为这些格式的系统库安装开发文件。

```
sudo apt-get install libpng-dev
sudo apt-get install libjpeg-dev
sudo apt-get install libopenexr-dev
sudo apt-get install libtiff-dev
sudo apt-get install libwebp-dev
```

> 注意
  如果你使用的是Ubuntu 16.04，则还可以安装`libjasper-dev`以添加对JPEG2000格式的系统级别支持。

#### 下载OpenCV

要从OpenCV的GitHub Repository:https://github.com/opencv/opencv下载最新的源代码。 （如果你想为OpenCV做出贡献，请选择此项。为此，你需要先安装**Git**）

```
$ sudo apt-get install git
$ git clone https://github.com/opencv/opencv.git
```

它将在当前目录中创建一个文件夹"opencv"。下载可能需要一些时间，具体取决于你的Internet网络。

现在打开一个终端窗口，并导航到下载的"opencv"文件夹。创建一个新的"build"文件夹并导航到它。

```
$ mkdir build
$ cd build
```

#### 配置和安装

现在我们有了所有必需的依赖项，让我们安装OpenCV。必须使用CMake配置安装。它指定要安装的模块，安装路径，要使用的其他库，是否要编译的文档和示例等。大多数工作都是使用配置良好的默认参数自动完成的。

以下命令通常用于配置OpenCV库构建（从构建文件夹执行）：
`$ cmake ../`

OpenCV的默认默认设置为"Release"构建类型，安装路径为`/usr/local`。有关CMake选项的更多信息，请参考OpenCV **C++编译指南**:https://docs.opencv.org/4.1.2/d7/d9f/tutorial_linux_install.html

你应该在CMake输出中看到以下几行（它们意味着正确找到了Python）：

```
--   Python 2:
--     Interpreter:                 /usr/bin/python2.7 (ver 2.7.6)
--     Libraries:                   /usr/lib/x86_64-linux-gnu/libpython2.7.so (ver 2.7.6)
--     numpy:                       /usr/lib/python2.7/dist-packages/numpy/core/include (ver 1.8.2)
--     packages path:               lib/python2.7/dist-packages
--
--   Python 3:
--     Interpreter:                 /usr/bin/python3.4 (ver 3.4.3)
--     Libraries:                   /usr/lib/x86_64-linux-gnu/libpython3.4m.so (ver 3.4.3)
--     numpy:                       /usr/lib/python3/dist-packages/numpy/core/include (ver 1.8.2)
--     packages path:               lib/python3.4/dist-packages
```

现在，使用`make`命令构建文件，然后使用`make install`命令安装文件。

```
$ make
# sudo make install
```

安装结束。所有文件都安装在`/usr/local/`文件夹中。打开终端，然后尝试导入`cv2`。

```python
import cv2 as cv
print(cv.__version__)
```