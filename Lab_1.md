### Task 1. Download and read excel file
```
url <- 'https://data.gov.ua/dataset/66b11f1a-a130-49e7-8a0b-aa146b962b1f/resource/efab0ec6-d027-4fb6-97d6-e1b6a4f822ab/download/coal_extraction_07_2020.xlsx'
destfile <- 'c:/rlabwd/coal_extraction_07_2020.xlsx'

download.file(url, destfile, mode='wb')

library(readxl)
head(read_excel('c:/rlabwd/coal_extraction_07_2020.xlsx', sheet=1, col_names=TRUE, col_types=NULL, skip=0), 6)

A tibble: 6 x 12
  month               company company_code mine  ministry_owned_~ region region_code mark  extraction_fact
  <dttm>              <chr>   <chr>        <chr>            <dbl> <chr>  <chr>       <chr>           <dbl>
1 2020-07-01 00:00:00 "ДП ш/~ 34032208     "ДП ~                1 Донец~ 1400000000  ДГ                  0
2 2020-07-01 00:00:00 "ДП \"~ 40695853     "ДП ~                1 Донец~ 1400000000  Г(Г2)           16920
3 2020-07-01 00:00:00 "ДП Ми~ 32087941     "ВП ~                1 Донец~ 1400000000  ГЖП              1000
4 2020-07-01 00:00:00 "ДП Ми~ 32087941     "ВП ~                1 Донец~ 1400000000  ГЖП             43500
5 2020-07-01 00:00:00 "ДП Ми~ 32087941     "ВП ~                1 Донец~ 1400000000  Г(Г2)             500
6 2020-07-01 00:00:00 "ДП Се~ 33426253     "ВП ~                1 Донец~ 1400000000  ДГ               1440
```

### Task 2. Download and read csv file
```
url <- 'https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv'
destfile <- 'c:/rlabwd/Fss06hid.csv'

download.file(url, destfile, mode='wb')

library(readr)
df1 <- read_csv('c:/rlabwd/Fss06hid.csv', col_names=TRUE, col_types=NULL, skip=0)
```
#### value $1000000+
```
nrow(subset(df1, df1$VAL==24))
[1] 53

length(which(df1$VAL==24))
[1] 53

sum(df1$VAL==24 & !is.na(df1$VAL))
[1] 53
```
### Task 3. Read hml file
```
library('xml2')
df2 <- read_xml('http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml')
```
##### find all of the nodes/records
```
nodes<-xml_find_first(df2, ".//row")
nodes

{xml_node}
<row>
 [1] <row _id="1" _uuid="93CACF6F-C8C2-4B87-95A8-8177806D5A6F" _position="1" _address="http://data.baltimorecity.g ...
 [2] <row _id="2" _uuid="44F06325-01EF-4430-A292-1F7F0271D902" _position="2" _address="http://data.baltimorecity.g ...
 [3] <row _id="3" _uuid="9042B410-5D83-4CD6-B573-916D54467720" _position="3" _address="http://data.baltimorecity.g ...
 [4] <row _id="4" _uuid="CDFAF444-3141-447F-B413-8EA8621F7432" _position="4" _address="http://data.baltimorecity.g ...
```
##### find the record '__id_' and the number of variables under each record
```
records<-xml_attr(xml_children(nodes), '_id')
records

 [1] "1"    "2"    "3"    "4"    "5"    "6"    "7"    "8"    "9"    "10"   "11"   "12"   "13"   "14"   "15"  
 [16] "16"   "17"   "18"   "19"   "20"   "21"   "22"   "23"   "24"   "25"   "26"   "27"   "28"   "29"   "30"  
 [31] "31"   "32"   "33"   "34"   "35"   "36"   "37"   "38"   "39"   "40"   "41"   "42"   "43"   "44"   "45"  
 [46] "46"   "47"   "48"   "49"   "50"   "51"   "52"   "53"   "54"   "55"   "56"   "57"   "58"   "59"   "60"  

columns<-xml_length(xml_children(nodes))
columns

  [1] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
 [56] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
[111] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
[166] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
```
##### find the variable names and values
```
names<-xml_name(xml_children(xml_children(nodes)))
names

 [1] "name"            "zipcode"         "neighborhood"    "councildistrict" "policedistrict"  "location_1"     
 [7] "name"            "zipcode"         "neighborhood"    "councildistrict" "policedistrict"  "location_1"     
[13] "name"            "zipcode"         "neighborhood"    "councildistrict" "policedistrict"  "location_1"     
[19] "name"            "zipcode"         "neighborhood"    "councildistrict" "policedistrict"  "location_1"   

vals<-xml_text(xml_children(xml_children(nodes)))
vals

[1] "410"                                                "21206"                                             
[3] "Frankford"                                          "2"                                                 
[5] "NORTHEASTERN"                                       ""                                                  
[7] "1919"                                               "21231" 
```
##### create dataframe
```
df<-data.frame(n=rep(records, times=columns), 
               variable=names, values=vals)

head(df)
  n        variable       values
1 1            name          410
2 1         zipcode        21206
3 1    neighborhood    Frankford
4 1 councildistrict            2
5 1  policedistrict NORTHEASTERN
6 1      location_1             
```
##### zipcode 21231?
```
nrow(subset(df, df$variable=='zipcode' & df$values=='21231'))

[1] 127
```
