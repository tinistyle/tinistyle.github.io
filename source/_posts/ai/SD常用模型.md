---
title: Stable Diffusion 常用模型
date: 2024-11-25 21:57:13
tags:
  - AI绘画
  - SD
categories: AI
---

## 大模型
### 二次元模型

AbyssOrangeMix、Counterfeit、Anything、Dreamlike Diffusion


### 真实系模型
Deliberate

Realitic Vision

LOFI

### 2.5D模型

国风3

Protogen

NeverEndingDream


### 特化风格

---

## 微调模型
对大模型起到微调作用
### Embeddings
词嵌入模型，对一组关键词的打包，对这些组合关键词应用特殊效果。类似指向大模型的特定页面的书签

### LoRA
低秩适应模型，一般用于还原角色、形象特征、也可以用于训练画风。类似给大模型加上的额外彩页

最早用于大预言模型。通过更少的数据量，对一个大模型实现微调

多个LoRA一起使用：使用Additional Networks扩展，最多支持五个LoRA



### Hypernetwork
超网格，一般用于画风训练，用的不多

