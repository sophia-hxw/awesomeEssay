

# 训练策略
- [ ] LR optimizations
- [ ] TrivialAugment
- [ ] Long Training
- [ ] Random Erasing
    RandomErasing是厦门大学在2017年提出的一种简单的数据增强（这个策略和同期的CutOut基本一样），基本原理是：随机从图像中擦除一个矩形区域而不改变图像的原始标签。DeiT的训练策略中也包括了RandomErasing。

- [ ] Label Smoothing
- [ ] MixUp 
    MixUp在FAIR在2017年提出的一种数据增强方法：两张不同的图像随机线性组合，而同时生成线性组合的标签。

- [ ] CutMix
    CutMix是2019年提出的一项和MixUp和类似的数据增强策略，它也是同时对两个图像和标签进行混合，与MixUp不同的是它的图像混合方式。CutMix不是对两个图像线性组合，而是从另外一张图像随机剪切一个patch并粘贴到第一张图像上，patch的起始坐标随机生成，而宽高是$\lambda$由来控制.

- [ ] Weight Decay Tuning
- [ ] FixRes
- [ ] EMA
- [ ] Inference Resize Tuning
- [ ] 过拟合问题
- [ ] 使用预训练模型