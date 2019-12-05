# MaskRCNN

## Overview

In this task, we used a classic semantic segmentation framework, Mask RCNN. Different from the previous approaches in this project, Mask RCNN needs to detect some regions of interests, and then classify the detected regions. This approaches can be used to conduct instance segmentation. Instance segmentation can treat multiple objects of the same class as separated individuals, but the previous approaches, like U-net and DeepLabV3, treat multiple objects of the same class as a whole entity. We planned to use this approach because this approach can solve the problem in an alternative way. We planned to explore how different the algorithm work. 

Similar to the U-net and DeepLabV3, we did different experiments on Mask RCNN. First, we used Mask RCNN directly on the RGB images. Second, we brutally 

## Experiment Details

Based on the above introduction, we first attempt to add one more thermal channel input, and then fusion the feature maps of RGB and thermal inputs. Hence, we set up experiments based on the three main different methods.

### Input Fusion

For the RGB images as input, it is just a simple semantic segmentation task. To do that, we first simply use Mask RCNN with the default parameters. Then, it is intuitive to optimize for better parameters. After a set of experiments, we finalize the parameters as:

| parameters 					| value	| 
|-------------------------------|-------|
| IMAGES_PER_GPU				| 1		|
| STEPS_PER_EPOCH				| 800	|
| DETECTION_MIN_CONFIDENCE		| 0.7	|
| MEAN_PIXEL					| [123.7, 116.8, 103.9]|
| MAX_GT_INSTANCES				| 20	|
| RPN_TRAIN_ANCHORS_PER_IMAGE	| 128	|
| RPN_ANCHOR_SCALES				| (32, 64, 128, 256, 512)|
| Mini_Mask						| (512, 512)|
| Image_size					| (512, 512) |
| DETECTION_MIN_CONFIDENCE		| 0.9|

After all the parameters are set, we test the IoUs for 5+1 classes again, and get the result:

| Input Data | Tuning | mIOU   | Background | Roof   | Facade | Roof Equip. | Car    | Ground Equip. |
|------------|--------|--------|------------|--------|--------|-------------|--------|---------------|
| RGB        | N      | 0.3176 | 0.5363     | 0.4550 | 0.4124 | 0           | 0.4452 | 0.0564        |
| RGBT       | Y      | 0.4421 | 0.7243     | 0.6105 | 0.6115 | 0           | 0.4889 | 0.2174        |

### Feature Map Fusion

After we get the best performance possible for RGB input, we attempt to add one more input channel. 

With the previous RGB parameters, we get a worse performance. After we look into from which part the Mask RCNN loss comes, we find the ```RPN_BBOX_LOSS``` and ```MRCNN_BBOX_LOSS``` is relatively high to others, and they are still slowly converging to a lower value. To speed up the training, we set a different weight to different loss parts. 

#### Stage 1:

```Python
RPN_BBOX_LOSS = 1 -> 4
MRCNN_BBOX_LOSS = 1 -> 3.2
MRCNN_MASK_LOSS = 1 -> 2
```

#### Stage 2:
After stage 1, the losses all converges and start to lower very slowly, we then increase the loss again. 

```Python
RPN_BBOX_LOSS = 4 -> 6
MRCNN_BBOX_LOSS = 3.2 -> 4
MRCNN_MASK_LOSS = 2 -> 3
```

After the two stages, we find that the loss is not going to change whatever we set the loss weight. Then we also detect the model on the test dataset, and get the IoU shown below.

| Input Data | Stage | mIOU    | Background | Roof    | Facade  | Roof Equip\. | Car     | Ground Equip\. |
|------------|-------|---------|------------|---------|---------|--------------|---------|----------------|
| RGBT       | 1     | 0\.4458 | 0\.7146    | 0\.5813 | 0\.5953 | 0            | 0\.6542 | 0\.1294        |
| RGBT       | 2     | 0\.4710 | 0\.7708    | 0\.7628 | 0\.6110 | 0            | 0\.6080 | 0\.0736        |


### Feature Fusion

After we try our best on the thermal channel input fusion, we come up with the idea on how to better extract the thermal information and at the same time give less confusion on the RGB features. The way we try on Mask RCNN is to fusion the RGB feature maps with the thermal feature maps. In this way, we expect the network can handle the feature maps as what is needed for detection and region proposal. 

However, when we try to add one parallel backbone ResNet 101, we find the hardware can not handle this large network, even the largest GPU we can have on GCP with 16 GB memory. We have attempted to use various ways to increase the hardware capacity, for instance to increase the number of GPUs, to add more RAM for the PC, and to use better GPU. None of them worked. 

Then we put our eye on reduce the network size. In other words, we have to trade off some network capacity for better thermal information extraction. To reduce the network, we reset the parameters as below:

```Python
BACKBONE_NETWORK = 'ResNet50' -> 'ResNet 101'
Mask_SIZE = (512, 512) -> (128, 128)
ROI_Proposal_Intance = 512 -> 128
```

After all parameters tuning, the best performance we can get is:

| Input Data | mIOU    | Background | Roof    | Facade  | Roof Equip\. | Car     | Ground Equip\. |
|------------|---------|------------|---------|---------|--------------|---------|----------------|
| RGBT       | 0\.3951 | 0\.6795    | 0\.6781 | 0\.5343 | 0            | 0\.4065 | 0\.0724        |


## Discussion and Conclusion 




