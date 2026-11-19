# Data

## adult.csv.gz (lesson)

One row for each of 32,561 adults from the 1994 United States Census Bureau database, with the task of predicting whether a person earned more than $50,000 a year. It's often called the **Adult** or **Census Income** dataset, and it's one of the most studied datasets in fairness research. The file is compressed with gzip; `pd.read_csv` opens it directly.

| column | description |
|---|---|
| `age` | age in years |
| `workclass` | type of employer, such as `Private`, `Self-emp-not-inc` or `State-gov` |
| `fnlwgt` | census sampling weight: roughly how many people in the population this record stands for (not used in the lesson) |
| `education` | highest education level, as text |
| `education_num` | the same, as a number from 1 (preschool) to 16 (doctorate) |
| `marital_status` | marital status, such as `Married-civ-spouse` or `Never-married` |
| `occupation` | type of job, such as `Exec-managerial` or `Craft-repair` |
| `relationship` | role in the household: `Husband`, `Wife`, `Own-child`, `Not-in-family`, `Unmarried` or `Other-relative` |
| `race` | race, in the census's five categories |
| `sex` | sex, recorded as `Male` or `Female` |
| `capital_gain` | capital gains in the year, in dollars |
| `capital_loss` | capital losses in the year, in dollars |
| `hours_per_week` | usual hours worked per week |
| `native_country` | country of birth |
| `income` | `>50K` or `<=50K` (the target) |

Missing answers, recorded as `?` in the original files, are empty cells here: 1,836 in `workclass`, 1,843 in `occupation` and 583 in `native_country`. 7,841 people (24.1%) earn over $50,000.

Keep the limits of this data in mind. It describes one country more than thirty years ago, sex and race are recorded in the census's categories of the time, and the income gap between groups it shows reflects the inequalities of that time. It's used here to **study** those problems, not to build anything that makes decisions about people.

**Source.** Becker, B. and Kohavi, R. (1996). Adult. UCI Machine Learning Repository. https://doi.org/10.24432/C5XW20. Licensed under a [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0) licence. Described in: Kohavi, R. (1996). Scaling up the accuracy of naive-Bayes classifiers: a decision-tree hybrid. *Proceedings of the Second International Conference on Knowledge Discovery and Data Mining*, 202-207.

This copy was made from the training file (`adult.data`) as distributed in the [SHAP project's repository](https://github.com/shap/shap), with column names added, spaces trimmed, `?` turned into empty cells, and the result compressed.

## Bank marketing data (final capstone)

The capstone uses `bank_marketing.csv.gz` from Module 13, described in [its data README](../../13-evaluating-classifiers/data/README.md). It's read from the neighbouring folder if you're running inside a copy of the course, and otherwise downloaded from the course's GitHub repository, so it isn't stored twice.
