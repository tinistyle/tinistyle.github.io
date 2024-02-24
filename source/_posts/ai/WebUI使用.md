---
title: Stable Diffusion WebUI使用
date: 2023-11-24 12:35:59
tags:
  - AI绘画
  - SD
  - WebUI
categories: AI
---

## 简介

Stable Diffusion是2022年发布的深度学习文本到图像生成模型。它主要用于根据文本的描述产生详细图像，官方项目其实并不适合新手直接使用，好在有一些基于 stable-diffusion 封装的 webui 开源项目，可以通过界面交互的方式来使用 stable-diffusion，极大的降低了使用门槛。

其中，AUTOMATIC1111 的 stable-diffusion-webui 是目前功能最多最好用的

> https://github.com/AUTOMATIC1111/stable-diffusion-webui

### 配置要求

要顺利运行stable-diffusion-webui和模型， 需要足够大的显存，最低配置4GB显存，基本配置6GB显存，推荐配置12GB显存。 当然内存也不能太小，最好大于16GB，总之内存越大越好


## 安装

webui的安装使用较为繁琐，推荐使用秋叶的整合包，整合了需要的环境配置，同时包含了一些基础的模型和插件，可以快速上手使用

> https://www.bilibili.com/video/BV1ne4y1V7QU/?vd_source=c056351bf5bc0b1c0d7e62eec64a0035

重要文件夹：
+ 安装文件夹/models：存放SD模型的文件夹，包含大模型、lora、Controlnet等
+ 安装文件夹/extensions/sd-webui-controlnet/annotator/downloads：存放ControlNet预处理器，23.10月后启动器自动下载启动器
+ 安装文件夹/output：存放生成的图片


## 常用参数解析

### Checkpoint大模型

SD的心脏，定义出图风格，常见文件类型：safetensors、ckpt。大小2-10GB左右。常见模型有：`anything-v5-PrtRE.safetensors`（动漫二次元风格）、`v1-5-pruned.ckpt`（欧美写实）、`chilloutmix-NiPrunedFp32Fix.safetensors`（亚洲写实）

### VAE

Variational Auto Encoder（变分自编码器），作用是增加图片饱和度，降低灰度，让图片更多色彩（相当于加滤镜）。有些大模型自带有vae模型。常用模型为840000

![image-20240224161232412](https://tinistyle-self.oss-cn-chengdu.aliyuncs.com/books/image-20240224161232412.png)



### Clip Skip

Contrastive Language Image Pre-training（Clip）语言与图像的对比预训练。让语言与图片建立关系

skip跳过的数值越高，语言关键词和图片的关联关系就越低（推荐2-4）

![image-20240224161349916](https://tinistyle-self.oss-cn-chengdu.aliyuncs.com/books/image-20240224161349916.png)

### 提示词

分为正向和反向，正向是需要图片内容，反向是规定图片不能出现的内容

#### 分类与书写逻辑

提示词逻辑可以分为标准化提升型提示词和内容型提示词

标准提示词可以用于画质提升，画风定义：

+ 通用画质提升：best quality,ultra-detailed,nsanely detailed,higres,8k,masterpiece,
+ 特定引擎渲染类型：extremely detailed CG unity 8K wallpaper, unreal engine rendered, octane render,
+ 插画风：painting,Illustration,drawing,paintbrush,
+ 二次元：anime,comic,game CG,
+ 写实：photorealistic, realistic, photograph,

内容型提示词基本框架：人物及主体特征、场景特征、环境光照、画幅、视角

+ 人物主体特征：
  + 服饰穿搭：white dress
  + 发型发色：blonde hair、long hair
  + 五官特征：big eyes、smill eys
  + 表情：smilling
  + 肢体动作：strething arms、hands up,
+ 场景特征：
  + 室内外：indoor、outdoor
  + 大场景：forest、city、street、universe、galaxy、mars、sky
  + 小细节：tree、bush、white flower
+ 环境光照：
  + 时间：day、night、morning、summer
  + 光照：sunlight、bright、dark
  + 天气：rain、snow
  + 灯光：cinimatic lighting、studio lighting
+ 画幅视角：
  + 主体占比：full body、upper body
  + 距离：close-up、distant
  + 观察视角：from above、view of back
  + 镜头：sony a7、wide angle

提示词权重：权重范围控制到0.5-1.6之间

+ `(word:1.5)`  ：1.5倍，数字控制倍数
+ `(word)` ： 1.1倍，每多一层额外1.1倍
+ `{word}` ：每多一层额外1.05倍
+ `[word]` ：每多一层额外0.9倍

高级写法：

+ `blue | yellow flower`： 混合关键词， 表示生成蓝色和黄色混合的花
+ `[ blue | red | green ] flower,`：交替算法，在步数中循环生成各种颜色然后融合，
+ `[tiger | cow],`  ：动物融合
+ `[moutain:moon:15`] ：前15步生成moutain，后面步数生成moon
+ `(moutain:moon:0.8 )` ：前80%步数生成moutain，后面生成moon

负面提示词：

+ 低质量的：low quality, lowres, low resolution, noise
+ 色调单一：monochorome, grayscale,
+ 身体特征：missing part, bad proportions, ugly,
+ 身体部位：extra hands, extra fingers, missing hands,
+ NSFW：（Not Safe For Work，工作场所不宜）



### 迭代步数

范围1-150，越大细节越多，渲染越慢，建议范围20-40。不同的采样器有不同的步数推荐



### 采样器/采样方法

webui中两者合并为一个选项，前者为采样器，后缀为采样方法

采样器：

+ DDIM & PLMS（过时 不建议用）：DDIM (Denoising Diffusion Implicit Model) 最开始的一sampler，PLMS (Pseudo Linear Multi-Step method)是DDIM的升级版，更快更清晰
+ DPM、DPM2、DPM++
+ UniPC

采样方法：

+ a：Ancestral sampler（比较古老的采样方式），采样时很随机，缺点是无法集中采集噪点，识别关键词不精准
+ Karras：噪点开始的时候采样方式大于默认，快结束时噪点会低于默认

推荐的：可以根据模型介绍中推荐的采样器

![脚本xyz介绍2 steps to sampler](https://tinistyle-self.oss-cn-chengdu.aliyuncs.com/books/脚本xyz介绍2 steps to sampler.png)

### CFG Scale

提示词引导系数，表示效果与提示词近似度，数值越高图片越会和提示词相似，数值越低AI的想象力就会多一些

+ 数值0-1 图片容易出错
+ 数值2-6时AI的想象力就很丰富
+ 数值7-12 推荐阈值
+ 大于15图片容易出错

![xyz steps to CFG scale](https://tinistyle-self.oss-cn-chengdu.aliyuncs.com/books/xyz steps to CFG scale.png)



### 重绘幅度

图生图选项，用于表示原图和成品图的相关性，太高容易变形，太低实现不了重绘


## 扩展

中文汉化包

图库浏览器

tag自动补全

tagger反推

Ultimate Upscale  终极放大插件，SD上位

LLul  局部潜空间增加细节处理

Cut Off  精确控制隔离提示词的描述成分

Infinite Zoom 无限放大的循环动画