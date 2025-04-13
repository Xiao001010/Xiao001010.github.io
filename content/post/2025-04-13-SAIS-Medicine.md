---
title:       "Datawhale AI春训营SAIS Medicine"
subtitle:    ""
description: "Notes for SAIS Medicine Improvements"
date:        "2025-04-13T12:39:34+01:00"
author:      "Hao Xu"
image:       ""
tags:        ["AI", "GNN", "RNA", "Datawhale"]
categories:  ["Tech" ]
draft:       false
---

# Improvements

相较于Datawhale 给出的 Baseline 代码，Task 2的代码有以下改进：

- 更丰富的几何特征: 新代码引入了几何特征生成器，提供了 RBF（径向基函数）特征、二面角（dihedrals）和方向特征（direction feature）的计算。这使得节点特征不再局限于单纯的展平坐标，而是融合了局部几何信息，为模型提供更多结构化信息。

- 改进的边构造与特征: 利用 radius_graph 代替了之前基于序列顺序构造边的方式，能够依据节点在空间上的距离构建更加合理的连接关系。边特征不仅计算了边的距离，还利用 RBF 将距离信息编码，再加上边向量的归一化结果，使得边特征更加丰富，便于后续信息传递。

- 更强大的模型架构: 模型更换为基于 TransformerConv 的结构，配合多头注意力机制，使得节点信息在传递时能够更灵活地捕捉关系。增加了残差连接及多层堆叠（num_layers=4），并配合 LayerNorm 和 Dropout，增强了网络的稳定性和泛化能力。对节点和边都引入了独立的编码器（全连接层＋激活＋LayerNorm＋Dropout），使得信息先经过预处理再进入卷积模块，从而降低特征之间的干扰。

- 训练策略: 新代码支持混合精度训练（amp），利用 torch.cuda.amp 进行梯度缩放，从而加速训练同时减少显存占用。使用了梯度裁剪（clip_grad_norm_）防止梯度爆炸，进一步稳定训练。引入了 CosineAnnealingLR 学习率调度器，使得学习率在训练过程中更加平滑地衰减。数据加载部分增加了 pin_memory 和 num_workers 参数，提高了数据加载效率。

- 数据增强: 增加了数据增强模块——在数据集加载过程中，随机对坐标进行旋转（CoordTransform.random_rotation），增加了数据多样性，提高模型鲁棒性。

- 参数设置更精细: 例如 batch_size 根据是否有 GPU 进行调整，学习率调低，隐藏层维度增大，从而更适合于更复杂、信息量更丰富的特征表示。新增了参数如 dropout、rbf_dim、num_heads 等，方便后续调参和控制模型复杂度。

# Future work

原始版本大概可达到0.25左右。
经过上述改进可达到0.4-0.5。
官方的baseline大概在0.6左右。
更高的分数可以考虑扩散模型。
