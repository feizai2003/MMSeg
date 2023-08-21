# MMSegmentation的安装
> 官网链接：[https://mmsegmentation.readthedocs.io/zh_CN/latest/get_started.html](https://mmsegmentation.readthedocs.io/zh_CN/latest/get_started.html)  
> [子豪兄链接](https://github.com/TommyZihao/MMSegmentation_Tutorials/blob/main/20230715/%E3%80%90A1%E3%80%91%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AEMMSegmentation.ipynb)
- [MMSegmentation的安装](#mmsegmentation的安装)
  - [安装Pytorch](#安装pytorch)
  - [安装MMSegmentation](#安装mmsegmentation)
  - [PR经历](#pr经历)
    - [那么我们当然就可以提个PR去修复它了](#那么我们当然就可以提个pr去修复它了)


## 安装Pytorch
1. 安装[miniconda](https://www.anaconda.com/)
2. 创建虚拟环境并激活
```conda
conda create --name openmmlab python=3.8 -y
conda activate openmmlab
```
3. 安装[Pytorch](https://pytorch.org/get-started/previous-versions/)
    - CPU版本：
    ```conda
    conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cpuonly -c pytorch
    ```
    - GPU版本：(使用，本机：GTX1650，CUDA11.4)
    > 仅仅对于训练来说，安装CUDA不是必须的,推理部署阶段还是需要的，这里就安装了CUDA（并非系统自带的CUDA）
    ```conda
    conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.3 -c pytorch
    ```
4. 验证安装情况  
在openmmlab虚拟环境中，输入python打开python命令行输入（退出快捷键Ctrl + Z）
```python
import torch
print('Pytorch 版本', torch.__version__)
print('CUDA 是否可用',torch.cuda.is_available())
```
```txt
Pytorch 版本 1.12.1
CUDA 是否可用 True
(类似且没有报错即是成功)
```

## 安装MMSegmentation
1. 使用 MIM 安装 MMCV
```pip
pip install -U openmim
mim install mmengine
mim install "mmcv>=2.0.0"
```
2. 验证MMCV安装情况
同样打开虚拟环境输入python打开命令行
```python
import mmcv
from mmcv.ops import get_compiling_cuda_version, get_compiler_version
print('MMCV版本', mmcv.__version__)
print('CUDA版本', get_compiling_cuda_version())
print('编译器版本', get_compiler_version())
```
```txt
MMCV版本 2.0.1
CUDA版本 11.3
编译器版本 MSVC 192930148
(类似且没有报错即是成功)
```

3. 安装MMSegmentation
    - 源码安装(使用)
    ```pip
    git clone -b main https://github.com/open-mmlab/mmsegmentation.git
    cd mmsegmentation
    pip install -v -e .
    ```
    - 依赖库安装
    ```pip
    pip install "mmsegmentation>=1.0.0"
    ```

4. 验证MMSegmentation安装情况
```python
import mmseg
from mmseg.utils import register_all_modules
from mmseg.apis import inference_model, init_model
print('mmsegmentation版本', mmseg.__version__)
```
```txt
mmsegmentation版本 1.1.1
(类似且没有报错即是成功)
```
## PR经历
> 当然我是使用源码安装，未出现问题，但是看到群友的交流，我就去尝试了另一种验证mmsegmentation方式

在官网安装[文档](https://mmsegmentation.readthedocs.io/zh_CN/latest/get_started.html)中，在通过依赖库的形式安装mmsegmentation后，需要对此检验是否安装成功。
```
# 图片的推理没有问题，后面视频帧的代码有误
from mmseg.apis import inference_model, init_model, show_result_pyplot
import mmcv

config_file = 'pspnet_r50-d8_4xb2-40k_cityscapes-512x1024.py'
checkpoint_file = 'pspnet_r50-d8_512x1024_40k_cityscapes_20200605_003338-2966598c.pth'

# 根据配置文件和模型文件建立模型
model = init_model(config_file, checkpoint_file, device='cuda:0')

video = mmcv.VideoReader('video.mp4')
for frame in video:
   result = inference_model(model, frame)
   show_result_pyplot(model, frame, result, wait_time=1)
```
通过查找相应的api（0.x时是inference_segmentor,1.x修改为inference_model）和翻看0.x的文档，可以确定是在迁移新版本Doc时候的修改遗漏。  
### 那么我们当然就可以提个PR去修复它了
简单来说简单的PR当然简单
1. Fork到本地仓库
2. 把自己Fork的仓库clone到本地
3. 安装pre-commit
4. 按照指定分支创建新分支
4. 修改内容
5. git commit
6. git pull
7. git push
8. Github上回到刚刚Fork的仓库填写PR详情
> 1. 克隆时https不稳定，随网络影响大，可以改用ssh的方式  
> 2. 使用pre-commit run --all-files时，在我的conda环境下会报ssl相关的错误，目前还未解决，但是使用原生python3.11不会问题，但是还是不能确定是python版本还是conda的问题（TODO

PR链接:[https://github.com/open-mmlab/mmsegmentation/pull/3261](https://github.com/open-mmlab/mmsegmentation/pull/3261)