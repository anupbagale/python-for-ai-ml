# Module 8: Visualization

Matplotlib and Seaborn, and the judgement to go with them: which chart answers which question, how colour should be used, and how charts mislead when it isn't. Until now the course drew charts with text characters. This module replaces them with real ones, and sets a consistent, colourblind-checked house style for the rest of the course.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | From Anscombe's quartet to report-ready figures | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/08-visualization/01_lesson.ipynb) |
| 2 | Exercises | 12 chart functions, with checks that inspect the figures you return | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/08-visualization/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with notes on what makes each chart honest and readable | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/08-visualization/03_solutions.ipynb) |
| 4 | Mini project | A one-page visual report on a penguin classifier | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/08-visualization/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**Why plot at all.** Anscombe's quartet: four datasets with the same means, variances and correlation, and completely different shapes.

**Matplotlib's structure.** Figures and axes, the object-oriented style, and multi-panel layouts.

**A house style.** Set once with `rcParams`: a recessive grid, thin marks, and colours with jobs. Categorical colours for identity, in a fixed order and checked for colour blindness. One-hue sequential scales for magnitude. Two-hue diverging scales for values around zero.

**Choosing the chart.** From the question to the form, and two rules that always hold: one y-axis, and bars start at zero.

**Distributions.** How the bin count changes a histogram's story, small multiples with shared bins and axes, and box plots.

**Relationships.** Scatter plots, **Simpson's paradox** in the real penguin data (bill length and depth are negatively related overall, but positively related within every species), and handling overplotting with `alpha` and `hexbin`.

**Comparisons.** Sorted, labelled bar charts, and how a truncated axis turns a 1% difference into what looks like a doubling.

**Time series.** Rolling averages, and indexing two measures to a common base instead of using two y-axes.

**Heatmaps.** Correlation matrices on a fixed -1 to 1 diverging scale.

**Seaborn.** Quick statistical plots, with `hue_order` and `palette` to keep colours consistent.

**Figures for reports.** Titles that state the finding, source notes, alt text, saving with `bbox_inches="tight"`, and closing figures.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `line_chart`, `histogram`, `sorted_barh`, `save_figure` |
| Medium | `scatter_by_group`, `small_multiples`, `learning_curves`, `confusion_heatmap`, `indexed_chart` |
| Hard | `correlation_heatmap`, `decision_regions`, `rolling_band` |

The checks open up the figure your function returns and test what a reader would notice: which way the bars are sorted and whether their labels moved with them, whether the axis starts at zero, whether colours stay with their groups when one is filtered out, whether there's a second y-axis, whether a correlation scale is fixed to -1 to 1, and whether the model was called once or 40,000 times to draw its decision regions.

## Mini project

A one-page diagnostic report on a penguin classifier, made of six charts:

- the data
- decision regions for k-NN (k = 1 and k = 15) and softmax regression
- a k-NN complexity curve
- a learning curve on a readable log scale
- a confusion matrix
- a confidence histogram

The report is assembled with `GridSpec` and saved with a Markdown summary and alt text generated from the real numbers.

The results are reported as they came out. All three models score exactly the same on validation, despite drawing very different boundaries, which shows why accuracy alone isn't enough. k = 1 wins the k-NN comparison by a single penguin. The final model reaches 98.6% test accuracy, and its one mistake was made at only 57% confidence.

## Prerequisites

Modules [1](../01-python-foundations) to [7](../07-pandas)

## Next

[Module 9: Math in Code](../09-math-in-code)
