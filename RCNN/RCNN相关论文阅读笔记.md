**RCNN**

*概述*
    
    近年来Object detection发展缓慢，作者提出使用CNN来进行region
    proposal并结合有监管的预训练方法来解决此问题，结果证明效果显著。


*具体实现*
    
    三个模块：
    region proposal(selective search), feature extraction(CNN), SVM进行分类
    (贪心非最大抑制)

    预训练：
        supervised pre-trainning：用以解决训练样本过小的问题。去除已有CNN模型最
        后一层，用自己的样本(标注了定位信息的样本)训练从而实现参数微调

    
    正式训练：
        1.使用selective search从输入图片中找出2K个候选框(候选框大小不定)
        2.将2K个候选框对应图片大小进行调整
        (因为现有caffe只能针对指定大小的图片进行处理)，
        然后输入到CNN中，提取特征向量
        3.使用SVM对特征向量进行分类

*创新点与优势*

    1.第一个把CNN引入到该问题中来，就如同汤晓鸥团队第一个把CNN引入到SR问题中一样
    2.在CNN上用fine-tuning来解决要训练的样本过小的问题
    （从时间上看，会不会是第一个啊）
    3.准确率大幅度提升（跟AlexNet碾压第二名一样）
    4.设计了对照试验，验证了fine-tuning对CNN参数的影响究竟在哪--没有fine-tuning
    ，尽管第六，七层的参数比第五层多，但是总体表现结果都差不多；然而如果加上
    fine-tuning，效果就直接蹭蹭蹭上去了。这个实验无疑说明了一个像AlexNet这样的
    网络，更多的具有一种普世性，可以完成相对来说比较基本的任务。但如果想再某一
    个样本集下取得非常好的效果，无疑还是需要fine-tuning的


*缺点*

    1. 生成2K个建议框并经由CNN进行计算，显然这会是一个相当费时的操作(其实速度相
        较以前滑动窗口遍历所有可能区域的方法，已经算是不错的了；另外其CNN网络的
        很多变量都是共享的，且输入的图片与很多其他方法相比也是low dimension的。
        已经很努力的在提速了。但显然人的伟大之处就是要不断挑战极限！！！从而玄
        妙的蝴蝶效应决定了微不足道的我要再读两篇论文的命运)

    2. muti-stage中，其独立的分类器与回归器需要大量特征作为训练样本，从而会占用
    较大的存储空间

*一点想法*
    
    暑假的时候看过Russell[1]的一篇关于行人检测的论文，感觉和这篇论文所描述的问
    题很相似，不过那里用的是LSTM以及jointly learning的方法，从而不需要非最大抑
    制的方法。且其有一个特点是最后识别的人脸是根据概率大小顺序输出的，那么能否
    在RCNN的算法里，对selective search的阶段就进行概率计算从而选取概率比较大
    的候选框从而输入到CNN层中呢？

    最后补充几句，感觉这篇论文，真的讲的好详细啊，怎么调参说的清清楚楚（当然让
    我实操肯定得踩坑踩到死）

[1]Stewart R, Andriluka M. End-to-end people detection in crowded scenes[J]. 2015:2325-2333.


**Fast-RCNN**

*概述*

    使用jointly learning来进行目标分类与确定位置，速度比RCNN更快


*具体实现*
    
    1. 将图片输入，产生一个特征图
    2. 每个目标通过RoI pooling layer生成特征向量,四元组
    3. 特征向量输入到全连层，产生两个分支：一个生成对各个目标的概率，一个生成bounding box的定位框坐标

*创新点*
    
    1. Single-stage training，不再使用独立的分类器与回归器（直接用softmax进行输
    出），从而节省了大量的磁盘存储，且实验证明效果上没有区别（甚至略有提升）。
    且不是一开始就向网络中输入候选框，这样避免了前期过多冗余的计算，从而减少计
    算复杂度并节省了时间
    
    2. 使用Truncated SVD，以较小的精度损失，换来了较大的提速（目前理解到的是，
    通过将一个矩阵近似表达为一个左奇异矩阵X对角矩阵X右奇异矩阵，从而降低计算量
    。但有一个疑问是，图片是一个相对来说非常大的矩阵，这种近似固然好，但是当误
    差累加到一定程度，不会产生重大错误吗？就像往图片中加入少量的高斯噪音，人眼
    虽然看不出来，但是图片的矩阵表示上有区别，当这种偏差累计起来，会不会影响最
    终结果？）

    3. Training can update all network layers--相较SPPnet有改进
    （没太明白为什么SPPnet就不能更新）


*缺点*
    
    缺点肯定得是有的，不然为什么会有Faster-RCNN呢？还剩下一个没有提速的就是
    selective search阶段生成的候选框了，且看下一篇怎么改吧
   
*一点想法*
    
    1. 文中说同时输入两个参数（图片和候选区域），这个一开始没有明白，一直以为该
    search是内嵌在网络中的，后来看了别人的博客贴了一张网络的结构图，才算是明白
    图片是从网络一开始输入进去的，候选区域是通过selective search
    选出再输入进去的。那我就不由得想提个问题了，selective search
    能不能内嵌到CNN中？(我就是个小白，瞎比比一下)

    2. Ross Girshick这个人写论文真的很赞，每次介绍完其成果之后，都会给你详细的
    介绍实验中具体操作，并且做了大量的对比实验（关于候选框数量的那个就解决了我
    对于2K这个数量的疑惑）。其得出的一些结论（不需要对图片进行太多的尺寸上变化
    ，fine-tuning应该从哪层开始做tradeoff最好等）对其他研究人员都有很好的指导意义


**Faster-RCNN**

*概述*

    实验发现Fast-RCNN中前五层网络所产生的convolutional feature
    同样可以用来进行Region Proposal，故而提出构建一个 
    Region Proposal Network来实现候选区域的选取，因为其可以与其
    他网络共享权值，从而实现加速（真的把proposal 用CNN实现了啊）

*具体实现*

    首先祭出整体网络结构图，从图中可以看出，网络的第一部分是卷积层等，随后解着
    的是RPN（正如论文所说，feature map可以在proposal阶段应用），
    RPN输出的proposal box与第一阶段的feature map
    一同输入到RoI层，后续即为Fast-RCNN的结构。
![总体结构][01]
[01]: https://raw.githubusercontent.com/Aaron0813/Pictures/master/%E6%80%BB%E4%BD%93%E7%BB%93%E6%9E%84.jpg "总体结构"


    在本文中，anchor是一个非常重要的概念，其大致样式如下图所示。
![Anchor][02]
[02]: https://github.com/Aaron0813/Pictures/blob/master/Anchor.jpg?raw=true "Anchor"

    作者们为convolution feature中的每个点设计了如上图所示的九种模式的anchor，试
    图用这来进行初始的边缘检测。显然，单凭这样的一次操作很难正确定位到物体。但
    是通过对比Ground Truth的情况，可以发现经由平移与缩放是可以不断的逼近甚至是
    两个box完全一致的。所以RPN网络的一个目的就是经过大量的学习，来掌握这样的平
    移技巧（即box regression）。
![平移][03]
[03]: https://github.com/Aaron0813/Pictures/blob/master/%E5%B9%B3%E7%A7%BB.jpg?raw=true "平移"

    而上述操作，就是经由下面的这条支路实现的。
![regression][04]
[04]: https://github.com/Aaron0813/Pictures/blob/master/box%20regression.jpg?raw=true "regression"

    
    
    在另一条线路中，RPN将会使用softmax来进行前景与后景的判断。随后，proposaled region
    与前后景信息将会被输入到proposal层，首先将图像恢复到原始大小，然后剔除严重
    超出边界的Anchor，随后进行非最大抑制，排序，选取前N个传入的RoI层


    RoI层接受feature map与proposal box，剩下操作同fast-rcnn

*创新点*
    
    1. 成功的将Region Proposal嵌入到CNN中，不得不承认，Anchor的这个设计非常巧妙。通过共享feature map，
    成功的节省了大量的计算


*一点想法*

    提出一个再次加速的设想，以每个点为中心进行边缘检测是不是计算量太大了，一张
    图片总共又能产生多少个边缘框呢？又相邻的像素点大概率属于同一个物体，所以设
    想以一定的步长，间隔的对每个点进行边缘检测。如果该数值选取得当，应该可以取
    得一定的加速，但是鉴于现在的数量级基本已经确定，所以在数值上的影响可能不是
    特别大