ubuntu [开机启动图像界面和命令行界面切换方式](https://blog.csdn.net/londa/article/details/90905575)

## nvidia显卡和cuda安装
使用实验室服务器(已安装nvidia驱动和cuda，[版本应该对应要求](https://github.com/RangiLyu/nanodet#requirements)
#### 1.命令行查看显卡运行状态 nvidia-smi
```
NVIDIA-SMI 450.57 Driver Version:450.57 CUDA Version:11.0
```
#### 2.命令行查看cuda版本 nvcc -V
```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Sun_Jul_28_19:07:16_PDT_2019
Cuda compilation tools, release 10.1, V10.1.243
```
#### 3.[创建conda虚拟环境](http://202.120.54.12/?p=840)

I.初始化Anaconda3(需要每次使用时在命令行执行)
```
source /usr/local/anaconda3/bin/activate
```
执行完成后命令行前出现(base)，说明已经启动了Anaconda

II.创建自己的环境，在该环境中可以安装python相关的包
```
conda create -n your_env_name python=X.X（2.7、3.5)
```

III.激活环境
```
conda activate your_env_name
```

IV.按照Nanodet要求配置环境(https://github.com/RangiLyu/nanodet#install)
> conda install 和 pip install
```conda install pytorch torchvision cudatoolkit=11.0 -c pytorch ```
> 要注意此处cuda的版本应该对应[nvcc的版本](https://zhuanlan.zhihu.com/p/163222176)
  所以此处服务器为`conda install pytorch torchvision torchaudio cudatoolkit=10.1 -c pytorch`



#### 4.[测试demo](https://github.com/RangiLyu/nanodet#pytorch-demo)
CONFIG_PATH对应config/nanodet-m.yml
MODEL_PATH从google硬盘下载(https://drive.google.com/file/d/1EhMqGozKfqEfw8y9ftbi1jhYu86XoW62/view?usp=sharing)对应nanodet_m.pth
IMAGE_PATH对应自己目录的任意图像文件

#### 5.[训练自己的模型](https://github.com/RangiLyu/nanodet#how-to-train)

I.[下载config文件即yml模板](https://github.com/RangiLyu/nanodet/blob/main/config/nanodet_custom_xml_dataset.yml)
II.修改该yml模板内容 
> 训练模型结果的保存目录，类别，xml文件夹和pic文件夹(在yml模板中注释位置)
III.测试 
> 训练结束后，在保存目录下找到model_last.pth，就是最终训练结果 
  测试效果运行`python demo/demo.py video --config CONFIG_PATH --model MODEL_PATH --path VIDEO_PATH`修改对应config和model路径即可

#### 6.[部署训练好的模型(将pytorch模型转换为IR)](https://github.com/RangiLyu/nanodet#how-to-deploy)

I.[先将pytorch(.pth)转换为.onnx模型](https://convertmodel.com/)  
> 或者利用tools文件夹export.py，修改cfg_path = r"config/test.yml"和model_path = r"model/model_last.pth"
  [记得修改optset_version=10](https://github.com/anhnktp/yolov5_openvino/issues/3)  
II.[简化onnx模型](https://github.com/daquexian/onnx-simplifier#python-version)   
III.[转换onnx为openvino的IR](https://docs.openvinotoolkit.org/cn/latest/openvino_docs_MO_DG_prepare_model_convert_model_Converting_Model.html)
> 使用openvino工具包中mo.py工具[得到bin,mapping,xml三个IR文件](https://docs.openvinotoolkit.org/cn/latest/openvino_docs_MO_DG_prepare_model_convert_model_Convert_Model_From_ONNX.html)
IV.写openvino的推理进行部署


