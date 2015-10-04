# Respirable Suspended Particulate matter of Indian states in 2008
Vikrant Yadav  
30 September 2015  

Air Quality with respect to Respirable Suspended Particulate Matter(RSPM) in air quality stations under National Air Quality Monitoring Programme (NAMP)in the year 2008. The dataset can be obtained from [here](https://data.gov.in/catalog/air-quality-respect-respirable-suspended-particulate-matterrspm-air-quality-stations-under#web_catalog_tabs_block_10).  

******

### Synopsis

The IARC and WHO designate airborne particulates a Group 1 carcinogen. Particulates are the deadliest form of air pollution due to their ability to penetrate deep into the lungs and blood streams unfiltered, causing permanent DNA mutations, heart attacks, and premature death.[4] In 2013, a study involving 312,944 people in nine European countries revealed that there was no safe level of particulates and that for every increase of 10 ??g/m3 in PM10, the lung cancer rate rose 22%. The smaller PM2.5 were particularly deadly, with a 36% increase in lung cancer per 10 ??g/m3 as it can penetrate deeper into the lungs.^[1](https://en.wikipedia.org/wiki/Particulates)

The ambient air quality monitoring network comprises of 342 monitoring stations covering 128 cities/towns of the country. The data is sourced from National ambient air quality status & trends in India-2008(NAMP). NA- Data not available/outlier/not classified(Less than 50 Monitoring days) ; n- number of days monitored for 16 and more hours a day; Low, Moderate, High and Critical levels of pollution based on exceedence factor (calculated for n >50 days), percentage violation of NAAQS (24 hourly average).  
Here we make a descriptive analysis of the air quality of all the states as per the type of pollutants.  

```r
library(ggplot2)
library(dplyr)
```

```
## Warning: package 'dplyr' was built under R version 3.2.2
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

******

### Data Processing

The dataset is grouped as per the State and Type of emission and later the PM values are averaged out.  
The *Air_Quality_RSPM_2008.csv* can be downloaded from [here](https://data.gov.in/sites/default/files/datafile/Air_Quality_RSPM_2008.csv).

```r
air.quality <- read.csv('https://data.gov.in/sites/default/files/datafile/Air_Quality_RSPM_2008.csv')
colnames(air.quality)[5:7] <- c('Confidence', 'RSPM', 'Percentage')
air.quality <- group_by(air.quality, State, Type)
air.quality <- summarise(air.quality, RSPM = mean(RSPM))
ggplot(air.quality, aes(x = State, y = RSPM)) + geom_bar(stat = 'identity', aes(fill = Type)) +
  geom_hline(yintercept = 20, color = 'orange', size = 2) +
  scale_y_continuous(breaks = c(0, 20, 200, 400)) +
  labs(title = 'Respiraable Suspended PM emission of Indian states in 2008', xlab = 'States', ylab = 'Particulate matter (??g/m^3)') + coord_flip()
```

![](Air_quality_files/figure-html/unnamed-chunk-2-1.png) 

******

### Result

Uttar Pradesh has the highest cumulative PM(particulate matter) emission in the country, while Haryana has the highest Industrial PM emission followed by Delhi. The recommeded PM10 levels in atmosphere is given to by 20 ??g/m^3 as per [WHO](http://www.who.int/mediacentre/factsheets/fs313/en/). We can see that none of the states have comply with the *WHO Air Quality Giudelines* value for  Pm^10 levels.

********

### References

1. [Open data Gov, India](https://data.gov.in/catalog/air-quality-respect-respirable-suspended-particulate-matterrspm-air-quality-stations-under#web_catalog_tabs_block_10)
2. [WHO](http://www.who.int/mediacentre/factsheets/fs313/en/)??g/m^3
