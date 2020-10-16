##### 1. Install packages
```
install.packages("DBI")
install.packages("RSQLite")

library(DBI)
library(RSQLite)
```

##### 2. Create a conection with data base
```
conn <- dbConnect(RSQLite::SQLite(), "c:\\rlabwd\\database.sqlite")
```

##### 3. Select paper title of spotlight event type
```
res1 <- dbSendQuery(conn, "SELECT Title, EventType 
                                  FROM Papers 
                                  WHERE EventType='Spotlight' 
                                  ORDER BY Title")
df1 <- dbFetch(res1, n=10)
df1

                                                                                          Title EventType
1  A Tractable Approximation to Optimal Point Process Filtering: Application to Neural Encoding Spotlight
2                                    Accelerated Mirror Descent in Continuous and Discrete Time Spotlight
3                        Action-Conditional Video Prediction using Deep Networks in Atari Games Spotlight
4                                                                      Adaptive Online Learning Spotlight
5                          Asynchronous Parallel Stochastic Gradient for Nonconvex Optimization Spotlight
6                                                 Attention-Based Models for Speech Recognition Spotlight
7                                                       Automatic Variational Inference in Stan Spotlight
8                                   Backpropagation for Energy-Efficient Neuromorphic Computing Spotlight
9                       Bandit Smooth Convex Optimization: Improving the Bias-Variance Tradeoff Spotlight
10                         Biologically Inspired Dynamic Textures for Probing Motion Perception Spotlight

dbClearResult(res1)
```

