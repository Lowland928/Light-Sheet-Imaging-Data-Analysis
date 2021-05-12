# Light Sheet Imaging Data Analysis 

参考：https://mulab2020.github.io/hg_gitbook//Methods/ 

高昊的项目目录：`10.10.49.10\Alumni\hgao\PJ`(**后面都称为根目录**)

# Overview

两张图: 细胞分割流程示意图和数据分析示意图

# Cell Segmentation

## Registration

## 核定位

参考：https://mulab2020.github.io/hg_gitbook/Methods/CellSegmentation/%E6%A0%B8%E5%AE%9A%E4%BD%8D.html

## Matlab (yuqi zhu)

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

## CNMF

参考：https://mulab2020.github.io/hg_gitbook/Methods/CellSegmentation/CNMF.html

## Fish_proc (ziqiang wei)

### 环境配置

**下面以李丹阳的个人目录为例子说明: `C:\Users\danyangl`表示李丹阳的用户目录，`E:\MuLab\ldy`表示李丹阳的工作目录。**

step1: 首先下载安装[miniconda](https://docs.conda.io/en/latest/miniconda.html)。注意：**其中miniconda安装选择个人使用，一定不能选择所有用户！！！！！！！！！！！！！**


step2: 把`根目录\pipeline_zqw\environment.yml`拷贝到个人的用户目录下面`C:\Users\danyangl`，在搜索栏打开`Anaconda Prompt(miniconda3)`的命令行界面，输入命令

```bash
 conda env create -f environment.yml
```

等待所有软件包安装完成。然后使用下述命令切换到工作环境`fish_proc`

```bash
conda activate fish_proc
```

step3: 安装[fish](https://github.com/d-v-b/fish)分析包。把`根目录\pipeline_zqw\backup\fish`文件夹拷贝到个人的用户目录下，在Anaconda Prompt(miniconda3)的命令行界面输入`cd fish`进入`fish`文件夹，输入命令

```bash
python setup.py build
python setup.py install
```

然后把`根目录\pipeline_zqw\backup\fish\fish`文件夹下所有内容拷贝到`C:\Users\personalusername\miniconda3\envs\fish_proc\Lib\site-packages\fish-0.1-py3.8.egg\fish`。同时，在Anaconda Prompt(miniconda3)的命令行界面返回到上级目录`cd ..`。

step4: 安装[修改后的fish_proc](https://github.com/zqwei/fish_processing)分析包。把`根目录\pipeline_zqw\backup\fish_proc`文件夹拷贝到个人的用户目录下，在Anaconda Prompt(miniconda3)的命令行界面输入`cd fish_proc`进入`fish_proc`文件夹，输入命令

```ba
python setup.py build
python setup.py install
```

**然后把`根目录\pipeline_zqw\fish_processing\fish_proc`文件夹下所有内容拷贝到**`C:\Users\hgao.WHPC-MULAB\miniconda3\envs\fish_proc\Lib\site-packages\fish_proc-1.0-py3.8.egg\fish_proc`。同时，在Anaconda Prompt(miniconda3)的命令行界面返回到上级目录`cd ..`。

以上，完成环境的配置。

### 数据存放要求

step1: 个人的工作目录`E:\MuLab\ldy`下建立一个Data文件夹`E:\MuLab\ldy\Data`。

step2: 创建一个文件夹用于存放实验数据`E:\MuLab\ldy\Data\expname`。

step3: 拷贝原始数据到文件夹raw下面：`E:\MuLab\ldy\Data\expname\raw`。其中会包括实验室参数文件`ch0.xml`和一系列的`*.h5`的数据文件。

step4: 从`根目录\pipeline_zqw\Data\gainMat20180208`拷贝到文件夹`E:\MuLab\ldy\Data\expname`下面。

### 测试流程

step1: [搭建jupyter notebook环境](https://mulab2020.github.io/hg_gitbook/Coding/Python/JupyterNotebookKernels.html)

step1.1: Anaconda Prompt(miniconda3)的命令行界面的`fish_proc`工作环境下输入`jupyter notebook --generate-config`。

step1.2: 在个人的用户目录下`C:\Users\danyangl\.jupyter`会生成`jupyter_notebook_config.py`文件，右击使用`notepad++`打开。

step1.3: 创建个人的jupyter工作文件夹`E:\MuLab\ldy\Jupyter`。

step1.4: 取消`#c.NotebookApp.notebook_dir = ''` 的注释，并且加入个人的Jupyter工作目录`c.NotebookApp.notebook_dir = 'E:\\MuLab\\ldy\\Jupyter\\'。`

step1.5: 输入`jupyter notebook`即可。

step2: 拷贝测试数据`根目录\pipeline_zqw\Data\raw`和`根目录\pipeline_zqw\Data\gainMat20180208`到个人的工作目录的Data文件夹下`E:\MuLab\ldy\Data\expname`。

step3: 拷贝原始流程notebook到`根目录\pipeline_zqw\test_pipeline.ipynb`到个人的工作目录jupyter文件夹下面`E:\MuLab\ldy\Jupyter`。

step4: 此时可以在notebook网页中进入`Jupyter`文件夹可以看到`test_pipeline`notebook，点击进去修改第二个block中的路径位置如下:

```python
dir_root = 'E:/MuLab/ldy/Data/expname/raw/'
savetmp = 'E:/MuLab/ldy/Data/expname/savetmp'
save_root = savetmp
cameraNoiseMat = 'E:/MuLab/ldy/Data/expname/gainMat20180208'
files = sorted(glob(dir_root+'/*.h5'))
# print(files)
chunks = File(files[0],'r')['default'].shape
nsplit = (chunks[1]//64, chunks[2]//64)
num_t_chunks = 2
dask_tmp = 'E:/MuLab/ldy/Data/expname/dask-worker-sapce'
memory_limit = 0 # unlimited
down_sample_registration = 3
baseline_percentile = 20
baseline_window = 1000   # number of frame
```

step5: 运行所有的blocks，直到完成。在个人的共组目录`E:\MuLab\ldy\Data\expname\savetmp`下会有一系列的生成文件，其中[`cell_raw_dff`包含分割后的细胞空间和时间信息](https://mulab2020.github.io/hg_gitbook/Others/Pipeline/)。

## CNN

参考：https://mulab2020.github.io/hg_gitbook/Methods/CellSegmentation/CNN.html

# Registration

由Xinlan Liu后期补充

# Ephys

`根目录\zebrafish\zebrafish\fishEphys`，该代码主要是针对contrast实验数据的分析，对于其它实验可能需要对代码进行修改。具体参数表示可以参照`根目录\others`文件夹下两个视频。

# Signal Analysis

## DF/F

## 粗分类

sensory sensory-motor motor noise intermediate sparse

### Whole-Signal Analysis(粗分类)

### Trial-by-Trial Analysis(粗分类)

## Clustering(精分类)

## Sequential Activity(看顺序)

Time-delayed 

## Structural mapping 

# Visualization

## Map

## Animator

## Interaction

# Python Available Pkgs

https://mulab2020.github.io/hg_gitbook/Coding/Python/PythonAvailablePackages.html

## Highlight

> - dask
> - numpy
> - dipy
> - skimage
> - hdf5
> - matplotlib
> - seaborn
> - sklearn
> - glob
> - os
> - os.path
> - functools

