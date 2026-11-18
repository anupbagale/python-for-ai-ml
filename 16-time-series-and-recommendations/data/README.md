# Data

## bike_day.csv and bike_hour.csv.gz

Rentals from Capital Bikeshare, the bike-sharing system of Washington, D.C., over 2011 and 2012, with weather and calendar information. `bike_day.csv` has one row per day (731 rows); `bike_hour.csv.gz` has one row per hour (17,379 rows, compressed with gzip; `pd.read_csv` opens it directly).

| column | description |
|---|---|
| `instant` | record number |
| `dteday` | date |
| `season` | 1 to 4; in the data, 1 covers roughly late December to March (the original readme labels the seasons 1 = spring to 4 = winter, which doesn't match the dates) |
| `yr` | 0 = 2011, 1 = 2012 |
| `mnth` | month, 1 to 12 |
| `hr` | hour, 0 to 23 (hourly file only) |
| `holiday` | 1 if the day is a public holiday |
| `weekday` | day of the week, 0 = Sunday |
| `workingday` | 1 if neither a weekend nor a holiday |
| `weathersit` | 1 = clear or partly cloudy, 2 = mist or cloudy, 3 = light rain or snow, 4 = heavy rain, snow or storm |
| `temp`, `atemp` | temperature and "feels like" temperature, scaled to 0 to 1 |
| `hum` | humidity, scaled to 0 to 1 |
| `windspeed` | wind speed, scaled to 0 to 1 |
| `casual`, `registered` | rentals by casual and by registered users |
| `cnt` | total rentals |

**Important.** The hourly file only contains hours with at least one rental, so 165 of the 17,544 hours in the two years are missing. Most are night hours; the rest fall on days when the system shut down (Hurricane Sandy on 29 and 30 October 2012, a snowstorm in January 2011). The mini project fills them with 0.

**Source.** Fanaee-T, H. (2013). Bike Sharing. UCI Machine Learning Repository. https://doi.org/10.24432/C5W894. Licensed under a [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0) licence. Described in: Fanaee-T, H., and Gama, J. (2013). Event labeling combining ensemble detectors and background knowledge. *Progress in Artificial Intelligence*, pp. 1-15. https://doi.org/10.1007/s13748-013-0040-3. The underlying trip data is published by Capital Bikeshare.

**This copy.** `day.csv` and `hour.csv` from the UCI repository, unchanged (the hourly file is gzip-compressed).

## online_retail.csv.gz

Every invoice line of a UK-based online retailer, mostly selling gifts and homewares, from 1 December 2010 to 9 December 2011: 541,909 rows. Many of its customers are wholesalers.

| column | description |
|---|---|
| `InvoiceNo` | invoice number; a number starting with `C` is a cancellation |
| `StockCode` | product code; real products start with five digits, others (`POST`, `M`, `BANK CHARGES`, ...) are charges and adjustments |
| `Description` | product name |
| `Quantity` | quantity on this line (negative for cancellations) |
| `InvoiceDate` | date and time of the invoice |
| `UnitPrice` | price per unit, in pounds sterling |
| `CustomerID` | customer number (missing for 135,080 lines) |
| `Country` | the customer's country |

**Source.** Chen, D. (2015). Online Retail. UCI Machine Learning Repository. https://doi.org/10.24432/C5BW33. Licensed under a [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0) licence. Described in: Chen, D., Sain, S. L., and Guo, K. (2012). Data mining for the online retail industry: A case study of RFM model-based customer segmentation using data mining. *Journal of Database Marketing and Customer Strategy Management*, 19(3), 197-208.

**This copy.** The contents of the UCI file `Online Retail.xlsx`, saved as a gzip-compressed CSV, with dates written as `YYYY-MM-DD HH:MM`. It was taken from the copy in the `onlineretail` R package by Allan Quadros, which redistributes the data with the donor's permission, and checked against the UCI description: 541,909 rows, 135,080 lines without a customer ID, the same date range, and a total quantity of 5,176,450.
