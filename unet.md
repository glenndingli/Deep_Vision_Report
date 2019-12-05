# Parameter Setting

For all the experiments, we set vgg16 as encoder backbone, and 512-256-128-64-32 upsampling channels as decoder, followed by 3x3 convolutional filters with 2x2 strides. Early stopping is adopted to prevent overfitting. We use Adam optimizer with learning rate 1e-3 and reduce learning rate on plateau. Cross Entropy loss is adopted.

# Ablation Results

We train U-net on 256x256 and 512x512 rgb and rgbt images with or without pretrained backbone models. We fuse thermal channel with rgb channels either by concatenation during input or adding an alternative path (identical to rgb path) and fuse feature channels later on.


| Channel |  Fusion | Size | Pretrain |  mIOU | Background |  Roof | Faced | Roof Equipment |  Car  | Ground Equipment |
|:-------:|:-------:|:----:|:--------:|:-----:|:----------:|:-----:|:-----:|:--------------:|:-----:|:----------------:|
|   RGB   |   N/A   |  512 |     N    | 0.442 |    0.806   | 0.771 | 0.653 |      0.018     | 0.305 |       0.096      |
|   RGB   |   N/A   |  256 |     N    | 0.400 |    0.746   | 0.663 | 0.625 |      0.002     | 0.287 |       0.059      |
|   RGB   |   N/A   |  256 |     Y    | 0.304 |    0.667   | 0.505 | 0.477 |        0       | 0.156 |       0.019      |
|   RGBT  |  Input  |  256 |     N    | 0.231 |    0.611   | 0.460 | 0.212 |        0       | 0.105 |         0        |
|   RGBT  | Feature |  256 |     N    | 0.250 |    0.610   | 0.458 | 0.326 |        0       | 0.102 |         0        |



<p align="center">
	<img src="figure/unet_rgb_256.png" height="200"/>
</p>

<p align="center">
	<img src="figure/unet_rgb_512.png" height="200"/>
</p>

<p align="center">
	<img src="figure/unet_rgb_256_pretrain.png" height="200"/>
</p>

<p align="center">
	<img src="figure/unet_rgbt_input.png" height="200"/>
</p>

<p align="center">
	<img src="figure/unet_rgbt_feature.png" height="200"/>
</p>


