## CE-Net：用于2D医学图像分割的上下文编码器网络，已开源！

摸鱼家 [CVer](javascript:void(0);) *昨天*

点击上方“**CVer**”，选择加"星标"或“置顶”

重磅干货，第一时间送达![img](https://mmbiz.qpic.cn/mmbiz_jpg/ow6przZuPIENb0m5iawutIf90N2Ub3dcPuP2KXHJvaR1Fv2FnicTuOy3KcHuIEJbd9lUyOibeXqW8tEhoJGL98qOw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> 作者：摸鱼家
>
> https://zhuanlan.zhihu.com/p/68953780
>
> 本文已授权，未经允许，不得二次转载

**CE-Net: Context Encoder Network for 2D MedicalImage Segmentation**

![img](https://mmbiz.qpic.cn/mmbiz_png/yNnalkXE7oUfyhMHe3IUoxFcuhcZ84kuUY0FBDfF38Nn4gt6HrKYGibH6cMDgKgFvibYyFSvJDBvc4wQ2vKibTt1g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



作者团队：上海大学&中科院&上海科技大学等

arXiv：https://arxiv.org/abs/1903.02740

github：https://github.com/Guzaiwang/CE-Net





![img](https://mmbiz.qpic.cn/mmbiz_png/yNnalkXE7oUfyhMHe3IUoxFcuhcZ84kufcBbOuw41TeyjteN3L7QAic2ed1WKXhibeJIyiamVg1xEJiaVfNtddRE1A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

\1. Abstract

- - 提出了一种用于医学图像分割的网络结构，创新点在于在传统的encoder-decoder结构中间加入了context extractor，用来减少由于池化和卷积导致的信息损失。

\2. Introduction

- - 传统的医学图像分割方法，深度学习的方法，主要是U-Net，然后是一些其他修改方法，如CRF，使用了多尺度的M-Net,BRU-net,InfiNet
  - 使用了residual connectio来避免梯度消失。
  - 提出了DAC block(dense atrous convolution )提取更多的去全局语义信息。the DACblock is proposed to extract enriched feature representationswith multi-scale atrous convolutions
  - 提出了residual multi-kernel pooling (RMP) ，它是受空间金字塔池化的启发，对DAC的输出进行多尺度的信息编码？？RMPblock for further context information with multi-scale poolingoperations 大概的意思就是，DAC进行多尺度特征提取，RMP进行池化。
  - 总结来说，本文的主要贡献就是提出了DAC和RMP block，并将其整合到基于encoder-decoder框架中，来进行医学图像的分割。

\3. Conclusion

- - 提出了end-to-end的网络，能够提升多个医学图像分割的表现，相信后续也可能扩展到其他医学图像的分割上，或者扩展到3D。

\4. Methed

· Feature Encoder Module

· 基于ResNet-34

· Context Extractor Module

· 本文提出的，由DAC和MRP组成，作者的说法是提取语义信息，产生更高层次的feature map

· 这个模块输入输出尺寸是一样的，也就是说，这个模块可以用在其他任何结构中。

· Dense Atrous Convolution

· 在这篇论文里，空洞卷积组成的模块是加在encoder之后的，另一篇论文（MULTI-SCALE CONTEXT AGGREGATION BYDILATED CONVOLUTIONS）也使用了类似的结构，不过它的模块是由串联的空洞卷积组成的。这篇文章里的dense atrous convolution （DAC）模块是用了并联，和GoogLe Net的inception模块类似。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

· 具体来说，每个并联分支使用了不同尺寸的空洞卷积，便于提供不同的感受野，能够针对不同大小的目标。

· Residual multi-kernel pooling (RMP )

![img](https://mmbiz.qpic.cn/mmbiz_png/yNnalkXE7oUfyhMHe3IUoxFcuhcZ84ku1YbREBVLKO1tiah1WKKaSvApib95sPxfg7Jf8TbwHxNCaQr2eMVKibFzA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

· 这个模块连接在DAC之后，使用不同的池化方式并联，作者的说法是为了提供不同的感受野

· 为了减少参数，在每一层之后加入了1*1的卷积

· Feature Decoder Module

· decoder部分使用了转置卷积进行上采样，每个部分如下：卷积—转置卷积-卷积，这里没有采用和encoder的对称结构。

![img](https://mmbiz.qpic.cn/mmbiz_png/yNnalkXE7oUfyhMHe3IUoxFcuhcZ84kun6Yd6MuWMssmpoAtvGicPgwJT1ReZjteaVK2YzCVCzWVibK8icgfcApKg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

· Loss Function

· Dice coefficient loss function ，因为医学图像目标都比较小

\5. 实验

· 在不同的医学图像进行了比较，如视杯视盘，血管等

· 血管分割的评价使用了3种方式，相关论文如何评价还可以再看看

![img](https://mmbiz.qpic.cn/mmbiz_png/yNnalkXE7oUfyhMHe3IUoxFcuhcZ84kud4RZbwpAlG8GBxSBJB5jMpboJGScOej4Y84TzMUMp908b6SuiaXSb6Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Reference

> *CE-Net: Context Encoder Network for 2D Medical Image Segmentation*



**CVer-医疗影像交流群**



扫码添加CVer助手，可申请加入**CVer-医疗影像群。****一定要备注：****研究方向+地点+学校/公司+昵称**（如医疗影像+上海+上交+卡卡）

![img](https://mmbiz.qpic.cn/mmbiz_jpg/yNnalkXE7oX7pdpBKibicSnmb8wRGicbT0Rhr61k0f922lbXcowibk5DTRibROvFB1yMCAZQvj1iaEe6Qsia9bU0UMJCA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

▲长按加群



这么硬的**论文分享**，麻烦给我一个在**在看**



![img](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWg3jTWCAZ4BrnvIuN20lLkhIjtg4GRSDhTk9NpeF0GGTJwUpKPatscIQU7Ndj9hgl8BPpGj2BJoFw/640?tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

▲长按关注我们

**麻烦给我一个在看****！**



[阅读原文](https://mp.weixin.qq.com/s?__biz=MzUxNjcxMjQxNg==&mid=2247489955&idx=4&sn=5c0afa328c560ba95538b20e91f2511a&chksm=f9a26b2cced5e23a7fcf1f3737ac44f3786343b3762514e6c51efc0aa27a5a2c452240c38190&mpshare=1&scene=1&srcid=&key=55f99d04c6be589cb2a89d89a97d47662d3b8f87395adfb671dfe198f3083ce66243a742414207d7c9eaa047d213900dd61d660b85d98546e31e75d42e7d77faaefdd22266c5e30ec86cc03708d8e226&ascene=1&uin=MjMzNDA2ODYyNQ%3D%3D&devicetype=Windows+10&version=62060833&lang=zh_CN&pass_ticket=gTcGMiRI%2FrgdalBpEedBJUR9Xe7se%2B0GfGmnO0zGcIM9Lj2iEqzyYiPvMtdKmsPA##)



![img](https://mp.weixin.qq.com/mp/qrcode?scene=10000004&size=102&__biz=MzUxNjcxMjQxNg==&mid=2247489955&idx=4&sn=5c0afa328c560ba95538b20e91f2511a&send_time=)

微信扫一扫
关注该公众号