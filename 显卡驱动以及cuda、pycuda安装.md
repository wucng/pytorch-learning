安装成功

[显卡驱动以及cuda、pycuda安装(安装成功)](http://note.youdao.com/noteshare?id=9999a01625c7827d57e8458bd1d6d198&sub=9F3E1AF839F34078A42E6765E2F69216)

http://blog.csdn.net/autocyz/article/details/52299889

-----

软件：
Ubuntu16.04.1 LTS
NVIDIA-375  (下载安装)
cuda_8.0.61_375.26_linux.run
cudnn-8.0-linux-x64-v5.1.tgz

# 1、安装nvidia驱动

删除之前所安装的nVidia驱动。

sudo apt-get remove --purge nvidia-*(需要清除干净)
sudo apt-get remove --purge xserver-xorg-video-nouveau
重启电脑

首先去官网上查看适合你GPU的驱动（http://www.nvidia.com/Download/index.aspx?lang=en-us） 

例如，本人的GPU适合的驱动如图： 
![这里写图片描述](http://img.blog.csdn.net/20171213113047539?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

执行如下语句，安装

```python
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-375  # 375 对应是显卡型号，根据上面的搜索的型号修改  我这里是375
sudo apt-get install mesa-common-dev
sudo apt-get install freeglut3-dev
```
执行完上述后，重启（reboot）。 

重启后输入：

```
nvidia-smi
```
如果出现了你的GPU列表，则说明驱动安装成功了。另外也可以通过

```
nvidia-settings
```
查看自己机器上详细的GPU信息，本人机器的信息如下： 

![这里写图片描述](http://img.blog.csdn.net/20171213113157846?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


# 2、安装CUDA
cuda是nvidia的编程语言平台，想使用GPU就必须要使用cuda。 
从这里下载cuda的安装文件 
https://developer.nvidia.com/cuda-release-candidate-download 

![这里写图片描述](http://img.blog.csdn.net/20171213113240832?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

注意这里下载的是cuda8.0的runfile（local）文件。 
这里是nvidia给出的官方安装指南（遇到问题时可以查阅）： 
http://docs.nvidia.com/cuda/cuda-installation-guide-linux/#axzz4HIBXnwyt 
下载完cuda8.0后，执行如下语句，运行runfile文件：

```
sudo sh cuda_8.0.27_linux.run
```
如果出现以下错误：

```
Installing the NVIDIA display driver...
It appears that an X server is running. Please exit X before installation. If you're sure that X is not running, but are getting this error, please delete any X lock files in /tmp.
```

先执行`sudo /etc/init.d/lightdm stop`
在执行 `sudo sh cuda_8.0.27_linux.run`

安装选项出现这个
![这里写图片描述](http://img.blog.csdn.net/20171213113348915?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

选择no  因为前面已经安装好了驱动

最后出现这个说明安装成功
![这里写图片描述](http://img.blog.csdn.net/20171213113419102?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
安装完毕后，再声明一下环境变量，并将其写入到 `~/.bashrc` 的尾部:

```
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export CUDA_ROOT=/usr/local/cuda-8.0:$CUDA_ROOT
```
![这里写图片描述](http://img.blog.csdn.net/20171213113455008?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后设置环境变量和动态链接库，在命令行输入：

```
$ sudo gedit /etc/profile
```
在打开的文件末尾加入：

```
# export PATH = /usr/local/cuda/bin:$PATH
export PATH=/usr/local/cuda/bin:$PATH
```

最后`source /etc/profile`   看看是否出错

保存之后，创建链接文件：

```
sudo gedit /etc/ld.so.conf.d/cuda.conf
```
在打开的文件中添加如下语句：

```
/usr/local/cuda/lib64
```
然后执行

```
sudo ldconfig
```
使链接立即生效。

如果出现 以下错误
![这里写图片描述](http://img.blog.csdn.net/20171213113631486?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
参考：http://blog.csdn.net/langb2014/article/details/54376716

解决方法：

```
sudo mv /usr/lib/nvidia-375/libEGL.so.1 /usr/lib/nvidia-375/libEGL.so.1.org
sudo mv /usr/lib32/nvidia-375/libEGL.so.1 /usr/lib32/nvidia-375/libEGL.so.1.org
sudo ln -s /usr/lib/nvidia-375/libEGL.so.375.39 /usr/lib/nvidia-375/libEGL.so.1
sudo ln -s /usr/lib32/nvidia-375/libEGL.so.375.39 /usr/lib32/nvidia-375/libEGL.so.1
```
# 3、测试cuda的Samples

```
cd /usr/local/cuda-8.0/samples/1_Utilities/deviceQuery
make
sudo ./deviceQuery
```
如果显示的是一些关于GPU的信息，则说明安装成功了。
![这里写图片描述](http://img.blog.csdn.net/20171213113723043?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


# 4、使用cudnn
首先去官网下载你需要的cudnn，下载的时候需要注册账号。选择对应你cuda版本的cudnn下载。这里我下载的是cudnn5.1，是个压缩文件（.tgz）
![这里写图片描述](http://img.blog.csdn.net/20171213113752853?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```
 tar -zxvf cudnn-8.0-linux-x64-v5.1.tgz
```

下载完cudnn5.0之后进行解压，cd进入cudnn5.1解压之后的include目录，在命令行进行如下操作：

```
sudo cp cudnn.h /usr/local/cuda/include/    #复制头文件
```
再将cd进入lib64目录下的动态文件进行复制和链接：

```
sudo cp lib* /usr/local/cuda/lib64/    #复制动态链接库
cd /usr/local/cuda/lib64/
sudo rm -rf libcudnn.so libcudnn.so.5    #删除原有动态文件
sudo ln -s libcudnn.so.5.0.5 libcudnn.so.5  #生成软衔接
sudo ln -s libcudnn.so.5 libcudnn.so      #生成软链接
```

# 5、pycuda

```
sudo apt-get install python3-pycuda就好了， 这里我安装的是python3的
sudo apt-get install python-pycuda   # python2 安装
```

参考：https://wiki.tiker.net/PyCuda/Installation/Linux/Ubuntu
Ubuntu 16.04 comes with CUDA support through the repositories: install nvidia-cuda-toolkit, nvidia-cuda-dev and python-pycuda using apt-get.

安装完成后 `sudo /etc/init.d/lightdm restart`  #切换到图像界面

补充：
Anaconda 安装pycuda
先安装Anaconda   如执行： `sudo bash  Anaconda**.sh`
再执行： pip install  pycuda 
注：但是安装后使用不了pycuda
