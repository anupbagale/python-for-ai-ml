# Glossary

The terms used throughout the course, in plain language. The module where each one is covered in depth is in brackets.

## The field

**Artificial intelligence (AI).** Computer systems that do things we'd call intelligent if a person did them. Includes learning and non-learning approaches. [0]

**Machine learning (ML).** The part of AI where systems learn from data instead of following hand-written rules. [0]

**Deep learning.** Machine learning with neural networks of many layers. [0, 3, 6]

**Generative AI.** Models that create new content (text, images, audio, code) rather than only classifying or predicting a number. [0]

**Large language model (LLM).** A very large neural network, usually a Transformer, trained to predict the next token of text, then tuned to follow instructions. [0]

## Data

**Dataset.** A collection of examples. In a table, usually one row per example. [5, 7]

**Feature.** An input to a model: a column such as body mass or bill length. Also called a variable or attribute. [1, 7]

**Label, target.** The answer a supervised model learns to predict. [0, 1]

**Training set.** The examples a model learns from. [1]

**Validation set.** Data held back to choose between models or settings during development. [8]

**Test set.** Data held back until the very end, used once, for an honest estimate of performance on new data. [1, 9, 11]

**Data leakage.** Information from the test data, or from the future, sneaking into training, so evaluation looks better than real performance will be. [2, 7, 11, 13]

**Feature engineering.** Turning raw data into useful model inputs. [7, 11]

**One-hot encoding.** Turning a category into 0/1 columns, one per category. [7, 11]

**Standardisation.** Rescaling a feature to mean 0 and standard deviation 1, using statistics from the training data. [1, 6, 9]

**Drift.** The data a deployed model sees changing over time, away from what it was trained on. [11, 13]

## Kinds of learning

**Supervised learning.** Learning from examples that include the right answer. [0]

**Classification.** Supervised learning where the answer is a category. [1, 2, 5]

**Regression.** Supervised learning where the answer is a number. [1, 9]

**Unsupervised learning.** Finding structure (groups, directions, outliers) in data without labels. [0, 15]

**Clustering.** Grouping similar examples together, for example with k-means. [0, 15]

**Dimensionality reduction.** Describing data with fewer numbers while keeping as much of its structure as possible. **PCA** (principal component analysis) finds the directions of greatest variation; t-SNE and UMAP make 2D maps that keep neighbours together. [15]

**Anomaly detection.** Finding examples that don't fit the pattern of the rest, such as fraud or faulty readings. [15]

**Silhouette.** A score from -1 to 1 for how well each point sits in its own cluster compared with the nearest other cluster. [15]

**Time series.** Measurements taken in order over time, such as daily sales. The order matters and neighbouring values depend on each other. [16]

**Seasonality.** A pattern that repeats with a fixed period: every day, week or year. [16]

**Autocorrelation.** The correlation between a series and a delayed copy of itself; the reason the past helps predict the future. [16]

**Lag feature.** A feature holding an earlier value of the series, such as yesterday's sales, used to forecast the next value. [16]

**Recommender system.** A model that predicts which items (products, films, songs) a person will want, usually from what many people liked or bought. [16]

**Collaborative filtering.** Recommending from the behaviour of many users: people who agreed in the past will probably agree in the future. [16]

**Matrix factorisation.** Describing each user and item with a short list of learnt numbers (factors), so that a rating is predicted by combining the two lists. [16]

**Cold start.** The problem of recommending to a new user, or recommending a new item, with no history to learn from. [16]

**Reinforcement learning (RL).** An agent learning which actions earn rewards, by trial and error. [0]

**Self-supervised learning.** Learning from data that labels itself, such as predicting the next word of a text. The main way LLMs are pretrained. [0]

## Models and training

**Model.** A function with adjustable numbers (parameters) that maps inputs to predictions. [0]

**Parameters, weights.** The numbers inside a model that training adjusts. [0, 3]

**Hyperparameters.** Settings chosen before training, such as the learning rate or `k` in k-nearest neighbours. [4, 11]

**Loss.** A number measuring how wrong a model's predictions are. Training tries to make it small. [0, 9]

**Gradient.** The direction in which a function increases fastest; for a loss, it says how to change each parameter to increase the loss, so training moves the other way. [9]

**Gradient descent.** Repeatedly nudging parameters a small step against the gradient to reduce the loss. [1, 9]

**Learning rate.** The size of each gradient descent step. Too small is slow; too big diverges. [1, 9]

**Epoch.** One pass through the whole training set. [4, 17]

**Batch, mini-batch.** A subset of training examples processed together in one step. [3, 4, 17]

**Backpropagation.** Computing the gradient of the loss for every parameter of a network with the chain rule, working backwards from the output. [3, 9, 17]

**Overfitting.** Fitting the training data so closely, noise included, that performance on new data gets worse. [0, 8, 12, 14, 17]

**Underfitting.** A model too simple to capture the real pattern. [0, 8]

**Generalisation.** How well a model performs on new data it hasn't seen. [0]

**Regularisation.** Anything that discourages a model from overfitting, such as penalising large weights (ridge and lasso). [4, 6, 11, 12, 17]

**Bias and variance.** The two ways a model's predictions go wrong: bias is being wrong the same way every time (too simple), variance is being wrong differently for each training set (too flexible). [12]

**Baseline.** The simplest reasonable prediction (such as always predicting the most common class), which any real model must beat. [6, 11]

## Models you build in this course

**k-nearest neighbours (k-NN).** Predicts from the labels of the k most similar training examples. [1, 6, 8]

**Naive Bayes.** A probabilistic classifier that treats features as independent given the class. [2, 5]

**Linear regression.** Predicts a number as a weighted sum of features. [1, 6, 9]

**Logistic regression.** A linear model passed through a sigmoid to give a probability, for classification. [6, 9, 10, 11]

**Neural network.** Layers of simple units (neurons), each a weighted sum followed by a non-linear function. [3, 6, 17]

**Decision tree, random forest, gradient boosting.** Models built from yes/no questions about features; forests average many trees, boosting builds them one after another. [11, 14]

**Bagging.** Training many models on bootstrap samples of the data and averaging them, which reduces variance. The examples a model didn't see are its **out-of-bag** examples, a free validation set. [14]

**Ensemble.** A model that combines the predictions of many models, such as a random forest or a boosted model. [14]

**Transformer.** The neural network architecture behind modern language models, built around attention. [0, 19]

**Attention.** A mechanism that lets each position in a sequence draw information from every other position, weighted by relevance. [0, 19]

## Deep learning

**Tensor.** PyTorch's array: like a NumPy array, but it can live on a GPU and record operations for autograd. [17]

**Autograd.** Automatic differentiation: the framework records the operations that produce the loss and computes every gradient from that record. [3, 9, 17]

**Activation function.** The non-linear function applied after each layer's weighted sum, such as ReLU, `max(0, x)`. Without it, a deep network would be one linear model. [6, 17]

**Logits.** A classifier's raw scores, one per class, before softmax turns them into probabilities. [17]

**Softmax.** Turns a vector of scores into probabilities that are positive and add up to 1. [9, 17]

**Cross-entropy.** The standard loss for classification: minus the log of the probability given to the true class. [9, 17]

**Optimiser.** The rule that turns gradients into weight updates: SGD, SGD with momentum, Adam, AdamW. [17]

**Momentum.** Keeping a running sum of recent gradients and stepping along it, which speeds up consistent directions and damps zig-zagging. [17]

**Adam, AdamW.** Optimisers that scale each weight's step by the size of its recent gradients. AdamW applies weight decay directly to the weights. [17]

**Weight decay.** Shrinking every weight a little at each step; the neural network version of ridge regularisation. [17]

**Dropout.** Switching off a random fraction of units at each training step, so the network can't rely on any one of them. Off during evaluation. [17]

**Initialisation.** The random starting weights. Too small or too large and the signal dies or explodes in a deep network; Kaiming initialisation suits ReLU networks. [17]

**Batch normalisation.** Rescaling each feature to mean 0 and variance 1 within a batch, then letting the network learn a new scale and shift. Helps deep networks train. [17]

**Learning-rate schedule.** Changing the learning rate during training, often a warmup followed by a slow decay (cosine or one-cycle). [17]

**Early stopping.** Keeping the weights from the epoch with the best validation loss, instead of the last epoch. [17]

**GPU.** A graphics processor: thousands of simple cores that do the matrix arithmetic of neural networks much faster than a CPU. [17]

**Convolution.** Sliding a small grid of weights (a kernel or filter) across an image and taking a weighted sum at each position. The basis of convolutional neural networks (CNNs). [18]

**Kernel (filter).** The small grid of weights in a convolution. A CNN learns its kernels; classic hand-made ones blur, sharpen or find edges. [18]

**Feature map.** The output of one kernel over the whole image: high where the image matches the pattern the kernel looks for. [18]

**Padding and stride.** Padding adds a border of zeros so the output keeps its size; stride is how far the kernel moves at each step. [18]

**Pooling.** Shrinking a feature map by summarising small blocks, usually with their maximum. [18]

**Receptive field.** The patch of the input image that can affect one unit of a layer. It grows with depth, pooling and strides. [18]

**Data augmentation.** Making new training examples by changing existing ones in ways that keep the answer, such as flips, crops and shifts. [18]

**Residual connection.** Adding a block's input to its output, so the block only has to learn a correction. Made very deep networks such as ResNet trainable. [18]

**Pretrained model.** A network already trained on a large dataset, such as ImageNet's 1.3 million photos, that you can use or adapt. [18]

**Transfer learning.** Reusing a pretrained network for a new task, either as a fixed feature extractor or by fine-tuning some of its layers. [18]

## Language models

**Token.** A unit of text a language model reads and writes: a word or a piece of a word. [0, 19]

**Tokenisation.** Splitting text into tokens and giving each a number. [19]

**Byte-pair encoding (BPE).** A way of learning subword tokens by repeatedly merging the most frequent pair of neighbouring tokens. [19]

**Language model.** A model that gives probabilities for the next token. Generating text means sampling one token at a time. [19]

**Perplexity.** `exp(cross-entropy)` of a language model on unseen text: roughly how many tokens it is choosing between. Lower is better. [19]

**Query, key, value.** The three vectors each position computes in attention: what it looks for, what it contains, and what it passes on. [19]

**Causal mask.** The mask that stops each position attending to later positions, so a model can't see the future. [19]

**Multi-head attention.** Several attentions run side by side on slices of the vector, each free to look for something different. [19]

**Position embedding.** A vector added to each token's embedding to tell the model where it is in the sequence. [19]

**Layer normalisation.** Rescaling each position's vector to mean 0 and standard deviation 1, then applying a learnt scale and shift. [19]

**GPT.** "Generative pretrained transformer": a stack of transformer blocks with a causal mask, trained to predict the next token. [19]

**Context window.** How many tokens a language model can take into account at once. [0, 19]

**Temperature.** A setting that controls how random a model's sampling is. Low is predictable, high is varied. [0, 19]

**Pretraining.** The first, largest stage of training an LLM: next-token prediction on huge amounts of text. [0]

**Fine-tuning.** Further training of a pretrained model on a smaller, specific dataset, usually with a small learning rate. [0, 18]

**RLHF.** Reinforcement learning from human feedback: tuning a model towards responses people prefer. [0]

**Hallucination.** A model stating something false fluently, because it produces likely text rather than verified text. [0]

**Embedding.** A vector of numbers representing a word, sentence or item, arranged so that similar things have similar vectors. [2, 9, 19]

**RAG (retrieval-augmented generation).** Searching documents for relevant passages and giving them to a language model as context. [0]

## Evaluation

**Accuracy.** The fraction of predictions that are correct. Misleading when one class is rare. [1, 13]

**Precision.** Of the cases predicted positive, the fraction that really are. [1, 2, 5, 13]

**Recall.** Of the cases that really are positive, the fraction the model found. Also called sensitivity. [1, 2, 5, 13]

**F1 score.** The harmonic mean of precision and recall, so it's only high when both are. [13]

**Base rate.** How common the positive class is. Every classification metric needs to be read next to it. [13]

**Confusion matrix.** A table of predicted classes against true classes: true and false positives and negatives. [8, 11, 13]

**Threshold.** The score above which a classifier predicts positive. 0.5 is a default, not a rule; the right value depends on what mistakes cost. [13]

**ROC curve, AUC.** The true positive rate against the false positive rate for every threshold. The area under it (AUC) is the chance that a random positive scores higher than a random negative. [13]

**Precision-recall curve.** Precision against recall for every threshold. More informative than the ROC curve when positives are rare. [13]

**Cross-validation.** Splitting data into k folds and testing on each in turn, to use every example for evaluation once. [11]

**Confidence interval.** A range that expresses the uncertainty of an estimate, such as a test accuracy. [9]

**Feature importance.** How much a model relies on each feature. Permutation importance, measured on held-out data, is the reliable kind. [14]

**Partial dependence.** A model's average prediction as one feature varies, showing the shape of its effect. [14]

**Calibration.** Whether predicted probabilities match how often things actually happen. [8, 9, 13]

**Class imbalance.** When one class is much rarer than another, as in fraud or disease detection. [13]

## Engineering

**Pipeline.** A chain of preprocessing steps and a model that are fitted and used as one object. [11]

**API.** An interface other software uses to send requests, such as data to predict on, and get responses. [11]

**Model card.** A short document describing what a model does, how it was evaluated, and its limitations. [11]

**Reproducibility.** Being able to get the same result again from the same code, data and settings. [10]
