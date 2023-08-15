# Label-Studio * SAM 半自动化标注
> 1. 什么是标注？  
对某个对象赋予一个可视化的意义，是一个为了更好区分事物的方式
> 2. 需要标注？  
在现实生活中，”标注“这个行为是每时每刻，潜移默化在发生的。一个例子就是姓名-学号-班级等等
> 3. 数据标注  
数据标注为通过分类、画框、标注、注释等，对图片、语音、文本等数据进行处理，标记对象的特征，以作为机器学习基础素材的过程

Label-Studio 和 SAM (Segment Anything) 半自动化标注方案  
- Point2label
- Bbox2label

[SAM](https://github.com/heartexlabs/label-studio) (Segment Anything) 是 Meta AI 推出的分割一切的模型。       
[Label Studio](https://github.com/heartexlabs/label-studio) 是一款优秀的标注软件，覆盖图像分类、目标检测、分割等领域数据集标注的功能。  
[喵喵数据集地址](https://download.openmmlab.com/mmyolo/data/cat_dataset.zip),这里使用的数据集

## 环境配置
> Windows + GTX1650 + CUDA11.4
1. 创建虚拟环境并激活
    ```conda
    conda create -n rtmdet-sam python=3.9 -y
    conda activate rtmdet-sam
    ```
2. 克隆 OpenMMLab PlayGround项目
    ```git
    git clone git@github.com:open-mmlab/playground.git
    ```
3. 安装Pytorch
    ```pip
    pip install torch==1.10.1+cu113 torchvision==0.11.2+cu113 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu113/torch_stable.html
    ```
4. 安装SAM，下载预训练模型
    ```pip
    conda install pycocotools -c conda-forge 
    pip install opencv-python pycocotools matplotlib onnxruntime onnx timm
    pip install git+https://github.com/facebookresearch/segment-anything.git

    # 下载sam预训练模型
    https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth
    # 如果想要分割的效果好请使用 sam_vit_h_4b8939.pth 权重
    https://dl.fbaipublicfiles.com/segment_anything/sam_vit_l_0b3195.pth
    https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth

    # 下载 HQ-SAM 预训练模型
    https://huggingface.co/lkeab/hq-sam/resolve/main/sam_hq_vit_b.pth
    https://huggingface.co/lkeab/hq-sam/resolve/main/sam_hq_vit_h.pth
    https://huggingface.co/lkeab/hq-sam/resolve/main/sam_hq_vit_l.pth

    # 下载 mobile_sam 预训练模型
    https://raw.githubusercontent.com/ChaoningZhang/MobileSAM/master/weights/mobile_sam.pt
    将其放置到playground/label_anything目录下
    ```
5. 安装 Label-Studio 和 label-studio-ml-backend
    ```pip
    pip install label-studio==1.7.3
    pip install label-studio-ml==1.0.9
    ```
