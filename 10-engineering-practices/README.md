# Module 10: Engineering Practices

The habits that keep ML code working after the notebook is closed: project layout, environments and dependencies, testing with pytest, logging, validation, configuration, command-line scripts, reproducibility and profiling. Every idea is shown by writing real files and running them, the way you would on your own machine.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | A small package with tests, logging, configs, a CLI script, and a profiled 60x speed-up | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/10-engineering-practices/01_lesson.ipynb) |
| 2 | Exercises | 12 building blocks of real projects, including one where you write the tests | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/10-engineering-practices/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the design choices and the failure each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/10-engineering-practices/03_solutions.ipynb) |
| 4 | Mini project | Turning Module 9's penguin classifier into a tested, configurable package with a CLI | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/10-engineering-practices/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**From notebook to project.** Why working code moves out of notebooks into a package, and how to import it back, with `%autoreload` while you edit.

**Environments and dependencies.** Virtual environments, `%pip` instead of `!pip`, `sys.executable`, and the difference between dependency ranges and exact pinned versions.

**Project layout.** A package, a `tests/` folder, configs, `pyproject.toml`, and what stays out of git.

**Testing with pytest.** Plain `assert`, `pytest.approx`, `pytest.raises`, fixtures and `parametrize`. Watching a test catch a real bug (`zip` silently ignoring a length mismatch), then fixing it.

**Logging.** Levels, handlers and formatters, a log file next to each run, and avoiding duplicate handlers in notebooks.

**Errors and validation.** Failing early with messages that name the problem, custom exception classes, and why `assert` isn't for input checks.

**Configuration and CLIs.** Frozen dataclass configs that reject unknown keys, and `argparse` scripts written so tests can call them.

**Reproducibility.** Why `np.random.seed` isn't enough, passing a `Generator` instead, and saving versions and a data fingerprint with every result.

**Profiling.** `cProfile`, `perf_counter`, `%timeit` and `tracemalloc`: finding the slow part, making it about 60 times faster, checking it still gives the same answer, and the memory it costs.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `parse_requirements`, `make_config`, `setup_logger`, `file_sha256` |
| Medium | `Timer`, `retry`, `split_indices`, `test_standardize`, `predict_in_batches` |
| Hard | `pairwise_distances`, `LRUCache`, `save_run` |

In `test_standardize`, you write the tests. They're run against a correct function, which they must pass, and against five subtly broken versions, which they must catch. The other checks cover the failures that matter in practice, for example:

- a config typo silently ignored
- a logger that prints every message three times
- a "seeded" split that changes everyone else's random numbers
- a distance function that needs 150 MB for a 2 MB answer
- a results file left half-written when saving fails

## Mini project

Module 9's notebook classifier becomes a project with a package, a config, a 28-test suite, a `train`/`predict` command-line interface, and a run folder per experiment holding the model, metrics, data fingerprint, library versions and log.

The tests catch two real problems along the way:

- **A saved model without its preprocessing.** It loads fine but can't predict.
- **A test that passed or failed depending on test order.** The CLI changed global logging settings, which affected later tests. It showed up only on an older pytest version.

A 20-split experiment then revisits the question Module 9 couldn't settle. The test accuracy moves by about 10 points depending on the split alone. Adding species gives a small gain of about one penguin per test set: it helped on 11 splits, tied on 6 and hurt on 3.

## Prerequisites

Modules [1](../01-python-foundations) to [9](../09-math-in-code). Modules 3 (classes) and 4 (decorators, context managers) are used most.

## Next

[Module 11: Capstone](../11-capstone)
