# Mask-RCNN adapted for building corner detection

This is a Mask-RCNN modified from [original repo](https://github.com/multimodallearning/pytorch-mask-rcnn). The following parts of the README are excerpts from the original README.


## Requirements
* Python 3
* Pytorch 0.4.0
* matplotlib, scipy, skimage, h5py, it's better to update these libs to the latest versions

## Installation
Note that we use CUDA 9.1 for the installation below, using CUDA>=10.1 + some older pytorch versions can lead to problems.

1. Clone this repository.

    
2. We use functions from two more repositories that need to be build with the right `--arch` option for cuda support.
The two functions are Non-Maximum Suppression from ruotianluo's [pytorch-faster-rcnn](https://github.com/ruotianluo/pytorch-faster-rcnn)
repository and longcw's [RoiAlign](https://github.com/longcw/RoIAlign.pytorch).

    | GPU | arch |
    | --- | --- |
    | TitanX | sm_52 |
    | GTX 960M | sm_50 |
    | GTX 1070 | sm_61 |
    | GTX 1080 (Ti) | sm_61 |
        
        cd model
        cd nms/src/cuda/
        nvcc -c -o nms_kernel.cu.o nms_kernel.cu -x cu -Xcompiler -fPIC -arch=[arch]
        cd ../../
        python build.py
        cd ../

        cd roialign/roi_align/src/cuda/
        nvcc -c -o crop_and_resize_kernel.cu.o crop_and_resize_kernel.cu -x cu -Xcompiler -fPIC -arch=[arch]
        cd ../../
        python build.py
        cd ../../
        cd ..
    
3. Download the pretrained models on COCO and ImageNet from [Google Drive](https://drive.google.com/open?id=1LXUgC2IZUYNEoXr05tdqyKFZY0pZyPDc).


## Training
Training and evaluation code is in main.py. You can run it from the command
line as such:

    # Train a new model starting from pre-trained COCO weights
    python main.py train --dataset=/path/to/coco/ --model=coco

    # Train a new model starting from ImageNet weights
    python main.py train --dataset=/path/to/coco/ --model=imagenet

    # Continue training a model that you had trained earlier
    python main.py train --dataset=/path/to/coco/ --model=/path/to/weights.h5

    # Continue training the last model you trained. This will find
    # the last trained weights in the model directory.
    python main.py train --dataset=/path/to/coco/ --model=last
