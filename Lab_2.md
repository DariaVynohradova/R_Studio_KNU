#### Task 1. Install packages
```
install.packages("BiocManager")
BiocManager::install("rhdf5")

library("rhdf5")
```
#### Task 2-3. Download file to the work directory and output its content
```
setwd("c:/rlabwd")

h5ls("H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5")

                 group            name       otype  dclass      dim
0                    /            meta   H5I_GROUP                 
1                /meta     Description H5I_DATASET  STRING    ( 0 )
2                /meta  DescriptionURL H5I_DATASET  STRING    ( 0 )
3                /meta        Detector H5I_DATASET  STRING    ( 0 )
4                /meta        Duration H5I_DATASET INTEGER    ( 0 )
5                /meta        GPSstart H5I_DATASET INTEGER    ( 0 )
6                /meta     Observatory H5I_DATASET  STRING    ( 0 )
7                /meta            Type H5I_DATASET  STRING    ( 0 )
8                /meta        UTCstart H5I_DATASET  STRING    ( 0 )
9                    /         quality   H5I_GROUP                 
10            /quality          detail   H5I_GROUP                 
11            /quality      injections   H5I_GROUP                 
12 /quality/injections InjDescriptions H5I_DATASET  STRING        5
13 /quality/injections   InjShortnames H5I_DATASET  STRING        5
14 /quality/injections         Injmask H5I_DATASET INTEGER     4096
15            /quality          simple   H5I_GROUP                 
16     /quality/simple  DQDescriptions H5I_DATASET  STRING        7
17     /quality/simple    DQShortnames H5I_DATASET  STRING        7
18     /quality/simple          DQmask H5I_DATASET INTEGER     4096
19                   /          strain   H5I_GROUP                 
20             /strain          Strain H5I_DATASET   FLOAT 16777216
```
#### Task 4. Read name Strain from group strain into variable strain
```
strain <- h5read("H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5", "strain/Strain")
strain

   [1] -2.391646e-18 -2.411660e-18 -2.427382e-18 -2.426351e-18 -2.427996e-18 -2.446291e-18 -2.462962e-18
   [8] -2.463456e-18 -2.464039e-18 -2.481933e-18 -2.499762e-18 -2.499653e-18 -2.499740e-18 -2.514827e-18
  [15] -2.534504e-18 -2.539017e-18 -2.534552e-18 -2.547276e-18 -2.567941e-18 -2.573345e-18 -2.569082e-18
  [22] -2.580038e-18 -2.601175e-18 -2.606781e-18 -2.602693e-18 -2.610871e-18 -2.633490e-18 -2.643470e-18
  [29] -2.636836e-18 -2.644074e-18 -2.664256e-18 -2.672044e-18 -2.668297e-18 -2.674602e-18 -2.694475e-18
  [36] -2.704304e-18 -2.701060e-18 -2.704327e-18 -2.721023e-18 -2.736411e-18 -2.734468e-18 -2.734657e-18
  [43] -2.750277e-18 -2.764038e-18 -2.762714e-18 -2.760657e-18 -2.775542e-18 -2.791775e-18 -2.792872e-18
  [50] -2.789694e-18 -2.802051e-18 -2.818514e-18 -2.819640e-18 -2.817563e-18 -2.825702e-18 -2.840493e-18

H5close()
```
#### Task 5. Read attributes from "strain/Strain" for Xspacing in variable st
```
st <- h5readAttributes(file="H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5", name="strain/Strain")$Xspacing
st

[1] 0.0002441406
```
#### Task 6. Find start date of the event and its duration
```
gpsStart <- h5read("H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5", "meta/GPSstart")
gpsStart

[1] 1187006834
'''
duration <- h5read("H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5", "meta/Duration")
duration

[1] 4096
```
#### Task 7. Find end date of the event
```
gpsEnd <- gpsStart + duration
gpsEnd

[1] 1187010930
```
#### Task 8. Create a vector myTime with time values
```
myTime <- seq(gpsStart, gpsEnd, st)
myTime

   [1] 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834
   [9] 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834
  [17] 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834
  [25] 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834
  [33] 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834
  [41] 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834
  [49] 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834 1187006834
```
#### Task 9. Build the graph for the first million of measures
```
numSamples <- 1000000
```
#### Task 10. Use a function for the graph
```
plot(myTime[0:numSamples], strain[0:numSamples], type = "l", xlab = "GPS Time (s)", ylab = "H1 Strain")
```
![Plot](R_Studio_KNU/image_l2.png)


