ubuntu 开机启动图像界面和命令行界面
https://blog.csdn.net/londa/article/details/90905575

使用实验室服务器(已安装nvidia驱动和cuda，版本应该对应要求https://github.com/RangiLyu/nanodet#requirements)
运行nvidia-smi
NVIDIA-SMI 450.57 Driver Version:450.57 CUDA Version:11.0
运行nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Sun_Jul_28_19:07:16_PDT_2019
Cuda compilation tools, release 10.1, V10.1.243


创建自己的anaconda环境(http://202.120.54.12/?p=840)
1.初始化（需要每次使用时在命令行执行）
使用 Anaconda3
source /usr/local/anaconda3/bin/activate
执行完成后命令行前出现(base)，说明已经启动了Anaconda
2.创建自己的环境
在该环境中可以安装python相关的包
conda create -n your_env_name python=X.X（2.7、3.5)
3.激活环境
conda activate your_env_name




按照官网install步骤配置环境(https://github.com/RangiLyu/nanodet#install)
第二步 conda install pytorch torchvision cudatoolkit=11.0 -c pytorch 要注意此处cuda的版本应该对应nvcc的版本
所以此处服务器为conda install pytorch torchvision torchaudio cudatoolkit=10.1 -c pytorch
（https://zhuanlan.zhihu.com/p/163222176）



测试demo(https://github.com/RangiLyu/nanodet#pytorch-demo)
CONFIG_PATH对应config/nanodet-m.yml
MODEL_PATH从google硬盘下载(https://drive.google.com/file/d/1EhMqGozKfqEfw8y9ftbi1jhYu86XoW62/view?usp=sharing)对应nanodet_m.pth
IMAGE_PATH对应自己目录的任意图像文件





训练自己的模型(https://github.com/RangiLyu/nanodet#how-to-train)
1.下载config文件即yml模板(https://github.com/RangiLyu/nanodet/blob/main/config/nanodet_custom_xml_dataset.yml)
2.修改该yml模板内容
训练模型结果的保存目录，类别，xml文件夹和pic文件夹(在yml模板中注释位置)
3.测试
训练结束后，在保存目录下找到model_last.pth，就是最终训练结果
测试效果运行python demo/demo.py video --config CONFIG_PATH --model MODEL_PATH --path VIDEO_PATH修改对应config和model路径即可




部署训练好的模型(将pytorch模型转换为IR)(https://github.com/RangiLyu/nanodet#how-to-deploy)
步骤1先将pytorch(.pth)转换为.onnx模型：(https://convertmodel.com/)
利用tools文件夹export.py，修改cfg_path = r"config/test.yml"和model_path = r"model/model_last.pth"
步骤2简化onnx模型(https://github.com/daquexian/onnx-simplifier#python-version)
步骤3转换onnx为openvino的IR(https://docs.openvinotoolkit.org/cn/latest/openvino_docs_MO_DG_prepare_model_convert_model_Converting_Model.html)

https://docs.openvinotoolkit.org/cn/latest/openvino_docs_MO_DG_prepare_model_convert_model_Convert_Model_From_ONNX.html

