# Python for AI & ML Engineering

A hands-on course on the Python you actually need for machine learning work.

Most Python tutorials teach the language in general. Most ML tutorials assume you already know Python well and jump straight into libraries. This repo sits in between. Every topic is taught with machine learning in mind: classes are explained by building a dataset loader, generators by streaming data in batches, and floats by looking at why your loss sometimes turns into `nan`.

Each module has four parts:

- **Lesson**: concepts explained step by step, with runnable code
- **Exercises**: problems with built-in checks, so you know right away if your answer is correct
- **Solutions**: worked answers with notes on why they're written that way
- **Mini project**: a small build that uses everything from the module

Everything runs in Google Colab. Click the badge next to any notebook and it opens in your browser, with nothing to install.

## Who this is for

- People who know a bit of Python and want to move into AI/ML engineering
- ML learners who can follow tutorials but get stuck when writing their own code
- Anyone who wants to understand what NumPy, pandas and PyTorch are doing underneath

No prior ML knowledge is needed. The math is introduced when it's needed and kept practical.

## Roadmap

| #  | Module | Topics | Status |
|----|--------|--------|--------|
| 01 | [Python Foundations](01-python-foundations) | Types, floats, strings, control flow, functions, comprehensions, errors | Available |
| 02 | [Data Structures](02-data-structures) | Lists, tuples, dicts, sets, `collections`, `heapq`, and how fast each one really is | Available |
| 03 | [OOP for ML](03-oop-for-ml) | Classes, dunder methods, properties, inheritance, dataclasses, PyTorch-style `Dataset`, `DataLoader` and autograd | Available |
| 04 | [Pythonic Tools](04-pythonic-tools) | Iterators, generators, `itertools`, decorators, context managers, type hints | Available |
| 05 | Files and Data | pathlib, CSV, JSON, reading messy real-world data | Planned |
| 06 | NumPy | Arrays, broadcasting, vectorization, linear algebra | Planned |
| 07 | pandas | Cleaning, groupby, merging, feature engineering | Planned |
| 08 | Visualization | Matplotlib and Seaborn for exploring data and models | Planned |
| 09 | Math in Code | Gradients, probability and statistics implemented by hand | Planned |
| 10 | Engineering Practices | Virtual environments, testing, logging, project layout, profiling | Planned |
| 11 | Capstone | End-to-end pipeline: raw data, trained model, simple API | Planned |

## Notebooks

| Module | Lesson | Exercises | Solutions | Mini project |
|--------|--------|-----------|-----------|--------------|
| [01 Python Foundations](01-python-foundations) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/04_mini_project.ipynb) k-NN classifier |
| [02 Data Structures](02-data-structures) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/04_mini_project.ipynb) Naive Bayes spam filter |
| [03 OOP for ML](03-oop-for-ml) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/04_mini_project.ipynb) Autograd engine + neural net |
| [04 Pythonic Tools](04-pythonic-tools) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/01_lesson.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/02_exercises.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/03_solutions.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/04_mini_project.ipynb) Hyperparameter search |

## How to use this repo

1. Go through the modules in order. Later modules build on earlier ones.
2. Read the lesson and run every cell. Change the code and see what breaks.
3. Do the exercises without looking at the solutions. Each exercise has a check cell that tells you which input your code got wrong.
4. Compare your answers with the solutions afterwards. There's usually more than one good way to solve a problem.
5. Build the mini project, then try the extension ideas at the end of it.

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

## Repo structure

```
python-for-ai-ml/
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
├── ...more modules as they're added
├── requirements.txt
└── LICENSE
```

## License

MIT. Use it, share it, adapt it for your own study group or class.
