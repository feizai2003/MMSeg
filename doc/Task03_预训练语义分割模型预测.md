# 预训练语义分割模型预测
- [预训练语义分割模型预测](#预训练语义分割模型预测)
  - [一个预测示例](#一个预测示例)
  - [利用推理API](#利用推理api)
    - [**首先了解三个重要的API：**](#首先了解三个重要的api)
    - [**然后了解图片的可视化**](#然后了解图片的可视化)
    - [**最后是视频的推理**](#最后是视频的推理)
  - [附上参考](#附上参考)

## 一个预测示例
> 1. 首先在克隆的MMseg下创建一个data目录，然后把要用到图片，视频下载到其中
> 2. 然后运行python脚本并带上相应的参数
```sh
python demo/image_demo.py \
        data/street_uk.jpeg \
        configs/pspnet/pspnet_r50-d8_4xb2-40k_cityscapes-512x1024.py \
        https://download.openmmlab.com/mmsegmentation/v0.5/pspnet/pspnet_r50-d8_512x1024_40k_cityscapes/pspnet_r50-d8_512x1024_40k_cityscapes_20200605_003338-2966598c.pth \
        --out-file outputs/B1_uk_pspnet.jpg \
        --device cuda:0 \
        --opacity 0.5
```
其中需要**输入图片**，**模型配置**，**模型权重**，**输出图片**，**在什么设备上运行**  
这种显然是脚本运行的方式，这么长当然不怎么方便
## 利用推理API
### **首先了解三个重要的API：**
- mmseg.apis.init_model：从配置文件初始化一个分割器。
- inference_model：使用分割器推理图像。
- mmseg.apis.show_result_pyplot：在图像上可视化分割结果。
```python
import torch
from mmseg.apis import init_model
from mmseg.apis import inference_model
from mmengine.model.utils import revert_sync_batchnorm

# img_path = 'demo/demo.png'
img_path = 'data/street_uk.jpeg'

# 模型 config 配置文件
config_file = 'configs/mask2former/mask2former_swin-l-in22k-384x384-pre_8xb2-90k_cityscapes-512x1024.py'

# 模型 checkpoint 权重文件
checkpoint_file = 'https://download.openmmlab.com/mmsegmentation/v0.5/mask2former/mask2former_swin-l-in22k-384x384-pre_8xb2-90k_cityscapes-512x1024/mask2former_swin-l-in22k-384x384-pre_8xb2-90k_cityscapes-512x1024_20221202_141901-28ad20f1.pth'

# 初始化模型，并加载权重
model = init_model(config_file, checkpoint_file, device='cuda:0')

if not torch.cuda.is_available():
    model = revert_sync_batchnorm(model)

# 模型推理
result = inference_model(model, img_path)
```
返回的result的格式是SegDataSample，包含了**gt_sem-seg**（语义分割的标注），**pred_sem_seg**（语义分割的预测），**seg_logits**（模型最后一层的输出结果）

### **然后了解图片的可视化**
1. 利用cv2.addweight图片融合
2. 使用MMseg官方的配色：show_result_pyplot()
3. 自定义配色：使用图片的索引格式

### **最后是视频的推理**
本质上也是图片推理，把每一帧推理结果按次序保存，最终合成一个视频文件。
> **tips**：
> 1. 设置临时目录，最后删除图片，就像缓存一样的效果。
> 2. config文件与checkpoint文件是相对应的。

## 附上参考
MMSeg官网教程的推理部分：[https://mmsegmentation.readthedocs.io/zh_CN/latest/user_guides/3_inference.html](https://mmsegmentation.readthedocs.io/zh_CN/latest/user_guides/3_inference.html)  
子豪兄教程：[https://github.com/TommyZihao/MMSegmentation_Tutorials/blob/main/20230816](https://github.com/TommyZihao/MMSegmentation_Tutorials/blob/main/20230816)