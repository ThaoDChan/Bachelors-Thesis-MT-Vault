![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000000_aeba0662fe9bddfc3b818c92196ddd0658e33af1bca143bee5482a6ee902225e.png)

Contents lists available at ScienceDirect

## Applied Soft Computing

journal homepage: www.elsevier.com/locate/asoc

## Aco-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems

Chao Wang a,b, ∗ , Biao Ma b , Jiye Sun b

- a Information Materials and Intelligent Sensing Laboratory of Anhui Province, Anhui University, Hefei 230601, China

Engineering Research Center of Autonomous Unmanned System Technology, Ministry of Education, School of Artificial Intelligence, Anhui

- b University, Hefei 230601, China

## A R T I C L E I N F O

## A B S T R A C T

Keywords: Vehicle routing problem Workload balancing Multi-objective optimization Evolutionary algorithm Knowledge transfer

The capacitated vehicle routing problem (CVRP) aims to find the routes that minimize transportation costs. However, in real-world applications, the equitable workload is essential for the drivers and logistics companies. For assigning an appropriate load for each vehicle, this paper formulates the CVRP with workload balancing as a multi-objective optimization problem (MO-CVRP). The goal is to simultaneously minimize the total length of routes and the variance of loads assigned to each vehicle. Although several multi-objective evolutionary algorithms (MOEAs) have been proposed for the MO-CVRP, they often suffer from slow convergence speeds due to the complexity of the search space, which hinders the identification of clear search directions. To address this issue, the knowledge transfer method is introduced that can leverage search experiences from related problems to expedite the solution process of the current problem. Specifically, a co-evolutionary genetic algorithm with knowledge transfer is proposed, where a simplified version of the traveling salesman problem (TSP) by disregarding the vehicle capacity constraint is utilized to facilitate the quick solution of the original MOCVRP. To enable information interaction between these two problems, the widely used denoising autoencoder is adopted to construct a transformation relation between solutions from the TSP to the MO-CVRP. Additionally, an adaptive migration strategy is proposed to adaptively select the migration methods and an improved split operator strategy is designed to ensure the feasibility of the solutions. The proposed algorithm is compared with four state-of-the-art MOEAs on MO-CVRP test sets with different scales. Experimental results show that the proposed algorithm exhibits faster convergence speed and achieves solutions with superior convergence.

## 1. Introduction

The vehicle routing problem (VRP) is a well-known NP-hard combinatorial optimization problem and has extensive applications in real world [1], such as food distribution, parcel delivery, and waste collection. Generally speaking, it is a generalization of the Traveling Salesman Problem (TSP), whose objective is to find the optimal routes for a fleet of vehicles to serve a set of customers with different locations and demands [2]. Over the last few decades, a large number of VRP variants have been widely studied for different real scenarios [3-8]. The capacitated VRP (CVRP) is the most fundamental variant among all existing VRPs [9-12]. Its objective is to minimize the total distance traveled by the vehicles to visit the customers within meeting the vehicle capacities [13], i.e., the total demand of customers over a route should not exceed the vehicle capacity. Moreover, in real-world applications, the workload balance is critical for both drivers and logistics companies. For the driver, it can reduce overtime and ensure each driver is treated equally and fairly. For the logistics company, it can save the transportation cost and improve the service quality. Hence, the CVRP with workload balancing has attracted much attention in recent years [7,14-16]. In this problem, there are two objectives should be optimized simultaneously: (1) The original total distance traveled by the vehicles; and (2) The additional workload distributed among vehicles, which can be expressed as the number of customers visited, the quantity of delivered goods, the tour length or the required time. This article focuses on the CVRP with load balancing.

Since a Pareto optimal set has to be generated for the CVRP with load balancing, multi-objective evolutionary algorithms (MOEAs) seem well-fitted due to their ability to produce a set of trade-off solutions in a single run [14,15,17,18]. Until now, several MOEAs have been proposed for solving the multi-objective CVRP (MO-CVRP), including the enhanced ant colony algorithm [15] and the memetic NSGAII [17]. Nevertheless, these MOEAs often rely on stochastic search techniques to achieve global exploration. As a result, they tend to

∗ Corresponding author.

E-mail addresses: wangchao8@ahu.edu.cn (C. Wang), wa22201016@stu.ahu.edu.cn (B. Ma), sunjy0708@126.com (J. Sun).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000001_04e47a4eddb462d10b73d9b180b53ee35acbb84ed7fb4542cb60328dab86bc47.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000002_e2461357d2bc3b5460075b08d1fe2ca2a0883b107ea416740c6fc6a962ccbf4d.png)

exhibit slow convergence speeds in the complex search space, especially when optimizing two objectives. Therefore, there is room for further improvement in terms of both computational efficiency and the quality of the obtained approximately optimal routes.

To mitigate the issue of slow convergence speed in existing algorithms for solving the MO-CVRP, this paper introduces a coevolutionary approach that incorporates a simplified helper problem known as the Traveling Salesman Problem (TSP). The TSP problem is constructed by disregarding the capacity constraint of the MO-CVRP, ensuring its similarity to the original problem. In this proposed method, two separate populations are employed to optimize the helper TSP problem and the original complex problem. Valuable information obtained from the solutions of the helper problem is utilized to guide the evolutionary search of the population for the original problem. This integration of information accelerates the convergence towards the Pareto optimal set. The main contributions of this work are summarized as follows.

- 1. A co-evolutionary genetic algorithm with knowledge transfer (CoEA-DAE) is proposed for the MO-CVRP. In this approach, a TSP is constructed by disregarding the vehicle capacity constraint in the original problem. The TSP solutions, which prioritize minimizing traveling distance, are leveraged to enhance the convergence speed of solving the MO-CVRP.
- 2. An effective information interaction strategy is designed for the above two heterogeneous problems of MO-CVRP and TSP due to their different constraints and optimization objectives. Specifically, the widely-used denoising autoencoder (DAE) from the field of transfer learning is employed to facilitate the transformation of solutions from the TSP to the MO-CVRP.
- 3. An adaptive migration strategy is introduced to dynamically select between direct migration and domain migration via the DAE, depending on the evolutionary state of the population. Additionally, an improved split operator strategy is developed to ensure the feasibility of solutions while simultaneously minimizing the load variance.

The remainder of this paper is organized as follows. In Section 2, the problem formulation of MO-CVRP, related work on solving CVRP, the background of the techniques used in this work, and the motivation behind conducting this research are presented. Then the details of the proposed CoEA-DAE are given in Section 3. The experimental results are shown in Section 4. In Section 5, we make a conclusion about this paper.

## 2. Problem formulation and related work

This section begins by providing a formal definition of MO-CVRP. It then introduces relevant solving algorithms and briefly discusses the background of the techniques used in this work. Finally, the motivation behind this research is presented.

## 2.1. Multi-objective capacitated vehicle routing problem

The MO-CVRP can be defined as an undirected graph 𝐺 = { 𝑉 , 𝐸 } , where 𝑉 = { 𝑣 𝑖 | 𝑖 = 0 1 2 … , , , , 𝑁 } is a set of vertices and 𝐸 = { 𝑒 𝑖𝑗 | 𝑖, 𝑗 = 0 1 … , , , 𝑁 } is the set of edges between vertices. The vertex 𝑣 0 represents the depot node so that all vehicles start from the depot point and return to the depot once the delivery task is completed. 𝑣 , 𝑣 1 2 , … , 𝑣 𝑁 represent the customers that need to be visited. The customer demand at each vertex is given by 𝑞 𝑖 ( 1 ≤ 𝑖 ≤ 𝑁 ). Each vehicle has a fixed loading capacity 𝐷 . Due to the limitation of the vehicle capacity, a vehicle can visit all customers in one route, only when the total demand of customers in this route does not exceed the vehicle capacity. The objective of MO-CVRP is to find a set of routes for the vehicles such that the total driving distance and the vehicle load variance are both minimized. Formally, the MO-CVRP can be defined as follows:

$$\underset { \mathfrak { g a l o } ^ { M } } { \text{quality} } \quad M i n \begin{cases} F _ { 1 } = & \sum _ { i = 0 } ^ { N } \sum _ { j = 0 } ^ { N } x _ { i j } c _ { i j } & \\ F _ { 2 } = & \sum _ { m = 1 } ^ { M } ( d _ { m } - \bar { d } ) ^ { 2 } / M & \\ \cdot \end{cases}$$

where 𝐹 1 is the total driving distance traveled by the vehicles, 𝑐 𝑖𝑗 represents the distance between the 𝑖 th customer and the 𝑗 th customer; when 𝑖 = 0 , 𝑐 0 𝑗 represents the distance from the depot to the 𝑗 th customer, when 𝑗 = 0 , 𝑐 𝑖 0 represents the distance from the 𝑖 th customer to the depot, 𝑥 𝑖𝑗 represents whether there is a path between the 𝑖 th customer and the 𝑗 th customer, where 𝑥 𝑖𝑗 = 1 means there is a path and 𝑥 𝑖𝑗 = 0 otherwise. 𝐹 2 is the load variance among vehicles, where 𝑀 is the number of vehicles used, 𝑑 𝑚 denotes the load of the 𝑚 th vehicle and ̄ 𝑑 is the average load over 𝑀 vehicles.

For better understanding the optimization objectives, Fig. 1 illustrates two different solutions for the same test instance that includes a depot and 10 customers. There are three homogeneous vehicles at the depot, with a capacity of 6 units each. Empty vehicles depart from the depot, visit customer to collect goods, and return to the depot. The vehicle needs to collect 1 unit of goods at every customer node it visits. In solution A, the route followed by Vehicle 1 to visit the customers is 0 → → → → → 1 2 3 4 0, resulting in a load 𝑑 𝐴 1 = 4. Vehicle 2 follows the route 0 → → → 8 9 10 → 0, with the load 𝑑 𝐴 2 = 3. Vehicle 3 follows the route 0 → → → → 5 6 7 0,with the load 𝑑 𝐴 3 = 3. The load variance among three routes in the solution A is 𝐹 2 ( 𝐴 ) = 0.222. In solution B, Vehicle 1 follows the route 0 → 10 → → → → → → 1 2 3 4 5 0, with the load 𝑑 𝐵 1 = 6. Vehicle 2 follows the route 0 → → → 8 9 0, with the load 𝑑 𝐵 2 = 2. Vehicle 3 follows the route 0 → → → 6 7 0, with the load 𝑑 𝐵 3 = 2. The load variance among these three routes in the solution B is 𝐹 2 ( 𝐵 ) = 2.782. 𝐹 2 ( 𝐴 ) &lt; 𝐹 2 ( 𝐵 ) means that the solution A is superior to the solution B in terms of the load balancing objective. Note that vehicles with higher loads can lead to increased wear and shorter lifespan, which is detrimental to vehicle maintenance. For instance, in the solution B, Vehicle 1 has a much higher load than Vehicle 2 and Vehicle 3, resulting in the highest level of wear and significantly shortened lifespan. To extend the lifespan of all vehicles in the fleet, it is crucial to minimize the load variance among the vehicles. This is the rationale for setting the objective function 𝐹 2 . MO-CVRP needs to satisfy the following seven constraints:

$$\begin{smallmatrix} s \text{ of the} \\ \text{ results} \\ \text{out this} \\ \end{smallmatrix} \quad \sum _ { i = 0 } ^ { N } x _ { i j } = 1, \forall j = 1, 2, \dots, N$$

$$\text{ou uns} \sum _ { j = 0 } ^ { N } x _ { i j } = 1, \forall i = 1, 2, \dots, N$$

$$\sum _ { i = 0 } ^ { N } x _ { i 0 } = K$$

$$\stackrel { \text{-cvr.r.} } { s s e s } \, \text{the} \quad \sum _ { j = 0 } ^ { N } x _ { 0 j } = K$$

$$& u _ { i } - u _ { j } + D x _ { i j } \leq D - q _ { j }, \forall i, j = 1, 2, \dots, N, i \neq j \\ & s. t. \ q _ { i } + q _ { j } \leq D$$

$$\overset { z } { i } _ { J } | u, J = \sum _ { \text{score} } q _ { i } \leq u _ { i } \leq D, \forall i = 1, 2, \dots, N$$

$$\underset { \dots } { \dots } v _ { x } \in \{ 0, 1 \}, \forall i, j = 1, 2, \dots, N.$$

Constraints (2) and (3) indicate that each customer node can only be driven in by one vehicle and out by one vehicle; Constraints (4) and (5) are the requirements of the total number of vehicles, where 𝐾 denotes the maximum number of vehicles permitted to be used; Constraint (6) is the subtour elimination constraint, where 𝑢 𝑖 and 𝑢 𝑗 represent the loading capacity of the vehicle after serving the 𝑖 th customer node and

Fig. 1. Illustration for the calculation of the load variance.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000003_b5601b8cb429fcb0c5bb288a598bb8a85803851561e5eeada55a9f00c01dd361.png)

the 𝑗 th customer node, respectively; 𝑞 𝑖 represents the demand of the 𝑖 th customer node; Constraint (7) indicates that the remaining capacity of the vehicle after serving the 𝑖 th customer node should be less than the maximum load 𝐷 ; Binary variables are defined in Constraint (8).

## 2.2. Related work

Generally, the goal of CVRP is to minimize one objective, i.e., the total driving distance or the total cost. During the past few decades, researchers have proposed a large number of algorithms to solve CVRP. These algorithms can be roughly divided into the three categories. The first category is the exact algorithms, such as branch-and-cut algorithm [19], cut plane method [20], column generation method [21], etc. For example, Fukasawa et al. [22] proposed a robust branchand-cut and-price algorithm by utilizing a lower bound obtained by minimizing over the intersection of the polytopes and the one defined by bounds, degree and the capacity constraints. These exact algorithms can find the optimal solution of small-scale CVRP within a specified time, and the quality of the solution is usually better than the other methods. However, these exact methods would be inefficient on larger problems with more than 50 customers. The second category is the individual-based heuristic methods, which mainly are based on different local search strategies. This kind of method can achieve an approximate optimal solution in a reasonable time for large-scale instances. These algorithms aim to develop effective local search operators, such as 2-opt [23], Lin-Kernighan heuristic [24], and exchange and relocation [25]. For example, Helsgaun et al. [26] proposed an extension of the Lin-Kernighan-Helsgaun which can transform CVRP into the constrained TSP and use the 𝑘 -opt method to explore the solution space. Accorsi et al. [13] designed the Iterated Local Search paradigm, which employs a carefully designed combination of existing acceleration techniques, as well as novel strategies to keep the optimization localized, controlled, and tailored to the current instance and solution. Generally, the effectiveness of these algorithms is commendable but their performance heavily depends on the local search operators that are problem-related. If the operator is not chosen properly, the algorithm can easily fall into a local optimum. The third kind is using population-based heuristic method to solve CVRP. Nowadays, many population-based heuristic methods have successfully solved CVRP problems, such as ant colony optimization algorithm [2729], genetic algorithm [30-33], etc. Vidal et al. [31] used the hybrid genetic search that combines the genetic algorithm with the improved SWAP operator which consists in exchanging two customers between different routes without an insertion in place.

Although numerous approaches have been proposed to address the CVRP, a significant majority of these methods primarily concentrate on the single-objective optimization of total distance or cost. However, it is crucial to note that achieving a balanced workload is of utmost importance in real-world applications, and thus should be regarded as an additional optimization objective. Consequently, several scholars have undertaken research on the MO-CVRP with a specific focus on workload balancing. Initially, Jozefowiez et al. [34] proposed CVRP with route balancing, where the route balance means to minimize the difference between the longest and the shortest route lengths among vehicles. In order to address this problem, Lacomme et al. [35] proposed a multi-start segmentation algorithm based on two search spaces and path reconnection. Fabien et al. [7] designed a heuristic algorithm based on multi-directional local search framework to solve the bi-objective optimization vehicle routing problem considering the workload balance. Mancini et al. [36] proposed to use an iterative local search algorithm to solve the cooperative vehicle routing problem with time consistency and workload balance. Li et al. [15] developed a cluster-based optimization framework with modified ACO to solve the workload balance issue that is the combination of route length and load.

It is worth noting that population-based heuristics have the capability to generate a diverse set of trade-off solutions within a single run. In recent years, these techniques have gained significant popularity for addressing the challenging multi-objective capacitated vehicle routing problem with an emphasis on workload balancing. For example, Mandal et al. [17] proposed an NSGA-II extension algorithm based on a dominance-based local search procedure and a clone management principle. Sun et al. [18] proposed an NSGA-II based memetic algorithm by combining four local search operators. Zhang et al. [14] developed a multiobjective memetic algorithm based on a problem-specific local search procedure and parallel computations on GPU devices. Recently, the knowledge-based acceleration method has shown the ability to improve the search efficiency of population-based heuristics for different VRPs. For example, Feng et al. [37] proposed an evolutionary memetic paradigm capable of learning and evolving knowledge memes across different but related problem domains. Along this line, Feng et al. [38] proposed a transfer optimization approach to leverage the useful traits buried in the optimized VRP solutions by learning and transferring a new customer representation across VRPs. Inspired by this method, this study constructs a simple TSP by removing the vehicle capacity constraint in the original MO-CVRP, to assist the quick solution of the original MO-CVRP.

## 2.3. Background

In recent years, three prominent concepts, namely co-evolution, knowledge transfer, and autoencoder, have gained significant attention in the field of multi-objective optimization (MOO), offering promising avenues for addressing complex multi-objective optimization problems.

Co-evolution is a concept borrowed from evolutionary computation, where multiple populations of solutions evolve simultaneously in a collaborative manner. Unlike traditional single-population evolutionary algorithms, co-evolution leverages the interactions and feedback between subpopulations to achieve high-quality solutions. This technique motivates the exploration of diverse solutions and the discovery of trade-offs among conflicting objectives, thereby enhancing the search capability of MOEAs [39]. Knowledge transfer technique focuses on the utilization of information gained from solving one problem to enhance the optimization process for a related or similar problem. In the context of MOO, the knowledge transfer between populations can provide the useful information about evolutionary search experiences from the source optimization problem to the target optimization problem. By leveraging the knowledge acquired during the source problems, it can significantly improve the search efficiency and accelerate the convergence towards optimal solutions for the target problem [40]. Autoencoder is a widely used self-supervised learning model in the transfer learning field, which can learn a compressed representation (encoding) of the input data and effectively reconstruct the original data from this representation (decoding). In the context of MOO, the autoencoder can be used to extract meaningful representations of the decision variables, enabling efficient exploration and exploitation in the search space. By leveraging the latent space representation captured by the autoencoder, the algorithms can make the search in desirable regions, facilitating the discovery of Pareto-optimal solutions [41].

## 2.4. Motivation

To address the MO-CVRP, this study proposes a novel co-evolutionary genetic algorithm with knowledge transfer based on a denoising autoencoder. The motivation behind this method arises from two key aspects. Firstly, while existing heuristic algorithms have shown promising results in solving VRPs, they often require manual design of local search operators and initial solutions. Inadequate design choices can lead to the algorithms becoming trapped in local optima, limiting their effectiveness [15,17]. Secondly, current evolutionary algorithms encounter challenges with slow convergence when tackling VRPs [14, 18]. However, it has been demonstrated that MOEAs employing cooperative coevolutionary strategies can effectively leverage the valuable information present within multiple populations, thereby accelerating the search processes.

The proposed algorithm adopts a widely used coevolutionary framework based on two populations, aiming to utilize a simplified and similar TSP to aid in solving the complex MO-CVRP. Initially, the TSP problem is derived by disregarding the vehicle capacity constraints of MO-CVRP, ensuring similarity with the original problem for subsequent knowledge transfer. Two distinct populations are established to independently address the TSP and MO-CVRP. Subsequently, a denoising autoencoder is introduced to establish the correspondence between solutions in the TSP and MO-CVRP populations, enabling effective interaction of search information between these two heterogeneous problems. Finally, to achieve adaptive adjustment of the number of migration solutions and obtain feasible solutions after the interaction, we propose an adaptive migration strategy and an improved Split operator strategy, respectively.

## 3. Solution approach and algorithm

In this section, the framework of the proposed CoEA-DAE is given first, and then the training and prediction process of DAE is introduced in detail, and an adaptive migration strategy that can adaptively select the migration method according to the evolutionary state of the population is introduced finally.

## Algorithm 1: The procedure of CoEA-DAE

Input: 𝑁𝑚 (Population size), 𝑔 𝑚𝑎𝑥 ( Maximum generation number), 𝐹ℎ𝑒𝑙𝑝 (Constructed simple TSP task), 𝐹𝑜𝑟𝑖𝑔𝑛𝑎𝑙 (The original MO-CVRP task).

Output: 𝑃 (Pareto Set of MO-CVRP).

- 1 𝑃 ← RandomInitialization( 𝑁𝑚 , 𝐹𝑜𝑟𝑖𝑔𝑛𝑎𝑙 );
- 2 𝑃ℎ𝑒𝑙𝑝 ← RandomInitialization( 𝑁𝑚 , 𝐹ℎ𝑒𝑙𝑝 );
- 3 𝑅𝑎𝑛𝑘 ← Do non-dominated sorting ( 𝑃 );
- 4 𝐶𝑟𝑜𝑤𝑑𝐷𝑖𝑠 ← Calculate the crowding distance ( 𝑃 );
- 5 for 𝑔 = 1 to 𝑔 𝑚𝑎𝑥 do
- Update DAE by taking 𝑃ℎ𝑒𝑙𝑝 and 𝑃 as input and output, respectively.;
- 20 return 𝑃 ;

```
                                                                                                                                                                                                        3  Rank += Do non-dominated sorting (P);
                                                                                                                                                                                                       
                                                                                                                                                                                                      

                                                                                                                                                                                                       3
                                                                                                                                                                                                       4
                                                                                                                                                                                                       1
                                                                                                                                                                                                       2
                                                                                                                                                                                                       0
                                                                                                                                                                                                       5
                                                                                                                                                                                                       6
                                                                                                                                                                                                       7
                                                                                                                                                                                                       8
                                                                                                                                                                                                       9
                                                                                                                                                                                                       10
                                                                                                                                                                                                       19
                                                                                                                                                                                                       20
                                                                                                                                                                                                       29
                                                                                                                                                                                                       17
```

## 3.1. Framework of the proposed CoEA-DAE

The framework of the proposed algorithm CoEA-DAE is presented in Algorithm 1. CoEA-DAE is mainly based on the co-evolutionary optimization framework, where the TSP and the MO-CVRP are solved at the same time, based on the two independent evolution populations. First of all, the main population 𝑃 and the auxiliary population 𝑃 ℎ𝑒𝑙𝑝 with the size of 𝑁𝑚 are set to solve the MO-CVRP and the TSP, respectively. At each generation, these two populations are evolved separately by using the evolution procedure of NSGA-II [42]. Then, the knowledge transfer strategy is implemented, i.e., the elite solutions in 𝑃 ℎ𝑒𝑙𝑝 would be regarded as the offspring of the main population by the DAE-based transfer or the direct migration. Here, the solutions in the auxiliary population can accelerate the convergence of the main population, since the solving for a TSP is relatively simpler and faster than the solution of MO-CVRP. Through the interaction between two populations, the evolution information of the TSP population is transferred to the main population by taking advantage of the fast convergence ability of the TSP population, so as to accelerate the convergence speed of the main population.

## 3.2. DAE-based knowledge transfer

In order to realize the knowledge transfer between the two heterogeneous problems TSP and MO-CVRP, the widely used DAE is adopted to construct the transformation relation between solutions from TSP to MO-CVRP. DAE is a stochastic version of autoencoder, which aims to encode the input by a stochastic corruption process. The hidden representation can be seen as a connection between the input and the output, which can conduct knowledge transfer across two problem domains [43].

Assume that the input solution vector 𝐱 from the TSP population, the encoding procedure transforms the input solution to obtain hiddenlayer representation 𝐲 through the function 𝐲 = 𝑠 ( 𝐖𝐱 + 𝐛 ) , where W and b mean the weight and bias between the input and the hidden

Fig. 2. Training for DAE.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000004_edb0ff2a7a84e64ea9ea17e3aef4a130bb967d54aae30db488b1412602efccc9.png)

layer, respectively, and 𝑠 is the commonly used sigmoid activation function, i.e., 𝑠 ( 𝐱 ) = [ 1∕ ( 1 + 𝑒 (-𝐱 ) )] . While the decoding procedure reprojects the encoded representation to a reconstructed solution vector 𝐳 , where 𝐳 = 𝑠 ( 𝐖𝐲 ′ + 𝐛 ′ ) , such that 𝐳 ≈ 𝐱 . Here, the solutions from the main population are regarded as the reconstructed solutions for the input of TSP solutions, not for itself. This way aims at constructing the mapping relation between solutions from TSP to MO-CVRP by using the DAE. The autoencoder parameters, i.e., 𝐖 𝐖 , ′ , 𝐛 , 𝐛 ′ are optimized to minimize the average reconstruction error, which is expressed as

$$\min _ { w, w ^ { \prime }, b, b ^ { \prime } } \frac { 1 } { N _ { m } } \sum _ { i = 1 } ^ { N _ { m } } L ( { \mathbf x } _ { i }, z _ { i } ) & & ( 9 ) & & 3. 3. \ A d$$

where 𝑁𝑚 denotes the number of training samples at each generation that is equal to the population size, and 𝐿 is a loss function, such as the square loss, Kullback-Leibler divergence, etc.

This study takes the solutions from TSP and MO-CVRP as the input and output samples respectively, to train the DAE network. Once trained, this model can approximate the transformation relation from TSP solutions to MO-CVRP solutions. Fig. 2 shows the training process for DAE, where the solution {1 2 3 … , , , , 𝑁 } from the auxiliary population is taken as the input, and the solution {1 3 2 … , , , , 𝑁 } from the main population is taken as the output. Note that the dimension of the input and the output is equal to the number of customers 𝑁 .

In this way, the transformation of solutions from TSP to MO-CVRP can be realized by using DAE. However, it is worth noting that, when DAE is used in the prediction process, the output solution of DAE is a real vector, not a customer visiting order. Hence, it needs to be mapped into an integer sequence. The detailed prediction and mapping process is shown in Fig. 3. Assume that {0 1 3 2 5 4 6 7 0} , , , , , , , , is a solution from the TSP population. Here, the customer visiting sequence after removing the depot nodes is used as the input of the DAE model. Then, the prediction result can be obtained, i.e. {1 05 3 12 5 31 2 67 4 39 6 57 . , . , . , . , . , . , 7 01} . . First, the prediction result is sorted in ascending order according to the predicted value. The sequence number to be sorted is {1 2 3 4 5 6 7} , , , , , , , and the index information is {1 4 2 5 3 6 7} , , , , , , . Then, the 𝑖 th sequence number (1 ≤ 𝑖 ≤ 𝑁 ) to be sorted is put into the corresponding position of the solution according to the 𝑖 th value of the index information. For example, the second sequence number to be sorted is 2. The second value of index information is 4, so the sequence number 2 is placed in the fourth position of the final solution.

Finally, this study modifies the Spilt operator [44] to make the solution feasible and contribute to the minimization of the objective of the load balance. Simply speaking, the load balance between vehicles should be considered when using the original Split operator to segment the solution. Specifically, if the sum of customer demand exceeds the average load 𝐶 , not reaching the maximum capacity 𝐷 , a new vehicle needs to be provided to serve the remaining customers. The average load 𝐶 is calculated as follows.

$$\underset { \substack { \text{occurence} \\ \text{ vector} } } { \text{ vector} } \quad C = ( \sum _ { i = 1 } ^ { N } q _ { i } ) / M, i = 1, 2, \dots, N \quad \text{(10)}$$

where 𝑞 𝑖 represents the 𝑖 th customer demand, 𝑀 represents the number of vehicles used in the prediction solution, and 𝐶 is the maximum vehicle capacity considered when using the improved Split operator to segment the solution. Thus, the feasible solution of the DAE prediction in Fig. 3 is {0 1 3 5 2 0 4 6 7 0} , , , , , , , , , .

## 3.3. Adaptive migration strategy

In the transfer process from the auxiliary population to the main population, two kinds of migration methods are adopted: one is using the converted solutions via DAE and the other is directly using the original solutions in the auxiliary population. Specifically, the first one is mapping the offspring of the auxiliary population to the offspring of the main population using a pre-trained DAE; the second one is directly using the original solutions from the auxiliary population as offspring of the main population. After the above solutions are added into the main population, the environmental selection strategy in NSGA-II is used to generate the next population. The detailed process is shown in Fig. 4. This hybrid way can combine the advantages of two methods to create diverse and promising offspring for the main population. Moreover, selecting which migration strategy for next generation is adaptively determined based on the survival rate of the offspring generated by the current migration strategy, which can be calculated as follows.

$$\text{solution} \quad \eta _ { t } = \lambda _ { t } / N _ { m },$$

where 𝜆 𝑡 represents the number of successful offspring solutions generated in the current migration strategy after the environmental selection in the 𝑡 th generation main population and 𝑁𝑚 denotes the population size. If the survival rate 𝜂 𝑡 is larger than 0.5, the current migration strategy remains unchanged, otherwise it switches to another migration strategy. In addition, if more than half of the solutions in the main population have not been improved after the continuous use of the current strategy for twenty generations, the algorithm will believe that the current strategy makes the population fall into local optimum, and it needs to switch to another migration strategy, so as to make the population get rid of the local optimum.

## 4. Experimental results and analysis

In this section, the related experimental settings are first given. Then, the effectiveness of adaptive migration strategy and DAE model is verified and analyzed. Finally, the proposed approach is compared with four state-of-the-art approaches to evaluate its performance.

Fig. 3. Solution transformation based on DAE.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000005_7cba91d96c2c4fc121eac29a1e27f18b3763ea9bcc8376a66991ebc33e4208cb.png)

Fig. 4. Flowchart for the adaptive migration strategy.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000006_ebc5554ef7137b9fb0550dc9344f3b7d6b0f04303815efd7fb70ef9ff5277481.png)

## 4.1. Experimental settings

## 4.1.1. Datasets and algorithms

The test instances for MO-CVRP can be directly obtained from various existing VRP test suites, two widely used VRP test suites, Class ''M'' [45] and Class ''X'' [46] are chosen. The ''M'' dataset consists of 5 instances and the ''X'' dataset consists of 100 instances. In this study, all test instances from the ''M'' dataset and the 27 representative test instances from the ''X'' dataset are chosen for conducting the experiments. For each test instance, ''n'' refers to the total number of nodes, including the depot, and ''k'' represents the upper limit of the number of vehicles allowed in that instance. The proposed CoEADAE is compared with four existing algorithms, namely, MOEA [47], M-MOEA/D [48], KBEA [49], and CCMO [50]. MOEA is an improved multiobjective evolutionary algorithm for the vehicle routing problem with time windows (VRPTW). It not only depends on the fitness value to improve the convergence, but also the similarity measurement to maintain the population diversity. M-MOEA/D is a decomposition-based memetic algorithm for VRPTW, where a new selection operator and three local search operators are used to improve the solution quality.

Table 1 Parameter values of the algorithms.

| General parameter             | Value   |
|-------------------------------|---------|
| Population size 𝑁 𝑚           | 100     |
| Maximum number of evaluations | 150,000 |
| Crossover probability 𝑝 𝑐     | 0.8     |
| Mutation probability 𝑝 𝑚      | 0.2     |

KBEA integrates the knowledge related to the problem into genetic operators, where the information of the shortest average distance was utilized to produce the high-quality solutions. Based on the coevolutionary framework, CCMO proposes the method of using simple TSP task to assist complex VRPTW task, and utilizes the fast convergence ability of simple task population to help accelerate the convergence of complex task population.

## 4.1.2. Parameter settings

The parameters needed in the experiment can be divided into two parts. The first part is the necessary parameters in the proposed algorithm and the comparison algorithms. The second part is the parameters needed in the DAE model. They are shown in Tables 1 and 2, respectively. As shown in Table 1, the maximum number of function evaluations is set to 150,000, the population size 𝑁𝑚 is set to 100, the crossover probability 𝑝 𝑐 is set to 0.8 and the mutation probability 𝑝 𝑚 is set to 0.2, respectively. The parameters of ''Population size'' and ''Maximum number of evaluations'' in Table 1 are set according to the Ref. [50], which is a general setting method when using the genetic algorithm to solve vehicle routing problems. The other parameters are set to the same as recommended in the original studies. Specifically, for MOEA, the population size is 100, the number of generations is 500, the crossover probability is 1, the mutation probability is 0.1, and the tournament selection size is set to 2 [47]. For MMOEA, the maximum number of function evaluations is set to 50,000, the number of subproblems is 100, the crossover probability is 0.8, the mutation probability is 0.2, the neighborhood size is 10, and the external population size is 100 [48]. For KBEA, the population size is 100, the number of generations is 4000, the number of routing solutions for crossover is 6, and the number of routing solutions for mutation is 30 [49]. For CCMO, the population size is set to 100, the number of generations is 500, the crossover probability is 1, and the mutation probability is 0.1 [50]. As shown in Table 2, the number of input and output neurons is 𝑁 , and the number of hidden neurons is 4 𝑁 , where 𝑁 is the number of customers. Both the activation functions and the training method are the functions and methods encapsulated in the MATLAB neural network toolbox. The parameters are listed in Table 2, which are set according to the Ref. [43]. In this reference, the setting of parameters has been validated effective for the vehicle routing problems. For the fair comparison, all algorithms are written in MATLAB and run on the CPU Intel Core i7-6700 with 3.4 GHz and 8-GB memory.

To demonstrate the rationality of setting the crossover and mutation probabilities in this study, a comparative experiment is conducted. CoEA-DAE with a crossover probability of 1 and a mutation probability of 0.001, as suggested in [51,52], is chosen as the compared algorithm. Both groups of CoEA-DAE algorithms have identical parameters except for the crossover and mutation probabilities. Both algorithms are applied to the same instances and executed on each test instance for 31 independent runs. The average and standard deviation values of HV results are shown in Table 3. From Table 3, it can be found that the CoEA-DAE with a crossover rate of 0.8 and a mutation rate of 0.2 performs better than the other version in almost all of instances. Therefore, it is reasonable for setting the crossover probability to 0.8 and the mutation probability to 0.2.

Table 2 Parameter setting for DAE.

| Parameters of DAE                  | Value    |
|------------------------------------|----------|
| Number of neurons in input layer   | 𝑁        |
| Number of neurons in hidden layer  | 4 𝑁      |
| Hidden layer activation function   | tansig   |
| Output layer activation function   | purelin  |
| Training method                    | traincgb |
| Epochs                             | 1500     |
| Termination conditions of training | 0.1      |
| Learning rate                      | 1e - 6   |

Table 3 Comparison of HV results obtained by CoEA-DAE with 𝑝 𝑐 = 0.8, 𝑝 𝑚 =

0.2 and CoEA-DAE with 𝑝 𝑐 = 1, 𝑝 𝑚 = 0.001. Best results is highlighted in bold.

| Instance   | 𝑝 𝑐 = 1, 𝑝 𝑚 = 0.001   | 𝑝 𝑐 = 0.8, 𝑝 𝑚 = 0.2   |
|------------|------------------------|------------------------|
| M-n101-k10 | 0.6726 (5.28e - 3) -   | 0.6879 (3.63e - 3)     |
| M-n121-k73 | 0.7424 (2.41e - 2) -   | 0.7523 (3.65e - 2)     |
| M-n151-k12 | 0.7429 (5.76e - 3) ≈   | 0.7413 (5.42e - 3)     |
| M-n200-k16 | 0.7648 (4.76e - 3) -   | 0.7674 (4.32e - 3)     |
| M-n200-k17 | 0.7739 (3.44e - 3) -   | 0.7849 (5.07e - 3)     |
| X-n101-k25 | 0.5359 (8.68e - 3) -   | 0.5453 (5.33e - 3)     |
| X-n106-k14 | 0.4929 (1.22e - 2) -   | 0.5017 (1.36e - 2)     |
| X-n110-k13 | 0.7036 (1.28e - 2) -   | 0.7061 (1.63e - 2)     |
| X-n115-k10 | 0.7528 (2.57e - 2) -   | 0.7654 (1.33e - 2)     |
| X-n120-k6  | 0.5613 (1.30e - 2) -   | 0.6734 (1.05e - 2)     |
| X-n125-k30 | 0.4432 (5.67e - 3) -   | 0.4683 (1.43e - 2)     |
| X-n129-k18 | 0.5493 (5.76e - 3) -   | 0.5622 (5.81e - 3)     |
| X-n134-k13 | 0.6845 (2.92e - 3) -   | 0.7394 (1.21e - 2)     |
| X-n139-k10 | 0.7413 (3.40e - 2) -   | 0.7495 (2.50e - 2)     |
| X-n143-k7  | 0.6974 (8.12e - 3) -   | 0.7410 (5.55e - 3)     |
| X-n148-k46 | 0.5214 (8.39e - 3) +   | 0.5037 (7.40e - 3)     |
| X-n153-k22 | 0.5907 (7.97e - 3) -   | 0.6467 (7.77e - 3)     |
| X-n157-k13 | 0.5267 (6.55e - 3) -   | 0.5421 (2.27e - 3)     |
| X-n162-k11 | 0.7312 (4.72e - 3) -   | 0.7865 (2.38e - 3)     |
| X-n167-k10 | 0.5305 (4.45e - 3) -   | 0.5940 (1.04e - 2)     |
| X-n172-k51 | 0.5662 (6.02e - 3) -   | 0.5726 (5.36e - 3)     |
| X-n176-k26 | 0.5835 (5.21e - 3) -   | 0.5901 (5.92e - 3)     |
| X-n181-k23 | 0.3330 (2.45e - 3) -   | 0.3742 (6.06e - 3)     |
| X-n186-k15 | 0.6828 (6.64e - 3) -   | 0.7253 (3.60e - 3)     |
| X-n190-k8  | 0.4685 (8.17e - 3) -   | 0.5595 (9.09e - 3)     |
| X-n195-k51 | 0.5835 (6.04e - 3) -   | 0.6158 (6.72e - 3)     |
| X-n200-k36 | 0.5541 (3.73e - 3) -   | 0.5935 (4.89e - 3)     |
| X-n204-k19 | 0.7394 (7.43e - 3) -   | 0.7804 (4.00e - 3)     |
| X-n209-k16 | 0.5962 (7.44e - 3) -   | 0.6809 (3.87e - 3)     |
| X-n214-k11 | 0.6756 (6.54e - 3) -   | 0.7661 (5.34e - 3)     |
| X-n219-k73 | 0.3074 (2.51e - 2) -   | 0.3087 (6.09e - 3)     |
| X-n223-k34 | 0.6009 (4.79e - 3) -   | 0.6647 (5.18e - 3)     |
| +∕ -∕ ≈    | 1/30/1                 |                        |

## 4.1.3. Performance indicator

The hypervolume (HV) [53] is used to evaluate the quality of the non-dominated solution set obtained by algorithms, which can be able to account for both the convergence (closeness to the true Pareto front) and the distribution of the achieved nondominated solutions. Let 𝑃 be the set of final non-dominated points obtained in the objective space by an algorithm, and 𝐳 = ( 𝑧 , 𝑧 1 2 , … , 𝑧 𝑛 ) 𝑇 be a reference point in the objective space which is dominated by all Pareto optimal points. The hypervolume indicator measures the volume of the region dominated by 𝑃 and bounded by 𝐳 .

$$\ddot { \L a l g o - } \underset { \text{ameters} } { \L a l g o - } \underset { F \in P } { \ H V ( P, z ) = V o l u m e ( \bigcup _ { F \in P } [ F _ { 1 }, z _ { 1 } ] \times \cdots \times [ F _ { n }, z _ { n } ] ) }$$

where n is the number of objectives and F is the objective vector. The larger the HV value, the better the quality of the solution set is. All the algorithms are executed on each test instance for 31 independent runs, and the mean value is recorded. Since the second objective of the loading variance is first proposed in this study, the true PF for the above two classes of problems is unknown. Therefore, in this study, the reference point for calculating HV is chosen as follows. This article first combines all solution sets obtained by these three algorithms in all

Table 4 The HV results of the proposed CoEA-DAE algorithm and four comparison algorithms on the ''M'' dataset. The running time (in seconds) consumed by these five algorithms is shown in the second brackets ''()''. Best result is highlighted in bold.

| Instance             | CCMO                           | MOEA                                                                                                                                             | M-MOEA/D                                                                                                                                                   | KBEA                                                                                                                             | CoEA-DAE                                                                               |
|----------------------|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| M-n101-k10 M-n121-k7 | 0.6734 (2.63e - 3) - (701.06)  | 0.5855 (1.45e - 2) - (1030.19) 0.5884 (1.43e - 2) - 0.6770 (1.24e - 2) - (2261.38) 0.6518 (1.28e - 2) - (3883.18) 0.6757 (1.15e - 2) - (3879.33) | 0.6298 (1.22e - 2) - (1030.19) 0.6335 (1.13e - 2) - (1421.10) 0.7269 (6.75e - 3) - (2261.38) 0.7417 (4.96e - 3) - (3883.18) 0.7584 (5.23e - 3) - (3879.33) | 0.6159 (8.51e - 3) - (69.60) 0.6249 (1.44e - 2) - (75.94) 0.7208 (4.94e - 3) - (86.80) 0.7323 (1.52e - 2) - 0.7472 (1.20e - 2) - | 0.6879 (3.63e - 3) (1997.42) 0.7523 (3.65e - 2) (2350.67) 0.7413 (5.42e - 3) (3090.04) |
|                      | 0.6907 (6.23e - 3) - (919.69)  | (1421.10)                                                                                                                                        |                                                                                                                                                            |                                                                                                                                  |                                                                                        |
| M-n151-k12           | 0.7422 (2.82e - 3) ≈ (1456.76) |                                                                                                                                                  |                                                                                                                                                            |                                                                                                                                  |                                                                                        |
| M-n200-k16           | 0.7558 (4.63e - 3) - (2492.96) |                                                                                                                                                  |                                                                                                                                                            | (127.61)                                                                                                                         | 0.7674 (4.32e - 3) (4698.51)                                                           |
| M-n200-k17           | 0.7726 (2.78e - 3) - (2488.70) |                                                                                                                                                  |                                                                                                                                                            | (124.62)                                                                                                                         | 0.7849 (5.07e - 3) (3876.87)                                                           |
| +∕ -∕ ≈              | 0/4/1                          | 0/5/0                                                                                                                                            | 0/5/0                                                                                                                                                      | 0/5/0                                                                                                                            |                                                                                        |

runs on the considered test instance, and then removes all dominated solutions in the combined solution set. The maximum values on each objective of all the non-dominated solutions are identified and multiplied by 1.1 as the reference point for calculating the HV. Furthermore, the Wilcoxon rank sum test with a significance level of 0.05 and the Holm procedure [54] are adopted to test the differences for statistical significance, where the symbols '' + '', '' -'' and '' ≈ '' indicate that the compared algorithm performs significantly better than, worse than, and equivalent to the proposed method.

## 4.2. Comparison between CoEA-DAE and existing algorithms

This section shows the performance comparison between the proposed algorithm and the comparison algorithms on the ''M'' and ''X'' datasets, and analyzes the experimental results to verify the competitiveness of the proposed algorithm in accelerating convergence and improving the solution quality.

to provide the explicit evolutionary direction for the main population, and thus faster reach the Pareto optimal solutions. For the M-n101-k10, M-n200-k16, M-n200-k17, X-n125-k30, X-n162-k11, X-n214-k11, Xn223-k34 test instances, it can be observed that the HV values obtained by CoEA-DAE at the early stage are not best, but at the late stage, CoEA-DAE outperforms other algorithms. The reason may be that, at the early stage, not enough solutions can be collected to well train the DAE and the transformed solutions may be not suitable for the evolution of the main population. However, as the evolution proceeds, the quality of the solution set from the main and auxiliary populations is getting better and better, the DAE can be well trained and represents the relationship from TSP to MO-CVRP more and more accurately, the proposed knowledge transfer strategy can make a positive function on solving MO-CVRP and facilitate the evolution of the main population.

Tables 4 and 5 show the statistical results obtained by the five algorithms, where the first column of the tables provides the instance information for the datasets, and the second to sixth columns display the average HV values (outside parentheses), the standard deviations (within the first parentheses), and average running time (within the second parentheses) over 31 independent runs. For the ''M'' dataset, CoEA-DAE obtains 4 best mean HV values among 5 test instances, only it loses the best result on M-n151-k12. For the ''X'' dataset, CoEADAE obtains 23 best mean HV values among 27 test instances and shows the best overall performance among five algorithms. It is mainly owing to that the knowledge transfer method can better learn the useful information contained by solutions in the auxiliary population, and thus accelerate the convergence of the main population. Through the comparison of the average running time (in seconds) of CoEA-DAE and the four comparison algorithms, it can be observed that CoEA-DAE has longer running time than the other four compared algorithms on most test instances. One limitation of the proposed CoEA-DAE method is the requirement for training a learning model, which introduces additional running time. However, it is important to note that this additional time remains within an acceptable range. While the training process may extend the overall computational time, it is a necessary step to ensure the effectiveness and efficiency of the algorithm. By investing the time to train the learning model, we can achieve improved performance and accuracy in solving the capacitated vehicle routing problem with workload balancing. Therefore, despite the slight increase in running time, the benefits gained from the trained learning model outweigh this limitation, making it a worthwhile trade-off for obtaining high-quality solutions.

To visually understand the performance of the proposed method during the evolutionary process, their convergence curves on the small-, medium- and large-scale problem instances of ''M'' and ''X'' datasets respectively, are displayed where the 𝑦 -coordinate shows the HV value of each generation averaged over 31 runs, and the 𝑥 -coordinate shows the number of generations. From Fig. 5, it can be found that, on M-n121-k7, X-n101-k25, X-n106-k14 and X-n153-k22 test instances, CoEA-DAE can obtain the better HV than other algorithms both at the early stage and the late stage. The reason is that, CoEA-DAE can take full use of the useful information contained in the auxiliary population

To visually understand the experimental results, Fig. 7 shows the final non-dominated solutions obtained by the proposed algorithm and four comparison algorithms on four ''X'' test instances with 101, 134, 167 and 200 customers, in terms of the objectives of the driving distance ( 𝐹 1 ) and the load variance ( 𝐹 2 ). From these four figures, it can be found that the diversity and convergence of the final non-dominated solutions obtained by the proposed algorithm is clearly better than those obtained by the other four compared algorithms, which indicates that the knowledge transfer between two problem domains can effectively improve the diversity and convergence of the solution set for the original problem. In addition, Fig. 6 shows the final route graph obtained by the proposed algorithm on three ''M'' test instances and three ''X'' test instances, respectively. It can be observed that the final route obtained by the proposed algorithm not only considers the objective of the driving distance, but also effectively addresses the second objective of the load variance among vehicles.

## 4.3. Effectiveness of knowledge transfer

In this subsection, an ablation study is implemented to verify the effectiveness of the proposed knowledge transfer from the simple TSP to the MO-CVRP, where CoEA-DAE is compared to its four variants. Variant-I only uses the main population in the whole optimization process, which means that no TSP auxiliary population collaborates with the main population, which is used to verify the function of the solution information contained in TSP. Variant-II and Variant-III still adopt the coevolutionary between two populations but not the hybrid migration strategy, that is, Variant-II only migrates the solutions generated by the DAE model to the main population while VariantIII migrates the original solutions in the auxiliary population, which is used to verify the effectiveness of the hybrid migration strategy. Variant-IV uses the random selection of two migration strategies not the proposed adaptive selection method, which is used to verify the effectiveness of using the survival rate to adaptively select migration method.

Table 6 presents the HV results obtained by CoEA-DAE and its four variants on two ''M'' test instances and six ''X'' test instances. It can be found that CoEA-DAE is significantly better than Variant-I, VariantII,Variant-III and Variant-IV on almost all of test instances, which verifies the effectiveness of the knowledge transfer from the simple TSP to the MO-CVRP and the proposed adaptive migration strategy.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000007_609d33fb376901f3b42ecf112f67422561785e41c34816dd019346fcb4c1aaf0.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000008_cc99c7789cf66913bbd2ea4f8107f713dd177ddd5cc535687515639f1e2fca93.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000009_7264d2ba535feb2fd8c935428a79b93ba9952d73cdfcd52f0fa9a18ccd8d72c9.png)

Fig. 5. Convergence curves of five algorithms on twelve test instances.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000010_e1935cdcf6c22383b40690117df873c7ea45bbebac2137a99784ed94501b67fd.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000011_44b2374c2cc0b0953bb4f1d362b61e56346088d877d9469b0d95b3e99e1b8a63.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000012_f62df05c4dc7875da08513cd529daafe4d985451c1979a2090ee4ff2934dd2bb.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000013_3efc3b6f067d6e2bd42438e8c17b0d9158d4edcc538570146f6456ac98c77c2c.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000014_91520d9d7dd92212810193c1b5ea5abeedd2bead65e67314380b0ff31880c8b0.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000015_a73daaec1b0416f89b19d19ca63fe16de594050734631ae3668453e3c8261496.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000016_431fe26b7d1fff943467b3b47246ff1afe6f55332a6d6f28b1d4239076467338.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000017_9f2169e2031d70eae62e2b47774621984bd114b8b3ed4ccd53a72e61b8e1d187.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000018_6eab7f3d76e40f564c7aea04b0d5d4e29525d5ee718c2ff942944a8b997fc027.png)

Table 5 The HV results of the proposed CoEA-DAE and four comparison algorithms on the ''X'' dataset. The running time (in seconds) consumed by these five algorithms is shown in the second brackets ''()''. Best result is highlighted in bold.

| Instance   | CCMO                           | MOEA                           | M-MOEA/D                       | KBEA                          | CoEA-DAE                     |
|------------|--------------------------------|--------------------------------|--------------------------------|-------------------------------|------------------------------|
| X-n101-k25 | 0.5269 (2.95e - 3) - (700.28)  | 0.4347 (1.29e - 2) - (579.41)  | 0.5171 (2.71e - 3) - (1088.84) | 0.3861 (4.87e - 2) - (176.89) | 0.5453 (5.33e - 3) (996.92)  |
| X-n106-k14 | 0.4783 (1.47e - 3) - (685.48)  | 0.4501 (5.00e - 3) - (547.67)  | 0.4545 (6.20e - 3) - (1049.07) | 0.3362 (1.07e - 1) - (49.73)  | 0.5017 (1.36e - 2) (948.95)  |
| X-n110-k13 | 0.6286 (3.10e - 3) - (741.06)  | 0.5326 (2.53e - 2) - (531.17)  | 0.6197 (5.36e - 3) - (1132.32) | 0.5968 (5.97e - 3) - (45.18)  | 0.7061 (1.63e - 2) (943.51)  |
| X-n115-k10 | 0.7252 (7.69e - 3) - (921.14)  | 0.6559 (1.18e - 2) - (558.02)  | 0.7049 (5.03e - 3) - (1423.94) | 0.6124 (4.69e - 2) - (116.57) | 0.7654 (1.33e - 2) (974.28)  |
| X-n120-k6  | 0.4861 (1.11e - 2) - (945.33)  | 0.4452 (1.64e - 2) - (576.20)  | 0.4494 (9.39e - 3) - (1434.11) | 0.4781 (2.64e - 2) - (58.07)  | 0.6734 (1.05e - 2) (998.19)  |
| X-n125-k30 | 0.4455 (2.17e - 3) - (1054.48) | 0.3917 (9.84e - 3) - (769.84)  | 0.4331 (2.66e - 3) - (1655.06) | 0.2941 (6.50e - 2) - (145.46) | 0.4683 (1.43e - 2) (1096.43) |
| X-n129-k18 | 0.5144 (3.72e - 3) - (1064.73) | 0.4271 (1.77e - 2) - (689.74)  | 0.5048 (3.25e - 3) - (1687.64) | 0.4721 (7.28e - 3) - (75.82)  | 0.5622 (5.81e - 3) (1310.34) |
| X-n134-k13 | 0.6575 (3.29e - 3) - (1128.87) | 0.5983 (1.23e - 2) - (711.15)  | 0.6502 (5.92e - 3) - (1838.70) | 0.6315 (2.82e - 2) - (73.23)  | 0.7394 (1.21e - 2) (1447.89) |
| X-n139-k10 | 0.7188 (3.07e - 3) - (1216.23) | 0.6315 (2.82e - 2) - (695.78)  | 0.7044 (7.02e - 3) - (1926.52) | 0.7053 (6.67e - 3) - (83.77)  | 0.7495 (2.50e - 2) (1369.83) |
| X-n143-k7  | 0.7169 (3.85e - 3) - (1373.14) | 0.6762 (1.05e - 2) - (758.19)  | 0.6960 (6.16e - 3) - (2141.09) | 0.6921 (1.57e - 2) - (86.62)  | 0.7410 (5.55e - 3) (1283.16) |
| X-n148-k46 | 0.5040 (2.53e - 3) ≈ (1486.82) | 0.3841 (1.81e - 2) - (1000.47) | 0.4945 (2.69e - 3) - (2393.50) | 0.354 (1.16e - 2) - (214.55)  | 0.5037 (7.40e - 3) (1503.97) |
| X-n153-k22 | 0.6201 (1.46e - 2) - (1679.65) | 0.5553 (1.62e - 2) - (1000.10) | 0.5940 (1.45e - 2) - (2784.70) | 0.3941 (4.19e - 2) - (262.28) | 0.6467 (7.77e - 3) (1626.20) |
| X-n157-k13 | 0.4312 (2.90e - 3) - (1455.01) | 0.3338 (2.07e - 2) - (833.81)  | 0.4067 (6.77e - 3) - (2255.10) | 0.4135 (9.22e - 4) - (47.74)  | 0.5421 (2.27e - 3) (2472.01) |
| X-n162-k11 | 0.7608 (6.67e - 3) - (1641.46) | 0.6952 (1.22e - 2) - (981.12)  | 0.7392 (8.79e - 3) - (2570.41) | 0.7550 (2.12e - 3) - (80.22)  | 0.7865 (2.38e - 3) (3166.36) |
| X-n167-k10 | 0.5766 (4.18e - 3) - (1785.87) | 0.5084 (1.43e - 2) - (944.01)  | 0.5590 (5.53e - 3) - (2783.35) | 0.5202 (7.03e - 2) - (104.22) | 0.5940 (1.04e - 2) (3322.86) |
| X-n172-k51 | 0.5727 (1.33e - 3) ≈ (2049.95) | 0.4558 (1.25e - 2) - (1337.40) | 0.5614 (2.35e - 3) - (3340.01) | 0.3335 (1.16e - 2) - (273.24) | 0.5726 (5.36e - 3) (3465.51) |
| X-n176-k26 | 0.5924 (6.15e - 3) + (2094.22) | 0.5204 (1.24e - 2) - (1131.83) | 0.5773 (8.69e - 3) - (3547.79) | 0.3212 (1.39e - 2) - (315.38) | 0.5901 (5.92e - 3) (3713.27) |
| X-n181-k23 | 0.3542 (1.92e - 3) - (1964.81) | 0.2689 (1.25e - 2) - (1073.92) | 0.3510 (1.93e - 3) - (3168.26) | 0.3475 (1.62e - 3) - (72.15)  | 0.3742 (6.06e - 3) (3837.60) |
| X-n186-k15 | 0.4493 (3.98e - 3) - (2113.89) | 0.6350 (9.43e - 3) - (1162.53) | 0.6020 (4.78e - 3) - (3310.08) | 0.7087 (3.59e - 3) - (82.63)  | 0.7253 (3.60e - 3) (3636.32) |
| X-n190-k8  | 0.4128 (6.56e - 3) - (2435.96) | 0.3798 (7.80e - 3) - (1163.83) | 0.3870 (5.50e - 3) - (3769.85) | 0.3906 (5.96e - 3) - (92.26)  | 0.5595 (9.09e - 3) (4236.96) |
| X-n195-k51 | 0.6101 (3.04e - 3) - (2694.47) | 0.4668 (1.17e - 2) - (1556.72) | 0.5967 (2.19e - 3) - (4068.10) | 0.3798 (3.88e - 2) - (307.04) | 0.6158 (6.72e - 3) (4835.15) |
| X-n200-k36 | 0.5801 (2.04e - 3) - (2566.97) | 0.4974 (1.31e - 2) - (1487.71) | 0.5619 (4.80e - 3) - (3792.51) | 0.4329 (1.36e - 2) - (102.61) | 0.5935 (4.89e - 3) (6148.86) |
| X-n204-k19 | 0.7641 (3.46e - 3) - (2414.78) | 0.6662 (1.81e - 2) - (1370.57) | 0.7519 (3.97e - 3) - (3890.76) | 0.7570 (8.49e - 4) - (67.26)  | 0.7804 (4.00e - 3) (5812.12) |
| X-n209-k16 | 0.6384 (5.45e - 3) - (2615.60) | 0.5543 (1.16e - 2) - (1326.53) | 0.6299 (3.53e - 3) - (4130.99) | 0.6382 (1.73e - 3) - (80.67)  | 0.6809 (3.87e - 3) (5838.80) |
| X-n214-k11 | 0.7235 (3.22e - 3) - (2978.01) | 0.6553 (6.53e - 3) - (1407.38) | 0.7089 (4.48e - 3) - (4637.73) | 0.6984 (1.26e - 2) - (153.40) | 0.7661 (5.34e - 3) (3148.40) |
| X-n219-k73 | 0.3089 (3.38e - 3) ≈ (3683.74) | 0.1680 (1.30e - 2) - (1503.16) | 0.3014 (4.41e - 3) - (4869.12) | 0.2806 (1.16e - 2) - (160.36) | 0.3087 (6.09e - 3) (3275.16) |
| X-n223-k34 | 0.6429 (5.33e - 3) - (3159.73) | 0.5095 (1.59e - 2) - (1591.47) | 0.6417 (4.11e - 3) - (4924.61) | 0.5789 (1.69e - 2) - (135.91) | 0.6647 (5.18e - 3) (2982.88) |
| +∕ -∕ ≈    | 1/23/3                         | 0/27/0                         | 0/27/0                         | 0/27/0                        |                              |

Table 6 Comparison of HV results obtained by CoEA-DAE and its four variants. Best result is highlighted in bold.

| Instance   | Variant-I            | Variant-II           | Variant-III          | Variant-IV            | CoEA-DAE           |
|------------|----------------------|----------------------|----------------------|-----------------------|--------------------|
| M-n101-k10 | 0.5880 (1.43e - 2) - | 0.6172 (2.63e - 3) - | 0.6138 (2.63e - 3) - | 0.6230 (3.28e - 2) -  | 0.6879 (3.63e - 3) |
| M-n121-k73 | 0.5911 (1.45e - 2) - | 0.6259 (6.23e - 3) - | 0.6152 (6.23e - 3) - | 0.6901 (3.16e - 2 ) - | 0.7523 (3.65e - 2) |
| M-n151-k12 | 0.6777 (1.23e - 2) - | 0.7038 (2.82e - 3) - | 0.7015 (2.82e - 3) - | 0.6641 (5.66e - 3) -  | 0.7279 (5.42e - 3) |
| M-n200-k16 | 0.6513 (1.27e - 2) - | 0.6749 (4.63e - 3) - | 0.6737 (4.63e - 3) - | 0.6637 (5.75e - 3) -  | 0.7274 (4.32e - 3) |
| M-n200-k17 | 0.6750 (1.16e - 2) - | 0.6996 (2.78e - 3) - | 0.6973 (2.78e - 3) - | 0.6828 (6.94e - 3) -  | 0.7448 (5.07e - 3) |
| X-n101-k25 | 0.4343 (1.30e - 2) - | 0.4571 (2.96e - 3) - | 0.4557 (2.95e - 3) - | 0.4613 (5.85e - 3) -  | 0.5130 (5.33e - 3) |
| X-n106-k14 | 0.4496 (4.95e - 3) - | 0.4723 (1.48e - 3) - | 0.4705 (1.47e - 3) - | 0.4408 (2.72e - 2) -  | 0.4907 (1.36e - 2) |
| X-n110-k13 | 0.5330 (2.51e - 2) - | 0.5559 (3.11e - 3) - | 0.5523 (3.10e - 3) - | 0.6051 (3.28e - 2) -  | 0.6461 (1.63e - 2) |
| X-n115-k10 | 0.6554 (1.17e - 2) - | 0.6791 (7.69e - 3) - | 0.6767 (7.69e - 3) - | 0.7258 (2.07e - 2) -  | 0.7654 (1.33e - 2) |
| X-n120-k6  | 0.4457 (1.63e - 2) - | 0.4680 (1.11e - 2) - | 0.4654 (1.11e - 2) - | 0.4468 (3.24e - 2) -  | 0.4734 (1.05e - 2) |
| X-n125-k30 | 0.3922 (9.79e - 3) - | 0.4162 (2.17e - 3) - | 0.4136 (2.17e - 3) - | 0.4419 (1.52e - 2) -  | 0.4683 (1.43e - 2) |
| X-n129-k18 | 0.4268 (1.75e - 2) - | 0.4503 (3.73e - 3) - | 0.4478 (3.72e - 3) - | 0.4860 (8.11e - 3) -  | 0.5122 (5.81e - 3) |
| X-n134-k13 | 0.5987 (1.21e - 2) - | 0.6270 (3.30e - 3) - | 0.6228 (3.29e - 3) - | 0.6330 (1.67e - 2) -  | 0.6594 (1.21e - 2) |
| X-n139-k10 | 0.6319 (2.80e - 2) - | 0.6601 (3.08e - 3) - | 0.6550 (3.07e - 3) - | 0.6890 (3.68e - 2) -  | 0.7396 (2.50e - 2) |
| X-n143-k7  | 0.6760 (1.03e - 2) - | 0.7009 (5.85e - 3) ≈ | 0.6997 (3.85e - 3) - | 0.6578 (3.33e - 2) -  | 0.7009 (5.55e - 3) |
| X-n148-k46 | 0.3839 (1.80e - 2) - | 0.4070 (2.53e - 3) - | 0.4026 (2.53e - 3) - | 0.4945 (1.11e - 2) -  | 0.5237 (7.40e - 3) |
| X-n153-k22 | 0.5550 (1.61e - 2) - | 0.5792 (1.46e - 2) - | 0.5756 (1.46e - 2) - | 0.6053 (1.87e - 2) -  | 0.6467 (7.77e - 3) |
| X-n157-k13 | 0.3342 (2.05e - 2) - | 0.4264 (2.90e - 3) - | 0.4202 (2.90e - 3) - | 0.5151 (2.09e - 2) -  | 0.5422 (2.27e - 3) |
| X-n162-k11 | 0.6949 (1.21e - 2) - | 0.7221 (6.67e - 3) - | 0.7160 (6.67e - 3) - | 0.6840 (4.12e - 3) -  | 0.7365 (2.38e - 3) |
| X-n167-k10 | 0.5080 (1.42e - 2) - | 0.5361 (4.19e - 3) - | 0.5316 (4.18e - 3) - | 0.5248 (4.92e - 2) -  | 0.5539 (1.04e - 2) |
| X-n172-k51 | 0.4555 (1.24e - 2) - | 0.4780 (1.33e - 3) - | 0.4732 (1.33e - 3) - | 0.5376 (3.81e - 2) -  | 0.5684 (5.36e - 3) |
| X-n176-k26 | 0.5199 (1.23e - 2) - | 0.5415 (6.16e - 3) - | 0.5367 (6.15e - 3) - | 0.5550 (2.65e - 2) -  | 0.5866 (5.92e - 3) |
| X-n181-k23 | 0.2692 (1.24e - 2) - | 0.3011 (1.92e - 3) - | 0.2955 (1.92e - 3) - | 0.3148 (7.06e - 2) -  | 0.3342 (6.06e - 3) |
| X-n186-k15 | 0.6347 (9.38e - 3) - | 0.6605 (3.99e - 3) - | 0.6545 (3.98e - 3) - | 0.6513 (3.73e - 2) -  | 0.6853 (3.60e - 3) |
| X-n190-k8  | 0.3795 (7.75e - 3) - | 0.3817 (6.56e - 3) - | 0.3764 (6.56e - 3) - | 0.3987 (6.31e - 2) ≈  | 0.3985 (9.09e - 3) |
| X-n195-k51 | 0.4663 (1.16e - 2) - | 0.4904 (3.04e - 3) - | 0.4837 (3.04e - 3) - | 0.5707 (4.51e - 2) -  | 0.6158 (6.72e - 3) |
| X-n200-k36 | 0.4972 (1.29e - 2) - | 0.5238 (2.04e - 3) - | 0.5174 (2.04e - 3) - | 0.5267 (3.08e - 2) -  | 0.5555 (4.89e - 3) |
| X-n204-k19 | 0.6622 (1.79e - 2) - | 0.6899 (3.47e - 3) - | 0.6858 (3.46e - 3) - | 0.6901 (5.56e - 2) -  | 0.7408 (4.00e - 3) |
| X-n209-k16 | 0.5540 (1.14e - 2) - | 0.5797 (5.46e - 3) - | 0.5751 (5.45e - 3) - | 0.5701 (3.23e - 2) -  | 0.6009 (3.87e - 3) |
| X-n214-k11 | 0.6551 (6.49e - 3) - | 0.6781 (3.22e - 3) - | 0.6739 (3.22e - 3) - | 0.6456 (3.98e - 2) -  | 0.6762 (5.34e - 3) |
| X-n219-k73 | 0.1682 (1.29e - 2) - | 0.2138 (3.38e - 3) - | 0.2096 (3.38e - 3) - | 0.2788 (5.79e - 2) -  | 0.3087 (6.09e - 3) |
| X-n223-k34 | 0.5091 (1.58e - 2) - | 0.5379 (5.34e - 3) - | 0.5336 (5.33e - 3) - | 0.5844 (4.18e - 2) -  | 0.6105 (5.18e - 3) |
| +∕ -∕ ≈    | 0/32/0               | 0/31/1               | 0/32/0               | 0/31/1                |                    |

Fig. 6. The optimal solution roadmap of CoEA-DAE on different instances.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000019_e87d73141ceb04d83464ff9c18754134b6c4646cff6b74c4d22601d97d1b931d.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000020_076df99e9d2dc846f7f47ca9e94cd685cc9d044c1e4c8a06167607d5bbc901cc.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000021_f3aa81aea17d3e6840ff6e717139394585d21a70921a9c8a47a7ae21830b07cc.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000022_ac54de747388bd4042adc08e8deb61cdc0db8429fa7123175d229c2877bedbcd.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000023_c92add1029e23bc3b847c66943e91b1302b841d8c9f7f4d1c317637d39602705.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000024_c5bdf291b08970d58cb0f158c47999103869c889eb6854d4b9574ab9c670fb94.png)

## 5. Conclusions and future work

In this paper, a co-evolutionary genetic algorithm with knowledge transfer was proposed to address the capacitated vehicle routing problem with workload balancing. To expedite the convergence speed of evolutionary algorithms in solving this problem, this article used a knowledge transfer method that constructs a simplified problem to aid in the rapid solution of the original problem. Specifically, the TSP is derived from MO-CVRP by disregarding the capacity constraint, ensuring similarity with the original problem and facilitating the subsequent migration process. Furthermore, a modified split operator was designed to better satisfy the objective of load balance minimization. Additionally, an adaptive migration strategy was proposed to combine the advantages of direct migration and denoising autoencoder-based migration, generating diverse and promising offspring for the main population. The effectiveness of the knowledge transfer method was verified through experiments, and comparisons with state-of-the-art algorithms demonstrate the competitive performance of the proposed approach in most instances.

Despite the competitive performance demonstrated by the proposed method across various test instances, it is important to acknowledge certain limitations. Firstly, as the problem size increases, the efficiency of the proposed method may diminish due to the increased dimensionality of the input and output, which significantly prolongs the training time. Secondly, the structure of the DAE needs to be adjusted according to the problem size. Specifically, different DAE models must be trained in an online manner for different problem sizes. To address this, a learning model with strong generalization ability should be developed, incorporating a specific embedding layer to fix the input size. This would allow for offline training of a single model, significantly reducing the running time. Additionally, it would be of interest to explore the knowledge transfer based coevolution framework for other complex

Fig. 7. Illustration of the objective values of the final solutions obtained by the proposed method and the comparison algorithms.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000025_a6886d219dda95e80a9bbd1ba426ddca648196e1a8881d3129f8a1635a22453f.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000026_ea3d66e5a4e836ccdda3e9ed9ef2cbaf53323009c743f797129b94372be81729.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000027_1c31e0f0b0c2776d66dd606c51ec18f4d1897f93031584b0376ff85e9e27f6b0.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023b] A co-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems_artifacts/image_000028_860d9674c9781e5dfa11a97d8b5149c38f5c2dd8a6d5d4c4d1ed7f76b3430051.png)

CVRP variants, such as CVRP with time windows [55] and CVRP with stochastic demands [56].

## References

## CRediT authorship contribution statement

Chao Wang: Investigation, Software, Validation, Writing. Biao Ma: Conceptualization, Methodology, Software. Jiye Sun: Validation, Writing.

## Declaration of competing interest

We declare that we have no financial and personal relationships with other people or organizations that can inappropriately influence our work, there is no professional or other personal interest of any nature or kind in any product, service and/or company that could be construed as influencing the position presented in, or the review of, the manuscript entitled.

## Data availability

Data will be made available on request.

## Acknowledgments

This work was supported in part by the National Natural Science Foundation of China under Grant 62106002; and in part by the Natural Science Foundation of Anhui Province under Grant 2008085QF308.

- [1] Thibaut Vidal, Gilbert Laporte, Piotr Matl, A concise guide to existing and emerging vehicle routing problem variants, European J. Oper. Res. 286 (2019) 401-416.
- [2] Diego Cattaruzza, Nabil Absi, Dominique Feillet, Jesús González-Feliu, Vehicle routing problems for city logistics, EURO J. Transp. Logist. 6 (1) (2017) 51-79.
- [3] Ricardo Fukasawa, Humberto Longo, Jens Lysgaard, Marcus Poggi de Aragão, Marcelo Reis, Eduardo Uchoa, Renato F Werneck, Robust branch-and-cut-andprice for the capacitated vehicle routing problem, Math. Program. 106 (3) (2006) 491-511.
- [4] Paolo Toth, Daniele Vigo, Models, relaxations and exact approaches for the capacitated vehicle routing problem, Discrete Appl. Math. 123 (1-3) (2002) 487-512.
- [5] Jiahai Wang, Taiyao Weng, Qingfu Zhang, A two-stage multiobjective evolutionary algorithm for multiobjective multidepot vehicle routing problem with time windows, IEEE Trans. Cybern. 49 (7) (2019) 2467-2478.
- [6] Sophie N. Parragh, Karl F. Doerner, Richard F. Hartl, A survey on pickup and delivery problems, J. Betr. 58 (1) (2008) 21-51.
- [7] Fabien Lehuédé, Olivier Péton, Fabien Tricoire, A lexicographic minimax approach to the vehicle routing problem with route balancing, European J. Oper. Res. 282 (1) (2020) 129-147.
- [8] Yong Wang, Yuanhan Wei, Xiuwen Wang, Zheng Wang, Haizhong Wang, A clustering-based extended genetic algorithm for the multidepot vehicle routing problem with time windows and three-dimensional loading constraints, Appl. Soft Comput. 133 (2023) 109922.
- [9] Asma M. Altabeeb, Abdulqader M. Mohsen, Abdullatif Ghallab, An improved hybrid firefly algorithm for capacitated vehicle routing problem, Appl. Soft Comput. 84 (2019) 105728.
- [10] Lars Magnus Hvattum, Adjusting the order crossover operator for capacitated vehicle routing problems, Comput. Oper. Res. 148 (2022) 105986.
- [11] Kam K.H. Ng, C.K.M. Lee, Shuzhu Zhang, Kan Wu, William Ho, A multiple colonies artificial bee colony algorithm for a capacitated vehicle routing problem and re-routing strategies under time-dependent traffic congestion, Comput. Ind. Eng. 109 (2017) 151-168.
- [12] Adam N. Letchford, Juan José Salazar-González, The capacitated vehicle routing problem: Stronger bounds in pseudo-polynomial time, European J. Oper. Res. 272 (2019) 24-31.

| [13]   | Luca Accorsi, Daniele Vigo, A fast and scalable heuristic for the solution of large-scale capacitated vehicle routing problems, Transp. Sci. 55 (4) (2021) 832-856.                                                                                                                                               |
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [14]   | Zizhen Zhang, Yuyan Sun, Hong Xie, Yi Teng, Jiahai Wang, GMMA: GPU- based multiobjective memetic algorithms for vehicle routing problem with route balancing, Appl. Intell. 49 (2018) 63-78.                                                                                                                      |
| [15]   | Jingjing Li, Yaohuiqiong Fang, Na Tang, A cluster-based optimization framework for vehicle routing problem with workload balance, Comput. Ind. Eng. 169 (2022) 108221.                                                                                                                                            |
| [16]   | Nicolas Jozefowiez, Frédéric Semet, El-Ghazali Talbi, Target aiming Pareto search and its application to the vehicle routing problem with route balancing, J. Heuristics 13 (2007) 455-469.                                                                                                                       |
| [17]   | Santosh Kumar Mandal, Dario Pacciarelli, Arne Løkketangen, Geir Hasle, A memetic NSGA-II for the bi-objective mixed capacitated general routing problem, J. Heuristics 21 (2015) 359-390.                                                                                                                         |
| [18]   | Yuyan Sun, Yuxuan Liang, Zizhen Zhang, Jiahai Wang, M-NSGA-II: A memetic algorithm for vehicle routing problem with route balancing, in: Salem Benferhat, Karim Tabia, Moonis Ali (Eds.), Advances in Artificial Intelligence: From Theory to Practice, Springer International Publishing, Cham, 2017, pp. 61-71. |
| [19]   | Jonathan F. Bard, Liu Huang, Moshe Dror, Patrick Jaillet, A branch and cut algorithm for the VRP with satellite facilities, IIE Trans. 30 (9) (1998) 821-834.                                                                                                                                                     |
| [20]   | Niklas Kohl, Jacques Desrosiers, Oli BG Madsen, Marius M Solomon, Francois Soumis, 2-path cuts for the vehicle routing problem with time windows, Transp. Sci. 33 (1) (1999) 101-116.                                                                                                                             |
| [21]   | Adam N. Letchford, Juan-José Salazar-González, The capacitated vehicle routing problem: Stronger bounds in pseudo-polynomial time, European J. Oper. Res. 272 (1) (2019) 24-31.                                                                                                                                   |
| [22]   | Ricardo Fukasawa, Humberto J. Longo, Jens Lysgaard, Marcus Poggi de Aragão, Marcelo L. Reis, Eduardo Uchoa, Renato F. Werneck, Robust branch-and-cut-and- price for the capacitated vehicle routing problem, Math. Program. 106 (2004) 491-511.                                                                   |
| [23]   | Shen Lin, Brian W. Kernighan, An effective heuristic algorithm for the traveling-salesman problem, Oper. Res. 21 (2) (1973) 498-516.                                                                                                                                                                              |
| [24]   | Keld Helsgaun, An effective implementation of the Lin-Kernighan traveling salesman heuristic, European J. Oper. Res. 126 (2000) 106-130.                                                                                                                                                                          |
| [25]   | Anand Subramanian, Eduardo Uchoa, Luiz Satoru Ochi, A hybrid algorithm for a class of vehicle routing problems, Comput. Oper. Res. 40 (10) (2013) 2519-2531.                                                                                                                                                      |
| [26]   | Keld Helsgaun, An extension of the Lin-Kernighan-Helsgaun TSP solver for constrained traveling salesman and vehicle routing problems: Technical report, 2017.                                                                                                                                                     |
| [27]   | John E. Bell, Patrick R. McMullen, Ant colony optimization techniques for the vehicle routing problem, Adv. Eng. Inform. 18 (1) (2004) 41-48.                                                                                                                                                                     |
| [28]   | Petr Stodola, Jan Mazal, Milan Podhorec, Ondrej Litvaj, Using the ant colony optimization algorithm for the capacitated vehicle routing problem, in: Proceed- ings of the 16th International Conference on Mechatronics - Mechatronika 2014, 2014, pp. 503-510.                                                   |
| [29]   | Xinyu Wang, Tsan-Ming Choi, Haikuo Liu, Xiaohang Yue, Novel ant colony optimization methods for simplifying solution construction in vehicle routing problems, IEEE Trans. Intell. Transp. Syst. 17 (2016) 3132-3141.                                                                                             |
| [30]   | Joseph H. Wilck, Tom M. Cavalier, A genetic algorithm for the split delivery vehicle routing problem, Am. J. Oper. Res. 2 (2012) 207-216.                                                                                                                                                                         |
| [31]   | Thibaut Vidal, Hybrid genetic search for the CVRP: Open-source implementation and swap* neighborhood, Comput. Oper. Res. 140 (2020) 105643.                                                                                                                                                                       |
| [32]   | Lars Magnus Hvattum, Adjusting the order crossover operator for capacitated vehicle routing problems, Comput. Oper. Res. 148 (2022) 105986.                                                                                                                                                                       |
| [33]   | Thibaut Vidal, Hybrid genetic search for the CVRP: Open-source implementation and SWAP* neighborhood, Comput. Oper. Res. 140 (2022) 105643.                                                                                                                                                                       |
| [34]   | Nicolas Jozefowiez, Frédéric Semet, El-Ghazali Talbi, An evolutionary algorithm for the vehicle routing problem with route balancing, European J. Oper. Res. 195 (2009) 761-769.                                                                                                                                  |
| [35]   | Philippe Lacomme, Christian Prins, Caroline Prodhon, Libo Ren, A multi-start split based path relinking (MSSPR) approach for the vehicle routing problem                                                                                                                                                          |

| [36]   | Simona Mancini, Margaretha Gansterer, Richard F. Hartl, The collaborative consistent vehicle routing problem with workload balance, European J. Oper. Res. 293 (3) (2021) 955-965.                                                    |
|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [37]   | Liang Feng, Yew-Soon Ong, Meng-Hiot Lim, Ivor W. Tsang, Memetic search with interdomain learning: A realization between cvrp and CARP, IEEE Trans. Evol. Comput. 19 (5) (2014) 644-658.                                               |
| [38]   | Liang Feng, Yuxiao Huang, Ivor W Tsang, Abhishek Gupta, Ke Tang, Kay Chen Tan, Yew-Soon Ong, Towards faster vehicle routing by transferring knowledge from customer representation, IEEE Trans. Intell. Transp. Syst. (2020).         |
| [39]   | Mohammad Nabi Omidvar, Xiaodong Li, Yi Mei, Xin Yao, Cooperative co- evolution with differential grouping for large scale optimization, IEEE Trans. Evol. Comput. 18 (3) (2013) 378-393.                                              |
| [40]   | Fuzhen Zhuang, Zhiyuan Qi, Keyu Duan, Dongbo Xi, Yongchun Zhu, Hengshu Zhu, Hui Xiong, Qing He, A comprehensive survey on transfer learning, Proc. IEEE 109 (1) (2020) 43-76.                                                         |
| [41]   | Yoshua Bengio, Aaron Courville, Pascal Vincent, Representation learning: A review and new perspectives, IEEE Trans. Pattern Anal. Mach. Intell. 35 (8) (2013) 1798-1828.                                                              |
| [42]   | Kalyanmoy Deb, Samir Agrawal, Amrit Pratap, T. Meyarivan, A fast and elitist multiobjective genetic algorithm: NSGA-II, IEEE Trans. Evol. Comput. 6 (2002) 182-197.                                                                   |
| [43]   | Liang Feng, Lei Zhou, Jinghui Zhong, Abhishek Gupta, Y. Ong, Kay Chen Tan, A.K. Qin, Evolutionary multitasking via explicit autoencoding, IEEE Trans. Cybern. 49 (2019) 3457-3470.                                                    |
| [44]   | Christian Prins, A simple and effective evolutionary algorithm for the vehicle routing problem, Comput. Oper. Res. 31 (12) (2004) 1985-2002.                                                                                          |
| [45]   | Nicos Christofides, Aristide Mingozzi, Paolo Toth, Exact algorithms for the vehicle routing problem, based on spanning tree and shortest path relaxations, Math. Program. 20 (1981) 255-282.                                          |
| [46]   | Eduardo Uchoa, Diego Pecin, Artur Alves Pessoa, Marcus Poggi de Aragão, Thibaut Vidal, Anand Subramanian, New benchmark instances for the capacitated vehicle routing problem, European J. Oper. Res. 257 (2017) 845-858.             |
| [47]   | Abel Garcia-Najera, John A. Bullinaria, An improved multi-objective evolutionary algorithm for the vehicle routing problem with time windows, Comput. Oper. Res. 38 (1) (2011) 287-300.                                               |
| [48]   | Yutao Qi, Zhanting Hou, He Li, Jianbin Huang, Xiaodong Li, A decomposition based memetic algorithm for multi-objective vehicle routing problem with time windows, Comput. Oper. Res. 62 (2015) 61-77.                                 |
| [49]   | Tsung-Che Chiang, Wei-Huai Hsu, A knowledge-based evolutionary algorithm for the multiobjective vehicle routing problem with time windows, Comput. Oper. Res. 45 (2014) 25-37.                                                        |
| [50]   | Ye Tian, Tao Zhang, Jianhua Xiao, Xingyi Zhang, Yaochu Jin, A coevolutionary framework for constrained multiobjective optimization problems, IEEE Trans. Evol. Comput. 25 (1) (2020) 102-116.                                         |
| [51]   | Odai Y. Dweekat, Sarah S. Lam, Cervical cancer diagnosis using an integrated system of principal component analysis, genetic algorithm, and multilayer perceptron, in: Healthcare, Vol. 10, MDPI, 2022, p. 2002.                      |
| [52]   | Odai Y. Dweekat, Sarah S. Lam, Optimized design of hybrid genetic algorithm with multilayer perceptron to predict patients with diabetes, Soft Comput. 27 (10) (2023) 6205-6222.                                                      |
| [53]   | Eckart Zitzler, Lothar Thiele, Multiobjective evolutionary algorithms: a compar- ative case study and the strength Pareto approach, IEEE Trans. Evol. Comput. 3 (4) (1999) 257-271.                                                   |
| [54]   | J. Derrac, S. Garcia, D. Molina, F. Herrera, A practical tutorial on the use of nonparametric statistical tests as a methodology for comparing evolutionary and swarm intelligence algorithms, Swarm Evol. Comput. 1 (1) (2011) 3-18. |
| [55]   | Jianhua Xiao, Jingguo Du, Zhiguang Cao, Xingyi Zhang, Yunyun Niu, A diversity- enhanced memetic algorithm for solving electric vehicle routing problems with time windows and mixed backhauls, Appl. Soft Comput. 134 (2023) 110025.  |
| [56]   | Xiaoyun Xia, Weizhi Liao, Yu Zhang, Xue Peng, A discrete spider monkey optimization for the vehicle routing problem with stochastic demands, Appl. Soft                                                                               |