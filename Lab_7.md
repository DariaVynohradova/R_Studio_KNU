```
library(dplyr)
library(tidyverse)
library(stringr)
library(ggplot2)
```

##### Read rds files
```
nei <- readRDS("C:/rlabwd/summarySCC_PM25.rds")
scc <- readRDS("C:/rlabwd/Source_Classification_Code.rds")
```

##### Graph 1 - PM2.5 Total Emissions from all the sources by year
```
nei_by_year <- nei %>% group_by(year) %>% summarise(Total=sum(Emissions)) %>% view

png("l7_p1.png", width=480, height=480)
barplot((nei_by_year$Total/10^3), names.arg=nei_by_year$year, col="blue",
        xlab="Year", ylab="PM2.5 Total Emissions, thousand tons")
dev.off()
```
![Plot](https://github.com/DariaVynohradova/R_Studio_KNU/blob/master/l7_p1.png)

##### Graph 2 - PM2.5 Total Emissions in Baltimore by year
```
nei_balti <- nei %>% group_by(year) %>%filter(fips=="24510") %>% summarise(Total=sum(Emissions)) %>% view

png("l7_p2.png", width=480, height=480)
barplot(nei_balti$Total, names.arg=nei_balti$year, col="red", xlab="Year",
        ylab="PM2.5 Total Emissions, tons", main="Total PM2.5 Emissions From All Baltimore City Sources")
dev.off()
```
![Plot](https://github.com/DariaVynohradova/R_Studio_KNU/blob/master/l7_p2.png)

##### Graph 3 - PM2.5 Total Emissions in Baltimore by year and type
```
nei_balti_type <- nei %>% group_by(year, type) %>%filter(fips=="24510") %>% summarise(Total=sum(Emissions)) %>% view

png("l7_p3.png", width=720, height=480)
ggplot(nei_balti_type, aes(factor(year), Total, fill=factor(type))) +
  geom_bar(stat="identity")  +  facet_grid(~type) + guides(fill=FALSE)+ 
  labs(x="year", y=expression("Total PM2.5 Emission (Tons)")) + 
  labs(title=expression("PM2.5 Emissions in Baltimore City 1999-2008 by Source Type"))
dev.off()
```
![Plot](https://github.com/DariaVynohradova/R_Studio_KNU/blob/master/l7_p3.png)

##### Graph 4  - Coal combustion-related sources in the USA
```
coal <- scc %>% filter(str_detect(EI.Sector, "Coal")) %>% select(SCC)
nei_coal <- nei[nei$SCC %in% coal$SCC,]
nei_coal_year <- nei_coal %>% group_by(year) %>% summarise(Total=sum(Emissions)) %>% view

png("l7_p4.png", width=480, height=480)
barplot((nei_coal_year$Total/10^3), names.arg=nei_coal_year$year, col="blue",
        xlab="Year", ylab="PM2.5 Total Emissions, thousand tons",
        main="PM2.5 Coal Combustion-Related Sources in the USA")
dev.off()
```
![Plot](https://github.com/DariaVynohradova/R_Studio_KNU/blob/master/l7_p4.png)

##### Graph 5  - Motor vehicle sources in Baltimore
```
motor <- scc %>% filter(str_detect(EI.Sector, "Comb")) %>% select(SCC, EI.Sector)
nei_motor <- nei[nei$SCC %in% motor$SCC,]
nei_motor_balti <- nei_motor %>%filter(fips=="24510") %>% group_by(year) %>% summarise(Total=sum(Emissions)) %>% view

png("l7_p5.png", width=480, height=480)
barplot((nei_motor_balti$Total), names.arg=nei_coal_year$year, col="blue",
        xlab="Year", ylab="PM2.5 Total Emissions, tons",
        main="PM2.5 Motor Vehicle Sources in Baltimore")
dev.off()
```


##### Graph 6  - Motor vehicle sources in Baltimore and Los Angeles
```
nei_motor_ba_la <- nei_motor %>%filter(fips=="24510" | fips=="06037")  %>% 
                 mutate(city=case_when(fips=="24510" ~ "Baltimore City", fips=="06037"~ "Los Angeles")) %>% 
                 group_by(year, city) %>% summarise(Emissions=sum(Emissions))

png("l7_p6.png", width=720, height=480)
ggplot(nei_motor_ba_la, aes(factor(year), Emissions, fill=factor(city))) +
  geom_bar(stat="identity")  +  facet_grid(~city) + guides(fill=FALSE)+ 
  labs(x="year", y=expression("Total PM2.5 Emission (Tons)")) + 
  labs(title=expression("Vehicle Source Emissions in Baltimore and LA, 1999-2008"))
dev.off()
```

