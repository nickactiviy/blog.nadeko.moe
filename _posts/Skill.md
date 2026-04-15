---
layout: post
title: "Penjelasan singkat 2. mengenai skill"
date: 2026-4-1
tags: [biology, filsuf]
author: Nicholas
---

Aku akan menjelaskan tentang apa itu skill, dari bahasa yang mudah dipahami manusia sampai ke level hard dan argumen<!-- more -->

## 2.1. 图像增广

图像增广：对训练图像进行随机变化生成相似但不同的样本，扩大训练集规模，减少模型对特定属性的依赖，提升泛化能力

### 13.1.1. 常用的图像增广方法

1. 翻转和裁剪
    - `RandomHorizontalFlip`：随机左右翻转
    - `RandomVerticalFlip`：随机上下翻转
    - `RandomResizedCrop`：随机裁剪

2. 改变颜色
    - `ColorJitter`：亮度、对比度、饱和度、色调调整

3. 组合增广
    - `Compose`：组合多种增广方法

### 13.1.2. 使用图像增广进行训练

通常仅对训练样本应用带随机操作的增广

- 数据处理：用 `ToTensor` 转换为（批量大小，通道数，高度，宽度）的 32 位浮点数（0~1）

- 数据加载：
    - 训练集：`torchvision.datasets.CIFAR10` + 训练增广 `train_augs`
    - 测试集：`torchvision.datasets.CIFAR10` + 测试增广 `test_augs`
    - 用 `DataLoader` 加载数据

- 模型训练：
    - 采用 ResNet-18 模型
    - 多 GPU 训练：`nn.DataParallel`
    - 优化器：Adam
    - 损失函数：`CrossEntropyLoss`

## 13.2. 微调

**迁移学习（transfer learning）**：将从源数据集学到的知识迁移到目标数据集

**微调（fine-tuning）**：迁移学习的一种常见技术，适用于目标数据集小于源数据集的场景

### 13.2.1. 步骤

1. 在源数据集上预训练源模型
2. 构建目标模型：复制源模型除输出层外的所有设计和参数
3. 为目标模型添加新输出层（输出数为目标数据集类别数），随机初始化其参数
4. 在目标数据集上训练：输出层从头训练，其他层基于源模型参数微调

### 
## 13.8. 区域卷积神经网络（R-CNN）系列

### 
实现方式：通过转置卷积将中间特征图的高和宽恢复至输入尺寸

### 13.11.1. 构造模型

1. 特征提取：
    合成图像与内容图像在内容层输出的均方误差

2. 风格损失

    合成图像与风格图像在风格层输出的 Gram 矩阵的均方误差。Gram 矩阵衡量不同通道间风格特征的相关性

3. 全变分损失

    减少合成图像中相邻像素间的差异，起到去噪作用

### 13.12.5. 初始化合成图像

将合成图像定义为模型参数（`SynthesizedImage` 类），初始化为内容图像

### 13.12.6. 训练模型

不断前向传播提取合成图像的特征，计算总损失，通过反向传播更新合成图像（即模型参数）
