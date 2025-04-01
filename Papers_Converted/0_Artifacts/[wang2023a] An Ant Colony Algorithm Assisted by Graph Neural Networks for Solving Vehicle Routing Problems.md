![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[wang2023a] An Ant Colony Algorithm Assisted by Graph Neural Networks for Solving Vehicle Routing Problems_artifacts/image_000000_22550b9d19c8cdc3188a700f0badb5edf8d2efd8e6640eef1f2d18a591ac6b28.png)

## An Ant Colony Algorithm Assisted by Graph Neural Networks for Solving Vehicle Routing Problems

Xiangyu Wang Bielefeld University Bielefeld, Germany xiangyu.wang@uni-bielefeld.de

Yaochu Jin Bielefeld University Bielefeld, Germany yaochu.jin@uni-bielefeld.de

## ABSTRACT

Vehicle routing problems have attracted increasing attention because of the rapid development of transportation. Companies want to reduce the cost by lowering the number of vehicles and the total distances, which can be considered as a combinatorial optimization problem. The ant colony algorithm shows great potential in solving vehicle routing problems. However, it suffers from a low convergence speed due to the randomly initialized pheromone, which may cause a waste of computational resources in the early search process. To address this problem, a graph neural network is pre-trained to provide prior knowledge to initialize the pheromone in the ant colony algorithm, which can boost the convergence process. In addition, some classic local research methods are applied to balance the exploration and exploitation of the evolutionary process.

## CCS CONCEPTS

· Computing methodologies → Neural networks ; Bio-inspired approaches .

## KEYWORDS

pre-trained neural networks, graph neural networks, ant colony optimization, combinatorial optimization problems

## ACMReference Format:

Xiangyu Wang and Yaochu Jin. 2023. An Ant Colony Algorithm Assisted by Graph Neural Networks for Solving Vehicle Routing Problems. In Genetic and Evolutionary Computation Conference Companion (GECCO '23 Companion), July 15-19, 2023, Lisbon, Portugal. ACM, New York, NY, USA, 2 pages. https://doi.org/10.1145/3583133.3596424

## 1 INTRODUCTION

Since vehicle routing problems (VRPs) were proposed in 1959, this kind of route planning problem has attracted increasing attention. With the development of transportation and economics, VRPs show their value in the application because of the increasing demand for delivery. Based on the conventional and classic VRPs, there are many different variants that have more practical meanings, and VRPs with a time window (VRPTW) are among these. VRPTW requires vehicles to deliver cargo to customers within a certain

Permission to make digital or hard copies of part or all of this work for personal or classroom use is granted without fee provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the full citation on the first page. Copyrights for third-party components of this work must be honored. For all other uses, contact the owner/author(s).

GECCO '23 Companion, July 15-19, 2023, Lisbon, Portugal

© 2023 Copyright held by the owner/author(s).

ACM ISBN 979-8-4007-0120-7/ 23/07.

https://doi.org/10.1145/3583133.3596424

time period, which makes it even harder to solve than the classic VRPs[1].

The best solutions of VRPs can be obtained by exhaustive methods or other exact methods, as such a combinatorial optimization problem has limited combinations. However, the number of solutions of VRPs increases exponentially with the number of variables growth, which makes it an NP-hard problem [2]. In terms of the above-mentioned methods, it is almost impossible to find the optimal solution within an acceptable time if the number of decision variables is large. However, heuristic algorithms, such as evolutionary algorithms, show their great potential in getting an approximate optimal solution (small gap from the optimal solution) using a relatively short time. Among these evolutionary algorithms, ant colony optimization (ACO) [3] is naturally suitable for solving path planning problems since it is inspired by the behavior of ants foraging for food, and ants prefer the shortest route to save energy.

Each ant in ACO releases a pheromone to the route it passes by. Pheromone accumulates if more ants choose the route, and it evaporates with time passing. The shorter route has a higher concentration of pheromone, which attracts ants more. With more ants choosing the shorter route, the concentration of pheromones accumulates. Therefore, the shortest route could be found in the end. The pheromone in ACO is the key information helping ants to find short routes, and there are a number of works focusing on modifying the update method of the pheromone. Thanks to its effectiveness of route planning ability, ACO is applied to solve vehicle routing problems [4] and traveling salesman problems [5]. However, as the pheromone is initialized randomly, it takes time to accumulate pheromones on several routes, which makes ACO suffer from low convergence speed.

Since VRPs can be described as a fully-connected graph, where the depot and all customers are connected, they can be put into the framework of graph neural networks (GNNs). As neural networkbased methods can be pre-trained and store knowledge of certain types of problems, they are able to solve problems without starting from the very beginning. Therefore, to deal with the low convergence speed of ACO, an associated and pre-trained graph neural network is proposed to initialize and adjust the pheromone, which can boost the convergence speed.

## 2 THE DEFINITION OF VEHICLE ROUTING PROBLEMS WITH TIME WINDOW

The vehicle routing problems with time window aim to minimize the following objective function,

$$F = 1 0 0 0 \times K + \sum _ { k \in K } \sum _ { i \in V } \sum _ { j \in V } X _ { i j } ^ { k } d _ { i j }, \text{ \quad \ \ } ( 1 )$$

where 𝐾 is the number of vehicles used in delivery, 𝑑 𝑖 𝑗 is the distance between node 𝑖 and 𝑗 , 𝑋 𝑘 𝑖 𝑗 is the binary variable, where 𝑋 𝑘 𝑖 𝑗 = 1 means that the 𝑘 -th vehicle delivers cargo from 𝑖 to 𝑗 . All the customers can only be delivered by one vehicle within the required time window. All vehicles should start from the depot and end at the depot, and the capacity of each vehicle is limited.

## 3 METHODOLOGY

To better accelerate the convergence speed of ACO in solving the VRPTW, a graph neural network is pre-trained by learning the knowledge of the optimal solutions of small-scale VRPTW. As we can use Gurobi to easily minimize the Equ. (1) and get the corresponding decision variable matrix X , the GNN can learn the pheromone distribution of routes and scale up to large-scale instances. After pretraining, the output of a new instance indicates which route should be chosen for shorter distances. Therefore, this information can be used to initialize the pheromone in ACO. After getting prior knowledge, ACO can converge faster. In addition, some local research strategies are also applied to avoid ACO getting into the local optima.

## 3.1 Pre-trained graph neural networks

To take advantage of supervised graph neural networks, VRPTW is first encoded into a graph, where the depot and customers are nodes. The location, time window, and demand of customers and depot can be encoded as the input of the graph neural network. This graph is a fully connected graph because a vehicle can deliver from one customer to any other one under some constraints. To train a graph neural network in a supervised way, the true label of each edge should be known. Therefore, some small-scale instances are generated in advance, Gurobi, i.e., the exact solver, are used to minimize the loss function 𝐹 , and the corresponding variable 𝑋 𝑘 𝑖 𝑗 is obtained.

The loss function of training the graph neural network can be considered as the link prediction target in GNN society [6], and it can be written as follows,

$$f _ { G N N } = \sum _ { i, j \in V } ( Y _ { i j } - \sum _ { k = 1 } ^ { K } X _ { i j } ^ { k } ) ^ { 2 }, \quad \quad ( 2 ) \quad \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \$$

where 𝑌 𝑖 𝑗 is the output of the graph neural network. As each customer can only be served by one vehicle, ˝ 𝐾 𝑘 = 1 𝑋 𝑘 𝑖 𝑗 = 1, if there is one vehicle delivers cargo from 𝑖 to 𝑗 ; ˝ 𝐾 𝑘 = 1 𝑋 𝑘 𝑖 𝑗 = 0, otherwise. Therefore, the ideal value of a real value output 𝑌 𝑖 𝑗 should be 0 or 1. To reduce coding difficulty and improve algorithm generalization ability, 𝑋 𝑘 𝑖 𝑗 is set to be 1, if 𝑋 𝑘 𝑖 𝑗 = 1.

Recently, there have been a number of graph neural networks with various structures proposed to solve different problems, such as node classification and link prediction. Among these, graph attention network (GAT) [7] shows its good ability in generalization, where a global vector a of each layer is learned and the specific 𝛼 𝑖 𝑗 can be generated by a and the node embedding of node 𝑖 and 𝑗 . Therefore, GAT is used in this paper to learn the pheromone distribution [8]. The main difference between our pre-trained method and other method based on link prediction is that we focus on learning the commonality through various graph instances rather than a single graph, which requires the scalability of a certain graph neural network.

## 3.2 ACO assisted by GNN solving VRPTW

After pre-training the graph neural network, it is ready to predict the distribution of pheromones of a given VRPTW, i.e., a fully connected graph. By adding the predicted pheromone into the randomly initialized pheromone, it can not only boost the convergence speed for not searching the whole searching space but also still give ants some chance to exploit.

The main process of ACO is to update the pheromone, accumulating and evaporating it on the route, which is conducted similarly with most ACOs. However, initializing the pheromone with prior knowledge might cause solutions to trapping into the local optimal. Therefore, some local research strategies, such as 2-opt, exchange, and relocate operations, are applied [9].

## 4 CONCLUSION

Ant colony optimization has shown its ability to solve route planning problems since it has been proposed last century. However, ACOsuffers from a low convergence speed. Therefore, a pre-trained graph neural network is proposed to accelerate the convergence by initializing the pheromone with prior knowledge. With this strategy, ACO may boost its convergence speed as the early search process of the large decision space is greatly shortened. On the other hand, some classical local search methods are also applied to avoid solutions being trapped in local optimal. With the help of the pre-trained neural network and local research methods, ACO can better solve the VRPTW by maintaining a good balance between exploration and exploitation.

## REFERENCES

- [1] Wenjie Yi, Rong Qu, Licheng Jiao, and Ben Niu. Automated design of metaheuristics using reinforcement learning within a novel general search framework. IEEE Transactions on Evolutionary Computation , 2022.
- [2] Gerhard J Woeginger. Exact algorithms for np-hard problems: A survey. In Combinatorial Optimization-Eureka, You Shrink! Papers Dedicated to Jack Edmonds 5th International Workshop Aussois, France, March 5-9, 2001 Revised Papers , pages 185-207. Springer, 2003.
- [3] Marco Dorigo and Luca Maria Gambardella. Ant colony system: a cooperative learning approach to the traveling salesman problem. IEEE Transactions on evolutionary computation , 1(1):53-66, 1997.
- [4] Huilin Chen, Xiaoquan Cai, Fangqing Gu, and Shengbing Xu. An improved aco algorithm to vehicle routing problem with time windows and uncertainty. In 2021 17th International Conference on Computational Intelligence and Security (CIS) , pages 50-54. IEEE, 2021.
- [5] Ziheng Rong, Xiaoling Gong, Xiangyu Wang, Wei Lv, and Jian Wang. A slime mold fractional-order ant colony optimization algorithm for travelling salesman problems. In Advances in Swarm Intelligence: 12th International Conference, ICSI 2021, Qingdao, China, July 17-21, 2021, Proceedings, Part I 12 , pages 322-332. Springer, 2021.
- [6] Muhan Zhang and Yixin Chen. Link prediction based on graph neural networks. Advances in neural information processing systems , 31, 2018.
- [7] Petar Veličković, Guillem Cucurull, Arantxa Casanova, Adriana Romero, Pietro Lio, and Yoshua Bengio. Graph attention networks. arXiv preprint arXiv:1710.10903 , 2017.
- [8] Weiwei Gu, Fei Gao, Xiaodan Lou, and Jiang Zhang. Link prediction via graph attention network. arXiv preprint arXiv:1910.04807 , 2019.
- [9] Mingde Liu, Qi Song, Qi Zhao, Ling Li, Zhiming Yang, and Yingbin Zhang. A hybrid bso-aco for dynamic vehicle routing problem on real-world road networks. IEEE Access , 10:118302-118312, 2022.