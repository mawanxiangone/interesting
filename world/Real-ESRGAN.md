#! https://zhuanlan.zhihu.com/p/670197018
github项目地址 [Real-ESRGAN](tps://github.com/xinntao/Real-ESRGAN/blob/master/README_CN.md).

# Real-ESRGAN的使用

## conda 创建虚拟环境

- 建议用conda的虚拟环境,python版本用3.11.0,我用3.12.0试过,3.12.0版本目前不匹配,会影响Real-ESRGAN安装

```bash

conda create --name my3110 python=3.11.0
conda activate my3110
```

## 本地构造Real-ESRGAN

```bash
# 下载项目到本地,前提已准备有git环境,可以从github拉取,如果没有git,直接github上下载下来也行
git clone https://github.com/xinntao/Real-ESRGAN.git
cd Real-ESRGAN
```

![1701456008769](image/Real-ESRGAN/1701456008769.png)

```bash
# 安装环境,建议用国内源
pip.exe install basicsr -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com

pip.exe install facexlib -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com

pip.exe install gfpgan -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com

pip.exe install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com

python.exe .\setup.py develop
```

## 下载训练模型

```bash
# 第一次执行动作的主要目的让它自动下载模型,如下命令,-n 指定模型RealESRGAN_x4plus,RealESRGAN_x4plus本地不存在,则会自动下载这个模型
python.exe inference_realesrgan.py -n RealESRGAN_x4plus -i D:\Tool\windowstool\github\linux\微信图片_20231114002958.jpg --face_enhance
```

![1701456400868](image/Real-ESRGAN/1701456400868.png)

## 修复图片命令解释

```bash
usage: inference_realesrgan.py [-h] [-i INPUT] [-n MODEL_NAME] [-o OUTPUT] [-dn DENOISE_STRENGTH] [-s OUTSCALE] [--model_path MODEL_PATH] [--suffix SUFFIX] [-t TILE] [--tile_pad TILE_PAD] [--pre_pad PRE_PAD] [--face_enhance] [--fp32]
                               [--alpha_upsampler ALPHA_UPSAMPLER] [--ext EXT] [-g GPU_ID]

options:
  -h, --help            show this help message and exit
  -i INPUT, --input INPUT
                        Input image or folder
  -n MODEL_NAME, --model_name MODEL_NAME
                        Model names: RealESRGAN_x4plus | RealESRNet_x4plus | RealESRGAN_x4plus_anime_6B | RealESRGAN_x2plus | realesr-animevideov3 | realesr-general-x4v3
  -o OUTPUT, --output OUTPUT
                        Output folder
  -dn DENOISE_STRENGTH, --denoise_strength DENOISE_STRENGTH
                        Denoise strength. 0 for weak denoise (keep noise), 1 for strong denoise ability. Only used for the realesr-general-x4v3 model

# -dn指定了在进行图像超分辨率重建时,去除图像中噪声的程度.
## 当设置为 0 时,表示弱去噪(保留噪声).
## 当设置为 1 时,表示强去噪能力.
# 使用 realesr-general-x4v3 模型时有效.调整此参数可能会对重建后图像的视觉效果产生影响,因为较强的去噪可能会损失图像细节,而较弱的去噪可能会保留更多原始图像中的噪声.

  -s OUTSCALE, --outscale OUTSCALE
                        The final upsampling scale of the image

# -s/--outscale OUTSCALE 是用来指定图像最终的上采样比例的.在超分辨率重建过程中,它控制了最终生成图像的放大比例.
# 如果将此参数设置为 4,则最终生成的图像将是原始输入图像尺寸的 4 倍.
# 增加放大倍数也可能导致更长的处理时间和更多的计算资源需求

  --model_path MODEL_PATH
                        [Option] Model path. Usually, you do not need to specify it
  --suffix SUFFIX       Suffix of the restored image

# --suffix SUFFIX 参数用于指定恢复(重建)图像的后缀.在处理图像时,这个参数允许您为重建后的图像文件添加一个后缀标识.

  -t TILE, --tile TILE  Tile size, 0 for no tile during testing

# -t/--tile TILE: 切片大小,用于处理大尺寸图像,0 表示在测试过程中不切片.

  --tile_pad TILE_PAD   Tile padding

# --tile_pad TILE_PAD 参数用于设置图像切片(tiling)时的填充大小.
# 在进行图像处理时,如果图像尺寸较大,可能会将图像切成较小的块进行处理.--tile_pad 参数允许您指定在每个图像块周围添加的填充量.
# 这个参数决定了图像切片时的额外填充量,以确保图像块之间在处理过程中没有信息丢失或边界伪影.通过设置这个参数,您可以控制图像块之间的填充大小,以便更好地处理大型图像,并确保图像块之间的无缝连接.

  --pre_pad PRE_PAD     Pre padding size at each border

# --pre_pad PRE_PAD 参数用于设置图像在每个边界预填充(pre-padding)的大小.

  --face_enhance        Use GFPGAN to enhance face

# --face_enhance: 使用 GFPGAN 增强人脸

  --fp32                Use fp32 precision during inference. Default: fp16 (half precision).

# --fp32: 在推理过程中使用 fp32 精度,默认是 fp16(半精度)

  --alpha_upsampler ALPHA_UPSAMPLER
                        The upsampler for the alpha channels. Options: realesrgan | bicubic

# --alpha_upsampler ALPHA_UPSAMPLER 参数用于指定 alpha 通道(透明度通道)的上采样方法.

  --ext EXT             Image extension. Options: auto | jpg | png, auto means using the same extension as inputs
  -g GPU_ID, --gpu-id GPU_ID
                        gpu device to use (default=None) can be 0,1,2 for multi-gpu

# GPU_ID:指定要使用的 GPU 设备的 ID.可以是单个 GPU 设备的 ID(如 0、1、2),也可以是多个 GPU 设备的 ID,用逗号分隔(例如 0,1,2),以便在多 GPU 环境下进行处理.

```

## 修复图片测试

我的笔记本性能可能有问题,试了几个模板都提示CUDA问题:

![1701623319391](image/Real-ESRGAN/1701623319391.png)

乱测试选择如下成功:
- 应该加上 --fp32 就行了
- 路径最好是英文,中文名图片出错了
- 有的模型消耗资源,直接进行不下去,建议选择消耗资源小的模型

```bash
python.exe inference_realesrgan.py -i D:\Tool\windowstool\github\linux\20231114002958.jpg -o D:\Tool\windowstool\github\20231114002958_1.jpg -n realesr-general-x4v3 --fp32 -s 2
```

![1701623419433](image/Real-ESRGAN/1701623419433.png)

对比可以看出修复后的效果十分明显

![1701623468059](image/Real-ESRGAN/1701623468059.png)

## 修复视频命令解释



# this

## this

### this