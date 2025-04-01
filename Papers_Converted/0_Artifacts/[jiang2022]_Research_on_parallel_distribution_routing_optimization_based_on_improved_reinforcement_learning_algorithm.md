## Research on Parallel Distribution Routing Optimization Based on Improved Reinforcement Learning Algorithm

Jun Jiang School of Information Beijing Wuzi University Beijing, China jiangjun\_4565@163.com

## Ran Ma

School of Information Beijing Wuzi University Beijing, China 1908122104@bwu.edu.cn

Guicheng Shen* School of Information Beijing Wuzi University Beijing, China guichengshen@126.com

Abstract -With the rapid development of e-commerce, the efficiency  requirements  for  logistics  distribution  are  getting higher  and  higher,  so  distribution  routing  optimisation  has important theoretical and practical significance. In this paper, a  new coding mechanism is designed to solve the distribution task in the multi-distribution  centre  scenario  and  vehicle scheduling problem with capacity constraints in the distribution scenario. Simulation results show that this algorithm  has  the advantages  of high solution efficiency, variable input and avoidance of re-referencing compared with two heuristics of Google OR-Tools, ant colony algorithm and simulated annealing algorithm, thus this algorithm can effectively  solve  the  logistics  distribution  routing  optimisation problem and improve the efficiency of logistics distribution.

Keywords -Logistics distribution, vehicle routing, multi-depot, reinforcement learning algorithm, transformer

## I. INTRODUCTION

With  the  rapid  development  of  the  e-commerce  and express delivery industry, the labour costs of enterprises are rising,  the  labour  efficiency  is  not  synchronized  with  the increase  of the industry, and  the  logistics industry  has entered a rapid upgrading stage. Optimising the distribution system and establishing and improving an intelligent distribution system is an inevitable trend for the reform and development of e-commerce and express delivery enterprises. Therefore, it is of great theoretical and practical significance to  study  the  distribution  routing  optimization  problem  of logistics vehicles and seek economic and efficient solutions.

In essence, the distribution routing optimisation problem of  logistics  vehicles  studied  in  this  paper  is  the  same  in nature as the classical Capacitated Vehicle Routing Problem (CVRP)  problem,  which  belongs  to  the  same  NP-hard problem.As  early  as  1953,  Metropolis  et  al.  proposed  a simulated  annealing  algorithm [1] ; in  1983  Kirkpatrick  et  al. applied it to combinatorial optimization problems [2] ; Zhang Hai et al. improved the ant colony algorithm by adding the parameter of negative pheromone to remember the already visited customers [3] .

Traditional routing optimisation methods  have  been faced with drawbacks such as poor search capabilities and long  iteration  times.  Therefore,  scholars  have  proposed  to use  knowledge  from  the  field  of  deep  learning  to  solve vehicle routing optimisation problems.

The  use  of  neural  networks  to  solve  combinatorial optimization  problems  dates  back  to  1985,  when  Hopfield and  Tank  used  Hopfield  networks  to  solve  small  TSP instances [4] . Subsequently, for the combinatorial optimization problem, Vinyals designed an offline learning-based pointer network  model  by  breaking  the traditional online learning approach [5] ; Nazari et al. replaced the pointer network with an Long-Short Term Memory(LSTM) network to efficiently compute the updated embedding after a state change [6] ; Ashish et al. proposed an attention  mechanism-based  transformer  model  to  address the shortcomings  of  low  parallelism  and  weak  context dependency  of  neural  network  models [7] ; Nowak  et  al. trained  graph  neural  networks  in  a  supervised  manner  and used  beam  search  to  solve  the  routing  problem [8] .  Wouter proposed  a  transformer  model  with  rollout  baselines  to remove  positional  embeddings  for  a  variety  of  routing optimization problems, drawing on the transformer model [9] . Kaempfer and Wolf trained a model based on the transformer architecture and output the results to the solution process of multiple TSP problems simultaneously [10] .

There  is  also  some  relevant  literature  on  the  use  of reinforcement learning methods to solve combinatorial optimization application problems in China. Jianfeng Ren et al.  proposed  a  reinforcement  learning  genetic  ant  colony algorithm for a shop floor handling robot [11] ; Qijie Zou et al. proposed a reinforcement learning-driven local routing replanning method for fast exploration of random trees for a mobile robot in an unknown environment  [12] .

In summary, although a large number of research results have been achieved on combinatorial optimisation problems, there  is  little  research  on  their  application  to  real-world scenarios, so the application of reinforcement learning methods  to  more  realistic  scenarios  should  be  a  research priority.

## II. MATHEMATICAL MODELS

## A. Description of the problem

Assuming that the distribution task is jointly completed by  a  number  of  distribution  centres  zoning  distribution services,  the  task  content  is  based  on  the  vehicle  capacity constraints,  each  distribution  centre  sends  a  number  of vehicles  at  a  time,  in  the  composition  of  multiple  routing conditions, while meeting the current order distribution task and routing optimal requirements, the distribution process is shown in Figure 1.

Fig. 1. Distribution diagram

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[jiang2022]_Research_on_parallel_distribution_routing_optimization_based_on_improved_reinforcement_learning_algorithm_artifacts/image_000000_8c4ca04437e97a4938981e04e2d97585e403892b3293665b8f816f8702fc5201.png)

## B. Model construction

The  distribution  task  studied  in  this  paper  is  jointly accomplished  by  multiple  distribution  centres  delineating regions,  and  the  number  of  distribution  points  in  each distribution centre is unequal. The model is simplified to a distribution  model  of  a  single  distribution  centre,  with  the minimum driving distance as the optimization objective, and in  order  to  facilitate  model  construction  and  subsequent calculations, the relevant variables are assumed as follows.

## 1) Related parameters

The distribution network is denoted by, where } , , , , { 2 1 0 L v v v v V … … = &gt; =&lt; E V G , denotes  the  set  of  nodes, 0 v denotes the location of the distribution centre, } is { \ 0 v V the set of distribution points, L is the number of distribution  points;  both  the  distribution  centre  and  the distribution points are denoted by points i and j is the K maximum  number  of  vehicles  owned  by  the  distribution centre, c is the maximum loading capacity of the distribution vehicles, } , 2 {1 K k  ， ， ∈ is the number of distribution vehicles, the demand for goods i at the distribution  point  is i d , c d i &lt; ; the  transportation  cost from  the i place to j the place is ij w , ijk c is The remaining  capacity  of  the  vehicle k from  the  distribution point i to the distribution point j .

## 2) Decision-making variables

   = otherwise , 0 j node on distributi to i node on distributi from k Vehicle , 1 ijk x

   = otherwise , 0 k by vehicle delivered is i node on Distributi , 1 ik y

## 3) Target function

$$\min M = \sum _ { k = 1 } ^ { K } \sum _ { i = 0 } ^ { L } \sum _ { j = 0 } ^ { L } x _ { i j k } w _ { i j } \quad \quad ( 1 )$$

## 4) Constraints

$$k \leq K$$

$$\sum _ { i = 1 } ^ { L } d _ { i } y _ { i k } \leq c, \ \forall k \in K \quad \begin{array} { c c c } ( 3 ) & \text{atte} \\ \text{alg} \\ \text{foll} \end{array}$$

$$\sum _ { k = 1 } ^ { K } y _ { i k } = 1, \quad \forall i$$

$$\sum _ { j = 1 } ^ { L } x _ { i j k } = y _ { i k }, \quad \forall i, k$$

$$c _ { 0 j k } = x _ { 0 j k } c, \ \forall j \in L, k \in K$$

$$\sum _ { k = 1 } ^ { K } x _ { i j k } ( c _ { i j k } - d _ { j } ) \geq 0, \ \forall i \in L, k \in K \quad ( 7 )$$

$$\sum _ { i, j = s, s } x _ { j = j } \leq | S | - 1, \ \ S \subset \{ 1, \dots \cdots, L \} a n d S \neq \varnothing \forall k \quad ( 8 )$$

$$x _ { _ { \psi k } } = 0 o r 1 \quad \forall i, k \quad y _ { _ { \psi k } } = 0 o r 1 \quad \forall j, k \quad ( 9 )$$

The objective function (1) indicates that the total distance travelled by the vehicle in completing the distribution  task  is  the  shortest;Equation  (2)  indicates  that the  vehicles  delivered  will  not  be  larger  than  the  total number  of  vehicles  in  the  distribution  centre;Equation  (3) indicates  that  the  amount  of  goods  transported  by  each vehicle  does  not  exceed  its  maximum  load;  Equation  (4) indicates  that  each  distribution  point  is  guaranteed  to  be visited by only one vehicle; Equation (5) indicates that each distribution point is visited only once;Equation (6) indicates that each  vehicle from  the distribution centre is fully loaded;Equation (7) indicates that before entering any distribution point, there is enough goods on board to supply that distribution point;Equation (8) indicates the elimination of sub-loops; and equation (9) indicates the range of values of the variables.

## III. MODEL DESIGN

Fig. 2. Coding and decoding diagram

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[jiang2022]_Research_on_parallel_distribution_routing_optimization_based_on_improved_reinforcement_learning_algorithm_artifacts/image_000001_fa5134f38abd478042ce41ebdb7ada0732839dc67d62fb33d56ac8d02a43d865.png)

The  model  selects  the  transformer  model  based  on  the attention mechanism as the encoder-decoder, and the algorithm  is  modelled  for  the  research  problem,  with  the following specific ideas: the transformation of unstructured

data  to  structured  data  is  achieved  by  using  placeholder expansion nodes, thus allowing the coding input in the batch to be a different number of distribution points; the decoding output of each batch is achieved by masking the placeholders  as  distribution  points  corresponding  to  the original  information  encoded  into  the  batch.  Finally  the model is solved based on Markov Decision Process (MDP), with the structure shown in Figure 2.

## A. Encoder structure

The analogy of the distribution scenario simulated in this paper is the CVRP model with the coding structure.

## 1) Placeholder expansion

In  the  distribution  scenario  of  logistics,  the  number  of distribution  points  covered  by  each  distribution  centre  is inconsistent, therefore, a new placeholder encoding mechanism  is  proposed.  The  principle  is  to  determine  the length  of  the  longest  number  of  distribution  points  in  an irregular  batch  distribution  point  array,  and  fill  the  batch distribution point array with placeholders by gaps with the longest length.

## 2) Embedding process

Firstly,  the  data  is  mapped  to  high  dimension  as  the encoding  input  and  the  data  is  stitched  together;  the  final embedded information value is obtained by high dimensional linear projection.

## 3) Multi-headed attention mechanism

Firstly calculate the relevance between nodes and neighbouring  nodes  after  calculating  each  picking  node feature  information,  according  to  the  compatibility,  use  to calculate the attention weight, i.e. the vector received by the node, subsequently calculate the distribution node's relevance  information  for  the  whole  network,  and  finally stitch the node relevance from different dimensions.

## 4) Regularisation processing layer and forward feedback layer

In order to prevent the model complexity from increasing  and  overfitting  due  to  too  many  parameters,  a skip connection and batch normalisation are added to each sub-layer of the model.

## B. Decoder structure

## 1) Distribution information embedding

The encoded input and node information are mapped to a high-dimensional space for data embedding.

## 2) Multi-headed attention mechanism

Acting on the same  principle as the multi-headed attention mechanism in the encoding process, the correlation information between the distribution nodes is calculated on the basis of the embedding layer information of the encoding output.

## 3) Masking mechanism

The mask in the original model is generated during the computation  of  the  context  when  using  the  multi-headed attention  mechanism.  Its  functions  are:  (1)  to  mask  the distribution  points  that  have  been  visited  to  improve  the decoding efficiency; (2) the decoder observes the mask and can understand which distribution points have been visited.

Algorithm 1: Improved reinforcement learning algorithm

Input: coordinates and demand for distribution centres and distribution points Output: sequential sequence of points for distribution

|   Output: sequential sequence of points for distribution | Output: sequential sequence of points for distribution                                                                                                  |
|----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                        1 | Initialising the transformer and rollout evaluation network model parameters Initialisation of optimiser parameters and initialisation of learning rate |
|                                                        2 | For i=1: E do-                                                                                                                                          |
|                                                        3 | Initialization parameters. θ θ θ ← BL ，                                                                                                                 |
|                                                        4 | } , , 1 { ) 1234 ( Batch i random s i   ∈ ∀ ←                                                                                                         |
|                                                        5 | } , , 1 { ) , ( Batch i p s rollout i i   ∈ ∀ ← θ π                                                                                                   |
|                                                        6 | } , , 1 { ) , ( Batch i p s rollout i BL i   ∈ ∀ ← θ π                                                                                                |
|                                                        7 | For do N n  ， 1 =                                                                                                                                      |
|                                                        8 | Initialization time step. 0 ← t                                                                                                                         |
|                                                        9 | Placeholder expansion                                                                                                                                   |
|                                                       10 | Repeat                                                                                                                                                  |
|                                                       11 | Selection of sequence points based on probability distribution ) , | ( 1 n t n t n t X Y y p + n t y 1 +                                                |
|                                                       12 | Observing the new state n t X 1 +                                                                                                                       |
|                                                       13 | 1 + ← t t                                                                                                                                               |
|                                                       14 | Until the termination conditions are met                                                                                                                |
|                                                       15 | Calculating the objective function for training ) , ( ) ( 0 n n i X Y L L = π                                                                           |
|                                                       16 | Calculating the evaluation value of the rollout baseline ) ( BL i L π                                                                                   |
|                                                       17 | To find the gradient for the model network. ) | ( log )) ; ( ) ( ( 1 0 1 0 ' n n N n n BL i T i t t X Y P X L L N n ∑ ∑ = = ∇ - ← ∇ θ σ π θ             |
|                                                       18 | For transformer model network gradient updates. ) , ( θ θ θ ∇ ← Adam                                                                                    |
|                                                       19 | End for                                                                                                                                                 |
|                                                       20 | If one-sided paired t-test then                                                                                                                         |
|                                                       21 | θ θ ← BL                                                                                                                                                |
|                                                       22 | End if                                                                                                                                                  |
|                                                       23 | End for                                                                                                                                                 |

## IV. EXAMPLE SIMULATION

This section simulates a vehicle scheduling problem for multiple distribution centres in a geographical area. In order to  improve  the  broad  applicability  of  the  algorithm  and  to more closely match real-life distribution tasks, a transformer model with a rollout baseline is used and combined with a new coding mechanism to solve the problem in the simulated  scenario.  In  this  section,  the  distribution  points are  first  divided  into  different  number  of  groups,  and  then the  route  planning  is  performed  in  one  go  based  on  the parallel computing capability of the model using the reinforcement learning algorithm proposed in this paper.

Example Description: Suppose there are four distribution centres in a district responsible for the distribution of the whole area of the district, and the number of  customers  in  each  distribution  centre k . so  that  the transport routes of the vehicles in the district are arranged in a  reasonable  manner  so  that  the  total  transport  mileage  is minimised  while  meeting  the  constraints  of  the  vehicle capacity.

To increase the readability of the solution sequence, the distribution route map for the zone, is shown in Figure 3.

Fig. 3. Route allocation map

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[jiang2022]_Research_on_parallel_distribution_routing_optimization_based_on_improved_reinforcement_learning_algorithm_artifacts/image_000002_7f746d3302d8d52de0fff76c0914ca43c831bcc2990f9d5ef36baded76e6b3bb.png)

V. ALGORITHM COMPARISON

The  experiments  in  this  paper  were  all  implemented using  Python  3.6,  pycharm  2019.1.3  with  pytorch  =  0.4.1. The  computer  configuration  used  was:  Intel(R)  Core(TM) i5-6300U  CPU@2.40GHz,  2  cores,  4  logical  processors, 12GB of RAM 64-bit Windows 10 operating system.

In  order  to  analyse  the  effectiveness  of  the  proposed model,  two  different  methods  from  the  Google  OR-Tools heuristic, namely the ARC OR \_ with CHR OR \_ algorithm, as  well  as  the  ant  colony  algorithm  (ACO),  the  simulated annealing  algorithm  (SA)  and  the  reinforcement  learning algorithm (RL) adopted in this paper, were chosen to carry out a comparative analysis in the length dimension and the time dimension in 50 groups of nodes with numbers 10, 20 and 50, respectively , is shown in table I.

TABLE I ALGORITHM COMPARISON TABLE

| Projects   | Projects   |   Time (s) |   Distance | Percentage   |
|------------|------------|------------|------------|--------------|
| 10nodes    | RL         |     0.5176 |     5.9465 | 2.93%        |
| 10nodes    | OR_ARC     |     1.2618 |     5.7774 | 0%           |
| 10nodes    | OR_CHR     |     1.5676 |     5.8106 | 0.57%        |
| 10nodes    | ACO        |     8.3392 |     6.2123 | 7.53%        |
| 10nodes    | SA         |    25.8294 |     6.2396 | 8%           |
| 20nodes    | RL         |     1.2401 |     8.2503 | 3.19%        |
| 20nodes    | OR_ARC     |     3.0326 |     7.9954 | 0%           |
| 20nodes    | OR_CHR     |     4.3381 |     8.3318 | 4.21%        |
| 20nodes    | ACO        |    48.5322 |     8.9836 | 12.3%        |
| 20nodes    | SA         |    55.8508 |     8.8497 | 10.6%        |
| 50nodes    | RL         |     2.8708 |    14.0502 | 2.47%        |
| 50nodes    | OR_ARC     |    23.2097 |    13.7108 | 0%           |
| 50nodes    | OR_CHR     |    25.5549 |    14.1306 | 3.06%        |
| 50nodes    | ACO        |   387.348  |    15.8338 | 15.4%        |
| 50nodes    | SA         |   189.581  |    16.1939 | 18.11%       |

## VI. CONCLUSION

In  the  current  IoT  mode,  optimizing  the  distribution management system, establishing and improving the intelligent distribution system, accelerating the development towards unmanned and intelligent transformation is the key direction  of  reform  and  development  of  e-commerce  and express delivery enterprises.

To  address  the  vehicle  scheduling  problem  of  multiple distribution centres in a geographical area, this paper proposes  a  reinforcement  learning  algorithm  using  a  new coding mechanism to solve the routing optimisation problem  of  CVRP  with  irregular  inputs.  The  experimental results  show  that  the  proposed  method  has  strong  solution efficiency in terms of solution accuracy and solution time in the routing length, time dimension and structure dimension, and  has  the  advantage  of  variable  input and  avoiding re-referencing  in  the  structure  dimension.  Therefore,  the improved  reinforcement  learning  method  is  very  effective for solving the CVRP problem with irregular inputs, and can effectively solve the logistics distribution problem  and reduce logistics transportation costs.

In  this  paper,  we  propose  a  solution  idea  based  on reinforcement  learning  algorithm  for  a  many-to-many  task distribution scenario under a simple model, which provides a new solution to solve such problems. In the future, we will conduct  more  in-depth  research  on  the  joint  distribution scenario between multiple distribution centres and multiple distribution points.

## REFERENCES

- [1] Metropolis. N, Rosenblith. A. W, Rosenblith M N, et al. Equation of state  calculations  by  fast  computing  machines.  Journal  of  Chemical Physics, 1953, 21(6), 1087~1092.
- [2] Kirkpatrick.  S,  Gelatt.  J,  Vecchi  M  P.  Optimization  by  simulated annealing. Science, 1983, 220(4598), 671~680.
- [3] Zhang.  N,  Xu,  T.  X,  Lu.  C,  Han,  Y.  CVRP  problem  based  on improved ant colony algorithm. Firepower and Command  and Control,2019,4401:67-71.
- [4] John  J  Hopfifield  and  David  W  Tank.  "Neural"  computation  of decisions in optimization problems[J]. Biological cybernetics, 1985,52(3):141-152.
- [5] Oriol. Vinyals, Samy. Bengio and Manjunath Kudlur. Order matters: Sequence to sequence for sets. arXiv, 1511. 06391, 2015.
- [6] Nazari. M, Oroojlooy. A, SnyderLV, etal. Reinforcement learning for solving the vehicle routing problem// Advances in Neural Information Processing Systems, 2018,9861-9871.
- [7] Vaswani.  A,  Shazeer.  N,  Parmar.  N,  etal.  Attention  Is  All  You Need//Proceedings of the advances in Neural Information Processing Systems,2017:5998-6008.
- [8] Alex. Nowak, Soledad. Villar, Afonso. S. Bandeira and Joan Bruna. A  note  on  learning  algorithms  for  quadratic  assignment  with  graph neural networks. arXiv:1706.07450, 2017.
- [9] Kool.  W  ,  Van.  Hoof.  H  ,  Welling.  M.  Attention,  Learn  to  Solve Routing Problems. arXiv, 1803. 08475v3, 2019.
- [10] Yoav.  Kaempfer  and  Lior.  Wolf,  Learning  the  multiple  traveling salesmen  problem  with  permutation  invariant  pooling networks. arXiv,1803.09621, 2018.
- [11] Ren. J. F, Ye. C. M, Yang. F, Modeling and algorithm research on routing optimization of workshop handling robot with time window. Operations Research and Management,2020,29(05):52-60.
- [12] Zou.  Q.  J,  Liu.  S.  W,  Zhang.  Y,  Hou.  Y.  E,  A  reinforcement learning-based  algorithm  for  routing  replanning  in  RRT  special environments.  Control  Theory  and  Applications:1-12,[2020-06-15]. http://kns.cnki.net/kcms/detail/44.1240.tp.20200508.1047.047.html.