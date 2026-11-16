# Data

## california_housing.csv

One row for each of the 20,640 **block groups** in California in the 1990 US Census. A block group is a small neighbourhood, typically 600 to 3,000 people.

| column | description |
|---|---|
| `longitude`, `latitude` | location of the block group's centre |
| `housing_median_age` | median age of the houses, in years (**capped at 52**) |
| `total_rooms` | total number of rooms in the block group |
| `total_bedrooms` | total number of bedrooms in the block group |
| `population` | number of people living in the block group |
| `households` | number of households in the block group |
| `median_income` | median household income, in tens of thousands of 1990 US dollars |
| `median_house_value` | median value of owner-occupied homes, in 1990 US dollars (**capped at 500,001**) |

There are no missing values. The rooms, bedrooms, population and household columns are totals for the whole block group, not per house. The two capped columns are discussed in the mini project.

**Source.** R. Kelley Pace and Ronald Barry, "Sparse Spatial Autoregressions", *Statistics and Probability Letters* 33 (1997), pages 291 to 297. They built it from the 1990 US Census, and it was distributed through the StatLib archive at Carnegie Mellon University. StatLib asks users to acknowledge both StatLib and the original contributors.

**This copy.** Taken from dataset 537 ("houses") of the Penn Machine Learning Benchmarks collection, which matches the original StatLib file. The columns were renamed and reordered, and latitude and longitude were rounded back to the two decimal places of the original (the benchmark file stores them with small floating-point artefacts, such as 37.880001). No rows or values were otherwise changed. The same data is listed on OpenML (dataset 537) as publicly available, and scikit-learn can download it with `sklearn.datasets.fetch_california_housing`.
