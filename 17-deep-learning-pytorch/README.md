# Module 17: Deep Learning with PyTorch

The start of Part 3. Image recognition, speech and language models all rest on the same few ideas: tensors to hold the data, automatic differentiation to compute gradients, layers stacked into a network, and a training loop that nudges the weights downhill. This module teaches those ideas in PyTorch, along with the practical skills that decide whether a network trains well or not at all: initialisation, normalisation, regularisation, learning-rate schedules and debugging.

Everything runs on an ordinary CPU, in Colab or on your own computer. No GPU is needed.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Tensors, autograd, the training loop by hand and with `nn`, `nn.Module`, datasets and data loaders, logits and cross-entropy, optimisers, initialisation, batch normalisation, dropout and weight decay, learning-rate schedules, devices, debugging, saving and loading | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/17-deep-learning-pytorch/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises: training tools, then SGD, Adam and batch normalisation written to match PyTorch exactly | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/17-deep-learning-pytorch/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/17-deep-learning-pytorch/03_solutions.ipynb) |
| 4 | Mini project | A clothing classifier for Fashion-MNIST, from a linear baseline to a regularised network, tested once and studied through its mistakes | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/17-deep-learning-pytorch/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The data is described in [data/README.md](data/README.md).

## What you'll learn

- **Tensors.** Creating them, shapes and broadcasting, `dtype`, and when a tensor shares memory with a NumPy array.
- **Autograd.** `requires_grad`, `backward()`, why gradients accumulate, and `torch.no_grad()`.
- **The training loop.** Written by hand first, then with `nn.Linear`, a loss function and an optimiser; a silent broadcasting bug caught in the act.
- **Building networks.** `nn.Module` classes, `nn.Sequential`, counting parameters.
- **Data.** `Dataset` and `DataLoader`, batches and shuffling, on handwritten digits.
- **Classification.** Logits, softmax and cross-entropy, and the standard training and evaluation functions.
- **Optimisers.** SGD, momentum and Adam compared, and what too high a learning rate does.
- **Initialisation.** Why weights that are too small or too large kill the signal in a deep network, and how Kaiming initialisation fixes it.
- **Normalisation.** A 10-layer network that doesn't train at all without batch normalisation, and does with it.
- **Overfitting.** Dropout, weight decay and early stopping, on a small dataset with noisy labels.
- **Learning-rate schedules.** Step, cosine and one-cycle.
- **Devices.** Writing code that runs on a CPU, an NVIDIA GPU or an Apple GPU.
- **Debugging.** Checking the initial loss, overfitting a single batch, watching gradient norms, and spotting the classic bugs.
- **Saving and loading.** State dictionaries, and loading safely with `weights_only=True`.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `count_parameters`, `accuracy_from_logits`, `stable_softmax`, `warmup_cosine_lr` |
| Medium | `gradient_at`, `make_mlp`, `cross_entropy_from_logits`, `train_one_epoch`, `evaluate_model` |
| Hard | `sgd_momentum_step`, `adam_step`, `batchnorm1d_forward` |

The checks compare your code with PyTorch itself wherever possible, and catch the classic mistakes:

- a softmax that overflows, or underflows for rows on different scales
- forgetting `zero_grad()`, `model.train()` or `model.eval()`
- averaging batch losses without weighting the smaller last batch
- momentum with the learning rate in the wrong place
- Adam without bias correction, or with `eps` inside the square root
- batch normalisation with the wrong variance, or with gradients cut off

## Mini project

**A clothing classifier.** 70,000 small photos of clothing from Zalando's catalogue, in ten classes.

- **Baseline.** A single linear layer, whose weights can be drawn as images.
- **An MLP** that beats it, then overfits within a few epochs: the validation loss shows it long before the accuracy does.
- **Regularisation.** Dropout, AdamW weight decay and a one-cycle schedule keep the validation loss down in a network twice the size.
- **One honest test.** The model is chosen on validation and tested once, reaching about 90% accuracy.
- **The mistakes.** Shirts are the hardest class by far; the most confident mistakes include some debatable labels, and the probabilities turn out to be a little overconfident.
- **Deployment.** The model is saved with its preprocessing and wrapped in a function that classifies raw images.

## Prerequisites

Modules [1](../01-python-foundations) to [16](../16-time-series-and-recommendations). Module 3 (the PyTorch-style `Dataset` and autograd), Module 6 (broadcasting and the network built in NumPy), Module 9 (gradients and the chain rule) and Module 12 (overfitting and regularisation) are used most.

## Next

[Module 18: Computer Vision](../18-computer-vision).
