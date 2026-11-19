# Module 18: Computer Vision

Convolutional neural networks were the first deep learning models to beat every other approach on a hard real-world task, and they're still at the heart of most vision systems. This module builds them from the ground up: convolutions written by hand, a CNN trained in PyTorch, data augmentation, residual connections, and then the way most real projects actually start, with a network pretrained on more than a million photos and adapted to a new task by transfer learning.

Everything runs on an ordinary CPU, in Colab or on your own computer. A GPU makes it faster but isn't needed.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Images as tensors, convolution by hand, padding, stride and channels, pooling and receptive fields, why convolutions suit images, a CNN on Fashion-MNIST, what it learns, shifted images, data augmentation, ResNet and residual connections, pretrained models, transfer learning, other vision tasks | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/18-computer-vision/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises, from output sizes to a batched convolution, a residual block and the backward pass of a convolution, checked against PyTorch | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/18-computer-vision/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/18-computer-vision/03_solutions.ipynb) |
| 4 | Mini project | A recycling sorter: a CNN from scratch against a pretrained ResNet-18, used for feature extraction and then fine-tuned, tested once and studied through its mistakes | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/18-computer-vision/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The data is described in [data/README.md](data/README.md).

## What you'll learn

- **Images as numbers.** Pixels and channels, and the `(N, C, H, W)` layout PyTorch expects.
- **Convolution.** Sliding a kernel by hand in NumPy, classic kernels for blurring, sharpening and edges, and the same thing with `F.conv2d`.
- **Shapes.** Padding, stride, the output-size formula, and convolution layers with many channels.
- **Pooling and receptive fields.** How later layers come to see more of the image.
- **Why convolutions work.** Locality, weight sharing and translation equivariance: 160 parameters against nearly 10 million.
- **A CNN in PyTorch.** Convolution blocks on Fashion-MNIST, beating the MLP from Module 17 with an eighth of the parameters.
- **What a CNN learns.** Its first-layer kernels and feature maps.
- **Moving the picture.** What happens to an MLP and a CNN when the items move a few pixels.
- **Data augmentation.** Random flips and shifts, and why augmentation should mimic the variation you expect in real use.
- **Residual connections.** From LeNet to ResNet, and a residual block in code.
- **Pretrained models.** ResNet-18 trained on ImageNet, its preprocessing, and where its answers stop making sense.
- **Transfer learning.** Freezing, replacing the last layer, fine-tuning, and when transfer helps less than you'd expect.
- **Beyond classification.** Detection, segmentation, similarity search and vision transformers.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `conv_output_size`, `image_to_tensor`, `conv_parameter_count`, `top_k_labels` |
| Medium | `conv2d_single`, `max_pool2d`, `receptive_field`, `pad_and_crop`, `make_transfer_model` |
| Hard | `conv2d_batch`, `ResidualBlock`, `conv2d_backward` |

The checks compare your code with PyTorch and torchvision, and catch the classic mistakes:

- flipping the kernel, or treating a rectangular kernel as square
- using `reshape` instead of `permute` to reorder image dimensions
- a softmax over only the top few classes
- creating the new last layer before freezing the network, so it's frozen too
- putting the stride in the wrong convolution of a residual block
- overwriting gradients where they should be added up

## Mini project

**A recycling sorter.** 2,527 phone photos of single items of rubbish in six classes: cardboard, glass, metal, paper, plastic and trash.

- **From scratch.** A small CNN with data augmentation, trained on fewer than 2,000 photos.
- **Feature extraction.** ResNet-18's pretrained features with logistic regression on top, already better.
- **Fine-tuning.** The last stage of ResNet-18 adapted to the task, with stored early-layer outputs so it runs on a CPU, reaching close to 90% validation accuracy.
- **One honest test.** The chosen model is tested once, then studied: glass and plastic are the hardest pair to separate, and the small, mixed "trash" class is the weakest.
- **Deployment.** The pieces are put back into one ResNet-18, saved, and wrapped in a function that sorts a photo.

## Prerequisites

Modules [1](../01-python-foundations) to [17](../17-deep-learning-pytorch). Module 17 (PyTorch, training loops, batch normalisation, dropout) is used throughout; Module 13 (precision, recall and the confusion matrix) and Module 12 (logistic regression) come back in the mini project.

## Next

Module 19: Sequences and Transformers (planned).
