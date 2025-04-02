## A cluster first-route second approach for a capacitated vehicle routing problem: a case study

## Serap Ercan Comert*, Harun Resit Yazgan, Sena Kır and Furkan Yener

Engineering Faculty,

Department of Industrial Engineering,

Sakarya University,

Sakarya, Turkey

Email: serape@sakarya.edu.tr

Email: yazgan@sakarya.edu.tr

Email: senas@sakarya.edu.tr

Email: fyener@sakarya.edu.tr

*Corresponding author

Abstract: In  this study, a capacitated vehicle routing problem (CVRP) which dealt  with  minimum  distance  routes  for  vehicles  that  serve  customers  having specific  demands  from  a  common  warehouse  under  a  capacity  constraint. This  problem  is  NP  hard.  We  solved  the  problem  in  a  hierarchical  way (i.e., cluster-first route-second method). Firstly, customers were clustered using three different clustering algorithms; K-means, K-medoids and random clustering with considering a vehicle capacity. Secondly, routing problems for each cluster  were  solved  using  a  branch  and  bound  algorithm.  The  proposed solution strategy was employed on a case study in a supermarket chain. Results of numerical investigation were presented to illustrate the effectiveness of the algorithms using paired sample t tests. The  results illustrated that the K-medoids algorithm provided better solution than the others.

Keywords: capacitated  vehicle  routing  problem;  CVRP;  k-means  clustering algorithm; K-medoids  clustering algorithm; random  clustering algorithm; branch and bound algorithm.

Reference to this paper should be made as follows: Comert, S.E., Yazgan,  H.R.,  Kır,  S.  and  Yener,  F.  (2018)  'A  cluster  first-route  second approach  for  a  capacitated  vehicle  routing  problem:  a  case  study', Int.  J. Procurement Management , Vol. 11, No. 4, pp.399-419.

Biographical notes: Serap Ercan Comert is a PhD student at the Department of Industrial  Engineering  in  the  Sakarya  University,  Turkey.  She  is  studying supply chain management.

Harun Resit Yazgan is an Associate Professor at the Department of Industrial Engineering of the Sakarya University in Turkey. His interests are in the areas of  modelling  and  optimisation,  supply  chain  management  and  multicriteria decision making.

Sena Kır is a PhD student at the Department of Industrial Engineering in the Sakarya  University,  Turkey.  She  is  studying  mathematical  modelling  and metaheuristic algorithms on transportation science.

Furkan Yener is a PhD student at the Department of Industrial Engineering in the  Sakarya  University,  Turkey.  He  is  studying  warehouse  operations  and management on the supply chain.

## 1 Introduction

Vehicle routing problem (VRP) is to determine best routes for distribution/collection of the  vehicles  which  are  assigned  to  serve  from  one  or  multiple  warehouses  to  the geographically dispersed centres (Laporte et al., 1988). VRP is a generic version of the travelling salesman problem (TSP). In VRP order point is used instead of customers in the TSP and vehicle term is used instead of dealer. There are different VRP types based on constraints; vehicle capacity, the minimum distance a vehicle can travel at once and travel durations can be mentioned for different type of VRP constraints. In this study, we studied a capacitated VRP (CVRP).

There are particular capacity limitations for vehicles on the CVRP. CVRP is one of the  most  important  problems  of  optimisation  of  distribution  network  and  the  most common  type  of  VRP.  Also,  capacities  of  all  vehicles  can  be  assumed  as  equal (homogeny fleet) or different (heterogen fleet) in the CVRP problem and vehicles start to move from one warehouse and finish in same warehouse (one depot) or start from more than  one  depot  (multi  depot).  Demands  of  a  customer  are  delivered  at  one  time  and fragmentation  is  not  concerned.  The  aim  is  to  minimise  the  total  distance  travelled  by vehicles  (Lin  et  al.,  2009).  If  a  time  is  considered  as  a  constraint,  the  CVRP  become CVRP time windows (CVRPTW) (hard or soft).

There are different solution methods to solve the CVRP in the literature. One of the methods is to employ an exact algorithm to a MILP model of the CVRP. The well-known exact algorithms are brand and bound, branch and price, Lagrange relaxation and column generation  etc.  These  algorithms'  performances  heavily  depend  on  the  problems  size because of the NP-Hard. Another method is heuristic methods. One of the famous one is Clarke and Wright (1964) method. There are numerous heuristic methods in the literature (Akpinar,  2016;  Chen  et  al.  2010;  Jepsen  et  al.  2013;  Jin  et  al.,  2014;  Nazif  and  Lee, 2012;  Tadei  and  Perboli,  2011).  The  last  method  to  solve  the  CVRP  is  a  hierarchical method.  The  problem  is  systematically  divided  as  different  level  in  the  hierarchical method. Different methods can be employed for each level. Cluster first and route second and different version of it route first and cluster second can be given an example of the hierarchical methods. In this article, a cluster first and route second hierarchical method was employed.

Clusters consist of data division into groups of similar objects. Cluster analysis bases on  a  principle  of  maximising  similarity  of  groups  among  themselves  and  minimising between group similarities. (Han and Kamber, 2001). However, large data sets are not clustered with classic clustering techniques. Therefore, some algorithms which emphasis on scalability has been developed (Venkates et al., 1999). Ability to work with noisy data directly  affects  the  quality  of  the  clustering  techniques  (Xu  et  al.,  2005).  Clustering method varies according to data type and purpose of the study.

In  this  study,  a  hierarchical  approach  was  proposed  to  solve  CVRP.  The  approach consisted  of  two  stages  (cluster  first  and  route  second).  Firstly,  cluster  was  performed using three different algorithms (i.e., K-means, K-medoids and random) separately at first stage. Route problem was solved using an exact algorithm (B&amp;B) for each cluster at the second stage. A case study was given to illustrate effectiveness of the proposed approach.

## 2 Literature review

In  this  study,  a  hierarchical  approach  consisted  of  two  stages,  cluster  first  and  route second  was  proposed.  First,  the  cluster  problems  were  solved  with  using  K-means, K-medoids and random. Second, route problem was solved using an exact algorithm (i.e., travelling  salesmen  problem).  Therefore,  our  literature  survey  was  limited  with  cluster first  and  route  second  approach,  cluster  algorithms  (K-means, K-medoids and random) and exact algorithms.

Cluster first and route second method was suggested and employed by Bodin, (1975), Fisher and Jaikumar (1981), Hiquebran et al. (1993), Sariklis and Powell (2000), Cakir et al. (2015), Kloimüllner et al. (2015) as shown in Table 1.

Firstly, cluster algorithms are discussed. Cluster algorithms frequently encountered in the  literature  in  recent  years  are  mostly  based  on  data  mining.  A  number  of  solution methods  have  been  proposed  for  clustering  in  literature. These  are  model-based, centre-based  indexer,  hierarchical,  density  based  and  grid-based  clustering  methods. K-means,  K-medoids  and  random  cluster  algorithms  were  chosen  in  this  article.  We surveyed studied related with these cluster algorithms.

In  the  literature,  there  are  different  versions  of  k-means  algorithms,  Ng  and  Wong (2002)  proposed  a  fuzzy  K-means  algorithm.  Tsai  et  al.  (2002)  introduced  a  genetic K-means algorithm (GKA) and fast self-organising map combined k-means (FSOM+K-means). Another article was related with assigning one vehicle  to  all  cities given  in  the  study  conducted  by  Nallusamy  et  al.  (2010)  for  distribution  purposes. K-means algorithm was utilised to determine to which cities served by which vehicles (Nallusamy et al., 2010). Elango et al. (2011) used k-means to group robots and claimed that  the  cost  was  less  by  reducing  distances  by  multiple  travelling  salesman  problems (MTSP)  among  robotics.  In  the  study  prepared  by  Hsieh  and  Huang  (2011),  two clustering algorithms were introduced, namely k-means batching (KMB) and self-organisation map batching (SOMB). They claimed that less total travel distance and higher performances in terms of vehicle usefulness were achieved using these algorithms. Besides, it was reported that using the SOMB provided less CPU running time than the KMB. Niknam et al.  (2011)  introduced  a  new  algorithm  that  was  a  hybrid  of  modify imperialist competitive algorithm (MICA) and k-means called as a K-MICA. Accordingly  a  their  simulation  result,  they  claimed  that  their  proposed  algorithm provided  better  results  than  MICA  and  K-MICA  in  terms  of  best,  worst,  average solutions and standard deviation. Kargari and Sepehri (2012) developed store similarity function (OC) for the k-means algorithm. They used this new function in a case study and achieved comparable results. Ekici and Retharekar (2013) used clustering process using K-means clustering algorithm in their study. In the study conducted by Fang et al. (2013),

clustering was performed without needing to determine the number of clusters which was a  prerequisite  for  the  operations  of  the  k-means  algorithm.  The  algorithm  proposed  by Luo and Chen (2014) k-means clustering analysis is used in the process of forming the frog  population.  They  solved  the  multi-depot  vehicle  routing  problem  (MDVRP)  more quickly.  Krishnasamy et al.  (2014)  proposed  a  hybrid data  clustering  algorithm  named K-modified  cohort  intelligence  (K-MCI)  which  was  claimed  to  be  very  efficient.  The algorithm consisted of cohort intelligence with K-means and tested on several standard data set with other clustering algorithms commonly used. As a result of these simulations, solution  quality  and  speed  of  the  K-MCI  algorithm  was  claimed  that  it  was  very promising.  Reed  et  al.  (2014)  made  a  clustering  using  K-means  about  collection  of household  waste  for  CVRP.  They  reported  that  the  solution  of  this  algorithm  had  a high-quality performance. K-means and hierarchical clustering methods were compared by  Shivshankar  and  Jamalipour  (2014)  with  a  simulation  approach.  Tu  et  al.  (2014) developed a graph-based K-means (GBK) algorithm and applied to both synthetic data sets  in  real  life  example.  GBK  with  some  algorithms  such  as  classic  k-means,  kernel k-means  and  spectral  clustering  were  compared  and  it  was  stated  that  stunning  results were obtained and an algorithm to recommend potential.

K-medoids  algorithm  was  also  used  to  cluster.  Lucasius  et  al.  (1993)  suggested K-medoids algorithm that using a genetic algorithm. In addition, Zhang and Couloigner (2005)  also  proposed  a  K-medoids  algorithm.  Barioni  et  al.  (2008)  proposed  a  new algorithm  called  as  PAM-SLIM.  Metric  access  methods  were  used  in  the  proposed algorithm  that  dealt  with  scale-up  K-medoid  based  algorithms.  It  was  claimed  that experimental results illustrated better solutions. Fei et al. (2009) suggested a two phase K-medoids  algorithm  for  health  sector  and  claimed  that  the  performance  of  medical service  was  improved.  Park  and  Jun  (2009)  suggests  K-medoids  algorithm  which  runs like K-means algorithm. The algorithm was tested on some data sets. It was claimed that the  proposed  algorithm  significantly  reduced  time  than  K-means  cluster.  Lewis  et  al. (2012) presented fuzzy K-means and K-medoids algorithms for tracking epileptogenesis progressions. Qi et al. (2012) also recommended K-medoids algorithm. Peng et al. (2014) proposed a new algorithm that based on ACO and K-medoids algorithm for balancing network energy consumption. Brito et al. (2015) used K-medoids  and  CN2-SD algorithms for solving marketing and manufacturing problems.

In the literature, there are studies that reported comparison among cluster algorithms. Harikumar and PV  (2015) proposed K-medoids  algorithm and then applied on heterogeneous  dataset.  They  reported  that  K-medoids  provided  better  results  than K-means algorithm. Nellore and Ward (2015) used K-medoids algorithms for a certain distributions class. Al-Shammari  et  al.  (2016)  used  fuzzy  c-means,  K-means  and K-medoids  algorithms  for  wake  effect  analysis  in  wind  farm.  It  is  claimed  that K-medoids  algorithm  were  better  than  other  two  algorithms.  Arora  et  al.  (2016)  used K-means and K-medoids algorithms for big data. They claimed that K-medoids algorithm provided better performance than K-means algorithm. Cerquitelli et al. (2016) proposed a multiple-level  clustering  method  that  contained  K-means,  K-medoids  and  DBSCAN algorithms for a medical care sector.

Second stage of the hierarchal approach is route problem. Route problem was also discussed extensively in the literature. There are different types of route problem base on their constraints. Therefore, exact, heuristics, metaheuristic algorithms have been developed. We found numerous studies in the literature. However, our route problem was transformed to a TSP after completing clustering process in each cluster. Therefore, exact

algorithm may provide a solution in reasonable time period. That is true of course if the problem size is large enough. That is the reason we focused on only exact algorithm and TSP problem literature. A large number of exact algorithms have been proposed for the TSP (Laporte,  1992).  Dantzig  et  al.  (1954)  presented  one  of  the  earliest  integer  linear programming  (ILP)  formulations  of  TSP.  Miller  et  al.  (1960)  proposed  an  alternative formulation  by  adding  sub-tour  elimination  constraints.  Branch  and  bound  algorithm (B&amp;B) commonly used solving TSP in literature. An initial lower bound can be obtained from  Carpaneto  et  al.  (1988)  by  relaxing  sub-tour  constraint.  TSP  solved  by  B&amp;B algorithm by Eastman (1958), Little et al. (1963), Bellmore and Malone (1971), Garfinkel (1973), Smith et al. (1977), Miller and Pekny (1991).

The above mentioned studies are summarised in Table 1.

Table 1 Studies related to clustering problem

| Algorithms                        | Studies                                                                                                                                                                                                                                                                                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cluster first route second method | Bodin, (1975), Fisher and Jaikumar (1981), Hiquebran et al. (1993), Sariklis and Powell (2000), Cakir et al. (2015), Kloimüllner et. al. (2015)                                                                                                                                                                                           |
| Clustering algorithms             |                                                                                                                                                                                                                                                                                                                                           |
| K-means                           | Ng and Wong (2002), Tsai et al. (2002), Nallusamy et al. (2010), Elango et al. (2011), Hsieh and Huang (2011), Niknam et al. (2011), Kargari and Sepehri (2012), Ekici and Retharekar (2013), Fang et al. (2013), Luo and Chen (2014), Krishnasamy et al. (2014), Reed et al. (2014), Shivshankar and Jamalipour (2014), Tu et al. (2014) |
| K-medoids                         | Lucasius et al. (1993), Zhang and Couloigner (2005), Barioni et al. (2008), Fei et al. (2009), Park and Jun (2009), Lewis et al. (2012), Qi et al. (2012), Peng et al. (2014), Brito et al. (2015), Harikumar and PV (2015), Nellore and Ward (2015), Al-Shammari et al. (2016), Arora et al. (2016), Cerquitelli et al. (2016)           |
| Random clustering algorithm       | Thangiah (1993, 1995), Ho et al. (2001), Dondo and Cerdá (2007), Yassen et al. (2015)                                                                                                                                                                                                                                                     |
| Routing problem                   | Eastman (1958), Little et. al. (1963), Bellmore and Malone (1971), Garfinkel (1973), Smith et al. (1977), Miller and Pekny (1991).                                                                                                                                                                                                        |
| B&B algorithm                     |                                                                                                                                                                                                                                                                                                                                           |

The main objective of our study is to solve the distribution planning problem that meets the  weekly  demands  of  a  supermarket  chain.  We  proposed  a  hierarchical  approach consisted of cluster first and route second method. First, we clustered all customers using three different algorithms (K-means, K-medoids and random) separately. We maintained each cluster capacity (i.e., between 32-40 pallets) for a single vehicle during clustering process.  Second,  we  solved  route  problem  to  a  vehicle  of  each  cluster.  We  searched performances of the three algorithms using different days (busy, not busy). We tested the proposed approach on a real case study and compared by paired sample t tests.

## 3 Problem description and formulation

In this study, CVRP is discussed. There is no effective algorithm that can find the optimal solution in a reasonable time for CVRP if a problem belongs to NP-hard class (Lenstra and  Kan,  1981).  The  use  of  mathematical  methods  may  achieve  a  best  solution.  But,

solution time will increase exponentially because the exact methods will search the entire solution space (Toth and Vigo, 2002).

There  are  several  mathematical  models  developed  for  CVRP.  In  the  literature, Christofides  et  al.  (1981)  indicate  whether x ijk variable  can  go  from  i  customer  to  j costumer, N indicates customer number and M indicates vehicle numbers.

Models are as follows:

$$x _ { i j k } = \begin{cases} 1 & \text{if customer $j$ immediately visited after customer $i$ by the vehicle $k$} \\ 0 & \text{otherwise} \end{cases}$$

$$i \ne j, i, j \in \{ 0, \dots \dots, N \} \, \text{and} \, ) \, \text{main depot}$$

Objective function:

$$\min Z = \sum _ { i = 0 } ^ { N } \sum _ { j = 0 } ^ { N } \left ( c _ { i j } \sum _ { k = 1 } ^ { M } x _ { i j k } \right )$$

Constraints:

$$\sum _ { i = 0 } ^ { N } \sum _ { j = 0 } ^ { M } x _ { j k } = 1, \, j = 1, \dots, \, N$$

$$\sum _ { i = 0 } ^ { N } x _ { i p k } - \sum _ { j = 0 } ^ { N } x _ { p k } = 0, k = 1, \dots, M, \ p = 0, \dots,$$

$$\sum _ { i = 0 } ^ { N } \left ( q _ { i } \sum _ { j = 0 } ^ { N } x _ { i j k } \right ) \leq Q, \, k = 1, \dots, \, M$$

$$\sum _ { i = 0 } ^ { N } x _ { 0 j k } = 1, k = 1, \dots, M$$

$$y _ { i } - y _ { j } + N \sum _ { k = 1 } ^ { M } x _ { i j k } \leq N - 1, \ i \neq j = 1, \dots, N$$

- c ij = The distance between the customer i and the customer j (cost)
- Q = The total amount of product that can be loaded on a vehicle
- qi = The amount of i customer demand
- y i = Random variable used to prevent sub-round

The  aim  of  the  objective  function  is  to  minimise  of  the  total  distance.  Constraint  (1) guarantees that each customer must be visited only once. If the vehicle continues to move starting  from  the  visit  after  visiting  a  customer  if  that  customer  again  according  to constraint (2). Constraint (3) impose that each vehicle can carry a total amount of product has  a  certain  limit.  All  vehicles  must  be  used  only  once  according  to  constraint  (4). Constraint (5) eliminates the sub tours.

In this study, our problem is similar with classical CVRP. There is no any new special and different constraints a part from classical CVRP given above model. It is a fact that we cannot solve our real life problem in reasonable computational time. Therefore, we proposed the hierarchical approach (heuristic) to solve our CVRP.

## 4 The proposed hierarchical approach

In this study, our CVRP was not able to solve by an exact algorithm in a reasonable time. Therefore, we proposed the hierarchical approach consisted of two phases called cluster first and route second.

The cluster  first  route  second  method  is  a  sequential  two-phase  method:  clustering followed by routing and each of these phases consists of stages. An outline of the method is given below:

Phase 1 Clustering

Stage 1 Forming the clusters

Stage 2

Capacity control of the clusters

Phase 2 Routing

TSP (B&amp;B).

We tested the proposed approach on real life example given in Section 5.

## 4.1 Capacitated K-means algorithm

K-means  algorithm  is  proposed  by  MacQueen  (1967).  After  that,  it  is  classified  as technical division or technique for non-hierarchic clustering method (San et al., 2004). Kmeans  clustering  algorithm  is  one  the  clustering  method  for  pre-processing  data  and identifies hidden patterns in data sets.

Firstly, algorithm specifies the number of the sets. K sets number is determined in two stages. As a beginning, K number of random point is chosen among the objects. Later, K sets  number is found by taking averages of the all objects. K parameter represents the mean of the initial set in both cases. Each object in data set is assigned the most similar set  based on the distances between it and mean of the set and again means of sets are calculated.  This  iteration  continues  until  function  of  criteria  is  combined  in  a  point. Function of criteria is shown as below (Han and Kamber, 2001).

![Image](Papers_Converted/0_Artifacts/[comert2018]_A_cluster_first-route_second_approach_for_a_capacitated_vehicle_routing_problem_artifacts/image_000000_8b7c4111e6f5db06c2c4f8bb18ca41b1ec5f1de124f3dbef112cd64e779876c4.png)

- E Mean of sum for all objects in database
- Ci set of data points in i th cluster
- p the point represents specified an object

mi the set of centres.

K-means algorithm divides data sets that consist of k parameter and n data given by the user  into  k  number  clusters.  The  steps  of  K-means  are  given  in  Table  2  (MacQueen, 1967).

Table 2 K-means algorithm steps

|   Step | Activities                                                                                                                     |
|--------|--------------------------------------------------------------------------------------------------------------------------------|
|      1 | Determine first set centre.                                                                                                    |
|      2 | Calculate the distance between specified point and centre points. All objects are assigned to set which is the closes to them. |
|      3 | New centre points switch with mean values of all objects in that set.                                                          |
|      4 | Repeat step 2 and 3 until centre points is not changed.                                                                        |

The complexity of K-means algorithm is O tkn ( ). Here; n: number of objects, k: number of sets, t: iteration number (Huang, 1998).

First, the initial values of parameters of K-means algorithm are specified. Set number base value is specified by considering the condition of having 32-40 pallets capacity for the suitable set according to strategy of the distribution company.

Determining set number base value:

k d c =

k: number of sets, c: capacity of a truck, d: total demand.

If the set number is fractional then it is rounded up to integer number. For instance, if the rate for the sets number is 3.2, the decimally rate will round up to 4 and the base value is specified. The reason why we round up is to find a feasible solution for a distribution for each cluster.

After determining the base value of the set number, K-means algorithm is generated dealing  with  all  stores  for  distribution  until  a  cluster  consists  of  32-40  pallets,  then, related  customers  are  assigned  to  a  cluster.  If  a  cluster  capacity  is  not  satisfied  with number pallets (less or more than 32-40 pallets), the base value is rearranged (up and down) and capacity control is repeated until set having 32-40 pallets.

## 4.2 Capacitated K-medoids algorithm

K-medoids algorithm is proposed to overcome of sensitivity of k-means algorithm for the noisy data (Kaufman and Rousseeuw, 1990). The difference between K-Means and Kmedoids algorithms is that centre of sets are specified as the closest object to the centre of sets, not mean values of the objects in the set. That is why the algorithm is more sensitive to the noisy data than the K-means method (Treur, 2003).

K-medoids algorithm can be summarised in Table 3 (Liu, 2005).

The complexity of each iteration is O k n [ ( -k ) ]. Here 2 n: number of object, k: number of  set  (Ju,  2007).  First,  the  initial  values  of  parameters  of  K-medoids  algorithm  are specified.  Set  number  base  value  is  specified  as  it  is  done  in  Subsection  4.1  by considering the condition of having 32-40 pallets capacity for the suitable set according to  strategy  of  a  distribution  company.  Clusters  are  generated  with  repeating  steps  in Table 3 and each cluster is satisfied to capacity of a vehicle that is between 32-40 pallets.

Table 3 K-medoids algorithm steps

|   Step | Activities                                                               |
|--------|--------------------------------------------------------------------------|
|      1 | Determine k set number.                                                  |
|      2 | Choose k objects as initial medoids                                      |
|      3 | Assign the rest of the sets to the closest medoids x                     |
|      4 | Calculate objective function                                             |
|      5 | Determine the y point coincidence                                        |
|      6 | If changing x and y will minimise the objective function, change x and y |
|      7 | If there is no change, keep repeating Step 3-6                           |

## 4.3 Capacitated random clustering algorithm

Random  clustering  algorithm  that  is  an  unsupervised  learning  method  is  a  grouping process of the data according to data attributes without any classification information.

The  aim  of  the  random  clustering  algorithm  is  to  provide  the  maximisation  of similarity  inside  the  set  and  minimisation  of  the  similarity  between  sets  of  the  data obtained by calculating with mean values of the distances between one object specified as the centre of the gravity of the set and other objects. Random clustering algorithm can be summarised as follows in Table 4:

Table 4 Random clustering algorithm steps

|   Step | Activities                                                                                                          |
|--------|---------------------------------------------------------------------------------------------------------------------|
|      1 | Specify randomly the set centre as many as number of set which are wanted to be in the first step of the algorithm. |
|      2 | Calculate the distance between data and set centre which is chosen.                                                 |
|      3 | Generate set by determining which set they belong to according to shortest distances given.                         |

## 5 Application

## 5.1 Problem definition

A case study deals with CVRP that meets weekly demand of the firm serving in a retail sector is discussed. This firm is a supermarket chain that located in Marmara region of Turkey and dealt with product sales in a retail sector. Distribution operations to the stores are practiced only weekdays. There are two kinds of trucks in our distribution fleet in this study.  The  first  kind  of  trucks  carries  fresh  products  that  require  a  cold  environment. Second type of the trucks carries only dry products. The difference between these two types of trucks is that there is a refrigerator and insulation in the fresh products truck.

There is only one main depot and 78 stores of the supermarket chain. There are two type  of  stores  demand  namely  'daily'  and  'not  daily'  distributing  processes.  The  daily stores  demand  fresh  and  dry  product  on  each  weekday.  Whereas  the  not  daily  stores demand  fresh  and  dry  product  only  three  days  of  a  week  (Monday,  Wednesday  and Friday).  The  number  of  daily  and  not  daily  stores  is  66  and  12  respectively.  Type  of

stores (daily and not daily) is illustrated in Table 5. Days are also classified as a busy and a not busy day. Distributed to each day or some days are called as 'busy day' and 'not busy day' respectively in a week.

Table 5 Store number and its type

| Type of stores   | # store                                                                                                                                                                                                                                                        |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Daily            | 1, 2, 3, 4, 5, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 25, 26, 28, 29, 31, 32, 34, 35, 37, 38, 40, 41, 43, 44, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 72, 73, 75, 76, 78 |
| Not daily        | 6, 24, 27, 30, 33, 36, 39, 42, 45, 71, 74, 77                                                                                                                                                                                                                  |

According to the day and type of products, the distribution problem can be divided as four sub problems shown in Table 6.

Table 6 Sub problems of distribution

|   Sub problem | Type of product   | Busy day   | Not busy day   |
|---------------|-------------------|------------|----------------|
|             1 | Dry               |           |                |
|             2 | Fresh             |           |                |
|             3 | Dry               |            |               |
|             4 | Fresh             |            |               |

Location of the main depot and stores on the map are shown in Figure 1. Dots represent stores and one main  store. A  vehicle fleet performing distribution consisted of homogeneous vehicles had 40 pallets capacity. Vehicles must be loaded between 32 and 40 pallets.

Figure 1 Location of the main depot and stores

![Image](Papers_Converted/0_Artifacts/[comert2018]_A_cluster_first-route_second_approach_for_a_capacitated_vehicle_routing_problem_artifacts/image_000001_23436017c4b700438f14f8f17586d54f0b5eeff8e1b93dc9e59229387361889e.png)

In this study, a comparison was made for each sub problem of K-means, K-medoids and Random clustering  algorithms.  Twenty  different  sets  of  data  were  applied  using  three methods. In this section; because of page limitations and readability of the manuscript, only detail solution phases of busy days which is the problem of the distribution of dry product on busy day (sub problem 1) are explained. The other 2, 3 and 4 sub problems results are only summarised in Table 15.

## 5.2 The solution of the problem

There are 20 different data set where store demands are specified in the distribution of dry product on busy day sub problem. Total 78 stores are distributed in each data sets, the pallets number which stores demand changed in a range of 6-14 and the total number is 724 pallets.  First,  we  determined  clusters  and  number of vehicle using  three  clustering algorithms,  which  were  explained  in  detail  Subsection  4.1,  4.2  and  4.3  respectively. Second, we solved route problem and found total distance for each cluster using the B&amp;B algorithm.

Total number of vehicle and distance are given in Table 7 and Table 8. We reached a point to test whether there is a significant difference among three algorithms' results. We applied paired sample t tests to investigate the question.

Table 7 Total vehicle of each sub problem found according K-Means algorithm ('1'), K-medoids algorithm ('2') and random clustering algorithm (RCA) ('3')

| Week   |   Sub problem 1 |   Sub problem 1 |   Sub problem 1 |   Sub problem 2 |   Sub problem 2 |   Sub problem 2 |   Sub problem 3 |   Sub problem 3 |   Sub problem 3 |   Sub problem 4 |   Sub problem 4 |   Sub problem 4 |
|--------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
| Week   |               1 |               2 |               3 |               1 |               2 |               3 |               1 |               2 |               3 |               1 |               2 |               3 |
| 1      |              20 |              21 |              21 |              12 |              12 |              12 |              19 |              18 |              18 |              12 |              12 |              11 |
| 2      |              21 |              20 |              21 |              12 |              12 |              12 |              18 |              18 |              19 |              12 |              11 |              11 |
| 3      |              20 |              21 |              21 |              12 |              12 |              12 |              18 |              18 |              18 |              11 |              11 |              11 |
| 4      |              21 |              21 |              21 |              12 |              13 |              12 |              18 |              19 |              18 |              12 |              12 |              11 |
| 5      |              20 |              20 |              22 |              13 |              13 |              13 |              18 |              19 |              18 |              11 |              12 |              11 |
| 6      |              20 |              20 |              21 |              12 |              13 |              13 |              18 |              19 |              18 |              11 |              11 |              11 |
| 7      |              21 |              21 |              21 |              13 |              12 |              12 |              18 |              19 |              19 |              11 |              11 |              11 |
| 8      |              21 |              21 |              20 |              13 |              13 |              12 |              18 |              19 |              19 |              11 |              12 |              12 |
| 9      |              20 |              21 |              21 |              13 |              12 |              13 |              18 |              19 |              18 |              11 |              11 |              11 |
| 10     |              20 |              21 |              21 |              13 |              13 |              12 |              18 |              18 |              19 |              12 |              11 |              11 |
| 11     |              21 |              21 |              21 |              13 |              13 |              13 |              18 |              18 |              18 |              12 |              12 |              11 |
| 12     |              21 |              20 |              21 |              12 |              13 |              12 |              18 |              17 |              18 |              11 |              11 |              11 |
| 13     |              20 |              20 |              21 |              13 |              13 |              12 |              19 |              18 |              19 |              12 |              12 |              12 |
| 14     |              20 |              21 |              21 |              12 |              12 |              13 |              18 |              19 |              18 |              11 |              12 |              11 |
| 15     |              21 |              21 |              20 |              12 |              12 |              13 |              19 |              18 |              18 |              12 |              11 |              11 |
| 16     |              20 |              20 |              21 |              13 |              12 |              12 |              18 |              18 |              19 |              11 |              11 |              12 |
| 17     |              20 |              20 |              20 |              13 |              12 |              12 |              18 |              18 |              19 |              11 |              12 |              11 |
| 18     |              20 |              21 |              21 |              13 |              12 |              12 |              19 |              19 |              18 |              11 |              12 |              11 |
| 19     |              21 |              21 |              21 |              13 |              12 |              12 |              18 |              18 |              18 |              12 |              12 |              12 |
| 20     |              20 |              21 |              20 |              12 |              12 |              14 |              19 |              18 |              18 |              11 |              11 |              12 |

410 S.E. Comert et al. Table 8 Total distance (kms) of each sub problem found according K-means algorithm ('1'),

## K-medoids algorithm ('2') and random clustering algorithm ('3')

![Image](Papers_Converted/0_Artifacts/[comert2018]_A_cluster_first-route_second_approach_for_a_capacitated_vehicle_routing_problem_artifacts/image_000002_b57aeab7bcf99bd7ec006960784455ffc083c53b369a7e7877c0ab30acd7c0a8.png)

We performed paired sample t test to compare the solutions illustrated in Table 7 and Table 8. First of all, we compared the solutions of the K-means and K-medoids algorithm in terms of total vehicle and distance with paired sample t test. The null hypotheses are below. The test results are illustrated in Table 9 and Table 10 respectively.

Null hypothesis 1 There is no significant difference between the solutions' means of K-means and K-medoids algorithm's solutions in terms of total vehicle.

Null hypothesis 2

There is no significant difference between the solutions' means of K-means and K-medoids algorithm's solutions in terms of total distance.

Table 9 Paired sample t test between the K-means and K-medoids algorithm's solutions in terms of total vehicle

| Paired T for total vehicle K-means - total vehicle K-medoids   | Paired T for total vehicle K-means - total vehicle K-medoids   | Paired T for total vehicle K-means - total vehicle K-medoids   | Paired T for total vehicle K-means - total vehicle K-medoids   | Paired T for total vehicle K-means - total vehicle K-medoids   |
|----------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|
|                                                                | N                                                              | Mean                                                           | St. dev.                                                       | SE mean                                                        |
| Total vehicle K-means                                          | 80                                                             | 15.650                                                         | 3.829                                                          | 0.428                                                          |
| Total vehicle K-medoids                                        | 80                                                             | 15.725                                                         | 3.933                                                          | 0.440                                                          |
| Difference                                                     | 80                                                             | -0.0750                                                        | 0.6894                                                         | 0.0771                                                         |

95% CI for mean difference: (-0.2284; 0.0784)

T-test of mean difference = 0 (vs. not = 0): T-value = -0.97, P-value = 0.334

Table 10 Paired sample t test between K-means and K-medoids algorithm's solutions in terms of total distance

| Paired T for total distance K-means - total distance K-medoids   | Paired T for total distance K-means - total distance K-medoids   | Paired T for total distance K-means - total distance K-medoids   | Paired T for total distance K-means - total distance K-medoids   | Paired T for total distance K-means - total distance K-medoids   |
|------------------------------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|
|                                                                  | N                                                                | Mean                                                             | St. Dev                                                          | SE mean                                                          |
| Total distance K-means                                           | 80                                                               | 1,688.4                                                          | 375.1                                                            | 41.9                                                             |
| Total distance K-medoids                                         | 80                                                               | 1,458.5                                                          | 297.8                                                            | 33.3                                                             |
| Difference                                                       | 80                                                               | 229.9                                                            | 118.4                                                            | 13.2                                                             |
| 95% CI for mean difference: (203.6; 25,603)                      | 95% CI for mean difference: (203.6; 25,603)                      | 95% CI for mean difference: (203.6; 25,603)                      | 95% CI for mean difference: (203.6; 25,603)                      | 95% CI for mean difference: (203.6; 25,603)                      |

According to the results in Table 9, Null hypothesis 1 is accepted with %95 confidence interval.  So  there  is  no  significant  difference  between  the  solutions'  means  of  the K-means  and  K-medoids  algorithm's  solutions  in  terms  of  total  vehicle.  But  Null hypothesis 2 is rejected with %95 confidence interval according to the results in Table 10.  This  means  that,  there  is  a  significant  difference  between  the  solutions'  means  of K-means and K-medoids algorithm's solutions in terms of total distance. Means of the K-means  algorithms'  solution  values  are  higher  than  K-medoids  algorithms'  solution values considering positive mean difference interval. This means that K-medoids is better than K-means in terms of total distance.

Then  we  compared  the  solutions  of  K-means  and  random  clustering  algorithm  in terms of total vehicle and total distance with paired sample t test and the null hypotheses are below. The test results are illustrated in Table 11 and Table 12 respectively.

Null hypothesis 3

There is no significant difference between the solutions' means of K-means and random clustering algorithm's solutions in terms of total vehicle.

Null hypothesis 4

There is no significant difference between the solutions' means of K-means and random clustering algorithm's solutions in terms of total distance.

Table 11 Paired sample t test for the K-means and random clustering algorithm's solutions in terms of total vehicle

| Paired T for total vehicle K-means - total vehicle RCA   | Paired T for total vehicle K-means - total vehicle RCA   | Paired T for total vehicle K-means - total vehicle RCA   | Paired T for total vehicle K-means - total vehicle RCA   | Paired T for total vehicle K-means - total vehicle RCA   |
|----------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------|
|                                                          | N                                                        | Mean                                                     | St. dev.                                                 | SE mean                                                  |
| Total vehicle K-means                                    | 80                                                       | 15.650                                                   | 3.829                                                    | 0.428                                                    |
| Total vehicle RCA                                        | 80                                                       | 15.712                                                   | 4.063                                                    | 0.454                                                    |
| Difference                                               | 80                                                       | -0.0625                                                  | 0.7850                                                   | 0.0878                                                   |

95% CI for mean difference: (-0.2372; -0.1122)

T-test of mean difference = 0 (vs. not = 0): T-value = -0.71, P-value = 0.0479

Table 12 Paired sample t test for the K-means and random clustering algorithm's solutions in terms of total distance

| Paired T for Total Distance K-Means - Total Distance RCA   | Paired T for Total Distance K-Means - Total Distance RCA   | Paired T for Total Distance K-Means - Total Distance RCA   | Paired T for Total Distance K-Means - Total Distance RCA   | Paired T for Total Distance K-Means - Total Distance RCA   |
|------------------------------------------------------------|------------------------------------------------------------|------------------------------------------------------------|------------------------------------------------------------|------------------------------------------------------------|
|                                                            | N                                                          | Mean                                                       | St. dev.                                                   | SE mean                                                    |
| Total distance K-means                                     | 80                                                         | 1,688.4                                                    | 375.1                                                      | 41.9                                                       |
| Total distance RCA                                         | 80                                                         | 1,724.2                                                    | 376.7                                                      | 42.1                                                       |
| Difference                                                 | 80                                                         | -35.81                                                     | 76.78                                                      | 8.58                                                       |

95% CI for mean difference: (-52.90; -18.73)

T-test of mean difference = 0 (vs. not = 0): T-value = -4.17, P-value = 0.000

According to the results in Table 11 and Table 12, Null hypothesis 3 and 4 are rejected with 95% confidence interval. This means that there is a significant difference between the solutions' means of K-means and random clustering algorithm's solutions in terms of total vehicle and distance. Means of the K-means algorithms' solution values are lower than random clustering algorithms' solution values considering negative mean difference interval.  This  means  that  K-means  is  better  than  random  clustering  in  terms  of  total vehicle and total distance.

Finally, we  compared  the  solutions  of  the  K-medoids  and  random  clustering algorithm in terms of total vehicle and total distance with paired sample t test and the null hypotheses  are  below.  The  test  results  are  illustrated  in  Table  13  and  Table  14 respectively.

Null hypothesis 5

There is no significant difference between the solutions' means of K-medoids and random clustering algorithm's solutions in terms of total vehicle.

Null hypothesis 6

There is no significant difference between the solutions' means of K-medoids and random clustering algorithm's solutions in terms of total distance.

Table 13 Paired sample t test for the K-medoids and random clustering algorithm's solutions in terms of total vehicle

| Paired T for total vehicle K-medoids - total vehicle RCA                     | Paired T for total vehicle K-medoids - total vehicle RCA                     | Paired T for total vehicle K-medoids - total vehicle RCA                     | Paired T for total vehicle K-medoids - total vehicle RCA                     | Paired T for total vehicle K-medoids - total vehicle RCA                     |
|------------------------------------------------------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------|
|                                                                              | N                                                                            | Mean                                                                         | St. dev.                                                                     | SE mean                                                                      |
| Total vehicle K-medoids                                                      | 80                                                                           | 15.725                                                                       | 3.933                                                                        | 0.440                                                                        |
| Total vehicle RCA                                                            | 80                                                                           | 15.712                                                                       | 4.063                                                                        | 0.454                                                                        |
| Difference                                                                   | 80                                                                           | 0.0125                                                                       | 0.7546                                                                       | 0.0844                                                                       |
| 95% CI for mean difference: (-0.1554; 0.1804)                                | 95% CI for mean difference: (-0.1554; 0.1804)                                | 95% CI for mean difference: (-0.1554; 0.1804)                                | 95% CI for mean difference: (-0.1554; 0.1804)                                | 95% CI for mean difference: (-0.1554; 0.1804)                                |
| T-test of mean difference = 0 (vs. not = 0): T-value = 0.15, P-value = 0.883 | T-test of mean difference = 0 (vs. not = 0): T-value = 0.15, P-value = 0.883 | T-test of mean difference = 0 (vs. not = 0): T-value = 0.15, P-value = 0.883 | T-test of mean difference = 0 (vs. not = 0): T-value = 0.15, P-value = 0.883 | T-test of mean difference = 0 (vs. not = 0): T-value = 0.15, P-value = 0.883 |

Table 14 Paired sample t test for the K-medoids and random clustering algorithm's solutions in terms of total distance

| Paired T for total distance K-medoids - total distance RCA                     | Paired T for total distance K-medoids - total distance RCA                     | Paired T for total distance K-medoids - total distance RCA                     | Paired T for total distance K-medoids - total distance RCA                     | Paired T for total distance K-medoids - total distance RCA                     |
|--------------------------------------------------------------------------------|--------------------------------------------------------------------------------|--------------------------------------------------------------------------------|--------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
|                                                                                | N                                                                              | Mean                                                                           | St. dev.                                                                       | SE mean                                                                        |
| Total distance K-medoids                                                       | 80                                                                             | 1,458.5                                                                        | 297.8                                                                          | 33.3                                                                           |
| Total distance RCA                                                             | 80                                                                             | 1,724.2                                                                        | 376.7                                                                          | 42.1                                                                           |
| Difference                                                                     | 80                                                                             | -265.7                                                                         | 111.8                                                                          | 12.5                                                                           |
| 95% CI for mean difference: (-290.6; -240.8)                                   | 95% CI for mean difference: (-290.6; -240.8)                                   | 95% CI for mean difference: (-290.6; -240.8)                                   | 95% CI for mean difference: (-290.6; -240.8)                                   | 95% CI for mean difference: (-290.6; -240.8)                                   |
| T-test of mean difference = 0 (vs. not = 0): T-value = -21.26, P-Value = 0.000 | T-test of mean difference = 0 (vs. not = 0): T-value = -21.26, P-Value = 0.000 | T-test of mean difference = 0 (vs. not = 0): T-value = -21.26, P-Value = 0.000 | T-test of mean difference = 0 (vs. not = 0): T-value = -21.26, P-Value = 0.000 | T-test of mean difference = 0 (vs. not = 0): T-value = -21.26, P-Value = 0.000 |

According to the results in Table 13, Null hypothesis 5 is accepted. This means that there is no significant difference between the solutions' means of the K-medoids' and random clustering  algorithm's  solutions  in  terms  of  total  vehicle.  In  addition,  according  to  the Table 14, Null hypothesis 6 is rejected. This means that there is a significant difference between  the  solutions'  means  of  the  K-medoids  and  random  clustering  algorithm's solutions in terms of total distance. Means of the K-medoids algorithms' solution values are lower than random clustering algorithms' solution values considering negative mean difference interval. This means that K-medoids is better than random clustering in terms of total distance.

All the results of the paired sample t test are summed up in Table 15. According to the Table 15, there is no difference between K-means and K-medoids in terms of total vehicle.  However,  both  are  better  than  random  clustering  algorithm.  In  terms  of  total distance, K-medoids is better than K-means and random clustering and K-means is better than random clustering algorithm.

Table 15 Paired sample t test results (sub problem 1)

|                       | Responses     | Responses      |
|-----------------------|---------------|----------------|
| Compared algorithms   | Total vehicle | Total distance |
| K-means and K-medoids | No difference | K-medoids      |
| K-means and random    | K-means       | K-means        |
| K-medoids and random  | No difference | K-medoids      |

Table 16

![Image](Papers_Converted/0_Artifacts/[comert2018]_A_cluster_first-route_second_approach_for_a_capacitated_vehicle_routing_problem_artifacts/image_000003_c321b96978d793bbc2a2047e177253a7a1a2d59953137ec1bab82346c5c7edb7.png)

As  mentioned  in  the  literature  section,  Harikumar  and  PV  (2015),  Al-Shammari  et  al. (2016) and Arora et al. (2016) compare K-medoids with any other algorithms. They also indicated that K-medoids was better than the others. In the light of this knowledge, we can say that K-medoids was outstanding algorithm for minimising total vehicle as well as total distance.

Four sub problems of the case study are summarised in Table 16. In addition to this, first week of sub problem 1's solution is given in detail with all routes in Table 16. For example; we employed K-means, K-medoids and random clustering to find total distance as 2,145.8 km, 1,860 km, 2224.4 km and total vehicle as 20, 21 and 21 respectively.

## 6 Discussion and conclusions

This paper proposes cluster first route second approach to reduce the solution time of a large scale CVRP. According to this approach, customers are clustered considering the distance  between  them  and  vehicle  capacities.  So  these  processes  reduce  the  scale  of CVRP and convert it to VRP due to control the capacity. After clustering, all clusters get service from a single vehicle need to a route. Therefore a small scale VRP can be solved by a best known travelling salesman mathematical model using B&amp;B algorithm.

The major challenge is about determining the clustering algorithm at this point. In this study  we  discussed  three  clustering algorithm; K-means,  K-medoids  and  random clustering algorithms on a case study in retail sector. The considered firm had 78 stores and demand data of 20 weeks were obtained using customers' demands. Vehicle fleet of the firm consisted homogeneous vehicles and all of them should be loaded with 32-40 pallets. And this delivery problem was clustered mentioned three algorithms. The results were tested statistically and illustrated that K-medoids algorithm is superior to others in terms of total vehicle and total distance.

## References

Akpinar,  S.  (2016)  'Hybrid  large  neighborhood  search  algorithm  for  capacitated  vehicle  routing problem', Expert Systems with Applications , Vol. 61, No. C, pp.28-38.

Al-Shammari,  E.T.,  Shamshirband,  S.,  Petkovi,  D.,  Zalnezhad,  E.,  Yee,  P.  L.,  Taher,  R.  S.  and Cojbasic, Z. (2016) 'Comparative study of clustering methods for wake effect analysis in wind farm', Energy , Vol. 95, pp.573-579.

Arora, P., Deepali, D. and Varshney, S. (2016) 'Analysis of K-means and K-medoids algorithm for big data', Procedia Computer Science , Vol. 78, pp.507-512.

Barioni, M.C.N., Razente, H.L., Traina, A.J.M. and Jr., C.T. (2008) 'Accelerating K-medoid-based algorithms  through  metric  access  methods', The Journal of Systems and Software ,  Vol.  81, No. 3, pp.343-355.

Bellmore,  M.  and  Malone,  J.C.  (1971)  'Pathology  of  traveling-salesman  subtour-elimination algorithms', Operations Research , Vol. 19, No. 2, pp.278-307.

Bodin,  L.D.  (1975)  'A  taxonomic  structure  for  vehicle  routing  and  scheduling  problems', Computers &amp;Urban Society , Vol. 1, No. 1, pp.11-29.

Brito, P.Q., Soares, C., Almeida, S., Monte, A. and Byvoet, M. (2015) 'Customer segmentation in a large database of an online customized fashion business', Robotics and Computer-Integrated Manufacturing , Vol. 36, No. C, pp.93-100.

Cakir,  F.,  Street,  W.N.  and  Thomas,  B.W.  (2015) Revisiting  Cluster  First,  Route  Second for  the Vehicle Routing Problem ,  Department of Management Sciences Tippie College of Business University of Iowa Iowa City, Iowa, USA.

Carpaneto,  G.,  Martello,  S.  and  Toth,  P.  (1988)  'Algorithms  and  codes  for  the  assignment problem', Annals of Operations Research , Vol. 13, No. 1, pp.191-223.

Cerquitelli, T., Chiusano, S. and Xiao, X. (2016) 'Exploiting clustering algorithms in a multiple-level fashion: a comparative study in the medical care scenario', Expert Systems with Applications , Vol. 55, pp.297-312.

Chen, P., Huang, H. and Dong, X-Y. (2010) 'Iterated variable neighborhood descent algorithm for the  capacitated vehicle routing problem', Expert Systems with Applications ,  Vol.  37,  No.  2, pp.1620-1627.

Christofides,  N.,  Mingozzi,  A.  and  Toth,  P.  (1981)  'Exact  algorithms  for  the  vehicle  routing problem, based on spanning tree and shortest path relaxations', Mathematical Programming , Vol. 20, No. 1, pp.255-282.

Clarke, G. and Wright, J.W. (1964) 'Scheduling of vehicles from a central depot to a number of delivery points', Operations Research , Vol. 12, No. 4, pp.568-581.

Dantzig,  G.,  Fulkerson,  R.  and  Johnson,  S.  (1954)  'Solution  of  a  large-scale  traveling-salesman problem', Journal of the Operations Research Society of America , Vol. 2, No. 4, pp.393-410.

Dondo,  R.  and  Cerdá,  J.  (2007)  'A  cluster-based  optimization  approach  for  the  multi-depot heterogeneous  fleet  vehicle  routing  problem  with  time  Windows', European  Journal  of Operational Research , Vol. 176, No. 3, pp.1478-1507.

Eastman,  W.L.  (1958) Linear  Programming  with  Pattern  Constraints , Harvard  University, Cambridge, USA.

Ekici,  A.  and  Retharekar,  A.  (2013)  'Multiple  agents  maximum  collection  problem  with  time dependent rewards', Computers &amp; Industrial Engineering , Vol. 64, No. 4, pp.1009-1018.

Elango,  M.,  Nachiappan,  S.  and  Tiwari,  M.K.  (2011)  'Balancing  task  allocation  in  multi-robot systems  using  K-means  clustering  and  auction  based  mechanisms', Expert  Systems  with Applications , Vol. 38, No. 6, pp.6486-6491.

Fang, C., Jin, W. and Ma, J. (2013) 'K-Means algorithms for clustering analysis with frequency sensitive discrepancy metrics', Pattern Recognition Letters , Vol. 34, No. 5, pp.580-586.

Fei,  H.,  Meskens,  N.  and  Moreau,  C-H.  (2009)  'Clustering  of  patient  trajectories  with  an auto-stopped bisecting K-Medoids algorithm', in Proceedings of the 13th IFAC Symposium on Information Control Problems in Manufacturing , Moscow, Russia, pp.355-360.

Fisher,  M.L.  and  Jaikumar,  R.  (1981)  'A  generalized  assignment  heuristic  for  vehicle  routing', Networks , Vol. 11, No. 2, pp.109-124.

Garfinkel,  R.S.  (1973)  'On  partitioning  the  feasible  set  in  a  branch-and-bound  algorithm  for  the asymmetric traveling-salesman problem', Operations Research , Vol. 21, No. 1, pp.340-343.

Han,  J.  and  Kamber,  M.  (2001) Data  Mining:  Concept  and  Techniques , Morgan  Kaufmann Publishers, San Francisco.

Harikumar,  S.  and  PV,  S.  (2015)  'K-medoid  clustering  for  heterogeneous  data  sets', Procedia Computer Science , Vol. 70, pp.226-237.

Hiquebran, D.T., Alfa, A.S., Shapiro, J.A. and Gittoes, D.H. (1993) 'A revised simulated annealing and cluster-first route-second algorithm applied to the vehicle routing problem', Engineering Optimization , Vol. 22, No. 2, pp.77-107.

Ho,  W-K.,  Ang,  J.C.  and  Lim,  A.  (2001)  'A  hybrid  search  algorithm  for  the  vehicle  routing problem with time windows', International Journal on Artificial Intelligence Tools ,  Vol. 10, No. 3, pp.431-449.

Hsieh, L-F. and Huang,  Y-C.  (2011)  'New  batch  construction  heuristics to optimise the performance  of  order  picking  systems', International  Journal  of  Production  Economics , Vol. 131, No. 2, pp.618-630.

Huang,  Z.  (1998)  'Extensions  to  the  k-means  algorithm  for  clustering  large  data  sets  with categorical values', Data Mining and Knowledge Discovery , Vol. 2, No. 3, pp.283-304.

Jepsen, M., Spoorendonk, S. and Ropke, S. (2013) 'A branch-and-cut algorithm for the symmetric two-echelon  capacitated  vehicle  routing  problem', Transportation  Science ,  Vol.  47,  No.  1, pp.23-37.

Jin,  J.,  Crainic,  T.G.  and  Løkketangen,  A.  (2014)  'A  cooperative  parallel  metaheuristic  for  the capacitated vehicle routing problem', Computers and Operations Research , Vol. 44, pp.33-41.

Ju, J.W.N. (2007) Data Mining Concept and Techniques [online] http://www.cs.sunysb.edu/~cse634/spring2007/group6\_final.ppt (accessed 12 February 2012).

Kargari,  M.  and  Sepehri,  M.M.  (2012)  'Stores  clustering  using  a  data  mining  approach  for distributing  automotive  spare-parts  to  reduce  transportation  costs', Expert  Systems  with Applications , Vol. 39, No. 5, pp.4740-4748.

Kaufman,  L.  and  Rousseeuw,  P.J.  (1990) Finding  Groups  in  Data:  An  Introduction  to  Cluster Analysis , John Wiley and Sons Inc., New York.

Kloimüllner, C., Papazek, P., Hu, B. and Raidl, G.R. (2015) 'A cluster-first route-second approach for  balancing  bicycle  sharing  systems',  in

International  Conference  on  Computer  Aided

Systems Theory

, pp.439-446.

Krishnasamy, G., Kulkarni, A.J. and Paramesran, R. (2014) 'A hybrid approach for data clustering based  on  modified  cohort  intelligence  and  K-means', Expert  Systems  with  Applications , Vol. 41, No. 13, pp.6009-6016.

Laporte,  G.  (1992)  'The  traveling  salesman  problem:  an  overview  of  exact  and  approximate algorithms', European Journal of Operational Research , Vol. 59, No. 2, pp.231-247.

Laporte, G., Nobert, Y. and Taillefer, S. (1988) 'Solving a family of multi-depot vehicle routing and location-routing problems', Transportation Science , Vol. 22, No. 3, pp.161-172.

Lenstra, J.K. and Kan, A.H.G. (1981) 'Complexity of vehicle routing and scheduling problems', Networks , Vol. 11, No. 2, pp.221-227.

Lewis,  R.,  Mello,  C.A.  and  White,  A.M.  (2012)  'Tracking  epileptogenesis  progressions  with layered  fuzzy  k-means  and  k-medoid  clustering', Procedia  Computer  Science , Vol.  9, pp.432-438.

Lin,  S-W.,  Lee,  Z-J.,  Ying,  K-C.  and  Lee,  C-Y.  (2009)  'Applying  hybrid  meta-heuristic  for capacitated  vehicle  routing  problem', Expert  Systems  with  Applications , Vol.  36,  No.  2, pp.1505-1512.

Little,  J.D.,  Murty,  K.G.,  Sweeney,  D.W.  and  Karel,  C.  (1963)  'An  algorithm  for  the  traveling salesman problem', Operations Research , Vol. 11, No. 6, pp.972-989.

Liu, B. (2005) Data Mining: Process and Techniques [online]

http://www.cs.uic.edu/~liub/teach/cs583-spring-05/cs583.html (accessed 5 April 2010). Lucasius, C.B., Dane, A.D. and Kateman, G. (1993) 'On k-medoid clustering of large data sets with Analytical Chimica the aid of a genetic algorithm: background, feasibility and comparison', Acta , Vol. 282, No. 3, pp.647-669.

Luo,  J.  and  Chen,  M-R.  (2014)  'Multi-phase  modified  shuffled  frog  leaping  algorithm  with extremal  optimization  for  the  MDVRP  and  the  MDVRPTW', Computers  &amp;  Industrial Engineering , Vol. 72, pp.84-97.

MacQueen, J. (1967) 'Some methods for classification and analysis of multi-variate observations' in Proceedings of the Fifth Berkeley Symposium on Mathematical, Statistics and Probability , Los Angeles, pp.281-297.

Miller, C.E., Tucker, A.W. and Zemlin, R.A. (1960) 'Integer programming formulation of traveling salesman problems', Journal of the ACM (JACM) , Vol. 7, No. 4, pp.326-329.

Miller,  D.L.  and  Pekny,  J.F.  (1991)  'Exact  solution  of  large  asymmetric  traveling  salesman problems', Science , Vol. 251, No. 4995, p.754.

Nallusamy,  R.,  Duraiswamy,  K.,  Dhanalaksmi,  R.  and  Parthiban,  P.  (2010)  'Optimization  of non-linear  multiple  traveling  salesman  problem  using  k-means  clustering,  shrink  wrap algorithm  and  meta-heuristics', International  Journal  of  Nonlinear  Science ,  Vol.  9,  No.  2, pp.171-177.

Nazif,  H.  and  Lee,  L.S.  (2012)  'Optimised  crossover  genetic  algorithm  for  capacitated  vehicle routing problem', Applied Mathematical Modelling , Vol. 36, No. 5, pp.2110-2117.

Nellore, A. and Ward, R. (2015) 'Recovery guarantees for exemplar-based clustering', Information and Computation , Vol. 245, pp.165-180.

Ng, M.K. and Wong, J.C. (2002) 'Clustering categorical data sets using tabu search techniques', Pattern Recognition , Vol. 35, No. 12, pp.2783-2790.

Niknam,  T.,  Fard,  E.T.,  Pourjafarian,  N.  and  Rousta,  A.  (2011)  'An  efficient  hybrid  algorithm based  on  modified  imperialist  competitive  algorithm  and  K-means  for  data  clustering', Engineering Applications of Artificial Intelligence , Vol. 24, No. 2, pp.306-317.

Park, H-S. and Jun, C-H. (2009) 'A simple and fast algorithm for K-medoids clustering', Expert Systems with Applications , Vol. 36, No. 2, pp.3336-3341.

Peng, L., Dong, G-Y., Dai, F.F. and Liu, G-P. (2014) 'A new clustering algorithm based on ACO and  K-medoids  optimization  methods',  in Proceedings  of  the  19th  World  Congress  The International Federation of Automatic Control , Cape Town, South Africa, pp.9727-9731.

Qi,  M.,  Lin,  W-H.,  Li,  N.  and  Miao,  L.  (2012)  'A  spatiotemporal  partitioning  approach  for large-scale  vehicle  routing  problems  with  time  windows', Transportation  Research  Part  E: Logistics and Transportation Review , Vol. 48, No. 1, pp.248-257.

Reed, M., Yiannakou, A. and Evering, R. (2014) 'An ant colony algorithm for the multi-compartment vehicle routing problem', Applied Soft Computing , Vol. 15, pp.169-175.

San,  O.  M.,  Huynh,  V-N.  and  Nakamori,  Y.  (2004)  'An  alternative  extension  of  the  k-means algorithm  for  clustering  categorical  data', International  Journal  Application  Mathematics Computer Science , Vol. 14, No. 2, pp.241-247.

Sariklis,  D.  and  Powell,  S.  (2000)  'A  heuristic  method  for  the  open  vehicle  routing  problem', Journal of the Operational Research Society , Vol. 51, No. 5, pp.564-573.

Shivshankar, S. and Jamalipour, A. (2014) 'Spatio-temporal multicast grouping for content-based routing  in  vehicular  networks:  a  distributed  approach', Journal  of  Network  and  Computer Applications , Vol. 39, pp.93-103.

Smith,  T.H.,  Srinivasan,  V.  and  Thompson,  G.L.  (1977)  'Computational  performance  of  three subtour elimination algorithms for solving asymmetric traveling salesman problems', Annals of Discrete Mathematics , Vol. 1, pp.495-506.

Tadei, R. and Perboli, G. (2011) 'The two-echelon capacitated vehicle routing problem: models and math-based heuristics', Transportation Science , Vol. 45, No. 3, pp.364-380

Thangiah, S.R. (1993) 'Vehicle routing with time windows using genetic algorithms', in Chambers, L. (Eds.): Applications Handbook of Genetic Algorithms , pp.253-277, CRC Press, Florida.

Thangiah, S.R. (1995) 'An adaptive clustering method using a geometric shape for vehicle routing problems with time windows', in Proceedings of the 6th International Conference on Genetic Algorithms , Morgan Kaufmann Publishers Inc., pp.536-545.

Toth,  P.  and  Vigo,  D.  (2002) The  Vehicle  Routing  Problem ,  SIAM  Monographs  on  Discrete Mathematics and Applications, Philadelphia.

Treur, R. (2003) Spatial Clustering Methods in Data Mining [online] http://www.cs.uu.nl/docs/vakken/gdm/ch8.ppt. (Accessed 26 September 2016).

Tsai, C-F., Wu, H-C. and Tsai, C-W. (2002) 'A new data clustering approach for data mining in large  databases'  in Proceedings  of  the  International  Symposium  on  Parallel  Architectures, Algorithms and Networks , Metro Manila.

Tu,  E.,  Cao,  L.,  Yang,  J.  and  Kasabov,  N.  (2014)  'A  novel  graph-based  k-means  for  nonlinear manifold clustering and representative selection', Neurocomputing , Vol. 143, pp.109-122.

Venkates,  G.,  Gehrke,  J.  and  Ramakrishnan,  R.  (1999)  'Mining  very  large  databases', IEEE Computer , Vol. 32, No. 9, pp.38-45.

Xu, Z., Wang, L., Luo, J. and Zhang, J. (2005) 'A modified clustering algorithm for data mining', in Proceedings of the 2005 IEEE International , pp.741-744.

Yassen, E.T., Ayob, M., Nazri, M.Z.A. and Sabar, N.R. (2015) 'Meta-harmony search algorithm for the vehicle  routing  problem  with  time  windows', Information  Sciences , Vol.  325, pp.140-158.

Zhang,  Q.  and  Couloigner,  I.  (2005)  'A  new  and  efficient  k-medoid  algorithm  for  spatial clustering', Lecture Notes in Computer Science , Vol. 3482, pp.181-189.