# Module 5: Files and Data

Reading, writing, checking and cleaning real data files, with a real dataset: measurements of 344 penguins from the Palmer Archipelago in Antarctica. Pure Python with `pathlib`, `csv` and `json`, so that when pandas does this for you in Module 7, you'll know exactly what it's doing.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | From paths and encodings to a reproducible data pipeline | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/05-files-and-data/01_lesson.ipynb) |
| 2 | Exercises | 12 problems with checks that run on real temporary files | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/05-files-and-data/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions with notes on the decisions that matter | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/05-files-and-data/03_solutions.ipynb) |
| 4 | Mini project | Raw CSV to a saved, reloadable penguin classifier | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/05-files-and-data/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The dataset and its license are described in [data/README.md](data/README.md).

## What you'll learn

**Paths.** `pathlib.Path`: joining with `/`, names and suffixes, creating folders, relative vs absolute paths.

**Text files.** `open` modes (and why `"w"` is dangerous), reading line by line, and why you should always pass `encoding="utf-8"`.

**Encodings.** Bytes vs characters, reading a Latin-1 file, and the invisible byte order mark that Excel adds to CSV files.

**Downloading data.** Download once, cache locally.

**CSV.** Why `split(",")` breaks, `csv.reader`, `DictReader`, `DictWriter`, `newline=""`, and converting types when every value is a string.

**Exploring data.** Row counts, class balance, missing values and per-class statistics on the penguin data.

**Cleaning messy data.** A deliberately messy file with mixed capitals, mixed units, an unquoted comma, duplicates, impossible values and two date formats, cleaned with one small function per problem and a log of every issue.

**JSON and JSON Lines.** Saving configs and results, what doesn't survive the round trip (tuples, non-string keys, `NaN`), and JSON Lines for logs and datasets.

**pickle.** Why you should never load a pickle file you don't trust.

**Putting it together.** A reproducible pipeline: load, drop, split, fill, save with a metadata file, and read everything back to check it.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `parse_value`, `count_lines`, `save_json`, `find_files` |
| Medium | `read_csv_records`, `write_csv`, `missing_report`, `fill_missing`, `read_jsonl` |
| Hard | `validate_records`, `deduplicate`, `Checkpointer` |

The checks create real temporary files: a CSV with a byte order mark, a JSON Lines file with a broken line, a folder of mixed file types, and a checkpoint folder that a second object has to find.

## Mini project

A Gaussian Naive Bayes species classifier, built from the raw CSV. It checksums the training data with SHA-256, makes a stratified split, and compares itself against a baseline and k-NN. On this split, k-NN gets every test penguin right and Naive Bayes misses two, and the notebook looks at why: both mistakes are Chinstraps with Adelie-sized bills, predicted with about 95% confidence. The model is then saved as a readable JSON file with its metadata, loaded back as if by another program, and used to predict new penguins from a CSV, including ones with missing measurements.

## Prerequisites

Modules [1](../01-python-foundations) to [4](../04-pythonic-tools)

## Next

[Module 6: NumPy](../06-numpy)
