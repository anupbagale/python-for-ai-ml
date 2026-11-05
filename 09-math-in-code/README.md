# Module 9: Math in Code

The maths behind machine learning, written as code and checked: floating point, vectors and matrices, derivatives and gradients, the chain rule, gradient descent, probability, statistics and cross-entropy. Every idea follows the same three steps: write the formula, implement it in NumPy, and check it against something independent.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | From floating-point traps to a tiny autodiff engine, gradient descent, Bayes' rule, the bootstrap and cross-entropy | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/09-math-in-code/01_lesson.ipynb) |
| 2 | Exercises | 12 formulas to implement, with checks against exact answers, numerical approximations and simulations | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/09-math-in-code/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the maths behind each one and the bug each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/09-math-in-code/03_solutions.ipynb) |
| 4 | Mini project | Logistic regression by hand, checked three ways | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/09-math-in-code/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**Numbers on a computer.** Why `0.1 + 0.2 != 0.3`, how a textbook variance formula returns a negative variance, and the max-subtraction trick behind a stable softmax and log-sum-exp.

**Vectors and matrices.** Dot products, norms and cosine similarity, `X @ w` as a prediction for every row, matrices as transformations, and why `solve` beats `inv`.

**Derivatives and gradients.** Forward and central differences, the step-size trade-off between approximation and rounding error, and gradient checks.

**The chain rule.** Backpropagation by hand for a single neuron, then a small `Value` class that does it automatically, the same idea as PyTorch's autograd.

**Gradient descent.** Why the steepest direction limits the learning rate, why standardising features helps, and testing training code against an exact least-squares answer.

**Probability.** The law of large numbers, Bayes' rule and the base-rate problem (checked by simulating a million people), and the normal distribution.

**Statistics.** Why `ddof=1`, the central limit theorem, standard errors, bootstrap confidence intervals, and permutation tests.

**Information.** Entropy, cross-entropy and KL divergence, and why cross-entropy punishes confident mistakes.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `stable_softmax`, `log_sum_exp`, `central_difference`, `cosine_similarity` |
| Medium | `numerical_gradient`, `gradient_descent`, `cross_entropy`, `bayes_posterior`, `bootstrap_ci` |
| Hard | `fit_linear_gd`, `logistic_loss_and_grad`, `permutation_test` |

The checks target the bugs that don't crash. They include:

- a softmax that subtracts the wrong maximum
- a numerical gradient that leaves its input slightly changed
- a gradient descent path where every row is the same array
- a bootstrap that resamples without replacement
- a log loss that turns into `nan` for large scores
- a one-sided permutation test

## Mini project

Can you tell a penguin's sex from its measurements? Logistic regression written from scratch, with every step checked independently:

- **The gradient.** Compared with a numerical one, with two planted bugs to show the check catches them.
- **The optimum.** Gradient descent and Newton's method train to the same weights, agreeing to 12 decimal places. Newton gets there in 8 steps, while gradient descent needs thousands of epochs.
- **The weights.** Bootstrap intervals show which measurements matter.
- **The result.** Test accuracy with a confidence interval, compared with baselines, and a paired permutation test between two models.

The results are reported as they came out:

- Body mass alone is barely better than guessing (58%), because it mostly tells you the species.
- All four measurements plus species reach 91% on the test set.
- The test can't show that adding species helps: the two models disagree on just 2 of 81 penguins.

## Prerequisites

Modules [1](../01-python-foundations) to [8](../08-visualization). Module 6 (NumPy) matters most.

## Next

Module 10: Engineering Practices (coming soon)
