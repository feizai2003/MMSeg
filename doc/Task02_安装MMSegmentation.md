# MMSegmentation的安装
> 官网链接：[https://mmsegmentation.readthedocs.io/zh_CN/latest/get_started.html](https://mmsegmentation.readthedocs.io/zh_CN/latest/get_started.html)  
> [子豪兄链接](https://github.com/TommyZihao/MMSegmentation_Tutorials/blob/main/20230715/%E3%80%90A1%E3%80%91%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AEMMSegmentation.ipynb)


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