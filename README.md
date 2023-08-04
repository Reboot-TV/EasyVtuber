# EasyVtuber  

> Use the money to buy skins to buy another 3080!！

![OBS Record With Transparent Virtural Cam Input](assets/new_sample.gif)

Fork from https://github.com/GunwooHan/EasyVtuber
to solve the quality problem of face capture, and reverse port the original demo https://github.com/pkhungurn/talking-head-anime-2-demo about ifacialmocap ios surface capture logic
and omit ifacialmocap pc end, through UDP direct connection to make ios surface capture refresh rate up to 60fps, which solves the bottleneck of surface capture refresh rate
Finally, add supporting Shader to the OBS virtual camera solution used in EasyVtuber Support, unlock the RGBA output capability, you can use it directly without a green back

[Video introduction and installation instructions](https://www.bilibili.com/video/BV1uu411r7DR)  

## Requirements  

### Hardware
- iPhone that supports FaceID (using ifacialmocap software, which needs to be purchased, requires a stable WIFI connection) or webcam (using OpenCV)
- NVIDIA graphics card that supports PyTorch CUDA (reference: TUF RTX3080 silent frequency 40FPS 80% occupied)

### Software
This solution is available for testing on Windows 10
- Python>=3.8
- OBS or Unity Capture (virtual camera solution)
- Photoshop or other image processing software
- Scientific Internet access plan, the ability to understand English websites and report errors

## Installation (embedded Python version)  

In the bin folder is a lightweight operating environment based on the Win64 embedded version of Python3.10.5
For users who just need to experience this library, it is recommended to use this method to install.

### Download the ZIP and unzip or clone this Repo

Click [`Download ZIP`](../../archive/master.zip) to download and unzip, or use git to clone the repository to where you can find it.
It takes about 5.5G of hard disk space to fully expand the venv.


### 下载预训练模型  

Use the 00B shortcut or the following link to download the model file
https://github.com/pkhungurn/talking-head-anime-3-demo#download-the-models

Download the compressed file from the original repo (this Dropbox link) and
extract it to data/modelsfolder, the correct directory hierarchy placeholder.txtis

```
+ models
  - separable_float
  - separable_half
  - standard_float
  - standard_half
  - placeholder.txt
```

If you are not sure whether you have decompressed to the correct location, you can use00.检查并补齐必需文件.bat


### Build the operating environment

Run the one suitable for your region 01A.构建运行环境（默认源）.bat or 01B.构建运行环境（国内源）.bat
this script will use pip to install all required dependencies in the bin directory. The
two scripts can replace each other and support continuing from where they were interrupted.
If there is a network-related error, just turn off the console and adjust the network. Just run it again The output of running the script again after the full installation is complete is shown in the figure. Generally speaking, if there is no red letter in the whole installation process, it means that it is successfully completed.

![step01success](assets/01Success.png)  

完全安装完成后再次运行脚本的输出如图所示。一般来说安装全程没有红字就是成功结束。  

### Test results using the launcher

Run 02B启动器（调试输出）.bat
and click directly at the bottom of the interface. Save & LaunchIf you see the pop-up opencv output form, the installation is successfully completed

![img.png](assets/02success.png)

### Configure input and output devices

After successful debugging output, please move to the next section of input and output devices for further configuration to output to OBS.

## Installation (Venv version)  
如果你还需要使用之前的Venv方案，请参考以下步骤  

### 下载ZIP并解压或者克隆本Repo  
点击[`Download ZIP`](../../archive/master.zip) 下载并解压，或者使用git克隆该仓库到你找得到的地方。  
完整展开venv需要大约5.5G的硬盘空间。  

### 创建虚拟环境
此处默认你有一个正确的Python安装，不会装的话请使用前文的嵌入式方案  
在项目目录下运行`python -m venv venv`创建虚拟环境

### 切换到虚拟环境
之后的操作都需要切换到虚拟环境中进行，分辨方式为命令行前会有`(venv)`标识  
在控制台运行`venv\Scripts\activate.bat`切换到刚才创建的虚拟环境  
之后你的python pip等操作都会在虚拟环境中执行  

### 安装依赖  
在虚拟环境中执行以下命令  
`pip install -r .\requirements.txt`  
`pip install torch --extra-index-url https://download.pytorch.org/whl/cu113`  

### 运行启动器  
在虚拟环境中执行以下命令  
`python launcher.py`  


## Installation(Conda version)  

### 克隆本Repo  

克隆完以后如果直接用Pycharm打开了，先不要进行Python解释器配置。

### Python和Anaconda环境  

这个项目使用Anaconda进行包管理  
首先前往https://www.anaconda.com/ 安装Anaconda  
启动Anaconda Prompt控制台  
国内用户建议此时切换到清华源（pip和conda都要换掉，尤其是conda的Pytorch Channel，pytorch本体太大了）  
然后运行 `conda env create -f env_conda.yaml` 一键安装所有依赖  
如果有报错（一般是网络问题），删掉配了一半的环境，`conda clean --all`清掉下载缓存，调整配置后再试

安装完成后，在Pycharm内打开本项目，右下角解释器菜单点开，`Add Interpreter...`->`Conda Environment`->`Existing environment`  
选好自己电脑上的`conda.exe`和刚才创建好的`talking-head-anime-2-demo`环境内的`python.exe`    
点击OK，依赖全亮即可  

### 下载预训练模型  

https://github.com/pkhungurn/talking-head-anime-3-demo#download-the-models  
从原repo中下载（this Dropbox link）的压缩文件  
解压到`data/models`文件夹中，与`placeholder.txt`同级  
正确的目录层级为  
```
+ models
  - separable_float
  - separable_half
  - standard_float
  - standard_half
  - placeholder.txt
```  

### 运行启动器  
在Conda环境中执行以下命令  
`python launcher.py`  

## I/O Device

#### OBS Virtual Camera

At present, this solution is recommended. UnityCapture has an unidentified performance bottleneck.
If you choose to do the keying yourself, you can directly output to obs. If you need RGBA support, you need to use an additional Shader
to download and install StreamFX https://github.com After downloading Shader from /Xaymar/obs-StreamFX
(thanks to the root of the tree) https://github.com/shugen002/shader/blob/master/merge%20alpha2.hlsl , run with parameters and you will see such an output screen, The transparent channel is sent in grayscale alone, and then add a filter to the video capture device in OBS - shader - select the one you downloaded - close, so that the transparent channel will be applied back to the image on the left, you may need to manually adjust the cropping to the right Cut off the useless screen (if you can't see the shader filter, it means that StreamFX is not installed or OBS is not the latest version)
--alpha_split

![alpha split](assets/alphasplit.png)  

merge alpha2.hlsl


#### UnityCapture  

If you need to use transparent channel output, refer to https://github.com/schellingb/UnityCapture#installation to install UnityCapture,
you only need to finish Install.bat normally, and you can see the corresponding device (Unity Video Capture) in OBS

After adding the camera in OBS, you need to manually configure the camera properties once to support ARGB
right-click properties-deactivate-resolution type customization-resolution 512x512 ( --output_sizeconsistent with the parameters)-video format ARGB-activate

#### iFacialMocap  

[https://www.ifacialmocap.com/download/  
你大概率需要购买正式版（非广告，只是试用版不太够时长）  
购买之前确认好自己的设备支持  
**不需要下载PC软件**，装好iOS端的软件即可，连接信息通过参数传入Python  ](https://www.ifacialmocap.com/download/
You will most likely need to buy the official version (not an advertisement, but the trial version is not long enough).
Before buying, confirm that your device supports it .
You don’t need to download PC software and install the iOS software. That is, the connection information is passed to Python through parameters)

#### OpenSeeFace  

https://github.com/emilianavt/OpenSeeFace/releases
directly download the latest version of the Release package and decompress it,
then enter the Binary folder in the decompression directory ,
right-click to edit run.bat, run the facetracker command on the penultimate line and add it --model 4, switch to model 4 You can save after wink
facetracker -c %cameraNum% -F %fps% -D %dcaps% -v 3 -P 1 --discard-after 0 --scan-every 0 --no-3d-adapt 1 --max-feature-updates 900 --model 4(for reference only)
and double-click run.batto run. Follow the prompts to select the camera, resolution, and frame rate. If the capture is normal, you can see the output screen.
Finally, select OpenSeeFace input in the launcher, or add startup parameters --osf 127.0.0.1:11573to access OpenSeeFace

## Run

full run commandpython main.py --output_webcam unitycapture --ifm 192.168.31.182:49983 --character test1L2 --extend_movement 1 --output_size 512x512

参数名 | 值类型 | 说明

:---: | :---: | :---:
--character|	string|`character` The input image file name in the directory, no extension required
--debug|none|Open the OpenCV preview window to output the rendering result. If there is no output configuration, this parameter takes effect by default
--input|string|When not using iOS face capture, pass in the name of the camera device to be used, the default is device 0, and it is invalid when there is an ifm parameter
--ifm|string|	When using iOS face capture, the incoming device IP:端口号, such as192.168.31.182:49983
--output_webcam|string|The available value is obs unitycaptureto select the corresponding output type, no transmission or output to the camera
--extend_movement|extend_movement	floating point number|Use the head position returned by iOS surface capture to further move and rotate the model output image to make the upper body movable.
The value passed in represents the movement magnification (recommended value is 1)
--output_size|string|The format is 256x256and must be a multiple of 4.
Increasing it will not make the image clearer, but it will increase the movable range with extend_movement
