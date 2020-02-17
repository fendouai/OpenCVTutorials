使用OCR手写数据集运行KNN

作者|OpenCV-Python Tutorials
编译|Vincent
来源|OpenCV-Python Tutorials 

### 目标
在本章中
- 我们将使用我们在kNN上的知识来构建基本的OCR应用程序。
- 我们将尝试使用OpenCV自带的数字和字母数据集。

### 手写数字的OCR
我们的目标是构建一个可以读取手写数字的应用程序。为此，我们需要一些`train_data`和`test_data`。OpenCV带有一个图片`digits.png`（在文件夹`opencv/samples/data/`中），其中包含`5000`个手写数字（每个数字500个）。每个数字都是`20x20`的图像。因此，我们的第一步是将图像分割成`5000`个不同的数字。对于每个数字，我们将其展平为`400`像素的一行。那就是我们的训练集，即所有像素的强度值。这是我们可以创建的最简单的功能集。我们将每个数字的前`250`个样本用作`train_data`，然后将`250`个样本用作`test_data`。因此，让我们先准备它们。

```python
import numpy as np
import cv2 as cv
img = cv.imread('digits.png')
gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
# 现在我们将图像分割为5000个单元格，每个单元格为20x20
cells = [np.hsplit(row,100) for row in np.vsplit(gray,50)]
# 使其成为一个Numpy数组。它的大小将是（50,100,20,20）
x = np.array(cells)
# 现在我们准备train_data和test_data。
train = x[:,:50].reshape(-1,400).astype(np.float32) # Size = (2500,400)
test = x[:,50:100].reshape(-1,400).astype(np.float32) # Size = (2500,400)
# 为训练和测试数据创建标签
k = np.arange(10)
train_labels = np.repeat(k,250)[:,np.newaxis]
test_labels = train_labels.copy()
# 初始化kNN，训练数据，然后使用k = 1的测试数据对其进行测试
knn = cv.ml.KNearest_create()
knn.train(train, cv.ml.ROW_SAMPLE, train_labels)
ret,result,neighbours,dist = knn.findNearest(test,k=5)
# 现在，我们检查分类的准确性
#为此，将结果与test_labels进行比较，并检查哪个错误
matches = result==test_labels
correct = np.count_nonzero(matches)
accuracy = correct*100.0/result.size
print( accuracy )
```

因此，我们的基本OCR应用程序已准备就绪。这个特定的例子给我的准确性是91％。一种提高准确性的选择是添加更多数据进行训练，尤其是错误的数据。因此，与其每次启动应用程序时都找不到该培训数据，不如将其保存，以便下次我直接从文件中读取此数据并开始分类。您可以借助一些Numpy函数（例如np.savetxt，np.savez，np.load等）来完成此操作。请查看其文档以获取更多详细信息。

```python
# 保存数据
np.savez('knn_data.npz',train=train, train_labels=train_labels)
# 现在加载数据
with np.load('knn_data.npz') as data:
    print( data.files )
    train = data['train']
    train_labels = data['train_labels']
```

在我的系统中，它需要大约`4.4 MB`的内存。由于我们使用强度值（uint8数据）作为特征，因此最好先将数据转换为`np.uint8`，然后再将其保存。在这种情况下，仅占用`1.1 MB`。然后在加载时，您可以转换回`float32`。

### 英文字母的OCR
接下来，我们将对英语字母执行相同的操作，但是数据和功能集会稍有变化。在这里，OpenCV代替了图像，而在`opencv/samples/cpp/`文件夹中附带了一个数据文件`letter-recognitiontion.data`。如果打开它，您将看到20000行，乍一看可能看起来像垃圾。实际上，在每一行中，第一列是一个字母，这是我们的标签。接下来的16个数字是它的不同功能。这些功能是从UCI机器学习存储库获得的。您可以在此页面中找到这些功能的详细信息。
现有20000个样本，因此我们将前10000个数据作为训练样本，其余10000个作为测试样本。我们应该将字母更改为ASCII字符，因为我们不能直接使用字母。

```python
import cv2 as cv
import numpy as np
# 加载数据，转换器将字母转换为数字
data= np.loadtxt('letter-recognition.data', dtype= 'float32', delimiter = ',',
                    converters= {0: lambda ch: ord(ch)-ord('A')})
# 将数据分为两个，每个10000个以进行训练和测试
train, test = np.vsplit(data,2)
# 将火车数据和测试数据拆分为特征和响应
responses, trainData = np.hsplit(train,[1])
labels, testData = np.hsplit(test,[1])
# 初始化kNN, 分类, 测量准确性
knn = cv.ml.KNearest_create()
knn.train(trainData, cv.ml.ROW_SAMPLE, responses)
ret, result, neighbours, dist = knn.findNearest(testData, k=5)
correct = np.count_nonzero(result == labels)
accuracy = correct*100.0/10000
print( accuracy )
```

它给我的准确性为`93.22%`。同样，如果要提高准确性，则可以迭代地在每个级别中添加错误数据。

### 附加资源
### 练习