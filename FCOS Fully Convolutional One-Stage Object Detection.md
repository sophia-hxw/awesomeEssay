# 一，论文翻译

## 摘要
本文提出了一种类似语义分割基于像素级的、一阶段全卷积目标检测网络。当前，几乎所有的一阶段网络，诸如RetinaNet，SSD，YOLOv3或者Faster RCNN等都是基于anchor的，本文是anchor-free的，也是proposal-free的。所以，不需要预定义anchor，FCOS也避免了所有与anchor相关的运算，比如训练时计算的IoU。更重要的是，避免了与anchor box的超参设置和调试，而这些参数往往会对推理结果造成较大影响。因为仅有NMS这个后处理，FCOS在使用ResNeXt-64x4d-101做backbone，且单模型和单尺度情况下能达到44.7AP的性能，远超之前的一阶段检测器，但是结构更简单。与此同时，我们首次证明了更简单、更灵活地检测框架也可以获得较好的检测准确率，期待FCOS将来能以其简单和强大成为其他类似实例分割任务的模型框架。

[github-link](https://tinyurl.com/FCOSv1)

## 介绍
目标检测是视觉领域中基础而有挑战的任务，算法需要能定位对象的同时预测对象的类别。当前的主流检测器都是基于anchor的，像Faster RCNN，SSD，YOLOv2，v3等，所以anchor也被认为是这类检测器成功地关键。尽管如此，但是也不得不指出基于anchor方法的[缺陷](../cvInterview/Detection/anchor_based_vs_anchor_free.md)：    

- 1，检测的性能对size，aspect ratio，anchor的数量等超参的变化特别敏感。比如，RetinaNet中，这些参数的变化可以在COCO benchmark上有约4%AP的性能差，所以，这些参数需要在训练时谨慎调试；   
- 2，即使谨慎地设计，因为scale和aspect ratio是固定的，所以检测器在面对对象尺寸变化较大时容易失效，尤其是对较小的对象，而这些预设的参数同样也会损坏模型的泛化能力；     
- 3，为了提高recall指标，基于anchor的模型就需要在输入图片上密集采样，比如说FPN模型在较小边为800的输入时可能需要18w的anchor框，但是训练时这些框中的大部分是负样本，这样就会造成正负样本比例失衡问题；   
- 4，anchor方法还会带来诸如与ground truth的IoU等复杂的计算；     

最近，FCN的方法在语义分割、深度估计、关键点检测和技术等应用中取得较大成功，作为高层应用之一的目标检测，也因为anchor框的使用可以追溯到基于像素的全卷积框架。一个问题也就随之而来，能用全卷积网络来解决目标检测问题吗？就类似解决语义分割问题的方式？如果答案是"yes"，那么所有的基础视觉任务都可以整合到一个框架。我们用食盐证明，问题的答案是肯定的，同时我们也第一次证明了更简单的基于FCN的检测器性能可优于基于anchor的检测器。



