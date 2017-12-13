http://pytorch.org/ 官网


----------


安装
If you want to compile with CUDA support, install

  - [NVIDIA CUDA 7.5](https://developer.nvidia.com/cuda-downloads) or above
  - [NVIDIA cuDNN v5.x](https://developer.nvidia.com/cudnn) or above

# 0、先安装完成 cuda（在root环境下安装即可）
参考： [TensorFlow安装-Linux篇](http://blog.csdn.net/wc781708249/article/details/77989339) 

# 1、先使用virtualenv 创建环境（或 使用Anaconda 创建环境）

https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/ Anaconda下载地址
推荐使用Anaconda（可以自动更改python的版本）
```python
bash Anaconda3-4.4.0-Linux-x86_64.sh 

conda create -n tensorflow python=3.5.3
# 或
conda create -n tensorflow python=2.7

source activate tensorflow 
```
再进行安装，即可

# 2、进入官网 打开下面这个页面，找到对应的安装版本命令

![这里写图片描述](http://img.blog.csdn.net/20171213112148640?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
```python
pip install http://download.pytorch.org/whl/cu80/torch-0.2.0.post3-cp27-cp27mu-manylinux1_x86_64.whl
pip install torchvision
# 或
pip3 install http://download.pytorch.org/whl/cu80/torch-0.2.0.post3-cp35-cp35m-manylinux1_x86_64.whl
```

如果第一条语句失败，可以按下面语句安装
```python
wget http://download.pytorch.org/whl/cu80/torch-0.2.0.post3-cp27-cp27mu-manylinux1_x86_64.whl
pip install torch-0.2.0.post3-cp27-cp27mu-manylinux1_x86_64.whl
pip install torchvision
```
# 3、按命令执行安装即可
注：如果Python版本太低，需要手动升级 
参考：http://www.cnblogs.com/fx2008/p/6177940.html
