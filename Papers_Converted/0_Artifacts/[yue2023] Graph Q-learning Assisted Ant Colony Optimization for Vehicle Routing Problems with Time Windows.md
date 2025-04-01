![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[yue2023] Graph Q-learning Assisted Ant Colony Optimization for Vehicle Routing Problems with Time Windows_artifacts/image_000000_22550b9d19c8cdc3188a700f0badb5edf8d2efd8e6640eef1f2d18a591ac6b28.png)

## Graph Q-learning Assisted Ant Colony Optimization for Vehicle Routing Problems with Time Windows

Peng Yue ∗ Northeastern University Liaoning, China 2010287@stu.neu.edu.cn

Shiqing Liu ∗ Bielefeld University Bielefeld, Germany shiqing.liu@uni-bielefeld.de

Yaochu Jin Bielefeld University Bielefeld, Germany yaochu.jin@uni-bielefeld.de

## ABSTRACT

Vehicle routing problem with time windows (VRPTW) is a typical class of constrained path planning problems in the field of combinatorial optimization. VRPTW considers a delivery task for a given set of customers with time windows, and the target is to find optimal routes for a group of vehicles that can minimize the total transportation cost. The traditional heuristics suffer from several limitations when solving VRPTW, such as poor scalability, sensitivity to hyperparameters and difficulty in handling complex constraints. Recent advance in machine learning makes it possible to enhance heuristic approaches via learned knowledge. In this paper, we propose a graph Q-learning assisted ant colony optimization algorithm named GQL-ACO to solve VRPTW. Compared to vanilla ant colony optimization (ACO), our proposed method first employs the learned heuristic values by using graph Q learning, instead of handcrafted ones, to define the hyperparameters of ACO. Second, we design a collaborative search strategy by combining ACO and Q-learning effectively, which can adaptively adjust the hyperparameters of ACO based on the search experiences.

## CCS CONCEPTS

- · Mathematics of computing → Combinatorial optimization ; Graph theory ; · Computing methodologies → Bio-inspired approaches Machine learning ; .

## KEYWORDS

Ant Colony Optimization, Deep Q-Learning, Graph Neural Network, Neural Combinatorial Optimization

## ACMReference Format:

Peng Yue, Shiqing Liu, and Yaochu Jin. 2023. Graph Q-learning Assisted Ant Colony Optimization for Vehicle Routing Problems with Time Windows. In Genetic and Evolutionary Computation Conference Companion (GECCO '23 Companion), July 15-19, 2023, Lisbon, Portugal. ACM, New York, NY, USA, 2 pages. https://doi.org/10.1145/3583133.3596423

## 1 INTRODUCTION

Vehicle routing problem with time windows (VRPTW) is a wellstudied combinatorial optimization problem that involves finding

∗ Both authors contributed equally to this research.

Permission to make digital or hard copies of part or all of this work for personal or classroom use is granted without fee provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the full citation on the first page. Copyrights for third-party components of this work must be honored. For all other uses, contact the owner/author(s).

GECCO '23 Companion, July 15-19, 2023, Lisbon, Portugal

© 2023 Copyright held by the owner/author(s).

ACM ISBN 979-8-4007-0120-7/23/07.

https://doi.org/10.1145/3583133.3596423

optimal routes for vehicles to service a set of customers within specified time windows [1, 9]. The introduction of time windows makes the problem more challenging, as an additional temporal dimension has to be considered in routing decisions [2].

Many heuristic approaches have been proposed in the field of combinatorial optimization, among which ant colony optimization (ACO) is a promising solution to VRPTW [7, 8]. It works by simulating the pheromone trails and the following behavior of ants, where a set of agents iteratively construct solutions to a given optimization problem [3, 4]. However, there are still some limitations. A set of heuristic values are often predefined in order to reduce the search space, while the human-designed heuristic is hard to maintain its effectiveness in the face of various problem instances. In addition, the traditional ACO tends to use fixed heuristic values during the search process, which results in the performance of the ACO being heavily influenced by the initially designed heuristic values.

Inspired by the success of machine learning techniques for combinatorial optimization, especially in graph neural networks and reinforcement learning [5, 6, 10]. This paper proposes a graph Qlearning assisted ant colony optimization algorithm (GQL-ACO) for solving VRPTW. Specifically, a pre-trained graph-based model will first provide the heuristic value for ACO instead of the handcrafted one. Then we construct an efficient collaborative search strategy by combining ACO with the Q-learning algorithm, which enables the adaptive adjustment of hyperparameters in ACO during the search process.

## 2 THE PROPOSED ALGORITHM

In this section, we will illustrate the specific implementation of our proposed method, and the overall framework is presented in Algorithm 1. Specifically, there are mainly two stages in the proposed GQL-ACO. First, we train a GNN model to provide initial heuristic values for the ACO algorithm. Second, we implement the collaborative search by combining ACO and Q learning effectively. The specific process is illustrated as follows.

## 2.1 Heuristic Generation via the Pre-trained GNN Model

At the beginning of the algorithm, a set of artificial ants mainly rely on the heuristic information to guide them towards promising areas of the search space. Therefore, how to generate good heuristic values is pretty important to improve the search efficiency of the algorithm. However, the existing works mainly design these values based on their own experience, which is hard to maintain good performance in the face of various problem instances. Here, we attempt to build a pre-trained GNN model by using Q learning, which

can provide the heuristic value automatically. Specifically, we first build an MDP process based on ACO's iterative process, including state, action, and reward. To capture good state representation, we model each VRPTW instance as an undirected graph, and define the raw feature of each node, including node locations, time windows, and whether it has been selected. Based on this, a GNN model can be used to extract state representation. Finally, we employ the Q learning algorithm to train this model, thus predicting the Q value for each route. Considering the meaning of Q values is consistent with the heuristic values in ACO, we, therefore, used the predicted Q value to estimate the heuristic values, thus replacing the manual design.

## Algorithm 1 Graph Q-learning assisted ant colony optimization

Require: 𝐺 𝑉,𝐸 ( ) : 𝑉 is the set of vertex, and 𝐸 is the set of edges. 𝑐 𝑖 𝑗 : distance between city 𝑖 and city 𝑗 . Iterative time 𝑁

Ensure: path: the shortest path

- 1: % Set initial heuristic values
- 2: if the pre-trained GNN model exists then
- 3: Build an MDP process based on ACO's iterative process
- 4: Employ the GNN model to extract state representation
- 5: Train this model by using the Q learning algorithm
- 6: end if
- 7: % Collaborative search
- 8: Set the initial heuristic value based on the learned Q values
- 9: for 𝑖 = 1 , · · · , 𝑁 do
- 10: Build solutions by choosing cities one by one based on equation (1)
- 11: Update the pheromones based on the global updating rule in ACO.
- 12: Update the heuristic values according to equation (2)
- 13: end for

## 2.2 Collaborative Search with ACO and Q-learning

In the original ACO, a set of fixed heuristic values are predefined and adopted for the route selection during the search process. It results in a high dependence of the algorithm on predefined heuristic rules, for example, short distances are preferred in VRP. However, this is not always the case when the problem comes with complex constraints. To avoid this limitation, we design a collaborative search strategy by combining ACO and Q-learning effectively. In GQL-ACO, artificial ants sequentially construct their routes based on a probabilistic rule in Equation 1 which considers both the pheromone level and the learned heuristic information.

$$p _ { s, t } = \begin{cases} \frac { \tau _ { s, t } ^ { \alpha } \cdot \eta ( \theta ) _ { s, t } ^ { \beta } } { \sum _ { u \in J _ { s } } \tau _ { s, t } ^ { \alpha } \cdot \eta ( \theta ) _ { s, t } ^ { \beta } }, & \text{if $t \in J_{s}$} \\ 0, & \text{otherwise} \end{cases} & [ 9 ]$$



where an ant positioned on source 𝑠 determines the target 𝑡 of next step among the set of unvisited nodes 𝐽 𝑠 following the probability 𝑝 𝑠,𝑡 . In Equation (1), 𝛼 and 𝛽 are weight factors, and 𝜏 represents the pheromone level which is updated via the global updating rule in ACO. 𝜂 𝜃 ( ) represents the learned heuristic value, which is updated during each step of the route construction via graph Q-learning

$$\mathfrak { e } _ { \mathfrak { e } } \quad \theta ^ { \prime } \leftarrow \theta - \alpha \left ( R + \gamma \underset { a \in \mathcal { A } ( s ) } { \arg \max } Q _ { \theta } ( s ^ { \prime }, a ) - Q ( s, a ) \right ) \cdot \nabla _ { \theta } Q ( s, a ) \quad ( 2 )$$

where 𝑄 𝑡ℎ𝑒𝑡𝑎 ( 𝑠, 𝑎 ) is provided by the pre-trained GNN model parameterized by 𝜃 , 𝛾 and 𝛼 are a discount factor and a learning rate, and 𝑅 indicates the reward, equal to reciprocal of the route distance. Note that we only use existing solutions found by ants to implement this updating process, thus achieving fuller exploitation of routing information. In this way, the heuristic values of ACO will be updated adaptively to cater to the feature of current instances.

## 3 CONCLUSION

This paper proposes the graph Q-learning assisted ant colony optimization with great potential in solving VRPTW. By incorporating the machine learning approach of graph Q-learning, the proposed algorithm can learn heuristic knowledge from its past experiences and adapt to new problem instances more efficiently. The ACO provides an effective way to construct solutions and update pheromones based on the quality of the solutions found, while the graph Q-learning model learns heuristic values to update the routing construction rule. The synergy between these two techniques enables the algorithm to converge to high-quality solutions with less computational effort. The proposed DQL-ACO enhances the data efficiency in heuristics and also provides a promising direction for further research in solving VRPTW and other combinatorial optimization problems.

## REFERENCES

- [1] Kris Braekers, Katrien Ramaekers, and Inneke Van Nieuwenhuyse. 2016. The vehicle routing problem: State of the art classification and review. Computers &amp; industrial engineering 99 (2016), 300-313.
- [2] Martin Desrochers, Jacques Desrosiers, and Marius Solomon. 1992. A new optimization algorithm for the vehicle routing problem with time windows. Operations research 40, 2 (1992), 342-354.
- [3] Marco Dorigo and Luca Maria Gambardella. 1997. Ant colony system: a cooperative learning approach to the traveling salesman problem. IEEE Transactions on Evolutionary Computation 1, 1 (1997), 53-66.
- [4] Marco Dorigo, Vittorio Maniezzo, and Alberto Colorni. 1996. Ant system: optimization by a colony of cooperating agents. IEEE Transactions on Systems, Man, and Cybernetics, Part B (Cybernetics) 26, 1 (1996), 29-41.
- [5] Shiqing Liu, Xueming Yan, and Yaochu Jin. 2023. End-to-End Pareto Set Prediction with Graph Neural Networks for Multi-objective Facility Location. In Evolutionary Multi-Criterion Optimization: 12th International Conference, EMO 2023, Leiden, The Netherlands, March 20-24, 2023, Proceedings . Springer, 147-161.
- [6] Nina Mazyavkina, Sergey Sviridov, Sergei Ivanov, and Evgeny Burnaev. 2021. Reinforcement learning for combinatorial optimization: A survey. Computers &amp; Operations Research 134 (2021), 105400.
- [7] Ziheng Rong, Xiaoling Gong, Xiangyu Wang, Wei Lv, and Jian Wang. 2021. A slime mold fractional-order ant colony optimization algorithm for travelling salesman problems. In Advances in Swarm Intelligence: 12th International Conference, ICSI 2021, Qingdao, China, July 17-21, 2021, Proceedings, Part I 12 . Springer, 322-332.
- [8] Marius M Solomon. 1987. Algorithms for the vehicle routing and scheduling problems with time window constraints. Operations Research 35, 2 (1987), 254265.
- [9] Kay Chen Tan, Loo Hay Lee, QL Zhu, and Ke Ou. 2001. Heuristic methods for vehicle routing problem with time windows. Artificial intelligence in Engineering 15, 3 (2001), 281-295.
- [10] Zonghan Wu, Shirui Pan, Fengwen Chen, Guodong Long, Chengqi Zhang, and S Yu Philip. 2020. A comprehensive survey on graph neural networks. IEEE Transactions on Neural Networks and Learning Systems 32, 1 (2020), 4-24.