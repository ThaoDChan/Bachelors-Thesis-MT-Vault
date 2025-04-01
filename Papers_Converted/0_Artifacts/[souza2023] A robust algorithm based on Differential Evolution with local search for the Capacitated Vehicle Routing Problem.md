![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000000_aeba0662fe9bddfc3b818c92196ddd0658e33af1bca143bee5482a6ee902225e.png)

Contents lists available at ScienceDirect

## SwarmandEvolutionary Computation

journal homepage: www.elsevier.com/locate/swevo

## Arobust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem

Israel Pereira Souza a, ∗ , Maria Claudia Silva Boeres a , Renato Elias Nunes Moraes b a Department of Informatics, Federal University of Espírito Santo, Av. Fernando Ferrari, 514 - Campus de Goiabeiras, Vitória, 29075-910, Espírito Santo, Brazil b Department of Industrial Technology, Federal University of Espírito Santo, Av. Fernando Ferrari, 514 - Campus de Goiabeiras, Vitória, 29075-910, Espírito Santo, Brazil

## A R T I C L E I N F O

## A B S T R A C T

Keywords: City logistics Metaheuristics Local search

Capacitated vehicle routing problem

Differential Evolution

Discrete optimization

The Capacitated Vehicle Routing Problem is a well-known combinatorial problem. In this paper, we propose a hybrid algorithm based on a discrete adaptation of the Differential Evolution metaheuristic, which is designed for continuous problems, combined with local search procedures to solve CVRP. An individual in the algorithm represents a CVRP solution. The algorithm presents a discrete adaptation for DE that consists on exchanging customers considering their routes positions, instead of arithmetically modifying the individuals. The proposed algorithm, called CDELS, is compared with three state-of-the-art algorithms. The computational experiments are performed on six classical datasets, with an extensive study for parameter tuning. A statistical analysis of the results was also conducted. It showed that the CDELS algorithm is highly significantly better than state-of-the-art methods with a confidence level of 99%. CDELS is open-source.

## 1. Introduction

The Vehicle Routing Problem (VRP) is a well know logistical model to minimize costs for delivering goods to customers with transportation economic impacts. The model aims to optimize traffic and reduce transportation trajectories which can directly decrease the final cost of the logistical processes.

Dantzig and Ramser in 1959 were the first to introduce the VRP with the proposal of a mathematical programming formulation and algorithms to model the delivery of gasoline to service stations [1]. In 1964 Clarke and Wright improved this approach by proposing an effective greedy heuristic [2]. Afterward, the problem was derived into several variants, such as VRP with time windows, multi-depot VRP, VRP with pickup and delivery, VRP with backhauls, split delivery VRP, and periodic VRP, among others.

VRP is an NP-hard [3] problem with several real-world applications. The classical VRP, also known as the Capacitated VRP (CVRP), designs optimal delivery routes where each vehicle should travel a single route. Each route must start at the depot, visit a subset of customers, and then return to the depot [4]. The goal of the CVRP is to find a set of least-cost routes where each customer is visited once by one vehicle. Each vehicle has a maximum capacity, which limits the number of customers it can visit before returning to the depot.

CVRP can be exactly solved only for small instances, thus, heuristics and metaheuristics are often more suitable for practical applications.

∗ Corresponding author. E-mail address: israel.souza@edu.ufes.br (I.P. Souza).

In this paper, we propose a new metaheuristic approach based on the Differential Evolution (DE) algorithm [5,6] and analyze its behavior through computational tests in solving CVRP.

## 1.1. Metaheuristic approaches for CVRP

Recent literature on CVRP involves many metaheuristic methodologies which have been proven to be remarkably effective even when there is limited computation capacity. Traditional metaheuristic methods orchestrate an interaction between local improvement procedures and higher-level strategies to create a process capable of escaping from local optima and performing a robust search of a solution space [7]. Unlike traditional methods, evolutionary algorithms in general build on the idea that natural evolution is a powerful adaptive system that is responsible for the diversity and adaptation of all life on earth [8]. Approaches that take advantage of the strengths of each of their metaheuristic components to better explore the solution space are named hybrid methods [7].

Considering traditional methods regarding CVRP, a variety of metaheuristics have been proposed such as variable neighborhood search [9], simulated annealing [10,11], GRASP [12], tabu search [13,14], among others. In addition to the traditional methods, over the last few years, several researchers have focused to develop hybrid methods and evolutionary algorithms to solve CVRP.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000001_203fe1ea644fc9e5a34ee3c80959bd90f7706788c2b82d737112fe0d5c508df7.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000002_0b48e150c1b98dcdb41689e41befb6d42f680229f141d90db6afeb8b0abbcef6.png)

Recent works [15-19] bring an extensive up-to-date literature review on metaheuristics applied to solve the CVRP problem, especially, hybridization techniques and/or population-based models. Tan and Yeh [20] identified the trends of VRP variants and the algorithms applied to solve them. Additionally, they considered the papers with the most citations to be the most significant and discussed them in detail. They also presented a classification table to guide future researchers to find relevant literature easily.

The extensive variety of metaheuristics proposed for the CVRP problem led us to develop a classification of the recent literature to select relevant metaheuristics to compare with our proposal. The classification was carried out through a bibliography search in Scopus dataset, using the string: ( ''cvrp'' OR ''capacitated vehicle routing problem'' ) AND ( ''metaheuristic'' OR ''meta-heuristic'' OR ''exact algorithm'' OR ''matheuristic'' OR ''hyper-heuristic'' ). The search limited to the years 2018-2022 returned 107 articles. In the following, the returned articles and also those used in their comparisons were ranked by the number of instances they have achieved the optimum solution. The three algorithms with the highest scores were: (i) a hybrid Genetic Variable Neighborhood Search Algorithm [21]; (ii) a two-phase heuristic combining LNS with Set Partitioning (2PH) [22] and (iii) a matheuristic GRASP + VNS (MGV-CP) [18].

Sbai et al. [21] develop a hybrid metaheuristic that embeds a Variable Neighborhood Search in a Genetic Algorithm. After a series of experiments that cover about 186 instances, they noted that the proposed algorithm performs better than most of the state-of-the-art methods. Pelletier et al. [22], among other results, proposed and applied a two-phase metaheuristic to solve CVRP. Machado et al. [18] present a hybrid matheuristic in two stages to solve CVRP by applying Greedy Randomized Adaptive Search Procedure, mathematical models and Variable Neighborhood Search.

For purposes of comparison, computational experiments (discussed in Section 5.2) were performed with the selected [18,21,22] and proposed algorithms considering six datasets from different authors obtained from CVRPLIB [23].

In conclusion, analyzing the recent literature reviews, it is possible to verify that among the best-published metaheuristics for the CVRP are hybrid and evolutionary approaches. Thus, in this paper, we propose a hybrid metaheuristic that combines a population-based evolutionary algorithm with local search procedures.

## 1.2. DE methods for combinatorial problems

Differential Evolution (DE) is a population-based evolutionary algorithm originally proposed for solving continuous optimization problems [5]. Solutions to a problem solved by DE are treated as individuals or chromosomes which are organized into populations. For every DE iteration, each chromosome in the population is designated as a target and DE employs mutation, crossover, and selection to create new candidate solutions.

Although DE was devised mainly for real parameter optimization, over the past few years, some works have been using DE to deal with discrete values. Two different methods can be highlighted in the literature review. Encoding methods are based on the application of some transformation scheme and a discrete method, named Discrete Differential Evolution (DDE) [24]. DDE implements mutation and crossover operators designed to be applied directly over the discrete solution representation such that the algorithm can directly work in the discrete domain [25].

According to [26], two encoding methods have been utilized to represent the candidate solutions of combinatorial optimization problems: real-valued vectors and arrays of countables (binary or integer values). Still according to [26], when the candidate solutions are encoded with countable values representing permutations, specialized variation operators such as the permutation matrix [6], the adjacency matrix [6], the list of movements scheme [27], the geometric search operators [28], and the algebraic-based scheme [29] are needed to generate feasible solutions.

Following the real-valued mapping strategy, Qian et al. [30] has proposed the Largest Order Value ( LOV ) to use DE for solving the discrete multi-objective permutation flow shop scheduling problem. LOV is based on random keys representation [31]. It converts DEs individuals to the job solution/permutation vectors. Sauer and Coelho [32] used the Smallest Position Value (SPV) [33,34] rule, borrowed from the random key representation of [31], for DE's solutions representation.

Recently, considering the multi-skill resource-constrained project scheduling problem, Myszkowski et al. [35] proposed hybrid Differential Evolution and Greedy Algorithm with a specialized indirect representation and transformation of solution space from discrete (typical for the problem), to continuous (typical for DE-approaches). Zhou et al. [36] studied a reentrant hybrid flow shop problem scheduling problem and presented, among other results, a hybrid differential evolution algorithm with an estimation of distribution algorithm using an ensemble model. They used two continuous-discrete transformation techniques known as forward and backward transformation proposed in [37].

Ali et al. [38] presented a review with DE for solving discrete traveling salesman problems (TSP) and proposed a novel discrete differential evolution algorithm. They used the ranked-order value (ROV) [39] mapping method that is based on the random key representation [31] to map continuous variables to discrete ones. The continuous representation is used by DE to produce continuous individuals which are a map to discrete individuals using a mapping method called Best-matched value (BMV) [40].

Algorithms associated with discrete differential evolution (DDE) were proposed in [24,25,41,42]. Pan et al. [24] proposed a novel discrete differential evolution algorithm for permutation flowshop scheduling problems. In the DDE they proposed, solutions are based on discrete item permutations. A mutant individual is obtained by perturbing the previous best solution of generation in the target population so that the differential variation is stochastically achieved through perturbation of the best solution from the previous generation in the target population. They also pointed out the characteristics that make their proposal different from the traditional GA in terms of solution generation schemes.

Wang et al. [25] proposed an enhanced DDE algorithm (DDE) for solving blocking flow shop scheduling problems to minimize the maximum completion time. They proposed a novel job-permutationbased mutation and crossover operators, and a problem-dependent local search. [41] proposed modifications to DDE presented by [24] to solve the Quadratic Assignment Problem (QAP). More recently, [42] used a DDE algorithm to solve QAP. For additional related literature with discrete and combinatorial applications of DE algorithms, the reader is referred to [43,44].

Regarding the VRP problem variants Mingyong and Erbao [45] proposes an improved DE algorithm for the VRP-SPDTW, which considers simultaneous pickups and deliveries and time windows. They proposed the IOR, which is based on the mapping strategy (LOV) that Qian et al. [30] proposed. Hou et al. [46] presented a DDE that can be used in the discrete domain directly for solving stochastic vehicle routing problems with simultaneous pickups and deliveries (VRPSPD). Xu and Wen [47] solves the unidirectional logistics distribution vehicle routing problem (ULVRP) proposing a novel differential evolution algorithm DE using an appropriate encoding method considering the particular characteristics of VRP. In [48], a list of literature on VRP variants and the DE approaches used to solve them are presented. They also proposed a DE algorithm for CVRP using the DE continuous-discrete mapping proposed in Mingyong and Erbao [45]. Dechampai et al. [49] proposed a differential evolution algorithm using encoding and decoding methods to represent the vector population and its parameters as continuous values. Song and Dong [50] proposes an improved DE algorithm with local search to solve the CVRP using an individual

Table 1 A comparative study of the existing literature.

| Author (year)              | DE method   | DE method   | Hybrid Meta-   | Local search   | Parameter   | Statistical   | Open   |
|----------------------------|-------------|-------------|----------------|----------------|-------------|---------------|--------|
|                            | Encoding    | Discrete    | heuristic      | procedure      | calibration | analysis      | source |
| Pan et al. (2008)          |             | ✓           | ✓              | ✓              |             |               |        |
| Sauer and Coelho (2008)    | ✓           |             | ✓              | ✓              |             |               |        |
| Qian et al. (2009)         | ✓           |             | ✓              | ✓              |             |               |        |
| Hou et al. (2010)          |             | ✓           |                |                |             |               |        |
| Mingyong and Erbao (2010)  | ✓           |             |                |                |             |               |        |
| Prado et al. (2010)        | ✓           |             |                |                |             |               |        |
| Wang et al. (2010)         |             | ✓           | ✓              | ✓              |             |               |        |
| Xu and Wen (2012)          | ✓           |             |                |                |             |               |        |
| Moraglio et al. (2013)     | ✓           |             |                |                |             |               |        |
| Tasgetiren et al. (2013)   |             | ✓           |                |                |             |               |        |
| Teoh et al. (2015)         | ✓           |             | ✓              | ✓              | ✓           | ✓             |        |
| Santucci et al. (2016)     | ✓           |             | ✓              | ✓              | ✓           |               |        |
| Dechampai et al. (2017)    | ✓           |             |                |                |             |               |        |
| Song and Dong (2018)       | ✓           |             | ✓              | ✓              |             |               |        |
| Ali et al. (2020)          | ✓           |             | ✓              | ✓              | ✓           | ✓             |        |
| Hameed et al. (2020)       |             | ✓           | ✓              | ✓              |             |               |        |
| Rivera-López et al. (2020) | ✓           |             |                |                | ✓           | ✓             |        |
| Pelletier et al. (2019)    |             |             |                | ✓              | ✓           |               |        |
| Sbai et al. (2020)         |             |             | ✓              | ✓              |             |               |        |
| Machado et al. (2021)      |             |             | ✓              | ✓              | ✓           | ✓             |        |
| This paper                 |             | ✓           | ✓              | ✓              | ✓           | ✓             | ✓      |

representation derived from a random decimal. Table 1 presents a comparative study of the existing literature.

Since many researchers are working on the task of developing a suitable discrete variant of DE for making it compatible with discrete and combinatorial optimization problems [51], in this work we present a hybrid metaheuristic that combines DE with local search procedures. We describe a new adaptation in the mutation and crossover DE operators for tackling the discrete CVRP optimization problem. The proposed operators allow DE to be used in CVRP over the discrete space. Instead of applying the operator directly to the content of individual positions as in the classical DE algorithm, our operators perform exchanges between pairs of positions of the individuals, where each position represents a customer visit in the CVRP solution. We also propose a local search procedure to improve the solutions found by DE.

The contributions of this paper are as follows:

- · A discrete DE adaptation designed for the CVRP problem, which includes the mutation and crossover operators.
- · Two local search procedures.
- · An extensive parameter calibration of 𝐹 and CR parameters and their application in four DE techniques considering CVRP.
- · A statistical analysis that demonstrates that CDELS is highly significantly better than the state-of-the-art methods with a confidence level of 99%.
- · The complete development code is available on GitHub.

The rest of the article is organized as follows. Section 2 presents the description of the problem studied in this work and its mathematical formulation. Section 3 briefly describes the Differential Evolution in its original version presented by Rainer Storn and Kenneth Price [5]. Our proposed hybrid algorithm for the CVRP, CDELS algorithm, is presented in Section 4. Section 5 discusses the algorithm results obtained over six instance packages from the literature, together with its parameters calibration and statistical study. Section 6 concludes this work.

## 2. Problem description

The Capacitated Vehicle Routing Problem (CVRP) can be defined using a complete undirected graph 𝐺 = ( 𝑉 , 𝐸 ) , where 𝑉 = { 𝑐 0 , 𝑐 1 , … , 𝑐 𝑁 } is the vertex set and 𝐸 = {( 𝑐 , 𝑐 𝑖 𝑗 ) ∶ 𝑐 , 𝑐 𝑖 𝑗 ∈ 𝑉 , 𝑖 &lt; 𝑗 } is the edge set. Vertices 𝑐 1 , … , 𝑐 𝑁 represent customers (or clients) and each customer 𝑐 𝑖 is associated with a nonnegative demand 𝑞 𝑖 . Vertex 𝑐 0 represents the depot at which a fleet of homogeneous vehicles of capacity 𝑄 will depart. Depot has no associated demand. Each edge ( 𝑐 , 𝑐 𝑖 𝑗 ) ∈ 𝐸 is associated to a nonnegative traveling cost or distance 𝑑 𝑖𝑗 . CVRP seeks to minimize a set of vehicle routes such that: (a) every route starts and ends at the depot, (b) every customer is visited once, (c) the total demand of a route does not exceed 𝑄 , and (d) the number of routes does not surpass the number of vehicles 𝐾 .

Input parameters:

- 𝑁 : number of customers.
- 𝐾 : maximum number of vehicles.
- 𝑄 : maximum capacity of each vehicle.
- 𝑑 𝑖𝑗 : distance between vertices 𝑐 𝑖 and 𝑐 𝑗 , with 𝑑 𝑖𝑗 = 𝑑 𝑗𝑖 ∀ 𝑖, 𝑗 ∈ {0 1 … , , , 𝑁 } .
- 𝑞 𝑖 : demand of vertices 𝑐 𝑖 ∈ 𝑉 .

Decision variables:

$$\text{ich} \quad \begin{array} { c c } x _ { i j } ^ { k } = \begin{cases} 1, & \text{if vehicle $k$ travels from customer $c_{i}$ to customer $c_{j}$,} \\ 0, & \text{otherwise.} \end{cases}$$

The mathematical model of the CVRP is presented below [16,52]: 𝑀𝑖𝑛𝑖𝑚𝑖𝑧𝑒 :

$$\intertext { a con-} \quad C o s t = \sum _ { k = 1 } ^ { K } \sum _ { i = 0 } ^ { N } \sum _ { j = 0 } ^ { N } d _ { i j } x _ { i j } ^ { k }$$

𝑠𝑢𝑏𝑗𝑒𝑐𝑡 𝑡𝑜 :

$$\mathbb { m } { s t h e } \mathbb { m } { a l c a l } \mathbb { m } { a l c a l } \mathbb { m } { a l c a l } \mathbb { m } { a l c a l } \mathbb { m } { a l c a l } \mathbb { m } { a l c a l } \mathbb { m } { a l c a l } \mathbb { m$$

$$\mathfrak { n } \, \text{in} \, \text{its} \quad \begin{array} { c } \kappa & N \\ 5 \end{array} \sum _ { k = 1 } ^ { N } \sum _ { j = 0 } ^ { N } x _ { i j } ^ { k } = 1, \quad i = 1, 2, \dots, N, \quad \begin{array} { c } \\ \\ \\$$

$$\overset { \dots \dots } { \text{ed over} } \sum _ { \stackrel { i = 0 } { \stackrel { N } { \stackrel { k } { i = 0 } } } } ^ { N } x _ { i h } ^ { k } - \sum _ { j = 0 } ^ { N } x _ { h j } ^ { k } = 0, \quad k = 1, 2, \dots, K ; h$$

$$\sum _ { \stackrel { j = 0 } { \stackrel { ^ { N } } { \stackrel { q } { j } } } } ^ { N } \left ( \sum _ { i = 0 } ^ { ^ { \cdot } } x _ { i j } ^ { k } \right ) \leq Q, \ k = 1, 2, \dots, \kappa,$$

$$\int _ { \dots, \, c _ { N } } \int _ { j = 1 } ^ { N } x _ { 0 j } ^ { k } \leq 1, \quad k = 1, 2, \dots, K,$$

$$\stackrel { \cdot \cdot } { \text{$\cdot \cdot \cdot } } \text{$\cdot \cdot \cdot } \text{$\cdot \cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$ \cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{ $\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{ $ \cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{\quad \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot  } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cd. } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdots } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cd< } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdot } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cd< } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdot } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cd< } \text{$\cd< } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cd< } \text{$\cdot } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdot } \text{$\cd< } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cd< } \text{$\cdots } \text{$\cdot } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cd< } \text{$\cdots } \text{$\cd< } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots } \text{$\cdots }$$

$$\overset {. } { \prime } \, Q \, \text{will} \quad x _ { i j } ^ { k } \in \{ 0, 1 \}, \ \ i \neq j ; i, j = 0, 1, \dots, N ; k = 1, 2, \dots, K.$$

The objective function (1) minimizes the total distance traveled by all vehicles. Constraints (2) and (3) guarantee each customer is visited exactly once. Constraint (4) guarantees the route continuity. Constraint (5) ensures that the total demand of any route does not exceed the vehicle maximum capacity 𝑄 . Constraints (6) and (7) guarantee that each vehicle is used no more than once. Constraint (8) ensures that the decision variable only takes the integer 0 or 1.

## 3. Differential evolution

Differential evolution (DE) was proposed by Storn and Price in 1997 for minimizing non-differentiable and nonlinear continuous functions [5]. DE is a population-based metaheuristic that optimizes a problem by iteratively improving candidate solutions in an evolutionary process. Each DE iteration creates a new generation of chromosomes called a population.

For every DE iteration, each chromosome (also called vector or individual) of the population is determined as a target, and a trial chromosome is created by mutation and crossover operators. Then, a selection process is applied to determine which chromosome will belong to the next generation. If the trial achieves a better value than the target, it replaces the target in the next generation, otherwise, the target is kept.

DE has three control parameters, which are the population size NP , the mutation factor 𝐹 (also called differential weight or scaling factor), and the crossover ratio CR (also called crossover probability). Different DE strategies and 𝐹 and CR parameters, may introduce structural biases [53]. Structural bias is a form of bias where artifacts in the algorithm lead to a preference for particular regions in the search space regardless of the objective function [54]. However, DE was proven to be robust to structural bias [55].

In the rest of this section, we detail the main DE elements. Mutation is described on Section 3.1, crossover is described on Section 3.2, selection is described on Section 3.3, and DE strategies are described on Section 3.4.

## 3.1. Mutation

For each target 𝑋𝑖,𝑔 from the population 𝑃 𝑔 , three other distinct chromosomes 𝑋𝑟 𝑖 1 ,𝑔 , 𝑋𝑟 𝑖 2 ,𝑔 , 𝑋𝑟 𝑖 3 ,𝑔 are randomly selected, with 𝑖 ≠ 𝑟 𝑖 1 ≠ 𝑟 𝑖 2 ≠ 𝑟 𝑖 3 . A mutant individual 𝑉 𝑖,𝑔 is created by the weighted difference of 𝑋𝑟 𝑖 2 ,𝑔 and 𝑋𝑟 𝑖 3 ,𝑔 , with the amplification controlled by the mutation factor 𝐹 , and added to 𝑋𝑟 𝑖 1 ,𝑔 . The mutation operation is presented by Eq. (9).

$$V _ { i, g + 1 } = X _ { r ^ { \prime } _ { 1 }, g } + F \cdot ( X _ { r ^ { \prime } _ { 2 }, g } - X _ { r ^ { \prime } _ { 3 }, g } ), \ F \in ( 0, 2 ] & & ( 9 ) \quad \text{$\in \omega$},$$

## 3.2. Crossover

The crossover operator is applied to increase the diversity of generated chromosomes [5]. A chromosome 𝑈𝑖,𝑔 +1 can be represented by a sequence of components 𝑈𝑖,𝑔 +1 = ( 𝑈 𝑖,𝑔 1 +1 , 𝑈 2 𝑖,𝑔 +1 , … , 𝑈 𝐷𝑖,𝑔 +1 ) , where 𝐷 is the number of components of the chromosome.

For each component 𝑗 of the chromosome, a random number 𝑟𝑎𝑛𝑑 𝑗 ( ) is generated and if 𝑟𝑎𝑛𝑑 𝑗 ( ) ≤ CR , then the 𝑗 -component of the mutant takes place on the trial chromosome, otherwise the 𝑗 -component of the target is used. To ensure that the trial will be different from the target chromosome, a random component of the mutant is taken without CR evaluation.

Eq. (10) presents the crossover operation.

$$U _ { j i, g + 1 } = \begin{cases} V _ { j i, g + 1 }, & \text{if $rand(j) \leq C R$, \ C R \in [0,1]$} \\ X _ { j i, g }, & \text{otherwise.} \end{cases} & \text{(10)} \quad \text{An i} \\ \text{matrix} \, 1 \end{cases}$$

## 3.3. Selection

To keep the population size constant over subsequent generations, the selection is performed to determine whether the target ( 𝑋𝑖,𝑔 ) or the trial chromosome ( 𝑈𝑖,𝑔 +1 ) survives to the next generation. Selection applies a greedy criteria over the target 𝑋𝑖,𝑔 and the trial 𝑈𝑖,𝑔 +1 . If 𝑈𝑖,𝑔 +1 achieves a lower cost compared to 𝑋𝑖,𝑔 , then 𝑈𝑖,𝑔 +1 takes its place on the generation 𝑔 + 1 . Otherwise, the target 𝑋𝑖,𝑔 is kept, remaining in the next generation as 𝑋𝑖,𝑔 +1 . Selection assures that population 𝑃 𝑔 +1 is equally or better than the previous populations.

## 3.4. DE strategies

DE literature presents several strategies (also known as techniques or variants) for the design of the mutation and crossover operators. Classical DE strategies presented by Storn and Price [5] are denoted as DE/x/y/z , where: 𝑥 specifies the mutation type rand or best , which defines if the first selected individual in the mutation ( 𝑋𝑟 𝑖 1 ,𝑔 ) will be a random or best individual of the population, respectively; 𝑦 represents the number of weighted differences used on mutation, generally 1 or 2; and 𝑧 specifies the crossover type ( bin or exp ).

The bin (binary) crossover type verifies all individual components for the operation while in the exp (exponential) crossover, the check is halted when the first randomly raffled value greater than CR is reached. A non-classical DE strategy example, DE/rand-to-best/y/z [56], is specified only for the mutation operator. It adds another weighted difference with the best individual on the random mutation type, with the purpose to combine the faster convergence of the best strategy with the robustness of the rand strategy. Sections 3.1 and 3.2 describe DE/rand/1/bin strategy.

## 4. CDELS algorithm

DE metaheuristic was proposed for continuous problems, however, it has also been used for discrete optimization. Section 1.2 presents many DE strategies for combinatorial problems. Among real-valued mapping strategies, Qian et al. [30] have proposed the Largest Order Value ( LOV ) to use DE for solving the discrete multi-objective permutation flow shop scheduling problem. LOV ranks the continuous values generated by DE in descending order and assigns integers for their respective ordered positions, where the highest real number is attributed to the first integer and the lowest real number is attributed to the highest integer. For example the sequence S = (2.31, 0.12, 1.4) would be converted to (1,3,2) by LOV . Mingyong and Erbao [45] proposed the IOR to use DE for solving the Vehicle Routing Problem with Simultaneous Pickups and Deliveries and Time Windows. IOR is similar to LOV method but assigns the integers in the opposite order of LOV , where it assigns the lowest integer to the lowest real number instead. The sequence 𝑆 would be converted to (3,1,2) with IOR .

Since generally the customers' identifiers (id) are randomly numbered on instances, customers with close ids could be placed very far from themselves on instance problems. Hence, a small arithmetical change on their ids, for example modifying the customer id 5 to 6, could result in a very far customer modification on the instance problem, which could not be wanted. For this reason, we have proposed to perturb the individual based on its customer positions instead of arithmetically changing their identifiers.

In the rest of this section, we describe our DE-based algorithm with Local Search, called CDELS ( Combinatorial Differential Evolution with Local Search ), to solve the CVRP. CDELS nomenclatures are presented in Table 2.

## 4.1. Representation of individuals

An individual is compounded by a group of routes organized in a matrix format, that represents a CVRP solution. Fig. 1 shows an example of an individual. Each row indicates a route 𝑅𝑖 , 𝑖 ∈ {1 2 … , , , 𝐾 } , to which the customers belong. A route is characterized by the set of customers it contains. Relative customer positioning 𝑝 𝑖 in its route is defined by a column, which can be traversed from the first to the last

Table 2 CDELS nomenclatures.

| Nomenclature                     | Description                                                                                             |
|----------------------------------|---------------------------------------------------------------------------------------------------------|
| 𝑔                                | Generation identifier                                                                                   |
| 𝑃 𝑔                              | Population 𝑃 of generation 𝑔                                                                            |
| NP                               | Population size                                                                                         |
| 𝐹                                | Mutation factor                                                                                         |
| CR                               | Crossover ratio                                                                                         |
| Penalty                          | Value added to objective function cost in infeasible individuals                                        |
| MaxGen                           | Maximum number of generations                                                                           |
| Stop _ criteria                  | Algorithm stop criteria                                                                                 |
| 𝑅 𝑗                              | Route 𝑗 of an individual                                                                                |
| 𝑝 𝑘                              | Relative position 𝑘 of a customer in a route                                                            |
| 𝐿                                | List of clients used in mutation                                                                        |
| rand()                           | Randomly uniformed generated number ∈ [0 . 0 , 1 . 0]                                                   |
| 𝑋 𝑖,𝑔                            | Individual 𝑖 in population 𝑔                                                                            |
| 𝑉 𝑖,𝑔 +1 , 𝑈 𝑖,𝑔 +1 , 𝑈 ′ 𝑖,𝑔 +1 | Temporary individuals generated from an individual 𝑋 𝑖,𝑔 in the evolutionary process of generation 𝑔 +1 |
| Cost( 𝑋 𝑖,𝑔 )                    | Objective cost evaluation for individual 𝑋 𝑖,𝑔                                                          |

Fig. 1. (a) A CVRP solution with 𝑁 = 9 customers distributed over 𝐾 = 3 routes, and (b) the individual matrix representation.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000003_57864ddb0e489f62b352b3d5cd38b0e2f79451f808f1550d46e9d914067f58ad.png)

customer or vice versa, represented by graph edges in Fig. 1-(a). The first and last customers of a route are connected with the depot.

An example of a CVRP solution with 𝑁 = 9 customers distributed over 𝐾 = 3 routes is illustrated in Fig. 1-(a). In this solution, customers 𝑐 1 to 𝑐 3 , 𝑐 4 to 𝑐 5 and 𝑐 6 to 𝑐 9 belong, respectively, to routes 𝑅 1 , 𝑅 2 and 𝑅 3 . The columns bring the route position to which the customer belongs. For instance, in the second column of the third row represented by Fig. 1-(b), the customer 𝑐 7 occupies position 𝑝 2 in route 𝑅 3 .

Since the algorithm proposed solves a combinatorial problem, the differential feature of Eq. (9), which answers for the perturbation strategy of the algorithm, is based on our proposal of shifting the positions of the customers in the individual. In the DE metaheuristic, the 𝐹 parameter plays the role of adjusting how perturbed the mutant can become concerning their generating individuals. It controls the difference between the individuals 𝑋𝑟 𝑖 2 ,𝑔 and 𝑋𝑟 𝑖 3 ,𝑔 , which is added to

The individual cost is computed by the objective function presented in Eq. (1).

## 4.2. Initial population

Initial population 𝑃 0 creates NP random individuals, each representing a solution for CVRP. The algorithm begins building 𝐾 feasible routes for each individual. A feasible route is composed of a group of unique customers randomly chosen from set 𝑉 - { 𝑐 0 } such as the route's vehicle capacity 𝑄 is not exceeded. If there are any remaining customers which cannot be feasibly allocated to a route, then all of them are assigned to a route that is randomly selected. In this case, the individual has one infeasible route and 𝐾 - 1 feasible routes, and its objective function is penalized by adding Penalty , which is given by a parameter.

## 4.3. Mutation

In the context of the evolutionary computing paradigm, the mutation is seen as a change or perturbation with a random element of a chromosome [43].

𝑋𝑟 𝑖 1 ,𝑔 , generating the mutant. The higher the value of 𝐹 , the farther from 𝑋𝑟 𝑖 1 ,𝑔 the mutant will be generated. To represent this feature, the mutant vector 𝑉 𝑖,𝑔 +1 is initialized as a clone of 𝑋𝑟 𝑖 1 ,𝑔 and 𝑚 = ⌈ 𝑁 2 × 𝐹 ⌉ ,

𝐹 ∈ (0 1] , , swaps are performed, where for higher 𝐹 values, more different of 𝑋𝑟 𝑖 1 ,𝑔 the individual 𝑉 𝑖,𝑔 +1 tends to be. The reason that 𝑁 is divided by two is that a single swap can change two positions of the individual and thus, 𝑚 swaps are sufficient to change the individual completely.

The trend of evolution of individuals over generations is to have the customers well positioned on the routes. Throughout the algorithm evolution, the mutation operator aims to perform a sequence of exchanges on the target individual preserving information acquired from previous generations.

In CDELS mutation, 𝑉 𝑖,𝑔 +1 is initialized as a clone of 𝑋𝑟 𝑖 1 ,𝑔 . Then, it is iteratively modified with 𝑚 = ⌈ 𝑁 2 × 𝐹 ⌉ , customer swaps, each considering a customer 𝑐 𝑗 ∈ 𝑋𝑟 𝑖 3 ,𝐺 . However, as each CVRP solution contains every customer, we can instead select 𝑐 𝑗 from a list 𝐿 of all customers 𝑉 - { 𝑐 0 } . Then, a randomly selected customer 𝑐 𝑗 in 𝐿 is removed from 𝐿 and searched in 𝑋𝑟 𝑖 2 ,𝐺 to identify its position in the individual. Since routes can have different sizes, it is possible that the

selected customer position in 𝑋𝑟 𝑖 2 ,𝐺 does not exist in 𝑉 𝑖,𝐺 +1 . In this case, the customer 𝑐 𝑗 is appended in 𝑉 𝑖,𝐺 +1 , to the last position of the corresponding route where 𝑐 𝑗 belongs in 𝑋𝑟 𝑖 2 ,𝐺 . To maintain the individual current number of vehicles, this operation is performed only if the previous 𝑐 𝑗 route has a size greater than 1.

Algorithm 1 describes the Mutation pseudocode.

## Algorithm 1: Mutation pseudocode

```
\text{Algorithm 1: Mutation pseudocode} \, \begin{array} { c } \text{Input: $X_{r_{1},g},X_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \leftarrow X_{r_{1},g}$} \, \end{array} \, \begin{array} { c } \text{Algorithm 1: Mutation pseudocode} \\ \text{Input: $X_{r_{1},g},X_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \leftarrow X_{r_{1},g}$} \, \end{array} \, \begin{array} { c } \text{Input: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \leftarrow \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 2 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i,g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Input: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{2},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g - 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output:' $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, U _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$,} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{0},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g+ 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output:\ $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.} \\ \, V _ { i, g + 1 } ^ { 1 } \end{array} \, \begin{array} { c } \text{Output: $X_{r_{1},g},V_{r_{2},g},N,F$.
```

Algorithm 2 describes the PerturbationMove pseudocode. This movement is applied in CDELS mutation and crossover operators. It swaps the customers based on their positions on individuals. In mutation, PerturbationMove is performed considering a random individual 𝑐 𝑗 selected from 𝑋𝑟 ,𝑔 3 or 𝐿 , which is located in 𝑋𝑟 ,𝑔 2 with the purpose to generate perturbations considering the population information. In crossover, PerturbationMove selects which components will take place on the trial individual 𝑈𝑖,𝑔 +1 and these components are not swapped again in 𝑈𝑖,𝑔 +1 by crossover.

## Algorithm 2: PerturbationMove pseudocode

```
Algorithm 2: PerturbationMove pseudocode
    Input: Y, Z, ( p _ { s }, R _ { \ell } )
  1 c _ { j } \leftarrow customer located at ( p _ { s }, R _ { \ell } ) of Z
  2 Y ^ { \prime } \leftarrow Y
  3 if there is a customer c _ { k } located at ( p _ { s }, R _ { \ell } ) of Y ^ { \prime } \text{ then }
  4 |   Y ^ { \prime } \leftarrow swap c _ { j } \text{ and } c _ { k } \text{ in } Y ^ { \prime }
  5 else if the route of customer c _ { j } \text{ in } Y ^ { \prime } \text{ has more than one customer }
      then
  6 |    p _ { i a s t } \leftarrow last position of route R _ { \ell } \text{ of } Y ^ { \prime }
  7 |    Y ^ { \prime } \leftarrow remove the customer c _ { j } \text{ and reinsert it at } ( p _ { i a s t } + 1, R _ { \ell } )
         of Y ^ { \prime }
  8 return Y ^ { \prime }
```

An example of a movement performed by the function PertubationMove is presented in Fig. 2. The customer 𝑐 𝑗 = 𝑐 9 is identified in position 𝑝 2 of route 𝑅 3 in the individual 𝑍 . Thus, the customer 𝑐 𝑘 = 𝑐 7 , which is in the 𝑌 corresponding position of 𝑍 , is selected and then swapped in 𝑌 ′ with 𝑐 9 , which is in position 𝑝 4 of route 𝑅 3 in 𝑌 . Another example of a movement performed by this function is an insertion of a customer in a route, as shown in Fig. 3. Since routes have different sizes, 𝑐 𝑗 corresponding location in 𝑌 cannot exist. In this case, the nearest existing position in 𝑌 is taken instead for 𝑐 𝑘 , which is the last customer of the 𝑐 𝑗 route from 𝑌 . For example, if 𝑐 𝑗 = 𝑐 7 is selected in position 𝑝 3 from route 𝑅 3 in 𝑍 , but this location does not exist in 𝑌 , then 𝑐 7 is removed from its location in 𝑌 (position 𝑝 2 from route 𝑅 2 ) and inserted in position 𝑝 3 of route 𝑅 3 , as showed in 𝑌 ′ .

## 4.4. Crossover

Algorithm 3 describes the Crossover pseudocode. It initially performs a PerturbationMove to ensure that the trial will be different from the target ( 𝑋𝑖,𝑔 ). For each customer 𝑐 𝑞 in the route, it generates a random number uniformly distributed in the interval [0 0 . , 1 0] . and if it is lower than the crossover ratio CR ∈ [0 1] , and 𝑐 𝑞 was not yet swapped, a PerturbationMove with 𝑐 𝑞 is performed. For instance, if CR = 0 7 . there is 70% of chance to perform a PerturbationMove . It repeats for all routes in the individual.

## Algorithm 3: Crossover pseudocode

```
            Algorithm 3: Crossover pseudocode
```

## 4.5. Local search

Local search procedures are applied to the trial individual created by the Crossover operator. The procedures combine the swapping algorithm, a classical heuristic described in Section 4.5.1 and an adapted version of the Drop one point algorithm proposed by Teoh et al. [48], denoted as Strong drop one point, described in Section 4.5.2. To handle infeasible individuals the Drop one point infeasible algorithm is used, which is described in Section 4.5.3.

Algorithm 4 describes the local search pseudocode.

## Algorithm 4: Local Search pseudocode Input: 𝑌 . 1 do 2 𝑌 ∗ ← 𝑌 3 𝑌 ← SwappingAlgorithm ( 𝑌 ) 4 𝑌 ← StrongDropOnePoint( 𝑌 ) 5 if 𝑌 is infeasible then 6 𝑌 ← DropOnePointInfeasible( 𝑌 ) 7 while 𝙲𝚘𝚜𝚝 ( 𝑌 ) &lt; 𝙲𝚘𝚜𝚝 ( 𝑌 ∗ ) ; 8 return 𝑌 ∗

## 4.5.1. Swapping algorithm

The swapping algorithm is a classic local search heuristic for discrete optimization. For this method, the customer 𝑐 𝑞 is swapped with another customer 𝑐 𝑝 such the cost is minimum without violating the vehicle capacity. All two vertices viable swaps are considered and the swap with the highest enhancement is chosen. An example of a Swapping algorithm movement is presented in Fig. 4.

## 4.5.2. Strong drop one point

Every customer is removed from the individual and feasibly reinserted in a different route, such as the highest enhancement is obtained, which is performed only if the cost of the individual reduces. To maintain the individual current number of vehicles, if the route has only one customer, that customer is kept. An example of a Strong drop one point movement is presented in Fig. 5. Algorithm 5 describes the Strong drop one point pseudocode.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000004_91c7d636740f308c1c8f20d06acda6b76816bbdaaf4f60525753d9ff3defb395.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000005_ec71022aba57e5e33dc0090d30901fd635a3b22e401345ffe99c219a0fc8eada.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000006_9674d4a45d14730b99650eef1704b61e8e3cf7bfb2965675f70e3a1c4c854b09.png)

Fig. 2. PerturbationMove example one: given 𝑝 𝑠 = 𝑝 2 and 𝑅 𝓁 = 𝑅 3 select 𝑐 𝑗 = 𝑐 9 from 𝑍 ; select customer 𝑐 𝑘 = 𝑐 7 located at ( 𝑝 , 𝑅 2 3 ) of 𝑌 ; swap 𝑐 𝑗 = 𝑐 9 and 𝑐 𝑘 = 𝑐 7 in 𝑌 generating 𝑌 ′ .

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000007_358ca5ffcdf162b0d9fbde4ff2d860867922110524c3e3bcee529dd1f7769d7d.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000008_ce1110ee75adc4bd58bc256ab005df0ddf0925f20e0722d12ff3387f8d7f55a6.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000009_25ae2532aeede20bb4c101910e7fea6bd06b77087007672bfe09592a11c392a8.png)

Fig. 3. PerturbationMove example two: given 𝑝 𝑠 = 𝑝 3 and 𝑅 𝓁 = 𝑅 3 select 𝑐 𝑗 = 𝑐 7 from 𝑍 ; since there is no customer located at ( 𝑝 , 𝑅 3 3 ) we remove customer 𝑐 𝑗 = 𝑐 7 from its current location in 𝑌 and insert it at ( 𝑝 , 𝑅 3 3 ) of 𝑌 generating 𝑌 ′ .

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000010_9a795ba1bfd518ec0a233e70d485341fe939cd07645b33df0f425210826e7276.png)

the algorithm cannot turn it feasible, then the algorithm returns an infeasible individual 𝑌 and its cost is penalized by adding a Penalty value. Algorithm 6 describes the Drop one point infeasible pseudocode.

## Algorithm 6: Drop one point infeasible pseudocode

## Input: 𝑌

- 1 while exists a customer in an infeasible route 𝑅𝑟 that can be feasibly reallocated to another route and 𝑌 is infeasible do
- 2 𝑐 𝑗 ← get a customer in a infeasible route 𝑅𝑟 that fits on route 𝑅𝑙

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000011_d9a0c3b966eac287e8f65fac28a96c6e46ee262630cb83d235f95a713faec88d.png)

3

- remove 𝑐 𝑗 from 𝑅𝑟 and reinsert it in 𝑅𝑙 on position which the highest enhancement is obtained on 𝑅𝑙

4

return

𝑌

## 4.6. Selection

DE selection described in Section 3.3 is applied, considering the target individual 𝑋𝑖,𝑔 +1 and trial obtained from local search 𝑈 𝑖,𝑔 ′ +1 , also considering infeasible individuals. An infeasible individual has its cost penalized by adding a positive real number Penalty , given by an input parameter.

Infeasible individuals that can be accepted by selection are kept in the subsequent generations once they may have information good enough to be leveraged by other individuals. For instance, an accepted individual can have an infeasible route, however, it can also present other better-constructed routes, that can be learned through the evolutionary process. Although, an infeasible individual should never be treated as a solution.

## 4.7. CDELS algorithm

Algorithm 7 describes the CDELS algorithm. It ends ( Stop criteria \_ ) when a maximum number of generations 𝑀𝑎𝑥𝐺𝑒𝑛 (given as input) is reached or the optimum solution cost is achieved (if this information is available) and the best solution 𝑋𝑏𝑒𝑠𝑡 for the CVRP problem is returned.

Fig. 4. Example of a Swapping algorithm movement.

## Algorithm 5: Strong drop one point pseudocode

## Input: 𝑌

1

2

3

4

5

1 to for

𝑟

←

forall

𝐾

do

𝑐

𝑞

do on route

𝑅𝑟

of

𝑌

do remove and feasibly reinsert

𝑐

𝑞

on route

𝑅𝑏

𝑅𝑏

≠

𝑅𝑟

if

,

in

𝑌

such as its cost is minimum the cost of

𝑌

reduces

;

- 6 return 𝑌

## 4.5.3. Drop one point infeasible

A random infeasible route 𝑅𝑟 is selected on the individual and if there is a customer 𝑐 𝑗 that can be reallocated, in other words, exists a route 𝑅𝑙 on the individual which 𝑅𝑙 ∪ 𝑐 𝑗 does not violate the constraints, then 𝑐 𝑗 is reinserted on the position that achieves the lowest cost on 𝑅𝑙 . It repeats while the individual remains infeasible and there are cities to be reallocated.

Drop one point infeasible seeks to turn feasible all infeasible individuals. However, if it is not possible to find any customer in an infeasible route that can be feasibly reallocated while 𝑌 is infeasible,

,

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000012_eeb7cd922ebb6b55cd7177740964d42a8ff874d8e6315bb165f28ce5690bd8f5.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000013_ca920baca40fd824c99922ec9235282abc0df054a21d5b755521c23fe017dac4.png)

Fig. 5. Example of a Strong drop one point movement.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000014_95bd8bbd9cc7aec98e3f5f89d60e12442a20a27543265b200236f8418e178602.png)

## 4.8. CDELS complexity

The complexity of CDELS major operators is described in Table 3. The basic movement is PerturbationMove and can be performed in 𝑂 (1) time complexity by using an auxiliary vector that maps the index of each customer in the individual.

CDELS complexity is connected to the population size. One iteration performs, for each in NP individuals of the population, a mutation, crossover, local search procedures, and selection. Both mutation and crossover time complexity are 𝑂 𝑁 ( ) . Mutation swaps 𝑁 components of the individual, at maximum (when 𝐹 = 1 ), and crossover runs on all 𝑁 components of the individual. Local Search includes three procedures where each is performed in 𝑂 𝑁 ( 2 ) complexity time. The selection operator is performed in 𝑂 (1) time complexity because it performs one comparison of individual costs, already evaluated in the local search procedures.

For large instances, we suggest keeping NP constant to control CDELS complexity time. In terms of asymptotic time complexity, CDELS is 𝑂 𝑁𝑃 ( ) × 𝑂 𝑁 ( 2 ) . However, CDELS is 𝑂 𝑁 ( 2 ) when population size is constant.

In terms of asymptotic time complexity, CDELS is 𝑂 𝑁𝑃 ( ) × 𝑂 𝑁 ( 2 ) . For large instances, we suggest keeping NP constant to maintain CDELS complexity time in 𝑂 𝑁 ( 2 ) .

## 5. Computational experiments and discussion

This section discusses the results obtained from the computational experiments to analyze CDELS performance. A sequential version of the

Table 3

CDELS procedures time complexity.

| Method                    | Complexity           | Reference     |
|---------------------------|----------------------|---------------|
| PerturbationMove          | 𝑂 (1)                | Algorithm 2   |
| CDELS Mutation            | 𝑂 ( 𝑁 )              | Algorithm 1   |
| CDELS Crossover           | 𝑂 ( 𝑁 )              | Algorithm 3   |
| Swapping Algorithm        | 𝑂 ( 𝑁 2 )            | Section 4.5.1 |
| Strong drop one point     | 𝑂 ( 𝑁 2 )            | Algorithm 5   |
| Drop one point infeasible | 𝑂 ( 𝑁 2 )            | Algorithm 6   |
| CDELS Selection           | 𝑂 (1)                | Section 4.6   |
| CDELS algorithm           | 𝑂 ( 𝑁𝑃 ) × 𝑂 ( 𝑁 2 ) | Algorithm 7   |

CDELS algorithm was coded in C language, compiled using 𝑔𝑐𝑐 , version 5.4.0 with -O3 optimization flag. The experiments were carried out on an Intel i5-3570 processor with 4 GB RAM, running under a Linux system Ubuntu 16.04. CDELS is open-source and the code is available at https://github.com/israelpereira55/CDELS.

CDELS was tested over 90 instances of the CVRP benchmark datasets A, B and P [57], E [58], F [59] and M [60], openly available at CVRPLIB [23]. Each instance was executed 10 times, using the first ten integers as seeds. We use double floating-point precision with values rounded to the nearest integer as specified in TSPLIB95 [61].

Four different strategies ( DE/rand/1/bin , DE/rand/1/exp , DE/best/ 1/bin , and DE/rand/1/exp , presented in Section 3.4) for mutation and crossover operations were implemented, tested and their performances were compared.

## 5.1. CDELS parameterization

In this section, the strategies adopted for tuning the algorithm parameters are detailed.

## 5.1.1. Population size NP

Das and Suganthan [62] suggest that suitable values to be set to NP are in the interval [ 3 𝑁 , 8 𝑁 ], where 𝑁 is the individual size (number of customers in CVRP). Teoh et al. [48] also have performed experimental tests for DELS using the interval [ 3 𝑁 , 8 𝑁 ] and the best results were achieved for NP = 3 𝑁 . For this reason, we have selected NP = 3 𝑁 for CDELS.

## 5.1.2. Parameters MaxGen and Penalty

A fixed number of generations MaxGen or achieving the optimum solution was adopted as the Stop criteria \_ for the experimental tests performed. This parameter was set to 10,000 in a preliminary round of the computational tests to tune other algorithm parameters. In the computational tests performed to generate the algorithm solutions, MaxGen was set to 200,000 for sets A,B,P,E,F and 500,000 for set M. The Penalty parameter used to penalize infeasible individuals was set to 100 after preliminary tests.

Fig. 6. Techniques comparison for 𝐹 × CR calibration using 27 instances for set-A (Augerat, 1995).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000015_4900828721ba8f5528de24e4399cb8d6c89b1f98def2a453b8b680ecea592f4c.png)

Table 4 Total of optimum results achieved for each strategy executed on set-A (Augerat, 1995).

Table 5 The best values for F and CR considering results obtained on set-A (Augerat, 1995).

| DE strategy   |   Score |   Score (%) |   𝐴𝑣𝑔. 𝑆𝑜𝑙. |   𝐴𝑣𝑔. 𝑇𝑖𝑚𝑒 (s) |
|---------------|---------|-------------|-------------|-----------------|
| rand/1/bin    |   14145 |       64.68 |     1047.24 |           73.26 |
| best/1/bin    |   14947 |       68.34 |     1044.19 |           29.57 |
| rand/1/exp    |   19993 |       91.42 |     1042.2  |           16.93 |
| best/1/exp    |   19320 |       88.34 |     1042.36 |           19.05 |

| DE strategy   |   F |   CR |   Score |   Score (%) |   𝐴𝑣𝑔. 𝑆𝑜𝑙. |   𝐴𝑣𝑔. 𝑇𝑖𝑚𝑒 (s) |
|---------------|-----|------|---------|-------------|-------------|-----------------|
| rand/1/bin    | 0.2 |  0.1 |     252 |       93.33 |     1042.14 |           28.05 |
| best/1/bin    | 0.5 |  0.1 |     255 |       94.44 |     1042.1  |           25.37 |
| rand/1/exp    | 0.7 |  0.7 |     257 |       95.19 |     1042.01 |           15.72 |
| best/1/exp    | 0.9 |  0.9 |     253 |       93.7  |     1042.11 |           20.12 |

## 5.1.3. Mutation and crossover parameters F and CR

The F and CR parameters were originally defined in ranges (0 , 2] and [0 , 1] , respectively by Storn and Price [5]. In CDELS, 𝐹 controls the number of PerturbationMove applications in the mutation operation. As our proposal works in a discrete finite space, 𝐹 = 1 is sufficient to modify all components of the individual. For this reason, the range of 𝐹 was reduced to (0 , 1] . The CR range is the same as the original.

To tune these parameters, exhaustive experiments with dataset A [23] were performed considering all combinations 𝐹 × CR , with 𝐹 , CR ∈ [0 1 0 2 . , . , 0 3 0 4 0 5 . , . , . , 0 6 0 7 . , . , 0 8 . , 0 9] . . Dataset A is one of the three datasets proposed by Augerat et al. [57]. The instance sizes of this dataset range from 32 to 80 customers, with locations and demands, randomly generated and representing different scenarios. This set is a well-known benchmark for CVRP performance testing, consolidated in the routing problem literature, and widely used by the community. Dataset A is also appropriate for parameter tuning since the number of customers for instances of this dataset approaches other sets (B,P,E,F), except for M. CDELS algorithm was executed 10 times for each instance of this dataset, considering 𝑃𝑒𝑛𝑎𝑙𝑡𝑦 = 100 and 𝑁𝑃 = 3 𝑁 .

The first set of experiments was performed for each strategy rand/1/ bin , best/1/bin , rand/1/exp , and best/1/exp . Fig. 6 shows the complete results considering 𝐹 × CR executions in the four strategies. Table 4 presents the strategies performance overview and Table 5 presents the best performing 𝐹 × CR combination for each strategy.

The 𝑥 -axis in Fig. 6 shows the CR values, while the 𝑦 -axis shows the F values, both in the discrete range [0 1 . , 0 2 . , … 0 8 0 9] , . , . . Each square on the chart is painted with a color assigned to integer values (right axis, plotted in the range [100 280] , , respectively, from black to white color) representing the number of executions where the optimum was achieved (denoted as Score in Tables 4 and 5). Considering that 10 executions are performed for each of 27 instances, the maximum value that can be reached by score is 270 .

Strategies rand/1/exp and best/1/exp presented more robust results, where their scores were inside the range [210 260] , (between blue and purple color). Strategies rand/1/bin and best/1/bin achieved their worst scores around value 120 (red color).

Table 4 presents in column Score the total number of executions that reached the optimum, over a total of 21 870 executions, accounted considering 81 possible variations of values for 𝐹 and CR and 270 CDELS runs. Column 𝐴𝑣𝑔 . 𝑆𝑜𝑙 . shows the average costs of the individuals (objective function value) and column 𝐴𝑣𝑔 . 𝑇𝑖𝑚𝑒 ( s ) shows the average time, in seconds, both over the total of 21870 executions. The strategy with the highest Score is in bold. CDELS with rand/1/exp strategy achieves the optimum in 91.42% of the executions with set A, followed by best/1/exp , with 88.34%, best/1/bin , with 68.34% and best1/bin , with 64.68%. The results with rand/1/exp also outperform those of the other strategies, considering the average objective function values ( Avg. Sol. ) and time ( Avg. Time (s) ).

Table 5 highlights, for each strategy, the pair of 𝐹 and CR values which the best scores were obtained over the total of 270 runs of CDELS (each square in Fig. 6). Column Avg. Sol. shows the average costs of the individuals (objective function value) and column Avg. Time (s) shows the average time, in seconds, both over the total of 270 executions. The pair 𝐹 = 0.7, CR = 0.7 on CDELS with the strategy rand/1/exp obtained the best average solutions and average time. CDELS with rand/1/exp strategy and 𝐹 = 0.7, CR = 0.7 achieved the optimum in 95.19% of the executions in set A, followed by best/1/bin ( 𝐹 = 0.5, CR = 0.1) with 94.44%, rand/1/bin ( 𝐹 = 0.9, CR = 0.9) with 93.70%, and best/1/bin ( 𝐹 = 0.2, CR = 0.1 ) with 93.33%.

Table 6 CDELS parameters values for the experimental tests.

| Parameter                     | Value      |
|-------------------------------|------------|
| NP                            | 3 𝑁        |
| F                             | 0.7        |
| CR                            | 0.7        |
| DE strategy                   | rand/1/exp |
| Penalty                       | 100        |
| MaxGen for A,B,E,F,P datasets | 200,000    |
| MaxGen for M dataset          | 500,000    |

Results presented in Fig. 6 and Tables 4 and 5 can also be analyzed by the impact of mutation ( 𝑟𝑎𝑛𝑑 or 𝑏𝑒𝑠𝑡 ) and crossover ( 𝑏𝑖𝑛 or 𝑒𝑥𝑝 ) in the strategies. When the mutation strategy is fixed and the crossover varies, the gain in the number of optima obtained is more remarkable than the opposite, presenting a higher impact of the crossover operator in the strategy. For example, the Score values of strategies rand/1/bin and rand/1/exp show a high increase of values and same occurs in best/1/bin and best/1/exp comparison (see Table 4).

We also point out the results behavior of crossover type bin . For both mutation strategies the highest Score was obtained with CR = 0.1 (see Fig. 6). Considering this CR value with full variation of F range, at least 90% of CDELS runs converged to optimum.

Considering all experiments conducted for 𝐹 × CR tuning and results presented in Fig. 6 and Tables 4 and 5, the most robust strategy is rand/1/exp . It obtained the optimum solution in 95.18% of executions with 𝐹 = 0.7 and CR = 0.7 (see Table 5). These values were adopted for CDELS algorithm computational experiments from now on.

## 5.2. Computational results and discussion

In this section, we present and discuss the results of the computational experiments performed. Table 6 summarizes the values used for CDELS parameters, according to their tuning (Section 5.1).

As discussed in Section 1.1 CDELS results were compared with three algorithms ranked considering the highest number of instances they have achieved the optimum solution: (i) a hybrid Genetic Variable Neighborhood Search Algorithm (HGA-VNS) [21]; (ii) a two-phase heuristic combining LNS with Set Partitioning (2PH) [22] and (iii) a matheuristic GRASP + VNS (MGV-CP) [18]. We also included in the analysis of the computational experiments, the results from the Differential Evolution with Local Search (DELS) algorithm [48], which is also based on the DE approach and an inspiration for our proposal, despite its mutation procedure which is a continuous operator, following the DE classic paradigm.

Tables 7, 8, 9, and 10 brings the BKS (Best Known Solution), which are optimum according to CVRPLIB [23], as well as the average (except for DELS, as it has not been reported) and the best solutions achieved by DELS, HGA-VNS, MGV-CP, 2PH, and CDELS algorithms, respectively for A, B, P, E, F and M datasets. Results not reported in the original papers are replaced by ''-''. The bottom line ̄ 𝑋 1 shows the average value considering all instances, while ̄ 𝑋 2 reports the average value only for those reported by all algorithms. For example, the computation of ̄ 𝑋 1 in Table 7 considers all instances, while ̄ 𝑋 2 discards the instance A-n45-k7, as it is not reported by 2PH algorithm. Table 11 brings for each algorithm the optima number it has achieved, by the number of instances it has presented. Column Best shows the number of instances it has found the optimum and column Avg. shows the number of instances it has found the optimum in all executions.

Table 7 shows results for dataset A. CDELS was able to find the optimal values for all 270 runs performed. When considering columns 𝐵𝑒𝑠𝑡 of all algorithms presented, DELS, HGA-VNS, MGV-CP, 2PH, and CDELS achieved the optimum for, respectively, 20, 25, 27, 26, and 27 of all instances in this set (see Table 11).

Table 8 shows results for dataset B. CDELS achieved the optimum in 229 of 230 runs. The best solution obtained by a single run of instance B-n68-k9 is far from the optimum by only 1 unit. DELS achieved the optimum for 15 instances, followed by HGA-VNS with 17, MGV-CP with 22, and both 2PH and CDELS, with 23, out of a total of 23 instances (see Table 11).

Table 9 shows results for dataset P. For this set, CDELS behavior was similar to that observed in the previous table (Table 8). Again,

Table 7 Algorithms performance comparison on dataset-A (Augerat, 1995).

| 𝐼𝑛𝑠𝑡𝑎𝑛𝑐𝑒   | BKS     | DELS    | HGA-VNS   | HGA-VNS   | MGV-CP   | MGV-CP   | 2PH     | 2PH     | CDELS   | CDELS   |
|------------|---------|---------|-----------|-----------|----------|----------|---------|---------|---------|---------|
| 𝐼𝑛𝑠𝑡𝑎𝑛𝑐𝑒   | BKS     | 𝐵𝑒𝑠𝑡    | 𝐴𝑣𝑔.      | 𝐵𝑒𝑠𝑡      | 𝐴𝑣𝑔.     | 𝐵𝑒𝑠𝑡     | 𝐴𝑣𝑔.    | 𝐵𝑒𝑠𝑡    | 𝐴𝑣𝑔.    | 𝐵𝑒𝑠𝑡    |
| A-n32-k5   | 784     | 784     | 802       | 784       | 784      | 784      | 784     | 784     | 784     | 784     |
| A-n33-k5   | 661     | 661     | 679       | 661       | 661      | 661      | 661     | 661     | 661     | 661     |
| A-n33-k6   | 742     | 742     | 760       | 742       | 742      | 742      | 742     | 742     | 742     | 742     |
| A-n34-k5   | 778     | 778     | 796       | 779       | 778      | 778      | 778     | 778     | 778     | 778     |
| A-n36-k5   | 799     | 799     | 799       | 799       | 799      | 799      | 799     | 799     | 799     | 799     |
| A-n37-k5   | 669     | 669     | 669       | 669       | 669      | 669      | 669     | 669     | 669     | 669     |
| A-n37-k6   | 949     | 949     | 949       | 949       | 949      | 949      | 949     | 949     | 949     | 949     |
| A-n38-k5   | 730     | 730     | 730       | 730       | 730      | 730      | 730     | 730     | 730     | 730     |
| A-n39-k5   | 822     | 822     | 822       | 822       | 822      | 822      | 822     | 822     | 822     | 822     |
| A-n39-k6   | 831     | 831     | 831       | 831       | 831      | 831      | 831.2   | 831     | 831     | 831     |
| A-n44-k6   | 937     | 937     | 937       | 937       | 937      | 937      | 937     | 937     | 937     | 937     |
| A-n45-k6   | 944     | 944     | 944       | 944       | 945.2    | 944      | 945.7   | 944     | 944     | 944     |
| A-n45-k7   | 1146    | 1146    | 1146      | 1146      | 1146     | 1146     | -       | -       | 1146    | 1146    |
| A-n46-k7   | 914     | 914     | 931       | 919       | 914      | 914      | 914     | 914     | 914     | 914     |
| A-n48-k7   | 1073    | 1073    | 1090      | 1073      | 1073     | 1073     | 1073    | 1073    | 1073    | 1073    |
| A-n53-k7   | 1010    | 1010    | 1010      | 1010      | 1010     | 1010     | 1010    | 1010    | 1010    | 1010    |
| A-n54-k7   | 1167    | 1167    | 1167      | 1167      | 1167     | 1167     | 1167    | 1167    | 1167    | 1167    |
| A-n55-k9   | 1073    | 1073    | 1073      | 1073      | 1073     | 1073     | 1073    | 1073    | 1073    | 1073    |
| A-n60-k9   | 1354    | 1354    | 1354      | 1354      | 1354     | 1354     | 1354    | 1354    | 1354    | 1354    |
| A-n61-k9   | 1034    | 1035    | 1034      | 1034      | 1034     | 1034     | 1034    | 1034    | 1034    | 1034    |
| A-n62-k8   | 1288    | 1288    | 1288      | 1288      | 1289.4   | 1288     | 1288.2  | 1288    | 1288    | 1288    |
| A-n63-k9   | 1616    | 1624    | 1616      | 1616      | 1616.2   | 1616     | 1616    | 1616    | 1616    | 1616    |
| A-n63-k10  | 1314    | 1316    | 1314      | 1314      | 1315.8   | 1314     | 1315.5  | 1314    | 1314    | 1314    |
| A-n64-k9   | 1401    | 1416    | 1401      | 1401      | 1402     | 1401     | 1401.5  | 1401    | 1401    | 1401    |
| A-n65-k9   | 1174    | 1181    | 1174      | 1174      | 1175.6   | 1174     | 1175.5  | 1174    | 1174    | 1174    |
| A-n69-k9   | 1159    | 1165    | 1159      | 1159      | 1159     | 1159     | 1159    | 1159    | 1159    | 1159    |
| A-n80-k10  | 1763    | 1779    | 1763      | 1763      | 1763     | 1763     | 1764.1  | 1763    | 1763    | 1763    |
| ̄ 𝑋 1      | 1041.93 | 1043.96 | 1045.85   | 1042.15   | 1042.19  | 1041.93  | -       | -       | 1041.93 | 1041.93 |
| ̄ 𝑋 2      | 1037.92 | 1040.04 | 1042      | 1038.15   | 1038.2   | 1037.92  | 1038.18 | 1037.92 | 1037.92 | 1037.92 |

I.P. Souza et al.

Table 8

Algorithms performance comparison on dataset-B (Augerat, 1995).

Table 9

| 𝐼𝑛𝑠𝑡𝑎𝑛𝑐𝑒      | BKS     | DELS    | HGA-VNS   | HGA-VNS   | MGV-CP   | MGV-CP   | 2PH     | 2PH     | CDELS   | CDELS   |
|---------------|---------|---------|-----------|-----------|----------|----------|---------|---------|---------|---------|
| 𝐼𝑛𝑠𝑡𝑎𝑛𝑐𝑒      | BKS     | 𝐵𝑒𝑠𝑡    | 𝐴𝑣𝑔.      | 𝐵𝑒𝑠𝑡      | 𝐴𝑣𝑔.     | 𝐵𝑒𝑠𝑡     | 𝐴𝑣𝑔.    | 𝐵𝑒𝑠𝑡    | 𝐴𝑣𝑔.    | 𝐵𝑒𝑠𝑡    |
| B-n31-k5      | 672     | 672     | 672       | 672       | 672      | 672      | 672     | 672     | 672     | 672     |
| B-n34-k5      | 788     | 788     | 788       | 788       | 788      | 788      | 788     | 788     | 788     | 788     |
| B-n35-k5      | 955     | 955     | 955       | 955       | 955      | 955      | 955     | 955     | 955     | 955     |
| B-n38-k6      | 805     | 805     | 822       | 807       | 805      | 805      | 805     | 805     | 805     | 805     |
| B-n39-k5      | 549     | 549     | 566       | 552       | 549      | 549      | 549     | 549     | 549     | 549     |
| B-n41-k6      | 829     | 829     | 829       | 829       | 829      | 829      | 829     | 829     | 829     | 829     |
| B-n43-k6      | 742     | 742     | 742       | 742       | 742      | 742      | 742     | 742     | 742     | 742     |
| B-n44-k7      | 909     | 909     | 921       | 909       | 909      | 909      | 909     | 909     | 909     | 909     |
| B-n45-k5      | 751     | 751     | 763       | 751       | 751      | 751      | 751     | 751     | 751     | 751     |
| B-n45-k6      | 678     | 678     | 690       | 678       | 678      | 678      | 678     | 678     | 678     | 678     |
| B-n50-k7      | 741     | 741     | 753       | 741       | 741      | 741      | 741     | 741     | 741     | 741     |
| B-n50-k8      | 1312    | 1313    | 1324      | 1315      | 1313     | 1313     | 1312.5  | 1312    | 1312    | 1312    |
| B-n51-k7      | 1032    | 1033    | 1053      | 1037      | 1032     | 1032     | 1032    | 1032    | 1032    | 1032    |
| B-n52-k7      | 747     | 747     | 768       | 751       | 747      | 747      | 747     | 747     | 747     | 747     |
| B-n56-k7      | 707     | 707     | 728       | 710       | 707      | 707      | 708.5   | 707     | 707     | 707     |
| B-n57-k7      | 1153    | 1166    | 1153      | 1153      | 1155     | 1153     | 1156.5  | 1153    | 1153    | 1153    |
| B-n57-k9      | 1598    | 1599    | 1598      | 1598      | 1598     | 1598     | 1598    | 1598    | 1598    | 1598    |
| B-n63-k10     | 1496    | 1504    | 1496      | 1496      | 1496.6   | 1496     | 1496    | 1496    | 1496    | 1496    |
| B-n64-k9      | 861     | 861     | 861       | 861       | 861.4    | 861      | 861     | 861     | 861     | 861     |
| B-n66-k9      | 1316    | 1322    | 1316      | 1316      | 1316.1   | 1316     | 1316.8  | 1316    | 1316    | 1316    |
| B-n67-k10     | 1032    | 1032    | 1032      | 1032      | 1033.1   | 1032     | 1032    | 1032    | 1032    | 1032    |
| B-n68-k9      | 1272    | 1281    | 1272      | 1272      | 1272     | 1272     | 1273.2  | 1272    | 1272.1  | 1272    |
| B-n78-k10     | 1221    | 1230    | 1221      | 1221      | 1221     | 1221     | 1226.6  | 1221    | 1221    | 1221    |
| ̄ 𝑋 1 = ̄ 𝑋 2 | 963.739 | 965.826 | 970.565   | 964.609   | 963.965  | 963.783  | 964.309 | 963.739 | 963.743 | 963.739 |

Algorithms performance comparison on dataset-P (Augerat, 1995).

| 𝐼𝑛𝑠𝑡𝑎𝑛𝑐𝑒      | BKS     | DELS 𝐵𝑒𝑠𝑡   | HGA-VNS   | HGA-VNS   | MGV-CP   | MGV-CP   | 2PH     | 2PH     | CDELS   | CDELS   |
|---------------|---------|-------------|-----------|-----------|----------|----------|---------|---------|---------|---------|
| 𝐼𝑛𝑠𝑡𝑎𝑛𝑐𝑒      | BKS     | DELS 𝐵𝑒𝑠𝑡   | 𝐴𝑣𝑔.      | 𝐵𝑒𝑠𝑡      | 𝐴𝑣𝑔.     | 𝐵𝑒𝑠𝑡     | 𝐴𝑣𝑔.    | 𝐵𝑒𝑠𝑡    | 𝐴𝑣𝑔.    | 𝐵𝑒𝑠𝑡    |
| P-n16-k8      | 450     | 450         | 450       | 450       | 450      | 450      | 450     | 450     | 450     | 450     |
| P-n19-k2      | 212     | 212         | 212       | 212       | 212      | 212      | 212     | 212     | 212     | 212     |
| P-n20-k2      | 216     | 216         | 216       | 216       | 216      | 216      | 216     | 216     | 216     | 216     |
| P-n21-k2      | 211     | 211         | 211       | 211       | 211      | 211      | 211     | 211     | 211     | 211     |
| P-n22-k2      | 216     | 216         | 216       | 216       | 216      | 216      | 216     | 216     | 216     | 216     |
| P-n22-k8      | 603     | 603         | 603       | 603       | 603      | 603      | 603     | 603     | 603     | 603     |
| P-n23-k8      | 529     | 533         | 529       | 529       | 529      | 529      | 529     | 529     | 529     | 529     |
| P-n40-k5      | 458     | 458         | 458       | 458       | 458      | 458      | 458     | 458     | 458     | 458     |
| P-n45-k5      | 510     | 510         | 510       | 510       | 510      | 510      | 510     | 510     | 510     | 510     |
| P-n50-k7      | 554     | 554         | 557       | 555       | 554      | 554      | 554     | 554     | 554     | 554     |
| P-n50-k8      | 631     | 641         | 631       | 631       | 631.3    | 631      | 633.4   | 631     | 631     | 631     |
| P-n50-k10     | 696     | 696         | 706       | 696       | 696      | 696      | 696     | 696     | 696     | 696     |
| P-n51-k10     | 741     | 742         | 759       | 745       | 741      | 741      | 741     | 741     | 741     | 741     |
| P-n55-k7      | 568     | 568         | 571       | 568       | 568      | 568      | 568     | 568     | 568     | 568     |
| P-n55-k10     | 694     | 694         | 694       | 694       | 694      | 694      | 694     | 694     | 694.1   | 694     |
| P-n60-k10     | 744     | 744         | 744       | 744       | 744      | 744      | 744     | 744     | 744     | 744     |
| P-n60-k15     | 968     | 968         | 979       | 970       | 968      | 968      | 968     | 968     | 968     | 968     |
| P-n65-k10     | 792     | 792         | 803       | 795       | 792      | 792      | 792     | 792     | 792     | 792     |
| P-n70-k10     | 827     | 827         | 827       | 827       | 827      | 827      | 827     | 827     | 827     | 827     |
| P-n76-k4      | 593     | 593         | 596       | 596       | 594.3    | 593      | 593.7   | 593     | 593     | 593     |
| P-n76-k5      | 627     | 629         | 627       | 627       | 627.5    | 627      | 627.3   | 627     | 627     | 627     |
| P-n101-k4     | 681     | 685         | 681       | 681       | 681.1    | 681      | 681     | 681     | 681     | 681     |
| ̄ 𝑋 1 = ̄ 𝑋 2 | 569.136 | 570.091     | 571.818   | 569.727   | 569.236  | 569.136  | 569.291 | 569.136 | 569.141 | 569.136 |

for one instance (P-n55-k10) the best solution was 1 unit far from the optimum in one run. Both DELS and HGA-VNS achieved the optimum in 17 instances, followed by 2PH, MGV-CP, and CDELS which achieved the optimum for all 22 instances of this set.

Table 10 shows the results for datasets E, F, and M. CDELS achieved the optimum in all executions for datasets E and F, except by one run for instance F-n135-k7, the largest of the F dataset. Besides, considering the M dataset, two executions attained the optimum for instance M-n151-k12, and none for the two largest instances of this dataset (M-n200-k16 and M-n200-k17). DELS and HGA-VNS reported results only for set E. The former achieved 5 and the latter, 8 optima, out of the total of 10 instances. For this dataset, the algorithms 2PH, MGVCP, and CDELS reached the optimum for all instances. Only 2PH and CDELS reported results for set M and both attained the optimum for all instances of this dataset. For dataset M, algorithms 2PH and MGV-CP

reported results, respectively, for 3 and 5, out of a total of 5 instances. CDELS, 2PH, and MGV-CP achieved the optimum for the first three instances, with particular regard to CDELS, which obtained the best average values. However, for the last two instances of this set, MGV-CP outperformed CDELS results.

The ̄ 𝑋 1 and ̄ 𝑋 2 averages bring information for the analysis when the goal is to compare the results of all runs performed. CDELS obtained the lowest ̄ 𝑋 1 and ̄ 𝑋 2 values among all the algorithms compared.

Table 11 summarizes, for each algorithm and dataset, the number of optima achieved over the total of instances reported. Except for DELS, with only the column 𝐵𝑒𝑠𝑡 , columns 𝐴𝑣𝑔 . and 𝐵𝑒𝑠𝑡 are provided for all the other algorithms. In the 𝐴𝑣𝑔 . columns, an rank is counted only if all executions for an algorithm reached the optimum in an instance. The last row of this table gives the total number of optima attained by each algorithm over the total number of instances reported.

I.P. Souza et al.

Table 10

Algorithms performance comparison on datasets E (Christofides and Eilon, 1969), F (Fisher, 1994), and M (Christofides, Mingozzi, and Toth, 1979).

| 𝐼𝑛𝑠𝑡𝑎𝑛𝑐𝑒   | BKS    | DELS   | HGA-VNS   | HGA-VNS   | MGV-CP   | MGV-CP   | 2PH    | 2PH   | CDELS   | CDELS   |
|------------|--------|--------|-----------|-----------|----------|----------|--------|-------|---------|---------|
| 𝐼𝑛𝑠𝑡𝑎𝑛𝑐𝑒   | BKS    | 𝐵𝑒𝑠𝑡   | 𝐴𝑣𝑔.      | 𝐵𝑒𝑠𝑡      | 𝐴𝑣𝑔.     | 𝐵𝑒𝑠𝑡     | 𝐴𝑣𝑔.   | 𝐵𝑒𝑠𝑡  | 𝐴𝑣𝑔.    | 𝐵𝑒𝑠𝑡    |
| E-n22-k4   | 375    | 375    | 375       | 375       | 375      | 375      | 375    | 375   | 375     | 375     |
| E-n23-k3   | 569    | 569    | 569       | 569       | 569      | 569      | 569    | 569   | 569     | 569     |
| E-n30-k3   | 534    | 534    | 534       | 534       | 534      | 534      | 534    | 534   | 534     | 534     |
| E-n33-k4   | 835    | 835    | 835       | 835       | 835      | 835      | 835    | 835   | 835     | 835     |
| E-n51-k5   | 521    | 521    | 521       | 521       | 521      | 521      | 521    | 521   | 521     | 521     |
| E-n76-k7   | 682    | 689    | 682       | 682       | 682      | 682      | 682    | 682   | 682     | 682     |
| E-n76-k8   | 735    | 738    | 735       | 735       | 735      | 735      | 735.5  | 735   | 735     | 735     |
| E-n76-k10  | 830    | 843    | 830       | 830       | 830      | 830      | 830.2  | 830   | 830     | 830     |
| E-n76-k14  | 1021   | 1042   | 1028      | 1022      | 1021     | 1021     | 1021   | 1021  | 1021    | 1021    |
| E-n101-k14 | 1067   | 1086   | 1075      | 1069      | 1068.3   | 1067     | 1068.7 | 1067  | 1067    | 1067    |
| F-n45-k4   | 724    | -      | -         | -         | -        | -        | 724.4  | 724   | 724     | 724     |
| F-n72-k4   | 237    | -      | -         | -         | -        | -        | 237    | 237   | 237     | 237     |
| F-n135-k7  | 1162   | -      | -         | -         | -        | -        | 1162   | 1162  | 1162.1  | 1162    |
| M-n101-k10 | 820    | -      | -         | -         | 820      | 820      | 820    | 820   | 820     | 820     |
| M-n121-k7  | 1034   | -      | -         | -         | 1034     | 1034     | 1043.9 | 1034  | 1034    | 1034    |
| M-n151-k12 | 1015   | -      | -         | -         | 1018     | 1015     | 1024   | 1015  | 1016.7  | 1015    |
| M-n200-k16 | 1274   | -      | -         | -         | 1303.3   | 1279     | -      | -     | 1329.3  | 1315    |
| M-n200-k17 | 1275   | -      | -         | -         | 1283.8   | 1277     | -      | -     | 1288.6  | 1284    |
| ̄ 𝑋 1      | 817.22 | -      | -         | -         | -        | -        | -      | -     | 821.15  | 820     |
| ̄ 𝑋 2      | 716.9  | 723.2  | 718.4     | 717.2     | 717.03   | 716.9    | 717.14 | 716.9 | 716.9   | 716.9   |

Table 11

Number of instances that achieved optimal solutions reported for each benchmark dataset.

|       | DELS   | HGA-VNS   | HGA-VNS   | MGV-CP   | MGV-CP   | 2PH   | 2PH   | CDELS   | CDELS   |
|-------|--------|-----------|-----------|----------|----------|-------|-------|---------|---------|
|       | 𝐵𝑒𝑠𝑡   | 𝐴𝑣𝑔.      | 𝐵𝑒𝑠𝑡      | 𝐴𝑣𝑔.     | 𝐵𝑒𝑠𝑡     | 𝐴𝑣𝑔.  | 𝐵𝑒𝑠𝑡  | 𝐴𝑣𝑔.    | 𝐵𝑒𝑠𝑡    |
| Set-A | 20/27  | 21/27     | 25/27     | 21/27    | 27/27    | 19/26 | 26/26 | 27/27   | 27/27   |
| Set-B | 15/23  | 13/23     | 17/23     | 17/23    | 22/23    | 17/23 | 23/23 | 22/23   | 23/23   |
| Set-P | 17/22  | 15/22     | 17/22     | 18/22    | 22/22    | 19/22 | 22/22 | 21/22   | 22/22   |
| Set-E | 5/10   | 8/10      | 8/10      | 9/10     | 10/10    | 7/10  | 10/10 | 10/10   | 10/10   |
| Set-F | 0/0    | 0/0       | 0/0       | 0/0      | 0/0      | 1/3   | 3/3   | 2/3     | 3/3     |
| Set-M | 0/0    | 0/0       | 0/0       | 2/5      | 3/5      | 1/3   | 3/3   | 2/5     | 3/5     |
| Total | 57/82  | 57/82     | 67/82     | 67/87    | 84/87    | 64/87 | 87/87 | 84/90   | 88/90   |

Regarding the 𝐵𝑒𝑠𝑡 columns of Table 11, we observe that, in the total of 90 instances, CDELS achieved the optimum for 88, followed by 2PH, with 87 of 87 reported instances, MGV-CP with 84 of 87 reported instances, HGA-VNS with 67 of 82 reported instances and DELS with 57 of 82 reported instances. These results show that CDELS was the most effective algorithm in finding optimal solutions for these datasets.

the instances from different packages have different characteristics, allowing the algorithms' performance to be tested in different scenarios. Datasets A, P, and E and randomly generated. Dataset B is generated based on clusters. Dataset F represents real-life problems and dataset M groups the customers into clusters in an attempt to represent a real-life problem [23,63].

Furthermore, we highlight the optimum was achieved in all runs for all instances of sets A and E, and for sets B, P, and F, only one execution did not find the optimum for the CDELS algorithm. CDELS presents the greater number of optimums when comparing all 𝐴𝑣𝑔 . columns of this table. We emphasize the algorithm robustness, as it shows a slight variation of results over the runs performed for the same instance.

Table 12 depicts the average CDELS CPU time, in seconds for each instance of the datasets tested.

For dataset A, CDELS found the optimum with less than 1 s for 15 instances over 27, and the longest times occurred for two instances, which were close to 200 s. For dataset B, CDELS found the optimum for 12 instances in less than one second. The longest average time was 845.60 s for instance B-n68-k9. For dataset E, CDELS found the optimum values for 4 instances in less than one second. The longest average time was 1840.07 for instance E-n101-k14. For dataset F, CDELS found the optimum for 2 instances with less than 1 s. However, the largest instance required 7009.65 s. For dataset P, CDELS ran in less than one second for 11 instances the longest average time was 266.15 s for the instance Pn55-k10. The M dataset was the hardest for CDELS. Also, it is the set with the largest instances. The worst average CDELS runtime in this set was close to 106,000 s.

We conclude that CDELS was fast in most instances. It runs with less than a second for 47.8% and less than a minute for 75.6% of the instances.

For illustration purposes, Fig. 7 shows CDELS solutions obtained for an instance of each dataset tested (A-n33-k6, B-n38-k6, P-n76k5, E-n101-k14, F-n72-k4 and M-n101-k10). It is interesting to note

## 5.3. Statistical analysis

In this section, we discuss a statistical analysis of the results shown in Section 5.2. The statistical methods chosen to carry out this analysis are explained in detail by Derrac et al. [64] and were performed using the IBM SPSS Statistics Version 20 tool. The experiments were performed with A, B, P, and E datasets, as they are used by all stateof-the-art algorithms reported in Section 5.2. The algorithms results were submitted to statistical tests and normalized in the range [0 , 1] , computed as 𝑆𝑜𝑙𝑢𝑡𝑖𝑜𝑛 𝑋 ( )-𝐿𝑜𝑤𝑒𝑠𝑡 𝑋 ( ) 𝐻𝑖𝑔ℎ𝑒𝑠𝑡 𝑋 ( )-𝐿𝑜𝑤𝑒𝑠𝑡 𝑋 ( ) , considering their highest ( Highest(X) )

and lowest ( Lowest(X) ) values achieved by all the algorithms for each test problem 𝑋 , in order to not prioritize any test problem [65].

Two distinct statistical tests were performed: (i) the Wilcoxon signed-rank test to perform a pairwise comparison of best solutions achieved by DELS and CDELS ( 𝐵𝑒𝑠𝑡 columns of Tables 7, 8, 9 and 10), in order to detect if CDELS is statistically better than DELS. The reason for using best results (instead of mean) of both algorithms is that DELS does not report average results; and (ii) the Friedman test is conducted over average solutions ( 𝐴𝑣𝑔 . columns of Tables 7, 8, 9 and 10) of the state-of-the-art algorithms HGA-VNS, 2PH and MGV-CP, to verify there is a significant difference between them. The datasets A,B,P,E are considered for the experiments, however as the algorithm 2PH did not report results for Instance A-n45-k7 (see Table 7), it was discarded from the test experiments. If Friedman rejects the null hypothesis, the

I.P. Souza et al.

Table 12 CDELS average time in seconds in experimental tests.

| Instance   |   Time (s) | Instance   |   Time (s) | Instance   |   Time (s) | Instance   |   Time (s) | Instance   |   Time (s) |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| A-n32-k5   |       0.05 | A-n60-k9   |       5.09 | B-n45-k6   |       2.25 | E-n51-k5   |       2.26 | P-n50-k7   |       0.78 |
| A-n33-k5   |       0.02 | A-n61-k9   |      12.2  | B-n50-k7   |       0.07 | E-n76-k7   |      60.04 | P-n50-k8   |      14.94 |
| A-n33-k6   |       0.02 | A-n62-k8   |      56.49 | B-n50-k8   |      33.77 | E-n76-k8   |     136.44 | P-n50-k10  |     115.39 |
| A-n34-k5   |       0.04 | A-n63-k9   |      24.89 | B-n51-k7   |       1.34 | E-n76-k10  |     318.13 | P-n51-k10  |       0.99 |
| A-n36-k5   |       0.13 | A-n63-k10  |     203.67 | B-n52-k7   |       0.9  | E-n76-k14  |     360.67 | P-n55-k7   |       6.09 |
| A-n37-k5   |       0.04 | A-n64-k9   |     234.09 | B-n56-k7   |       0.63 | E-n101-k14 |    1840.07 | P-n55-k10  |     266.15 |
| A-n37-k6   |       0.11 | A-n65-k9   |      29.96 | B-n57-k7   |      15.53 | F-n45-k4   |       0.21 | P-n60-k10  |       1.98 |
| A-n38-k5   |       0.25 | A-n69-k9   |      12.06 | B-n57-k9   |       8.72 | F-n72-k4   |       9.01 | P-n60-k15  |       2.36 |
| A-n39-k5   |       0.23 | A-n80-k10  |     135.01 | B-n63-k10  |     151.06 | F-n135-k7  |    7009.65 | P-n65-k10  |       3.23 |
| A-n39-k6   |       0.43 | B-n31-k5   |       0.03 | B-n64-k9   |       6.75 | P-n16-k8   |       0.01 | P-n70-k10  |      51.44 |
| A-n44-k6   |       0.43 | B-n34-k5   |       0.03 | B-n66-k9   |      28.14 | P-n19-k2   |       0.01 | P-n76-k4   |     140.94 |
| A-n45-k6   |       2.65 | B-n35-k5   |       0.04 | B-n67-k10  |     178.52 | P-n20-k2   |       0.01 | P-n76-k5   |     245.66 |
| A-n45-k7   |       0.48 | B-n38-k6   |       0.2  | B-n68-k9   |     845.6  | P-n21-k2   |       0.01 | P-n101-k4  |     249.55 |
| A-n46-k7   |       0.88 | B-n39-k5   |       0.31 | B-n78-k10  |      73.57 | P-n22-k2   |       0.01 | M-n101-k10 |      19.47 |
| A-n48-k7   |       0.62 | B-n41-k6   |       0.36 | E-n22-k4   |       0.01 | P-n22-k8   |       0.01 | M-n121-k7  |    6277.5  |
| A-n53-k7   |       4.27 | B-n43-k6   |       0.19 | E-n23-k3   |       0.01 | P-n23-k8   |       0.01 | M-n151-k12 |   33995.2  |
| A-n54-k7   |       3.1  | B-n44-k7   |       0.2  | E-n30-k3   |       0.05 | P-n40-k5   |       0.04 | M-n200-k16 |   60910.1  |
| A-n55-k9   |       0.91 | B-n45-k5   |       0.66 | E-n33-k4   |       0.05 | P-n45-k5   |       0.15 | M-n200-k17 |  106481    |

Table 13

Wilcoxon signed-rank test results for CDELS and DELS comparison.

| Comparison        |   R + |   R - |     𝑝 |   𝑛 |
|-------------------|-------|-------|-------|-----|
| CDELS versus DELS |    53 |    28 | 4e-06 |  82 |

Table 14 shows the ranks (in parentheses) of the Friedman test computed for all the algorithms considered. It also shows Friedman statistics: sample size ( 𝑛 ), 𝜒 2 values for standard deviation, degrees of freedom ( 𝑑𝑓 ) and 𝑝 -value ( 𝑝 ). We can conclude with a level of significance 𝛼 = 0 01 . the existence of highly significant differences among the algorithms considered ( 𝑝 &lt; 𝛼 ).

Wilcoxon signed-rank test is conducted and the 𝛼 is adjusted using the Holm-Bonferroni post-hoc method, to verify if CDELS is statistically better than state-of-the-art algorithms.

## 5.3.1. First statistical analysis

The first statistical test has been conducted using the Wilcoxon signed-rank test to verify if CDELS is statistically better than DELS considering the level of significance 𝛼 = 0 01 . .

The Wilcoxon signed-rank test is a nonparametric pairwise comparison that aims to answer the following question: do two samples represent two different populations? [64] In other words, the default assumption for the test, the null hypothesis 𝐻 0 , states that the two methods are not statistically different.

Let 𝑅 + be the sum of ranks for the problems in which the CDELS algorithm outperformed the DELS, and 𝑅 -the sum of ranks for the opposite. The number of ties is split evenly between the algorithms (if ties are an odd number, one tie is not considered). Table 13 shows the 𝑅 + , 𝑅 -, 𝑝 -value ( 𝑝 ) computed and sample size ( 𝑛 ) for the pairwise CDELS-DELS comparison using the Wilcoxon signed-rank test. We can conclude with a level of significance 𝛼 = 0 01 . that CDELS is a highly significant improvement over DELS ( 𝑝 &lt; 𝛼 ).

## 5.3.2. Second statistical analysis

The Friedman test was applied to the algorithms average solutions to verify significant differences. Friedman test is a nonparametric multiple comparison procedure. It can be used for answering the following question [64]: in a set of 𝜂 samples (where 𝜂 ≥ 2 ), do at least two of the samples represent populations with different median values? Friedman test ranks the algorithms for each data set separately, the best performing algorithm getting the rank of 1, the second best rank 2, and so on, as shown in parentheses on Table 14.

The Friedman test is a multiple comparisons test that was applied to detect significant differences between the behavior of the considered algorithms. Friedman test starts with:

- · Null hypothesis 𝐻 0 : There is no difference in the average solution among the four algorithms compared.
- · Alternative Hypothesis 𝐻 1 : There are differences in the average solution among the four algorithms compared.

Since the null hypothesis of the Friedman test was rejected, we can proceed with post-hoc analysis. The Wilcoxon signed-rank test is conducted and the 𝛼 is adjusted with Holm-Bonferroni post-hoc test.

The Holm-Bonferroni post-hoc test adjusts the value of 𝛼 in a stepdown procedure. The obtained 𝑝 , 𝑝 1 2 , … , 𝑝 𝜅 -1 are ordered (smallest to largest), so that 𝑝 1 ≤ 𝑝 2 ≤ ⋯ ≤ 𝑝 𝜅 -1 . And let 𝐻 ,𝐻 , 1 2 … , 𝐻 𝜅 -1 be the corresponding null hypotheses. Holm-Bonferroni procedure starts with the most significant (lowest) 𝑝 -value and rejects 𝐻 1 to 𝐻𝑖 -1 , if 𝑖 is the smallest integer such that 𝑝 𝑖 &gt; 𝛼 ∕( 𝜅 - ) 𝑖 . However, if there is no 𝑖 ∈ [1 , 𝜅 - 1] , such as 𝑝 𝑖 &gt; 𝛼 ∕( 𝜅 - ) 𝑖 , all the null hypotheses considered are rejected [64].

Table 15 shows the 𝑅 + , 𝑅 -, 𝑝 -value ( 𝑝 ) computed and sample size ( 𝑛 ) and corrected 𝛼 with Holm-Bonferroni procedure (Evaluation column) for each pairwise comparison. With a level of significance 𝛼 = 0 01 . we can conclude that CDELS is highly significantly better than HGA-VNS, 2PH, and MGV-CP, however, there is not a highly significant difference among the other methods.

## 6. Conclusion

This work proposes an algorithm based on the Differential Evolution metaheuristic with local search, called CDELS, to solve the CVRP, which incorporates the Swapping method, Strong drop one point, Drop one point, and Drop one point infeasible local search procedures. An exhaustive calibration assessment was tackled for 𝐹 and CR parameters, considering each of the four classical DE strategies.

The experiments results showed that CDELS is a robust and competitive algorithm, achieving 847 optimum solutions of 850 (99,65%) executions, for datasets A,B,P,E,F (see Tables 7, 8, 9, and 10). Dataset M was the hardest submitted to CDELS, also containing the largest instances. The algorithm obtained the optimum solution for 3 of 5 instances (see Table 10). It ran fast for most instances, with less than a second for 47.8% and less than a minute for 75.6% of them (see Table 12).

CDELS was also compared with three state-of-the-art algorithms: HGA-VNS, MGV-CP, and 2PH. Statistical analysis was conducted over the experiments results and the conclusion, with a level of significance 𝛼 = 0 01 . , was that CDELS is highly significantly better than the compared state-of-the-art algorithms.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000016_d156dca98904ffe9a1fb7d0b334bbb095d0d52ee8398e353a4e2a0c827bb76d1.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000017_721fba143d87a97b99a9bf2364afca2c953f9543ee14fb82e93fba3dd1eb2926.png)

Fig. 7. CDELS solutions for instances (a) A-n33-k6, (b) B-n38-k6, (c) P-n76-k5, (d) E-n101-k14, (e) F-n72-k4, (f) M-n101-k10.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000018_9993c4499cc29aa236e22431605402ed0fc5f4bbdd88e516e2412f6bd3b12861.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000019_479332e2c29068e7d327520017174babcfd072a5739b0c8724642731251604d4.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000020_10bcafe85e6a4e2e6e0d851f82b9e1774e2d86fce3dd4ebf126450a2cdc8e80a.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[souza2023] A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem_artifacts/image_000021_5b59322824bfb35bd1f367140625833f569fcb4136a10e0b33b408213cf72be1.png)

Table 14 Friedman test with HGA-VNS, MGV-CP, 2PH, and CDELS.

|          | Mean ranks   |          |          |          | Test statistics   |       |    |          |
|----------|--------------|----------|----------|----------|-------------------|-------|----|----------|
|          | HGA-VNS      | MGV-CP   | 2PH      | CDELS    | 𝑛                 | 𝑋 2   | 𝑑𝑓 | 𝑝        |
| Set-ABPE | 2.77 (4)     | 2.52 (2) | 2.55 (3) | 2.15 (1) | 81                | 24.07 | 3  | 0.000024 |

We attribute the good convergence and performance of CDELS, shown by the computational results, to the combination of an accurate design of the local search, a sophisticated implementation of the whole algorithm, and an exhaustive parameter setting with an expressive computational time decrease (see Tables 4 and 5). The statistical analysis results reinforce the CDELS better performance.

In future work, CDELS applications can be investigated to other VRP variations, for instance, those with time window constraints. Moreover, we plan to enhance the current CDELS proposal, for example, adopting constructive methods to build the initial population with lower-cost solutions. CDELS can be investigated further using more datasets, such as the X dataset [66], with instances ranging from 100 to 1000 customers.

## CRediT authorship contribution statement

Israel Pereira Souza: Conceptualization, Methodology/Study design, Software, Validation, Formal analysis, Investigation, Resources, Data curation, Writing - original draft, Writing - review &amp; editing,

I.P. Souza et al.

Table 15

Wilcoxon signed-rank test results with Holm-Bonferroni adjustment ( 𝛼 = 0.01).

| Comparison            |   R + |   R - |        𝑝 | Evaluation   |   n |
|-----------------------|-------|-------|----------|--------------|-----|
| CDELS versus HGA-VNS  |    52 |    29 | 3e-06    | 𝑝 < 𝛼 ∕6     |  81 |
| CDELS versus 2PH      |    49 |    31 | 0.000698 | 𝑝 < 𝛼 ∕5     |  81 |
| CDELS versus MGV-CP   |    48 |    33 | 0.001062 | 𝑝 < 𝛼 ∕4     |  81 |
| MGV-CP versus HGA-VNS |    46 |    35 | 0.032116 | 𝑝 > 𝛼 ∕3     |  81 |
| 2PH versus HGA-VNS    |    45 |    35 | 0.045831 | 𝑝 > 𝛼 ∕2     |  81 |
| 2PH versus MGV-CP     |    40 |    40 | 0.750568 | 𝑝 > 𝛼        |  81 |

Visualization, Funding acquisition. Maria Claudia Silva Boeres: Conceptualization, Methodology/Study design, Validation, Formal analysis, Investigation, Resources, Writing -original draft, Writing -review &amp; editing, Visualization, Supervision, Project administration. Renato Elias Nunes Moraes: Conceptualization, Validation, Formal analysis, Writing -original draft, Writing -review &amp; editing, Visualization, Supervision.

## Declaration of competing interest

The authors declare that they have no known competing financial interests or personal relationships that could have appeared to influence the work reported in this paper.

## Data availability

Link to implementation is available in the paper.

## Acknowledgment

This work was supported by the Federal University of Espírito Santo, Brazil.

## References

- [1] G.B. Dantzig, J.H. Ramser, The truck dispatching problem, Manage. Sci. 6 (1) (1959) 80-91.
- [2] G. Clarke, J.W. Wright, Scheduling of vehicles from a central depot to a number of delivery points, Oper. Res. 12 (4) (1964) 568-581.
- [3] J.K. Lenstra, A.R. Kan, Complexity of vehicle routing and scheduling problems, Networks 11 (2) (1981) 221-227.
- [4] K. Braekers, K. Ramaekers, I. Van Nieuwenhuyse, The vehicle routing problem: State of the art classification and review, Comput. Ind. Eng. 99 (2016) 300-313.
- [5] R. Storn, K. Price, Differential evolution-a simple and efficient heuristic for global optimization over continuous spaces, J. Global Optim. 11 (4) (1997) 341-359.
- [6] K. Price, R.M. Storn, J.A. Lampinen, Differential Evolution: A Practical Approach to Global Optimization, Springer Science &amp; Business Media, 2006.
- [7] J.-Y. Potvin, M. Gendreau, Handbook of Metaheuristics, Springer, 2018.
- [8] D. Whitley, Next generation genetic algorithms: A user's guide and tutorial, in: Handbook of Metaheuristics, Springer International Publishing, Cham, 2019, pp. 245-274, http://dx.doi.org/10.1007/978-3-319-91086-4\_8.
- [9] J. Kytojoki, T. Nuortio, O. Braysy, M. Gendreau, An efficient variable neighborhood search heuristic for very large scale vehicle routing problems, Comput. Oper. Res. 34 (9) (2007) 2743-2757, http://dx.doi.org/10.1016/j.cor.2005.10. 010.
- [10] A. Van Breedam, Improvement heuristics for the vehicle routing problem based on simulated annealing, European J. Oper. Res. 86 (3) (1995) 480-490.
- [11] W.-C. Chiang, R.A. Russell, Simulated annealing metaheuristics for the vehicle routing problem with time windows, Ann. Oper. Res. 63 (1) (1996) 3-27.
- [12] C. Prins, C. Prodhon, R.W. Calvo, Solving the capacitated location-routing problem by a GRASP complemented by a learning process and a path relinking, 4OR 4 (3) (2006) 221-238.
- [13] M. Gendreau, A. Hertz, G. Laporte, A tabu search heuristic for the vehicle routing problem, Manage. Sci. 40 (10) (1994) 1276-1290.
- [14] P. Toth, D. Vigo, The granular tabu search and its application to the vehicle-routing problem, Informs J. Comput. 15 (4) (2003) 333-346.
- [15] E. Queiroga, R. Sadykov, E. Uchoa, A POPMUSIC matheuristic for the capacitated vehicle routing problem, Comput. Oper. Res. 136 (2021) 105475.
- [16] İ. İlhan, An improved simulated annealing algorithm with crossover operator for capacitated vehicle routing problem, Swarm Evol. Comput. 64 (2021) 100911.
- [17] A.M. Altabeeb, A.M. Mohsen, A. Ghallab, An improved hybrid firefly algorithm for capacitated vehicle routing problem, Appl. Soft Comput. 84 (2019) 105728.
- [18] A.M. Machado, G.R. Mauri, M.C.S. Boeres, R. de Alvarenga Rosa, A new hybrid matheuristic of GRASP and VNS based on constructive heuristics, set-covering and set-partitioning formulations applied to the capacitated vehicle routing problem, Expert Syst. Appl. 184 (2021) 115556.
- [19] H. Jiang, M. Lu, Y. Tian, J. Qiu, X. Zhang, An evolutionary algorithm for solving capacitated vehicle routing problems by using local information, Appl. Soft Comput. 117 (2022) 108431, http://dx.doi.org/10.1016/j.asoc.2022.108431.
- [20] S.-Y. Tan, W.-C. Yeh, The vehicle routing problem: State-of-the-art classification and review, Appl. Sci. 11 (21) (2021) http://dx.doi.org/10.3390/app112110295.
- [21] I. Sbai, S. Krichen, O. Limam, Two meta-heuristics for solving the capacitated vehicle routing problem: the case of the Tunisian post office, Oper. Res. (2020) 1-43.
- [22] S. Pelletier, O. Jabali, G. Laporte, The electric vehicle routing problem with energy consumption uncertainty, Transp. Res. B 126 (2019) 225-255.
- [23] CVRPLIB, Capacitated vehicle routing problem library, 2021, http://vrp.atdlab.inf.puc-rio.br. [Online; accessed September-2022].
- [24] Q.-K. Pan, M.F. Tasgetiren, Y.-C. Liang, A discrete differential evolution algorithm for the permutation flowshop scheduling problem, Comput. Ind. Eng. 55 (4) (2008) 795-816, http://dx.doi.org/10.1016/j.cie.2008.03.003.
- [25] L. Wang, Q.-K. Pan, P. Suganthan, W.-H. Wang, Y.-M. Wang, A novel hybrid discrete differential evolution algorithm for blocking flow shop scheduling problems, Comput. Oper. Res. 37 (3) (2010) 509-520, http://dx.doi.org/10. 1016/j.cor.2008.12.004.
- [26] R. Rivera-López, E. Mezura-Montes, J. Canul-Reich, M.A. Cruz-Chávez, A permutational-based differential evolution algorithm for feature subset selection, Pattern Recognit. Lett. 133 (2020) 86-93, http://dx.doi.org/10.1016/j.patrec. 2020.02.021.
- [27] R.S. Prado, R.C.P. Silva, F.G. Guimarães, O.M. Neto, Using differential evolution for combinatorial optimization: A general approach, in: 2010 IEEE International Conference on Systems, Man and Cybernetics, 2010, pp. 11-18, http://dx.doi. org/10.1109/ICSMC.2010.5642193.
- [28] A. Moraglio, J. Togelius, S. Silva, Geometric differential evolution for combinatorial and programs spaces, Evol. Comput. 21 (4) (2013) 591-624, http: //dx.doi.org/10.1162/EVCO\_a\_00099.
- [29] V. Santucci, M. Baioletti, A. Milani, Algebraic differential evolution algorithm for the permutation flowshop scheduling problem with total flowtime criterion, IEEE Trans. Evol. Comput. 20 (5) (2016) 682-694, http://dx.doi.org/10.1109/ TEVC.2015.2507785.
- [30] B. Qian, L. Wang, D.-x. Huang, W.-l. Wang, X. Wang, An effective hybrid DEbased algorithm for multi-objective flow shop scheduling with limited buffers, Comput. Oper. Res. 36 (1) (2009) 209-233.
- [31] J.C. Bean, Genetic algorithms and random keys for sequencing and optimization, ORSA J. Comput. 6 (2) (1994) 154-160.
- [32] J.G. Sauer, L.d.S. Coelho, Discrete differential evolution with local search to solve the traveling salesman problem: Fundamentals and case studies, in: 2008 7th IEEE International Conference on Cybernetic Intelligent Systems, 2008, pp. 1-6, http://dx.doi.org/10.1109/UKRICIS.2008.4798955.
- [33] M. Tasgetiren, M. Sevkli, Y.-C. Liang, G. Gencyilmaz, Particle swarm optimization algorithm for single machine total weighted tardiness problem, in: Proceedings of the 2004 Congress on Evolutionary Computation (IEEE Cat. No. 04TH8753), Vol. 2, 2004, pp. 1412-1419, http://dx.doi.org/10.1109/CEC.2004.1331062, Vol.2.
- [34] M.F. Tasgetiren, M. Sevkli, Y.-C. Liang, G. Gencyilmaz, Particle swarm optimization algorithm for permutation flowshop sequencing problem, in: M. Dorigo, M. Birattari, C. Blum, L.M. Gambardella, F. Mondada, T. Stützle (Eds.), Ant Colony Optimization and Swarm Intelligence, Springer Berlin Heidelberg, Berlin, Heidelberg, 2004, pp. 382-389, http://dx.doi.org/10.1007/978-3-540-28646-2\_38.
- [35] P.B. Myszkowski, Ł.P. Olech, M. Laszczyk, M.E. Skowroński, Hybrid differential evolution and greedy algorithm (DEGR) for solving multi-skill resourceconstrained project scheduling problem, Appl. Soft Comput. 62 (2018) 1-14, http://dx.doi.org/10.1016/j.asoc.2017.10.014.
- [36] B.-h. Zhou, L.-m. Hu, Z.-y. Zhong, A hybrid differential evolution algorithm with estimation of distribution algorithm for reentrant hybrid flow shop scheduling problem, Neural Comput. Appl. 30 (1) (2018) 193-209, http://dx.doi.org/10. 1007/s00521-016-2692-y.
- [37] G. Onwubolu, D. Davendra, Scheduling flow shops using differential evolution algorithm, European J. Oper. Res. 171 (2) (2006) 674-692, http://dx.doi.org/ 10.1016/j.ejor.2004.08.043.
- [38] I.M. Ali, D. Essam, K. Kasmarik, A novel design of differential evolution for solving discrete traveling salesman problems, Swarm Evol. Comput. 52 (2020) 100607, http://dx.doi.org/10.1016/j.swevo.2019.100607.
- [39] B. Liu, L. Wang, Y.-H. Jin, An effective PSO-based memetic algorithm for flow shop scheduling, IEEE Trans. Syst. Man Cybern. B 37 (1) (2007) 18-27, http://dx.doi.org/10.1109/TSMCB.2006.883272.
- [40] I.M. Ali, D. Essam, K. Kasmarik, A novel differential evolution mapping technique for generic combinatorial optimization problems, Appl. Soft Comput. 80 (2019) 297-309, http://dx.doi.org/10.1016/j.asoc.2019.04.017.

- [41] M.F. Tasgetiren, Q.-K. Pan, P.N. Suganthan, I.E. Dizbay, Metaheuristic algorithms for the quadratic assignment problem, in: 2013 IEEE Symposium on Computational Intelligence in Production and Logistics Systems, CIPLS, 2013, pp. 131-137, http://dx.doi.org/10.1109/CIPLS.2013.6595210.
- [42] A. Hameed, B. Aboobaider, M. Mutar, N. Choon, A new hybrid approach based on discrete differential evolution algorithm to enhancement solutions of quadratic assignment problem, Int. J. Ind. Eng. Comput. 11 (1) (2020) 51-72.
- [43] S. Das, P.N. Suganthan, Differential evolution: A survey of the state-of-the-art, IEEE Trans. Evol. Comput. 15 (1) (2011) 4-31, http://dx.doi.org/10.1109/TEVC. 2010.2059031.
- [44] S. Das, S.S. Mullick, P. Suganthan, Recent advances in differential evolution -An updated survey, Swarm Evol. Comput. 27 (2016) 1-30, http://dx.doi.org/10. 1016/j.swevo.2016.01.004.
- [45] L. Mingyong, C. Erbao, An improved differential evolution algorithm for vehicle routing problem with simultaneous pickups and deliveries and time windows, Eng. Appl. Artif. Intell. 23 (2) (2010) 188-195.
- [46] L. Hou, H. Zhou, J. Zhao, A novel discrete differential evolution algorithm for stochastic VRPSPD, J. Comput. Inf. Syst. 6 (8) (2010) 2483-2491.
- [47] H. Xu, J. Wen, Differential evolution algorithm for the optimization of the vehicle routing problem in logistics, in: 2012 Eighth International Conference on Computational Intelligence and Security, 2012, pp. 48-51, http://dx.doi.org/ 10.1109/CIS.2012.19.
- [48] B.E. Teoh, S.G. Ponnambalam, G. Kanagaraj, Differential evolution algorithm with local search for capacitated vehicle routing problem, Int. J. Bio-Inspir. Comput. 7 (5) (2015) 321-342.
- [49] D. Dechampai, L. Tanwanichkul, K. Sethanan, R. Pitakaso, A differential evolution algorithm for the capacitated VRP with flexibility of mixing pickup and delivery services and the maximum duration of a route in poultry industry, J. Intell. Manuf. 28 (6) (2017) 1357-1376.
- [50] L. Song, Y. Dong, An improved differential evolution algorithm with local search for capacitated vehicle routing problem, in: 2018 Tenth International Conference on Advanced Computational Intelligence, ICACI, 2018, pp. 801-806, http://dx.doi.org/10.1109/ICACI.2018.8377563.
- [51] Bilal, M. Pant, H. Zaheer, L. Garcia-Hernandez, A. Abraham, Differential evolution: A review of more than two decades of research, Eng. Appl. Artif. Intell. 90 (2020) 103479, http://dx.doi.org/10.1016/j.engappai.2020.103479.
- [52] A.-l. Chen, G.-k. Yang, Z.-m. Wu, Hybrid discrete particle swarm optimization algorithm for capacitated vehicle routing problem, J. Zhejiang Univ. Sci. A 7 (4) (2006) 607-614.
- [53] L. Zheng, S. Luo, Adaptive differential evolution algorithm based on fitness landscape characteristic, Mathematics 10 (9) (2022) http://dx.doi.org/10.3390/ math10091511.
- [54] D. Vermetten, B.v. Stein, A.V. Kononova, F. Caraffini, Analysis of structural bias in differential evolution configurations, in: Differential Evolution: From Theory to Practice, Springer, 2022, pp. 1-22.
- [55] F. Caraffini, A.V. Kononova, D. Corne, Infeasibility and structural bias in differential evolution, Inform. Sci. 496 (2019) 161-179.
- [56] G.C. Onwubolu, D. Davendra, Differential Evolution: A Handbook for Global Permutation-Based Combinatorial Optimization, Vol. 175, Springer Science &amp; Business Media, 2009.
- [57] P. Augerat, J.M. Belenguer, E. Benavent, A. Corberán, D. Naddef, G. Rinaldi, Computational Results with a Branch and Cut Code for the Capacitated Vehicle Routing Problem, IMAG, 1995.
- [58] N. Christofides, S. Eilon, An algorithm for the vehicle-dispatching problem, J. Oper. Res. Soc. 20 (3) (1969) 309-318.
- [59] M.L. Fisher, Optimal solution of vehicle routing problems using minimum k-trees, Oper. Res. 42 (4) (1994) 626-642.
- [60] N. Christofides, A. Mingozzi, P. Toth, Loading problems, in: N. Christofides, et al. (Eds.), Combinatorial Optimization, 1979, pp. 339-369.
- [61] G. Reinelt, Tsplib95, Vol. 338, Interdisziplinäres Zentrum für Wissenschaftliches Rechnen (IWR), Heidelberg, 1995.
- [62] S. Das, P.N. Suganthan, Differential evolution: A survey of the state-of-the-art, IEEE Trans. Evol. Comput. 15 (1) (2010) 4-31.
- [63] T. Pichpibul, R. Kawtummachai, An improved clarke and wright savings algorithm for the capacitated vehicle routing problem, ScienceAsia 38 (3) (2012) 307-318.
- [64] J. Derrac, S. García, D. Molina, F. Herrera, A practical tutorial on the use of nonparametric statistical tests as a methodology for comparing evolutionary and swarm intelligence algorithms, Swarm Evol. Comput. 1 (1) (2011) 3-18.
- [65] C. García-Martínez, M. Lozano, Evaluating a local genetic algorithm as contextindependent local search operator for metaheuristics, Soft Comput. 14 (10) (2010) 1117-1139.
- [66] E. Uchoa, D. Pecin, A. Pessoa, M. Poggi, A. Subramanian, T. Vidal, New benchmark instances for the capacitated vehicle routing problem, European J. Oper. Res. 257 (3) (2017) 845-858.