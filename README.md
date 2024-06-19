Faster R-CNN Implementation in Pytorch
========

This repository implements [Faster R-CNN](https://arxiv.org/abs/1506.01497) with training, inference and map evaluation in PyTorch.
The aim was to create a simple implementation based on PyTorch faster r-cnn codebase and to get rid of all the abstractions and make the implementation easy to understand.

The implementation caters to batch size of 1 only and uses roi pooling on single scale feature map.
The repo is meant to train faster r-cnn on voc dataset. Specifically I trained on VOC 2007 dataset.


## Faster R-CNN Explanation Video


## Faster R-CNN Implementation Video



## Sample Output by training Faster R-CNN on VOC 2007 dataset


## Data preparation
For setting up the VOC 2007 dataset:
* Download VOC 2007 train/val data from http://host.robots.ox.ac.uk/pascal/VOC/voc2007 and name it as `VOC2007` folder
* Download VOC 2007 test data from http://host.robots.ox.ac.uk/pascal/VOC/voc2007 and name it as `VOC2007-test` folder
* Place both the directories inside the root folder of repo according to below structure
    ```
    FasterRCNN-Pytorch
        -> VOC2007
            -> JPEGImages
            -> Annotations
        -> VOC2007-test
            -> JPEGImages
            -> Annotations
        -> tools
            -> train.py
            -> infer.py
        -> config
            -> voc.yaml
        -> model
            -> faster_rcnn.py
        -> dataset
            -> voc.py
    ```

## For training on your own dataset

* Update the path for `im_train_path`, `im_test_path`, `ann_train_path`, `ann_test_path` in config
* Modify dataset file `dataset/voc.py` to load images and annotations accordingly
* Dataset class should the following:
  ```
  im_tensor(C x H x W) , 
  target{
        'bboxes': Number of Gts x 4,
        'labels': Number of Gts,
        }
  file_path(just used for debugging)
  ```

## Differences from Faster RCNN paper
This repo has some differences from actual Faster RCNN paper.
* Caters to single batch size
* Uses a randomly initialized fc6 fc7 layer of 1024 dim.
* Most of the hyper-parameters have directly been picked from official version and have not been tuned to this setting of 1024 dimensional fc layers. As of now using this I am getting ~61-62% mAP.
* To improve the results one can try the following:
  * Use VGG fc6 and fc7 layers
  * Tune the weight of different losses
  * Experiment with roi batch size
  * Experiment with hard negative mining

## For modifications 
* To change the fc dimension , change `fc_inner_dim` in config
* To use a different backbone
* To use hard negative mining change `roi_low_bg_iou` to say 0.1(this will ignore proposals with < 0.1 iou)
* To use gradient accumulation change `acc_steps` in config to > 1

# Quickstart
* Create a new conda environment with python 3.8 then run below commands
* ```git clone https://github.com/explainingai-code/FasterRCNN-PyTorch.git```
* ```cd FasterRCNN-PyTorch```
* ```pip install -r requirements.txt```
* For training/inferencee use the below commands passing the desired configuration file as the config argument in case you want to play with it. 
* ```python -m tools.train``` for training Faster R-CNN on voc dataset
* ```python -m tools.infer --evaluate False --infer_samples True``` for generating inference predictions
* ```python -m tools.infer --evaluate True --infer_samples False``` for evaluating on test dataset

## Configuration
* ```config/voc.yaml``` - Allows you to play with different components of faster r-cnn on voc dataset  


## Output 
Outputs will be saved according to the configuration present in yaml files.

For every run a folder of ```task_name``` key in config will be created

During training of FasterRCNN the following output will be saved 
* Latest Model checkpoint in ```task_name``` directory

During inference the following output will be saved
* Sample prediction outputs for images in ```task_name/samples/*.png``` 

## Citations
```

```