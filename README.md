# 图像分类
通用图像分类步骤，Pytorch实现。

## 一、数据准备
准备方式：
* 现有数据集
* 网络爬取数据
* 手动下载数据

现有数据集不多介绍，主要介绍网络数据集爬取（因为我们需要的类别大概率不是现有数据集中的）；

数据爬取代码：1_claw.py

作用是爬取数据并生成数据集，目前百度反爬取相关策略比较严格，可能会不成功一些图像，不成功多爬取几次或者自己下载；参考：[图像爬取](https://github.com/QianyanTech/Image-Downloader) 

Tips: 请遵守相关法律法规进行正当爬取，勿用于商业用途；

数据爬取成功后，得到数据集Dataset目录，并生成标签class_name.xlsx的文件，用于标签和类别对应(标签编码）；
如：
```
class_name.xlsx标签文件：
        000: 猫
        001: 狗
        002: 大象
```
路径示例：

```
数据集路径：
        Dataset --------------------------
                |-000         ｜-1.jpg
                |             ｜-2.jpg
                |             ｜-3.jpg
                --------------------------
                |-001         ｜-1.jpg
                |             ｜-2.jpg
                |             ｜-3.jpg
                --------------------------
                |……
```
## 二、数据集划分
通过爬虫/下载得到相应的数据集之后我们需要对其进行数据集划分。

数据集划分：生成train_list.txt/val_list.txt/test_list.txt;

## 三、训练网络
训练一个深度学习网络包括以下几个部分：
* 数据加载
* 数据预处理
* 定义预训练模型
```
预训练model替换：
model = torchvision.models.resnet50(weights=models.ResNet50_Weights.DEFAULT)
可替换其他model：
resnet101: model = torchvision.models.resnet101(weights=models.ResNet101_Weights.DEFAULT)
vgg19: model = torchvision.models.vgg19_bn(weights=models.VGG19_BN_Weights.DEFAULT)
mobileNet: model = torchvision.models.mobilenet_v3_large(weights=models.MobileNet_V3_Large_Weights.DEFAULT)
densenet201: model = torchvision.models.densenet201(weights=models.DenseNet201_Weights.DEFAULT)
efficientnet: model = torchvision.models.efficientnet_b6(weights=models.EfficientNet_B6_Weights.DEFAULT)
swin-transformer: model = torchvision.models.swin_b(weights=models.Swin_B_Weights.DEFAULT)
```
更多模型可参考：[torchvision预训练模型](https://pytorch.org/vision/stable/models.html)

```
全连接层替换：
num_ftrs = model.fc.in_features
model.fc = nn.Linear(num_ftrs, num_classes)  # 假设 num_classes 是你的分类类别数，示例代码类别为3。
```
* 定义损失函数
* 训练模型：训练模型，每个Epoch结束在验证集上验证模型的性能
* 保存模型
* 在测试集上测试模型
