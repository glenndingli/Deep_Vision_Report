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

