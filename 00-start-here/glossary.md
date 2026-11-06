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

**Data leakage.** Information from the test data, or from the future, sneaking into training, so evaluation looks better than real performance will be. [2, 7, 11]

**Feature engineering.** Turning raw data into useful model inputs. [7, 11]

**One-hot encoding.** Turning a category into 0/1 columns, one per category. [7, 11]

**Standardisation.** Rescaling a feature to mean 0 and standard deviation 1, using statistics from the training data. [1, 6, 9]

**Drift.** The data a deployed model sees changing over time, away from what it was trained on. [11]

## Kinds of learning

**Supervised learning.** Learning from examples that include the right answer. [0]

**Classification.** Supervised learning where the answer is a category. [1, 2, 5]

**Regression.** Supervised learning where the answer is a number. [1, 9]

**Unsupervised learning.** Finding structure (groups, directions, outliers) in data without labels. [0]

**Clustering.** Grouping similar examples together, for example with k-means. [0]

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

**Epoch.** One pass through the whole training set. [4]

**Batch, mini-batch.** A subset of training examples processed together in one step. [3, 4]

**Backpropagation.** Computing the gradient of the loss for every parameter of a network with the chain rule, working backwards from the output. [3, 9]

**Overfitting.** Fitting the training data so closely, noise included, that performance on new data gets worse. [0, 8]

**Underfitting.** A model too simple to capture the real pattern. [0, 8]

**Generalisation.** How well a model performs on new data it hasn't seen. [0]

**Regularisation.** Anything that discourages a model from overfitting, such as penalising large weights. [4, 6, 11]

**Baseline.** The simplest reasonable prediction (such as always predicting the most common class), which any real model must beat. [6, 11]

## Models you build in this course

**k-nearest neighbours (k-NN).** Predicts from the labels of the k most similar training examples. [1, 6, 8]

**Naive Bayes.** A probabilistic classifier that treats features as independent given the class. [2, 5]

**Linear regression.** Predicts a number as a weighted sum of features. [1, 6, 9]

**Logistic regression.** A linear model passed through a sigmoid to give a probability, for classification. [6, 9, 10, 11]

**Neural network.** Layers of simple units (neurons), each a weighted sum followed by a non-linear function. [3, 6]

**Decision tree, random forest, gradient boosting.** Models built from yes/no questions about features; forests average many trees, boosting builds them one after another. [11]

**Transformer.** The neural network architecture behind modern language models, built around attention. [0]

**Attention.** A mechanism that lets each position in a sequence draw information from every other position, weighted by relevance. [0]

## Language models

**Token.** A unit of text a language model reads and writes: a word or a piece of a word. [0]

**Context window.** How many tokens a language model can take into account at once. [0]

**Temperature.** A setting that controls how random a model's sampling is. Low is predictable, high is varied. [0]

**Pretraining.** The first, largest stage of training an LLM: next-token prediction on huge amounts of text. [0]

**Fine-tuning.** Further training of a pretrained model on a smaller, specific dataset. [0]

**RLHF.** Reinforcement learning from human feedback: tuning a model towards responses people prefer. [0]

**Hallucination.** A model stating something false fluently, because it produces likely text rather than verified text. [0]

**Embedding.** A vector of numbers representing a word, sentence or item, arranged so that similar things have similar vectors. [2, 9]

**RAG (retrieval-augmented generation).** Searching documents for relevant passages and giving them to a language model as context. [0]

## Evaluation

**Accuracy.** The fraction of predictions that are correct. [1]

**Precision.** Of the cases predicted positive, the fraction that really are. [1, 2, 5]

**Recall.** Of the cases that really are positive, the fraction the model found. [1, 2, 5]

**Confusion matrix.** A table of predicted classes against true classes. [8, 11]

**Cross-validation.** Splitting data into k folds and testing on each in turn, to use every example for evaluation once. [11]

**Confidence interval.** A range that expresses the uncertainty of an estimate, such as a test accuracy. [9]

**Calibration.** Whether predicted probabilities match how often things actually happen. [8, 9]

## Engineering

**Pipeline.** A chain of preprocessing steps and a model that are fitted and used as one object. [11]

**API.** An interface other software uses to send requests, such as data to predict on, and get responses. [11]

**Model card.** A short document describing what a model does, how it was evaluated, and its limitations. [11]

**Reproducibility.** Being able to get the same result again from the same code, data and settings. [10]
