---
title:       "LLM Miscellaneous"
subtitle:    ""
description: "LLM 笔记 / 杂记"
date:        "2025-01-21T14:23:16Z"
author:      "Hao Xu"
image:       ""
tags:        ["AI", "LLM"]
categories:  ["Tech" ]
draft:       false
---

# LLM显存占用计算

## 精度

FP32 (32 位浮点数): 32 bits = 4 bytes。标准精度，每个参数占用 4 字节。
FP16 (16 位浮点数): 16 bits = 2 bytes。半精度浮点数，每个参数占用 2 字节。
BP16 (16 位脑浮点数): 16 bits = 2 bytes。与 FP16 类似，但具有更大的指数范围，适用于深度学习。
INT8 (8 位整数): 8 bits = 1 byte。低精度整数精度，每个参数占用 1 字节。
量化 (4 位整数): 4 bits = 0.5 bytes。每个参数占用 0.5 字节。或者更低的精度，如 2 位整数。

![Precision](/img/2025-01-21-LLM-Miscellaneous/precision.png)

## 模型显存占用

神经网络模型由多个层组成，每一层都包含权重（weight）和偏置（bias），这些统称为模型参数。而模型的参数量一般直接影响它的学习和表示能力。模型参数量的计算公式为：

B (Billion) 和 G (Giga) 都是十亿级别的量级。例如，1B = 1,000,000,000，1G = 1,000,000,000。而 K (Kilo) 和 M (Mega) 都是百万级别的量级。例如，1K = 1,000，1M = 1,000,000。

模型显存占用的计算公式为：参数量 x 参数精度。

## GPU 显存需求

### 训练显存占用

训练过程中，显存占用主要包括：

- 模型参数: 模型参数量 x 参数精度
- 梯度: 模型参数量 x 参数精度
- 优化器状态: 优化器状态量 x 参数精度。如动量和二阶矩估计等，取决于优化器的类型，单纯的 SGD 不占显存。Adam 优化器：需要存储一阶和二阶动量，各自占用模型参数大小的空间。
- 中间激活值: 每个 batch 的激活值 x 参数精度。激活值的大小取决于模型的结构和 batch size。前向传播过程中产生的激活值，需要在反向传播中使用，其显存占用与 Batch Size、序列长度以及模型架构相关。
- 批量大小 (Batch Size): 一次处理的批样本占用的显存。batch size 越大，显存占用越大。
- 其他开销: CUDA 上下文、显存碎片等。

### 推理显存占用

推理过程中，显存占用主要包括：

- 模型参数: 模型参数量 x 参数精度
- 激活值: 激活值 x 参数精度。推理过程中，激活值的显存占用与模型的结构和 batch size 有关。
- 批量大小 (Batch Size): 一次处理的批样本占用的显存。batch size 越大，显存占用越大。

# Reference

[[1] 7. 探究模型参数与显存的关系以及不同精度造成的影响](https://zhuanlan.zhihu.com/p/721250766)
