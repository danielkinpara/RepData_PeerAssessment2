# Reproducible Research: Peer Assessment 2

#Major Weather Events and its Impacts in the United States, 1955-2011

Author: Daniel Kinpara  
Date: April, 2015  


### 1. Synopsis

The objective of this study is to determine the five most harmful types of weather events in the United States among the 48 listed in the Storm Data Event Table listed by the National Weather Service (NWS). The criteria used to rank the harmfulness is the injuries and fatalities caused to the population and the economic damages due to weather events. The study will comprehend the period of 1955 to 2011. It will use the National Oceanic & Atmospheric Administration's (NOAA) Storm Data database. For more details of how injuries and fatalities are accounted and economic damages calculated, see the document **Storm Data Preparation** available at [this website](http://www.nws.noaa.gov/directives/).

---

### 2. Data processing

#### 2.1 Loading dataset

The dataset is available in a compressed format (*bz2* extension) from [this link](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2). The file size is 47MB, what consumes time and memory resources to be completed load. In order to speed up the loading process and to use less resources, only ten variables were loaded, as listed below:

1. **BGN-DATE** (character): date and time of the event occurrence ;
2. **STATE** (factor): US state where the event took place;
3. **EVTYPE** (factor): 985 detailed event type;
4. **FATALITIES** (numeric): number of death, direct or indirect related to the event;
5. **INJURIES** (numeric): number of injuried people;
6. **PROPDMG** (numeric): mantissa of the number of damages to properties;
7. **PROPDMGEXP** (character): exponent of the previous number, coded as:
    - "K", thousand US dollars;
    - "M", million US dollars;
    - "B", billion US dollars;
8. **CROPDMG** (numeric): mantissa of the crop damage;
9. **CROPDMGEXP** (character): exponent of the previous number, same coding of PROPDMGEXP;
10. **REFNUM** (numeric): unique index number.

The following code loads the dataset into **dados**, skipping unused columns. Missing data was assigned *NA*.


```r
dados <- read.csv(bzfile("repdata-data-StormData.csv.bz2"),
                  header = TRUE, na.strings = "NA",
                  colClasses = c("NULL", "character", rep("NULL", 4), "factor",
                                 "factor", rep("NULL", 4), rep("NULL", 10),
                                 "numeric","numeric", "numeric", "character",
                                 "numeric", "character",rep("NULL", 8),
                                 "numeric"))
```
  
The dataset structure is:


```r
str(dados)
```

```
## 'data.frame':	902297 obs. of  10 variables:
##  $ BGN_DATE  : chr  "4/18/1950 0:00:00" "4/18/1950 0:00:00" "2/20/1951 0:00:00" "6/8/1951 0:00:00" ...
##  $ STATE     : Factor w/ 72 levels "AK","AL","AM",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ EVTYPE    : Factor w/ 985 levels "   HIGH SURF ADVISORY",..: 834 834 834 834 834 834 834 834 834 834 ...
##  $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
##  $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
##  $ PROPDMG   : num  25 2.5 25 2.5 2.5 2.5 2.5 2.5 25 25 ...
##  $ PROPDMGEXP: chr  "K" "K" "K" "K" ...
##  $ CROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ CROPDMGEXP: chr  "" "" "" "" ...
##  $ REFNUM    : num  1 2 3 4 5 6 7 8 9 10 ...
```

#### 2.2 Pre-processing data

Some pre-processing was necessary in order to reduce the number of variables and to allow numerical calculation. Date was extracted from **BGN-DATE** and stored into the variable **tempo**, a class date variable. **PROPDMG** and **PROPDMGEXP** were combined into the numeric variable **prejuizo**. **CROPDMG** and **CROPDMGEXP** were combined into the numeric variable **campo**. Both were set to the same scale of thousand of dollars.

  

```r
dados <- transform(dados, tempo = as.Date(dados$BGN_DATE, "%m/%d/%Y"),
                   prejuizo = (dados$PROPDMG * 1e6 * (dados$PROPDMGEXP == "B") +
                               dados$PROPDMG * 1e3 * (dados$PROPDMGEXP == "M") +
                               dados$PROPDMG * (dados$PROPDMGEXP == "K") +
                               dados$PROPDMG / 1e3 * (dados$PROPDMGEXP == "")),
                   campo = (dados$CROPDMG * 1e6 * (dados$CROPDMGEXP == "B") +
                               dados$CROPDMG * 1e3 * (dados$CROPDMGEXP == "M") +
                               dados$CROPDMG * (dados$CROPDMGEXP == "K") +
                               dados$CROPDMG / 1e3 * (dados$CROPDMGEXP == "")))
dados <- dados[, c(2:5,11:13)]
```
  
The new dataset structure is:


```r
str(dados)
```

```
## 'data.frame':	902297 obs. of  7 variables:
##  $ STATE     : Factor w/ 72 levels "AK","AL","AM",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ EVTYPE    : Factor w/ 985 levels "   HIGH SURF ADVISORY",..: 834 834 834 834 834 834 834 834 834 834 ...
##  $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
##  $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
##  $ tempo     : Date, format: "1950-04-18" "1950-04-18" ...
##  $ prejuizo  : num  25 2.5 25 2.5 2.5 2.5 2.5 2.5 25 25 ...
##  $ campo     : num  0 0 0 0 0 0 0 0 0 0 ...
```
  
### 3. Results


### 4. Final considerations

