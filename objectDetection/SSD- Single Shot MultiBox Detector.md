# 基本思想
SSD网络的特点是对不同尺度下的feature map中的每一个点都设置一些default box,这些default box有不同的大小和横纵比例，对这些default box进行分类和边框回归的操作。SSD的核心是对固定设置的default box（不同尺度feature map中每一个空间位置都设置一组default box，这里只考虑空间位置，不考虑feature的通道个数）计算属于各类物体的概率以及坐标调整的数值。这个计算方式是对每层的feature map做卷积操作，卷积核设定为3*3，卷积核的个数是与default box个数相关。


# 优点
SSD的优点是运行速度超过yolo，精度在一定条件下超过faster rcnn。缺点是需要人工设置先验框（prior box）和min_size，max_size和长宽比（aspect_ratio）值，网络中default_box的基础大小和形状不能直接通过学习获得，而是需要手工设置，虽然使用了图像金字塔的思路，但是对小目标的recall（召回率）依然一般。

# 前向传播
- 1，数据增强，获取训练样本，将训练数据统一resize到固定尺寸；
- 2.通过卷积网络获取feature map：
   - ①使用的卷积网络，前半部分使用基础分类网络获取各层的feature map，这部分称为base network。
   - ②下一步计算的输入，就是上述的不同尺寸的feature map；
- 3.通过卷积操作，从特征图中获取检测信息。
   - ①此处实现方式与yolo类似；
   - ②与Faster R-CNN类似，在特征图中每个点新建若干固定尺寸的anchor。

检测信息包括每个anchor的信息。主要包括：confidence（代表这个anchor中是否存在物体）、分类信息以及bbox信息。


# 缺点和改进
对小目标的检测效果一般。

改进：
- 1. 增大输入尺寸
- 2. 使用更低的特征图做检测(比如S3FD中使用更低的conv3_3检测)
- 3. FPN(已经是检测网络的标配了)




