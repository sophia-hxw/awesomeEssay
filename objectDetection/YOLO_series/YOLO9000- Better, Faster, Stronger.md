俗称"YOLOv2"

## 问题背景
- YOLOv1 检测性能低    
- 当前的检测任务受数据集标签的限制    
  数据集必须有标签或通过分类赋予标签，但是，标记检测图像比标记分类图像昂贵得多，所以检测数据和分类数据不是一个规模。

## 创新点
- 针对第一个问题，使用一些方法提升 YOLOv1 的性能，得到 YOLOv2。
- 针对第二个问题，提出了 ImageNet 和 COCO 数据集的结合方法，以及联合训练方法，训练 YOLOv2 后得到的模型叫 YOLO9000。
- 能检测超过9000种类别的物体；
- 相比v1提高了训练图像的分辨率；

## 提升性能的方法

- Accuracy    
  Batch Normalization, High Resolution Classifier, Convolutional With Anchor Boxes,  Direct location prediction, Fine-Grained Features, Multi-Scale Training
- Speed    
  提出一个新网络 Darknet-19

![yolov2_tricks](https://github.com/sophia-hxw/awesomeEssay/blob/main/objectDetection/YOLO_series/assets/yolov2_tricks.png)

## 训练流程

论文提出了一种联合训练算法，该算法可以在检测和分类数据上训练目标检测器。 利用标记的检测图像来学习精准定位，同时使用分类图像来增加其“词汇量”和健壮性。

### 分类检测数据集结合方法：
检测数据集的标签少且普通，分类数据集的标签多且具体，如果我们想在两个数据集上训练，就得把它们的标签合并起来。很多分类方法都用一个 softmax layer ，但它的前提是假设所有类互斥，但我们的数据集类别是不都是互斥的（有可能是包含关系，例如狗和金毛犬），所以我们使用了一个多标签模型来组合数据集（无互斥的要求），及使用多个 softmax 。 大多数分类方法都假定标签采用扁平结构，但是对于组合数据集我们需要层次化的结构。

ImageNet 标签采用有向图结构。在这里，作者把数据集的结构简化为结构树（hierarchical tree）。通过改造图，最后得到一个 WordTree，这样每个节点/标签都有自己的概率，解决了类别之间不互斥的问题，就能在检测集和分类集上联合训练。

### 联合训练方法：
把检测和分类数据混合，训练过程中遇到带标签的检测图像，就基于 YOLOv2 整个损失函数进行反向传播，遇到分类图像，只反向传播网络的分类损失。

## 结果
相比YOLOv1，YOLO9000在识别种类、精度、速度、和定位准确性等方面都有大大提升。

## 对比YOLOv1的改进
- 1.Anchor: 引入了Faster R-CNN中使用的Anchor，通过在所有训练图像的所有边界框上运行k-means聚类来选择锚的个数和形状(k = 5，因此它找到五个最常见的目标形状) 
- 2. 修改了网络结构，去掉了全连接层，改成了全卷积结构。
- 3. 使用Batch Normalization可以从model中去掉Dropout，而不会产生过拟合。
- 4. 训练时引入了世界树（WordTree）结构，将检测和分类问题做成了一个统一的框架，并且提出了一种层次性联合训练方法，将ImageNet分类数据集和COCO检测数据集同时对模型训练。



