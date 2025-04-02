![Image](image_000000_113a55498a64c5f6b7c87cd87ab92a7fa1dd282aa8a364a0eccbe25b2841754d.png)

Received 22 September 2023, accepted 9 October 2023, date of publication 18 October 2023, date of current version 14 November 2023.

Digital Object Identifier 10.1 109/ACCESS.2023.3325787

![Image](image_000001_9d57193b786d600825f737d1e808ab7a3cb92a099b439059320a0498c44d401f.png)

## Hybrid Algorithms for Energy Minimizing Vehicle Routing Problem: Integrating Clusterization and Ant Colony Optimization

## NICOLÁS FRÍAS , FRANKLIN JOHNSON , AND CARLOS VALLE

Departamento de Ciencia de Datos e Informática, Facultad de Ingeniería, Universidad de Playa Ancha, Valparaíso 2360002, Chile

Corresponding author: Nicolás Frías (nicolas.friasrojas@gmail.com)

This work was supported by Universidad de Playa Ancha under Grant DGI APFON 03-22.

ABSTRACT In the field of engineering, complex problems often arise that require solutions. The implementation of these algorithms plays a crucial role in achieving favorable outcomes with the available resources. The Vehicle Routing Problem (VRP) has been a central topic in distribution and logistics for decades. New VRP models and tools are developed to address the challenges of modern logistics. The Energy Minimizing Vehicle Routing Problem (EMVRP) is a ''green''-oriented variant of the VRP where the objective is to minimize the total amount of energy consumed by a fleet of vehicles. The VRP literature has focused on solving the problem using a variety of approaches and techniques, including exact methods, heuristics, metaheuristics, and hybrid algorithms. Hybrid algorithms combine different techniques to obtain more effective and better solutions. This work presents four innovative hybrid algorithms to address the EMVRP problem. These algorithms combine Machine Learning (ML) clustering techniques with metaheuristic approaches inspired by an Ant Colony Optimization (ACO). The proposed algorithms are: Free Ant + K-Means, Free Ant + K-Medoids, Restricted Ant + K-Means, and Restricted Ant + KMedoids. Each of them combines the benefits of clustering with the optimization capacity of ACO. Proposed algorithms were subjected to testing using instances from CVRPLIB. Both Free Ant and Restricted Ant efficiently solved EMVRP problems. The results obtained were analyzed and compared with the proposals of other authors in the literature. Overall, the results are promising, but they also indicate a significant scope for experimentation and parameter tuning of the proposed algorithms.

INDEX TERMS Complex problems, free ant, green-vrp, heuristics, k-means, k-medoids, machine learning, metaheuristics, restricted ant, vehicle routing problem (VRP).

## I. INTRODUCTION

One of the most widely known and oldest combinatorial optimization problem is the Traveling Salesman Problem (TSP), which involves determining the shortest possible route that visits all cities (from a city list) exactly once and returns to the starting city. Over time, the idea of optimizing routes for a finite number of cities has been extended to find optimal routes for vehicles from one or more depots to a set of locations or customers, thus being understood as the natural evolution of the TSP. Since its inception, this problem has generated significant interest among the Operations Research and mathematical communities. These problems are known as Vehicle Routing Problems (VRP) and belong to the field of combinatorial optimization.

The associate editor coordinating the review of this manuscript and approving it for publication was Wentao Fan .

Introduced in the early 1960s, the VRP has been extensively studied by the scientific community, primarily because of its practical applications in the field of distribution and logistics. Additionally, the VRP presents inherent complications that exist in this type of combinatorial optimization problem, similar to the TSP, making it part of the NP-Hard problem set. Whereas the TSP seeks the shortest route to visit

VOLUME 11, 2023

a certain number of cities and return to the origin, the VRP aims to determine the optimal set of routes for vehicles, visit all customers once, and serve their associated demands per vehicle. In the VRP, there is a finite number of routes, one for each available vehicle, and the goal is to determine the optimal set of routes. This characteristic makes the VRP a generalization of TSP.

The formal definition of a VRP states that m vehicles initially located at a depot must serve positive amounts of goods to n customers. The objective is to determine the K optimal routes used by the fleet of vehicles to fulfill the demands of the n customers. VRP have many practical applications, especially in transportation and distribution logistics. Consequently, various VRP variants have been developed to better represent our reality and involve variables that approximate the actual costs associated with traversing different vehicle routes. Models such as the Capacitated Vehicle Routing Problem (CVRP), Dynamic Vehicle Routing Problem (DVRP), and Vehicle Routing Problem with Time Windows (VRPTW) are only a few current frameworks within this problem class, and they can differ significantly from one another or involve constraints from other types of problems.

This article presents the results of a research project focused on the Energy Minimizing Vehicle Routing Problem (EMVRP), which is one of the most interesting models in the field of vehicle routing optimization. This model stands out for its ability to minimize both the financial costs and environmental impact of a fleet of vehicles while traversing different routes. Therefore, the EMVRP has emerged as a valuable tool for addressing logistical and environmental challenges in route planning and distribution. Originally published by Kara et al. [1], the EMVRP is a key problem within the Green VRP domain. In this study, we propose an algorithm to solve EMVRP problems using constructive metaheuristic techniques and clustering algorithms. The study is divided into three parts: the EMVRP problem, clustering algorithms, and the use of the Ant Colony Optimization (ACO) metaheuristic technique. Our hypothesis in this work is that we believe that mixed strategies combining metaheuristics and machine learning propose better solutions than these techniques separately. We present two innovative metaheuristic algorithms for problem resolution based on the Best-Worst Ant System algorithm proposed by Cordón et al. [2]. These algorithms, named Free Ant and Restricted Ant, are combined with clustering techniques, specifically the K-Means and K-Medoids algorithms with capacity restriction, inspired by the work of Geetha et al. [3].

## II. ENERGY MINIMIZING VEHICLE ROUTING PROBLEM

Let us consider an arbitrary Capacitated Vehicle Routing Problem (CVRP). This problem is defined by a graph G = ( V , A ), where V = v 1 , v 2 , . . . , vi is the set of nodes representing the customers in the problem, with v 0 is the depot, and vi is the i -th node or vertex. These nodes have positions with coordinates represented as ci = ( xi , yi ) in a

![Image](image_000002_3041fbd45ef3625371e1409d9b01a651ca4774bd544ec23c688389fde603fa86.png)

̸

Cartesian plane. A = ( i , j ) : i , j ∈ V , i = j is the set of arcs or edges connecting each node in set V , where ( i , j ) represents the arc connecting node vi to node vj .

Additionally, we have the following characteristics that complete the EMVRP model:

- · dij is the distance between node i and node j ,
- · qi is the positive integer load of node vi , i.e., its demand or supply,
- · m is the number of heterogeneous vehicles,
- · Q 0 is the tare weight (weight of the vehicle when empty) of these vehicles,
- · Q is the maximum capacity of a vehicle.

Kara et al. [1] defined the Energy Minimizing Vehicle Routing Problem (EMVRP) as a vehicle routing problem, such that:

- · Each node vi is served by exactly one vehicle,
- · Each route starts and ends at the depot v 0,
- · The load on the arcs accumulates as the sum of quantities qi supplied by the preceding nodes (in the case of pickup) and decreases as the sum of quantities qi demanded by the preceding nodes (in the case of delivery),
- · The load of a vehicle must not exceed the capacity Q ,
- · The objective is to find a set of K vehicle routes with the minimum total cost, that is, the minimum total energy.

Furthermore, the following decision variables are defined for the formulation of this problem:

- 1) Xij = 1 if the arc ( i , j ) is part of a route, and 0 otherwise,

2)

Yij is

the weight of the vehicle when traveling from node

vi to node

vj

, and 0 otherwise.

As can be inferred, Yij is the component that gives meaning to this new energy-oriented approach. The weight of the vehicle on the first arc of any route must have a predetermined value; for example, tare weight Q 0, and it should always increase (or decrease) by units of qi immediately after visiting node vi . In the case of pickup, the flow of the variable exhibits an incremental step function, whereas in the delivery case, it exhibits a decremental step function. Therefore, a model constructed for one case may not be suitable for its counterpart. However, Kara et al. demonstrated that when distances are symmetric, the optimal route for the delivery (or pickup) case is equal to the optimal route for the pickup (or delivery) case, but traversed in the reverse order. Thus, if the distances are symmetric, there is no need to differentiate between delivery and pickup, because the solution for one case is the solution for the other, but in the reverse order. Therefore, the objective function to be minimized is as follows :

$$\min \sum _ { i = 0 } ^ { n } \sum _ { j = 0 } ^ { n } d _ { i j } Y _ { i j }.$$

This objective function (1) is subject to the following constraints :

$$\sum _ { i = 1 } ^ { n } X _ { 0 i } = m.$$

![Image](image_000003_3041fbd45ef3625371e1409d9b01a651ca4774bd544ec23c688389fde603fa86.png)

$$\sum _ { i = 1 } ^ { n } X _ { i 0 } & = m. & ( 3 ) & \quad \text{B} \\ \quad \text{prop} \\ \quad \text{term}$$

$$\sum _ { \substack { i = 0 \\ n } } ^ { \cdot - \cdot \\ X _ { i j } } = 1, \ \ j = 1, 2, 3, \dots, n. \quad \ ( 4 ) \quad \text{ term} \\ \text{ char} \\ \text{ conc} \\ \text{ the } \colon$$

$$\sum _ { \stackrel { j = 0 } { \stackrel { n } { \stackrel { n } { \stackrel { n } } } } } X _ { i j } = 1, \quad i = 1, 2, 3, \dots, n. \quad \quad ( 5 ) \quad \text{the } \colon \\ \quad \text{$\quad$ will} \\ \quad \text{$\quad$emp}$$

̸

$$\sum _ { \substack { j = 0 \\ j \neq i } } ^ { n } Y _ { i j } - \sum _ { \substack { j = 0 \\ j \neq i } } ^ { n } Y _ { j i } = q _ { i }, \quad i = 1, 2, 3, \dots, n. \quad ( 6 ) \quad \text{$\cdots r$} \\ \cdots \quad \sim \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \end{matrix}$$

̸

$$Y _ { 0 i } = Q _ { 0 } X _ { 0 i }, \ \ i = 1, 2, 3, \dots, n. \quad \ \ ( 7 ) \quad \frac { \text{com} } { \text{ori} \alpha }$$

$$Y _ { i j } \leq ( Q + Q _ { 0 } - q _ { j } ) X _ { i j }, \ \ ( i, j ) \in A. \quad \ \ ( 8 ) \quad \stackrel { \cdots \pi } { \text{spec} }$$

$$Y _ { i j } \geq ( Q _ { 0 } + q _ { i } ) X _ { i j }, \quad \forall ( i, j ) \in A. \quad \quad ( 9 ) \quad \bar { \text{ACC} }$$

$$X _ { i j } = 0 \, o \, 1, \quad ( i, j ) \in A. \quad \quad ( 1 0 ) \quad \text{ with }$$

The cost of traversing an arc ( i , j ) is the product of the distance dij (the distance between nodes vi and vj ) and the weight Yij carried by the vehicle on this arc, which is satisfied by the objective function given in (1). Furthermore, Constraints (2) and (3) ensure that all m vehicles are used. Constraints (4) and (5) represent the degrees of restriction for each node. Constraint (6) represents the classical conservation of the flow equation, which balances the inflow and outflow of each node (in terms of demand or supply) and prohibits any illegal sub-trips. Constraint (7) initializes the flow in the first arc of each route. Constraint (8) handles the capacity constraints and forces the value of Yij to be zero when arc ( i , j ) is not a part of any route. Constraint (9) sets the lower bounds for flow on any arc. The integrity constraint is given in (10), and corresponds to the binary variable for each arc.

## III. BACKGROUND

The state-of-the-art analysis in this work focused on addressing contemporary challenges in logistics and distribution, using the Energy Minimizing Vehicle Routing Problem (EMVRP) as a case study. These logistical problems encompass a wide array of challenges and complexities. We believe that the application of heuristics, metaheuristics and hybrid algorithms, such as ant colony optimization and clustering, has the potential to provide optimal or near-optimal solutions for logistics and distribution-related issues. The combination of these techniques can be beneficial in approaching these problems from various perspectives:

- 1) Optimization: Metaheuristics can find optimal or near-optimal solutions for logistics and distribution problems.
- 2) Adaptability: Metaheuristics can adapt to changing situations and cater to different types of problems, depending on current needs.
- 3) Complex Problems: Hybrid algorithms combine different techniques, such as machine learning and optimization, to effectively address challenging problems.

Before delving into the development of the algorithms proposed in this study, it is essential to introduce the terms that will be frequently used throughout the following chapters. Therefore, this section presents some fundamental concepts necessary to comprehend the processes involved in the algorithms proposed in this study. By doing so, readers will be able to gain a clear understanding of the terms employed in the analysis of Free Ant and Restricted Ant.

## A. ANT COLONY OPTIMIZATION

Ant Colony Optimization (ACO) acts as a tool for solving complex combinatorial optimization problems and was originally designed to find the optimal path within a graph, specifically a Traveling Salesman Problem (TSP) graph. ACO can be understood as a bio-inspired metaheuristic with a general-purpose application, arising from the genuine observation that a colony of ants is capable of finding an optimal path between their nest and a food source. Initially, real ants, explore the area around their nest in a random manner when searching for food, leaving behind a trail of pheromones. This pheromone trail affects subsequent ants, gradually causing them to converge towards an optimal path.

The foundation of the Ant Colony Optimization theory was introduced by Marco Dorigo in his doctoral thesis in 1992. However, it was not until the mid-1990s that Dorigo, M., Maniezzo, V., and Colorni, A. [4] published it as a novel metaheuristic called Ant System (AS). AS employs a fixed number of artificial ants ( w ) where each ant a (considered a computational agent) constructs a solution to the problem, previously represented by a graph. In this graph, each arc ( i , j ) corresponds to one of the possible choices available to the ant to transition from its current state to a new state.

Ant a is only aware of the information associated with these arcs ( i , j ), which consists of the following:

- 1) Heuristic information η , which determines the heuristic preference of an ant to move from one state to another. This information typically refers to the cost of transitioning from node vi to node vj , and in practice, it remains unchanged throughout the execution of the algorithm.
- 2) Pheromone trail information τ , which measures the learned preference for state transitions among the w ants. This information aims to mimic the real pheromone deposited by natural ants and is a data value that will be modified during the course of the execution of the algorithm, with the purpose of aiding future ants in selecting better routes.

The development of a generic ACO algorithm requires artificial ant a to have memory Ma , where it stores each visited node vi . This list is commonly known as the taboo list because it contains all nodes that cannot be revisited (as they are part of a route). The list of the remaining nodes that the ant can visit is denoted as Ja . To determine the next node vj that the ant will visit, the transition rule (11), also known as the standard probability distribution pa of AS, is used to

determine the next node vj that the ant will visit.

$$p _ { a } ( r, s ) = \begin{cases} \frac { [ \tau ( r, s ) ] ^ { \alpha } [ \eta ( r, s ) ^ { \beta } ] } { \sum _ { u \in J _ { a } ( r ) } [ \tau ( r, u ) ] ^ { \alpha } [ \eta ( r, u ) ] ^ { \beta } }, & \text{if} s \in J _ { a } ( r ), & \text{prero} \\ 0, & \text{otherwise}, & \text{rapid} \\ \dots \, \cdots \, \cdots \, \cdots \end{cases}$$

(11)

where r corresponds to node vi and s corresponds to node vj . τ represents the pheromone trail on arc ( i , j ), and η denotes the heuristic information on the same arc, typically the inverse of the distance d r ( , s ) ( 1 d r s ( , ) ). Ja ( r ) represents the set of nodes that are yet to be visited by ant a starting from node r . Furthermore, α and β are adjustable parameters that determine the relative importance of the pheromone trail versus the relative importance of the heuristic information, respectively.

In ACO, it is necessary to modify the pheromone trails associated with arcs in each iteration. For example, in AS, at the end of each iteration, there is an evaporation of the pheromone trails, followed by the deposition of a determined number of pheromones by the w ants. The update of the pheromone τ rs , associated with the arc ( i , j ) connecting nodes vi and vj , occurs as follows :

$$\tau _ { r s } \leftarrow ( 1 - \rho ) \cdot \tau _ { r s } + \sum _ { a = 1 } ^ { w } \Delta \tau _ { r s } ^ { a }. \quad \ \ ( 1 2 ) \quad \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \$$

where ρ is the evaporation coefficient (a predefined value within the range [0 , 1]), and 1τ a rs is the amount of pheromone deposited on arc ( i , j ) by ant a . The amount of pheromone 1τ a rs that ant a can deposit can be calculated as follows :

$$\Delta \tau _ { r s } ^ { a } = \begin{cases} \frac { 1 } { L _ { a } } & \text{if ant $a$ uses arc $(i,j)$ in its solution,} \\ 0, & \text{otherwise,} \end{cases} \quad ( 1 3 ) \quad \text{separ}$$

where La is the length of the solution constructed by ant a . Since the introduction of AS, various variants of ACO algorithms have been proposed to solve combinatorial optimization problems. Some are more closely inspired by AS, whereas others diverge further. However, their main ideas are similar:

- 1) The use of a colony of ''ants'' that cooperate with each other,
- 2) Artificial ''pheromone'' trails for local communication,
- 3) A sequence of local movements, and
- 4) A stochastic decision policy that utilizes local information.

Somewell-known ACO algorithms include the Ant Colony System (ACS) [5], AS rank [6], and MAX-MIN Ant System (MMAS) [7], among others. For more details on Ant Colony Optimization, please refer to [8].

## B. MAX-MIN ANT SYSTEM

As presented by Stützle, T. &amp; Hoos, H. [7], the MAX-MIN Ant System (MMAS) differs from standard Ant Colony Optimization (ACO) algorithms in that it limits the amount of pheromone that can be deposited on an arc ( i , j ). This is

![Image](image_000004_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

achieved by setting a minimum value τ min and a maximum value τ max of pheromone for each arc and updating the pheromone value only if it falls within this range. This restriction ensures that the algorithm does not converge rapidly to suboptimal solutions. The determination of τ min and τ max in MMAS are given by :

$$\tau _ { m a x } = \frac { 1 } { 1 - \rho } \frac { 1 } { f ( S ^ { g b } ) }. \quad \quad \quad ( 1 4 )$$

$$\tau _ { \min } = \frac { \tau - \rho _ { J } \, \mathfrak { \omega } \, \mathfrak { \omega } } { ( a v g - 1 ) \sqrt { P _ { b e s t } } }. \quad \quad ( 1 5 )$$

Here, f ( S gb ) represents the value of the best global solution of the problem, Pbest is the probability of convergence to the best-found solution, which is typically assumed to be 0 05. Finally, . avg denotes the average choice among the different components of the solution, i.e., avg = n / 2.

In general, the MAX-MIN Ant System is an effective and widely used optimization algorithm that has been successfully applied to a broad range of problems, including the Traveling Salesman Problem (TSP), Quadratic Assignment Problem (QAP), and Vehicle Routing Problem (VRP).

## C. BEST-WORST ANT SYSTEM

The Best-Worst Ant System (BWAS) algorithm is an ACO type algorithm developed by Cordón et al. [2], which incorporates concepts from evolutionary computation. This algorithm utilizes the AS transition rule (11) and does not require online pheromone updates (12), thus making it an optional step. Furthermore, this algorithm relies on three essential actions performed by an external program separate from the ant colony. This program is a special type of non-interactive computational process known as a ''daemon.'' We briefly outline these actions as follows:

- 1) Only the best global solution, referred to as ''bestglobal,'' and the current worst solution, referred to as ''worst-current,'' are considered for performing positive and negative pheromone updates, respectively (offline update). This update is applied once the w ants have constructed their solutions and before the iteration end.
- a) The offline update is applied as follows :

$$\tau _ { r s } \leftarrow ( 1 - \rho ) \cdot \tau _ { r s } + \triangle \tau _ { r s }. \quad \quad ( 1 6 )$$

$$\begin{cases} \bigtriangleup \tau _ { r s } = f ( C ( S _ { b e s t - g l o b a l } ) ), \\ \text{ if } ( r, s ) \in S _ { b e s t - g l o b a l }, \\ 0, \text{ otherwise.} \end{cases}$$

where f ( C ( )) · corresponds to a function that evaluates the quality of any given solution.

- b) On the other hand, the penalty of arcs in the current worst solution, ''worst-current,'' is performed as follows :

$$\forall ( r, s ) \in S _ { w o r s t - c u r r e n t } \text{ and } ( r, s ) \notin S _ { b e s t - g l o b a l }, \\ \tau _ { r s } \leftarrow ( 1 - \rho ) \cdot \tau _ { r s }. \quad ( 1 8 )$$

![Image](image_000005_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

- 2) The daemon process also incorporates a pheromone matrix reset procedure. This occurs whenever a defined stagnation condition is satisfied.
- 3) The pheromone matrix underwent a series of stochastic mutations that were loosely inspired by evolutionary computations. The purpose of these mutations was to introduce greater diversity into the search process:
- a) Each row of the pheromone matrix is mutated (with a probability of Pm ) as follows :

$$\tau _ { r s } ^ { \prime } = \begin{cases} \tau _ { r s } + m u t ( i t, \tau _ { t h r e s h o l d } ), & \text{if $e=1$} \\ \tau _ { r s } - m u t ( i t, \tau _ { t h r e s h o l d } ), & \text{if $e=0$} \end{cases} \quad. \quad \text{u}$$

(19)

$$\tau _ { t h r e s h o l d } = \frac { \sum _ { ( r, s ) \in S _ { b e s t - g l o b a l } } \tau _ { r s } } { | S _ { b e s t - g l o b a l } | }. \quad \quad ( 2 0 ) \quad \text{$Anc} \\ \text{$\text{for}$} ]$$

where e is a random value in { 0 1 , , } it represents the current iteration, and τ threshold is the average pheromone value of the best global solution.

- b) Furthermore, the function mut () is described as follows :

$$\ m u t ( i t, \tau _ { t h r e s h o l d } ) = \frac { i t - i t _ { r } } { N _ { i t } - i t _ { r } } \cdot \sigma \cdot \tau _ { t h r e s h o l d }. \quad \inf _ { \text{ are} } \\ ( 2 1 ) \quad \text{$\text{$\text{$\text{$\text{$if}$} }$} }$$

where Nit is the maximum number of iterations, it r is the last iteration where a pheromone matrix reset is performed, and σ is a constant that specifies the intensity of the mutation relative to the number of iterations conducted so far.

## D. K-MEANS

One of the most commonly used algorithms for clustering datasets is K-Means, which is known for its simplicity and effectiveness. This algorithm (also known as Lloyd's Algorithm) was originally designed by Lloyd, S. P. [9], although the term ''K-Means'' was coined in 1967 by MacQueen, J. B. [10]. K-Means belongs to the field of unsupervised machine learning and takes a set of elements and assigns each of them to one of the K groups. This is based on the inherent features of each element, that is, their values xi . The goal of the algorithm is to find the optimal K centroids that minimize the sum of the squared distances between all the objects in a group and their centroid.

In the classical sense of this algorithm, the centroids are initially placed randomly in space, and the algorithm iteratively assigns each element vi to one of the K groups based on its similarity or proximity to the centroids. Each element is assigned to group G with the smallest distance to its respective centroid, where G = G 1 , G 2 , G 3 , . . . , GK represents the set of groups. The equation for minimizing the sum of squared distances across all groups Gi , is given by :

$$\arg m i n \sum _ { i = 1 } ^ { K } \sum _ { c _ { j } \in G _ { i } } ^ { n } \left \| | c _ { j } - \mu _ { i } | \right \| ^ { 2 }. \quad \quad ( 2 2 ) \quad \text{$\mathfrak { by }$} \\ \text{cron}$$

.

where n corresponds to the number of elements in group Gi , cj represents the Cartesian coordinates of the j -th element (i.e., the values xi ), and i is the average of the Cartesian coordinates among the nodes in group Gi , representing the coordinates of the i -th centroid belonging to that group. The K centroids (1 , 2 3 , , . . . , K ) are then recalculated as the average of the coordinates of all nodes within each respective group in G . Because it is an iterative algorithm, the groups are redefined as the K centroids are recalculated, and the nodes are reassigned to the groups organically after each iteration until convergence is reached.

## E. K-MEDOIDS

Another algorithm in the field of unsupervised machine learning is the Partition Around Medoids (PAM), originally proposed by Kaufman, L., &amp; Rousseeuw, P. J. [11], and is commonly referred to as ''K-Medoids.'' Similar to K-Means, this algorithm works by dividing the dataset into K groups. The idea behind K-Medoids arises, in part, from the fact that K-Means is often sensitive to outliers. This sensitivity occurs because the centroids, denoted as i , can be strongly influenced by extreme values (elements whose coordinates are significantly distant from the rest), because the centroids are calculated as the average coordinates of the elements. Thus, K-Medoids is a classification technique that segments the elements of a dataset; however, unlike K-Means, where the centroids are not necessarily part of the groups, in KMedoids, an object from the dataset is selected as the centroid of its group. This central object is referred to as a medoid.

The working mechanism of K-Medoids follows an iterative process similar to that of K-Means. In the initial step, the medoids of each group are initialized randomly. Each element is then associated with one of the groups in G based on the closest proximity to its respective medoid Mi . In each subsequent iteration, the K medoids ( M 1 , M 2 , M 3 , . . . , MK ) are recalculated in terms of the similarity among the elements within the group. In other words, the new medoid Mi of the i -th group is determined as the object that, on average, has the lowest distance to the rest of the objects within the same group. The cost of group Gi in K-Medoids is given by :

$$\text{cost} _ { G i } = \sum _ { c _ { j } \in G _ { i } } | c _ { j } - M _ { i } |.$$

where G represents the set of groups, n is the total number of non-medoid elements in a group, Mi denotes the coordinates of the medoid belonging to group Gi , and cj represents the coordinates of the j -th non-medoid element within group Gi .

## F. VARIABLE NEIGHBORHOOD SEARCH

Variable Neighborhood Search (VNS) is commonly described as a framework rather than a specific local search algorithm, as it provides guidelines for implementing more effective local searches. The VNS was originally introduced by Hansen, P. &amp; Mladenovic, N. [12] in 1997 as a method for constructing local search heuristics. It is based on systematic changes of neighborhoods, involving a descent phase to reach

a local minimum and a shaking phase to escape the current valley, that is, to search for a better local minimum.

The VNS is considered a metaheuristic that exploits the idea of changing neighborhoods to find improved solutions. To achieve this, the algorithm relies on the following three observations:

- 1) A local minimum with respect to one neighborhood structure may not be the same for another.
- 2) A global minimum is a local minimum with respect to all possible neighborhood structures.
- 3) For many problems, the local minimum with respect to one or more neighborhoods is relatively close to each other.

This empirical observation implies that a local optimum often provides some information about the global optimum.

## G. CLARKE AND WRIGHT'S ALGORITHM

The Clarke &amp; Wright algorithm (also known as Saving's Algorithm) was introduced by Clarke, G. &amp; Wright, J.W. [13] in 1964. It is one of the most popular heuristic techniques for solving the Vehicle Routing Problem (VRP). The central idea of this algorithm is to combine two routes into a single new route, ensuring that ''savings'' are achieved. The potential savings from combining two routes is given by :

$$g _ { i j } = d _ { i 0 } + d _ { 0 j } - d _ { i j }. \quad \quad \ ( 2 4 ) \quad \ \bigcap \.$$

Here, gij represents the savings value when two nodes are integrated into the same route (greater values indicate higher savings); di 0 is the distance between node vi and the depot v 0, d j 0 is the distance between depot v 0 and node vj , and dij is the distance between node vi and node vj .

The algorithm operates as follows: first, each customer is assigned to a vehicle. Then, it attempts to combine two routes, provided that the vehicle's capacity allows it. The order in which the routes are combined is determined using (24). These savings values are sorted in descending order, and the route that generates the highest savings is selected and added to the merged route. The Clarke &amp; Wright algorithm has two approaches: Parallel and Sequential. The Parallel approach creates routes simultaneously, whereas the Sequential approach creates one route at a time.

## IV. METHODOLOGY

This section presents the working methodology and the processes involved in each of the proposed algorithms: Free Ant and Restricted Ant. Furthermore, the notation k is used interchangeably to refer to both centroids and medoids M from this point onward.

The overall behavior of the proposed algorithms in this work can be observed in Fig. 1, which presents, in a flowchart, the clustering stage and then the ant colony optimization stage.

## A. CLUSTERING ALGORITHMS

The purpose of these algorithms is to obtain information that can assist the ants in making better decisions during

![Image](image_000006_3041fbd45ef3625371e1409d9b01a651ca4774bd544ec23c688389fde603fa86.png)

state transitions. These clustering algorithms are used to generate groups of nodes that can potentially form a single route. Therefore, the clustering algorithms are executed at the beginning of the main program execution, once the data for an instance has been loaded, various parameters that control the algorithms have been set, and the distances between all nodes, as well as the energy cost Cij between each node, have been calculated.

## 1) IMPROVED K-MEANS ALGORITHM FOR CAPACITATED CLUSTERING PROBLEM

The K-Means algorithm typically does not consider any constraints when generating clusters because it focuses solely on identifying similar elements. To maintain the vehicle capacity constraint within each route, an improved version of K-Means called the Improved K-Means Algorithm for Capacitated Clustering Problem, has been utilized. This algorithm was proposed by Geetha et al. [3] and its main advantage lies in considering the maximum capacity Q of the m vehicles in a Capacitated Vehicle Routing Problem (CVRP). To avoid breaking this capacity constraint, nodes are grouped using a prioritization system.

The Improved K-Means Algorithm incorporates several modifications to better handle situations where there is an intra-cluster maximum capacity constraint. The customer nodes are assigned to the nearest groups in a similar manner to any K-Means algorithm, but the assignment occurs in an ordered manner based on a characteristic of each customer: their demand. Customers with higher demands qi and minimum distances dikj with respect to the centroids are given higher priority when being assigned to a group. Therefore, it is necessary to sort the customers (based on their respective demands) at the beginning of the algorithm in descending order.

Once the nodes are sorted, they are assigned to the groups whose centroid are closest to them. To determine the initial centroid locations, the coordinates of the nodes with higher demand that are farthest from the depot and the already selected centroids are considered. This approach is based on the work of Arthur, D. and Vassilvitskii, S. [14], who demonstrated that the initial position of the centroids directly affects the final result of the clustering process. They refer to this improved version of the K-Means algorithm as K-Means ++ .

To determine the optimal number of routes, that is, the optimal K , the following equation is used :

$$K = \sum _ { i = 1 } ^ { n } \frac { q _ { i } } { Q }.$$

On the other hand, to calculate the priority of each node vi , the next equation is used :

$$\text{Priority } P _ { i } = d _ { i k _ { j } } / q _ { i }. \text{ \quad \ \ } \text{(2.6)}$$

Here, dikj corresponds to the distance between node vi and the k -th centroid of each j -th group. Once all nodes have been

FIGURE 1. Generalized flowchart of the proposed hybrid algorithms.

![Image](image_000007_3041fbd45ef3625371e1409d9b01a651ca4774bd544ec23c688389fde603fa86.png)

![Image](image_000008_28215b47d9d98390057e71a55afe3bee77afc4f80a97fe6d6aae031cf46d7c92.png)

assigned to a group, the coordinates of each centroid kj is recalculated. If convergence is not achieved, the priority Pi needs to be recalculated for a new iteration.

In the context of VRP problems, the optimal number of clusters that clustering algorithms should generate will always be equal to the number of routes, that is, the optimal K . Therefore, the number of clusters is determined by (25).

## 2) IMPROVED K-MEDOIDS ALGORITHM FOR CAPACITATED CLUSTERING PROBLEM

The K-Medoids algorithm for Capacitated Clustering follows the same principles as the algorithm proposed by Geetha et al. [3], but it is governed by the criteria of K-Medoids. Once again, we employ the idea of K-Means++ to select the most distant nodes as the initial medoids.

Each node is assigned to its nearest group, that is, the group whose medoid is closest in terms of distance. Similar to the Capacitated K-Means approach, a priority system based on distance and demand was used for node assignment. Once all nodes have been assigned to a group, the K medoids must be recalculated. To choose the new medoid for each group in Gj , it is necessary to determine which node in the group has the lowest average cost compared with the other members of the same group, according to (23).

## B. ACO ALGORITHMS

The ACO Algorithms developed in this work are Free Ant and Restricted Ant, and both are based on the BWAS system. The execution of these algorithms begins after the clustering process.

## Algorithm 1 Clustering Algorithms

## 1: Initialization of variables

2: K

3: totalCost

4: function GetFirstCentroids( K )

5:

for idxk in K do

6:

if centroidsList is empty then

7:

Find the farthest node from the depot

8:

centroidsList ← node

9:

else

10:

Find the farthest node from the rest of

centroids

11:

centroidsList ← node

12:

end if

13:

end for

14:

return centroidsList

15:

end function

16:

while actualIteration &lt; maxIterations do

17:

unassignedNodes

18:

Calculate

distances

from

every

node

to

every

centroid

19:

for node in nodesList do

20:

if noder exists in unassignedNodes then

21:

Gets closest clusters to noder

22:

for cluster in closestClusters do

23:

Calculate distances from every node to

cluster

24:

Calculate priority for every node to

cluster

25:

Assign to this cluster the node with

highest priority

26:

Removethis node from unassignedNodes if noder not exists in unassignedNodes

27:

then

28:

Exit this FOR loop

29:

end if

30:

end for

31:

end if

32:

end for

33:

function CalculateTotalCost

34:

Sum all distances from every node to its centroid

35:

return newCost

36:

end function

37:

if newCost &lt; totalCost then

38:

bestClusters ← actualClusters

39:

bestCentroids ← actualCentroids

40:

totalCost ← newCost

41:

end if

42:

Recalculate centroids

43:

if actualCentroids = bestCentroids then

44:

Exit this WHILE loop

45:

end if

46:

end while

The pseudo-code for the ACO Algorithms is outlined in Algorithm 2.

![Image](image_000009_de51dee99cf343bd6ff25fb7338867596ad828e5a134b2111fbaf2407d2aab90.png)

## Algorithm 2 ACO Algorithms

## 1: Initialization of variables

2: unvisitedNodes

3: routes

4: while length of unvisitedNodes &gt; 0 do

5: newRoute

6: actualNode ← depot

7: maxCapacity

8: vehicleWeight ← tare

9: Calculate all the validNodes from unvisitedNodes

10:

while length of validNodes &gt; 0 do

11:

Calculate the heuristics from actualNode to

validNodes

12:

Calculate the

pheromones

from

actualNode

to

validNodes

13:

pseudoRandomChoice

14:

if pseudoRandomChoice ≤ l 0 then

15:

Calculate bestNode

16:

nextNode ← bestNode

17:

else

18:

Calculate all the probabilities with heuristics

and pheromones

19:

Choose

a

randomNode

based

on

these

probabilities

20:

nextNode ← randomNode

21:

end if

22:

Append nextNode to newRoute

23:

maxCapacity subtract demand of nextNode

24:

vehicleWeight sum demand of nextNode

25:

unvisitedNodes remove nextNode

26:

actualNode ← nextNode

27:

Recalculate all the validNodes from

unvisitedNodes

28:

end while

29:

Append newRoute to routes

30: end while

31: return routes

The algorithm begins by initializing the variables and setting up the necessary data structures. The main loop iterates until all the nodes have been visited. In each iteration, a new route is constructed. The algorithm selects the next node to visit based on the heuristics and pheromone information. The selection is influenced by a pseudo-random choice using parameter l 0. If the pseudo-random choice is less than or equal to l 0, then the best node is chosen. Otherwise, the selection is based on the probabilities calculated from the heuristics and pheromones. The selected node is added to the new route and the algorithm updates the remaining capacity and weight of the vehicle. The selected node is removed from the list of unvisited nodes, and the process continues until all the nodes have been visited. The resulting routes are returned as the outputs of the algorithm.

## 1) FREE ANT

The Free Ant algorithm utilizes information obtained from the best K clusters to determine the nodes belonging to each

![Image](image_000010_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

group. In addition, this information is used to construct the initial pheromone matrix. For this purpose, an amount of pheromone τ max is assigned to each arc ( i , j ) connecting nodes within the same group, whereas arcs ( i , j ) connecting nodes from different groups are assigned an initial amount of pheromone τ 0. The calculation of τ 0 is based on the equation developed by M. Dorigo and L.M. Gambardella in their work on the Ant Colony System [5]:

$$\tau _ { 0 } = \frac { 1 } { L _ { c } }.$$

Here, Lc corresponds to the best total energy obtained by a greedy algorithm.

To prevent possible stagnation, the amount of pheromone associated with each arc ( i , j ) is limited to the ranges { τ min , τ max } . The specific values for the minimum pheromone amount τ min and maximum pheromone amount τ max are determined according to (15) and (14), proposed in the Max-Min Ant System (MMAS).

Once the pheromone matrix is created and initialized, iterative execution of the Free Ant algorithm begins. In each iteration, a limited number of ants are deployed, and to maximize exploration, each ant is placed in a different node to start its construction. However, to minimize computation time, a candidate node list is used. For the design of the ACO algorithms in this work, it was decided that the composition of this list would be a homogeneous mixture of the closest nodes to depot v 0 in each group, the first selected nodes in each route from the solutions with the best results obtained in the previous iteration, and a small number of randomly selected nodes.

Once ant a is placed in its initial node, it starts constructing one of the K routes. The ant continues to select nodes to visit as long as the accumulated total demand in route r is less than the maximum capacity Q of the vehicle. When it is no longer possible to add nodes without violating this constraint, or when all nodes have been visited, the ant returns to the depot. This means that although there are nodes that can be added to the list representing the current route, the size of this list continues to increase by adding more nodes. When no more nodes are available to visit, a new route is created for the remaining available nodes. Once all nodes have been visited, the quality of the generated solution is calculated.

The quality of the solution is determined by the sum of all energies required for each route. This objective function is given by (1). Thus, we can observe that each ant a in the iterative process of the Free Ant algorithm has the total freedom (probabilistically) to choose which node in the graph to visit. Hence its name.

To improve the performance and decision-making capability of each ant a , this work decided to complement the heuristic information η by considering not only the distance dij but also the energy cost Cij and the savings value gij , such that :

$$\eta = [ d _ { i j } ] ^ { \beta } \cdot [ C _ { i j } ] ^ { \gamma } \cdot [ g _ { i j } ] ^ { \delta }. \quad \quad ( 2 8 ) \quad \text{pr}$$

where γ and δ are constants that determine the relative importance of Cij and gij , respectively. Thus, the standard probability distribution used by Free Ant and Restricted Ant is given by :

$$\text{ing} \quad & \text{is given by} \colon \\ \text{t of} \\ \text{ork} \quad & p _ { a } ( r, s ) = \begin{cases} \frac { [ \tau ( r, s ) ] ^ { \alpha } \cdot ( [ d _ { i j } ] ^ { \beta } [ C _ { i j } ] ^ { \gamma } [ g _ { i j } ] ^ { \delta } ) } { \sum _ { u \in J _ { a } ( r ) } [ \tau ( r, u ) ] ^ { \alpha } \cdot ( [ d _ { i j } ] ^ { \beta } [ C _ { i j } ] ^ { \gamma } [ g _ { i j } ] ^ { \delta } ) }, \\ \text{if} \, s \in J _ { a } ( r ), & \quad. \\ 0, & \quad \text{otherwise}, & \quad \\ \text{y a} \end{cases}.$$

The details of this strategy, along with others (such as the use of a candidate node list), can be found in the application recommendations of ACO for VRP problems by Bullnheimer, B., Hartl, R., and Strauss, C. [15] and Rizzoli, A.E., Montemanni, R., Lucibello, E. et al. [16].

## 2) RESTRICTED ANT

In Restricted Ant, the global problem is decomposed into smaller sub-problems: groups. Each group corresponds to one of the K routes. In this manner, we transform the original VRP problem into K TSP problems of lower complexity.

In this version of the BWAS algorithm, all elements τ ij are initialized with a value of τ max .

The iterative process of this algorithm operates differently from the Free Ant algorithm, as the iterations are performed for each group. For each group Gi , a determined number of ant deployments are repeated, and each ant a creates only a partial solution to the problem, specifically the solution for that group Gi .

Once again, we work with a candidate node list, one list for each group, but this time it will consist of a homogeneous distribution of nodes that are closest to the depot, the initial nodes of the best solutions obtained from the previous iteration, and a small number of randomly selected nodes. Each ant a starts its tour at one of these candidate nodes and probabilistically creates a solution based on (29). Naturally, ant a can only visit the nodes belonging to the same group.

Once all iterations in each group are completed, the best solution found in each group is combined into a single final solution, which represents the solution to the original problem.

## C. VNS ALGORITHMS

In this study, the VNS was used as a tool to guide the design and development of local search algorithms. In our case, these types of algorithms require an initial solution, provided by the ACO algorithms. Based on these solutions, the algorithm starts exploring the neighborhood space to find (or not) a better local optimum.

Two VNS-inspired algorithms were used in the program: one focused on improving the individual solution of a group, that is, the solution obtained by the Restricted Ant, and another to improve the solution of the entire problem provided by Free Ant.

## 1) 2-OPT VNS

This first algorithm is a simple variant of VNS and was implemented to improve the solutions delivered by the ants of the Restricted Ant. As the solutions provided by each of these ants are solutions to specific groups, the design of this algorithm is based solely on two intra-cluster operators: ''single-route-relocate'' and ''single-route-swap''.

The single-route-relocate operator relocates a node to a new position within the same route, whereas the single-routeswap operator exchanges the positions of two nodes, within the same route.

## 2) GENERAL VNS

This algorithm corresponds to a more comprehensive design of the VNS, and its implementation is aimed at improving the solutions obtained in the Free Ant.

In this version of the algorithm, the operators are divided into two groups: intra-cluster and inter-cluster. The intra-cluster operators are the same as in the 2-OPT VNS, while the inter-cluster operators are as follows: ''two-routesrelocate,'' which relocates a node from one route to a new position in another route; ''two-routes-swap,'' that exchanges two nodes between different routes; and finally, ''two-routesexchange,'' which exchanges two consecutive nodes from one route with two consecutive nodes from another route.

The inter-cluster operators are only viable and applicable when the newly generated solution does not violate the capacity constraint Q for any of the K routes.

## D. HYBRID ALGORITHMS

Due to the NP-Hard nature of many problems in the field of engineering, especially those of a combinatorial nature, various authors have proposed algorithms that successfully combine different techniques, such as heuristics, metaheuristics, machine learning, etc., in an attempt to solve these problems. A hybrid algorithm is one that combines two or more algorithms that solve the same problem, either by choosing one (based on the data) or switching between them during the course of the algorithm. This is generally done to combine desired features of each algorithm so that the final algorithm is better than its individual components. Some examples of hybrid algorithms include Machine Learning (ML) with Tabu Search (TS), iterative algorithms with integer programming, Genetic Algorithm (GA) with Local Search (LS), Ant Colony Optimization (ACO) with Local Search (LS), and so on.

Since the algorithms proposed in this work are hybrid algorithms, as they use a combination of clustering techniques with the ACO metaheuristic, a review of the state of the art related to these areas was conducted. We highlight the recent proposals of two studies focused on using K-Means to subdivide the search space for ACO.

One of these studies is by S.A. El-Khatib, Y.A. Skobtsov &amp; S.I. Rodzin [17], who present an efficient algorithm for image segmentation using a method that leverages all the

![Image](image_000011_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

TABLE 1. Specifications of the testing machine.

advantages of K-Means and ACO algorithms. The work of Kusumahardhini, N., Hertono, G. F. &amp; Handari, B. D. [18] was also reviewed, in which they combined K-Means with ACO to solve the Multiple Traveling Salesman Problem (MTSP).

## V. EXPERIMENTAL RESULTS

In this section, we will present the results of experiments obtained with the Free Ant and Restricted Ant algorithms, which were conducted on a series of instances from the CVRPLIB library. The libraries used for experimentation correspond to ''Set A (Augerat, 1995),'' ''Christofides, Mingozzi and Toth (1979),'' and ''Golden et al. (1998).''

The choice of these libraries was made because during the literature review stage of the EMVRP, two articles related to EMVRPwere identified. The first corresponds to the authors Rathnakumara and Rupasinghe [19], who developed a Tabu Search (TS) algorithm guided by a Machine Learning (ML) model. This algorithm is called ML-TS and was tested by the authors on the ''Set A (Augerat, 1995)'' instance as an EMVRP problem.

The second article is by the authors Kramer et al. [20], who developed a metaheuristic that combines Local Search (LS) with an integer programming approach and a speed optimization algorithm. This metaheuristic is called ILSSP-SOA and was tested by the authors on the instances ''Christofides, Mingozzi and Toth (1979)'' and ''Golden et al. (1998)'' as an EMVRP problem.

The specifications of the testing machine used for the benchmarks are listed in Table 1. The parameter settings are presented in Table 2. The choice of the values for these parameters is the result of an experimentation phase where empirically, values were sought that would generally yield the best results for both algorithms. The configuration used by Stützle, T., Hoos, H. [7] and that of Cordón et al. [2] served as a starting point, and the search for an optimal configuration began from there.

For this study, each instance of a CVRPLIB library was executed 10 times for each variant of the proposed algorithms. For each instance of each CVRPLIB library, ''Set A (Augerat, 1995),'' ''Christofides, Mingozzi and Toth (1979),'' and ''Golden et al. (1998),'' two tables are presented. The first table corresponds to the best result of each program execution, that is, the best total energy cost of all routes (objective function (1)) and the total computation time in seconds to obtain this solution. The calculation of time considers the time required by the clustering algorithm, added

![Image](image_000012_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

TABLE 2. Parameter configuration.

to the time of the ACO algorithm, and the time of the VNSlocal searches. Additionally, a second table presents the averaged results of the 10 executions and their average Gap. The average Gap is calculated by :

$$\ G a p = 1 0 0 \cdot \frac { S _ { a v g } - S _ { b e s t } } { S _ { b e s t } }. \quad \quad ( 3 0 ) \quad \ m a \quad \text{in}$$

where Savg corresponds to the average value of the 10 solutions found for an instance, and Sbest is the value of the best solution found among the 10 solutions.

The parameter configurations presented in Table 2 were used during the execution of the experiments. For instances from the libraries ''Set A (Augerat, 1995)'' and ''Christofides, Mingozzi and Toth (1979),'' 200 iterations were performed for each execution of the Free Ant algorithm, and 100 iterations were performed per group in the Restricted Ant algorithm. For instances from the ''Golden et al. (1998)'' library, the number of iterations were 300 and 200, respectively. The number of ants per iteration for the proposed algorithms was as follows: for Free Ant, it was determined by considering half of the total number of nodes in the problem, meaning that the number of ants was equal to 50% of the total number of nodes. In the case of Restricted Ant, the number of nodes varies depending on the group being solved. The numberofants was calculated by taking half of the node count of the current group, resulting in the number of ants being 50% of the number of nodes per group.

In Tables 3-5, we have six columns: Instance, Algorithm, Best Energy (Kj), Avg. Energy (Kj), Gap (%) and Time (Sec). The ''Instance'' column indicates the name of the instance being solved. The ''Best Energy (Kj)'' column provides the best energy value found for that instance in terms of kilojoules across the 10 executions. The ''Avg. Energy (Kj)'' column provides the average energy value across all executions for each algorithm. The ''Gap (%)'' column represents the averaged gap of all executions, as presented in (30). The ''Time (Sec)'' column indicates the time taken by the algorithm to find the best energy value in seconds. Finally, the ''Algorithm'' column indicates the algorithm applied to solve the instance and to which others columns values correspond. Four different algorithms are compared in the table: Free Ant + K-Means, Free Ant + K-Medoids, Restricted

Ant + K-Means, and Restricted Ant + K-Medoids. The best energy value found is the lowest value in terms of kilojoules among all the applied algorithms for a particular instance. The ant-based algorithms are divided into two types: ''free'' and ''restricted.'' The ''free'' algorithms use groups generated by the clustering-based algorithms to initially populate the pheromone matrix. In contrast, clustering-based algorithms are used to establish the search space in the ''restricted'' algorithms.

During the development of the experiments, we noticed that in some instances of CVRPLIB, the maximum capacity constraint per route was very tight compared to the total node demand. This caused the clustering algorithm (K-Means or K-Medoids) to be unable to find optimal solutions with each node assigned to a group. At the end of an iteration of this algorithm, some nodes remained unassigned. For this reason, the results tables do not show data for Restricted Ant in certain instances. However, in the case of Free Ant, this issue did not hinder the generation of the necessary pheromone matrix. The arcs related to the unassigned nodes still had an initial pheromone trail.

## A. SET A (AUGERAT, 1995)

This was the first set of instances considered in this study. This set consists of 22 groups of instances ranging from 30 to 78 nodes (excluding the depot). A vehicle capacity of zero has been considered for this set. The results obtained for this set are presented in Table 3.

## B. CHRISTOFIDES, MINGOZZI AND TOTH (1979)

This set consists of six groups of instances ranging from 50 to 199 nodes (excluding the depot). A vehicle tare equivalent to 15% of the maximum capacity was used for this set. For example, if the vehicle capacity Q is 200 units, the tare would be 30 units. 200 iterations were run per execution for the Free Ant algorithm, and 100 iterations were run for the Restricted Ant algorithm. The results obtained for this set are presented in Table 4.

## C. GOLDEN ET AL. (1998)

This set consists of 15 groups of instances ranging from 200 to 483 nodes (excluding the depot). The same tare used for the Christofides, Mingozzi, and Toth set, i.e., 15% of the maximum capacity Q , was used for this set as well. The results obtained for this set are presented in Table 5.

## VI. RESULTS ANALYSIS

In this section, we analyze the obtained results. To compare the results of the proposed algorithms, we referred to two different works. In the first one, Rathnakumara and Rupasinghe [19] presented the ML-TS algorithm, which is based on clustering and tabu search, to solve the EMVRP. In the second work, Kramer et al. [20] proposed the ILS-SPSOA algorithm, which combines exact techniques and local searches to solve the Pollution-Routing Problem and other Green-VRP problems.

![Image](image_000013_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

## TABLE 3. Results of Set A (Augerat, 1995).

![Image](image_000014_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

TABLE 3. (Continued.) Results of Set A (Augerat, 1995).

Both authors achieved favorable results in their respective benchmarks using the CVRPLIB library.

in most problems. This is evident in the lower values in the Time (Sec) column.

## A. SET A (AUGERAT, 1995)

Table 6 present the best results obtained and compare them with the best results of the ML-TS algorithm. Additionally, the difference (Gap) between the best energy obtained by Free Ant + K-Means, Free Ant + K-Medoids, Restricted Ant + KMeans, and Restricted Ant + K-Medoids is shown compared to the results provided by Rathnakumara and Rupasinghe. The calculation of this Gap is done using the next equation :

$$\text{Gap Best Energy} = 1 0 0 \cdot \frac { S _ { M L - T S } - S _ { \text{best} } } { S _ { \text{best} } }. \quad ( 3 1 ) \quad \text{Min}. \quad \text{obta}$$

Here, SML -TS corresponds to the best energy obtained by ML-TS for the instances in Set A (Augerat, 1995).

The data presented in Table 3 indicate that, in general, in terms of energy minimization, the Free Ant algorithms achieve better results than the Restricted Ant algorithms.

Furthermore, Table 3 reveals that Free Ant + K-Medoids achieves a higher solution quality, where lower values indicate better solutions. On the other hand, the Restricted Ant algorithms tend to generate lower-quality solutions compared to Free Ant algorithm. However, Restricted Ant algorithms are much more efficient, with Restricted Ant + KMeans being the fastest algorithm in completing its iterations

Compared to ML-TS, the Free Ant algorithms achieved better results for 20 out of 28 instances in Set A (Augerat, 1995), with an average Gap of -7.08% below the results reported by G. W. H. H. P. Rathnakumara and T. D. Rupasinghe. Restricted Ant yielded an average Gap of 12.21% above the results.

## B. CHRISTOFIDES, MINGOZZI AND TOTH (1979)

Table 7 presents data corresponding to the ''Christofides, Mingozzi and Toth (1979)'' dataset. In this case, the results obtained in the experimentation phase are compared to those presented by Kramer et al., and unlike Table 6, two additional columns are added: ''Avg. Energy (Kj)'' and ''Gap Avg. Energy (%)''. The first column shows the average energy obtained in all runs for each instance, whereas the second column shows a new Gap (33), which evaluates the average differences between each ACO algorithm and that of Kramer et al.

$$\ e a _ { \text{ns} } \quad \text{Gap Best Energy} = 1 0 0 \cdot \frac { S _ { I L S - S P - S O A _ { b e s t } } - S _ { b e s t } } { S _ { b e s t } }. \quad ( 3 2 )$$

$$\underset { \mathbf k } { \text{$m$} } ^ { \text{$\mathbf k$} } _ { \text{$\mathbf k$} } \quad \text{$\text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } } \quad \text{$\text{$\mathbf k$} } }
 \underset { \mathbf k } { \mathbf k } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$ \mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \\ \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf l } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \ \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf j } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{
\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \ } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad
 \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbfk } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{#\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \text{$\mathbf k } } \quad \$$

![Image](image_000015_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

TABLE 4. Results of Christofides, Mingozzi and Toth (1979).

Here, SILS -SP -SOAbest corresponds to the best solution presented by Kramer et al. for each instance, and SILS -SP -SOAavg is the average of Kramer et al.'s solutions for each instance.

During the experimentation phase, we decided to test the effectiveness of using clusters with the ant colony optimization metaheuristic. As a result, we conducted tests with Free Ant, both with and without the use of K-Means or K-Medoids. We observed that when the algorithm was assisted by clusters, it tended to converge towards a set of very similar local optima. This behavior did not manifest in the same way when using Free Ant without clustering; in this case, the convergence occurred in a much more staggered manner throughout the iterations. This discrepancy is attributed to the bias introduced by the information of cluster arcs in the pheromone matrix. The convergence graph without the use of clustering can be seen in Fig. 2, while

![Image](image_000016_8575d54b19367f4deb3aff29bfbc69af102dc53b2195dda3f0b77289328b239a.png)

## TABLE 5. Results of Golden et al. (1998).

TABLE 5. (Continued.) Results of Golden et al. (1998).

![Image](image_000017_6e4fbc6100b0dfbb4512b1c378d6254574fcd4429934e1c1d2f03653479a9696.png)

FIGURE 2. Convergence chart of the free ant algorithm without the use of a clustering algorithm in the CMT2 instance.

![Image](image_000018_c5c2baaf87774b606e73d4df702d78415985938be44bb19635cba5508d9aa43c.png)

![Image](image_000019_2a70cc1ccd885cce109ab5af34d7951080391721b8522182420cb1e6d9543ef2.png)

the convergence graph with the use of K-Means is depicted in Fig. 3.

Now, according to Table 4, we can see that once again, the Free Ant algorithms achieve better solutions in terms of energy consumption than the Restricted Ant algorithms. However, it is also evident that the Restricted Ant has significantly shorter execution times than the Free Ant. This suggests that allowing ants to explore the search space more freely and using clustering techniques such as K-Means and K-Medoids to guide pheromone formation produces superior results. However, the idea that restricting ant exploration helps reduce the time required to find solutions is also confirmed. It is important to note that this restriction limits the ability of the algorithm to find optimal solutions.

Table 7 indicate that the Free Ant algorithms achieved better results in 1 out of the 14 instances of Christofides, Mingozzi and Toth (1979), specifically CMT13 using Free Ant + K-Means. However, an average Gap of 10 33% above . the results reported by Kramer et al. was observed in the ''Gap Best Energy (%)'' column for the Free Ant algorithms, and a

FIGURE 3. Convergence chart of the free ant algorithm with the use of K-Means in the CMT2 instance.

Gap of 30 20% for the Restricted Ant in the same column. . In the ''Gap Avg. Energy (%)'' column, Free Ant had an average gap of 12 90% above ILS-SP-SOA, and 29 88% for . . Restricted Ant.

This time, there is an increase in the average Gap value for both Free Ant and Restricted Ant. This increase in the average Gapis approximately double that of the Set A (Augerat, 1995) dataset.

## C. GOLDEN ET AL. (1998)

Table 5 presents the results obtained in Golden et al. (1998). In general, the Free Ant algorithms achieved the best results in terms of the energy consumption. The K-Means and K-Medoids algorithms showed similar results in terms of the number of optimal solutions found. On this occasion, the Restricted Ant algorithms proved to be much more efficient and faster in completing the executions, suggesting that for medium to large-sized problems, this type of restricted algorithm is a better choice than Free Ant when the runtime is crucial.

![Image](image_000020_8575d54b19367f4deb3aff29bfbc69af102dc53b2195dda3f0b77289328b239a.png)

TABLE 6. Comparison results of Set A (Augerat, 1995).

![Image](image_000021_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

TABLE 6. (Continued.) Comparison results of Set A (Augerat, 1995).

Furthermore, the results presented in Table 5 were compared once again with those obtained by Kramer et al. A comparison with the results of Kramer et al. is presented in Table 8. The Free Ant algorithms achieved better results in one out of the 20 instances of Golden et al. (1998), specifically in Golden\_17 using the Free Ant + KMeans algorithm. On the other hand, an average Gap of

24 28% above the results . reported by Kramer et al. was observed for the Free Ant algorithms in the ''Gap Best Energy (%)'' column, and a 32 87% Gap for the Restricted . Ant algorithms in the same column. Regarding the ''Gap Avg. Energy (%)'' column, Free Ant had an average of 27 45% above ILS-SP-SOA, and a 35 00% difference for the . . Restricted Ant.

![Image](image_000022_8575d54b19367f4deb3aff29bfbc69af102dc53b2195dda3f0b77289328b239a.png)

TABLE 7. Comparison results of Christofides, Mingozzi and Toth (1979).

![Image](image_000023_b1198c1b9a06d95d0b87913b939a474d54e78bed644774b5188bff6cc2996e08.png)

## TABLE 8. Comparison results of Golden et al. (1998).

![Image](image_000024_3041fbd45ef3625371e1409d9b01a651ca4774bd544ec23c688389fde603fa86.png)

TABLE 8. (Continued.) Comparison results of Golden et al. (1998).

Again, we observed an increase in the average Gap value for both algorithms. However, it is important to note that the increase in Restricted Ant is considerably smaller than the increase experienced by Free Ant in relation to the previous set of instances. This difference suggests that the Restricted Ant can operate more efficiently with a smaller number of iterations, while Free Ant requires a higher number of iterations as the problem size increases.

Finally, we encountered a limitation regarding the scalability of Free Ant in comparison to Restricted Ant. In the case of the former, both the Gap and the total algorithm execution time, begin to increase as we attempt to solve problems with a greater number of inputs (i.e., a higher number of nodes). These issues are not reflected in the same manner in the Restricted Ant algorithms, which prove to be more efficient with larger-scale instances.

The results obtained in an extensive experimentation phase indicate that the algorithms proposed in this work are capable of consistently generating high-quality solutions. In particular, the results show that the Free Ant algorithm outperformed its counterpart, Restricted Ant, in all scenarios, while the latter demonstrated greater robustness and balance in medium and large-scale instances, primarily due to its superior runtime efficiency. This may be attributed to the advantages of the clustering algorithms used, which allowed Restricted Ant to find higher-quality routes in a much more confined search space. It is important to note that the performance of the algorithms should be tested on a larger number of instances and iterations to draw more conclusive conclusions.

Another limitation of the proposed algorithms (Free Ant and Restricted Ant) is that, in order to operate efficiently, a proper parameter configuration must be established. This entails a preliminary experimentation process to determine the configuration that yields the best results for a specific instance.

## VII. CONCLUSION

Combinatorial optimization is a complex and challenging field in which significant advances have been achieved through operations research, applied mathematics, and computer science research. In this work, two hybrid ant colony-based algorithms with clustering techniques, namely Free Ant and Restricted Ant, were presented for solving the Energy Minimizing Vehicle Routing Problem (EMVRP), a ''green''-oriented variant of the Vehicle Routing Problem (VRP). These algorithms has applications in green logistics and distribution operations.

While the results are promising, it is important to highlight that there is still room for improvement and further research is needed to achieve more efficient solutions. Green VRP problems are still relatively unexplored, leaving an ample scope for further investigations.

Nonetheless, both algorithms are innovative and utilize techniques that have not been extensively explored together in the resolution of combinatorial optimization problems. The proposed algorithms are novel in how they use clustering techniques to guide the search of the ACO metaheuristic. One of the novelties is the combined use of two clustering algorithms (K-Means with capacity-constrained and K-Medoids with capacity-constrained) with the ant colony system. Additionally, the Free Ant algorithm is a completely new proposal to the best of the authors' knowledge. Future work includes considering the reuse of clustering algorithms in later stages after initialization, using active re-clustering techniques to modify the ants' search space. It is believed that this new approach will help the Restricted Ant algorithm improve its results, as it tends to

converge to worse solutions than Free Ant due to the cluster shapes.

Furthermore, it would be interesting to investigate the combined use of these algorithms with other machine learning techniques to improve the self-regulation of the ACO system's hyperparameters, which are highly variabledependent. Additionally, the implementation of a parallelization system for deploying ants in the iterations of Free Ant and Restricted Ant should be considered.

One of the main challenges researchers in this field face is the lack of documented instances for benchmarking. Currently, there are only a limited number of standardized and widely used instance sets, such as those available in CVRPLIB. However, these solutions do not always expose the optimal solution for the EMVRP model, where the goal is to minimize the total energy consumed. Therefore, more research and collaboration are needed to develop additional instances and optimal solutions for such problems.

In summary, the proposed algorithms were able to solve EMVRPproblems and achieved good results in the instances from CVRPLIB where they were tested. However, although the algorithms did not reach the best results in all instances, this work lays the foundation for future research and improvements in the use of these metaheuristic techniques. The proposed algorithms are innovative in the efficient integration of clustering techniques and the ACO metaheuristic.

## REFERENCES

- [1] I. Kara, B. Y. Kara, and M. K. Yetis, ''Energy minimizing vehicle routing problem,'' in Combinatorial Optimization and Applications (Lecture Notes in Computer Science), vol. 4616, A. Dress, Y. Xu, and B. Zhu, Eds. Berlin, Germany: Springer, 2007, doi: 10.1007/978-3-540-73556-4\_9.
- [2] O. Cordon, I. F. D. Viana, F. Herrera, and L. Moreno, ''A new ACO model integrating evolutionary computation concepts: The best-worst ant system,'' in Proc. Ants , 2000, pp. 22-29.
- [3] S. Geetha, G. Poonthalir, and P. T. Vanathi, ''Improved k-means algorithm for capacitated clustering problem,'' INFOCOMP J. Comput. Sci. , vol. 8, no. 4, pp. 52-59, 2009. [Online]. Available: https://infocomp.dcc.ufla.br/index.php/infocomp/article/view/282
- [4] M. Dorigo, V. Maniezzo, and A. Colorni, ''Ant system: Optimization by a colony of cooperating agents,'' IEEE Trans. Syst., Man, Cybern. B, Cybern. , vol. 26, no. 1, pp. 29-41, Feb. 1996, doi: 10.1109/3477.484436.
- [5] M. Dorigo and L. M. Gambardella, ''Ant colony system: A cooperative learning approach to the traveling salesman problem,'' IEEE Trans. Evol. Comput. , vol. 1, no. 1, pp. 53-66, Apr. 1997, doi: 10.1109/4235.585892.
- [6] B. Bullnheimer, R. Hartl, and C. Strauß, ''Working papers SFB adaptive information systems and modelling in economics and management science,'' WU Vienna Univ. Economics Bus., Working Paper, Apr. 1997
- [7] T. Stützle and H. Hoos, ''Improvements on the ant-system: Introducing the MAX-MIN ant system,'' in Artificial Neural Nets and Genetic Algorithms . Vienna, Austria: Springer, 1998, doi: 10.1007/978-3-7091-6492-1\_54.
- [8] F.-Y. Wang and D. Liu, Advances in Computational Intelligence . World Scientific, 2006. [Online]. Available: https://www.worldscientific. com/doi/abs/10.1142/6072
- [9] S. Lloyd, ''Least squares quantization in PCM,'' IEEE Trans. Inf. Theory , vol. IT-28, no. 2, pp. 129-137, Mar. 1982, doi: 10.1109/TIT.1982.1056489.
- [10] J. B. MacQueen, ''Some methods for classification and analysis of multivariate observations,'' in Proc. 5th Berkeley Symp. Math. Statist. Probab., Statist. , vol. 1. Berkeley, CA, USA: University of California Press, Berkeley, 1967, pp. 281-297. [Online]. Available: http://projecteuclid.org/euclid.bsmsp/1200512992
- [11] L. Kaufman and P. J. Rousseeuw, ''Partitioning around medoids (program PAM),'' in Finding Groups in Data , L. Kaufman and P. J. Rousseeuw, Eds. Wiley, 1990, ch. 2, pp. 68-125. [Online]. Available: https://onlinelibrary.wiley.com/doi/pdf/10.1002/9780470316801.ch2, doi: 10.1002/9780470316801.ch2.
- [12] P. Hansen and N. Mladenovic, ''Variable neighborhood search,'' in Search Methodologies , E. K. Burke and G. Kendall, Eds. Boston, MA, USA: Springer, 2005, doi: 10.1007/0-387-28356-0\_8.
- [13] G. Clarke and J. W. Wright, ''Scheduling of vehicles from a central depot to a number of delivery points,'' Oper. Res. , vol. 12, no. 4, pp. 568-581, Aug. 1964. [Online]. Available: http://www.jstor.org/stable/167703
- [14] D. Arthur and S. Vassilvitskii, ''K-means ++ : The advantages of careful seeding,'' in Proc. 18th Annu. ACM-SIAM Symp. Discrete Algorithms (SODA) , vol. 8, Philadelphia, PA, USA. New Orleans, LA, USA: Society for Industrial and Applied Mathematics, 2007, pp. 1027-1035. [Online]. Available: https://www.bibsonomy.org/bibtex/2553bbfa74b13c 47b4e9c7c0034a8406e/sdo, doi: 10.1145/1283383.1283494.
- [15] B. Bullnheimer, R. F. Hartl, and C. Strauss, ''Applying the ANT system to the vehicle routing problem,'' in Meta-Heuristics , S. Voß, S. Martello, I. H. Osman, and C. Roucairol, Eds. Boston, MA, USA: Springer, 1999 doi: 10.1007/978-1-4615-5775-3\_20.
- [16] A. E. Rizzoli, R. Montemanni, E. Lucibello, and L. M. Gambardella, ''Ant colony optimization for real-world vehicle routing problems,'' Swarm Intell. , vol. 1, pp. 135-151, Sep. 2007, doi: 10.1007/s11721-007-0005-x.
- [17] S. S. El-Khatib, Y. A. Skobtsov, and S. I. Rodzin, ''Theoretical and experimental evaluation of hybrid ACO-k-means image segmentation algorithm for MRI images using drift-analysis,'' Proc. Comput. Sci. , vol. 150, pp. 324-332, Jan. 2019, doi: 10.1016/j.procs.2019.02.059.
- [18] N. Kusumahardhini, G. F. Hertono, and B. D. Handari, ''Implementation of K-means and crossover ant colony optimization algorithm on multiple traveling salesman problem,'' J. Phys., Conf. Ser. , vol. 1442, no. 1, Jan. 2020, Art. no. 012035, doi: 10.1088/1742-6596/1442/1/012035.
- [19] G. W. H. H. P. Rathnakumara and T. D. Rupasinghe, ''Clustering-based augmented Tabu search for energy minimizing vehicle routing problem (EMVRP),'' in Proc. 12th Annu. Sessions . Nugegoda, Sri Lanka: Sri Lanka Association for Artificial Intelligence, 2017, p. 77.
- [20] R. Kramer, A. Subramanian, T. Vidal, and L. D. A. F. Cabral, ''A matheuristic approach for the pollution-routing problem,'' Eur. J. Oper. Res. , vol. 243, no. 2, pp. 523-539, Jun. 2015, doi: 10.1016/j.ejor.2014.12.009.

![Image](image_000025_3041fbd45ef3625371e1409d9b01a651ca4774bd544ec23c688389fde603fa86.png)

![Image](image_000026_9a9446515c3228556201ae8934520ea5ee0a2c99cf740667874849c1035d9c6f.png)

NICOLÁS FRÍAS received the Graduate degree from Universidad de Playa Ancha, Valparaíso, Chile. He is currently a computer engineer in Chile. With a specialization in information management, he has built a successful career as a software developer. He stands out for his ability to merge theoretical concepts with practical applications.

![Image](image_000027_58ce1787d4bab80c3cf1fd4d91e3777ae7bd9104e3678a2f26c2ce87bd9713ec.png)

FRANKLIN JOHNSON received the B.E. degree, the master's degree in informatics, and the Ph.D. degree from Pontificia Universidad Católica de Valparaíso, in 2006, 2010, and 2017, respectively. He is currently the Head of the Department of Computer Sciences, Universidad de Playa Ancha, Valparaíso, Chile. His research interests include metaheuristics, autonomous search, and combinatorial optimization. His research interests include metaheuristics, autonomous search, and combinatorial optimization.

![Image](image_000028_8046165276b65e92acdfadbcc701775ec9a6a3d26f05d0a189b9dd85205801a2.png)

CARLOS VALLE received the Ph.D. degree in computer science from Universidad Técnica Federico Santa María, Chile, in 2014. He is currently with the Department of Computer Science and Informatics, Universidad de Playa Ancha, Valparaíso, Chile. From 2014 to 2018, he was a Researcher with the Computer Science Department, Universidad Técnica Federico Santa María. Since 2018, he has been an Associate Professor with the Department of Computer Science and

Informatics, Universidad de Playa Ancha, Chile. His research interests include machine learning, ensemble learning, deep neural networks, and pattern recognition with applications to time series.