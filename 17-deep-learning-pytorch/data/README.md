# Data

## Fashion-MNIST (mini project)

70,000 greyscale photos of clothing and accessories from Zalando's online catalogue, each 28 x 28 pixels, in ten classes:

| label | class | label | class |
|---|---|---|---|
| 0 | T-shirt/top | 5 | Sandal |
| 1 | Trouser | 6 | Shirt |
| 2 | Pullover | 7 | Sneaker |
| 3 | Dress | 8 | Bag |
| 4 | Coat | 9 | Ankle boot |

There are 60,000 training images and 10,000 test images, with exactly 6,000 training and 1,000 test images per class. Pixel values run from 0 (background) to 255.

**Not stored here.** The four files are about 30 MB together, which is more than this repository should hold, so the mini project downloads them from the official Fashion-MNIST repository on GitHub the first time it runs, and saves them in `data/fashion_mnist/`. Later runs read the saved copy. If you're working offline, download the four files from the address below yourself and put them in that folder.

| file | contents |
|---|---|
| `train-images-idx3-ubyte.gz` | training images, 60,000 x 28 x 28 |
| `train-labels-idx1-ubyte.gz` | training labels |
| `t10k-images-idx3-ubyte.gz` | test images, 10,000 x 28 x 28 |
| `t10k-labels-idx1-ubyte.gz` | test labels |

The files use the simple IDX format from the original MNIST: a short header giving the number of dimensions and their sizes, followed by the raw bytes. The notebook reads them with a few lines of NumPy.

**Source.** Xiao, H., Rasul, K., and Vollgraf, R. (2017). Fashion-MNIST: a Novel Image Dataset for Benchmarking Machine Learning Algorithms. arXiv:1708.07747. https://arxiv.org/abs/1708.07747. Data from https://github.com/zalandoresearch/fashion-mnist, released under the [MIT licence](https://github.com/zalandoresearch/fashion-mnist/blob/master/LICENSE), copyright 2017 Zalando SE.

## Handwritten digits (lesson)

The lesson uses the small digits dataset that comes with scikit-learn (`sklearn.datasets.load_digits`): 1,797 images of handwritten digits, 8 x 8 pixels each, with values from 0 to 16. It's a copy of the test set of the UCI Optical Recognition of Handwritten Digits dataset by E. Alpaydin and C. Kaynak (1998), https://doi.org/10.24432/C50P49, licensed under CC BY 4.0. Nothing needs downloading.
