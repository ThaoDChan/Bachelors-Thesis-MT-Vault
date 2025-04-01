![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[gupta2022]_Deep_reinforcement_learning_algorithm_for_fast_solutions_to_vehicle_routing_problem_with_timewindows_artifacts/image_000000_662e2980fcb35349de2e0ad624ddadf4fd2c7ff3dc216272dca79d567e419d68.png)

## Deep Reinforcement Learning Algorithm for Fast Solutions to Vehicle Routing Problem with Time-Windows

Abhinav Gupta TCS Research New Delhi, India abhi.gupta6@tcs.com

Supratim Ghosh TCS Research New Delhi, India supratim.ghosh2@tcs.com

Anulekha Dhara TCS Research New Delhi, India anulekha.dhara@tcs.com

## ABSTRACT

Vehicle routing problem (VRP) is a well known NP-hard combinatorial optimization problem having several variants. In this paper, we consider VRP along with additional constraints of capacity and time-windows (CVRPTW) and aim to provide a fast and approximately optimal solutions to large-scale CVRPTW problems. We present a deep Q-network with encoder-decoder based reinforcement learning approach to solve CVRPTW. The encoder is based on the attention mechanism whereas decoder is fully connected neural network. Via numerical experiments on benchmark datasets, we show the efficacy and computational speed our approach compared to baseline heuristics, a meta-heuristic algorithm, and a multi-agent reinforcement learning (RL) based framework.

## CCS CONCEPTS

- · Computing methodologies → Reinforcement learning ; · Applied computing → Multi-criterion optimization and decision making ; Transportation .

## KEYWORDS

reinforcement learning, capacitated vehicle routing, time-windows

## ACMReference Format:

Abhinav Gupta, Supratim Ghosh, and Anulekha Dhara. 2022. Deep Reinforcement Learning Algorithm for Fast Solutions to Vehicle Routing Problem with Time-Windows. In 5th Joint International Conference on Data Science &amp; Management of Data (9th ACM IKDD CODS and 27th COMAD) (CODSCOMAD 2022), January 8-10, 2022, Bangalore, India. ACM, New York, NY, USA, 5 pages. https://doi.org/10.1145/3493700.3493723

## 1 INTRODUCTION

The advent of pandemic has accelerated the process of shifting the shopping habits of consumers from traditional brick and mortar retail stores to online e-commerce websites. With these accelerating trends, one of the critical challenges faced by the e-commerce and postal industries is the transportation and delivery of orders to a large number of consumers (hundreds to potentially thousands in a day or shift). It is estimated that upto 50% of transportation costs are reserved for the last mile segment [10]. At its core, the last

Permission to make digital or hard copies of all or part of this work for personal or classroom use is granted without fee provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the full citation on the first page. Copyrights for components of this work owned by others than ACM must be honored. Abstracting with credit is permitted. To copy otherwise, or republish, to post on servers or to redistribute to lists, requires prior specific permission and/or a fee. Request permissions from permissions@acm.org.

CODS-COMAD 2022, January 8-10, 2022, Bangalore, India

© 2022 Association for Computing Machinery.

ACM ISBN 978-1-4503-8582-4/22/01...$15.00

https://doi.org/10.1145/3493700.3493723

mile delivery is a vehicle routing problem (VRP) [6] which involves finding optimal routes to distribute goods from depot to customers while minimizing the total cost incurred and is well-known to be NP-hard [8]. Adding extra constraints on vehicle capacity and time windows (TW) makes the problem even harder [13].

The problem of capacitated vehicle routing with time-windows (CVRPTW) can be considered with different objectives. Most of the cost functions include some combination of manpower, distance, and time related costs [9]. Based on discussions with our client partner, we have come to understand that the cost components are directly related either to the total number or the total distance travelled by the vehicles in operation. While the former affects the manpower related costs; other components such as cost of fuel are affected by a combination of both.

The CVRPTW has widespread attention from people in operations research community due to its ubiquity in real-world applications. A number of solutions using various techniques exist: ranging from mixed integer linear programs (MILP) providing exact solutions [5, 14] to meta-heuristics involving involving either tabu search, genetic algorithms, neighborhood search, or ant colony optimization [4, 7, 12, 17, 20]. While the MILP-based approaches produce optimal, and the meta-heuristic based ones produce nearly -optimal solutions, these are not scalable to problems involving hundreds to potentially thousands of customers. Solving such largescale problems in quick time with a reasonable degree of accuracy thus, still remains an open challenge prompting the need for learning-based approaches.

Recent advances in RL have motivated the research community to look at its applicability in solving large-scale combinatorial optimization problems [2] such as TSP (travelling salesman problem: VRP with just one vehicle) and CVRP. For e.g., the TSP has been addressed via pointer networks [22] in [1, 15]. A simplified version of pointer network was used for CVRP [16] by replacing the LSTM encoder of the pointer network by element-wise projections. An alternate approach is the use attention mechanism with the encoder-decoder framework [11]. These approaches solve the CVRP problem by starting out with one vehicle, and subsequently adding more when any of the constraints for the vehicles in operation are violated. The problems of TSPTW and CVRPTW have also received attention in recent times [3, 15, 21]. The DQN-cumheuristic approach in [21] produces reasonably optimal results for CVRPTW (up to 60% optimality gap). On the other hand, [3] uses a multi-agent RL (MARL) approach with attention mechanism to produce solutions at least 10 times faster than [21]; albeit with a higher optimality gap. In addition, [3] uses soft time-windows which may result in routes involving violations of TW constraints.

Our goal in this article is to present an algorithm for fast and approximate solutions to CVRPTW using a combination of deep-RL and heuristics. Our approach aims to have the computational speeds similar to or better than that of [3] while closing the optimality gap as much as possible. Our primary focus is to reduce the number of vehicles in operation so that overall cost of transportation can be reduced. To this end, we generate the initial set of routes using a deep-RL based approach using attention mechanism, and postprocess the solution using heuristics.

## 2 PROBLEM FORMULATION

We consider a single depot multi-customer CVRPTW problem as a graph G = ( V E , ) where the set of nodes V = { 0 , . . . , n } denote the customers and the depot, and the set of edges E ⊂ V × V denote the paths connecting the customers and the depot. We denote the depot node by i = 0. Let the depot have a fleet of m vehicles to complete the deliveries of n customers. Each customer node i ∈ V has the following features: the location ( x i , y i ) , the weight of the demand order w i , the start time ts i and end time te i within which the service should start i.e., TW ( ts i , te i ) , ts i &lt; te i and the service time s i . For simplicity, we consider a homogeneous fleet of vehicles i.e., all the vehicles have the same capacity and operate at the same speed. Let the maximum vehicle capacity be D with speed v . All the vehicles start and end their routes at the depot. Also we assume that there is a path between all the nodes i.e., G is fully connected. The weight of each edge consists of the distance d i j between the i -th and j -th node with travel time t i j = d i j / v where v is the speed.

We consider TW of the customer as a hard constraint i.e., the vehicle cannot start servicing the delivery order to the customer outside this time window. If it were a soft constraint, then a vehicle is allowed to reach the customer location after the end time and still service the customer with a lateness penalty. Also similar to the vehicle capacity, restrictions on limit of total driving distance and time exist for each vehicle. Moreover, extensions like heterogeneous fleet of vehicles can be considered using different capacities D i j and different speeds v i j for the vehicles.

Furthermore, we assume that a customer is visited only once. So, a customer which is already serviced by a vehicle will not be considered as the feasible customer candidate for other vehicles. For a customer i to be a feasible candidate to be visited by a vehicle j , the following conditions need to be satisfied: the vehicle j should be able to visit customer i within TW ( ts i , te i ) after finishing the service of the current customer. In case, j is not currently servicing a customer and is just starting from the depot, then it should be able to reach customer i 's location before ts i . If vehicle j reaches customer i before ts i , then it has to wait until ts i before the servicing can start. Also, the total demand of a vehicle route cannot exceed the maximum vehicle capacity.

## 3 METHODOLOGY

The problem is modeled as a Markov Decision Process (S A R , , , γ ) where the state S represents the states, A the set of actions, R denotes the reward function, and γ is the discount factor. In the RL setup, at a given time-step t , the agent receives the current state s t from the environment. Based on the policy, the agent takes an action a t and receives a reward r t . The objective is to maximize the

Abhinav Gupta, Supratim Ghosh, and Anulekha Dhara

Table 1: State inputs at time step t

| Notation        | Explanation                                                 |
|-----------------|-------------------------------------------------------------|
| d i j           | Distance between current customer i and feasible customer j |
| [ ts j , te j ] | Time-window of the feasible customer j                      |
| tw j            | Waiting time before servicing feasible customer j           |
| w j             | Weight of the feasible customer j demand                    |
| T t             | Total time travelled the vehicle                            |
| W t             | Total vehicle capacity filled                               |

expected cumulative discounted reward E GLYPH&lt;0&gt; ˝ ∞ i = t γ t r t GLYPH&lt;1&gt; . We model a value-based deep Q-network (DQN) model having an encoderdecoder framework.

Based on the feasibility conditions, the customers violating these conditions are masked for the active vehicle under consideration. In this approach, multiple vehicles start from the depot and create multiple trips simultaneously. The initial set of vehicles to start with is based on the minimum number of vehicles required to satisfy the total customer demand. Due to TW constraints, the actual number of vehicles required may be more than the minimum number calculated. At a given time-step, the q-values are calculated simultaneously for the active vehicles and the corresponding feasible customers which satisfy the feasibility check for the vehicle to decide on the action to be taken.

After a candidate solution is generated by the deep-RL, it is further refined by an insertion heuristic. In this procedure, the routes with the least number of customers are broken up and each node in these routes is checked for feasibility by inserting them between various nodes in the remaining routes. Whenever such feasible locations are found, the one with the least marginal increase in distance is chosen and the node is inserted to form a new route.

## 3.1 States and actions

At a given time-step t , the features associated with the active vehicles and their corresponding feasible customers are used as state inputs S to obtain the q-values between the vehicle-customer pairs. The action space A associated with an active vehicle consists of all the feasible customer nodes for this vehicle which have not been serviced by any other vehicle yet. The action taken by an active vehicle is the feasible customer node which it will visit depending on the q-values. For the active vehicle k , q-values are calculated for all the feasible customers and the customer having the maximum q-value is selected as the next node to be visited by the vehicle. In case of conflict wherein two active vehicles choose the same customer to be serviced at the next time-step, the vehicle-customer pair which has the higher q-value is given priority. The other active vehicles at that time-step are allocated the feasible customer having the next best q-value. Therefore, the action taken at this time-step is the collection of all the vehicle-customer pair for the current active vehicles. Some of the important state inputs are given in Table 1. The distance d i j is not only between the current and feasible customer nodes, it also includes the depot node 0. While calculating the q-values, the state inputs are normalized. The distance related inputs are normalized by the diagonal or the maximum range of

Table 2: Reward terms

| Term        | Explanation                                                                         |
|-------------|-------------------------------------------------------------------------------------|
| d ( k , t ) | Distance travelled by vehicle k at time step t                                      |
| s ( k , t ) | Time remaining in the time-window after service is done by vehicle k at time-step t |
| w ( k , t ) | Waiting time for vehicle k at time step t before it starts servicing                |
| F           | Fulfillment ratio                                                                   |

the xy -coordinates, time related inputs by the closing time of the depot, and the weight related inputs by the vehicle capacity.

## 3.2 Vehicle-customer logic

To allocate the first customer to the active vehicles starting from the depot, we use a heuristic motivated by circle covering method in [18]. The customers are sorted based on increasing start time and then increasing distance in case of conflict. The first customer in the sorted list is assigned to one of the active vehicles starting from depot. With this customer as center, a cluster is created by adding the remaining customers in the system based on the nearest neighbor condition with respect to this customer node. The customers are added to the cluster until the total weight of the customer demands in the cluster exceeds the vehicle capacity. Once the cluster is formed, the next best customer in the sorted list which is not in the cluster is associated with the next active vehicle from the depot and the process continues. The clustering is done so that the initially active vehicles do not start serving customers which are in the immediate vicinity of each other (these customers can potentially be served by the same vehicle). After all the active vehicles starting from depot have been assigned their first customer, the q-values obtained from the model are used to decide on the next customer node to be visited and resolve any conflict in customer assignment between two or more vehicles. For an active vehicle, if no feasible customer is left, it returns to the depot. After all the initial set of active vehicles have completed their trips, if there are still customers waiting to be services and there are unutilized vehicles left at the depot, a new vehicle is introduced to service these remaining customers. If there is no unused vehicle left, the customers remain unfulfilled.

## 3.3 Rewards

The overall goal is to minimize the number of vehicles used in the operation and the total distance travelled by the vehicles while satisfying the feasibility constraints discussed in previous section. Since the decision regarding which action should be taken by the active vehicle happens locally at every time-step with the aim of fulfilling the global objective, the local step rewards and the global terminal reward are considered. The terms involved in the reward structure are given in Table 2. At any time-step, the distance travelled to the next customer should be minimum. Based on the distance, the vehicle may reach the next proposed customer well in advance and needs to wait for hours, the time in which some other feasible customer could have been served. So it is desirable to also reduce the waiting times for vehicles. Moreover, if the delivery

Figure 1: Training performance in terms of mean reward

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[gupta2022]_Deep_reinforcement_learning_algorithm_for_fast_solutions_to_vehicle_routing_problem_with_timewindows_artifacts/image_000001_0215be079b9be72e9c71caea84654dc3f9af59aa2cdcf7b607c667915a047106.png)

service is finished for the proposed customer before the end of TW, it is advantageous as the vehicle becomes available to serve other feasible customers and hence a positive reward is provided in such a case. The overall step reward involves the weighted combination of these terms:

$$\mathcal { R } _ { s t e p } ( k, t ) = - a _ { 1 } d ( k, t ) + a _ { 2 } s ( k, t ) - a _ { 3 } w ( k, t ).$$

The global reward involves the fulfillment ratio F , that is the number of customers serviced with respect to the total customers n . Therefore the total weighted reward for the instance is given by

$$\mathcal { R } = \sum _ { k = 1 } ^ { m } \sum _ { t = 1 } ^ { T } \mathcal { R } _ { s t e p } ( k, t ) + \alpha \mathcal { F }.$$

Here, the following weights were considered: a 1 = 0 3, . a 2 = 0 5, . a 3 = 1 and α = 0 95. .

## 3.4 Model architecture

Neural network with experience replay is used to train the RL agent. The network involves graph attention model as the encoder and fully connected layers as the decoder. The graph attention encoder is a multi-head attention model with 8 heads and 2 self attention layers. We have chosen the embedding size as 90. The decoder consists of 2 linear layers in which layer normalization is applied after first layer followed by ReLU activation. The output layer uses linear activation. The normalized input is passed to the encoderdecoder network to obtain the final q-values corresponding to each action (here customer), which helps in deciding the action to be chosen. While calculating the q-values for the vehicle-customer pair with respect to the current active vehicle, the customers violating the feasibility conditions are masked and only feasible customers are considered. The network is trained using SGD optimizer with a learning rate of 0.01 and uses MSE loss. The experience replay buffer used for training the model is of size 100000 with a batch size of 32.

## 4 EXPERIMENTS

## 4.1 Training data

We train the neural network model using 20 randomly generated datasets with 25 customers and 13 vehicles. The customer location is uniformly generated over [-100 100 , ] × [-100 100 , ] while the depot location is from [-25 25 , ] × [-25 25 . The vehicle capacity , ]

Table 3: Output for Solomon datasets (#C: number of customers; Cl: dataset class and type; RH: our RL+Hueristic model; GA: Genetic Algorithm based meta-heuristic; G1: Greedy heuristic with distance priority; G2: Greedy heuristic with time-window priority; M: MARDAM; * v : number of vehicles used; * d : total distance travelled; * t : computational time (in seconds)

| #C   | Cl   |   GA v |   GA d |   GA t |   RH v |    RH d |   RH t |   M v |     M d |   M t |   G1 v |    G1 d |   G1 t |   G2 v |    G2 d |   G2 t |
|------|------|--------|--------|--------|--------|---------|--------|-------|---------|-------|--------|---------|--------|--------|---------|--------|
|      | C1   |   3    | 191.1  |  19.64 |  3     |  283.06 |  0.668 | 3     |  404.83 |  0.6  |   5.44 |  379.61 |   1.15 |   3    |  318.21 |   1.08 |
| 25   | R1   |   4.75 | 469.82 |  54.16 |  4.91  |  619.21 |  2.42  | 5     |  658.8  |  2.35 |   9.08 |  753.47 |   1.91 |   6.5  |  723.84 |   1.61 |
|      | RC1  |   3.25 | 351.11 |  35.05 |  3.25  |  380.85 |  0.74  | 3.5   |  459.21 |  1.72 |   6.5  |  649.42 |   1.17 |   5.5  |  737.84 |   1.9  |
|      | C1   |   5    | 362.57 |  82.78 |  5.67  |  676.65 |  2.19  | 5.55  | 1077.78 |  2.24 |   9.66 |  778.56 |   2.52 |   6.22 |  896.35 |   2.41 |
| 50   | R1   |   7.75 | 785.53 | 147.76 |  8.58  | 1114.45 |  2.57  | 8.83  | 1219.85 |  2.71 |  14    | 1192.57 |   3.43 |  11.25 | 1373.09 |   3.29 |
|      | RC1  |   6.5  | 741.31 |  98.19 |  7.87  | 1011.87 |  2.84  | 8.37  | 1219.79 |  3    |  11.62 | 1319.05 |   2.61 |  10.37 | 1477    |   3.91 |
|      | C2   |   1.12 | 245.44 |  17.65 |  1.125 |  301.18 |  0.52  | 1.125 |  297.03 |  0.4  |   6    |  485.15 |   1.12 |   1.75 |  328.3  |   1.2  |
| 25   | R2   |   1.36 | 420.62 |  29.08 |  1.27  |  539.68 |  0.79  | 1.27  |  540.42 |  0.78 |   4.36 |  577.98 |   1.36 |   2    |  779.14 |   1.3  |
|      | RC2  |   1.62 | 365.14 |  28.38 |  1.25  |  526.37 |  0.85  | 1.25  |  598.26 |  0.91 |   4    |  593.6  |   0.96 |   2.12 |  912.91 |   1.19 |
|      | C2   |   2    | 401.18 | 133.32 |  2     |  775.59 |  2.69  | 2     |  926.14 |  2.44 |   7.75 |  858.05 |   3.11 |   2.12 |  732.88 |   4    |
| 50   | R2   |   2.18 | 652.48 | 218.91 |  2.09  |  989.44 |  2.66  | 2.09  | 1141.03 |  2.48 |   5.54 |  970.92 |   2.92 |   3.09 | 1509.18 |   3.52 |
|      | RC2  |   2.87 | 621.21 | 174.11 |  2.25  | 1203.23 |  3.01  | 2.37  | 1350.01 |  2.78 |   6.12 | 1167.44 |   4.06 |   3.87 | 1937.88 |   2.73 |

is taken as 200 with speed 10. The customer demand is randomly chosen from an exponential distribution with β = 0 1. The start . TWis randomly generated over [ 0 200 , ] with width of TW having a Gaussian distribution.

## 4.4 Results

The training is done for 500 episodes with each episode consisting of the 20 datasets. The /uni03F5 -greedy policy is used for exploration for the first 300 episodes with /uni03F5 decaying exponentially at the end of each episode with decay rate of 0.01. This model is directly used for testing followed by post-processing using insertion heuristic to improve the solution. Figure 1 shows the performance of the model while training in terms of the mean reward.

## 4.2 Test data

We consider the Solomon benchmark datasets [20] for CVRPTW [19] consisting of 25 and 50 customers. The datasets are classified based on vehicle capacities (type 1 and 2 with type 2 having higher capacity) and customer locations (type C: clustered; type R: uniformly random; type RC: mix of clustered and random). We run the test for each class based on vehicle capacity and customer location type. The results obtained are compared with the baselines with respect to the number of vehicles used, total distance of all the trips taken together and the computational time.

## 4.3 Baselines

We compare our results obtained from the combination of RL model and insertion heuristic with the following algorithms: a metaheuristic based genetic algorithm (GA) [21], MARDAM approach [3] and greedy heuristics. The meta-heuristic baseline is based on genetic algorithms involving insertion, binary tournament selection procedure for parent selection, crossovers and mutations. The second baseline is a recent sequential multi-agent decision making actor-critic RL model to solve the CVRPTW routing problem called MARDAM (Multi-Agent Routing Deep Attention Mechanism) [3].

The greedy heuristic (G1) baseline assigns the nearest customer to the vehicle and in case of conflict, the customer with earlier start time is given preference. In an alternate greedy heuristic (G2), the customer having the earliest start time is assigned to the vehicle and in case of conflict, the one nearest to the current vehicle location is assigned. The delivery trips are generated sequentially.

Table 3 presents the results obtained by our RL-cum-heuristic (RH) approach on the test data, and compares them with the baseline algorithms mentioned in Section 4.3. The results for each row are the average of about 9 datasets for each class-type (C/R/RC) and number of customers (25/50). All computations are carried out on an Intel i5 laptop with 8 GB RAM, Ubuntu 16.04, and no dedicated GPU. In comparison, RH performs better against baselines (MARDAM, G1, and G2) both in terms of number of the number of vehicles used and total distance travelled. While, against GA, RH provides near optimal solution in terms of the number of vehicles used (with lower numbers in certain cases), at the cost of distance travelled. Moreover, RH either outperforms or performs close to all the baselines in terms of computational speeds with a few exceptions (especially against GA, by up to 2 orders of magnitude). Furthermore, the key takeaways on comparing RH with the RL model in [21] are: fewer vehicles used (by up to 20%); much faster computational times (by up to 20 times); and higher distance travelled by up to 50 -60%.

One can observe from Table 3 that there is an increase in the distance travelled for clustered datasets like (C and RC) even with 50 customers. This is due to the emphasis on reducing the waiting time in the reward structure which incentivizes a vehicle to serve customers which may be spatially distant but have temporally close TWs i.e., after a vehicle has served a customer, to avoid waiting time, it would choose to serve a potentially distant customer with a close enough TW.

## 5 CONCLUSIONS

In this paper, we presented an approach for the solving the CVRPTW using a combination of value-based deep-RL model involving attention mechanism and heuristics. Via numerical simulations on benchmark datasets, we showed that this approach performs faster and uses same/fewer vehicles than several baselines and other methods in literature, albeit with a higher optimality gap in terms of total distance travelled. Currently work is in progress to close this optimality gap; and extend this framework to problems involving hetereogeneous vehicles and dynamic routing problems where the customer details are not known a priori .

## REFERENCES

- [1] Irwan Bello, Hieu Pham, Quoc V. Le, Mohammad Norouzi, and Samy Bengio. 2016. Neural Combinatorial Optimization with Reinforcement Learning. ArXiv abs/1611.09940 (2016).
- [2] Yoshua Bengio, Andrea Lodi, and Antoine Prouvost. 2021. Machine learning for combinatorial optimization: A methodological tour d'horizon. European Journal of Operational Research 290, 2 (2021), 405-421.
- [3] Guillaume Bono, Jilles S. Dibangoye, Olivier Simonin, Laëtitia Matignon, and Florian Pereyron. 2020. Solving Multi-Agent Routing Problems Using Deep Attention Mechanisms. IEEE Transactions on Intelligent Transportation Systems (2020), 1-10. https://doi.org/10.1109/TITS.2020.3009289
- [4] Olli Bräysy. 2003. A Reactive Variable Neighborhood Search for the VehicleRouting Problem with Time Windows. INFORMS Journal on Computing 15, 4 (2003), 347-368.
- [5] Huey-Kuo Chen, Che-Fu Hsueh, and Mei-Shiang Chang. 2006. The real-time time-dependent vehicle routing problem. Transportation Research Part E: Logistics and Transportation Review 42, 5 (2006), 383-408.
- [6] G. B. Dantzig and J. H. Ramser. 1959. The Truck Dispatching Problem. Management Science 6, 1 (1959), 80-91.
- [7] Luca Maria Gambardella, Éric Taillard, and Giovanni Agazzi. 1999. MACSVRPTW: A Multiple Ant Colony System for Vehicle Routing Problems with Time Windows. In New Ideas in Optimization . McGraw-Hill, 63-76.
- [8] M. R. Garey and D. S. Johnson. 1979. Computers and intractability . Vol. 174. Freeman San Francisco.
- [9] R. Gevaers, E. Van de Voorde, and T. Vanelslander. 2014. Cost modelling and simulation of last-mile characteristics in an innovative B2C supply chain environment with implications on urban areas and cities. Procedia-Social and Behavioral Sciences 125 (2014), 398-411.
- [10] M. Joerss, J. Schroder, F. Neuhaus, C. Klink, and F. Mann. 2016. Parcel delivery: The future of last mile. McKinsey &amp; Company (2016), 1-32.
- [11] Wouter Kool, Herke van Hoof, and Max Welling. 2019. Attention, Learn to Solve Routing Problems!. In International Conference on Learning Representations .

CODS-COMAD 2022, January 8-10, 2022, Bangalore, India

| [12]   | Hoong Chuin Lau, Melvyn Sim, and Kwong Meng Teo. 2003. Vehicle routing problem with time windows and a limited number of vehicles. European Journal of Operational Research 148, 3 (2003), 559-569.                                                                                       |
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [13]   | J.-Q Li, P. B. Mirchandani, and D. Borenstein. 2009. Real-time vehicle rerouting problems with time windows. European Journal of Operational Research 194, 3 (2009), 711-727.                                                                                                             |
| [14]   | Wenli Li, Kunpeng Li, P N Ram Kumar, and Qiannan Tian. 2021. Simultaneous product and service delivery vehicle routing problem with time windows and order release dates. Applied Mathematical Modelling 89 (2021), 669-687.                                                              |
| [15]   | Qiang Ma, Suwen Ge, Danyang He, Darshan D. Thaker, and Iddo Drori. 2019. Combinatorial Optimization by Graph Pointer Networks and Hierarchical Rein- forcement Learning. ArXiv abs/1911.04936 (2019).                                                                                     |
| [16]   | Mohammadreza Nazari, Afshin Oroojlooy, Martin Takáč, and Lawrence V. Snyder. 2018. Reinforcement Learning for Solving the Vehicle Routing Problem. In Proceedings of the 32nd International Conference on Neural Information Processing Systems (Montréal, Canada) (NIPS'18) . 9861-9871. |
| [17]   | Christian Prins. 2004. A simple and effective evolutionary algorithm for the vehicle routing problem. Computers & Operations Research 31, 12 (2004), 1985- 2002.                                                                                                                          |
| [18]   | M.W.P. Savelsbergh. 1990. A parallel insertion heuristic for vehicle routing with side constraints. Statistica Neerlandica 44, 3 (1990), 139-148.                                                                                                                                         |
| [19]   | SINTEF. 2008. Solomon Benchmark . https://www.sintef.no/projectweb/top/ vrptw/solomon-benchmark/                                                                                                                                                                                          |
| [20]   | Marius M. Solomon. 1987. Algorithms for the Vehicle Routing and Scheduling Problems with Time Window Constraints. Operations Research 35, 2 (1987), 254-265.                                                                                                                              |
| [21]   | Nazneen N. Sultana, Vinita Baniwal, Ansuma Basumatary, Piyush Mittal, Supra- tim Ghosh, and Harshad Khadilkar. 2021. Fast Approximate Solutions using Reinforcement Learning for Dynamic Capacitated Vehicle Routing with Time Windows. ArXiv abs/2102.12088 (2021).                      |
| [22]   | Oriol Vinyals, Meire Fortunato, and Navdeep Jaitly. 2015. Pointer Networks. In Advances in Neural Information Processing Systems , C. Cortes, N. Lawrence, D. Lee, M. Sugiyama, and R. Garnett (Eds.), Vol. 28. Curran Associates, Inc.                                                   |