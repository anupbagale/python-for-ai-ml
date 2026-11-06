# Python for AI & ML Engineering

A hands-on course that takes you from your first line of Python to building, evaluating and deploying machine learning systems.

Most Python tutorials teach the language in general. Most ML tutorials assume you already know Python well and jump straight into libraries. This repo sits in between. Every topic is taught with machine learning in mind: classes are explained by building a dataset loader, generators by streaming data in batches, and floats by looking at why your loss sometimes turns into `nan`.

The course is planned in three parts. **Part 1**, available now, teaches Python, data and machine learning foundations together and ends with a capstone that goes from raw field data to a tested prediction API. **Part 2** goes deeper into machine learning itself, and **Part 3** into deep learning and modern AI, including how large language models work and how to build with them.

New to AI? Start with [Start Here](00-start-here), a short introduction to what AI and machine learning actually are, with programs you can run in your browser.

Each module has four parts:

- **Lesson**: concepts explained step by step, with runnable code
- **Exercises**: problems with built-in checks, so you know right away if your answer is correct
- **Solutions**: worked answers with notes on why they're written that way
- **Mini project**: a small build that uses everything from the module (in Module 11, the capstone project)

Everything runs in Google Colab. Click the badge next to any notebook and it opens in your browser, with nothing to install.

## Free for everyone

This course is free, and it will stay free. The goal is simple: anyone, anywhere, with an internet connection and the willingness to work through it, should be able to become a capable machine learning engineer.

- **No cost, no sign-up for the course.** Everything is in this repository under the MIT licence. You can study it, copy it, translate it, and teach classes with it.
- **Several ways to run it**, so nobody is locked out:
  - **Google Colab**: click a badge; needs a free Google account.
  - **Binder**: [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/anupbagale/python-for-ai-ml/HEAD) opens the whole course in a browser with **no account at all**. It can take a few minutes to start.
  - **Your own computer**: any ordinary laptop works (see [Running locally](#running-locally)), and once installed it works offline.
  - **Just reading**: every notebook is saved with its outputs, so you can read it on GitHub with the results and charts already shown.
- **No expensive hardware.** Part 1 runs on a basic laptop or free Colab, with small datasets and no graphics card. Parts 2 and 3 will be designed the same way: free Colab is enough, with open datasets and open models, and **never a paid API key**.
- **No background assumed.** No maths beyond school arithmetic is needed to start; the maths is taught in code when it's needed. Plain English throughout, with a [glossary](00-start-here/glossary.md) for every term.
- **Charts are readable by everyone.** Colours are chosen to stay distinguishable with colour blindness, groups also differ by marker shape, and every chart is explained in the text.

## Who this is for

- Complete beginners who want to learn AI and machine learning properly, from the ground up
- People who know a bit of Python and want to move into AI/ML engineering
- ML learners who can follow tutorials but get stuck when writing their own code
- Anyone who wants to understand what NumPy, pandas and PyTorch are doing underneath

No prior ML knowledge is needed. The math is introduced when it's needed and kept practical.

## Roadmap

### Part 1: Python, data and ML foundations

| #  | Module | Topics | Status |
|----|--------|--------|--------|
| 00 | [Start Here](00-start-here) | What AI, machine learning, deep learning and LLMs are, how learning works, the kinds of ML, overfitting, and how a real project runs | Available |
| 01 | [Python Foundations](01-python-foundations) | Types, floats, strings, control flow, functions, comprehensions, errors | Available |
| 02 | [Data Structures](02-data-structures) | Lists, tuples, dicts, sets, `collections`, `heapq`, and how fast each one really is | Available |
| 03 | [OOP for ML](03-oop-for-ml) | Classes, dunder methods, properties, inheritance, dataclasses, PyTorch-style `Dataset`, `DataLoader` and autograd | Available |
| 04 | [Pythonic Tools](04-pythonic-tools) | Iterators, generators, `itertools`, decorators, context managers, type hints | Available |
| 05 | [Files and Data](05-files-and-data) | pathlib, encodings, CSV, JSON, cleaning messy data, saving models, with real penguin data | Available |
| 06 | [NumPy](06-numpy) | Arrays, views, broadcasting, vectorization, linear algebra, a neural network from scratch | Available |
| 07 | [pandas](07-pandas) | Reading, cleaning, groupby, merging, encoding, time-based features and leakage | Available |
| 08 | [Visualization](08-visualization) | Matplotlib, Seaborn, choosing charts, honest colour and axes, model diagnostic plots | Available |
| 09 | [Math in Code](09-math-in-code) | Floating point, vectors, gradients, the chain rule, gradient descent, probability and statistics, by hand and checked | Available |
| 10 | [Engineering Practices](10-engineering-practices) | Project layout, environments, pytest, logging, configs, CLIs, reproducibility, profiling | Available |
| 11 | [Capstone](11-capstone) | scikit-learn pipelines, honest evaluation, and a tested FastAPI service, built from raw field data | Available |

### Part 2: Machine learning in depth

| #  | Module | Topics | Status |
|----|--------|--------|--------|
| 12 | Supervised Learning in Depth | Linear and logistic models, L1 and L2 regularisation, bias and variance, feature engineering, regression metrics, learning curves | Planned |
| 13 | Evaluating Classifiers | Precision, recall, F1, ROC and precision-recall curves, thresholds, calibration, class imbalance, cost-sensitive decisions | Planned |
| 14 | Trees and Ensembles | Decision trees and gradient boosting from scratch, random forests, XGBoost and LightGBM, feature importance and SHAP | Planned |
| 15 | Unsupervised Learning | k-means, hierarchical clustering, DBSCAN, PCA from scratch, t-SNE and UMAP, anomaly detection | Planned |
| 16 | Time Series and Recommendations | Forecasting, time-based validation, lag features, collaborative filtering, matrix factorisation | Planned |

### Part 3: Deep learning and modern AI

| #  | Module | Topics | Status |
|----|--------|--------|--------|
| 17 | Deep Learning with PyTorch | Tensors, autograd, `nn.Module`, optimisers, initialisation, normalisation, dropout, learning-rate schedules, GPUs, debugging training | Planned |
| 18 | Computer Vision | Convolutions from scratch, CNNs, data augmentation, transfer learning with pretrained models | Planned |
| 19 | Sequences and Transformers | Tokenisation, embeddings, attention from scratch, training a small GPT | Planned |
| 20 | LLMs in Practice | Pretrained models, prompting, embeddings and vector search, retrieval-augmented generation, fine-tuning with LoRA, evaluating LLM applications | Planned |
| 21 | Responsible and Production AI | Fairness, interpretability, privacy, experiment tracking, containers, monitoring, and a final capstone | Planned |

## Notebooks

| Module | Lesson | Exercises | Solutions | Mini project |
|--------|--------|-----------|-----------|--------------|
| [00 Start Here](00-start-here) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/00-start-here/01_lesson.ipynb) | Self-check questions in the lesson | Answers in the lesson | [Glossary](00-start-here/glossary.md) |
| [01 Python Foundations](01-python-foundations) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/04_mini_project.ipynb) k-NN classifier |
| [02 Data Structures](02-data-structures) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/04_mini_project.ipynb) Naive Bayes spam filter |
| [03 OOP for ML](03-oop-for-ml) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/04_mini_project.ipynb) Autograd engine + neural net |
| [04 Pythonic Tools](04-pythonic-tools) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/04_mini_project.ipynb) Hyperparameter search |
| [05 Files and Data](05-files-and-data) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/05-files-and-data/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/05-files-and-data/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/05-files-and-data/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/05-files-and-data/04_mini_project.ipynb) Penguin classifier, saved as JSON |
| [06 NumPy](06-numpy) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/06-numpy/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/06-numpy/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/06-numpy/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/06-numpy/04_mini_project.ipynb) Digit-reading neural network |
| [07 pandas](07-pandas) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/07-pandas/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/07-pandas/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/07-pandas/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/07-pandas/04_mini_project.ipynb) Customer churn from raw tables |
| [08 Visualization](08-visualization) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/08-visualization/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/08-visualization/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/08-visualization/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/08-visualization/04_mini_project.ipynb) Visual model report |
| [09 Math in Code](09-math-in-code) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/09-math-in-code/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/09-math-in-code/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/09-math-in-code/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/09-math-in-code/04_mini_project.ipynb) Logistic regression by hand |
| [10 Engineering Practices](10-engineering-practices) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/10-engineering-practices/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/10-engineering-practices/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/10-engineering-practices/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/10-engineering-practices/04_mini_project.ipynb) From notebook to project |
| [11 Capstone](11-capstone) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/11-capstone/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/11-capstone/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/11-capstone/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/11-capstone/04_capstone.ipynb) Penguin sexing service, raw data to API |

## How long it takes

Roughly 6 to 10 hours per module, including the exercises and the project, and about 1 hour for Start Here. At 5 to 8 hours a week, Part 1 takes two to three months. There's no deadline: go at the pace that works for you, and repeat anything that didn't stick.

A module a week works well for most people:

1. **Day 1 to 2:** read the lesson and run every cell, changing things as you go.
2. **Day 3 to 4:** the exercises, without looking at the solutions.
3. **Day 5:** compare with the solutions and read the notes.
4. **Day 6 to 7:** the mini project and at least one of its extension ideas.

## How to use this repo

1. If AI and machine learning are new to you, read [Start Here](00-start-here) first.
2. Go through the modules in order. Later modules build on earlier ones.
3. Read the lesson and run every cell. Change the code and see what breaks.
4. Do the exercises without looking at the solutions. Each exercise has a check cell that tells you which input your code got wrong.
5. Compare your answers with the solutions afterwards. There's usually more than one good way to solve a problem.
6. Build the mini project, then try the extension ideas at the end of it.

The `cheatsheet.md` in each module is a one-page summary for revision.

## Running locally

If you prefer working on your own machine:

```bash
git clone https://github.com/anupbagale/python-for-ai-ml.git
cd python-for-ai-ml
python -m venv .venv
source .venv/bin/activate        # on Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

Python 3.10 or newer is recommended.

## Getting help

Stuck on something, or found a mistake? [Open an issue](https://github.com/anupbagale/python-for-ai-ml/issues) on GitHub. Say which module and notebook, which cell, what you expected and what happened (copy the full error message). Questions are welcome: if something confused you, it probably confused someone else too, and the answer can improve the course for everyone.

## Contributing

Corrections, clearer explanations, extra exercises and **translations** are all welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Repo structure

```
python-for-ai-ml/
├── 00-start-here/
│   ├── README.md
│   ├── 01_lesson.ipynb
│   └── glossary.md
├── 01-python-foundations/
│   ├── README.md
│   ├── 01_lesson.ipynb
│   ├── 02_exercises.ipynb
│   ├── 03_solutions.ipynb
│   ├── 04_mini_project.ipynb
│   └── cheatsheet.md
├── 02-data-structures/
│   └── (same layout)
├── 03-oop-for-ml/
│   └── (same layout)
├── 04-pythonic-tools/
│   └── (same layout)
├── 05-files-and-data/
│   ├── (same layout)
│   └── data/penguins.csv
├── 06-numpy/
│   └── (same layout)
├── 07-pandas/
│   └── (same layout)
├── 08-visualization/
│   └── (same layout)
├── 09-math-in-code/
│   └── (same layout)
├── 10-engineering-practices/
│   └── (same layout)
├── 11-capstone/
│   ├── (same layout, with 04_capstone.ipynb)
│   └── data/penguins_raw.csv
├── CONTRIBUTING.md
├── requirements.txt
└── LICENSE
```

## Data

The penguin data used throughout the course comes from the Palmer Station Long Term Ecological Research Program, collected by Dr. Kristen Gorman, and packaged as [palmerpenguins](https://allisonhorst.github.io/palmerpenguins/) by Allison Horst, Alison Hill and Kristen Gorman. It's public domain (CC0). Details and citations are in [05-files-and-data/data](05-files-and-data/data/README.md) and [11-capstone/data](11-capstone/data/README.md).

## License

MIT. Use it, share it, adapt it for your own study group or class.
