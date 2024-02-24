---
title: SD 高清修复
date: 2024-02-24 02:57:13
tags:
  - AI绘画
  - SD
categories: AI
---


用于通过添加额外控制条件，引导 Stable Diffusion 按照创作者的创作思路生成图像，从而提升 AI 图像生成的可控性和精度。

基本结构：预处理器 -> 模型 （预处理器从图片中提取特征信息，训练过的ControlNet模型读取这些信息，并引导SD生成过程

相关参数：
+ 控制权重：影响控制的力度
+ 引导时机：在生成过程中ControlNet的生效时间（0-1）
+ 控制模式：更倾向于提示词还是ControlNet


五大ControlNet：
+ Openpose：控制姿势、手部、面部细节。选用不同的姿势控制部位预处理
+ Depth：控制空间组成，生成深度图（黑远白近），预处理器中leres精度高更费时，midas更泛用
+ Canny：控制线条轮廓，预处理时设置线条阈值，不宜太过密集。多应用于线稿上色
+ SoftEdge：控制线条轮廓，但更加柔和。与canny相比，最终效果对轮廓处理更加生动，不刻板，适当降低ControlNet的权重可以让ai发挥更多创造力
+ Scribble：涂鸦引导画面生成

多重控制应用：组合的逻辑是互补
+ Depth + Openpose，让人物肢体前后顺序正常
+ Canny + Depth，利用线条补足深度里的细节

