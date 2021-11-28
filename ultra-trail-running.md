---
title: "ultra-trail-running"
output:
  html_document: default
  pdf_document: default
---



```{r message=FALSE, warning=FALSE, error=FALSE}```

## R Markdown

#Reading in the data

```r
ultra_rankings <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-10-26/ultra_rankings.csv')
```

```
## Rows: 137803 Columns: 8
```

```
## ── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): runner, time, gender, nationality
## dbl (4): race_year_id, rank, age, time_in_seconds
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
race <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-10-26/race.csv')
```

```
## Rows: 1207 Columns: 13
```

```
## ── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (5): event, race, city, country, participation
## dbl  (6): race_year_id, distance, elevation_gain, elevation_loss, aid_stations, participants
## date (1): date
## time (1): start_time
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

#Prepping the data 

#Data Exploration

```r
glimpse(ultra_rankings)
```

```
## Rows: 137,803
## Columns: 8
## $ race_year_id    [3m[38;5;246m<dbl>[39m[23m 68140, 68140, 68140, 68140, 68140, 68140, 68140, 68140, 68140, 68140, 68140, 68140, 68140, 68140…
## $ rank            [3m[38;5;246m<dbl>[39m[23m 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, 1, 2,…
## $ runner          [3m[38;5;246m<chr>[39m[23m "VERHEUL Jasper", "MOULDING JON", "RICHARDSON Phill", "DYSON Fiona", "FRONTERAS Karen", "THOMAS …
## $ time            [3m[38;5;246m<chr>[39m[23m "26H 35M 25S", "27H 0M 29S", "28H 49M 7S", "30H 53M 37S", "32H 46M 21S", "32H 46M 40S", "33H 30M…
## $ age             [3m[38;5;246m<dbl>[39m[23m 30, 43, 38, 55, 48, 31, 55, 40, 47, 29, 48, 47, 52, 49, 41, 40, 35, 47, 42, 35, 56, 55, 49, 43, …
## $ gender          [3m[38;5;246m<chr>[39m[23m "M", "M", "M", "W", "W", "M", "W", "W", "M", "M", "M", "M", "W", "W", "M", "W", "M", "M", "M", "…
## $ nationality     [3m[38;5;246m<chr>[39m[23m "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR"…
## $ time_in_seconds [3m[38;5;246m<dbl>[39m[23m 95725, 97229, 103747, 111217, 117981, 118000, 120601, 120803, 125656, 125979, 125984, 127192, 12…
```



```r
glimpse(race)
```

```
## Rows: 1,207
## Columns: 13
## $ race_year_id   [3m[38;5;246m<dbl>[39m[23m 68140, 72496, 69855, 67856, 70469, 66887, 67851, 68241, 70241, 69945, 67218, 65713, 66425, 66003,…
## $ event          [3m[38;5;246m<chr>[39m[23m "Peak District Ultras", "UTMB®", "Grand Raid des Pyrénées", "Persenk Ultra", "Runfire Salt Lake U…
## $ race           [3m[38;5;246m<chr>[39m[23m "Millstone 100", "UTMB®", "Ultra Tour 160", "PERSENK ULTRA", "100 Mile", "160KM", "Salomon Rondan…
## $ city           [3m[38;5;246m<chr>[39m[23m "Castleton", "Chamonix", "vielle-Aure", "Asenovgrad", "ulukisla", "Münster", "Folldal", "Spa", "B…
## $ country        [3m[38;5;246m<chr>[39m[23m "United Kingdom", "France", "France", "Bulgaria", "Turkey", "Switzerland", "Norway", "Belgium", "…
## $ date           [3m[38;5;246m<date>[39m[23m 2021-09-03, 2021-08-27, 2021-08-20, 2021-08-20, 2021-08-20, 2021-08-15, 2021-08-14, 2021-08-14, …
## $ start_time     [3m[38;5;246m<time>[39m[23m 19:00:00, 17:00:00, 05:00:00, 18:00:00, 18:00:00, 17:00:00, 07:00:00, 07:00:00, 22:00:00, 10:00:…
## $ participation  [3m[38;5;246m<chr>[39m[23m "solo", "Solo", "solo", "solo", "solo", "solo", "solo", "solo", "solo", "solo", "solo", "Solo", "…
## $ distance       [3m[38;5;246m<dbl>[39m[23m 166.9, 170.7, 167.0, 164.0, 159.9, 159.9, 163.8, 163.9, 158.9, 163.7, 165.2, 164.8, 149.4, 173.9,…
## $ elevation_gain [3m[38;5;246m<dbl>[39m[23m 4520, 9930, 9980, 7490, 100, 9850, 5460, 4630, 6410, 3100, 3130, 7050, 2930, 9980, 9010, 2700, 15…
## $ elevation_loss [3m[38;5;246m<dbl>[39m[23m -4520, -9930, -9980, -7500, -100, -9850, -5460, -4660, -6240, -3100, -3170, -7050, -3040, -9980, …
## $ aid_stations   [3m[38;5;246m<dbl>[39m[23m 10, 11, 13, 13, 12, 15, 5, 8, 13, 23, 13, 5, 12, 15, 0, 14, 3, 17, 3, 7, 17, 12, 7, 19, 13, 8, 15…
## $ participants   [3m[38;5;246m<dbl>[39m[23m 150, 2300, 600, 150, 0, 300, 0, 200, 120, 100, 300, 50, 100, 170, 120, 250, 75, 300, 50, 300, 0, …
```




