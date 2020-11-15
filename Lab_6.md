##### 1. Read txt file
```
df <- read.table("C:/rlabwd/household_power_consumption.txt", header=TRUE, sep=";", na.strings = "?")
```
##### Create a subset for period from 2007-02-01 to 2007-02-02
```
sub_df <- subset(df, Date=="1/2/2007" | Date =="2/2/2007")
```
##### Convert "Date" and "Time" to Date/Time class
```
sub_df$Date<-as.Date(sub_df$Date, format = "%d/%m/%Y")
sub_df$DateTime<-strptime(paste(sub_df$Date,sub_df$Time),"%F %T")
```

##### Graph 1 - histogram
###### main - an overall title for the plot
###### xlab - a title for the x axis
```
png("plot1.png", width=480, height=480)
hist(sub_df$Global_active_power, col="red", main="Global Active Power", 
     xlab="Global Active Power (kilowatts)")
dev.off()
```

##### Graph 2 - time series
###### type - 1-character string giving the type of plot desired, "l" is line
```
png("plot2.png", width=480, height=480)
plot(sub_df$DateTime,sub_df$Global_active_power, ylab="Global Active Power (kilowatts)", 
     xlab="", type="l")
dev.off()
```

##### Graph 3
```
png("plot3.png", width=480, height=480)
plot(sub_df$DateTime,sub_df$Sub_metering_1,ylab="Energy sub metering", xlab="", type="l", col="black")
points(sub_df$DateTime,sub_df$Sub_metering_2, col="red", type="l")
points(sub_df$DateTime,sub_df$Sub_metering_3, col="blue", type="l")
legend("topright", lwd=1, col=c("black", "red", "blue"), legend=names(subdt[,7:9]))
dev.off()
```

##### Graph 4
```
png("plot4.png", width=480, height=480)
par(mfcol=c(2,2))
```
##### Plot 4.1
```
plot(sub_df$DateTime, sub_df$Global_active_power, ylab="Global Active Power (kilowatts)", 
     xlab="", pch =".", type="l")
```
##### Plot 4.2
```
plot(sub_df$DateTime,sub_df$Sub_metering_1,ylab="Energy sub metering", xlab="", type="l", col="black")
points(sub_df$DateTime,sub_df$Sub_metering_2, col="red", type="l")
points(sub_df$DateTime,sub_df$Sub_metering_3, col="blue", type="l")
legend("topright", lwd=1, col=c("black", "red", "blue"), legend=names(sub_df[,7:9]), bty="n")
```
##### Plot 4.3
```
plot(sub_df$DateTime, sub_df$Voltage, ylab="Voltage", xlab="datetime", type="l")
```
##### Plot 4.4
```
plot(sub_df$DateTime, sub_df$Global_reactive_power, ylab="Global_reactive_power", xlab="datetime", type="l")
dev.off()
```
