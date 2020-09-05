### Task 1. Download and read excel file
```
url <- 'https://data.gov.ua/dataset/66b11f1a-a130-49e7-8a0b-aa146b962b1f/resource/efab0ec6-d027-4fb6-97d6-e1b6a4f822ab/download/coal_extraction_07_2020.xlsx'
destfile <- 'c:/rlabwd/coal_extraction_07_2020.xlsx'

download.file(url, destfile, mode='wb')

library(readxl)
head(read_excel('c:/rlabwd/coal_extraction_07_2020.xlsx', sheet=1, col_names=TRUE, col_types=NULL, skip=0), 6)
```
A tibble: 6 x 12
  month               company company_code mine  ministry_owned_~ region region_code mark  extraction_fact
  <dttm>              <chr>   <chr>        <chr>            <dbl> <chr>  <chr>       <chr>           <dbl>
1 2020-07-01 00:00:00 "ДП ш/~ 34032208     "ДП ~                1 Донец~ 1400000000  ДГ                  0
2 2020-07-01 00:00:00 "ДП \"~ 40695853     "ДП ~                1 Донец~ 1400000000  Г(Г2)           16920
3 2020-07-01 00:00:00 "ДП Ми~ 32087941     "ВП ~                1 Донец~ 1400000000  ГЖП              1000
4 2020-07-01 00:00:00 "ДП Ми~ 32087941     "ВП ~                1 Донец~ 1400000000  ГЖП             43500
5 2020-07-01 00:00:00 "ДП Ми~ 32087941     "ВП ~                1 Донец~ 1400000000  Г(Г2)             500
6 2020-07-01 00:00:00 "ДП Се~ 33426253     "ВП ~                1 Донец~ 1400000000  ДГ               1440


### Task 2. 
```

```
### Task 3. 
##### Creating matrix from vector by adding a dimension attribute
```

```
