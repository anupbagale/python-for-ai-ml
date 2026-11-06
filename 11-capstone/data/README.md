# Data

## penguins_raw.csv

The original field records behind the penguin dataset used throughout the course: 344 adult penguins of three species (Adelie, Chinstrap and Gentoo), sampled on three islands in the Palmer Archipelago, Antarctica, over three breeding seasons (2007/08, 2008/09 and 2009/10).

This is the **raw** version, with the researchers' original column names and values. The clean `penguins.csv` in Module 5 was derived from it. The capstone does that cleaning itself.

| column | description |
|---|---|
| `studyName` | sampling season: `PAL0708`, `PAL0809` or `PAL0910` |
| `Sample Number` | sequence number within the season |
| `Species` | full species name, e.g. `Adelie Penguin (Pygoscelis adeliae)` |
| `Region` | always `Anvers` |
| `Island` | Biscoe, Dream or Torgersen |
| `Stage` | always `Adult, 1 Egg Stage` |
| `Individual ID` | nest and adult position (e.g. `N1A1`); reused across seasons, so not a unique bird ID |
| `Clutch Completion` | whether the nest was seen with a full clutch of two eggs |
| `Date Egg` | date the first egg was observed |
| `Culmen Length (mm)` | bill length in millimetres |
| `Culmen Depth (mm)` | bill depth in millimetres |
| `Flipper Length (mm)` | flipper length in millimetres |
| `Body Mass (g)` | body mass in grams |
| `Sex` | `MALE` or `FEMALE`, from molecular (DNA) sexing of a blood sample |
| `Delta 15 N (o/oo)` | nitrogen stable isotope ratio of the blood, in parts per thousand |
| `Delta 13 C (o/oo)` | carbon stable isotope ratio of the blood, in parts per thousand |
| `Comments` | notes, e.g. failed sexing tests or missing blood samples |

Missing values are written as `NA`.

**Source.** The data was collected and made available by Dr. Kristen Gorman and the Palmer Station Long Term Ecological Research (LTER) Program, part of the US Long Term Ecological Research Network. It was packaged as the `palmerpenguins` dataset by Allison Horst, Alison Hill and Kristen Gorman: <https://allisonhorst.github.io/palmerpenguins/>

**License.** CC0 1.0 (public domain). This file is an unmodified copy of `inst/extdata/penguins_raw.csv` from the `palmerpenguins` repository.

**Citations.**

- Horst AM, Hill AP, Gorman KB (2020). palmerpenguins: Palmer Archipelago (Antarctica) penguin data. doi:10.5281/zenodo.3960218
- Gorman KB, Williams TD, Fraser WR (2014). Ecological sexual dimorphism and environmental variability within a community of Antarctic penguins (genus *Pygoscelis*). PLoS ONE 9(3): e90081. doi:10.1371/journal.pone.0090081
