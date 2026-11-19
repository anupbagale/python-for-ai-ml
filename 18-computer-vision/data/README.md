# Data

Nothing is stored in this folder. The notebooks download what they need the first time they run and keep a copy here.

## TrashNet (mini project)

2,527 photos of single items of rubbish, each placed on a white posterboard and photographed with an iPhone in sunlight or room lighting, in six classes:

| class | photos |
|---|---|
| cardboard | 403 |
| glass | 501 |
| metal | 410 |
| paper | 594 |
| plastic | 482 |
| trash | 137 |

"Trash" means items that can't be recycled. The photos in the file used here have been resized to 512 x 384 pixels by the authors; the notebook shrinks them further, to 192 x 144.

**Download.** `dataset-resized.zip` (43 MB) is downloaded from the official repository the first time the mini project runs, and saved in `data/trashnet/`. If you're working offline, download it yourself from https://raw.githubusercontent.com/garythung/trashnet/master/data/dataset-resized.zip and put it in that folder.

**Source.** Gary Thung and Mindy Yang (2016). *Classification of Trash for Recyclability Status.* CS 229 project, Stanford University. Data and code: https://github.com/garythung/trashnet, released under the [MIT licence](https://github.com/garythung/trashnet/blob/master/LICENSE), copyright 2017 Gary Thung.

## Fashion-MNIST (lesson)

The lesson trains its CNN on Fashion-MNIST, the clothing photos from Module 17. The two training files (about 30 MB) are downloaded from the official repository the first time the lesson runs, and saved in `data/fashion_mnist/`. See [Module 17's data README](../../17-deep-learning-pytorch/data/README.md) for the details and the MIT licence (Xiao, Rasul and Vollgraf, 2017, Zalando Research).

## Sample photos (lesson)

The photos of the coffee, the cat, the rocket, the horse silhouette and the galaxies come with scikit-image (`skimage.data`), so nothing needs downloading. According to scikit-image's documentation, the coffee photo (Rachel Michetti) and the cat photo (Stefan van der Walt) are CC0, the horse silhouette (Andreas Preuss) is CC0, the rocket launch photo was released into the public domain by SpaceX, and the Hubble photo was taken by NASA and may be freely used in the public domain.

## Pretrained network (lesson and mini project)

ResNet-18 trained on ImageNet (`ResNet18_Weights.IMAGENET1K_V1`, about 45 MB) is downloaded by torchvision from PyTorch's servers on first use and cached in your home folder (`~/.cache/torch`), not here. ResNet was introduced by Kaiming He, Xiangyu Zhang, Shaoqing Ren and Jian Sun in *Deep Residual Learning for Image Recognition* (2016). torchvision is released under the BSD 3-Clause licence.
