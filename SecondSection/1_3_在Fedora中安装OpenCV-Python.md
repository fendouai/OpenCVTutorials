# 在Fedora中安装OpenCV-Python

###### 作者|OpenCV-Python Tutorials 
###### 编译|Vincent
###### 来源|OpenCV-Python Tutorials 

### 目标

在本教程中

- 我们将学习在你的Fedora系统中设置OpenCV-Python。针对Fedora 18（64位）和Fedora 19（32位）进行以下步骤。

### 介绍

可以通过两种方式在Fedora中安装OpenCV-Python：1）从fedora存储库中可用的预构建二进制文件安装，2）从源代码进行编译。在本节中，我们将同时看到这两种方法。

另一个重要的事情是所需的其他库。OpenCV-Python仅需要**Numpy**（除了其他依赖关系，我们将在后面看到）。但是在本教程中，我们还使用**Matplotlib**进行一些简单而又漂亮的作图（与OpenCV相比，感觉好多了）。Matplotlib是可选的，但强烈建议安装。同样，我们还将看到**IPython**，这是一个强烈推荐的交互式Python终端。

### 从预构建的二进制文件安装OpenCV-Python

以root用户身份在终端中使用以下命令安装所有软件包。

`$ yum install numpy opencv *`

打开Python IDLE（或IPython），然后在Python终端中键入以下代码。

```python
>>> import cv2 as cv
>>> print( cv.__version__ )
```

如果打印出来的结果没有任何错误，那就恭喜！你已经成功安装了OpenCV-Python。

这很简单。但是这里有一个问题。Yum仓库可能不总是包含最新版本的 OpenCV。例如，在撰写本教程时，yum 库包含2.4.5，而最新的 OpenCV 版本是2.4.6。对于 Python API，最新版本总是包含更好的支持。另外，取决于所使用的驱动程序、ffmpeg、gstreamer软件包等，相机支持，视频播放等可能会出现问题。

所以我个人的偏好是下一种方法，即从源代码编译。在某个时候，如果你想为OpenCV 做贡献，你也需要这个。

### 从源代码安装OpenCV

从源代码编译起初可能看起来有点复杂，但是一旦你成功了，就没有什么复杂的了。

首先，我们将安装一些依赖项。有些是强制性的，有些是可选的。可选的依赖项，如果不需要，可以跳过。

#### 强制依赖

我们需要**CMake**来配置安装，**GCC**进行编译，**Python-devel**和**Numpy**来创建Python扩展等。

```
yum install cmake
yum install python-devel numpy
yum install gcc gcc-c++
```

接下来，我们需要**GTK**对GUI功能的支持，相机支持(libdc1394，v4l)，媒体支持(ffmpeg，gstreamer)等。

```
yum install gtk2-devel
yum install libdc1394-devel
yum install ffmpeg-devel
yum install gstreamer-plugins-base-devel
```

#### 可选依赖项

以上依赖关系足以在你的fedora计算机中安装OpenCV。但是根据你的要求，你可能需要一些额外的依赖项。此类可选依赖项的列表如下。你可以跳过或安装它，取决于你:)

OpenCV附带了用于图像格式（例如PNG，JPEG，JPEG2000，TIFF，WebP等）的支持文件。但是它可能有些旧。如果要获取最新的库，可以安装这些格式的开发文件。

```
yum install libpng-devel
yum install libjpeg-turbo-devel
yum install jasper-devel
yum install openexr-devel
yum install libtiff-devel
yum install libwebp-devel
```

几个OpenCV功能与**英特尔的线程构建模块**（TBB）并行。但是，如果要启用它，则需要先安装TBB。（同样在使用CMake配置安装时，请不要忘记设置`-D WITH_TBB = ON`。下面更多详细信息。）

```
yum install tbb-devel
```

OpenCV使用另一个**Eigen**库来优化数学运算。因此，如果你的系统中装有Eigen，则可以利用它。（同样在使用CMake配置安装时，请不要忘记设置`WITH_EIGEN = ON`。下面更多详细信息。）

```
yum install eigen3-devel
```

如果你要构建**文档**（*是的，你可以使用完整的搜索功能以HTML格式在系统中创建OpenCV完整官方文档的脱机版本，这样，如果有任何问题，你就不必总是访问Internet，而且非常快捷！！！*），你需要安装**Doxygen**（文档生成工具）。

```
yum install doxygen
```

### 下载OpenCV

接下来，我们必须下载OpenCV。你可以从sourceforge网站：http://sourceforge.net/projects/opencvlibrary/ 下载最新版本的OpenCV 。然后解压缩文件夹。

或者，你可以从OpenCV的github存储库下载最新的源代码。（如果你想为OpenCV做出贡献，请选择此项。它始终使你的OpenCV保持最新状态）。为此，你需要先安装**Git**。

```
yum install git
git clone https://github.com/opencv/opencv.git
```

它将在主目录（或你指定的目录）中创建一个文件夹OpenCV。克隆可能需要一些时间，具体取决于你的Internet网络。

现在打开一个终端窗口，然后导航到下载的OpenCV文件夹。创建一个新的构建文件夹并导航到它。

```
mkdir build
cd build
```

### 配置和安装

现在，我们已经安装了所有必需的依赖项，让我们安装OpenCV。必须使用CMake配置安装。它指定要安装的模块，安装路径，要使用的其他库，是否要编译的文档和示例等。下面的命令通常用于配置（从build文件夹执行）。

`cmake -D CMAKE_BUILD_TYPE = RELEASE -D CMAKE_INSTALL_PREFIX = / usr / local ..`

它指定构建类型为“发布模式”，安装路径为/usr/local。在每个选项之前标志`-D`，在最后观察标志`..`。简而言之，这是一种格式：

`cmake [-D <flag>] [-D <flag>] ..`

你可以指定任意数量的标志，但是每个标志前面应带有-D。

因此，在本教程中，我们将安装具有TBB和Eigen支持的OpenCV。我们还构建了文档，但是不包括性能测试和构建示例。我们还会禁用与GPU相关的模块（因为我们使用的是OpenCV-Python，因此我们不需要与GPU相关的模块。这为我们节省了一些时间）。

*（以下所有命令都可以在单个cmake语句中完成，但为了便于理解，此处将其拆分。）*

- 启用TBB和Eigen支持：

  `cmake -D WITH_TBB=ON -D WITH_EIGEN=ON ..`

- 启用文档并禁用测试和示例

  `cmake -D BUILD_DOCS=ON -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=OFF ..`

- 禁用所有与GPU相关的模块。

  `cmake -D WITH_OPENCL=OFF -D BUILD_opencv_gpu=OFF -D BUILD_opencv_gpuarithm=OFF -D BUILD_opencv_gpubgsegm=OFF -D BUILD_opencv_gpucodec=OFF -D BUILD_opencv_gpufeatures2d=OFF -D BUILD_opencv_gpufilters=OFF -D BUILD_opencv_gpuimgproc=OFF -D BUILD_opencv_gpulegacy=OFF -D BUILD_opencv_gpuoptflow=OFF -D BUILD_opencv_gpustereo=OFF -D BUILD_opencv_gpuwarping=OFF ..`

- 设置安装路径和构建类型

  `cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..`

  每次输入cmake语句时，它都会打印出结果配置设置。在完成的最终设置中，请确保填写以下字段（以下是我获得的一些重要配置）。这些字段也应在你的系统中适当填写。否则将会发生一些问题。因此，请检查你是否正确执行了上述步骤。

```
...
--   GUI:
--     GTK+ 2.x:                    YES (ver 2.24.19)
--     GThread :                    YES (ver 2.36.3)
--   Video I/O:
--     DC1394 2.x:                  YES (ver 2.2.0)
--     FFMPEG:                      YES
--       codec:                     YES (ver 54.92.100)
--       format:                    YES (ver 54.63.104)
--       util:                      YES (ver 52.18.100)
--       swscale:                   YES (ver 2.2.100)
--       gentoo-style:              YES
--     GStreamer:
--       base:                      YES (ver 0.10.36)
--       video:                     YES (ver 0.10.36)
--       app:                       YES (ver 0.10.36)
--       riff:                      YES (ver 0.10.36)
--       pbutils:                   YES (ver 0.10.36)
--     V4L/V4L2:                    Using libv4l (ver 1.0.0)
--   Other third-party libraries:
--     Use Eigen:                   YES (ver 3.1.4)
--     Use TBB:                     YES (ver 4.0 interface 6004)
--   Python:
--     Interpreter:                 /usr/bin/python2 (ver 2.7.5)
--     Libraries:                   /lib/libpython2.7.so (ver 2.7.5)
--     numpy:                       /usr/lib/python2.7/site-packages/numpy/core/include (ver 1.7.1)
--     packages path:               lib/python2.7/site-packages
...
```

还有许多其他标志和设置。它留给你以作进一步的探索。

现在，你可以使用`make`命令构建文件，并使用`make install`命令进行安装。`make install`应该以`root`身份执行。

```
make
su
make install
```

安装结束。所有文件都安装在/usr/local/文件夹中。但是要使用它，你的Python应该能够找到OpenCV模块。你有两个选择。

1. **将模块移动到Python路径中的任何文件夹**：可以通过在Python终端中输入`import sys; print(sys.path)`来找到Python路径。它将打印出许多位置。将/usr/local/lib/python2.7/site-packages/cv2.so移至该文件夹中的任何一个。例如，
   `su mv /usr/local/lib/python2.7/site-packages/cv2.so /usr/lib/python2.7/site-packages`
   但是，每次安装OpenCV时都必须这样做。

2. **将/usr/local/lib/python2.7/site-packages添加到PYTHON_PATH**：只需执行一次。只需打开/.bashrc并向其添加以下行，然后注销并返回即可。
   `export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages`
   至此，OpenCV安装完成。打开终端，然后尝试`import cv2 as cv`。

要构建文档，只需输入以下命令：

`make doxygen`

然后打开opencv/build/doc/doxygen/html/index.html并将其添加到浏览器中。

### 其他资源

### 练习题

1. 在Fedora系统的机器中采用源代码编译OpenCV。