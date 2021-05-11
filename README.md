# Light Sheet Imaging Data Analysis 

参考：https://mulab2020.github.io/hg_gitbook//Methods/ (后面转到Mulab.github上)

高昊的项目目录：`10.10.49.10\Alumni\hgao\PJ`(**后面都称为根目录**)

## Overview

两张图: 细胞分割流程示意图和数据分析示意图

## Cell Segmentation

### Registration

### 核定位

参考：https://mulab2020.github.io/hg_gitbook/Methods/CellSegmentation/%E6%A0%B8%E5%AE%9A%E4%BD%8D.html

#### Matlab (yuqi zhu)

`根目录\Ca_img_processing_tiff_brief\Extract_roi_demo_hgao.m`

```matlab
clear 
clc

%% Read data
filename = 'anat_ref.tif';       % 10帧平均的模板脑(average brain)
tiffInfo = imfinfo(filename);     %返回图像结构，每张图像有一个元素
tiffStack = imread(filename, 1); 
    
for idx = 2 : size(tiffInfo, 1)
    tiffTemp = imread(filename, idx);
    tiffStack = cat(3 , tiffStack, tiffTemp);
end

%% 
opt.maxrad = 7;
opt.minrad = 3;
opt.thres = [];

env.vol = tiffStack;
env.height = size(tiffStack, 1);
env.width = size(tiffStack, 2);
env.depth = size(tiffStack, 3);

[env.volmask, env.mask] = background_segmentation_nucleus(env, opt);

[env.supervoxel, scoremaps, radmaps] = supervoxel_segmentation_n(env, opt);
```

- **最好使用100帧以上的平均数据**
- 如果有更高的分割需求，可以参照`根目录\Ca_img_processing_tiff_brief\Cacium_pipeline_demo_zyq.m`

### CNMF

参考：https://mulab2020.github.io/hg_gitbook/Methods/CellSegmentation/CNMF.html

#### Fish_proc (ziqiang wei)

##### 环境配置

step1: 首先安装miniconda，**其中miniconda安装选择个人使用，一定不能选择所有用户**


step2: 把`根目录\pipeline_zqw\environment.yml`拷贝到个人用户目录下面`C:\Users\danyangl`，在搜索栏打开`Anaconda Prompt(miniconda3)`的命令行界面，输入命令

```bash
 conda env create -f environment.yml
```

然后等待安装完成，切换环境名称为`fish_proc`

```bash
conda activate fish_proc
```



step3: 安装[fish](https://github.com/d-v-b/fish)分析包,。把`根目录\pipeline_zqw\backup\fish`文件夹拷贝到独立目录下，在搜索栏打开`Anaconda Prompt(miniconda3)`的命令行界面进入`fish`文件夹，输入命令

```bash
python setup.py build
python setup.py install
```

然后把`根目录\pipeline_zqw\backup\fish\fish`文件夹下所有内容拷贝到`C:\Users\personalusername\miniconda3\envs\fish_proc\Lib\site-packages\fish-0.1-py3.8.egg\fish`。

step4: 安装[修改后的fish_proc](https://github.com/zqwei/fish_processing)分析包。把`根目录\pipeline_zqw\backup\fish_proc`文件夹拷贝到独立目录下，搜索栏打开`Anaconda Prompt(miniconda3)`的命令行界面进入`fish_proc`文件夹，输入命令

```ba
python setup.py build
python setup.py install
```

**然后把`根目录\pipeline_zqw\fish_processing\fish_proc`文件夹下所有内容拷贝到**`C:\Users\hgao.WHPC-MULAB\miniconda3\envs\fish_proc\Lib\site-packages\fish_proc-1.0-py3.8.egg\fish_proc`

基于以上步骤基本上配置完成数据分析环境

##### 数据存放要求

每个人必须建立一个Data文件夹，为每次实验数据创建一个文件夹，其下有(I)raw: 存放原始数据文件；(II)gainMat20180208 : 从`根目录\pipeline_zqw\Data\gainMat20180208`拷贝

运行结束核心生成文件夹为: `.\Data\savetmp\cell_raw_dff`，其中包含了分割后神经元相关数据

### CNN

参考：https://mulab2020.github.io/hg_gitbook/Methods/CellSegmentation/CNN.html

## Registration

由Xinlan Liu后期补充

## Ephys

根目录\zebrafish\zebrafish\fishEphys

### Stim

### Camera

### Swim

## Signal Analysis

### DF/F

### 粗分类

sensory sensory-motor motor noise intermediate sparse

#### Whole-Signal Analysis(粗分类)

#### Trial-by-Trial Analysis(粗分类)

### Clustering(精分类)

### Sequential Activity(看顺序)

Time-delayed 

### Structural mapping 

## Visualization

### Map

### Animator

### Interaction

## Python Available Pkgs

https://mulab2020.github.io/hg_gitbook/Coding/Python/PythonAvailablePackages.html

> - dask
> - numpy