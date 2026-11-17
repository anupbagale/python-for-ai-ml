# Data

## wholesale_customers.csv

Annual spending of 440 business customers of a wholesale distributor in Portugal, on six kinds of product. Amounts are in "monetary units" (m.u.); the source doesn't name the currency.

| column | description |
|---|---|
| `Channel` | type of customer: `1` = Horeca (hotel, restaurant or café), `2` = retail |
| `Region` | `1` = Lisbon, `2` = Oporto, `3` = other |
| `Fresh` | annual spending on fresh products |
| `Milk` | annual spending on milk products |
| `Grocery` | annual spending on grocery products |
| `Frozen` | annual spending on frozen products |
| `Detergents_Paper` | annual spending on detergents and paper products |
| `Delicassen` | annual spending on delicatessen products (spelled this way in the original file; the mini project renames it `Delicatessen`) |

There are no missing values. Channel: 298 Horeca, 142 retail. Region: 77 Lisbon, 47 Oporto, 316 other.

**Source.** Cardoso, M. (2013). Wholesale customers. UCI Machine Learning Repository. https://doi.org/10.24432/C5030X. Licensed under a [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0) licence.

**This copy.** The file `Wholesale customers data.csv` from the UCI repository, unchanged. All 440 rows and 8 columns match the published summary statistics and frequencies.
