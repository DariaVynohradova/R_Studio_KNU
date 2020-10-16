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

##### 3. Select paper titles of spotlight event type
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

##### 4. Select Josh Tenenbaum' papers
```
res2 <- dbSendQuery(conn, "SELECT Name, Title 
                                  FROM Authors as a inner join PaperAuthors as b on a.id=b.Authorid
                                  inner join Papers as c on c.id=b.Paperid  
                                  WHERE Name='Josh Tenenbaum'
                                  ORDER BY Title")
df2 <- dbFetch(res2, n=10)
df2

            Name                                                                                             Title
1 Josh Tenenbaum                                                       Deep Convolutional Inverse Graphics Network
2 Josh Tenenbaum Galileo: Perceiving Physical Object Properties by Integrating a Physics Engine with Deep Learning
3 Josh Tenenbaum                                                Softstar: Heuristic-Guided Probabilistic Inference
4 Josh Tenenbaum                                                        Unsupervised Learning by Program Synthesis

dbClearResult(res2)
```

##### 5. Select all titles that contain "statistical"
```
res3 <- dbSendQuery(conn, "SELECT Title
                                  FROM Papers 
                                  WHERE Title LIKE '%statistical%' 
                                  ORDER BY Title")
df3 <- dbFetch(res3, n=10)
df3

                                                                                 Title
1 Adaptive Primal-Dual Splitting Methods for Statistical Learning and Image Processing
2                                Evaluating the statistical significance of biclusters
3                  Fast Randomized Kernel Ridge Regression with Statistical Guarantees
4     High Dimensional EM Algorithm: Statistical Optimization and Asymptotic Normality
5                Non-convex Statistical Optimization for Sparse Tensor Graphical Model
6            Regularized EM Algorithms: A Unified Framework and Statistical Guarantees
7                            Statistical Model Criticism using Kernel Two Sample Tests
8                         Statistical Topological Data Analysis - A Kernel Perspective

dbClearResult(res3)
```

##### 6. Select auther names and number of their papers
```
res4 <- dbSendQuery(conn, "SELECT Name, count(Title) as NumPapers
                                  FROM Authors as a inner join PaperAuthors as b on a.id=b.Authorid
                                  inner join Papers as c on c.id=b.Paperid  
                                  GROUP by 1
                                  ORDER BY 2 desc")
df4 <- dbFetch(res4, n=10)
df4

                   Name NumPapers
1  Pradeep K. Ravikumar         7
2        Lawrence Carin         6
3               Han Liu         6
4     Zoubin Ghahramani         5
5               Le Song         5
6   Inderjit S. Dhillon         5
7          Zhaoran Wang         4
8         Yoshua Bengio         4
9  Simon Lacoste-Julien         4
10          Shie Mannor         4

dbClearResult(res4)

dbDisconnect(conn)
```
