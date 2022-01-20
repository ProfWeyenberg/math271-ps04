---
title: "test solution"
author: "Math 271"
date: "1/20/2022"
output: 
  html_document: 
    keep_md: yes
---



Get in the habit of loading all packages in a setup chunk at the top of the file. Add change the chunk options area to have `include=FALSE, warning=FALSE, message=FALSE` to hide all the loading chatter.

## Daily Solar Irradiance for Hawaii

Here is a published collection of data of [solar irradiance](https://figshare.com/articles/dataset/Daily_Incoming_Solar_Radiation_in_Hawaii/5579134)
for the state of Hawaii.

Below is an example of how to download a file from a URL and save it to whatever computer runs the Rmd file. The `if()` statement ensures that the file is not downloaded repeatedly. This is an example of _conditional code execution_. Copy and paste this code into your document.


```r
solar_file <- "daily_solar_radiation.txt"
if( !file.exists(solar_file) )
  download.file("https://ndownloader.figshare.com/files/9700246", solar_file)

raw_solar <- read_csv(solar_file)
```

```
## Rows: 82 Columns: 9139
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr    (4): Station.Name, Sta..ID, Island, Network
## dbl (9134): SKN, LON, LAT, Elev., X1990.01.01, X1990.01.02, X1990.01.03, X19...
## lgl    (1): X1990.05.10
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
solar <- raw_solar %>% pivot_longer(starts_with("X"), names_to = "date", names_prefix = "X", values_to = "irradiance", values_drop_na = TRUE)
solar <- solar %>% rename(name=Station.Name, id=Sta..ID, lon=LON, lat=LAT, elev=Elev.)
solar <- solar %>% mutate(date=lubridate::ymd(date), month=lubridate::month(date,T))
```
    - `Station.Name` to `name`
    - `Sta..ID` to `id`
    - `LON` to `lon`
    - `LAT` to `lat`
    - `Elev.` to `elev`
    

```r
1 + 1
```
