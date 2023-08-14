# OpenMMLab介绍

## OpenMMlab简介
OpenMMLab是一个计算机视觉领域的开源项目，适用与学术研究和工业应用，涵盖了许多视觉方向的研究课题。学术者可以研究通过这个项目，更好的关注于研究方向，不必花大量时间重复代码的工作，拿来简单修改就可以使用。行业从事着也可以对此应用于现实场景，极市平台的工业项目打榜使用这个完成项目封装并顺利通过相应算法验收的人也不少。

## 算法库的特点
- 模块化组合设计
- 高性能
- SOTA方法（State of the Art , 各个领域最先进的算法）

### [MMsegmentation](https://github.com/open-mmlab/mmsegmentation)是什么？
MMsegmentation是OpenMMLab开源项目中的语义分割领域的算法工具箱，为语言分割提供了统一的框架与基准测试，同时实现了许多高质量算法模型与数据集
- 统一性
- 灵活性
- 全面性
#### 重点的模块
1. 高层API的使用
2. 数据集:BaseSegDataset
3. 数据变换:Data Pipeline
4. 模型:Segmentor
    - 6个功能模块
        - data_preprocessor
        - backbone
        - neck
        - decode_head
        - auxilary_head
        - loss
    - 2种模型结构
        - EncoderDecoder
        - CascadeEncoderDecoder
    - 3种前传模式
        - tensor
        - loss
        - predit
    - 2种推理模式
        - whole inference
        - slide inference
5. 数据流
6. 部署：MMDeploy

## 社区支持
+ 开发者社区：
    + 超级视客营
    + 开源贡献体系
    + MMSig小组
    + . . . 
+ 社区开放麦
+ 在多个知识平台都可以了解到分享的知识

## 项目链接
Github：[https://github.com/open-mmlab](https://github.com/open-mmlab)  
官网：[https://openmmlab.com/](https://openmmlab.com/)