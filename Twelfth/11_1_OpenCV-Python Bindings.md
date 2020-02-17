# OpenCV-Python Bindings 如何工作？   

作者|OpenCV-Python Tutorials  
编译|Vincent  
来源|OpenCV-Python Tutorials  

### 目标
了解：
- 如何生成OpenCV-Python bindings？
- 如何将新的OpenCV模块扩展到Python？

### OpenCV-Python bindings如何生成？

在OpenCV中，所有算法均以C ++实现。但是这些算法可以从不同的语言（例如Python，Java等）中使用。绑定生成器使这成为可能。这些生成器在C ++和Python之间建立了桥梁，使用户能够从Python调用C ++函数。为了全面了解后台发生的事情，需要对Python / C API有充分的了解。在官方Python文档中可以找到一个有关将C ++函数扩展到Python的简单示例[1]。因此，通过手动编写包装函数将OpenCV中的所有函数扩展到Python是一项耗时的任务。因此，OpenCV以更智能的方式进行操作。 OpenCV使用位于`modules/python/src2`中的一些Python脚本，从C ++头自动生成这些包装器函数。我们将调查他们的工作。

首先，`modules/python / CMakeFiles.txt`是一个CMake脚本，用于检查要扩展到Python的模块。它将自动检查所有要扩展的模块并获取其头文件。这些头文件包含该特定模块的所有类，函数，常量等的列表。

其次，将这些头文件传递到Python脚本`modules/python/src2/gen2.py`。这是Python Binding生成器脚本。它调用另一个Python脚本`module/python/src2/hdr_parser.py`。这是标头解析器脚本。此标头解析器将完整的标头文件拆分为较小的Python列表。因此，这些列表包含有关特定函数，类等的所有详细信息。例如，将对一个函数进行解析以获取一个包含函数名称，返回类型，输入参数，参数类型等的列表。最终列表包含所有函数，枚举的详细信息，头文件中的structs，classs等。

但是标头解析器不会解析标头文件中的所有函数/类。开发人员必须指定应将哪些函数导出到Python。为此，在这些声明的开头添加了某些宏，这些宏使标头解析器可以标识要解析的函数。这些宏由对特定功能进行编程的开发人员添加。简而言之，开发人员决定哪些功能应该扩展到Python，哪些不应该。这些宏的详细信息将在下一个会话中给出。

因此头解析器将返回已解析函数的最终大列表。我们的生成器脚本（gen2.py）将为标头解析器解析的所有函数/类/枚举/结构创建包装函数（你可以在编译期间在`build/modules/python/`文件夹中以pyopencv_genic_*.h文件找到这些标头文件）。但是可能会有一些基本的OpenCV数据类型，例如Mat，Vec4i，Size。它们需要手动扩展。例如，Mat类型应扩展为Numpy数组，Size应扩展为两个整数的元组，等等。类似地，可能会有一些复杂的结构/类/函数等需要手动扩展。所有这些手动包装函数都放在`modules/python/src2/cv2.cpp`中。

所以现在剩下的就是这些包装文件的编译了，这给了我们**cv2**模块。因此，当你调用函数时，例如在Python中说`res = equalizeHist(img1,img2)`，你将传递两个numpy数组，并期望另一个numpy数组作为输出。因此，将这些numpy数组转换为cv::Mat，然后在C++中调用**equalizeHist**()函数。最终结果将res转换回Numpy数组。简而言之，几乎所有操作都是在C++中完成的，这给了我们几乎与C++相同的速度。

因此，这是OpenCV-Python bindings生成方式的基本形式。

### 如何扩展新的模块到Python？
头解析器根据添加到函数声明中的一些包装宏来解析头文件。 枚举常量不需要任何包装宏。 它们会自动包装。 但是其余的函数，类等需要包装宏。

使用`CV_EXPORTS_W`宏扩展功能。 一个例子如下所示。

```
CV_EXPORTS_W void equalizeHist( InputArray src, OutputArray dst );
```

标头解析器可以理解诸如InputArray，OutputArray等关键字的输入和输出参数。但是有时，我们可能需要对输入和输出进行硬编码。 为此，使用了`CV_OUT`,`CV_IN_OUT`等宏。

```C
CV_EXPORTS_W void minEnclosingCircle( InputArray points,
                                     CV_OUT Point2f& center, CV_OUT float& radius );
```

对于大类，也使用`CV_EXPORTS_W`。为了扩展类方法，使用`CV_WRAP`。同样，`CV_PROP`用于类字段。

```C
class CV_EXPORTS_W CLAHE : public Algorithm
{
public:
    CV_WRAP virtual void apply(InputArray src, OutputArray dst) = 0;
    CV_WRAP virtual void setClipLimit(double clipLimit) = 0;
    CV_WRAP virtual double getClipLimit() const = 0;
}
```

可以使用`CV_EXPORTS_AS`扩展重载的函数。 但是我们需要传递一个新名称，以便在Python中使用该名称调用每个函数。 以下面的积分函数为例。 提供了三个函数，因此每个函数在Python中都带有一个后缀。 类似地，`CV_WRAP_AS`可用于包装重载方法。

```C
CV_EXPORTS_W void integral( InputArray src, OutputArray sum, int sdepth = -1 );
CV_EXPORTS_AS(integral2) void integral( InputArray src, OutputArray sum,
                                        OutputArray sqsum, int sdepth = -1, int sqdepth = -1 );
CV_EXPORTS_AS(integral3) void integral( InputArray src, OutputArray sum,
                                        OutputArray sqsum, OutputArray tilted,
                                        int sdepth = -1, int sqdepth = -1 );
```

小类/结构使用`CV_EXPORTS_W_SIMPLE`进行扩展。 这些结构按值传递给C ++函数。 示例包括`KeyPoint`,`Match`等。它们的方法由`CV_WRAP`扩展，而字段由`CV_PROP_RW`扩展。

```C
class CV_EXPORTS_W_SIMPLE DMatch
{
public:
    CV_WRAP DMatch();
    CV_WRAP DMatch(int _queryIdx, int _trainIdx, float _distance);
    CV_WRAP DMatch(int _queryIdx, int _trainIdx, int _imgIdx, float _distance);
    CV_PROP_RW int queryIdx; // query descriptor index
    CV_PROP_RW int trainIdx; // train descriptor index
    CV_PROP_RW int imgIdx;   // train image index
    CV_PROP_RW float distance;
};
```

可以使用`CV_EXPORTS_W_MAP`导出其他一些小类/结构，并将其导出到Python本机字典中。`Moments()`就是一个例子。

```C
class CV_EXPORTS_W_MAP Moments
{
public:
    CV_PROP_RW double  m00, m10, m01, m20, m11, m02, m30, m21, m12, m03;
    CV_PROP_RW double  mu20, mu11, mu02, mu30, mu21, mu12, mu03;
    CV_PROP_RW double  nu20, nu11, nu02, nu30, nu21, nu12, nu03;
};
```

因此，这些是OpenCV中可用的主要扩展宏。通常，开发人员必须将适当的宏放在适当的位置。其余的由生成器脚本完成。有时，在某些特殊情况下，生成器脚本无法创建包装。此类函数需要手动处理，为此，你需要编写自己的`pyopencv_*.hpp`扩展标头，并将其放入模块的misc / python子目录中。但是大多数时候，根据OpenCV编码指南编写的代码将由生成器脚本自动包装。

更高级的情况涉及为Python提供C ++接口中不存在的其他功能，例如额外的方法，类型映射或提供默认参数。稍后，我们将以`UMat`数据类型为例。首先，要提供特定于Python的方法，`CV_WRAP_PHANTOM`的用法与`CV_WRAP`相似，不同之处在于它以方法标头作为参数，并且你需要在自己的`pyopencv_*.hpp`扩展名中提供方法主体。 `UMat::queue()`和`UMat::context()`是此类幻象方法的示例，这些幻象方法在C++接口中不存在，但在Python端处理OpenCL功能时需要使用。其次，如果一个已经存在的数据类型可以映射到你的类，则最好使用`CV_WRAP_MAPPABLE`以源类型作为其参数来指示这种容量，而不是精心设计自己的绑定函数。从Mat映射的UMat就是这种情况。最后，如果需要默认参数，但本机C++接口中未提供，则可以在Python端将其作为`CV_WRAP_DEFAULT`的参数提供。按照下面的`UMat::getMat`示例：

```C
class CV_EXPORTS_W UMat
{
public:
    // 你需要提供 `static bool cv_mappable_to(const Ptr<Mat>& src, Ptr<UMat>& dst)`
    CV_WRAP_MAPPABLE(Ptr<Mat>);
    /! returns the OpenCL queue used by OpenCV UMat.
    // 你需要在资料夹代码中提供方法主体
    CV_WRAP_PHANTOM(static void* queue());
    // 你需要在资料夹代码中提供方法主体
    CV_WRAP_PHANTOM(static void* context());
    CV_WRAP_AS(get) Mat getMat(int flags CV_WRAP_DEFAULT(ACCESS_RW)) const;
};  
```