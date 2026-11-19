# Module 18 Cheatsheet

## Images as tensors

```python
import numpy as np, torch
img = np.asarray(pil_image)                      # (H, W, C) uint8, values 0 to 255
x = torch.from_numpy(img).permute(2, 0, 1)       # (C, H, W): PyTorch's order
x = x.float() / 255                              # floats in [0, 1]
batch = x.unsqueeze(0)                           # (N, C, H, W)
shown = x.permute(1, 2, 0).numpy()               # back to (H, W, C) for plt.imshow
```

- Use `permute` to reorder dimensions, never `reshape`.
- Greyscale images have one channel: `(1, H, W)`.

## Convolution

```python
import torch.nn.functional as F
from torch import nn

F.conv2d(x, weight, bias, stride=1, padding=0)   # weight: (C_out, C_in, kH, kW)
nn.Conv2d(in_channels, out_channels, kernel_size=3, padding=1, stride=1, bias=True)
```

- Output size: `(n + 2 * padding - kernel_size) // stride + 1`. A 3 x 3 kernel with padding 1 keeps the size.
- Parameters: `C_out * C_in * kH * kW` (+ `C_out` biases). The image size doesn't matter.
- Deep learning's "convolution" is cross-correlation: the kernel isn't flipped.
- Hand-made kernels: blur (all equal), sharpen (5 in the middle, -1 around), Sobel edges (`[[-1, 0, 1], [-2, 0, 2], [-1, 0, 1]]`).

## Pooling and receptive field

```python
nn.MaxPool2d(2)                                  # halves height and width
nn.AdaptiveAvgPool2d(1)                          # global average pooling: one number per channel, any image size
```

- Receptive field: start with `field = 1, jump = 1`; for each layer `field += (k - 1) * jump`, then `jump *= stride`.

## A small CNN

```python
def conv_block(c_in, c_out):
    return [nn.Conv2d(c_in, c_out, 3, padding=1, bias=False), nn.BatchNorm2d(c_out), nn.ReLU(), nn.MaxPool2d(2)]

model = nn.Sequential(*conv_block(3, 32), *conv_block(32, 64), *conv_block(64, 128),
                      nn.AdaptiveAvgPool2d(1), nn.Flatten(), nn.Linear(128, n_classes))
```

- Halve the image, double the channels.
- Print the shape after each layer when building a new architecture.
- CNNs have few parameters but a lot of computation: use a GPU for anything big.

## Why convolutions

| property | meaning |
|---|---|
| locality | each output looks at a small patch |
| weight sharing | the same kernel everywhere, so few parameters |
| translation equivariance | shift the input, and the feature maps shift too |
| pooling | some tolerance to small shifts, wider view in later layers |

## Data augmentation

```python
from torchvision.transforms import v2

train_transform = v2.Compose([
    v2.RandomResizedCrop((224, 224), scale=(0.7, 1.0), antialias=True),
    v2.RandomHorizontalFlip(),
    v2.ColorJitter(brightness=0.2, contrast=0.2),
])
# apply it in a Dataset's __getitem__, one image at a time, on training data only
```

- Mimic the variation you expect in real use; don't use changes that alter the answer (flipping digits or text).
- `v2.RandomCrop(28, padding=3)` shifts small images by up to 3 pixels.

## Residual connections

```python
out = F.relu(bn2(conv2(F.relu(bn1(conv1(x))))) + shortcut(x))
```

- `shortcut` is the identity when the shape doesn't change, otherwise a 1 x 1 convolution (with the stride) plus batch norm.
- Residual blocks made networks of 50 to 150 layers trainable.

## Pretrained models

```python
import torchvision
weights = torchvision.models.ResNet18_Weights.IMAGENET1K_V1
model = torchvision.models.resnet18(weights=weights).eval()
preprocess = weights.transforms()                # always use the matching preprocessing
names = weights.meta["categories"]               # the 1,000 ImageNet classes

with torch.no_grad():
    probabilities = model(preprocess(img_tensor).unsqueeze(0)).softmax(dim=1)
```

- ImageNet means `[0.485, 0.456, 0.406]`, standard deviations `[0.229, 0.224, 0.225]`.
- A classifier can only answer with its own classes, and is reliable only on images like its training data.

## Transfer learning

```python
for p in model.parameters():
    p.requires_grad = False                      # freeze
model.fc = nn.Linear(model.fc.in_features, n_classes)   # new trainable head
for p in model.layer4.parameters():
    p.requires_grad = True                       # optionally fine-tune the last stage

optimizer = torch.optim.AdamW([
    {"params": model.layer4.parameters(), "lr": 1e-4},   # pretrained: small steps
    {"params": model.fc.parameters(), "lr": 1e-3},       # new layer: larger steps
])
```

| approach | trains | when |
|---|---|---|
| feature extraction | only a new classifier on the features | very little data, little computing power |
| fine-tune the last stage(s) | the top of the network plus the new layer | a few hundred images per class or more |
| fine-tune everything | all layers, with a small learning rate | plenty of data, a GPU |
| from scratch | everything, from random weights | lots of data unlike photos (medical scans, spectrograms, satellites) |

- Frozen batch norm layers still update their statistics in `train()` mode: keep the frozen part in `eval()`, or store its outputs once.
- Transfer works best when your images resemble the pretraining images.

## Other vision tasks

| task | output | example networks |
|---|---|---|
| classification | one label per image | ResNet, EfficientNet, ViT |
| detection | a box and label for each object | Faster R-CNN, YOLO |
| segmentation | a label for every pixel | U-Net, DeepLab |
| similarity search | a feature vector per image | any pretrained backbone + cosine similarity |
