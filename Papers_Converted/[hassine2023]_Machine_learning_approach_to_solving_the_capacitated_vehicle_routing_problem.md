## Machine learning approach to solving the capacitated vehicle routing problem

## Marwa Ben Hassine

## Mohamed Salim Amri Sakhri

## Mounira Tlili

VPNC Lab, Higher Institute of Transport and Logistics of Sousse, University of Sousse, Tunisia benhassinemarwa2023@gmail.com

VPNC Lab, Faculty of Law, Economics and Management Sciences, University of Jendouba, Tunisia mohamedsalim.amrisakhri@fsjegj.rnu.tn

Abstract -The Vehicle Routing Problem (VRP) is a combinatorial optimization problem of practical importance and complexity that attracts the attention of researchers. The Capacitated Vehicle Routing Problem (CVRP) is one of the best known variants of the VRP. It involves finding the optimal routes for a set of vehicles with limited carrying capacity, while satisfying customer demand deterministically. Many approaches have been proposed to solve CVRP. In our case, we have chosen a solution method that uses a powerful machine learning approach that researchers are currently investing in such concepts. We have developed two algorithms that, in a first phase, use the K-mean method to create groups of customers to be delivered and, in a second phase, use two different optimisation heuristics to find the optimal delivery route. A comparative study of computational results and execution time was carried out to determine the best approach to use in such an operational research problem.

MARS Lab, Higher Institute of Transport and Logistics of Sousse, University of Sousse, Tunisia mounira.tlili@istls.u-sousse.tn

VRP, like other combinatorial optimization problems, has been extensively studied and solved using traditional methods such as exact algorithms, specific heuristics, and metaheuristics [2]. The advent of new technologies such as Artificial Intelligence (AI) and Machine Learning (ML) has revolutionised problem solving by enabling faster and higher quality solutions. The study of solving VRPs using AI and ML methods is very recent, which prompted us to take the lead in such a solution approach. The aim of this research is to use an approach that combines ML methods and heuristics to find the map of customers to be served and the optimal route for delivery vehicles. This approach aims to solve the problem of delivering orders to a set of customers during each replenishment period. This combination of methods is an innovative approach to solving the vehicle routing problem and can potentially provide faster and better solutions.

KeywordsVehicle Routing Problem, Machine Learning, K-means algorithm, Nearest Neighbour algorithm, Branch and Cut algorithm, Optimization

## I. INTRODUCTION

Transport plays a crucial role in the economy and in business. Not only does it connect different places of production and consumption, but it also transports raw materials, finished products, goods, and people. However, transport also represents a significant cost for companies, especially when it comes to delivering products to customers. The Vehicle Routing Problem (VRP) is a major challenge for companies seeking to optimise their deliveries. This problem consists of finding the best way to serve a set of customers while minimising transport costs, given certain logistical constraints such as delivery times and vehicle load capacities, [1]. Although the VRP has been extensively reviewed in the scientific literature and effective for solving it have been proposed, the problem is still relevant today and represents a major challenge for companies seeking to improve their customer service while reducing their transport costs.

In this paper, we present a comprehensive review of machine learning techniques that have been applied to the challenging problem of the Capacity Vehicle Routing Problem (CVRP). Our main focus in this study is on the CVRP variant of the well-known Vehicle Routing Problem (VRP). We present a new solution method that combines ML models and heuristics, which are increasingly being used to solve operations research problems. Our approach uses the K-mean model to define clusters and two types of heuristics, the nearest neighbour (NN) algorithm and the branch and cut (B&amp;C) algorithm, to determine optimal routes. We then compare the performance of the two approaches. By incorporating the K-mean model into this problem, we clearly depart from traditional solution methods and aim to demonstrate its potential to improve the quality and speed of CVRP solutions.

The paper is structured as follows: Section 2 provides a comprehensive review of previous studies that have used machine learning models to solve the CVRP. The mathematical formulation of the CVRP is presented in Section 3. In Section 4, we explain our solution methods. The results of our computational experiments are presented in Section 5. Finally, in

Section 6, we summarise our main findings, discuss their implications, and identify potential avenues for future research.

## II. LITERATURE REVIEW

VRP belongs to the class of NP-hard combinatorial problems, which means that for large instances with a large number of parameters, it is difficult to find optimal solutions in a reasonable time. Two main categories of methods are generally used to solve this type of problem: exact methods and approximation methods. These methods have been widely used in the past and have provided good solutions for some variants of VRP. In our paper we will discuss a new category based on ML models. We have chosen to use K-means as a tool to tackle our VRP problem. This unsupervised learning model can generate clusters that effectively reduce the complexity of the problem, thereby improving the efficiency of vehicle routing. Furthermore, K-means has demonstrated its flexibility and scalability, making it a suitable solution for both small and large-scale VRP applications. In the following, we provide an overview of the results obtained by researchers who have successfully applied this model to solve the VRP problem and its variants.

Wang et al [3] proposed a machine learning approach to distribute cigarettes to a two-tier network of retailers with 100 customers. Their approach uses the K-means algorithm, an unsupervised learning technique, and a multi-stage algorithm specifically designed for large-scale VRP. At the heart of their algorithm is a custom K-means clustering method used to allocate customers to different routes. Compared to the Farthest Adjacent algorithm, their multi-stage algorithm showed significant improvements in both solution quality and performance for large-scale VRP processing. Their approach provides a promising solution for optimising VRP solutions in a practical and efficient manner.

The article by [4] presented an analysis of the well-known 100-client Solomon benchmark and proposed two methods for solving VRP, namely clustering and two-stage algorithms. The clustering algorithm used in the first stage was a modified K-means approach, while the second stage used a two-stage algorithm that evaluates a partial solution and transforms it into a single feasible solution. The two-stage algorithm used a hybridisation of four types of insertions that interact randomly to acquire individuals. Through experimentation, the proposed methods generated populations that varied by instance type and distribution. This study demonstrated the potential of clustering and two-stage algorithms to solve the VRP, and the results showed their effectiveness in producing near-optimal solutions.

The paper by [5] discussed the importance of minimising transport costs and finding the best route for logistics transport, and proposed a method that combines Ant Colony Optimisation (ACO) and K-means clustering for route optimisation. The K-means algorithm is used to cluster geographical localisation into smaller parts, and the ACO algorithm is used to find the shortest route. The proposed method shows promising results in logistic transport route optimisation and could potentially be applied in real scenarios to reduce transport costs and improve efficiency.

In his article, [6] proposed a two-step algorithm to solve the multi-depot vehicle routing problem with time windows (MDVRPTW) using crowdsourcing. The first step was to decompose the multi-depot problem into single-depot problems using the k-means algorithm. The second step was to determine the optimal routes for each depot using an adaptive ant colony algorithm. The results showed that the crowdsourced MDVRPTW resulted in a cost saving of 10.02% compared to the traditional MDVRPTW, and that ADACO was more efficient than the GUROBI solver for vehicle routing problems.

The results obtained by these researchers led to the conclusion that the use of the K-means algorithm can be useful to improve the computational solutions and the time to obtain results. This finding led us to consider this method in our solution approach. In our solution model, the K-means algorithm can be used to divide a set of locations (clients) into a predetermined number of clusters. Each cluster is then assigned to a vehicle that is responsible for servicing all the locations in that cluster. This approach can significantly improve the efficiency of vehicle routing by minimising travel time and distance between locations. What makes the K-means algorithm even more attractive is its flexibility and scalability, making it an ideal solution for both small and large VRP applications. By integrating the K-means algorithm into our solution approach, we aim to achieve high quality results that are not only fast, but also reliable.

## III. OPTIMIZATION OF CVRP

In this paper we consider a two-stage supply chain consisting of a supplier and customers with deterministic demand for a single type of product. The supplier's fleet of vehicles delivers the product to the customers, starting and stopping at the supplier's location. The vehicles in the supplier's fleet are identical. Our goal is to optimise the total cost of the supply chain while avoiding customer shortages. In this section, we first describe the CVRP we studied, followed by the mathematical formulation of the model.

## A. Presentation of CVRP model

Our problem can be defined as a Mixed Integer Programming (MIP) model in the sense that the supplier needs to define inventory levels alongside with routing decisions. The problem can be represented by a graph G (N, E), where N is the set of supplier and customer nodes such that N= N' ∪ {0} , with N' = {1, ... n} : set of customer nodes, and 0 represents the supplier node, and E is the set of corresponding arcs that connect the different nodes. At each discrete time over a finite horizon , the 𝑡 ∈ 𝑇 supplier is called to meet the demand of customers . 𝑞 𝑖 𝑡 𝑖 ∈ 𝑁' 𝑌 𝑖𝑗𝑘 𝑡 is a binary variable equal to 1 if the edge (i, j) ∈ N is traversed by the vehicle k at period , otherwise 0. is the cost of the 𝑡 ∈ 𝑇 𝐶 𝑖𝑗

𝑡

we use only one vehicle. The variable is the quantities that 𝑣 𝑖𝑘 the vehicle carries after delivering to the retailer i in each 𝑘 period t .

route between customers i and j . The binary variable is equal 𝑥 𝑖𝑘 to 1 if node is visited by vehicle at period , 𝑖 ∈ 𝑁 𝑘 𝑡 ∈ 𝑇 otherwise 0. is the capacity of the vehicle , in our case, 𝑄 𝑘 𝑘 ∈ 𝐾 𝑡

B. Index, Parameters and Decision Variables

## Index:

k: vehicle index ( ). 𝑘 = 1, …𝐾

i, j : The supplier and customer index.

## Parameters:

- : transportation between customers i and j. 𝐶 𝑖𝑗

: the capacity of the vehicle k 𝑄 𝑘

## Decision variables:

𝑡 = { 1 𝑖𝑓 𝑡ℎ𝑒 𝑒𝑑𝑔𝑒(𝑖,  𝑗) 𝑖𝑠 𝑡𝑟𝑎𝑣𝑒𝑟𝑠𝑒𝑑 𝑏𝑦 𝑡ℎ𝑒 𝑣𝑒ℎ𝑖𝑐𝑙𝑒 𝑘 𝑎𝑡 𝑡ℎ𝑒

𝑌 𝑖𝑗𝑘 𝑥 𝑖𝑘 𝑡 = { 1 𝑖𝑓 𝑡ℎ𝑒 𝑛𝑜𝑑𝑒 𝑖 𝑖𝑠 𝑡𝑟𝑎𝑣𝑒𝑟𝑠𝑒𝑑 𝑏𝑦 𝑡ℎ𝑒 𝑣𝑒ℎ𝑖𝑐𝑙𝑒 𝑘 𝑎𝑡 𝑡ℎ𝑒 𝑝𝑒𝑟 : the demand at node customer i ( ∈ N') at the period t 𝑞 𝑖 𝑡 : the quantity carried by the supplier's vehicle K after visiting 𝑣 𝑖𝑘 𝑡 the node i, in each period t

## C. Assumptions

The studied model is inspired by the CVRP model of [7]. In our model, five assumptions are initially made:

- ● Each customer must be visited only once.
- ● Each vehicle is assigned to a predefined set of customers.
- ● The demand of each set of customers must not exceed the vehicle capacity.
- ● It was impossible to distinguish between each customer's demands.

## D. Mathematical Modelling of CVRP

In this subsection, we present the mathematical formulation of our studied CVRP. We start with the objective function. It aims at minimising the total function costs of the network, namely the route costs over the time horizon:

$$\sum _ { \substack { t = 1 \\ \longrightarrow } } ^ { T \ K \ N \ N } \sum _ { k = 1 } ^ { N } \sum _ { i = 1 } ^ { N } C _ { i j } X _ { i j k } ^ { t } \\ \vdots \quad \vdots \quad \vdots \quad \vdots \quad \vdots \quad \vdots \quad \vdots \quad \text{$predete} \\ \text{formed} \\ \text{that $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\boldmath $\text{\bold$\colon$}$}$}$}$}$}$}$$

The vehicle capacity constraints:

$$\sum _ { i = 1 } ^ { N ^ { \prime } } q _ { i } ^ { t } & \leq Q _ { k } x _ { 0 k } ^ { t } & \forall \, t \, = \, 1 ; \forall \, k \, \ ( 2 ) \quad \text{cluster} \\ \vdots + 1 \dots + 1$$

The flow conservation constraints:

$$\begin{smallmatrix} 1 & t \in T, & & N \\ \text{ur case}, & & \sum _ { i = 1 } ^ { N } Y _ { i j k } ^ { t } & = \sum _ { i = 1 } ^ { N } Y _ { j i k } ^ { t } & & & \forall \, t, \forall \, j, \forall \, k & ( 3 ) \end{smallmatrix}$$

Subtour elimination constraints for each vehicle route at each period:

$$\nu _ { i k } ^ { t } - \nu _ { j k } ^ { t } + Q _ { k } Y _ { i j k } ^ { t } \, \leq Q _ { k } - q _ { j } ^ { t } \, \quad \quad \forall \, i, j, \forall \, k, \forall \, t \quad ( 4 )$$

$$d _ { i } ^ { t } & \leq Y _ { i } ^ { t } \leq Q _ { _ { K } } & \forall \, i, \forall \, t, \forall \, k \quad ( 5 )$$

Non-negativity and integrity constraints:

$$Y ^ { t } _ { i i k } & \in \{ 0, 1 \} & \forall \, k, \forall \, t \quad ( 6 )$$

$$i j k$$

$$x _ { _ { i k } } ^ { t } \in \{ 0, \, 1 \}$$

$$x _ { i k } ^ { \iota }$$

∀ 𝑘, ∀ 𝑡      (7)

$$\L _ { k } \, a t \, t h \epsilon \ \ q _ { i } ^ { t } \geq 0 & & i \, \in \, N ^ { \prime }, \forall \, t \ \ ( 8 )$$

(8)

$$\ t h e p e r \quad v ^ { t } _ { i k } \geq 0 & & i \in N, \forall \, k, \forall \, t \quad ( 9 )$$

## IV. PRESENTATION OF THE RESOLUTION METHOD

There are several approaches that can be taken to tackle the VRP problem. In this study, however, we have chosen to use modern techniques that make use of AI and machine learning models. Machine learning involves mathematical and data transformation methods that enable machines to learn from any available data source and automatically improve their task performance. Supervised and unsupervised learning are the two main branches of machine learning. In our research, we used unsupervised learning, specifically the K-means model, to cluster customers with requests at a given time according to their similarities, which in our case was their location. We then used the Nearest Neighbour (NN) algorithm and the Branch and Cut (B&amp;C) algorithm in each method to determine the shortest path within each cluster and compare the performance of each approach. In the following section, we will discuss the algorithms used to solve the VRP problem in more detail.

## A. K-Means Approach

The first step in our research is to cluster customers using the K-Means algorithm. This algorithm involves grouping data into a predetermined number of clusters, denoted by 'K'. Each cluster is formed by a centre, which is the mean of all the data points in that cluster. The algorithm starts by randomly initialising the 'K' cluster centres and then assigns each data point to the nearest cluster centre. This process is repeated until there is minimal change in the cluster centres from the previous iteration. The cluster centres are then updated as the mean of all data points belonging to that cluster. The K-means algorithm aims to keep

the clusters as small as possible while maintaining maximum differentiation between them.

The main objective of using the K-means algorithm is to optimise the order management process by grouping customers into clusters based on their proximity and vehicle capacity. In our case, the number of clusters is equal to the number of vehicles and the number of tours [8]. The flowchart in Figure 1 below illustrates how K-means clustering works.

Fig. 1: K-Means algorithm Flowchart

B. Nearest Neighbour Algorithm (NN algorithm)

![Image](image_000000_a8514f66a9bce453f115fa2e11a869a13702821d35b4b8ef6dc3d9719bd1bfdf.png)

Two heuristics were used to optimise the routing process. The first heuristic used was the Nearest Neighbour (NN) algorithm, which uses the clusters generated by the K-means model to determine the most efficient route composition. Although it is an approximation, NN is one of the earliest methods used to solve the travelling salesman problem. The algorithm starts with a random customer and visits the nearest customer sequentially until all customers have been visited. While this method can quickly produce a relatively short route, it typically does not produce the optimal solution due to its 'greedy' nature [9]. Figure 2 illustrates the steps of the NN algorithm.

Fig. 2: Nearest Neighbour Algorithm Flowchart

C. Branch and Cut Algorithm

![Image](image_000001_d966f504145d1ed1b649be302ffa776c2ec80805ef2725b4789b73c39982f91f.png)

The second method used to determine the best delivery route is the Branch and Cut (B&amp;C) algorithm. The B&amp;C algorithm is a successful and efficient method for solving various integer programming problems while maintaining optimality guarantees. It starts by generating a set of initial feasible solutions and then iteratively refines them through branching and cutting operations. In the branching step, a variable such as a vehicle or a customer is selected and two subproblems are created by assigning different values to that variable. This process creates a tree of subproblems, with each node representing a partial solution, [10].

In the cutting step, additional constraints are added to eliminate suboptimal solutions. These constraints are derived from valid inequalities that help reduce the feasible region without excluding the optimal solution. The algorithm then uses an LP solver to solve each sub-problem, enforcing capacity constraints and ensuring that each customer is visited exactly once. It continues to branch and cut until it converges on the optimal solution or reaches a stopping criterion. By using B&amp;C, CVRP can be solved efficiently and accurately, providing an optimal or near optimal solution for large problems, [11]. The organisational chart of the B&amp;C algorithm used in this study is shown in Figure 3.

Fig.3: Flow chart of the Branch and Cut Algorithm

V. COMPUTATIONAL EXPERIMENTS

![Image](image_000002_957f89bb74d30c8b01f943858abf872172b8a654342dc2e2d98791dba1251c21.png)

In this part, we will first present the technological tools used to program and solve our model. Then we will present the results of our experiments and finally we will interpret these results.

## A. Implementation

To solve the vehicle routing problem with capacity, we used the Spyder environment running on Python 2.7. This environment is accessible through the Anaconda platform and we executed the code on a Lenovo laptop with the following specifications: a ThinkPad PC E 480 equipped with an Intel Core i5-8250U processor (Quad-Core 1.6 GHz Turbo -6 MB Cache). The following libraries have been used in our work:

- ● NumPy: is a library for the Python programming language, designed to manipulate matrix or multidimensional arrays and mathematical functions that operate on them.
- ● Sklearn: is a widely used open-source Python library for machine learning, providing a wide range of efficient tools
- for various machine learning tasks such as classification, regression, clustering, dimensionality reduction, and more.
- ● Matplotlib: is a library written for the Python programming language that allows data to be plotted and visualised as graphs.

Our goal is to tackle the problem of capacitated vehicle routing with unsupervised machine learning. Our approach is to group customers into clusters using the K-means algorithm and to determine the shortest paths within these clusters using an optimisation algorithm.

## B. Computational Results

In this subsection, we have chosen a set of 4 customer instances, ranging from 10 to 100 customers with a predetermined demand in the interval [0; 7]. The customers are served by vehicles with a capacity Q, which is determined by the formula:

$$Q \geq \sum _ { i = 1 } ^ { N ^ { ^ { * } } } q _ { i } ^ { t } / K _ { 1 } \,.$$

This formula is used to allocate vehicle capacity from a single depot. The resulting solution consists of a schematic of each customer's location on a 2D map. To group the customers, the K-means algorithm is used, which divides the customers into three groups based on their location and vehicle capacity. The distribution of customers in the 2D search space is shown in Figure 4. The K-means algorithm was used to group the customers by colour. The algorithm was successful in grouping the customers by location, and the resulting groups are clearly visible in Figure 4.

Fig. 4: Clusters provided by the K-means algorithm for the instance of 30 customers

![Image](image_000003_4562e52277bcb80be82cb3a39eb1dcf94b3c9ad140ef1e06b983d3645cc27e98.png)

We will continue with the example of the 30 customers instance to show the output of the program. Figure 5 shows the second output generated using the nearest neighbour algorithm. This approach determined the most efficient routes for each group, as well as the total amount to be transported through them.

Fig. 5: Vehicles Routes Generated by NN Algorithm for the instance of 30 customers

The same approach was adapted using the B&amp;C algorithm. Figure 6 shows the resulting routes generated by the branch and cut algorithm. This approach determines the most efficient routes for each group, as well as the total quantity that needs to be transported through them.

![Image](image_000004_aeadceace9c1a5934c459d81b3aecbadd11a18b770ef2154fdb7b4945ac80a05.png)

Fig. 6: Vehicles Routes Generated by Branch and cut Algorithm for the instance of 30 customers

![Image](image_000005_fc068bedf9371f2fcae4e5363b5a3b988e922d25a1cc0f3493760700b3dc0600.png)

We will now make a comparative study of the results obtained by the two heuristics applied to the set of instances. This comparative study will provide us with valuable insights into the performance of both methods and help us to determine the best approach to solving the vehicle routing problem with capacity. Table 1 gives the results obtained for each instance in terms of optimal solution and computation time.

Table 1: Result of running the two methods K-means/NN and K-means/B&amp;C on the instance set.

| Method   | K-means - Nearest Neighbour   | K-means - Nearest Neighbour   | K-means - Branch and Cut Algorithm   | K-means - Branch and Cut Algorithm   |
|----------|-------------------------------|-------------------------------|--------------------------------------|--------------------------------------|
| Instance | Distance                      | Execution Time / s            | Distance                             | Execution Time / s                   |
| 10       | 115.41                        | 1.42                          | 77.211                               | 0.25                                 |
| 30       | 160.25                        | 0.81                          | 134.85                               | 0.36                                 |
| 50       | 454.62                        | 0.33                          | 335.38                               | 0.31                                 |
| 70       | 604.89                        | 0.30                          | 580.38                               | 0.28                                 |
| 100      | 1169.41                       | 0.31                          | 975.86                               | 0.30                                 |

This table shows that the K-means/B&amp;C method is more efficient than the K-means/NN method. In fact, the first method, using the B&amp;C algorithm, obtained a better solution in terms of path distance than the method using the NN algorithm, and in a shorter execution time. We found that the combination of K-means and B&amp;C is not only beneficial for small instances, but also for medium-sized instances. This is because K-means promotes a better distribution of the search space. In fact, K-means has made it easier for the B&amp;C algorithm to find the best solution to the problem without wasting time. We conclude by saying that a new line of research, which lies in the hybridisation of K-means with exact algorithms, may prove useful and appropriate in future research on this type of optimisation problem.

## VI. CONCLUSION

The capacitated vehicle routing problem (CVRP) remains a challenging optimization problem in the field of logistics. In this work, we addressed this problem using a machine learning approach, specifically the K-means algorithm combined with an exact method (B&amp;C) and an approximate method (NN)algorithm. Our results showed that the K-means algorithm coupled with B&amp;C performed well in terms of route and execution time optimisation compared to the other developed methods.

This study highlights the potential of using machine learning techniques to solve CVRP and encourages further research in this direction. As a future perspective, the proposed methodology can be applied to larger instances or real-world cases. In addition, it would be interesting to combine K-means with other exact or approximate algorithms and compare the results with those obtained in this paper. Another possible direction would be to extend this work to other variants of VRP using the methods developed in this paper.

## REFERENCES

- [1] Amri Sakhri M. S, Tlili M, and Korbaa O. (2022).  A memetic algorithm for the inventory routing problem. Journal of Heuristics, 28, 351-375.
- [2] Konstantakopoulos, G. D., Gayialis, S. P., Kechagias, E. P., Papadopoulos, G. A., &amp; Tatsiopoulos, I. P. (2020). A multi-objective large neighbourhood search metaheuristic for the vehicle routing problem with time windows. Algorithms, 13(10), 243.
- [3] Wang W, and Wu Y. (2012). A multi-stage algorithm for the real-world large-scale vehicle routing problem.
- [4] Cruz-Chávez M. A, and Martínez-Oropeza A. (2016). Feasible Initial Population with Genetic Diversity for a Population-Based Algorithm Applied to the Vehicle Routing Problem with Time Windows. Hindawi Publishing. Corporation Mathematical Problems in Engineering. Vol. 2016, 1-11.
- [5] Srinivasan M. and Sireesha K. Optimal Path Finding Algorithm for Logistic Routing Problem. In: International Conference on Intelligent Innovations in Engineering and Technology (ICIIET). IEEE, 2022. p. 203-209.
- [6] Xue S. (2023). An adaptive ant colony algorithm for crowdsourcing multi-depot vehicle routing problems with time windows. Sustainable Operations and Computers.

- [7] Beresneva E, and Avdoshin S. (2018). Analysis of Mathematical Formulations of Capacitated Vehicle Routing Problem and Methods for their Solution. Труды Института системного программирования РАН, 30(3), 233-250.
- [8] Hedayetul I. S, and Haque M. (2012). An Approach of Improving Student's Academic Performance by using K-means clustering algorithm and Decision tree. International Journal of Advanced Computer Science and Applications, Vol.3, No. 8, 146-149.
- [9] Alfandari, L., Plateau, A., Schepler, X. (2015). A branch-and-price-and-cut approach for sustainable crop rotation planning. Eur. J. Oper. Res. 241 (3), 872e879.
- [10] Amri Sakhri M. S, Tlili M, and Korbaa O. An Exact Algorithm for A Multi-Period Inventory Routing Problem with Lateral Transshipment. In: The 18th International Conference on Computer Systems and Applications (AICCSA), IEEE/ACS, 2021, p. 1-8.
- [11] Mitchell, J. E. (2002). Branch-and-cut algorithms for combinatorial optimization problems. Handb. Appl. Optim, 1, 65-77.