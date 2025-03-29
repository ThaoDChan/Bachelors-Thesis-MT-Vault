See discussions, stats, and author profiles for this publication at: https://www.researchgate.net/publication/357897375

## Adaptive Gradient Descent Enabled Ant Colony Optimization for Routing Problems

![Image](image_000000_8ae90dc85f02fadc57be5b333d169d032d8a750c89b43ecfe8eb9b3419917dcc.png)

## Adaptive Gradient Descent Enabled Ant Colony Optimization for Routing Problems glyph[star]

Yi Zhou , Weidong Li a b, ∗ , Xiaomao Wang , Yimin Qiu , Weiming Shen a a c

- a School of Information Science and Engineering, Wuhan University of Science and Technology, Wuhan 430081, China
- b School of Mechanical Engineering, University of Shanghai for Science and Technology, Shanghai 200093, China
- c School of Mechanical Science &amp; Engineering, Huazhong University of Science and Technology, Wuhan 430074, China

## Abstract

The design of Ant Colony Optimization (ACO) has been inspired by the foraging behavior of ant colonies. ACO is one of the most widely used metaheuristic algorithms applicable to various optimization problems. Nevertheless, the deficiency of ACO is early maturity and slow convergence, usually leading to dissatisfactory performance. In this research, a new type of ACO is proposed with innovative adaptive learning mechanism is devised (the algorithm is called ADACO). In the paper, first, the ACO algorithm for Traveling Salesman Problem (TSP) is modeled in the framework of reinforcement learning (RL) and learns a best policy by stochastic gradient descent (SGD). Then, an adaptive gradient descent strategy, which can exploit the update history of per-dimensional pheromones to achieve intelligent convergence, is integrated into the ACO algorithm as ADACO. A parallel computation process is implemented in the process to expedite computational efficiency. Finally, through case studies in TSP and Capacitated Vehicle Routing Problem (CVRP), ADACO is validated. ADACO is trialed on various sizes of TSP and CVRP instances and compared with other state-of-the-art algorithm-

∗

Corresponding author

Email address: weidongli@usst.edu.cn (Weidong Li)

s. Results show that ADACO is competitive in terms of accuracy (better in most cases), stability (statistically significant) and adaptability (support various types of problems). The results also elucidate that the algorithm maintains good computational efficiency owing to its parallel implementation. It can be concluded that ADACO is effectively applicable to routing problems with good performance.

Keywords: Ant colony optimization, stochastic gradient descent, adaptive learning, Traveling salesman problem.

## 1. Introduction

Ant Ant Colony Optimization (ACO) and its variant improved versions are eminent swarm intelligence algorithms [1, 2, 3] widely used to support various applications [4, 5, 6, 7, 8, 9, 10]. The ACO algorithm consists of two fundamental stages: solution construction and pheromone update [11]. That is, based on initially constructed solutions, all ants deposit and continuously update pheromones to achieve optimization.

Research has shown that ACO has achieved good results in typical combinatorial optimization problems (COPs), including Traveling Salesman Problems (TSP), Quadratic Assignment Problems (QAP), Job Shop Scheduling Problems (JSSP) and Vehicle Routing Problems (VRP). Thus, the algorithm has demonstrated good potential in various engineering applications, such as vehicle/mobile robotic routing, manufacturing system planning, scheduling and logistics, etc. A theoretical proof was provided to reveal that the convergence of ACO highly depends on the pheromone update stage [12]. To improve the algorithmic convergence, two classic variants of ACO, i.e., Ant Colony System (ACS) [13] and Max-min Ant System (MMAS) [14], were designed. In ACS, a random greedy strategy was proposed to select the next node with the highest value of heuristic information in a high probability. The function of the strategy is to accelerate the optimization efficiency through conducting intensive search during solution construction. To alleviate the issue of ACO in the aspects of slow convergence and potential trapping in local optima, two strategies were adopted to design MMAS: (i) in each computational iteration, only the best solution is used to update the pheromone trail, (ii) the pheromone value on each traveling path is bounded within a certain range to avoid early maturity.

Nevertheless, the aforementioned variant algorithms of ACO are still not

adaptive yet. For a specified problem, choosing appropriate parameters of the algorithm requires users knowledge and experience. To address this issue, new parameter optimization schemes for ACO were designed [15]. For instance, some schemes modified hyper-parameter settings on a per-instance basis based on statistics. Other schemes were to adjust parameters on the fly by employing strategies like a trade-off between exploration and exploitation of the solution space. For instance, Olivas et al. [16] developed interval type-2 fuzzy systems for parameter adaptation. The idea is to achieve adaptability by controlling the exploration and exploitation capabilities of ACO. Sandhya et al. [17] proposed a fuzzy-based parameter control mechanism for ACO to solve TSP and VRP. However, the improvements by the aforementioned themes are unstable, sometimes leading to even worse performances [18]. An idea is whether it is possible to explore parameter optimization schemes that have been effectively used in other intelligent algorithms to be integrated into ACO for more stable performance improvement.

Deep learning (DL) is a quickly developed branch of artificial intelligence [19]. In DL, an important research area is how to design a sensible parameter optimization approach to train DL algorithms. Gradient descent is one of the most common approaches to tackle this complex problem. The relevant implementation is contained in nearly every DL library (e.g., PyTorch [20], Caffe [21] and Keras [22]). Due to the similar nature in parameter optimization between DL and swarm intelligence algorithms (e.g., ACO), effective strategies from either side can be considered for borrowing to achieve mutual improvement. For instance, some works of swarm intelligence tried to utilize DL to learn heuristics for solving combinatorial optimization problems (COPs) automatically [23, 24, 25, 26]. Some works of DL improved gradient descent by taking optimization strategies from swarm intelligence algorithms [27, 28, 29, 30]. These useful explorations have motivated us to fuse DL into ACO somehow for its performance improvement. However, current DL approaches for tackling COPs mainly focus on the automatic learning of heuristics. The performance is less competitive and the scale of COPs to solve based on neural networks is limited. Therefore, in this research, an adaptive Stochastic Gradient Descent (SGD) approach from DL is integrated into ACO for solving hard COPs. To the best of our knowledge, it is still rare to use strategies of DL such as SGD for improving swarm intelligence algorithms in solving COPs.

With this aim, in this research, an innovative ACO algorithm based on adaptive gradient descent (ADACO) is developed. The major contribution

is as follows.

- · A learning-based ACO model is proposed upon the theory of reinforcement learning. In this model, the transition probability is defined as a policy, where the actions are the choices of the next node. The ants explore problem graphs to generate samples (routes), and then reward related to the routes is applied to the pheromone matrix. A new insight is provided that ACO is a learning-based algorithm with the objective to find the best police, which can be solved by SGD. This paves the way of integrating adaptive approach into learning-based ACO as ADACO.
- · Based on the theoretic model, ADACO is designed to adaptively learn to update the pheromone matrix by gradients. Two key advantages, i.e., asymptotic convergence in ACO and adaptive learning in SGD, are combined to achieve a better solution quality and stability. The algorithm is implemented with a parallel scheme to accelerate the computational efficiency. Based on that, the time-complexity of the algorithm is kept similarly as the classic ACO.

The ADACO algorithm was trialed on various types of datasets including symmetric TSPs, asymmetric TSPs and CVRPs, and compared with ACS, MMAS, Iterated Local Search (ILS) and other state-of-the-art algorithms. The results demonstrate that the improvement of ADACO is significant, and its performance is competitive. The validation of the algorithm on TSP and CVRP proves that the algorithm can be effectively applicable to various routing planning problems, such as vehicle routing, logistics, manufacturing planning and scheduling, etc.

The rest of this paper is organized as follows. Section 2 reviews related works. Section 3 gives a general background of the ACO algorithm and parameter optimization in DL. In Section 4, the ADACO algorithm is presented in detail. The experimental and benchmarking results are given in Section 5. Finally, conclusion is made in Section 6.

## 2. Related Works

Many previous related works have been contributed to the advancement of ACO. Recently, a systematic review of ACO [6] has covered studies including categories such as theories, algorithms, and applications, etc. Because this research concentrates on the algorithmic aspect, the following related works are reviewed here.

## 2.1. Improving ACO algorithms

The goal to improve ACOs is pursuing higher accuracy and efficiency. Typically, ACS and MMAS provide us with insights to overcome the drawbacks of AS, meanwhile not increase the computational complexity. They give directions of how to advance the ACO algorithm in the accuracy aspect.

Yang et al. [31] presented an improved ACO algorithm for solving mobile agent routing problem. A Genetic Algorithm was integrated into ACO to provide a better convergence performance and give a revised global pheromone update principle. The performance of the algorithm is better for escaping from the trap of a local minimum as well as faster convergence. Elloumi et al. [32] researched the performance of an algorithm hybridizing of ACO and PSO. The junction point is the pheromone deposit by ACO would be transferred to the particle weights of PSO, thus to make the global search more intelligent. Olivas et al. [16] designed a dynamic parameter approach for ACO based on interval type-2 fuzzy systems. The method utilizes fuzzy logic to manipulate the diversity of the solutions, thus to control the exploration and exploitation of ACO. Engin et al. [5] proposed an effective hybrid ACO algorithm based on crossover and mutation mechanism for no-wait flow shop scheduling with a criterion to minimize the maximum completion time. The crossover and mutation operators were investigated by introducing a Genetic Algorithm into the ACO algorithm. Chen et al. [33] introduced an entropy-based dynamic heterogeneous ACO. The research enhances the stability and precision of classic ACOs with dual heterogeneous colonies and communication by an allotropic mechanism. However, the above research works only tested small-scale problem instances, so their performance could not be guaranteed in large-scale ones. Besides, most works still lack of solid theoretical support for experiments. More recently, Wei Gao [34] modified ACO (MACO) to improve performance for solving TSP. The work proposed two strategies, path combining and pheromone density polarizing, applied on the tour construction and pheromone updating stages respectively. The experimental results illustrated that MACO achieved state-of-the-art performance. Wang et al. [35] hybridized symbiotic organisms search and ACO (SOS-ACO), to optimize a part of parameters adaptively. It was reported that SOS-ACO had good adaptivity and competitive solutions.

In the aspect of GPU-based approaches, parallelizing ACO is also an actively explored research topic especially. GPU-based AS [36, 37], ACS [38] and MMAS [36] were proved to make significant progresses in performance. Multi-SIMD CPU parallel was also feasible to accelerate ACO [39] large-

ly against sequential ACO. The ACO computational performance could be effectively enhanced by data-parallelism. However, the parallel approaches usually could not increase accuracy.

## 2.2. Models to Solve TSPs

Since ACO algorithms always take TSP as their benchmark dateset, solving TSP by modeling is another topic closely related to this research. The TSP problem also could be modelled by a probability model that is able to tackle by the SGD.

Cvetkovic et al. [40] treated TSP as a problem of discrete semidefinite programming. They applied semi-definite relaxation to TSP and then solved it by a branch and bound algorithm. Some preliminary test results of randomly generated problems were given. Focacci et al. [41] modelled an Integer Linear Programming with Constraint Programming (CP). The approach overcomes limitations in dealing with objective functions of CP. TSP and its variant problem were experimented and some satisfactory results were obtained. Xie et al. [42] proposed a multi-agent optimization system to perform cooperative search to solve TSP. Memory and rule modules were designed with considering existing knowledge components for TSP. Some positive results were obtained, and they were comparative with several famous TSP solvers. Kool et al. [23] integrated an attentional encoder-decoder model into a neural network to learn heuristics for solving COPs including TSP. It was found to be a feasible approach in achieving results close to traditional TSP solvers. It could save costly development of dedicated heuristics, but at a very high computational price. The TSP instances for testing were limited to 100 nodes. Shi et al. [43] studied a new method named homotopic convex transformation to smoothen the landscape of TSP. They combined the original TSP and a convex-hull TSP by a coefficient to generate a new smoother problem, which was easier to address than original TSP. Experimental results showed the algorithm outperformed iterated local search for most TSP cases tested.

The above works have a very strong theoretical background that could provide inspiration to future works. Nevertheless, the efficiency and adaptability is not considered enough what makes the approaches inapplicable in some complex scenarios. Besides, the works only experimented with smallscale simple cases and computational time was not considered enough.

## 3. Background

## 3.1. Problem Definition

TSP is one of the most popularly studied problems in theoretical computer science and operations research. It is a typical NP-hard combinatorial optimization problem. In this research, TSP is taken to evaluate the effectiveness of the developed ADACO algorithm.

glyph[negationslash]

The process of TSP is below. Given a complete undirected acyclic graph G = ( V, D ) with N nodes, where V = { v , v 1 2 , · · · , v N } is a set of nodes and D = [ d ij ] ∈ R N × N is the weight of an edge 〈 v , v i j 〉 . The solution of TSP is ordered combinations of all nodes S = { x , x 1 2 , · · · , x N } , x i = x , i j = j , in which x i ∈ { 1 2 , , · · · , N } are indexes of the nodes. The goal of TSP is to minimize the total weights of solutions as shown in Equation 1.

glyph[negationslash]

$$\min L \left ( s \right ) = d \left ( x _ { N }, x _ { 1 } \right ) + \sum _ { i = 1 } ^ { N - 1 } d \left ( x _ { i }, x _ { i + 1 } \right )$$

where L ( ) is the fitness function and · s ∈ S is a solution.

VRP is one of the most widely researched problem in the fields of operations research and combinatorial optimization, where Capacitated VRP (CVRP) [44] occupies a central position among the variants of VRPs. And it is also closely related to TSP which is suitable for case studies. Given a complete undirected acyclic graph G = ( V, D ) with N nodes (cities or customers) and K vehicles (all identical with a capacity C ), The definition of CVRP is as follows.

- · V = { v , v 0 1 , v 2 , · · · , v N -1 } is a set of nodes, where:
- -v 0 is a special node named a depot.
- -{ v i | i &gt; 0 } are nodes of cities.
- · D = [ d ij ] ∈ R N × N is the distance of an edge 〈 v , v i j 〉 .
- · q is a vector of requirements, where q i represents the demand of the customer i in the city v i .
- · R i is a route of vehicle i , where R i = { v , v 0 x 1 , v x 2 , · · · , v x l } .
- - Except for the depot, each node could exist only once in all routes.

- -The capacity constraint is that total demand of a route should satisfy Q R ( i ) ≤ C , where Q R ( i ) = l ∑ j =1 q x j .
- - The total distance of R i is calculated as shown in Equation 2.
- · A solution of CVRP is a set routes R = { R i i | ∈ [1 , K ] } of the K vehicles that travel each city only once. If the capacity constraint is satisfied, the solution is feasible, otherwise it is infeasible.
- · The objective of solving CVRP is to minimize the total distance L R ( ) = K ∑ k =1 L R ( i ).

$$L \left ( R _ { i } \right ) = d \left ( 0, x _ { 1 } \right ) + \sum _ { j = 1 } ^ { l - 1 } d \left ( x _ { j }, x _ { j + 1 } \right ) + d ( x _ { l }, 0 )$$

In Section 4, we will show the transferring a CVRP to a TSP, and how could ADACO adapt to solve the CVRP. Thus, our proposed approach can be benchmarked with CVRP datasets.

## 3.2. Ant colony optimization for the TSP

Ant System (AS), the first version of the ACO algorithm, was inspired by the foraging behavior of ants. A colony of ants work collaboratively to find the shortest path to the food source by a mechanism named stigmergy. In the process, the ants communicate through chemical substances emitted by themselves, namely pheromone trails. AS is designed to solve TSP with two major phases, i.e., tour construction and pheromone update. In the first phase, the ants move randomly on the graph to create solutions biased by pheromone trails. In the second phase, ants deposit pheromone on the path they travelled. Better routes are more likely to aggregate higher quantities of pheromone deposited by the ants. Thus, the routes with more pheromone left would guide other ants to travel through. This is also recognized as a positive feedback process. Besides, as time goes by, the pheromone on the path becomes weaker, like chemical matter evaporates in the air. This subprocedure of the pheromone update is named pheromone evaporation. The AS algorithm performs the two phases iteratively until stop criterion, i.e., convergence is attained.

In the tour construction phase, the ants construct solutions independently and stochastically on a step-wise basis according to a pheromone matrix θ . The transition probability of selecting a next node to travel is below:

$$P r \left ( x \left | \theta, s ^ { k } \right ) = \begin{cases} 0 & \text{if $x\in s^{k}$} \\ \frac { \theta _ { \left ( s _ { k }, x \right ) ^ { \eta } \left ( s _ { k }, x \right ) } } { \sum _ { y \not \in s ^ { k } } \theta _ { \left ( s _ { k }, y \right ) ^ { \eta } \left ( s _ { k }, y \right ) } } & \text{otherwise,} \end{cases}$$

where θ = [ θ ij ] ∈ R N × N presents pheromone content on each edge shared among the ants, η = [ η ij ] ∈ R N × N denotes the heuristic value matrix, s k = 〈 s , s 1 2 , · · · , s k 〉 is a temporary sequence of travelled nodes; k is the count for constructing steps; s 0 = {∅} , and s N ∈ S ; an edge 〈 v , v i j 〉 ∈ s k is true, if and only if s i = v i &amp; s i +1 = v j exists in s k .

After the ants reach the end of their tours, the lengths (or fitness values) of the routes (or solutions) are evaluated. In the end of tour construction, the best ant (iteration-best-ant) is selected to compare with the global best one (global-best-ant), thus to determine whether replace it or not.

In the pheromone update phase, the matrix θ is managed for receiving feedback from the ants. One or more solutions are selected to perform deposit of pheromone on paths. In AS, all solutions are employed for pheromone deposit, while in ACS and MMAS only the iteration-best-ant or the global-bestant are chosen. Each edge is related to two conditions, i.e., evaporation-only and evaporation-with-update. The edges that are not included in the selected solutions will perform an evaporation operation only (evaporation-only). Otherwise, an additional positive value will be added to the edge followed with an evaporation operation (evaporation-with-update). The equation of pheromone update for an edge 〈 v , v i j 〉 according to a selected solution s is as follow.

$$\theta _ { i j } ^ { ( t + 1 ) } = \begin{cases} \, ( 1 - \rho ) \, \theta _ { i j } ^ { ( t ) } \quad \text{if $\langlev_{i},v_{j}\rangle>\notin s$,} \\ \, ( 1 - \rho ) \, \theta _ { i j } ^ { ( t ) } + \frac { Q } { L ( s ) } \quad \text{otherwise,} \end{cases}$$

where L ( ) · is the fitness function, Q is a constant factor, t is the iteration number, and ρ ∈ (0 , 1) is an evaporation ratio.

The ACO algorithms could be generalized into a framework as presented in Algorithm 1 based on the above equations. Note that the local search module (line 4) is applied to exploit neighbors of a solution, which is a technique to speed up convergence and is not a mandatory procedure.

## Algorithm 1 ACO framework pseudo-code

- 1: initialize ants data();
- 2: repeat
- 3: solution construction();
- 4: local search(); // Optional
- 5: pheromone update();
- 6: until termination criterion()
- 7: end

## 3.3. Stochastic gradient descent in DL

The efficiency of parameter optimization is a process of highly concern in DL, because there are hundreds of millions parameters commonly within a deep neural network. What's more, the loss functions of DL algorithms are always non-convex, making it arduous to obtain a global-optimum set of network parameters. SGD (stochastic gradient descent) algorithms have been increasingly popular to optimize DL algorithms [19, 45].

The basic version of gradient descent algorithms, a.k.a batch gradient descent (BGD), is defined as follows. The objective of parameter optimization in DL algorithms is to minimize a loss function L θ, x ( ), where θ is the network parameters, and x is the sample data for learning. The gradient is defined as g θ ( ) = ∇ θ L θ ( ) = ∂L θ,x ( ) ∂θ , where x is full samples of a dataset. The update equation of batch gradient descent is bellow:

$$\theta _ { t + 1 } = \theta _ { t } - \rho \cdot g ( \theta _ { t }, x )$$

where ρ is the learning rate, and t is a discrete time-step.

The BGD algorithm is very time-consuming for calculating an exact gradient based on the entire samples in every iteration, which makes it inapplicable for large datasets or deep networks. To overcome the issue, the basic SGD only selects one sample x ′ ∈ x randomly to estimate gradient. Nonetheless, the gradient is too rough to cause fluctuation of the objective function. As a compromise, the mini-batch SGD version, is further designed by using multiple samples x ′ ⊂ x for the update process. This version is more stable than the basic SGD while being computationally efficient too.

However, the mini-batch SGD still cannot guarantee a good convergence for the following reasons:

- · A proper learning rate ρ is hard to determine. Inappropriate settings may lead to early stagnation, slow convergence or fluctuation.

- · Changing learning rate ρ over time, such as decaying and annealing, is an approach to alleviate problems caused by a static setting. It sometimes could help the SGD converging around the minimum, but a pre-defined decay schedule introduces new hyper-parameters. They are usually difficult to decide and not adaptive with characteristics of the training data.
- · In addition, since all of the parameters apply a same learning rate ρ , it would ignore discrepancies in frequencies of features in training data. For instance, training data with sparse features would prefer a larger learning rate setting.
- · Non-convexity of the loss function is also a challenging problem, which makes the optimization process easily traps into local optima or saddle points.

In order to solve the problems, the DL community exploits patterns hidden in each dimension. Since the basic and mini-batch SGD methods adopt only one learning rate across all dimensions, they fail to utilize unique historical information of each parameter, such as discrepancy among them. Therefore, smarter approaches should be explored.

Meuleau et al. [46] analyzed the connection between ACO and SGD, giving an insight on the mechanisms ACO algorithm works. Based on the theory, they proposed an idea of stabilizing ACO by modifying the probability equation in the tour construction stage. Dorigo et al. [47] derived two pheromone update rules based on the stochastic gradient ascent algorithm and the cross-entropy method. However, no implementation and benchmark were presented in their works. There are also some key questions remain. First, they did not consider regularization in the loss function. Second, no specific gradient computation method was given. Third, the evaporation rate of the pheromone matrix remains static. Therefore, we still do not know the potential of mutual contributions of ACO and SGD based on the research.

Zhou et al. [48] reported a preliminary model of SGD-based ACO for TSPs. They treated the pheromone matrix as parameters for learning. The evaporation process was explained as a weight decaying of the pheromone matrix. They also implemented a simple version of ADACO and performed experiments on some small TSP instances. The results showed the feasibility of the approach. Nevertheless, this version of ADACO was not validated enough to proof the effectiveness of the approach. It is limited to apply

to small TSP instances and not compared with state-of-the-art algorithms. The performance, scalability and practicability of the ADACO are still not verified.

An adaptive SGD has the ability to deal with sparse data, commonly by applying a square root operation on historical gradients. The parameters with infrequent features would receive a larger update, on the contrary frequently updated parameters are set to a lower learning rate. For example, Adagrad [49] put the square root of all previous gradients as a denominator of the update equation. However, it causes an early decay problem for that the learning rate decreases monotonically. RMSProp [50] avoids the problem by substituting the square root sum to the Root Mean Square (RMS), thus the learning rate does not always decrease. Furthermore, Adadelta [51] not only adjusts the learning rate by the RMS of the gradient but also computes the RMS of parameter updates. The fluctuation of the learning rate could be controlled by the approach.

The deficiencies of ACO, early maturity and slow convergence, may be overcome by the adaptive SGD technique for the following reasons. First, ACO is not adaptive due to the static evaporation rate setting, and thereby it is easily trapped in local optima. Integrating adaptive SGD methods into ACO may help it to jump out of a saddle point or local optima. Second, the performance of ACO highly depends on choosing good parameters, which usually needs a time-consuming trial-and-error process on different datasets. The adaptive SGD could randomly initialize the pheromone matrix or design an adaptive evaporation scheme to dynamically change parameters. Besides, ACO ignores historical optimization information, leading to the loss of utilizing hidden features. For instance, there is only one static evaporation rate during the whole optimization process, while the amount of pheromone deposit is also the same for the path of the iteration-best-ant. If ACO could exploit the historical update of the pheromone matrix, the adaptation to datasets with different characteristics may be achieved. Based on this analysis, it is clear that fusing ACO and adaptive SGD is necessary.

## 4. Adaptive SGD-based Ant Colony Optimization

In this section, the ADACO algorithm is detailed which integrates adaptive SGD and ACO to enhance the classic ACO. First, a formal definition of learning in ACO is given. Then, the design and implementation of ADACO is provided, following the complexity analysis of the proposed algorithm.

Finally, a case study with CVRP is introduced.

This ADACO algorithm significantly modifies the pheromone update stage to realize adaptability. Fig. 1 demonstrates the difference between the ADACO algorithm and the classic ACO. In the classic ACO, pheromone on all edges evaporates by a learning rate ρ . Then, the density of pheromone on edges in selecting the elite solution is strengthened by deposition. ADACO is based on a learning process, which manages the pheromone update as a parameters optimization process: (1) the quantity of pheromone deposited is calculated and saved for gradient computation later. It indicates rewards for the selected edges that are more probably included in the global optimum; (2) an approximate gradient is computed according to the deposited pheromone value; (3) an adaptive pheromone update scheme is devised by taking advantage of the gradient and historical gradient information.

## 4.1. Connection with Reinforcement Learning

To emphasize the role of learning, we analyze the similarity of ACO to Reinforcement Learning (RL) [52]. Ant-Q is the first work exploring the tie of ACOs to reinforcement learning, in particular Q-learning [53]. It introduces a local pheromone updating phase with an exploration strategy of a glyph[epsilon1] -greedy policy [52]. Liu et al. [54] incorporated a glyph[epsilon1] -greedy policy and levy flight mechanism into ACO to further improve the algorithmic performance.

Figure 1: Comparison of the classic ACO and an adaptive ACO in pheromone update stage.

![Image](image_000001_db7dc56c9dfb0a99f6fe2e6952f74db079efe39839fd7115ccaf697f9c39c825.png)

Table 1: Similarity of ACO to Reinforcement Learning.

| Reinforcement Learning   | ACO                                | Remarks                                           |
|--------------------------|------------------------------------|---------------------------------------------------|
| Agent                    | Ant                                |                                                   |
| Environment              | Problem graph                      | e.g., TSP graph.                                  |
| State                    | Partial or feasible solution       |                                                   |
| Action                   | Choices of next node to move       |                                                   |
| Policy                   | Selection probability of next node | Refer to Equation (3).                            |
| Reward                   | Pheromone deposition               | Only iteration-best- ant can be rewarded in MMAS. |

However, the glyph[epsilon1] -greedy policy-based approaches update pheromone for each ant locally step by step, which process requires extensive computational time and is hardly parallelized. In this work, the policy-gradient method [52] is applied to ACO, in which the ants explore the search space independently and they could learn an optimal policy adaptively.

The similarity of ACO to Reinforcement Learning is summarized in Table 1. Therefore, the ACO could be described that, multiple ants explore the problem graph synchronously, to learning a policy that could achieve highest reward. Since the policy of ACO is computed by Equation (3) and the reward is taken to update the pheromone matrix, the learning process of ACO could be mapped to the pheromone deposition. As aforementioned, the θ in the gradient (refer to Equation (A.5)) would multiply the learning rate and then be subtracted in the pheromone update process, which is the same as the evaporation process does. The formal definition is as follows.

At the beginning, we define the policy as the transition probability from a state to an action.

$$\pi _ { \theta } \left ( a | s ^ { k } \right ) = P r \left ( x = s _ { k + 1 } | \theta, s ^ { k } \right ),$$

where a ∈ A is an action, A is the set of all actions, s k represents a state in time step k . In ACO for TSP, A is all nodes that can be selected by an ant.

By applying this policy, a group of ants explore the TSP graph, and outputs solutions as the samples for learning. The route in each sample

could then get rewarded by the following equation.

$$G _ { t } = \frac { Q } { L ( s _ { i } ) },$$

where s i is the solution created by the i th ant. It reveals that the lower fitness value, the better reward.

Then the policy π θ should be updated according to the reward, because it depends on the pheromone matrix θ . From Equation (B.5), the policy is updated by the following equation.

$$\theta _ { t + 1 } = \theta _ { t } - \rho \theta _ { t } + \rho G _ { t } = \theta _ { t } - \rho \left ( \theta _ { t } - G _ { t } \right ),$$

where θ t is the weight decaying item, and G t is the reward for the route constructed by ants. The learning process is an instance of SGD, which has been proved in our past publication [48]. For better readability, the proof is also provided in the appendix sections.

Therefore, the learning process of ACO is clear, and this gives a direction for incorporating adaptability to the learning-based ACO as ADACO.

## 4.2. Design and Implementation of ADACO

According to the theoretical models, ACO, which is learning-based, could make use of the adaptive SGD. This section introduces how to devise such adaptive SGD and integrate it into ADACO.

As aforementioned, the classic ACO applies a static approach to update the pheromone matrix, in which learning rate ρ is fixed as well as θ ′ of the iteration-best-ant is the same within an iteration. Thus, update histories of each edge and discrepancies in the edges of a candidate solution for pheromone depositing are not exploited. As a result, two major problems are identified as the following categories:

## (1) Inefficient convergence

- · The convergent process is easily fluctuant if the pheromone of an edge is updated without employing previous knowledge.
- · Owing to the random nature of ACO, the constructed solutions are different from the optimum for the most of the time. But the edges of the solution share only an incremental value. Sparse features would be ignored for that an edge belongs to global optimum could be hardly constructed again.

## (2) Hyper-parameter dependency

- · The solution quality of ACO highly depends on hyper-parameters, such as ρ . Setting it too high would cause premature while too low value would introduce slow convergence. Selecting a proper value needs some knowledge of the problem characteristics.
- · The pheromone matrix also should be initialized with suitable values considering experiences of the problems. However, this makes it hardly to transfer the values to solve other problems.

The ADACO algorithm is designed to address the above issues. first, the pheromone matrix is updated in a per-dimension basis and adaptively in each dimension. In this research, ADACO adopts Adadelta [51] to let the pheromone update process adaptive. And it integrates max-min pheromone limit to guarantee asymptotically convergence. The equation is defined as below:

$$G _ { t } & = \gamma G _ { t - 1 } + ( 1 - \gamma ) \, g _ { t } \odot g _ { t } \\ H _ { t } & = g _ { t } \frac { \sqrt { \Delta G _ { t } + \varepsilon } } { \sqrt { G _ { t } + \varepsilon } } \\ \theta _ { t } & = \max \min ( \theta _ { t - 1 } - H _ { t } ) \\ \Delta G _ { t } & = \gamma \Delta G _ { t - 1 } + ( 1 - \gamma ) \, H _ { t } \odot H _ { t } \\ \text{the environmentally down as a way of the around coordinate with}$$

where G t is the exponentially decaying average of the squared gradients with a decay factor γ , and glyph[circledot] is element-wise dot product. It is used to control fluctuation of the stochastic gradient. ∆ G t is an exponentially decaying average of the squared updates with the decay factor γ . It functions to restrain fluctuation of the updates of θ . H t is a corrected update matrix through an approximation to the Hessian of the pheromone matrix, which is calculated by dividing the RMS of the updates by the RMS of the gradients. The maxmin function is defined below:

$$\max \min \left ( x \right ) = \begin{cases} \theta _ { \max } & x > \theta _ { \max } \\ x & \theta _ { \min } \leq x \leq \theta _ { \max } \\ \theta _ { \min } & x < \theta _ { \min } \end{cases} \quad \text{(10)}$$

where θ max and θ min are the up and down limits of a pheromone value.

Second, ADACO initializes parameters with a random strategy. The classic ACO fills the pheromone matrix by θ ij = θ max , where θ max is calculated

according to the length of a greedy constructed solution and θ min = θ max / 2 N . Obviously, the process is problem dependence. To overcome this, ADACO stochastically initializes the pheromone matrix in a fixed range below θ max as below:

$$\theta _ { i j } = \theta _ { \max } - K r \left ( \theta _ { \max } - \theta _ { \min } \right )$$

where K is a constant that controls the distribution range and r is a random value between 0 and 1.

The algorithm is implemented in two parts. The first part of the algorithm is to compute the pheromone deposition value on the route of iteration-bestant. The value can be obtained by the rewarding according to Equation (7) as below:

## Algorithm 2 The pseudo code of the pheromone deposition kernel.

- 1: for each i = 0 → n -1 parallel do
- 2: delta theta[s[i]][s[i+1]] += 1.0 / Fitness(s); { Record deposit value on the edge 〈 s , s i i +1 〉}
- 3: end for
- 4: end

In Algorithm 2, delta theta is a N × N matrix that each element represents the reward of a corresponding edge, which is initialized by 0. And s is an array of the iteration-best-ant solution.

The second part is to adaptively update the pheromone matrix by the gradients. Algorithm 3 uses the deposition value processed in the first part based on Equation (8). The array theta contains N × N elements which are one to one mapped with the pheromone matrix. The dimension of other supporting arrays are also N × N . Since each parameter is updated independently in its own dimension, the algorithm could be effortlessly parallelized. Thus, a parallel implementation is developed according to Equation (9) of adaptive pheromone update. The pseudo code of the adaptive pheromone update is below.

## Algorithm 3 The pseudo code of the adaptive pheromone update kernel.

- 1: for each i = 0 → n 2 -1 parallel do
- 2: gt[i] = theta[i] - delta theta[i] / rho;
- 3: acc[i] = beta1*acc[i] + (1-beta1)*gt[i]*gt[i];
- 4: update = gt[i]*sqrt(d acc[i] + epsilon) / sqrt(acc[i] + epsilon);
- 5: theta[i]=theta[i] - update;
- 6: d acc[i] = beta1*d acc[i] + (1-beta1)*update*update; { Apply maxmin limitation }
- 7: if(theta[i] &gt; THETA MAX) theta[i]=THETA MAX;
- 8: if(theta[i] &lt; THETA MIN) theta[i]=THETA MIN;
- 9: end for
- 10: end

Therefore, the superiorities of ADACO are summarized as below:

- · The adaptive tuning of learning rate would contribute to avoiding trapped in local optima, thus make the optimization process more adaptively.
- · The historical optimization information of parameters is utilized, so that less hyper-parameter dependency should be achieved.
- · The algorithm would be more robust and accurate than the classic ACO.

## 4.3. Complexity

The ADACO algorithm modifies the pheromone update process, so the time complexity is analyzed. Algorithm 2 and Algorithm 3 show that the elements of the pheromone matrix ( N × N ) is processed only once in the loop. As a result, the time complexity of ADACO is O n ( 2 ), which is the same as the classic ACO. In Algorithm 3, only three aidant arrays are added to store gradients (gt[ n 2 ]), accumulated squared gradients (acc[ n 2 ]) and accumulated delta (d acc[ n 2 ]), thus the space complexity is also maintained. Therefore, the time and space complexity of ADACO are kept at the same level as classic ACOs. In addition, the pheromone update phase is a less time-consuming part in ACO, and plus its parallel implementation, hence the adaptability could be achieved with trivial extra computational overhead.

Figure 2: An example of CVRP solution.

![Image](image_000002_e1d5c84a440d04cf7215f71123f40c64b28e6999f1680635ad8cd300d848afe2.png)

## 4.4. Case Study with CVRP

## 4.4.1. Transferring CVRP to TSP

To ensure practicability of ADACO, we conduct a case study with CVRP. As depicted in Fig. 2, there are one depot and 10 cities in a CVRP instance. Typically, two major tasks are considered to solve the problem. First task is the partition of cities. For each vehicle, a group of cities are assigned to plan for delivery. Second task is optimizing the routes for the vehicles. Constructing a route could be regarded as a small TSP problem.

We could not directly solve CVRP with ADACO due to the differences between CVRP and TSP. Therefore, in this work, we transfer the CVRP to a TSP (VRP2TSP) as follows.

- (1) Copying of the depot. In CVRP solution, the depot could exist K times which violates with the TSP. Therefore, we copy depot K -1 times to make the CVRP solution compatible with the TSP's.
- (2) Distance matrix modification. Since the copied depots shared the same location of the original depot, the distance between depots would be 0. We replace the distance by a very large number which is a penalty to continuous depots.

- (3) Parsing solution. The CVRP solution should consider the capacity constraint. First, routes can be extracted by splitting the solution with depots. Second, capacity and distance of each route are calculated. Finally, a fitness value is computed by an equation which is the weighting sum of total distance, max route length and overloading.

## 4.4.2. Adapting ADACO to CVRP

Based on the VRP2TSP transfer, ADACO could solve CVRP directly similar to TSP. However, some minor modifications should be made in the tour construction phase. The main reason is that the ACO for TSP constructs solutions without satisfaction of capacity constraint. Besides, the heuristic information also could be strengthened to utilize characteristic of CVRP. Our method is as follows.

- (1) When a vehicle starts from the depot, a variable named residual cap = C is initialized to record residual capacity of the vehicle. If residual cap = 0, the vehicle should return to the depot instantly.
- (2) A vehicle should return to the depot in an appropriate timing to avoid under-loading. We set up a variable named min cap to ensure the condition.
- (3) Saving value [56] is applied to the heuristic information computation. It could make use of the CVRP problem feature.

For CVRP, probability selection for a next travelled node is below:

$$P r \left ( x \left | \theta, s ^ { k } \right ) = \begin{cases} 0 & \text{if $x\in s^{k}$} \\ \frac { \theta _ { ( s _ { k }, x ) ^ { \eta } ( s _ { k }, x ) } \xi _ { ( s _ { k }, x ) } } { \sum _ { y \notin s ^ { k } } \theta _ { ( s _ { k }, y ) ^ { \eta } ( s _ { k }, y ) } \xi _ { ( s _ { k }, y ) } } & \text{otherwise,} \end{cases}$$

where ξ = [ ξ ij ] ∈ R N × N denotes the saving value matrix and ξ ij = d 0 i + d 0 j -d ij .

Besides, the local search procedure is a common choice to be integrated into a metaheuristic to efficiently solve CVRPs. In this work, insertion, inter-routes 2-swap, intra-route 2-opt and inter-routes 2-opt operators are implemented. The operators are explained as follows.

- · Insertion: moving a customer to a new route.
- · Inter-routes 2-swap: swap two customers in two different routes.

Table 2: Hyper-parameter configuration. N.A. stands for 'not applicable'. N is the number of nodes which is a problem specific configuration.

| Property                    | Notation        | Classic ACO   | ADACO   |
|-----------------------------|-----------------|---------------|---------|
| Nodes number                | n               | N             | N       |
| Ants number                 | m               | N             | N       |
| Pheromone factor            | α               | 1             | 1       |
| heuristic factor            | β               | 2             | 2       |
| RMS decay                   | γ               | N.A.          | 0.95    |
| Epsilon                     | glyph[epsilon1] | N.A.          | 1e-7    |
| Heuristic strength          | Q               | 1             | 1       |
| Evaporation (Learning) rate | ρ               | 0.2           | N.A.    |
| Iterations                  | i               | 2000          | 2000    |

- · Intra-route 2-opt: in a route, two links are break and reconnected.
- · Inter-routes 2-opt: two links in two different routes are break and reconnected.

## 5. Performance Evaluation

The ADACO algorithm was validated with experiments conducted on a PC with an Intel(R) Xeon(R) CPU E5-4607 v2 @ 2.60 GHz. It was implemented by C++ and OpenCL on the parallel ACO framework developed by the authors [39].

The experiments chose a standard set of benchmark instances from the TSPLIB library [57] and CVRP library [58]. For the pheromone information and city distances, single-precision floatpoint numbers were used in the experiments. On the algorithmic level, conforming to the experimental principles suggested by St¨tzle u et al. [14], key parameters were configured as follows: m = n ( n being the number of cities), α = 1, β = 2, and ρ = 0.2. These hyper-parameter configurations are summarized in Table 2.

The program tests each TSP instance for 2000 iterations and averaged over 25 independent runs, which is aligned with the previous high-performance ACOs [36, 39]. The MMAS algorithm proposed by St¨zle in [14] was taken u as a baseline for benchmarking. MMAS was chosen to perform a particular

comparison because it is the closest ACO variant to ADACO, thus the results make sense for the theoretic analysis. Solution quality comparisons were conducted including the AS, ACS, MMAS and ILS algorithms to further ensure that experimental results are convincing.

Statistics of the results were treated by the following processes. First, average, maximum, minimum and standard deviation are obtained from each group of the runs. Second, the solution quality (accuracy) is computed by A s ( ) = L s ( ∗ ) /L s ( ), where s ∗ is the optimal solution, and s is the averaged output. Third, statistical tests were done to ensure our improvement is significant.

## 5.1. Convergence Analysis

As illustrated in Fig. 3, the convergence curves of ADACO and MMAS in 9 TSP instances were compared. Every curve is the best one selected from its group of runs. This figure shows the convergent speed of MMAS is higher than ADACO at early stages (first 128 iterations approximately). This phenomenon is caused by the static learning rate of MMAS, and that would make a higher possibility to stick in a local optimum. On the contrary, ADACO performs a smoother descend during the whole optimization process. At the beginning, it drops down slower than MMAS, but after 256 iterations it still descends slowly and surpasses MMAS finally (it could be better observed through Table 3 in Section 5.3). Owing to the exploitation of history gradient, ADACO converges adaptively and thus it could achieve better results more probably.

## 5.2. Parameter Adaptation

The adaptation performance of ADACO was assessed by analyzing results generated with changing the evaporation rate ρ from 0.2 to 0.8 with a stride 0.2 in each TSP instance. In ADACO, the learning rate is dynamic, so ρ is only used for calculating the gradient. The results in Fig. 4 demonstrate that ADACO achieves better accuracy in all cases. It can be observed that this hyper-parameter has a high impact on the performance of MMAS because of its fixed learning rate. A larger learning rate may accelerate convergence but impede finding the global optimum. ADACO performs more stable due to correcting gradient in each iteration. In addition, the best setting for both algorithms is ρ = 0 2, reveals that it is not only an experiential good . choice in MMAS but also advantageous to the adaptive gradient calculation in ADACO.

Figure 3: Results of convergence curve. Horizontal axis is log scale.

![Image](image_000003_7676b75eb99601bcce39fcc5bf46ee00ddccbb88ecaf5667860efa5f489b7022.png)

## 5.3. Solution Quality

The comparison between ADACO and MMAS on accuracy and travel length are illustrated in Fig. 5. ADACO improves MMAS significantly observed from the figure. The trend is the larger problem will gain better performance advance, and the maximum improvement of traveled length is 1850 in the problem with 1002 nodes. Besides, ADACO achieves better accuracy in all cases.

The stability of ADACO was evaluated by the statistical tests. Table 3 displays that the standard deviation index of ADACO is at least 8% better than MMAS for most cases; meanwhile, the min and max values of ADACO are better. To ensure our improvement is significant, we conducted Wilcoxon signed-rank test [59] between our method and the algorithm for comparison. The solution data of each TSP instance generated by each testing pair algorithms are analyzed. The test results in Table 3 show that the improvement of our method is significant statistically in 16 out of 18 cases. The left 2

Figure 4: Hyper-parameter adaptation test result.

![Image](image_000004_f817622788d4b79f15a6f82a8c905ea12f2e74d875b9f2609e98fcf3dd8f77f5.png)

problems are also slightly improved. The results indicate that ADACO is more accurate and stable than MMAS and ACS.

## 5.4. Comparison with Other High Performance TSP solvers

The ADACO algorithm was compared with other high-performance TSP solvers including ACS with local search, MMAS with local search, and Iterated Local Search (ILS). The original AS algorithm is chosen as a baseline result and deviations from the baseline are given. To guarantee the comparison is fair, each algorithm ran for 25 times in 2000 iterations and average results were calculated. A 2-opt local search operator was appended for all ACO algorithms ending with 'LS' (local search). Table 4 summarizes the average results and percentage deviations from the baseline.

The ADACO performed better than other variants of ACO without local search illustrated from the results. This can be explained by the effect of adaptive learning mechanism. The adaptive SGD indeed increased the ac-

Figure 5: Average solution quality comparison result.

![Image](image_000005_b7f6234ce5fa2017c68285aba1f4b3189c9aae8c6068a85eea909c95a9274124.png)

Table 3: Stability comparison with classic ACOs

| Algorithm   | Index   | eil51   | pr107    | d198    | a280    | lin318   | pcb442   | u574     | rat783   | pr1002    |
|-------------|---------|---------|----------|---------|---------|----------|----------|----------|----------|-----------|
| ADACO       | Min     | 426     | 44303    | 15845   | 2583    | 42679    | 51393    | 37446    | 9032     | 266992    |
|             | Max     | 433     | 44785    | 16129   | 2666    | 43582    | 52456    | 38559    | 9230     | 273351    |
|             | Std     | 1.9209  | 140.9054 | 78.6893 | 23.8459 | 266.8539 | 278.2229 | 264.104  | 47.2242  | 1773.1195 |
| MMAS [14]   | Min     | 426     | 44303    | 15867   | 2597    | 42860    | 51709    | 37932    | 9124     | 268999    |
|             | Max     | 440     | 45089    | 16195   | 2698    | 44186    | 53006    | 39123    | 9287     | 277042    |
|             | Std     | 3.7639  | 185.7855 | 91.6942 | 24.564  | 312.8496 | 350.8479 | 316.9181 | 39.4093  | 1939.5163 |
| Wilcoxon    | T stat. | 82      | 160.5    | 63.5    | 156     | 79       | 52.5     | 73       | 1        | 60        |
| signed-rank | Sig.    | yes     | no       | yes     | no      | yes      | yes      | yes      | yes      | yes       |
| test result | α       | 0.05    | -        | 0.01    | -       | 0.05     | 0.01     | 0.02     | 0.001    | 0.01      |
| ACS [13]    | Min     | 426     | 44303    | 16136   | 2649    | 43273    | 52324    | 39918    | 9506     | 293807    |
|             | Max     | 445     | 44978    | 17156   | 2774    | 45346    | 56075    | 44228    | 10535    | 314304    |
|             | Std     | 4.4956  | 197.8749 | 279.103 | 31.2616 | 491.102  | 870.5477 | 948.5039 | 296.1372 | 4101.6553 |
| Wilcoxon    | T stat. | 18.5    | 99       | 0       | 0       | 0        | 1        | 0        | 0        | 0         |
| signed-rank | Sig.    | yes     | yes      | yes     | yes     | yes      | yes      | yes      | yes      | yes       |
| test result | α       | 0.001   | 0.1      | 0.001   | 0.001   | 0.001    | 0.001    | 0.001    | 0.001    | 0.001     |

curacy of the ACO, which validated the theoretic analysis in this research. To check the adaptation of the ADACO, a local search operator was applied. A positive result was also obtained that the ADACO algorithm achieved the best performance in 8 TSP instances out of 9. The reason is that the local search operator helps generate better samples by searching the neighborhoods, and the gradients are also more accurate. Thus, the ADACO

Table 4: Comparison with other high performance TSP solvers.

| Problem   |   AS [11] | ACS [13]           | MMAS[14]            | ADACO              | ILS [60]            | ACS-LS             | MMAS-LS             | ADACO-LS            |
|-----------|-----------|--------------------|---------------------|--------------------|---------------------|--------------------|---------------------|---------------------|
| eil51     |    437.36 | 431.28 (-1.39%)    | 429.6 (-1.77%)      | 428.76 (-1.97%)    | 426.48 (-2.49%)     | 426.72 (-2.43%)    | 426.16 (-2.56%)     | 426.12 (-2.57%)     |
| pr107     |  47257.8  | 44657.64 (-5.50%)  | 44604.52 (-5.61%)   | 44581.56 (-5.66%)  | 44316.88 (-6.22%)   | 44333.96 (-6.19%)  | 44303 (-6.25%)      | 44303 (-6.25%)      |
| d198      |  17210.6  | 16713.72 (-2.89%)  | 16022 (-6.91%)      | 15955.8 (-7.29%)   | 15796.84 (-8.21%)   | 15792.44 (-8.24%)  | 15788.16 (-8.26%)   | 15785.96 (-8.28%)   |
| a280      |   3046.2  | 2690.04 (-11.69%)  | 2625.16 (-13.82%)   | 2618.72 (-14.03%)  | 2588.16 (-15.04%)   | 2585.52 (-15.12%)  | 2579 (-15.34%)      | 2579 (-15.34%)      |
| lin318    |  47091.4  | 43989.84 (-6.59%)  | 43286.04 (-8.08%)   | 43102.64 (-8.47%)  | 42384.84 (-9.99%)   | 42261.84 (-10.26%) | 42129.16 (-10.54%)  | 42116.76 (-10.56%)  |
| pcb442    |  61128.4  | 53847.64 (-11.91%) | 52240.6 (-14.54%)   | 51958.44 (-15.00%) | 51222.68 (-16.20%)  | 51054.68 (-16.48%) | 50999.8 (-16.57%)   | 50974.16 (-16.61%)  |
| u574      |  44758.9  | 41871.32 (-6.45%)  | 38462.48 (-14.07%)  | 38202.48 (-14.65%) | 37412.28 (-16.41%)  | 37212.32 (-16.86%) | 37090.96 (-17.13%)  | 37022.28 (-17.29%)  |
| rat783    |  10934.2  | 9910.92 (-9.36%)   | 9212.44 (-15.75%)   | 9120.04 (-16.59%)  | 8984.64 (-17.83%)   | 8879.92 (-18.79%)  | 8852.92 (-19.03%)   | 8853.68 (-19.03%)   |
| pr1002    | 328556    | 304847.68 (-7.22%) | 272098.96 (-17.18%) | 270248.4 (-17.75%) | 263825.92 (-19.70%) | 261571 (-20.39%)   | 261057.52 (-20.54%) | 260806.64 (-20.62%) |

algorithm could improve solution quality by learning adaptively from these good samples.

## 5.5. Time Cost

In the previous section, the time complexity was analysed. In this section, the absolute execution time of ADACO and SIMD parallel MMAS (SPMMAS) [39] is compared. The benefits of a parallel version against a purely sequential version were detailed in [39]. We recorded the execution time for each run and averaged them for comparison. The iteration number of each algorithm is set to 2000. Owing to the parallel implementation of ADACO, Figure 6 verifies that the computation time for ADACO and SP-MMAS is nearly the same. The results show that ADACO has increased a very little computational time, but could achieve much better results.

## 5.6. Extend to Large and Asymmetric TSPs

Since the proposed method is parallelized, we further perform experiment on more difficult problems, e.g., large TSP instances and asymmetric TSPs. For large TSP instances, only basic ACO could not achieve enough convengence and would cost a huge execution time. Considering convergence and time cost, we choose the nearest neighbour heuristic and 3-opt local search applied. Key parameters in this experiment were configured as follows: m = 25, nearest neighbor lists are 20 and 40 for the tour construction

Figure 6: Comparison of absolute execution time of parallel implementation.

![Image](image_000006_5aca279cebb1fcb239cc2d893d9f4dfbdf578aa59a7890f26e4f3def97bcba8e.png)

and 3-opt local search respectively. Two state-of-the-art ACO algorithms were chosen for comparison: (1) VTPAM-CPU-MMAS [39] is an SIMD parallel implementation. We compiled the source code and tested it on our platform. (2) MACO [34] is a modified ACO algorithm for TSP. Since its source code was not provided, we took the results from their paper. For all the algorithms, iteration number setting was 2000 and each instance was tried 25 times independently except for MACO was 30. Table 5 demonstrates that ADACO performs better in most of the TSP instances up to 4,461 nodes, and statistical tests were also made to guarantee the improvement to VTPAM-CPU-MMAS is statistically significant. The confidence level of the difference ranges from 80% to 99.9%. Only in instance 'fl1577', ADACO is a little behind MACO in average and minimum results, but is far more better in standard deviation.

We also modified ADACO to solve asymmetric TSPs (ATSPs), so as to validate our method in a different context. The major distinction between symmetrical TSP and ATSP is that the graph of an ATSP instance is directed. Seven typical ATSP instances were chosen which scales from 70 to 443 nodes for the benchmark. Table 6 shows that ADACO performs better in all ATSP instances. In only one instance the statistical test result is not

Table 5: Benchmark with large TSP instances

| Algorithm                        | Index          | pcb442             | rat783          | pr1002                | fl1577             | d2103              | pcb3038               | fnl4461          |
|----------------------------------|----------------|--------------------|-----------------|-----------------------|--------------------|--------------------|-----------------------|------------------|
| ADACO                            | Min Avg Std    | 50778 50838.3 66.8 | 8806 8809.6 4.8 | 259045 259203.1 227.7 | 22287 22327.9 19.7 | 80451 80493.1 35.4 | 137894 138086.1 121.1 | 183154 183312 97 |
| VTPAM-CPU-MMAS [39]              | Min Avg        | 50778 50854.68     | 8806 8810.72    | 259045 259374.44      | 22281 22339.24     | 80468 80511.36     | 137903 138155.36      | 183175 183381.88 |
| Wilcoxon signed-rank test result | T stat. Sig. α | 25 yes 0.001       | 81.5 yes 0.05   | 74.5 yes 0.02         | 111 yes 0.2        | 84.5 yes 0.05      | 98 yes 0.1            | 90 yes 0.1       |
| MACO [34]                        | Min Avg        | - -                | 8806 8817.1     | 259045 259211.7       | 22250 22306.8      | 80462 80870.4      | 137694 138168.5       | 182987 184356.1  |

Table 6: Benchmark with asymmetric TSP instances

| Algorithm   | Index   | ry48p   | ft70    | kro124p   | ftv170   | rbg323   | rbg403   | rbg443   |
|-------------|---------|---------|---------|-----------|----------|----------|----------|----------|
| ADACO       | Min     | 14422   | 39048   | 36230     | 2790     | 1420     | 2682     | 3007     |
|             | Avg     | 14573.5 | 39382.4 | 37148.5   | 2844.1   | 1435.4   | 2709.7   | 3026     |
|             | Std     | 99.9    | 188.1   | 592.4     | 33.5     | 7.7      | 13.5     | 10.6     |
| MMAS [14]   | Min     | 14446   | 39253   | 36230     | 2813     | 1457     | 2677     | 3006     |
|             | Avg     | 14684.2 | 39907   | 37542.4   | 2881.5   | 1477.3   | 2712.2   | 3041     |
|             | Std     | 162.6   | 397.4   | 649.5     | 51.6     | 12.2     | 10.5     | 12.5     |
| Wilcoxon    | T stat. | 46      | 17      | 84        | 73       | 0        | 132      | 38.5     |
| signed-rank | Sig.    | yes     | yes     | yes       | yes      | yes      | no       | yes      |
| test result | α       | 0.002   | 0.001   | 0.05      | 0.02     | 0.001    | -        | 0.001    |

significant, but the average value of ADACO in this instance is better.

The above results determine the superiority of ADACO over other variants of ACO in solving difficult TSP and ATSP problems.

## 5.7. Benchmark with CVRP Instances

CVRP is closely related to TSP, nevertheless, it is more complex due to the constraint of the vehicle number and its capacity. More practical, CVRP has tight connections with real world applications in transportation, logistics and networking. ADACO adapts to solving CVRP. We choose recently updated CVRP datasets [58] for our benchmark.

The result in Table 7 shows that ADACO could find best solution in 10 CVRP instances range from 106 to 701 cities, and 6 to 44 vehicles. Our approach achieved state-of-the-art performance in complex CVRP instances.

Table 7: Benchmark with CVRP instances

| Instances   |   Best Known Solution | Type of Solution   |   Best Solution by ILS-SP [61] |   Best Solution by GELS [62] | Best Solution by ACSS [63]   |   Best Solution by ADACO |
|-------------|-----------------------|--------------------|--------------------------------|------------------------------|------------------------------|--------------------------|
| X-n106-k14  |                 26362 | OPT                |                          26362 |                        26362 | 26362                        |                    26362 |
| X-n120-k6   |                 13332 | OPT                |                          13332 |                        13332 | 13332                        |                    13332 |
| X-n143-k7   |                 15700 | OPT                |                          15726 |                        15709 | 15700                        |                    15700 |
| X-n167-k10  |                 20557 | OPT                |                          20562 |                        20557 | 20557                        |                    20557 |
| X-n181-k23  |                 25569 | OPT                |                          25569 |                        25569 | 25569                        |                    25569 |
| X-n200-k36  |                 58578 | OPT                |                          58626 |                        58578 | 58578                        |                    58578 |
| X-n359-k29  |                 51505 | no                 |                          51706 |                        51509 | 51505                        |                    51505 |
| X-n459-k26  |                 24139 | no                 |                          24209 |                        24181 | -                            |                    24181 |
| X-n573-k30  |                 50673 | no                 |                          51092 |                        50780 | -                            |                    50780 |
| X-n701-k44  |                 81934 | no                 |                          82888 |                        82294 | 81934                        |                    81934 |

## 6. Conclusion

ACOis a typical swarm intelligent approach that can solve many NP-hard optimization problems. The performance of ACO, However, is not satisfactory enough. Due to the static process of the pheromone update, ACO has problems such as early maturing and slow convergence. The research in this article provides a new vision to improve the performance of ACO through adaptive learning. A theoretic analysis between ACO and RL is given in the context of solving TSPs, and it can be concluded that ACO learns an optimal policy from exploration and rewarding of the ants by SGD. Based on this discovery, an improved adaptive gradient descent approach for ACO, namely ADACO, is explored and realized. In this approach, a method of gradient calculation is devised for solving TSPs, and then integrated into an adaptive parameter update scheme. A broad range of TSP instances varying from 51 to 4,461 nodes was used to evaluate the designed ADACO algorithm. What's more, the proposed algorithm was adapted to other types of problems, i.e., ATSP and CVRP. Case studies demonstrated that ADACO achieved improvement over classic MMAS as high as 1850 in traveled length, and better accuracy in all cases. Additionally, statistical test results determined the improvement was significant, and other types of problems such as ATSP and CVRP were also tested to ensure its applicable in different contexts. Meanwhile, our algorithm was more stable and less hyper-parameter dependent, showing that its convergent process was more intelligent. Furthermore, the ADACO algorithm was compared with the AS, ACS, ILS and several stateof-the-art algorithms for validation. The results showed the performance is competitive.

An interesting point is that the proposed approach can be generalized

to other metaheuristics. Stochastic selection processes, which are frequently appeared in intelligent algorithms, could potentially apply adaptive SGD to enhance performance. Besides, ADACO can be applied to other graph-based COPs owing to their similarities in problem structures.

In the future, we could investigate introducing other DL approaches, i.e., transfer learning and generative adversarial networks, to the swarm intelligence domain. We should also apply the proposed research to solve real problems in the industry, such as dynamic VRPs and scheduling problems.

## Appendix A. Loss Function and Gradient Definition for ACO

The first stage of the ADACO algorithm is for tour construction. Since ants create solutions independently, the events of the solutions construction are independent and identically distributed. Then looking into the process of constructing a solution, the probability of selecting a node in each construction step can be defined by a conditional probability P x ( k +1 = s k +1 | θ, s k ), where k is the step number. The selection probability is conditional on the pheromone matrix θ , since it is static during the tour construction process. According to the theorem on multiplication of probabilities, the probability of a solution can be constructed by the following equation.

$$P \left ( X = s \left | \theta \right ) = \frac { 1 } { N } \prod _ { k = 1 } ^ { N - 1 } P r \left ( x = s _ { k + 1 } \right | \theta, s ^ { k } \right ) \right )$$

where Pr is the probability distribution function of selecting nodes defined in Equation (3) and 1 N is the probability of selecting the first node.

The objective of the ADACO algorithm is to maximize accuracy of randomly constructed solution s following probability distribution P as below:

$$A ( \theta ) = \frac { L ( s ^ { * } ) } { L ( s ) }$$

where L s ( ∗ ) is the fitness of the global best solution, L s ( ) represents the fitness of a randomly constructed solution s , and θ is the variable of the function means the goal is to find a optimal pheromone matrix that the accuracy equals to 1.

Unlike SGD in DL takes labeled samples for learning, the ADACO algorithm generates solutions in the tour construction process. It is stochastic,

and therefore the loss function could be defined as minimizing the expectation of error ratio 1 -A θ ( ). Since the solutions are randomly sampled, a L2 regularization term is also added to avoid overfitting. The loss function is defined as follows:

$$\text{me as rows.} \\ J \left ( \theta \right ) & = \mathbb { E } \left [ 1 - A ( \theta ) \right ] + \frac { 1 } { 2 } \ell ^ { 2 } ( \theta ) \\ & = \mathbb { E } [ 1 ] + \mathbb { E } \left [ - A ( \theta ) \right ] + \frac { 1 } { 2 } \ell ^ { 2 } ( \theta ) \\ & = 1 + \sum _ { s \in X ^ { N } } - \frac { L ( s ^ { * } ) } { L ( s ) } \cdot P \left ( X = s \left | \theta \right ) + \frac { 1 } { 2 } \left \| \theta \right \| _ { 2 } ^ { 2 } \\ \cdot \quad \text{$\lambda \wedge } \quad. \quad. \quad. \quad. \quad. \quad. \quad. \quad..$$

where 1 -A θ ( ) represents the error ratio, glyph[lscript] 2 is the L2 norm and its coefficient is 0.5. The goal of ADACO in solving TSP is to minimize the loss function J θ ( ).

Thus, the gradient of the loss function is deduced as follows:

$$\text{us, the gradient of the loss function is deduced as follows:} \\ & \nabla J \left ( \theta \right ) = \frac { \partial J } { \partial \theta } = \left [ \frac { \partial J } { \partial \theta _ { i j } } \right ] _ { N \times N } \\ & = \left [ \frac { \partial \left ( \sum _ { s \in X ^ { N } } - \frac { P ( X = s | \theta ) } { L ( s ) } + \frac { 1 } { 2 } \theta _ { i j } ^ { 2 } \right ) } { \partial \theta _ { i j } } \right ] _ { N \times N } ( A. 4 ) \\ & = \left [ - \sum _ { s \in X ^ { N } } \frac { 1 } { L \left ( s \right ) } \cdot \frac { \partial P \left ( X = s \left | \theta \right ) } { \partial \theta _ { i j } } + \theta _ { i j } \right ] _ { N \times N } \\ \cdot L ( s ^ { * } ) \text{ is not prior known and fixed for a TSP problem } \text{$2. Thus, $3.$} \quad.$$

where L s ( ∗ ) is not prior known and fixed for a TSP problem instance. Thus, it is set to equal to 1 for simplicity in Equation (A.4). Then, according to Equation (A.1), the partial derivative of construction probability P is defined

by chain rules to get the following equation:

$$& \frac { \partial P \left ( X = s \left | \theta \right ) } { \partial \theta _ { i j } } = \frac { \partial } { \partial \theta _ { i j } } \left [ \left [ n P \left ( X = s \left | \theta \right ) \right ] \\ & = P \left ( X = s \left | \theta \right ) \cdot \frac { \partial } { \partial \theta _ { i j } } \left [ \ln P \left ( X = s \left | \theta \right ) \right ] \\ & = P \left ( X = s \left | \theta \right ) \cdot \frac { \partial \left [ \ln \frac { 1 } { N } + \sum _ { k = 1 } ^ { N - 1 } \ln P r \left ( x = s _ { k + 1 } \left | \theta, s ^ { k } \right ) \right ] } { \partial \theta _ { i j } } \\ & = P \left ( X = s \left | \theta \right ) \cdot \sum _ { k = 1 } ^ { N - 1 } \frac { \partial \ln P r \left ( x = s _ { k + 1 } \left | \theta, s ^ { k } \right ) } { \partial \theta _ { i j } } \\ \intertext { f o t } \tilde { T. w } \Delta \Delta \Delta _ { 1 } \underline { ( } \underline { ( } X = s \left | \theta \right ) } - \sum _ { k = 1 } ^ { N - 1 } \frac { \partial \ln P r \left ( x = s _ { k + 1 } \left | \theta, s ^ { k } \right ) } { \partial \theta _ { i j } } \text{ based on $P $contin on $L$} \Delta \Delta \end{$$

Let T ij ( θ ) glyph[defines] ∂ ln P X ( = s θ | ) ∂θ ij = N -1 ∑ k =1 ∂ ln Pr x ( = s k +1 | θ,s k ) ∂θ ij , based on Equation (A.4) and (A.5), the following equation can be obtained:

$$\mathbf n \left ( \mathbf A. \mathfrak { D } \right ), \, \text{the following equation can be obtained} \colon \\ \nabla J \left ( \theta \right ) = \left [ - \sum _ { s \in X ^ { N } } \frac { 1 } { L \left ( s \right ) } \cdot \frac { \partial P \left ( X = s \left | \theta \right ) } { \partial \theta _ { i j } } + \theta _ { i j } \right ] _ { N \times N } \\ = \left [ - \sum _ { s \in X ^ { N } } \frac { T _ { i j } ( \theta ) } { L \left ( s \right ) } \cdot P \left ( X = s \left | \theta \right ) + \theta _ { i j } \right ] _ { N \times N } \\ = - \mathbb { E } \left [ \frac { T ( \theta ) } { L \left ( s \right ) } \right | \theta \right ] + \theta \\ \mathbf \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \convolution } \right ] \mathbf \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \mathrm \convolution }$$

where T θ ( ) = [ T ij ( θ )] ∈ R N × N .

As a result, the evolution process of the ADACO algorithm could be viewed as a learning process as below. That is, it iteratively updates the learning parameter (same as pheromone) matrix to reduce results of the loss function:

$$\text{ion} \colon & \quad \theta \coloneqq \theta - \rho \nabla _ { \theta } J = \theta - \rho \left ( - \mathbb { E } \left [ \frac { T ( \theta ) } { L \left ( s \right ) } \Big | \, \theta \right ] + \theta \right ) \\ & \quad = \left ( 1 - \rho \right ) \theta + \rho \mathbb { E } \left [ \frac { T ( \theta ) } { L \left ( s \right ) } \Big | \, \theta \right ] \\ \quad \text{$\quad \dots \quad $\lambda \, \infty \, \cdots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \end{array}$$

From Equation (A.7), it is noted that the L2 regularization term in the update equation is the same as the pheromone evaporation in Equation (4). This reveals that the evaporation process is actually functioned as weight

decays. Then, the expectation item in Equation (A.7) that is related to the pheromone deposition process in ADACO is discussed in the next section.

## Appendix B. Calculation of Gradient in ACO

Since it is impossible to list all solutions as samples for learning, a sample of the expectation could be substituted for the gradient in Equation (A.7) and yield the following equation:

$$\theta \coloneqq \left ( 1 - \rho \right ) \theta + \rho \frac { T \left ( \theta \right ) } { L \left ( s \right ) }$$

In the AS algorithm, all ants deposit pheromone on their travelled paths. The AS algorithm follows the direction of Equation (B.1) by averaging multiple samples as 1 M i M ∑ =1 T θ ( ) L s ( ) . However, the convergence speed is slow due to the samples may contain 'bad' solutions. In ACS and MMAS, only the best ants are allowed to deposit pheromone, which is a way to accelerate convergence speed on a random greedy basis with a small loss of accuracy. According to the theory of SGD, learning by random gradient performs better than by exact gradient in cases of non-convex problems [64]. Just like estimating a gradient that is based on a mini-batch from whole samples in the SGD, the solution created by the iteration-best-ant is selected to calculate a proximate gradient.

Next, the calculation process of T ( θ ) in ADACO is shown below in detail:

$$& T _ { i j } ( \theta ) = \sum _ { k = 1 } ^ { N - 1 } \frac { \partial } { \partial \theta _ { i j } } \left [ \ln P r \left ( x = s _ { k + 1 } \left | \theta, s ^ { k } \right ) \right ] \\ & = \sum _ { k = 1 } ^ { N - 1 } \frac { \partial } { \partial \theta _ { i j } } \left [ \ln \theta _ { ( s _ { k }, s _ { k + 1 } ) } \right | _ { \substack { \nabla ( s _ { k }, y ) } \right | _ { ( s _ { k }, s _ { k + 1 } ) \\ \sum _ { y \in s ^ { k } } } \theta _ { ( s _ { k }, y ) } \right | _ { ( s _ { k }, y ) } \right ] \\ & = \sum _ { k = 1 } ^ { N - 1 } \left [ \frac { \partial } { \partial \theta _ { i j } } \left [ \ln \theta _ { ( s _ { k }, s _ { k + 1 } ) } \right | _ { \substack { \nabla ( s _ { k }, y ) } \right | _ { ( s _ { k }, s _ { k + 1 } ) \\ \partial \theta _ { i j } } } \right ] \partial \theta _ { i j } \right ] \\ & = \sum _ { k = 1 } ^ { N - 1 } \left [ \frac { \partial } { \partial \theta _ { ( s _ { k }, s _ { k + 1 } ) } \right | _ { \substack { \nabla ( s _ { k }, y ) } \right | _ { ( s _ { k }, s _ { k + 1 } ) \\ \partial \theta _ { i j } } } \partial \theta _ { i j } \right ] \partial \theta _ { i j } \right ] \quad \left ( B. 2 \right ) \\ & = \sum _ { i = s _ { k }, k = 1 } ^ { N - 1 } \frac { 1 _ { ( i, j ) \in s } } { \partial \theta _ { i j } } \sum _ { y \in s ^ { k } } \theta _ { i j } \right | _ { \substack { \nabla ( i j } \nabla _ { i j } \\ \partial \theta _ { i j } } } \sum _ { i = s _ { k }, k = 1 } ^ { \partial _ { ( i, j ) \in s } } \theta _ { i j } \right ] \partial \theta _ { i j } \right ] \\ & = \sum _ { i = s _ { k }, k = 1 } ^ { N - 1 } \frac { 1 _ { ( i, j ) \in s } } { \partial \theta _ { i j } } - P r \left ( x = j \left | \theta, s ^ { k } \right ) \right ) \theta _ { i j } \\ & = \frac { 1 _ { ( i, j ) \in s } - P r \left ( x = j \left | \theta, s _ { k } = i, s ^ { k } \right ) } { \partial i j } \\ & = \frac { 1 _ { ( i, j ) \in s } - P r \left ( x = j \left | \theta, s _ { k } = i, s ^ { k } \right ) } { \partial i j } \\ \end{cases}$$

where 1 〈 i,j 〉∈ s is an indicator function defined to be 1 if 〈 i, j 〉 ∈ s , else 0. Note that the summation is removed in the last step of the above equation. The reason is that the start node i of an edge 〈 i, j 〉 is appeared in the s only once, so that the probability of selecting the edge just calculated once. From Equation (B.2), Equation (A.7) can be written as:

$$\cdots \nolimits _ { j }, \, & \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolimits _ { j } \, \colon \nolipseq \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimil \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimis \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimpleq \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolipseq \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolimile \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \colon \nolipseq \, \text{$\nabla$} \\ & \theta _ { t + 1 } = ( 1 - \rho ) \, \theta _ { t } + \rho \theta _ { t } ^ { \prime } \\ & \theta _ { t } ^ { \prime } = [ \theta _ { i j } ^ { \prime } ] _ { N \times N } \\ & \quad = \left [ \frac { 1 _ { \langle i, j \rangle \in s } - P r _ { \theta } \left ( x = j \, | s _ { k } = i \, \right ) } { L \left ( s \right ) \cdot \theta _ { i j } ^ { ( t ) } } \right ] _ { N \times N }$$

where Pr θ ( x = j | s k = ) denotes the probability of selecting node i j when the ant is in node i . Equation (B.3) demonstrates that the pheromone is strengthened on the route of the iteration-best-ant solution, and weakened otherwise.

It needs to compute the probability of all the edges if directly using Equation (B.3) to calculate the gradient. It also requires treating multiple starting nodes for the solution of the TSP problem that is a cycle. Considering the above conditions, it is time-consuming and hard to implement. Therefore, the pheromone update scheme of the classic ACO algorithm in Equation (4) is reorganized to replace the theory gradient as follow:

$$\rho \theta ^ { \prime } _ { i j } = 1 _ { \langle i, j \rangle \in s } \cdot \frac { Q } { L ( s _ { t } ^ { * } ) } \\ \theta ^ { \prime } _ { i j } = 1 _ { \langle i, j \rangle \in s } \cdot \frac { Q } { \rho L ( s _ { t } ^ { * } ) } \\ \intertext { t i m b o t } \tan \det \tan \lim i t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t w h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t ht h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h a t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t k t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t l a t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t ht t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t a t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h ht h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t ht ht h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h h t t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t t ht h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t t h t t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t ht t ht h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t
 h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t h t t h t h t h t h t h t t h t h t h t h t h t h t h t h t t h t h t t h t h t h t h t t h t h t h t t h t h t h t h t t h t t h t h t t h t h t t h t h t t h t h t t h t h t h t h t t h t h t t h t h t t h t h t t h t t h t t h t h t h t h t t h t h t t h t t h t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t t h t$$

where s ∗ t is the iteration-best-ant solution in the t -th iteration. It makes sense because it follows the direction that pheromone on the iteration-best-ant solution will strengthen given by Equation (B.3). Accord to the Equations (A.7), (B.1), (B.3) and (B.4), the gradient of the ADACO is defined as follow:

$$& g _ { t } = \theta _ { t } - \theta ^ { \prime } = \left [ g _ { i j } ^ { ( t ) } \right ] _ { N \times N } \\ & g _ { i j } ^ { ( t ) } = \theta _ { i j } ^ { ( t ) } - \theta _ { i j } ^ { ( t ) } ^ { ( t ) } ^ { \prime } = \theta _ { i j } ^ { ( t ) } - 1 _ { ( i, j ) \in s } \cdot \frac { Q } { \rho L ( s _ { t } ^ { * } ) }$$

The above process shows that the ADACO is an instance of SGD, and both the pheromone deposition and pheromone evaporation process are explainable in the SGD framework. The above theoretical models can assure that ADACO can achieve robust convergence based on SGD, which leads to the idea of applying adaptive SGD to enhance ACO.

## Acknowledgment

The authors would like to thank the anonymous reviewers for their valuable comments.

## References

- [1] C. Blum, X. Li, Swarm intelligence in optimization, in: Swarm Intelligence: Introduction and Applications, Springer Berlin Heidelberg, 2008, pp. 43-85. doi:10.1007/978-3-540-74089-6 2.
- [2] A. Slowik, H. Kwasnicka, Nature inspired methods and their industry applications-swarm intelligence algorithms, IEEE Transactions on Industrial Informatics 14 (3) (2018) 1004-1015.
- [3] M. Schranz, G. A. D. Caro, T. Schmickl, W. Elmenreich, F. Arvin, A. S ¸ekercio˘lu, g M. Sende, Swarm intelligence and cyber-physical systems: Concepts, challenges and future trends, Swarm and Evolutionary Computation 60 (2021) 100762. doi:10.1016/j.swevo.2020.100762.
- [4] K. Socha, M. Dorigo, Ant colony optimization for continuous domains, European Journal of Operational Research 185 (3) (2008) 1155-1173.
- [5] O. Engin, A. G¨¸ cl¨, u u A new hybrid ant colony optimization algorithm for solving the no-wait flow shop scheduling problems, Applied Soft Computing 72 (2018) 166-176.
- [6] M. Dorigo, T. St¨tzle, u Ant colony optimization: overview and recent advances, in: Handbook of Metaheuristics, Springer, 2019, pp. 311-351.
- [7] X. Wen, Modeling and performance evaluation of wind turbine based on ant colony optimization-extreme learning machine, Applied Soft Computing 94 (2020) 106476. doi:10.1016/j.asoc.2020.106476.
- [8] Y. Wang, L. Wang, G. Chen, Z. Cai, Y. Zhou, L. Xing, An improved ant colony optimization algorithm to the periodic vehicle routing problem with time window and service choice, Swarm and Evolutionary Computation 55 (2020) 100675. doi:10.1016/j.swevo.2020.100675.
- [9] Z. B. Imtiaz, A. Manzoor, S. ul Islam, M. A. Judge, K.-K. R. Choo, J. J. Rodrigues, Discovering communities from disjoint complex networks using multi-layer ant colony optimization, Future Generation Computer Systems 115 (2021) 659-670. doi:10.1016/j.future.2020.10.004.
- [10] B. Guan, Y. Zhao, Y. Li, An improved ant colony optimization with an automatic updating mechanism for constraint satisfaction

|      | problems, Expert Systems with Applications 164 (2021) 114021. doi:10.1016/j.eswa.2020.114021.                                                                                                                                                             |
|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [11] | M. Dorigo, V. Maniezzo, A. Colorni, et al., Ant system: optimization by a colony of cooperating agents, IEEE Transactions on Systems, man, and cybernetics, Part B: Cybernetics 26 (1) (1996) 29-41.                                                      |
| [12] | T. Stutzle, M. Dorigo, A short convergence proof for a class of ant colony optimization algorithms, IEEE Transactions on Evolutionary Computa- tion 6 (4) (2002) 358-365. doi:10.1109/tevc.2002.802444.                                                   |
| [13] | M. Dorigo, L. M. Gambardella, Ant colony system: a cooperative learn- ing approach to the traveling salesman problem, IEEE Transactions on Evolutionary Computation 1 (1) (1997) 53-66.                                                                   |
| [14] | T. St¨tzle, H. H. Hoos, Max-min ant system, Future Generation Com- u puter Systems 16 (8) (2000) 889-914.                                                                                                                                                 |
| [15] | T. St¨tzle, u M. L´ opez-Ib´nez, a P. Pellegrini, M. Maur, M. M. De Oca, M. Birattari, M. Dorigo, Parameter adaptation in ant colony optimiza- tion, in: Autonomous Search, Springer, 2011, pp. 191-215.                                                  |
| [16] | F. Olivas, F. Valdez, O. Castillo, C. I. Gonzalez, G. Martinez, P. Melin, Ant colony optimization with dynamic parameter adaptation based on interval type-2 fuzzy logic systems, Applied Soft Computing 53 (2017) 74-87. doi:10.1016/j.asoc.2016.12.015. |
| [17] | Sandhya, R. Goel, Fuzzy based parameter adaptation in ACO for solving VRP, International Journal of Operations Research and Information Systems 10 (2) (2019) 65-81. doi:10.4018/ijoris.2019040104.                                                       |
| [18] | P. Pellegrini, T. Sttzle, M. Birattari, A critical analysis of parameter adaptation in ant colony optimization, Swarm Intelligence 6 (1) (2011) 23-48. doi:10.1007/s11721-011-0061-0.                                                                     |
| [19] | Y. LeCun, Y. Bengio, G. Hinton, Deep learning, Nature 521 (7553) (2015) 436-444. doi:10.1038/nature14539.                                                                                                                                                 |
| [20] | A. Paszke, S. Gross, F. Massa, A. Lerer, J. Bradbury, G. Chanan, T. Killeen, Z. Lin, N. Gimelshein, L. Antiga, A. Desmaison, A. Kopf,                                                                                                                     |

|      | E. Yang, Z. DeVito, M. Raison, A. Tejani, S. Chilamkurthy, B. Stein- er, L. Fang, J. Bai, S. Chintala, Pytorch: An imperative style, high- performance deep learning library, in: H. Wallach, H. Larochelle, A. Beygelzimer, F. d' Alch´-Buc, E. Fox, R. Garnett (Eds.), Advances e in Neural Information Processing Systems 32, Curran Associates, Inc., Vancouver, Canada, 2019, pp. 8026-8037.   |
|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [21] | Y. Jia, E. Shelhamer, J. Donahue, S. Karayev, J. Long, R. Gir- shick, S. Guadarrama, T. Darrell, Caffe: Convolutional architec- ture for fast feature embedding, in: Proceedings of the ACM In- ternational Conference on Multimedia - MM '14, ACM Press, 2014. doi:10.1145/2647868.2654889.                                                                                                        |
| [22] | N. Ketkar, Introduction to keras, in: Deep Learning with Python, A- press, 2017, pp. 97-111. doi:10.1007/978-1-4842-2766-4 7.                                                                                                                                                                                                                                                                       |
| [23] | W. Kool, H. van Hoof, M. Welling, Attention, learn to solve routing problems!, arXiv preprint arXiv:1803.08475 (2018).                                                                                                                                                                                                                                                                              |
| [24] | E. Khalil, H. Dai, Y. Zhang, B. Dilkina, L. Song, Learning combinato- rial optimization algorithms over graphs, in: I. Guyon, U. V. Luxburg, S. Bengio, H. Wallach, R. Fergus, S. Vishwanathan, R. Garnett (Eds.), Advances in Neural Information Processing Systems 30, Curran Asso- ciates, Inc., Long Beach, USA, 2017, pp. 6348-6358.                                                           |
| [25] | Y. Bengio, A. Lodi, A. Prouvost, Machine learning for combinatori- al optimization: a methodological tour d'horizon, arXiv preprint arX- iv:1811.06128 (2018).                                                                                                                                                                                                                                      |
| [26] | N. Klug, A. Chauhan, V. V, R. Ragala, k-RNN: Extending NN-heuristics for the TSP, Mobile Networks and Applications 24 (4) (2019) 1210-1213. doi:10.1007/s11036-019-01258-y.                                                                                                                                                                                                                         |
| [27] | K. Socha, C. Blum, An ant colony optimization algorithm for contin- uous optimization: application to feed-forward neural network train- ing, Neural Computing and Applications 16 (3) (2007) 235-247. doi:10.1007/s00521-007-0084-z.                                                                                                                                                               |
| [28] | I. Aljarah, H. Faris, S. Mirjalili, Optimizing connection weights in neural networks using the whale optimization algorithm, Soft Computing 22 (1) (2016) 1-15. doi:10.1007/s00500-016-2442-1.                                                                                                                                                                                                      |

| [29]   | Z. Tian, S. Fong, Survey of meta-heuristic algorithms for deep learn- ing training, in: Optimization Algorithms - Methods and Applications, InTech, 2016. doi:10.5772/63785.                                                                        |
|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [30]   | S. Fong, S. Deb, X. she Yang, How meta-heuristic algorithms contribute to deep learning in the hype of big data analytics, in: Advances in Intelligent Systems and Computing, Springer Singapore, 2017, pp. 3- 25. doi:10.1007/978-981-10-3373-5 1. |
| [31]   | J. Yang, Y. Zhuang, An improved ant colony optimization algorithm for solving a complex combinatorial optimization problem, Applied Soft Computing 10 (2) (2010) 653-660.                                                                           |
| [32]   | W. Elloumi, H. E. Abed, A. Abraham, A. M. Alimi, A compara- tive study of the improvement of performance using a PSO modified by ACO applied to TSP, Applied Soft Computing 25 (2014) 234-241. doi:10.1016/j.asoc.2014.09.031.                      |
| [33]   | J. Chen, X.-M. You, S. Liu, J. Li, Entropy-based dynamic heterogeneous ant colony optimization, IEEE Access 7 (2019) 56317-56328.                                                                                                                   |
| [34]   | W. Gao, Modified ant colony optimization with improved tour construc- tion and pheromone updating strategies for traveling salesman problem, Soft Computing 25 (4) (2021) 3263-3289. doi:10.1007/s00500-020-05376- 8.                               |
| [35]   | Y. Wang, Z. Han, Ant colony optimization for traveling salesman prob- lem based on parameters optimization, Applied Soft Computing 107 (2021) 107439. doi:10.1016/j.asoc.2021.107439.                                                               |
| [36]   | J. M. Cecilia, J. M. Garc´ ıa, A. Nisbet, M. Amos, M. Ujald´n, o Enhancing data parallelism for ant colony optimization on GPUs, Journal of Parallel and Distributed Computing 73 (1) (2013) 42-51. doi:10.1016/j.jpdc.2012.01.002.                 |
| [37]   | Y. Zhou, F. He, Y. Qiu, Dynamic strategy based parallel ant colony optimization on GPUs for TSPs, Science China Information Sciences 60 (6) (Feb. 2017). doi:10.1007/s11432-015-0594-2.                                                             |

| [38]   | R. Skinderowicz, The GPU-based parallel ant colony system, Journal of Parallel and Distributed Computing 98 (2016) 48-60. doi:10.1016/j.jpdc.2016.04.014.                                                                                                                             |
|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [39]   | Y. Zhou, F. He, N. Hou, Y. Qiu, Parallel ant colony optimization on multi-core SIMD CPUs, Future Generation Computer Systems 79 (2018) 473-487. doi:10.1016/j.future.2017.09.073.                                                                                                     |
| [40]   | D. Cvetkovi´ c, M. Cangalovi´ c, ˇ V. Kovaˇ cevi´-Vujˇi´, c c c Semidefinite pro- gramming methods for the symmetric traveling salesman problem, in: Integer Programming and Combinatorial Optimization, Springer Berlin Heidelberg, 1999, pp. 126-136. doi:10.1007/3-540-48777-8 10. |
| [41]   | F. Focacci, A. Lodi, M. Milano, Embedding relaxations in global con- straints for solving tsp and tsptw, Annals of Mathematics and Artificial Intelligence 34 (4) (2002) 291-311. doi:10.1023/a:1014492408220.                                                                        |
| [42]   | X.-F. Xie, J. Liu, Multiagent optimization system for solving the traveling salesman problem (TSP), IEEE Transactions on Systems, Man, and Cybernetics, Part B (Cybernetics) 39 (2) (2009) 489-502. doi:10.1109/tsmcb.2008.2006910.                                                   |
| [43]   | J. Shi, J. Sun, Q. Zhang, Homotopic convex transformation: A new method to smooth the landscape of the traveling salesman problem, arXiv preprint arXiv:1906.03223 (2019).                                                                                                            |
| [44]   | T. Ralphs, L. Kopman, W. Pulleyblank, L. Trotter, On the capacitated vehicle routing problem, Mathematical Programming 94 (2-3) (2003) 343-359. doi:10.1007/s10107-002-0323-0.                                                                                                        |
| [45]   | S. Ruder, An overview of gradient descent optimization algorithms, arX- iv preprint arXiv:1609.04747 (2016).                                                                                                                                                                          |
| [46]   | N. Meuleau, M. Dorigo, Ant colony optimization and stochastic gradient descent, Artificial Life 8 (2) (2002) 103-121.                                                                                                                                                                 |
| [47]   | M. Dorigo, M. Zlochin, N. Meuleau, M. Birattari, Updating ACO pheromones using stochastic gradient ascent and cross-entropy method- s, in: Lecture Notes in Computer Science, Springer Berlin Heidelberg, 2002, pp. 21-30. doi:10.1007/3-540-46004-7 3.                               |

| [48]   | Y. Zhou, W. D. Li, X. Wang, Q. Qiu, Enhancing ant colony optimization by adaptive gradient descent, in: Data Driven Smart Manufacturing Technologies and Applications, Springer International Publishing, 2021, pp. 191-215. doi:10.1007/978-3-030-66849-5 9.   |
|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [49]   | J. Duchi, E. Hazan, Y. Singer, Adaptive subgradient methods for on- line learning and stochastic optimization, Journal of Machine Learning Research 12 (61) (2011) 2121-2159.                                                                                   |
| [50]   | T. Tieleman, G. Hinton, Lecture 6.5-RMSPROP: Divide the gradient by a running average of its recent magnitude, in: COURSERA: Neural networks for machine learning, Vol. 4, 2012, pp. 26-31.                                                                     |
| [51]   | M. D. Zeiler, ADADELTA: an adaptive learning rate method, arXiv preprint arXiv:1212.5701 (2012).                                                                                                                                                                |
| [52]   | R. S. Sutton, A. G. Barto, Reinforcement Learning: An introduction, 2nd Edition, The MIT Press, Cambridge, MA, 2018.                                                                                                                                            |
| [53]   | L. M. Gambardella, M. Dorigo, Ant-q: A reinforcement learning ap- proach to the traveling salesman problem, in: Machine Learning Pro- ceedings 1995, Elsevier, 1995, pp. 252-260. doi:10.1016/b978-1-55860- 377-6.50039-6.                                      |
| [54]   | Y. Liu, B. Cao, H. Li, Improving ant colony optimization algorithm with epsilon greedy and levy flight, Complex & Intelligent Systems 7 (4) (2020) 1711-1722. doi:10.1007/s40747-020-00138-3.                                                                   |
| [55]   | M. Paniri, M. B. Dowlatshahi, H. Nezamabadi-pour, Ant-TD: Ant colony optimization plus temporal difference reinforcement learning for multi-label feature selection, Swarm and Evolutionary Computation 64 (2021) 100892. doi:10.1016/j.swevo.2021.100892.      |
| [56]   | M. Stanojevi´ c, B. Stanojevi´, M. Vujoˇevi´, Enhanced savings calcula- c s c tion and its applications for solving capacitated vehicle routing problem, Applied Mathematics and Computation 219 (20) (2013) 10302-10312. doi:10.1016/j.amc.2013.04.002.        |
| [57]   | G. Reinelt, TSPLIB-a traveling salesman problem library, ORSA Jour- nal on Computing 3 (4) (1991) 376-384. doi:10.1287/ijoc.3.4.376.                                                                                                                            |

View publication stats

- [58] E. Uchoa, D. Pecin, A. Pessoa, M. Poggi, T. Vidal, A. Subramanian, New benchmark instances for the capacitated vehicle routing problem, European Journal of Operational Research 257 (3) (2017) 845-858. doi:10.1016/j.ejor.2016.08.012.
- [59] B. Rosner, R. J. Glynn, M.-L. T. Lee, The wilcoxon signed rank test for paired comparisons of clustered data, Biometrics 62 (1) (2005) 185-192. doi:10.1111/j.1541-0420.2005.00389.x.
- [60] H. R. Louren¸ co, O. C. Martin, T. Sttzle, Iterated local search: Framework and applications, in: Handbook of Metaheuristics, Springer International Publishing, 2018, pp. 129-168. doi:10.1007/978-3-319-910864 5.
- [61] A. Subramanian, E. Uchoa, L. S. Ochi, A hybrid algorithm for a class of vehicle routing problems, Computers and Operations Research 40 (10) (2013) 2519-2531. doi:10.1016/j.cor.2013.01.013.
- [62] A. A. R. Hosseinabadi, N. S. H. Rostami, M. Kardgar, S. Mirkamali, A. Abraham, A new efficient approach for solving the capacitated vehicle routing problem using the gravitational emulation local search algorithm, Applied Mathematical Modelling 49 (2017) 663-679. doi:10.1016/j.apm.2017.02.042.
- [63] M. L. Mutar, M. Burhanuddin, A. S. Hameed, N. Yusof, H. J. Mutashar, An efficient improvement of ant colony system algorithm for handling capacity vehicle routing problem, International Journal of Industrial Engineering Computations (2020) 549-564doi:10.5267/j.ijiec.2020.4.006.
- [64] L. Bottou, Large-scale machine learning with stochastic gradient descent, in: Proceedings of COMPSTAT 2010, Physica-Verlag HD, 2010, pp. 177-186. doi:10.1007/978-3-7908-2604-3 16.