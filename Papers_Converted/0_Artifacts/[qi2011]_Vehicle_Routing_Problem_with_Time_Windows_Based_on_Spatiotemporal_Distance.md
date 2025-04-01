## JOURNAL OF TRANSPORTATION

## SYSTEMS ENGINEERING AND INFORMATION TECHNOLOGY

Volume 11, Issue 1, February 2011

Online English edition of the Chinese language journal

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[qi2011]_Vehicle_Routing_Problem_with_Time_Windows_Based_on_Spatiotemporal_Distance_artifacts/image_000000_610b0b774d7a90539584f1ee17b9aa5418280959bb40592a3f9a3d1a276ada37.png)

Cite this article as: J Transpn Sys Eng &amp; IT, 2011, 11(1), 85 -

89.

RESEARCH PAPER

## Vehicle Routing Problem with Time Windows Based on Spatiotemporal Distance

QI Mingyao 1, *, DING Guoxiang 2 , ZHOU You 1 , MIAO Lixin 1

- 1, Graduate School at Shenzhen, Tsinghua University, Shenzhen, 518055;
- 2, Department of Geography, The Ohio State University, Columbus, OH USA

Abstract: Vehicle Routing Problem with Time Windows (VRPTW) is a well known NP-hard problem. A typical way to solve this problem is to partition the customers into groups first and then construct optimal routes for each group or groups. Conventionally, customers are partitioned according to the spatial distance only, while ignoring their time window requirements. In this paper, we consider jointly spatial and temporal characteristics of customers and propose a spatiotemporal distance based criteria for customers partitioning.  To  improve  the  initial  solution,  a  tabu  search  algorithm  is  developed.  To  improve  the  convergence  efficiency,  this algorithm limits the objective value ranges that around the recently visited solutions, Instead of limiting recently visited solutions, the  performance  of  the  proposed  algorithm  is  examined  with  56  Solomon  benchmark  problems.  The  results  indicate  that spatiotemporal distance is more effective than spatial distance when solving vehicle routing problems with narrow time windows. Some results are even comparable to the best results obtained so far.

Key Words: logistics engineering; spatiotemporal distance; tabu search algorithm; vehicle routing problem; two-phase heuristic algorithm; generalized assignment problem

## 1    Introduction

Vehicle Routing Problem (VRP) is a well-known combinatorial  optimization  problem,  consisting  of  designing optimal delivery or collection routes from a central depot to a set  of  geographically  scattered  customers,  subject  to  various constraints, such as vehicle capacity, route length, time windows, precedence relations between customers, et al. The VRP is  NP-hard  because  it  includes  the  Traveling  Salesman Problem (TSP) as a special case when the vehicle number is one  and  the  vehicle  capacity  is  infinite [1] . During  the  past decade, it has been extensively studied in many fields, such as Operations  Research,  Management  Science,  Transportation Engineering, Logistics, et al. With the increasing stringency of service time required by customers, solving the VRP with time windows has become more important in real applications.

Basically, three kinds of VRP algorithms can be found from previous studies: accurate algorithms, classic heuristic algorithms, and meta-heuristic algorithms [1] . Accurate algorithms  can  generate  high  quality  solutions  to  problems,

* Corresponding author. E-mail: qimy@sz.tsinghua.edu.cn

but they are slow and only effective for small size problems. In contrast, heuristic algorithms can solve large size problems faster with better flexibility and adaptability. Classic heuristic algorithms mainly comprise of the saving algorithm [2] , greedy algorithm,  two-phase  algorithm,  whereas  the  meta-heuristic algorithms include the genetic algorithm, tabu search algorithm [3] , simulated annealing algorithm, ant colony algorithm and so on. Although the meta-heuristic algorithms can achieve better results than classic heuristic algorithms, it is still valuable to improve the classic heuristic algorithms for the following reasons. Firstly, classic heuristic algorithms can often provide quasi-optimal solutions quickly. Secondly, classic  heuristics  can  be  used  to  find  the  initial  solutions  for meta-heuristic algorithms, and better initial solution will improve the result in most cases. Finally, the tactics of classic heuristic algorithm, including the saving mechanism, two-phase  method,  and  greedy  searching,  can  be  used  in meta-heuristic algorithms.

Two-phase  heuristic  algorithm  is  originally  proposed  for solving  VRP  without  time  window  constraints [4] .  Later,  it  is

modified to solve VRP with time window constraints (VRPTW). The process of the two stages can be generalized as two strategies, one is partitioning the customers first, then building  and  improving  the  routes.  The  other  is  building  the routes first, and then partitioning the customers. Most researches adopt the first strategy, such as the following:

- (1) Generalized assignment problem based two-phase heuristic algorithm [4, 5] ;
- (2) K-means clustering based two-phase heuristic algorithm [6, 7] ;
- (3) Sweep based two-phase heuristic algorithm [8] .

Each  of  them  should  deal  with  the  distance  measurement between the customers. For example, during the first phase of Fisher's algorithm [4] , the distance between  the  customers needs  to  be  calculated  to  solve  a  generalized  assignment problem.  Another  example  is  the  K-means  clustering  based two-phase  heuristic  algorithm,  in  which  a  distance  matrix between the customers needs to be derived before clustering them. This study proposes a new measurement method based on the spatiotemporal distance rather than spatial distance only, which can take the spatial location and service time window into consideration simultaneously.

The  rest  of  the  study  is  organized  as  follows.  Section  2 illustrates the measurement of spatiotemporal distance; Section 3 introduces the fundamental principles of the algorithm, including seed customer choice, route construction, and  route  adjustment.  A  tabu  search  algorithm  based  on  an objective range tabu is proposed to speed up the calculation. Section  4  discusses  the  implementation  and  performance  of the proposed algorithm using Solomon's benchmark problems [9] , followed by the conclusion part in Section 5.

## 2    The spatiotemporal distance

The  VRPTW  discussed  in  this  study  can  be  defined  as follows:

Let { , } be G V E = a complete undirected graph, where {0,1,..., } V n = is the node set and {( , ), , , }  is the E i j i j V i j = ∈ ≠ edge  set. \{0} 0 V V = is  the  node  set  of  the  customers  and  0 denotes the point of station. A fleet of vehicles with the same carrying  capability Q is  set  out  for  distribution  service  for customers  from  the  station.  Each  customer  has  a  fixed demand g i , a  fixed  service  time s i , and  a  service  time window  [ , ] . Vehicles that arrive early need to wait until a i b i service time becomes feasible, whereas late vehicles will not be accepted, which is described as hard time windows. Each edge ( , )   has i j a  given  weight,  which  denotes  the  travel distance d ij with  the  same  numerical  value  as  travel  time. Thus, the optimization objective of the problem is to determine the route set with minimum values of the number of vehicles and travel distance.

The  spatiotemporal distance is required to reflect the distance between two customers in terms of both the location and  time  window  aspects.  For  instance,  when  the  spatial distance between the two customers is very close whereas the time  window  of  service  is  far  away,  assigning  them  to  the same group under the  conditions  of  hard  time  windows  will cause an increase in waiting time, and therefore will lose more opportunities to serve other customers. Therefore, the spatiotemporal distance is defined as follows:

$$D _ { i j } = w _ { _ { 1 } } \cdot d _ { i j } + w _ { 2 } \cdot T _ { i j } \quad \quad ( 1 )$$

Where d ij is the spatial distance between the two customers i and j ; T ij denotes  the  distance between  the  two  time windows; 1 w and 2 w denote the weight coefficients, where 1 2 0, 0 .  Here,  it  is  necessary  to  consider  the  scale w w ≥ ≥ between T ij and ij d . Both are need to be normalized by their corresponding  maximum  or  mean  values  before  using    the Equation (1). Time window distance ij T used in Equation (1) is defined as follows:

Let [ a b , ],  [ c d , ]  be  the  time  windows  of  customer i and j , respectively. Suppose a &lt; , and then the three situations can be c defined as follows:

1) if b &lt; c ， then, no overlap occurs

$$\frac { | a \, \left [ b \, | c \, \right ] \, | d \, \right ] } { }$$

2) if b c ≥ and b&lt;d ， then, partial overlap occurs

$$\frac { | a \, \left ( \Delta | c \, \right ) \, \left | b \, \right ) \, | d \, \right ) } { }$$

3) if b d ≥ ， then, total overlap occurs

$$\frac { | a \, \left ( \frac { | c \, \right ) } { 2 } \, | d \, \right ) } { 2 }$$

According  to  the  above  situations,  if  two  customers  have overlapped time windows, they can be served during the same time  period.  Therefore,  the  time  window  distance  between them is 0. Otherwise, it can be calculated as in Equation (2):

$$T _ { i j } = \begin{cases} c - b & \text{if $b<c$} \\ 0 & \text{if $b\geq c$} \end{cases}$$

## 3    Fundamental Principle of the Algorithms

## 3.1    Fundamental procedure

The fundamental procedure of the spatiotemporal distance based on the two-phase heuristic algorithm can be divided into the following two phases:

Phase I ： Select a number of customers as seeds, and solve a generalized  assignment  problem.  Other  non-seed  customers will be assigned to seed customers, and routes are constructed simultaneously during the process of assignment.

Phase  II:  Consider  the  results  from  Phase  I  as  the  initial solutions,  and  improve  the  results  through  a  meta-heuristic algorithm. Tabu search algorithm is adopted in this stage.

The  number  of  initial  seed  customers K is  determined  as follows:

$$K = \left [ \sum _ { i = 1 } ^ { n } g _ { i / Q } \right ] + 1 \quad \quad ( 3 ) \quad \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \$$

Where [ ] stands for the integer value.

To  select  the  seed  customers,  furthest  priority  strategy  is widely used: choose the furthest customer from the station as the first seed customer, then select the furthest customer from the collection of the station and the known seed customers as the  next  one,  and  repeat  the  process  in  sequence  until  the required number of seeds determined.

The  above  seed  selection  method  is  based  on  the  spatial distance. In this study, the spatial distance is replaced by the spatiotemporal distance formulated in Equation (1).

## 3.3    Non-seed customer assignment

The  process  of  assigning  the  non-seed  customer i to  the seed k is to solve a generalized assignment problem. The steps of implementation are described as follows:

- (1) Calculate the cost matrix ( ) K n K C × -. There  are two methods:  One  is  to  directly  consider  the  spatial  distance between  the  customers;  the  other  is  to  consider  the  added spatial  distance  after  inserting  non-seed  customers  between the  depot  and  seed  customer.  As  the  insertion  points  can  be before or after the seed, we take the minimum value of both:

$$c _ { k i } = \min \{ d _ { 0 i } + d _ { i j } + d _ { j 0 }, d _ { 0 j } + d _ { i j } + d _ { i 0 } \} - ( d _ { 0 j } + d _ { j 0 } ) \quad ( 4 ) \quad \text{more}$$

- (2)  Calculate  the  column  penalty  value  that  indicates  the difference between the second smallest price element and the minimum price element.
- (3)  Start  from  the  column  with  the  largest  column  penalty value, and  choose  the smallest price element ki c in the column. Check the vehicle capacity constraint corresponding to the line. If the value meets the constraint, then move to the next step; otherwise switch to step 6;
- (4) Insert the customer into the current route of the seed k j ， based on the best saving method that will be introduced in the next section. If succeed (that means meeting the time window constraints)  in  the  given  chances,  move  to  the  next  step; otherwise switch to step 6;
- (5) Mark ki c as the basic variable assigning customer i to the seeded customer j k , and delete the column.  If the remaining load capacity is 0 in the path of the seed customer, delete the line;
- (6) Select a minimal price element, and switch back to step 3. If the minimal price does not exist (possibly because each attempt is inconsistent with the time window or load constraints), then increase the number of vehicles, that is 1,  and  select  the  seeded  customer  again  and  switch K K = + back to step 1;
- (7)  Determine  whether  there  are  any  surplus  columns  and terminate  the  whole  process  if  not.  If  there  are  remaining columns without any remaining row, then increase the number

of vehicles 1, and select the seeded customer again and K K = + switch back to step 1.

## 3.4    Route construction

Many strategies can be adopted for route construction, such as  nearest  inserting,  saving  based  inserting,  time  window based inserting, and so on. Here we use saving based inserting method.

Suppose that the current solution is 0 i -j -,  … , -p -  0, and the length of route is r . To insert a non-seed customer into this  route,  we  have r +1  optional  locations.  If  we  insert  the customer u between , i j , the additional cost is as follows:

$$c ( i, u, j ) = d _ { i u } + d _ { u j } - d _ { i j }$$

Choose the customer with a minimal additional fee. When it does not satisfy with the time windows or loading constraint, then  choose  the  next  minimum  insertion  fee.  If  not  able  to insert the customer after predefined number of attempts, then try the next route to insert.

## 3.5    Tabu search

This study uses a tabu search algorithm [10, 11] to improve the initial solutions. Tabu strategy and neighbor structure are the  key  factors  that  determine  the  quality  of  a  TS  algorithm. Traditional  tabu  strategy  is  designed  to  memorize  and  tabu some  visited  solutions.  In  a  previous  study,  we  proposed  an adaptive  tabu  strategy  based  on  an  objective  function.  For more  details  refer  to  [3].  The  computation  procedure  is  as follows ：

Step 0 : Get the initial solution. Set cursol (current solution) = sofarbest (so far best solution) = initial solution.

- Step 1 :  If  sofarbest  has  not  been  updated  for  m  iterations, stop and output the sofarbest; else go to Step 2.
- Step  2 :  Choose  the  best  un-forbidden  solution  from  the neighbors generated through the neighbor construction methods such as the cursol. If all the neighbors are forbidden, choose the best solution as cursol and update the tabu list. If cursol is better than sofarbest, let sofarbest =cursol and go to Step 1.

## 4    Experiment results

We test our algorithm using Solomon's VRPTW instances (Solomon 1987) [9] ， which can be classified into six groups( C 1, R 1, RC 1, C 2, R 2, RC 2). For problems C 1 and C 2, customers are clustered through the spatial distance criteria. For problems R 1 and R 2, customers are randomly distributed. Half of  the  customers  in  problems RC 1  and RC 2  have  the  same distribution as in problems C 1 and C 2, and the others have the same as in problems R 1 and R 2. Due to different capacity and time windows, the number of customers a vehicle can serve is different. Problems C 1, R 1 and RC 1 can be considered having narrow  time  windows  and  small  capacity,  therefore  each vehicle  can  serve  fewer  customers  than  in  problems C 2, R 2 and CR 2.

The  best  known  solutions  of  Solomon's  VRPTW  can  be found  at  http://www.sintef.no/static/am/opti/projects/top/vrp/ bknown.html. Our algorithm is programmed using Visual C++. All results reported below are run on Intel Core 2 Duo CPU 2.4GHz, 2GB RAM and Windows XP.

As  discussed  above,  two  strategies  can  be  adopted  to choose  the  seed  customer.  One  is  only  clustered  though  the spatial distance, and the other is grouped though the spatiotemporal  distance.  They  are  denoted  as S 1  and S 2. Spatial and temporal distances are equally weighted 1 2 100 .  Also,  for  generalized  assignment  problems, w w = = the  distance  between  the  customers  can  be  calculated  in  two different ways. The first one only concerns the spatial distance between  the  customers,  whereas  the  second  one  concerns additional distance when  customer  is inserted, which  is denoted by A 1 and A 2, respectively.

For Solomon's benchmark problems, the calculation results of the first phase  algorithm  are  shown  in  Table  1.  For problems R 1, C 1, and RC 1 ， with the same strategy A ( A 1 or A 2),  strategy S 2  is  better  than S 1  because  both  the  average number of vehicles and the average cost decline significantly, which proves the effectiveness of the spatiotemporal distance. Meanwhile, with the same strategy S ( S 1 or S 2),  strategy A 2 performs  better  than A 1.  For  problems R 2, C 2  and RC 2, strategy A 2 is still better than A 1. However, strategy S 2 does not dominate S 1. It is reasonable because these problems have less  constraint in time windows. Therefore, we can conclude that  the  spatiotemporal  distance  is  better  than  the  spatial distance  when  solving  the  VRP  with  narrow  time  windows. Specifically,  best  performance  can  be  achieved  when  used with strategy A 2.

|          | R 1, C 1 and RC 1   | R 1, C 1 and RC 1   | R 2, C 2 and RC 2   | R 2, C 2 and RC 2   |
|----------|---------------------|---------------------|---------------------|---------------------|
|          | Vehicle number      | Total distance      | Vehicle number      | Total distance      |
| S 1+ A 1 | 15.41               | 1805.89             | 4                   | 1142.45             |
| S 2+ A 1 | 14.59               | 1633.35             | 4.11                | 1312.54             |
| S 1+ A 2 | 15.52               | 1762.51             | 4                   | 1117.3              |
| S 2+ A 2 | 14.14               | 1617.11             | 4                   | 1268.4              |

Table 1 The results of the first phase

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[qi2011]_Vehicle_Routing_Problem_with_Time_Windows_Based_on_Spatiotemporal_Distance_artifacts/image_000001_9771041c24a9fad2f90785e7d25068b6443246e8a87acd209be9a6f63b9b32a6.png)

TC: Total cost; NV: number of vehicles used Fig.1 The results of the first phase with different weight

In order to further verify the effectiveness of the spatiotemporal  distance,  we  run  the  algorithm  with  different weights  for  strategy S 2+ A 2.  The  results  are  shown  in  Fig.1. The 1 w is fixed at 100 and 2   varies from 0 to 100 as the w horizontal  axis  denotes.  From  Fig.1,  we  can  see  that  when 2 0 , the number of vehicles and the total distance are the w = greatest.  It  declines  significantly  when 2 w increases  until 2 30 .  There  are  no  obvious  improvements  with  more w = increase in 2 w , which means that the factor of time windows has been fully taken into account when 2 w reaches 30. Fig. 1 also proves that the spatiotemporal distance is effective.

In the second phase (tabu search), we set the length of tabu list  as  7.  For  the  tabu  interval,  if  the  last  function  value increases,  choose  one  percent  of  addition  as  the  tabu  size; otherwise choose two percent of decline as the tabu size. At the same time, the tabu size should be no more than 1 and no less than 0.05. We selected the number of optional neighbor as 10 and the number of neighbors searched randomly as 5,000. The algorithm will  stop  when  there  is  no  improvement  after 1,000 iterations. Using these parameters, we tested the problem R 1  by  tabu  search  based  on  the  results  of  three algorithms:  greedy  algorithm  ( G TS + ),  assignment  algorithm by the spatial distance criteria ( S 1+ A 2+ TS ), and the spatiotemporal distance criteria ( S 2+ A 2+ TS , 1 100   , w = 2 30 ).  Because  the  results  of  tabu  search  are  stochastic, w = we  choose  the  best  results  of  three  algorithms,  as  shown  in Table 2.

Table  2  shows  that  the  results  based  on G TS + are  worst because of their bad initial solutions (local optima). Comparing S 2+ A 2+ TS with S 1+ A 2+ TS ,  we  can  see  that  the former one is better, which means the spatiotemporal distance criteria outperform the spatial only criteria.

Table 2 The results based on different strategies

|              |   Vehicle number |   Total distance | Average processing time   |
|--------------|------------------|------------------|---------------------------|
| G + TS       |            13.58 |          1243.96 | 71.84                     |
| S 1+ A 2+ TS |            13.5  |          1223.15 | 75.87                     |
| S2 + A 2+ TS |            13.33 |          1219.46 | 83.31                     |
| S 2+ A 2     |            14.14 |          1617.11 | <1                        |

Table 3 The results comparing with the best known solutions

|                | S 2+ A 2+ TS   | S 2+ A 2+ TS   | the best known solutions   | the best known solutions   |
|----------------|----------------|----------------|----------------------------|----------------------------|
|                | Vehicle number | Total distance | Vehicle number             | Total distance             |
| C 101 to C 102 | 10             | 828.94         | 10                         | 828.94                     |
| C 103          | 10             | 828.06         | 10                         | 828.06                     |
| C 104          | 10             | 825.11         | 10                         | 824.78                     |
| C 105 to C 109 | 10             | 828.94         | 10                         | 828.94                     |
| R 101          | 19             | 1691.43        | 19                         | 1645.79                    |

Table  3  lists  the  comparison  of S 2+ A 2+ TS with  the  best known solutions. Results of C 101 to C 103 and C 105 to C 109 reach  the  best  known  solutions  whose  vehicle  number  is  10

and the total distance is 828.94. The results of C 104 and R 101 are  also  very  close  to  the  best  known  solutions,  as  shown  in Table 3.

## 5    Conclusions remarks

## 5.1    Conclusion

- (1) Significant improvement has been observed when using the spatiotemporal distance compared with the spatial distance criteria to solve the GAP.
- (2) Because the initial solutions influence the effect of tabu search  algorithm,  the  GAP  based  on  spatiotemporal  distance criteria  can  lead  to  better  VRPTW  solution  than  those  based on  greedy  algorithms,  and  those  based  on  spatial  distance criteria.

## 5.2    Future research

Although the proposed inclusion of spatiotemporal distance shows  the  potential for VRP,  there is room  for further improvement. For example, the temporal distance doesn't take into consideration the service time and travel time between the two  customers.  Other  grouping  methods,  such  as  K-means clustering  model,  hierarchical  clustering  model,  and  V oronoi diagram, can also be used in the first phase of the algorithm for better initial solution.

## Acknowledgements

This  research  was  supported  by  National  Natural  Science Foundation of China (No. 70702003) and Science and Technology  Foundation  of  Dongguan  (No.  200910810115). These supports are gratefully acknowledged.

## References

- [1]    Laporte  G.  What  you  should  know  about  the  VRP.  Naval
- Research Logistics, 2007, 54(8): 811-819.
- [2]    Peng  X,  Qi  M  Y,  Miao  L  X.  Review  and  analysis  of  saving algorithm for vehicle routing problem//China Logistics Research: State of the art (2008-2009). Beijing: China Logistics Publishing House, 2008: 399-408.
- [3]    Qi M Y, Miao L X. A new tabu search heuristic algorithm for the vehicle routing problem with time windows// Proceedings of  2008  International  Conference  on  Management  Science  &amp; Engineering 15th Annual Conference. Long Beach, USA:IEEE Press, 2008:1648-1653.
- [4]    Fisher  M  L,  Jaikumar  R.  A  generalized  assignment  heuristic for vehicle routing . Networks,1981, 11(2):109-124.
- [5]    Li J, Guo H H. Theories and methods on logistics distribution routing optimization. Beijing: China Logistics Publishing House, 2001.
- [6]    Liu W M. Research on heuristics algorithm for vehicle routing problem with time windows and limited capacity. Guangzhou: South China University of Technology, 2007.
- [7]    Wang S Y, Li J. Application of two-phase heuristic algorithm on vehicle routing problem with time windows. Market Modernization, 2007, 520: 114-115.
- [8]    Qiu  A  H.  Study  on  the  application  of  sweep  method  and genetic  algorithm  in  vehicle  routing  problem  in  logistics  and distribution service. Beijing: Beijing Jiaotong University, 2008.
- [9]    Solomon M. Algorithms for the vehicle routing and scheduling problems with time window constraints. Operations Research, 1987, 35: 254-265.
- [10] Glover F. Tabu search - part I. ORSA Journal on Computing, 1989, 1(3):190-206.
- [11] Glover F. Tabu search - part II. ORSA Journal on Computing, 1990, 2(1):4-32.