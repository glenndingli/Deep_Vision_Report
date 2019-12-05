# Parameter Setting

For all the experiments, we set vgg16 as encoder backbone, and 512-256-128-64-32 upsampling channels as decoder, followed by 3x3 convolutional filters with 2x2 strides. Early stopping is adopted to prevent overfitting. We use Adam optimizer with learning rate 1e-3 and reduce learning rate on plateau. Cross Entropy loss is adopted.

# Ablation Results

We train U-net on 256x256 and 512x512 images.
