# Data

## bank_marketing.csv.gz

One row for each of 41,188 phone calls made by a Portuguese bank in marketing campaigns between May 2008 and November 2010. The calls offered customers a term deposit. The file is compressed with gzip to keep it small; `pd.read_csv` opens it directly.

The rows are in time order. There's no year column, but the year can be recovered from the months, as the mini project shows.

| column | description |
|---|---|
| `age` | age in years |
| `job` | type of job (12 categories, including `unknown`) |
| `marital` | marital status: `married`, `single`, `divorced` (which includes widowed) or `unknown` |
| `education` | highest education level (8 categories, including `unknown`) |
| `default` | has credit in default? `no`, `yes` or `unknown` |
| `housing` | has a housing loan? `no`, `yes` or `unknown` |
| `loan` | has a personal loan? `no`, `yes` or `unknown` |
| `contact` | type of phone: `cellular` or `telephone` (landline) |
| `month` | month of the call |
| `day_of_week` | weekday of the call |
| `duration` | length of the call in seconds (**known only after the call; see below**) |
| `campaign` | number of calls to this customer during this campaign, including this one |
| `pdays` | days since the customer was last contacted in an earlier campaign (**999 means never**) |
| `previous` | number of contacts before this campaign |
| `poutcome` | outcome of the previous campaign: `failure`, `success` or `nonexistent` |
| `emp.var.rate` | employment variation rate, quarterly |
| `cons.price.idx` | consumer price index, monthly |
| `cons.conf.idx` | consumer confidence index, monthly |
| `euribor3m` | 3-month Euribor interest rate, daily |
| `nr.employed` | number of employees in thousands, quarterly |
| `y` | did the customer subscribe to a term deposit? `yes` or `no` (the target) |

There are no blank cells. Missing information is recorded as the category `unknown`, and `pdays` uses 999 for customers who were never contacted before.

**Important.** `duration` is only known after a call has ended, and a call of length 0 always means `no`. Using it to decide whom to call is target leakage. The dataset's authors recommend leaving it out of any realistic predictive model, and the mini project shows why.

**Source.** Moro, S., Rita, P., and Cortez, P. (2014). Bank Marketing. UCI Machine Learning Repository. https://doi.org/10.24432/C5K306. Licensed under a [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0) licence.

Described in: Moro, S., Cortez, P., and Rita, P. (2014). A data-driven approach to predict the success of bank telemarketing. *Decision Support Systems*, 62, 22-31.

**This copy.** The file `bank-additional-full.csv` from the UCI repository, unchanged except that it was saved with commas instead of semicolons as separators and compressed with gzip. All 41,188 rows and 21 columns are as published.
