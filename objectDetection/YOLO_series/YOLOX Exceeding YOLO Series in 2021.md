





# 主要贡献
在YOLOV3的基础上，引入了“Decoupled Head”，“Data Augmentation”，“Anchor Free” 和“SimOTA样本匹配”的方法，构建了一种anchor-free的端到端目标检测框架，并且达到了顶级的检测水平。
- Decoupled Head   
    Decoupled Head提升了YOLOX的性能和收敛速度，但更深层次的，它为YOLO与检测下游任务的一体化带来可能：例如，YOLOX+Yolact/CondInst/SOLO，可实现端侧的实例分割；YOLOX + 34层输出，实现端侧人体的 17 个关键点检测。
- Data Augmentation
   - 注意：要在训练结束前的15个epoch关掉Mosaic 和Mixup ，这对于YOLOX非常重要。可以想象，Mosaic+Mixup生成的训练图片，远远脱离自然图片的真实分布，并且Mosaic大量的crop操作会带来很多不准确的标注框
   - 当模型容量足够大的时候，相对于先验知识（各种 tricks，hand-crafted rules ），更多的后验（数据/数据增强）才会产生本质影响。
   - 原理上更接近Copypaste的Mixup数据增强方式
- Anchor Free & SimOTA样本匹配
    一个好的样本匹配算法可以天然缓解拥挤场景的检测问题，缓解极端长宽比的物体的检测效果差的问题，以及极端大小目标正样本不均衡的问题，甚至可能可以缓解旋转物体检测效果不好的问题；
   - loss/quality/prediction aware
   - center prior
   - 不同目标设定不同的正样本数量(dynamic k)
   - 全局信息


# 先验知识
- AutoAssign，LLA，DeFCN，IQDet，OTA   
- Anchor Free和Label Assignment   
- 额外涨点的tricks：deformable conv，额外数据做pretrain，momentum=0.937   
- DeFCN首先提出“解耦头”？    

