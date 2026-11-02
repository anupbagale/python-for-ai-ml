# Module 2: Data Structures

Lists, tuples, dictionaries and sets in depth, plus the `collections` and `heapq` modules. The focus is on two things: what each structure can do, and how fast it does it. Picking the wrong structure is one of the most common reasons Python data code is slow.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Concepts, speed experiments, and a text vocabulary builder | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/01_lesson.ipynb) |
| 2 | Exercises | 12 problems (Easy, Medium, Hard) with automatic checks | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions with notes on choosing the right structure | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/03_solutions.ipynb) |
| 4 | Mini project | A Naive Bayes spam filter built from scratch | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/02-data-structures/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**Lists.** `append` vs `extend`, `sort` vs `sorted`, multi-key sorting with tuple keys, star unpacking, the `[[0] * 3] * 3` trap, and shallow vs deep copies of nested configs.

**Tuples.** Packing and unpacking, the single-item tuple gotcha, why tuples can be dictionary keys and lists can't, and `namedtuple`.

**Dictionaries.** Safe lookups with `.get`, merging configs with `{**a, **b}` and `|`, nested dictionaries, and grouping with `setdefault`.

**Sets.** Removing duplicates (with and without keeping order), set operations, finding out-of-vocabulary words, and detecting train/test leakage.

**Speed.** Measured experiments showing `x in list` vs `x in set`, why hash tables make lookups O(1), and why `list.insert(0, x)` is slow. A cost table for every structure.

**collections and heapq.** `Counter`, `defaultdict`, `deque` (moving averages and replay buffers), `heapq.nlargest` for top-k predictions, and priority queues.

**Putting it together.** A vocabulary for a text model: token counts, special tokens, encoding, decoding and padding.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `unique_in_order`, `invert_dict`, `merge_configs`, `oov_words` |
| Medium | `group_by_label`, `top_k`, `word_frequencies`, `deep_get`, `records_to_columns` |
| Hard | `stratified_split`, `most_similar`, `bigram_model` |

The last exercise builds a tiny bigram language model and uses it to generate text.

## Mini project

A spam filter using Naive Bayes, written with nothing but `Counter`, `defaultdict` and sets. It covers Laplace smoothing, why you add log probabilities instead of multiplying probabilities, softmax with the max-subtraction trick, precision and recall, and inspecting which words the model finds most "spammy". It reaches about 95% accuracy on held-out messages, and the notebook also walks through a message it gets wrong and explains why.

## Prerequisites

[Module 1: Python Foundations](../01-python-foundations)

## Next

[Module 3: OOP for ML](../03-oop-for-ml)
