# Running Prednet on discrete and continuous datasets

## Reference for Prednet code
Cloned code at https://github.com/coxlab/prednet
Ref. https://coxlab.github.io/prednet/

## Overview

Clone this repository, download datasets from ..., modify paths in the `datasets_settings/..._settings.py` files, process images in train/test datasets with `process_images.py`, run prednet with `train.py` or `evaluate.py`.

## Setup environment for Prednet

### Install python, pip and virtualenv according to your OS 

We recommend using python 3.6 or above rather than python 2.

python, pip and virtualenv install manual: https://www.tensorflow.org/install/pip

Example for windows:
Go to Visual Studio https://visualstudio.microsoft.com/vs/older-downloads/
Select Redistributables and Build Tools,
Download and install the Microsoft Visual C++ 2015 Redistributable Update 3.

Go to https://www.python.org/downloads/windows/
Install the 64-bit Python 3 release for Windows (3.6), select pip as an optional feature.

Install virtualenv through a new command prompt (run as administrator)
`pip3 install -U pip virtualenv`
or
`pip install -U pip virtualenv`

### Install Tensorflow
Tensorflow install manual: https://www.tensorflow.org/install/

Create a virtual environment
`virtualenv --system-site-packages -p py ./path_to_your_venv`
Activate env and install packages
`path_to_your_venv\Scripts\activate`
`pip install --upgrade pip`
For CPU tensorflow install
`pip install --upgrade tensorflow` (`pip install --upgrade tensorflow-gpu` for gpu)


Update path to add cuda and cudnn
`SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\bin;%PATH%
SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\extras\CUPTI\libx64;%PATH%
SET PATH=C:\tools\cuda\bin;%PATH%` 

### Install Keras
Keras install manual : https://keras.io/#installation

From your virtual env
`pip install keras`

## Setup Prednet

Install packages
`pip install requests bs4 imageio scipy hickle matplotlib opencv-python`
`pip install numpy==1.16`


### Dowload KITTI images

Install wget and unzip how you can (OS dependent), then download training data
`cd prednet_dir`
`py process_kitti.py` or faster, `sh download_data.sh` or download directly from https://figshare.com/projects/PredNet_Game_of_Life/60971


## Generate FPSI images

Download images from https://figshare.com/projects/PredNet_Game_of_Life/60971 or generate images from FPSI video
 
`python generate_fpsi_images.py INPUT_FILE [--prefix <prefix>] [--dir <directory>] [--help]'.format(__file__)`

Process images with

`python images_processing/process.py [image directory]`
 

## Generate GoL images

Download images from https://figshare.com/projects/PredNet_Game_of_Life/60971

Process images with

`python images_processing/process.py [image directory]`


## Run Prednet

### Training

Change data path to appropriate values in prednet/datasets_settings directory.
If using pretrained model, download weights from https://figshare.com/projects/PredNet_Game_of_Life/60971

Or train the model using `py train.py`

You can specify the number of GPUs on a multi-GPUs server by using the argument `num_gpus`.
For example, to run prednet with the default parameters on 2 GPUs, you can use `python prednet/kitti_train_original.py --num_gpu=2`. 

You can also train a single layer prednet on the game of life dataset by using `python prednet/gol_train_one_layer.py` with the `num_gpus` argument.

You can also train a single layer prednet on the game of life dataset by using:  
`python prednet/gol_train_one_layer.py` with the `num_gpus` argument.


### Testing

Run the model: `py evaluate.py`

### hkl pickle error:

In case of error while reading images in hkl files, download hkl files from https://figshare.com/projects/PredNet_Game_of_Life/60971 or 

Install python2.7 and make a virtual env
Install old hickle
`pip install hickle==3.2.1`
`cd fix_prednet_data`
`py hkl_py_py2.py`

Go back to python3 venv
`pip install hickle==3.3.2`
`py hkl_py2_py3.py`

Change hkl read names in approprite files, eg in kitti_evaluate.py `X_test.hkl` -> `X_test_36.hkl`

(ref: https://stackoverflow.com/questions/51413618/loading-hickle-filecomes-from-python2-in-python-3
https://github.com/telegraphic/hickle)


## Run Convnet
Install chainer

`pip install chainer jupyter`

As chainer has changed a lot since version 1.5, please install chainer version > 1.5.

All experiments about Convnet are written in jupyter notebooks at 'convnet' directory. So change directory and run jupyter notebook.

`cd convnet`

`jupyter notebook`

Change data 'PATH' appropriately in second cell of 'predict_next_GoL_image_using_simpleAE.ipynb' and make directly for save results and setting it to 'out' variable.
After that, only you have to do is run cells from top to bottom.

 
## Tips

### Switch python versions: 
```
> python --version
Python 3.4.2
> set PATH=C:\tools\python2\;%PATH%
> python --version
Python 2.7.9
```

### GPU setup (Windows)

References: 
https://www.tensorflow.org/install/pip
https://www.tensorflow.org/install/gpu
https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/

Install tensorflow-gpu 
`pip install --upgrade tensorflow-gpu`

Install Nvidia drivers. Find your GPU model (https://www.cisco.com/c/en/us/td/docs/telepresence/endpoint/articles/cisco_telepresence_movi_find_out_graphics_card_driver_on_windows_pc_kb_540.html)
eg `NVDIA GeForce RTX 2080 Ti`
Download and install corresponding driver https://www.nvidia.com/Download/index.aspx?lang=en-us
Install Visual Studio https://visualstudio.microsoft.com/
Install CUDA toolkit, *legacy release 9.0* https://developer.nvidia.com/cuda-zone (ref https://github.com/tensorflow/tensorflow/issues/22794), `exe local` (network installer fails if PC inside csl)
Install cudnn https://developer.nvidia.com/cudnn (requires developper account)
Place cudnn cuda folder in `C:/tools/cuda`

Update path to add cuda and cudnn
`SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\bin;%PATH%
SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\extras\CUPTI\libx64;%PATH%
SET PATH=C:\tools\cuda\bin;%PATH%` 

Check installation `nvcc -V`

Check that tensorflow is using gpu: run `py test_gpu.py` from your virtualenv

In case of import tensorflow error, manually remove tensorflow folder in your virtualenv, then uninstall tf and tf-gpu with pip, and reinstall tf-gpu

#### In case of errors...

Tensorflow-gpu should run even if the samples below don't run, but if you manage to run the samples, for sure tensorflow should run

Install or enable (https://docs.microsoft.com/en-us/dotnet/framework/install/dotnet-35-windows-10) .NET 3.5
Install DirectX https://www.microsoft.com/en-us/download/details.aspx?id=6812

Check that sample runs:
open `"C:\ProgramData\NVIDIA Corporation\CUDA Samples\v9.0\Samples_vs2017.sln"` with Visual Studio
If there are `file not found` errors, make sure that you install cuda *after* installing visual studio.
If there are SDK errors, right-click the solution in the solution explorer and click `retarget solution`
The compiled file is built at `C:\ProgramData\NVIDIA Corporation\CUDA Samples\v10.0\bin\win64\Release` , run it and check that there are no errors

## TODO

Upload corrected hkl files
Upload structure json file
Create test and train in process file (use args)
Sort imports.

