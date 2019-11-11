# CenterNet - PyTorch 1.0
[CenterNet: Keypoint Triplets for Object Detection](https://arxiv.org/abs/1904.08189)

**CenterNet is an one-stage detector which gets trained from scratch. On the MS-COCO dataset, CenterNet achieves an AP of 47.0%, which surpasses all known one-stage detectors, and even gets very close to the top-performance two-stage detectors.**

## Preparation
Please first install [Anaconda](https://anaconda.org) and create an Anaconda environment using following commands.
```
conda env create -f packagelist.yml
conda activate CornerNet-PT10
conda install pytorch==1.0.0 torchvision==0.2.1 cuda100 -c pytorch
```

## Compiling Corner Pooling Layers
```
cd <CenterNet dir>/models/py_utils/_cpools/
python setup.py install --user
```

## Compiling NMS
```
cd <CenterNet dir>/external
make
```

## Installing MS COCO APIs
```
cd <CenterNet dir>/data/coco/PythonAPI
make
```

## Downloading MS COCO Data
- Download the training/validation split we use in our paper from [here](https://drive.google.com/file/d/1dop4188xo5lXDkGtOZUzy2SHOD_COXz4/view?usp=sharing) (originally from [Faster R-CNN](https://github.com/rbgirshick/py-faster-rcnn/tree/master/data))
- Unzip the file and place `annotations` under `<CenterNet dir>/data/coco`
- Download the images (2014 Train, 2014 Val, 2017 Test) from [here](http://cocodataset.org/#download)
- Create 3 directories, `trainval2014`, `minival2014` and `testdev2017`, under `<CenterNet dir>/data/coco/images/`
- Copy the training/validation/testing images to the corresponding directories according to the annotation files

N.B. 2014 Train has 82783 images. 2014 Val has 40504 images. These two are both put in trainval2014 folder (123287 images). The provided training/validation split (instances_minival2017.json and instances_trainval2017.json) then extracts 5000 for validation and uses the rest 118287 for training. 

## Training and Evaluation

To train CenterNet-104:
```
python train.py CenterNet-104
```
We provide the configuration file (`CenterNet-104.json`) and the model file (`CenterNet-104.py`) for CenterNet in this repo. 

We also provide a trained model for `CenterNet-104`, which is trained for 480k iterations using 8 Tesla V100 (32GB) GPUs. You can download it from [BaiduYun CenterNet-104](https://pan.baidu.com/s/1OQwMAPLcZkHWbTzD28cxow) (code: bfko) or [Google drive CenterNet-104](https://drive.google.com/open?id=1GVN-YrgExbPPcmzn_Lkr49f2IKjodg15) and put it under `<CenterNet dir>/cache/nnet` (You may need to create this directory by yourself if it does not exist). If you want to train you own CenterNet, please adjust the batch size in `CenterNet-104.json` to accommodate the number of GPUs that are available to you.

To use the trained model:
```
python test.py CenterNet-104 --testiter 480000 --split <split>
```

To train CenterNet-52:
```
python train.py CenterNet-52
```
We provide the configuration file (`CenterNet-52.json`) and the model file (`CenterNet-52.py`) for CenterNet in this repo. 

We also provide a trained model for `CenterNet-52`, which is trained for 480k iterations using 8 Tesla V100 (32GB) GPUs. You can download it from [BaiduYun CenterNet-52](https://pan.baidu.com/s/1xZHB7jq7Hmi0qKu46qnotw) (code: 680t) or [Google Drive CenterNet-52](https://drive.google.com/open?id=14vJYw4P9sxDoltjp5zDkOS3QjUa2zZIP) and put it under `<CenterNet dir>/cache/nnet` (You may need to create this directory by yourself if it does not exist). If you want to train you own CenterNet, please adjust the batch size in `CenterNet-52.json` to accommodate the number of GPUs that are available to you.

To use the trained model:
```
python test.py CenterNet-52 --testiter 480000 --split <split>
```

We also include a configuration file for multi-scale evaluation, which is `CenterNet-104-multi_scale.json` and `CenterNet-52-multi_scale.json` in this repo, respectively. 

To use the multi-scale configuration file:
```
python test.py CenterNet-52 --testiter <iter> --split <split> --suffix multi_scale
```
or
```
python test.py CenterNet-104 --testiter <iter> --split <split> --suffix multi_scale
```
