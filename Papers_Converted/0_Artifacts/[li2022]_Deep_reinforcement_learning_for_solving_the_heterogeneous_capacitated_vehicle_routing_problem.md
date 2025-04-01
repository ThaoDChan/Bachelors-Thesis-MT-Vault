## Deep Reinforcement Learning for Solving the Heterogeneous Capacitated Vehicle Routing Problem

Jingwen Li, Yining Ma, Ruize Gao, Zhiguang Cao , Andrew Lim, Wen Song, Jie Zhang †

Abstract -Existing deep reinforcement learning (DRL) based methods for solving the capacitated vehicle routing problem (CVRP) intrinsically cope with homogeneous vehicle fleet, in which the fleet is assumed as repetitions of a single vehicle. Hence, their key to construct a solution solely lies in the selection of the next node (customer) to visit excluding the selection of vehicle. However, vehicles in real-world scenarios are likely to be heterogeneous with different characteristics that affect their capacity (or travel speed), rendering existing DRL methods less effective. In this paper, we tackle heterogeneous CVRP (HCVRP), where vehicles are mainly characterized by different capacities. We consider both min-max and min-sum objectives for HCVRP, which aim to minimize the longest or total travel time of the vehicle(s) in the fleet. To solve those problems, we propose a DRL method based on the attention mechanism with a vehicle selection decoder accounting for the heterogeneous fleet constraint and a node selection decoder accounting for the route construction, which learns to construct a solution by automatically selecting both a vehicle and a node for this vehicle at each step. Experimental results based on randomly generated instances show that, with desirable generalization to various problem sizes, our method outperforms the state-of-theart DRL method and most of the conventional heuristics, and also delivers competitive performance against the state-of-the-art heuristic method, i.e., SISR. Additionally, the results of extended experiments demonstrate that our method is also able to solve CVRPLib instances with satisfactory performance.

which leads to the heterogeneous CVRP (HCVRP) [1], [2]. According to the objectives, CVRP can also be classified as min-max and min-sum ones, respectively. The former objective requires that the longest (worst-case) travel time (or distance) for a vehicle in the fleet should be as satisfying as possible since fairness is crucial in many real-world applications [3][9], and the latter objective aims to minimize the total travel time (or distance) incurred by the whole fleet [10]-[13]. In this paper, we study the problem of HCVRP with both min-max and min-sum objectives, i.e., MM-HCVRP and MS-HCVRP.

Index Terms -Heterogeneous CVRP, Deep Reinforcement Learning, Min-max Objective, Min-sum Objective.

## I. INTRODUCTION

T HE Capacitated Vehicle Routing Problem (CVRP) is a classical combinatorial optimization problem, which aims to optimize the routes for a fleet of vehicles with capacity constraints to serve a set of customers with demands. Compared with the assumption of multiple identical vehicles in homogeneous CVRP, the settings of vehicles with different capacities (or speeds) are more in line with the real-world practice,

## † Corresponding author.

Jingwen Li and Yining Ma are with the Department of Industrial Systems Engineering and Management, National University of Singapore, Singapore (emails: lijingwen@u.nus.edu, yiningma@u.nus.edu).

Ruize Gao is with the Departmant of Computer Science and Engineering, Chinese Universiry of Hong Kong, China (email: ruizegao@cuhk.edu.hk).

Zhiguang Cao is with the Singapore Institute of Manufacturing Technology, Singapore (email: zhiguangcao@outlook.com).

Andrew Lim is with the School of Computing and Artificial Intelligence, Southwest Jiaotong University, China (email: i@limandrew.org).

Wen Song is with the Institute of Marine Science and Technology, Shandong University, China (email: wensong@email.sdu.edu.cn).

Jie Zhang is with the School of Computer Science and Engineering, Nanyang Technological University, Singapore (email: zhangj@ntu.edu.sg).

Conventional methods for solving HCVRP include exact and heuristic ones. Exact methods usually adopt branch-andbound or its variants as the framework and perform well on small-scale problems [10], [11], [14], [15], but may consume prohibitively long time on large-scale ones given the exponential computation complexity. Heuristic methods usually exploit certain hand-engineered searching rules to guide the solving processes, which often consume much shorter time and are more desirable for large-scale problems in reality [12], [16][18]. However, such hand-engineered rules largely rely on human experience and domain knowledge, thus might be incapable of engendering solutions with high quality. Moreover, both conventional exact and heuristic methods always solve the problem instances independently, and fail to exploit the patterns that potentially shared among the instances.

Recently, researchers tend to apply deep reinforcement learning (DRL) to automatically learn the searching rules in heuristic methods for solving routing problems including CVRP and TSP [19]-[24], by discovering the underlying patterns from a large number of instances. Generally, those DRL models are categorized as two classes, i.e., construction and improvement methods, respectively. Starting with an empty solution, the former constructs a solution by sequentially assigning each customer to a vehicle until all customers are served. Starting with a complete initial solution, the latter selects either candidate nodes (customers or depot) or heuristic operators, or both to improve and update the solution at each step, which are repeated until termination. By further leveraging the advanced deep learning architectures like attention mechanism to guide the selection, those DRL models are able to efficiently generate solutions with much higher quality compared to conventional heuristics. However, existing works only focus on solving homogeneous CVRP which intrinsically cope with vehicles of the same characteristics, in the sense that the complete route of the fleet could be derived by repeatedly dispatching a single vehicle. Consequently, the key in those

works is to solely select the next node to visit excluding the selection of vehicles, since there is only one vehicle essentially. Evidently, those works would be far less effective when applied to solve the more practical HCVRP, given the following issues: 1) The assumption of homogeneous vehicles is unable to capture the discrepancy in vehicles; 2) The vehicle selection is not explicitly considered, which should be of equal importance to the node selection in HCVRP; 3) The contextual information in the attention scheme is insufficient as it lacks states of other vehicles and (partially) constructed routes, which may render it incapable of engendering highquality solutions in view of the complexity of HCVRP.

In this paper, we aim to solve the HCVRP with both minsum and min-max objectives while emphasizing on addressing the aforementioned issues. We propose a novel neural architecture integrated with the attention mechanism to improve the DRL based construction method, which combines the decision-making for vehicle selection and node selection together to engender solutions of higher quality. Different from the existing works that construct the routes for each vehicle of the homogeneous fleet in sequence, our policy network is able to automatically and flexibly select a vehicle from a heterogeneous fleet at each step. In specific, our policy network adopts a Transformer-style [25] encoder-decoder structure, where the decoder consists of two parts, i.e., vehicle selection decoder and node selection decoder, respectively. With the problem features (i.e., customer location, customer demand and vehicle capacity) processed by the encoder for better representation, the policy network first selects a vehicle from the fleet using the vehicle selection decoder based on the states of all vehicles and partial routes, and then selects a node for this vehicle using the node selection decoder at each decoding step. This process is repeated until all customers are served.

Accordingly, the major contribution of this paper is that we present a deep reinforcement learning method to solve CVRP with multiple heterogeneous vehicles, which is intrinsically different from the homogeneous ones in existing works, as the latter is lacking in selecting vehicles from a fleet. Specifically, we propose an effective neural architecture that integrates the vehicle selection and node selection together, with rich contextual information for selection among the heterogeneous vehicles, where every vehicle in the fleet has the chance to be selected at each step. We test both min-max and min-sum objectives with various sizes of vehicles and customers. Results show that our method is superior to most of the conventional heuristics and competitive to the state-of-the-art heuristic (i.e., SISR) with much shorter computation time. With comparable computation time, our method achieves much better solution quality than that of other DRL method. In addition, our method generalizes well to problems with larger customer sizes.

The remainder of the paper is organized as follows. Section II briefly reviews conventional methods and deep models for routing problems. Section III introduces the mathematical formulation of MM-HCVRP and MS-HCVRP and the reformulation in the RL (reinforcement learning) manner. Section IV elaborates our DRL framework. Section V provides the computational experiments and analysis. Finally, Section VI concludes the paper and presents future works.

## II. RELATED WORKS

In this section, we briefly review the conventional methods for solving HCVRP with different objective functions, and deep models for solving the general VRPs.

The heterogeneous CVRP (HCVRP) was first studied in [1], where the Clarke and Wright procedure and partition algorithms were applied to generate the lower bound and estimate optimal solution. An efficient constructive heuristic was adopted to solve HCVRP in [26] by merging small start trips for each customer into a complete one, which was also capable for multi-trip cases. Baldacci and Mingozzi [14] presented a unified exact method to solve HCVRP, reducing the number of variables by using three bounding procedures. Feng et al. [27] proposed a novel evolutionary multitasking algorithm to tackle the HCVRP with time window, and occasional driver, which can also solve multiple optimization tasks simultaneously.

The CVRP with min-sum objective was first proposed by Dantzig and Ramsey [28], which was assumed as the generalization of Travelling Salesman Problem (TSP) with capacity constraints. To address the large-scale multi-objective optimization problem (MOP), a competitive swarm optimizer (CSO) based search method was proposed in [29]-[31], which conceived a new particle updating strategy to improve the search accuracy and efficiency. By transforming the large-scale CVRP (LSCVRP) into a large-scale MOP, an evolutionary multi-objective route grouping method was introduced in [32], which employed a multi-objective evolutionary algorithm to decompose the LSCVRP into small tractable sub-components. The min-max objective was considered in a multiple Travelling Salesman Problem (TSP) [15], which was solved by a tabu search heuristic and two exact search schemes. An ant colony optimization method was proposed to address the min-max Single Depot CVRP (SDCVRP) [33]. The problem was further extended to the min-max multi-depot CVRP [34], which could be reduced to SDCVRP using an equitable region partitioning approach. A swarm intelligence based heuristic algorithm was presented to address the rich min-max CVRP [18]. The minmax cumulative capacitated vehicle routing problem, aiming to minimize the last arrival time at customers, was first studied in [35], [36], where a two-stage adaptive variable neighbourhood search (AVNS) algorithm was introduced and also tested in min-sum objective to verify generalization.

The first deep model for routing problems is Pointer Network, which used supervised learning to solve TSP [37] and was later extended to reinforcement learning [19]. Afterwards, Pointer Network was adopted to solve CVRP in [21], where the Recurrence Neural Network architecture in the encoder was removed to reduce computation complexity without degrading solution quality. To further improve the performance, a Transformer based architecture was incorporated by integrating self-attention in both the encoder and decoder [22]. Different from the above methods which learn constructive heuristics, NeuRewriter was proposed to learn how to pick the next solution in a local search framework [23]. Despite their promising results, these methods are less effective for tackling the heterogeneous fleet in HCVRP. Recently, some learning based methods have been proposed to solve HCVRP. Inspired

by multi-agent RL, Vera and Abad [38] made the first attempt to solve the min-sum HCVRP through cooperative actions of multiple agents for route construction. Qin et al. [39] proposed a reinforcement learning based controller to select among several meta-heuristics with different characteristics to solve min-sum HCVRP. Although yielding better performance than conventional heuristics, they are unable to well handle either the min-max objective or heterogeneous speed of vehicles.

## III. PROBLEM FORMULATION

In this section, we first introduce the mathematical formulation of HCVRP with both min-max and min-sum objectives, and then reformulate it as the form of reinforcement learning.

## A. Mathematical Formulation of HCVRP

Particularly, with n +1 nodes (customers and depot) represented as X = { x i } n i =0 and node x 0 denoting the depot, the customer set is assumed to be X ′ = X \ { x 0 } . Each node x i ∈ R 3 is defined as { ( s , d i i ) } , where s i contains the 2-dim location coordinates of node x i , and d i refers to its demand (the demand for depot is 0). Here, we take heterogeneous vehicles with different capacities into account, which respects the real-world situations. Accordingly, let V = { v i } m i =1 represent the heterogeneous fleet of vehicles, where each element v i is defined as { Q ( i ) } , i.e., the capacity of vehicle v i . The HCVRP problem describes a process that all fully loaded vehicles start from depot, and sequentially visit the locations of customers to satisfy their demands, with the constraints that each customer can be visited exactly once, and the loading amount for a vehicle during a single trip can never exceed its capacity.

Let D x , x ( i j ) be the Euclidean distance between x i and x j . Let y v ij be a binary variable, which equals to 1 if vehicle v travels directly from customer x i to x j , and 0 otherwise. Let l v ij be the remaining capacity of the vehicle v before travelling from customer x i to customer x j . For simplification, we assume that all vehicles have the same speed f , which could be easily extended to take different values. Then, the MM-HCVRP could be naturally defined as follows,

$$\min \, \max _ { v \in V } ( \sum _ { i \in X } \sum _ { j \in X } \frac { D ( x ^ { i }, x ^ { j } ) } { f } y _ { i j } ^ { v } ). \quad \quad ( 1 ) \, \begin{array} { c } \binom { 1 } { c } \\ \text{at set} \\ \text{at set} \end{array}$$

subject to the following six constraints,

$$\sum _ { v \in V } \sum _ { j \in X } y _ { i j } ^ { v } = 1, \quad i \in X ^ { \prime } \quad \quad ( 2 ) \quad \text{vehic} \quad \text{from}$$

$$\sum _ { i \in X } y _ { i j } ^ { v } - \sum _ { k \in X } y _ { j k } ^ { v } = 0, \quad v \in V, j \in X ^ { \prime } \quad \ ( 3 ) \quad \text{is the 1} \\ \text{node s} \quad \ \ } \.$$

$$\sum _ { v \in V } \sum _ { i \in X } l _ { i j } ^ { v } - \sum _ { v \in V } \sum _ { k \in X } l _ { j k } ^ { v } = d ^ { j }, \quad j \in X ^ { \prime } \quad ( 4 ) \, \stackrel { \theta } { \text{represent} } \, \\ \text{represent} \, \\ \text{thead} \, \Phi \, \Phi \, \Phi \, \Phi \, \Phi \, \Phi \, \Phi \, \Phi \, \Phi$$

$$d ^ { j } y _ { i j } ^ { v } \leq l _ { i j } ^ { v } \leq \left ( \mathcal { Q } ^ { v } - d ^ { i } \right ) \cdot y _ { i j } ^ { v }, \quad v \in V, i \in X, j \in X \ ( 5 ) \ \text{ splitting} \text{$\text{$\text{$action}$}$}$$

$$y _ { i j } ^ { v } = \{ 0, 1 \} \,, \quad v \in V, i \in X, j \in X \quad \ \ ( 6 ) \, \underset { \text{specif} } { \ a v e n d }$$

$$l _ { i j } ^ { v } \geq 0, \ d ^ { i } \geq 0, \quad v \in V, i \in X, j \in X. \quad ( 7 )$$

The objective of the formulation is to minimize the maximum travel time among all vehicles. Constraint (2) and (3) ensure that each customer is visited exactly once and each route is completed by the same vehicle. Constraint (4) guarantees that the difference between amount of goods loaded by a vehicle before and after serving a customer equals to the demand of that customer. Constraint (5) enforces that the amount of goods for any vehicle is able to meet the demands of the corresponding customers and never exceed its capacity. Constraint (6) defines the binary variable and constraint (7) imposes the non-negativity of the variables.

The MS-HCVRP shares the same constraints with MMHCVRP, while the objective is formulated as follows,

$$\min \sum _ { v \in V } \sum _ { i \in X } \sum _ { j \in X } \frac { D ( x ^ { i }, x ^ { j } ) } { f ^ { v } } y _ { i j } ^ { v },$$

where f v represents the speed of vehicle v , and it may vary with different vehicles. Thereby, it is actually minimizing the total travel time incurred by the whole fleet.

## B. Reformulation as RL Form

Reinforcement learning (RL) was originally proposed for sequential decision-making problems, such as self-driving cars, robotics, games, etc [40]-[45]. The construction of routes for HCVRP step by step can be also deemed as a sequential decision-making problem. In our work, we model such process as a Markov Decision Process (MDP) [46] defined by 4-tuple M = { S, A, τ, r } (An example of the MDP is illustrated in the supplementary material). Meanwhile, the detailed definition of the state space S , the action space A , the state transition rule τ , and the reward function r are introduced as follows.

State: In our MDP, each state s t = ( V , X t t ) ∈ S is composed of two parts. The first part is the vehicle state V t , which is expressed as V t = { v , v 1 t 2 t , ....v m t } = { ( o , T 1 t 1 t , G 1 t ) , ( o , T 2 t 2 t , G 2 t ) , ..., ( o m t , T m t , G m t ) } , where o i t and T i t represent the remaining capacity and the accumulate travel time of the vehicle v i at step t , respectively. G i t = { g , g i 0 i 1 , ..., g i t } represents the partial route of the vehicle v i at step t , where g i j refers to the node visited by the vehicle v i at step j . Note that the dimension of partial routes (the number of nodes in a route) for all vehicles keeps the same, i.e., if the vehicle v i is selected to serve the node x j at step t , other vehicles still select their last served nodes. Upon departure from the depot (i.e., t = 0 ), the initial vehicle state is set to V 0 = { Q ( 1 , 0 , { 0 } ) , ( Q 2 , 0 , { 0 } ) , ..., ( Q m , 0 , { 0 } } ) where Q i is the maximum capacity of vehicle v i . The second part is the node state S t , which is expressed as X t = { x , x 0 t 1 t , ..., x n t } = { ( s , d 0 0 t ) , ( s , d 1 1 t ) , ..., ( s n , d n t ) } , where s i is a 2-dim vector representing the locations of the node, and d i t is a scalar representing the demand of node i ( d i t will become 0 once that node has been served). Here, we do not consider demand splitting, and only nodes with d i &gt; 0 need to be served.

Action: The action in our method is defined as selecting a vehicle and a node (a customer or the depot) to visit. In specific, the action a t ∈ A is represented as ( v , x i t j t ) , i.e., the

selected node x j will be served (or visited) by the vehicle v i at step t . Note that only one vehicle is selected at each step.

Transition: The transition rule τ will transit the previous state s t to the next state s t +1 based on the performed action a t = ( v , x i t j t ) , i.e., s t +1 = ( V t +1 , X t +1 ) = τ ( V , X t t ) . The elements in vehicle state V t +1 are updated as follows,

$$o _ { t + 1 } ^ { k } = \begin{cases} o _ { t } ^ { k } - d _ { t } ^ { j }, & \text{if $k=i$}, \\ o _ { t } ^ { k }, & \text{otherwise}, \end{cases}$$

$$T _ { t + 1 } ^ { k } = \begin{cases} T _ { t } ^ { k } + \frac { D ( g _ { t } ^ { k }, x ^ { j } ) } { f }, & \text{if $k=i$}, \\ T _ { t } ^ { k }, & \text{otherwise}, \end{cases}$$

$$G _ { t + 1 } ^ { k } = \begin{cases} [ G _ { t } ^ { k }, x ^ { j } ], & \text{if $k=i$,} \\ [ G _ { t } ^ { k }, g _ { t } ^ { k } ], & \text{otherwise,} \end{cases} \quad \text{(11)}$$

where g k t is the last element in G k t , i.e., last visited customer by vehicle v k at step t , and [ · , · , · ] is the concatenation operator. The element in node state X t +1 is updated as follows,

$$d _ { t + 1 } ^ { l } = \begin{cases} 0, & \text{if $l=j$,} \\ d _ { t } ^ { l }, & \text{otherwise}, & \text{the $s$} \\ & \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Psi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi\Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phis \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \ Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi \Phi$$

where each demand will retain 0 after being visited.

Reward: For the MM-HCVRP, to minimize the maximum travel time of all vehicles, the reward is defined as the negative value of this maximum, where the reward is calculated by accumulating the travel time of multiple trips for each vehicle, respectively. Accordingly, the reward is represented as R = -max v ∈ V { ∑ T t =0 r t } , where r t is the incremental travel time for all vehicles at step t . Similarly, for the MS-HCVRP, the reward is defined as the negative value of the total travel time of all vehicles, i.e., R = -∑ m i =1 ∑ T t =1 r t . Particularly, assume that node x j and x k are selected at step t and t +1 , respectively, which are both served by the vehicle v i , then the reward r t +1 is expressed as a m -dim vector as follows,

$$r _ { t + 1 } = r ( s _ { t + 1 }, a _ { t + 1 } ) & = r ( ( V _ { t + 1 }, X _ { t + 1 } ), ( v _ { t + 1 } ^ { i }, x _ { t + 1 } ^ { k } ) ) \\ & = \{ 0, \dots, 0, D ( x ^ { j }, x ^ { k } ) / f, 0, \dots, 0 \}, \quad ( 1 3 ) \quad \text{$\text{$\text{$as ill}$} } \quad \text{$\text{$of\text{ an ei}$} }.$$

where D x , x ( j k ) /f is the time consumed by the vehicle v i for traveling from node x j to x k , with other elements in r s ( t +1 , a t +1 ) equal to 0.

## IV. METHODOLOGY

In this section, we introduce our deep reinforcement learning (DRL) based approach for solving HCVRP with both min-max and min-sum objectives. We first propose a novel attention-based deep neural network to represent the policy, which enables both vehicle selection and node selection at each decision step. Then we describe the procedure for training our policy network.

## A. Framework of Our Policy Network

In our approach, we focus on learning a stochastic policy π θ ( a t | s t ) represented by a deep neural network with trainable parameter θ . Starting from the initial state s 0 , i.e., an empty solution, we follow the policy π θ to construct the solution by complying with the MDP in section III-B until the terminate state s T is reached, i.e., all customers are served by the whole

Fig. 1: The framework of our policy network. With raw features of the instance processed by the encoder, our policy network first selects a vehicle ( v i t ) using the vehicle selection decoder and then a node ( x j t ) using the node selection decoder for this vehicle to visit at each route construction step t . Both the selected vehicle and node constitute the action at that step, i.e., a t = ( v , x i t j t ) , where the partial solution and state are updated accordingly. To a single instance, the encoder is executed once, while the vehicle and node selection decoders are executed multiple times to construct the solution.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000000_61f412ed60e2c8c53221187b6bd5a319aea2e6397496d8ccda5e44a4048bf1bf.png)

fleet of vehicles. The T is possibly longer than n +1 due to the fact that sometimes vehicles need to return to the depot for replenishment. Accordingly, the joint probability of this process is factorized based on the chain rule as follows,

$$p ( s _ { \mathcal { T } } | s _ { 0 } ) = \prod _ { t = 0 } ^ { \mathcal { T } - 1 } \pi _ { \theta } ( a _ { t } | s _ { t } ) p ( s _ { t + 1 } | s _ { t }, a _ { t } ), \quad ( 1 4 )$$

where p s ( t +1 | s , a t t ) = 1 always holds since we adopt the deterministic state transition rule.

As illustrated in Fig. 1, our policy network π θ is composed of an encoder, a vehicle selection decoder and a node selection decoder. Since a given problem instance itself remains unchanged throughout the decision process, the encoder is executed only once at the first step ( t = 0 ) to simplify the computation, while its outputs could be reused in subsequent steps ( t&gt; 0 ) for route construction. To solve the instance, with raw features processed by the encoder for better representation, our policy network first selects a vehicle ( v i ) from the whole fleet via the vehicle selection decoder and identify its index, then selects a node ( x j ) for this vehicle to visit via the node selection decoder at each route construction step. The selected vehicle and node constitute the action for that step, which is further used to update the states. This process is repeated until all customers are served.

## B. Architecture of Our Policy Network

Originating from the field of natural language processing [25], the Transformer model has been successfully extended to many other domains such as image processing [47], [48], recommendation systems [49], [50] and vehicle routing problems [22], [51] due to its desirable capability to handle sequential data. Rather than the sequential recurrent or

Fig. 2: Architecture of our policy network with m heterogeneous vehicles and n customers. It is worth noting that our vehicle selection decoder leverages the vehicle features (last node location and accumulated travel time), the route features (max pooling of the routes for m vehicles), and their combinations to compute the probability of selecting each vehicle.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000001_7a2f1f04db69cf5f2089969434b42316d8ffbcea48cbfe1d9de4a8328b5d994d.png)

convolutional structures, the Transformer mainly hinges on the self-attention mechanism to learn the relations between arbitrary two elements in a sequence, which allows more efficient parallelization and better feature extraction without the limitation of sequence-aligned recurrence. Regarding the general vehicle routing problems, the input is a sequence of customers characterized by locations and demands, and the construction of routes could be deemed as a sequential decision-making, where the Transformer has desirable potential to engender high quality solutions with short computation time. Specially, the Transformer-style models [22], [51] adopt an encoder-decoder structure, where the encoder aims to compute a representation of the input sequence based on the multi-head attention mechanism for better feature extraction and the decoder sequentially outputs a customer at each step based on the problem-related contextual information until all customers are visited. To solve the HCVRP with both min-max and min-sum objectives, we also propose a Transformer-style models as our policy network, which is designed as follows.

problem instance (i.e., customer location, customer demand, and vehicle capacity) into a higher-dimensional space, and then processes them through attention layers for better feature extraction. We normalize the demand d i 0 of customer x i by dividing the capacity of each vehicle to reflect the differences of vehicles in the heterogeneous fleet, i.e., ˜ x i = ( s , d i i 0 / Q 1 , d i 0 / Q 2 , ...d i 0 / Q m ) . Similar to the encoder of Transformer in [22], [25], the enhanced node feature ˜ x i is then linearly projected to h i 0 in a high dimensional space with dimension dim = 128 . Afterwards, h i 0 is further transformed to h i N through N attention layers for better feature representation, each of which consists of a multi-head attention (MHA) sublayer and a feed-forward (FF) sublayer.

As depicted in Fig. 2, our policy network adopts an encoderdecoder structure and the decoder consists of two parts, i.e., vehicle selection decoder and node selection decoder. Based on the stipulation that any vehicle has the opportunity to be selected at each step, our policy network is able to search in a more rational and broader action space given the characteristics of HCVRP. Moreover, we enrich the contextual information for the vehicle selection decoder by adding the features extracted from all vehicles and existing (partial) routes. In doing so, it allows the policy network to capture the heterogeneous roles of vehicles, so that decisions would be made more effectively from a global perspective. To better illustrate our method, an example of two instances with seven nodes and three vehicles is presented in Fig. 3. Next, we introduce the details of our encoder, vehicle selection decoder, and node selection decoder, respectively.

Encoder. The encoder embeds the raw features of a

The l -th MHA sublayer uses a multi-head self-attention network to process the node embeddings h l = ( h , h 0 l 1 l , ..., h n l ) . We stipulate that dim k = dim Y is the query/key dimension, dim v = dim Y is the value dimension, and Y = 8 is the number of heads in the attention. The l -th MHA sublayer first calculates the attention value Z l,y for each head y ∈ { 1 2 , , ..., Y } and then concatenates all these heads and projects them into a new feature space which has the same dimension as the input h l . Concretely, we show these steps as follows,

$$\text{ty} \quad Q _ { l, y } = h _ { l } W ^ { Q } _ { l, y }, \ K _ { l, y } = h _ { l } W ^ { K } _ { l, y }, \ V _ { l, y } = h _ { l } W ^ { V } _ { l, y }, \quad ( 1 5 )$$

$$Z _ { l, y } = \text{softmax} ( \frac { Q _ { l, y } K _ { l, y } ^ { \quad T } } { \sqrt { d i m _ { k } } } ) V _ { l, y }, \quad \quad ( 1 6 )$$

$$\text{MHA} ( h _ { l } ) & = \text{MHA} ( h _ { l } W _ { l } ^ { Q }, h _ { l } W _ { l } ^ { K }, h _ { l } W _ { l } ^ { V } ) \\ & = \text{Concat} ( Z _ { l, 1 }, Z _ { l, 2 }, \dots, Z _ { l, Y } ) W _ { l } ^ { O },$$

where W ,W Q l K l ∈ R Y × dim × dim k , W V l ∈ R Y × dim × dim v , and W O l ∈ R dim × dim are trainable parameters in layer l and are independent across different attention layers.

Afterwards, the output of the l -th MHA sublayer is fed to the l -th FF sublayer with ReLU activation function to get the next embeddings h l +1 . Here, a skip-connection [52] and a

batch normalization (BN) layer [53] are used for both MHA and FF sublayers, which are summarised as follows,

$$r _ { l } ^ { i } = B N ( h _ { l } ^ { i } + M H A ^ { i } ( h _ { l } ) ), \quad \quad ( 1 8 ) \ \ e m b e$$

$$h _ { l + 1 } ^ { i } = B N ( r _ { l } ^ { i } + F F ( r _ { l } ^ { i } ) ). \quad \quad ( 1 9 ) \quad \text{$P$\alpha$}.$$

Finally, we define the final output of the encoder, i.e., h i N , as the node embeddings of the problem instance, and the mean of the node embeddings, i.e., ˆ h N = 1 n ∑ i ∈ X h i N , as the graph embedding of the problem instance, which will be reused for multiple times in the decoders.

Vehicle Selection Decoder. Vehicle selection decoder outputs a probability distribution for selecting a particular vehicle, which mainly leverages two embeddings, i.e., vehicle feature embedding and route feature embedding , respectively.

1) Vehicle Feature Embedding: To capture the states of each vehicle at current step, we define the vehicle feature context C V t ∈ R 1 × 3 m at step t as follows,

$$C _ { t } ^ { V } = [ \tilde { g } _ { t - 1 } ^ { 1 }, T _ { t - 1 } ^ { 1 }, \tilde { g } _ { t - 1 } ^ { 2 }, T _ { t - 1 } ^ { 2 }, \dots, \tilde { g } _ { t - 1 } ^ { m }, T _ { t - 1 } ^ { m } ], \quad ( 2 0 ) \ \text{distrib}$$

where ˜ g i t -1 denotes the 2-dim location of the last node g i t -1 in the partial route of vehicle v i at step t -1 , and T i t -1 is the accumulated travel time of vehicle v i till step t -1 . Afterwards, the vehicle feature context is linearly projected with trainable parameters W 1 and b 1 and further processed by a 512-dim FF layer with ReLU activation function to engender the vehicle feature embedding H V t at step t as follows,

$$H _ { t } ^ { V } = F F ( W _ { 1 } C _ { t } ^ { V } + b _ { 1 } ). \quad \quad ( 2 1 ) \stackrel { w _ { n } } { \text{the c} }$$

2) Route Feature Embedding: Route feature embedding extracts information from existing partial routes of all vehicles, which helps the policy network intrinsically learn from the visited nodes in previous steps, instead of simply masking them as did in previous works [19], [21], [22], [37]. For each vehicle v i at step t , we define its route feature context ˜ C i t as an arrangement of the node embeddimgs (i.e., h k N is the node embeddimgs for node x k ), corresponding to the node in its partial route G i t -1 . Specifically, the route feature context ˜ C i t for each vehicle v i , i = 1 2 , , ..., m is defined as follows,

$$\tilde { C } _ { t } ^ { i } = [ \tilde { h } _ { 0 } ^ { i }, \tilde { h } _ { 1 } ^ { i }, \dots, \tilde { h } _ { t - 1 } ^ { i } ],$$

where ˜ C i t ∈ R t × dim (the first dimension is of size t since G i t -1 should have t elements at step t ) and ˜ h i j represents the corresponding node embeddings in h N of the j -th node in partial route G i t -1 of vehicle v i . For example, assume t = 4 and the partial route of vehicle v i is G i 3 = { x , x 0 3 , x 3 , x 1 } , then the route feature context of this vehicle at step t = 4 would be ˜ C i 4 = [ ˜ h , h i 0 ˜ i 1 , h ˜ i 2 , h ˜ i 3 ] = [ h 0 N , h 3 N , h 3 N , h 1 N ] . Afterwards, the route feature context of all vehicles is aggregated by a max-pooling and then concatenated to yield the route context ˆ C R t for the whole fleet, which is further processed by a linear projection with trainable parameters W 2 and b 2 and a 512-dim FF layer to engender the route feature embedding H R t at step t as follows,

$$\bar { C } _ { t } ^ { i } = m a x ( \tilde { C } _ { t } ^ { i } ), i = 1, 2, \dots, m, \quad \quad ( 2 3 ) \text{ \ \ server} \,.$$

$$\hat { C } _ { t } ^ { R } = [ \bar { C } _ { t } ^ { 1 }, \bar { C } _ { t } ^ { 2 }, \dots, \bar { C } _ { t } ^ { m } ],$$

$$H _ { t } ^ { R } = F F ( W _ { 2 } \hat { C } _ { t } ^ { R } + b _ { 2 } ). \text{ \quad \ \ } ( 2 5 )$$

Finally, the vehicle feature embedding H V t and route feature embedding H R t are concatenated and linearly projected with parameter W 3 and b 3 , which is further processed by a softmax function to compute the probability vector as follows,

$$H _ { t } = W _ { 3 } [ H _ { t } ^ { V }, H _ { t } ^ { R } ] + b _ { 3 }, \text{ \quad \ \ } ( 2 6 )$$

$$p _ { t } = \text{softmax} ( H _ { t } ),$$

where p t ∈ R m and its element p i t represents the probability of selecting vehicle v i at time step t . Depending on different strategies, the vehicle can be selected either by retrieving the maximum probability greedily, or sampling according to the vector p t . The selected vehicle v i is then used as input to the node selection decoder.

Node Selection Decoder. Given node embeddings from the encoder and the selected vehicle v i from the vehicle selection decoder, the node selection decoder outputs a probability distribution ¯ p t over all unserved nodes (the nodes served in previous steps are masked), which is used to identify a node for the selected vehicle to visit. Similar to [22], we first define a context vector H c t as follows, and it consists of the graph embedding ˆ h N , node embedding of the last (previous) node visited by the selected vehicle, and the remaining capacity of this vehicle,

$$H _ { t } ^ { c } = [ \hat { h } _ { N }, \tilde { h } _ { t - 1 } ^ { i }, o _ { t } ^ { i } ],$$

where the second element ˜ h i t -1 has the same meaning as the one defined in Eq. (22) and is replaced with trainable parameters for t = 0 . The designed context vector highlights the features of the selected vehicle at the current decision step, with the consideration of graph embedding of the instance from the global perspective. The context vector H c t and the node embeddings h N are then fed into a multi-head attention (MHA) layer to synthesis a new context vector ˆ H c t as a glimpse of the node embeddings [54]. Different from the selfattention in the encoder, the query of this attention comes from the context vector, while the key/value of the attention come from the node embeddings as shown below,

$$\hat { H } _ { t } ^ { c } = \mathbf M \text{HA} ( H _ { t } ^ { c } W _ { c } ^ { Q }, h _ { N } W _ { c } ^ { K }, h _ { N } W _ { c } ^ { V } ), \quad ( 2 9 )$$

where W ,W Q c K c and W V c are trainable parameters similar to Eq. (17). We then generate the probability distribution ¯ p t by comparing the relationship between the enhanced context ˆ H c t and the node embeddings h N through a Compatibility Layer . The compatibility of all nodes with context at step t is computed as follows,

$$u _ { t } = C \cdot t a n h ( \frac { q _ { t } ^ { T } k _ { t } } { \sqrt { d i m _ { k } } } ), \text{ \quad \ \ } ( 3 0 )$$

where q t = ˆ H W c t Q mp co and k t = h N W K mp co are trainable parameters, and C is set to 10 to control the entropy of u t . Finally, the probability vector is computed in Eq. (31) where all nodes visited in the previous steps are masked for feasibility and element ¯ p j t represents the probability of selecting node x j served by the selected vehicle v i at step t as follows,

$$\bar { p } _ { t } = \text{softmax} ( u _ { t } ).$$

Fig. 3: An illustration of our policy network for two instances with seven nodes and three vehicles, where the red frame indicates the two stacked instances with same data structure. Given the current state s t , the features of nodes and vehicles are processed through the encoder to compute the node embeddings and the graph embedding. In the vehicle selection decoder, the node embeddings of the three tours for three vehicles in current state s t , i.e., {{ h , h 0 1 } { , h , h 0 3 } { , h , h 0 5 }} , are processed for route feature extraction, and the current location and the accumulated travel time of three vehicles are processed for vehicle feature extraction, which are then concatenated to compute the probability of selecting a vehicle. With the selected vehicle v 1 in this example, the current node embedding h 1 and the current loading ability of this vehicle are first concatenated and linearly propagated, then added with the graph embedding, which are further used to compute the probability of selecting a node with masked softmax, i.e., ¯ p 1 = ¯ p 3 = ¯ p 5 = 0 . With the selected node x 2 in this example, the action is represented as a t = { v , x 1 2 } and the state is updated and transited to s t +1 .

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000002_bc1b9b395d164add979b788a3ba18f53b6210ff554b03e54c49e612efd2a2fed.png)

## ALGORITHM 1: Deep Reinforcement Learning Algorithm

input

:

Initial parameters

θ

for policy network

π

θ

;

initial parameters

φ

number of iterations for baseline network

v

I

;

iteration size

N

;

number of batches maximum training steps

foreach

M

;

T

;

significance iter

= 1 2

,

, ..., I

do

Sample

N

problem instances randomly;

foreach

i

= 1 2

,

, ..., M

do

Retrieve batch

b

=

N

foreach

t

= 0 1

,

i

;

, ...,

T

Pick an action

a

Observe reward t,b

r

T

=

end

R

b

-

max

(

∑

r

t,b do

∼

t,b

)

π

θ

(

a

t,b

|

s

t,b

)

and next state

;

t

=0

GreedyRollout with baseline

v

φ

and compute its reward

R

;

1

BL

B

θ

←

b

∑

B b

(

R

=1

Adam

b

-

(

R

θ, d

θ

BL

b

)

;

;

s

t

+1

,b

;

φ

.

;

with baseline to train the policy of vehicle selection and node selection for route construction. The policy gradient are characterized by two networks: 1) the policy network, i.e., the policy network π θ aforementioned, picks an action and generates probability vectors for both vehicles and nodes with respect to this action at each decoding step; 2) the baseline network v φ , a greedy roll-out baseline with a similar structure as the policy network, but computes the reward by always selecting vehicles and nodes with maximum probability. A Monte Carlo method is applied to update the parameters to improve the policy iteratively. At each iteration, we construct routes for each problem instance and calculate the reward with respect to this solution in line 9, and the parameters of the policy network are updated in line 13. In addition, the expected reward of the baseline network R BL b comes from a greedy roll-out of the policy in line 10. The parameters of the baseline network will be replaced by the parameters of the latest policy network if the latter significantly outperforms the former according to a paired t-test on several instances in line 15. By updating the two networks, the policy π θ is improved iteratively towards finding higher-quality solutions.

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

d

θ

end if

ONESIDEDPAIREDTTEST

)

∇

θ

log π

π , v

(

φ

←

θ

;

end

17

end

Similar to the decoding strategy of vehicle selection, the nodes could be selected by always retrieving the maximum ¯ p j t , or sampling according to the vector ¯ p in a less greedy manner.

## C. Training Algorithm

The proposed deep reinforcement learning method is summarized in Algorithm 1, where we adopt the policy gradient

θ

θ

φ

)

←

(

s

T

,b

|

s

0

,b

)

;

&lt;α

then

α

## V. COMPUTATIONAL EXPERIMENTS

In this section, we conduct experiments to evaluate our DRL method. Particularly, a heterogeneous fleet of fully loaded vehicles with different capacities, start at a depot node and depart to satisfy the demands of all customers by following certain routes, the objective of which is to minimize the longest or total travel time incurred by the vehicles. Moreover, we further verify our method by extending the experiments to benchmark instances from the CVRPLib [55]. Note that, the

TABLE I: Parameter Settings of Heuristic Methods.

| SISR   | ¯ c = 10 , L max = 10 , α = 0 . 01 , β = 0 . 01 , T 0 = 100 , T f = 1 , iter = it ( size ) # , it (40) = 1 . 2 × 10 7 , it (60) = 1 . 8 × 10 7 , it (80) = 2 . 4 × 10 7 , it (100) = 3 . 0 × 10 7 , it (120) = 3 . 6 × 10 7 , it (140) = 4 . 2 × 10 7 , it (160) = 4 . 8 × 10 7                     |
|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VNS    | r n = 0 . 1 , r m = 0 . 25 , p ol = 5 , p ot = 5 , p otd = 1 , MaxTolerance = 0 . 04 , iter = it ( size ) ‡ , it (40) = 500 , it (60) = 600 , it (80) = 700 , it (100) = 800 , it (120) = 900 , it (140) = 1000 , it (160) = 1100                                                                   |
| ACO    | m = 20 , ω 0 = 0 . 2 , α = 0 . 1 , γ = 0 . 1 , δ = 3 , β = 3 , Q 0 = N - 1 ∑ N i =0 ∑ N j =0 D ij , iter = it ( size ) ‡ , it (40) = 10 4 , it (60) = 1 . 2 × 10 4 , it (80) = 1 . 4 × 10 4 , it (100) = 1 . 6 × 10 4 , it (120) = 1 . 8 × 10 4 , it (140) = 2 . 0 × 10 4 , it (160) = 2 . 2 × 10 4 |
| FA     | β = 1 , γ = 1 , n = 25 , α = 0 . 001 , iter = it ( size ) ‡ , it (40) = 1000 , it (60) = 1200 , it (80) = 1400 , it (100) = 1600 , it (120) = 1800 , it (40) = 2000 , it (160) = 2200                                                                                                               |

# The iterations are increased as the problem size scales up following the original paper.

‡ The original papers use same iterations for all problem sizes. We linearly increase the iterations as the problem size scales up.

HCVRP with min-max and min-sum objectives are both NPhard problems, and the theoretical computation complexity grows exponentially as problem size scales up.

## A. Experiment Settings for HCVRP

We describe the settings and the data generation method (which we mainly follow the classic ways in [19], [21], [22], [37]) for our experiments. Pertaining to MM-HCVRP, the coordinates of depot and customers are randomly sampled within the unit square [0 , 1] × [0 , 1] using the uniform distribution. The demands of customers are discrete numbers randomly chosen from set { 1 2 , , .., 9 } (demand of depot is 0). To comprehensively verify the performance, we consider two settings of heterogeneous fleets. The first fleet considers three heterogeneous vehicles (named V3), the capacity of which are set to 20, 25, and 30, respectively. The second fleet considers five heterogeneous vehicles (named V5), the capacity of which are set to 20, 25, 30, 35, and 40, respectively. Our method is evaluated with different customer sizes for the two fleets, where we consider 40, 60, 80, 100 and 120 for V3; and 80, 100, 120, 140 and 160 for V5. In MM-HCVRP, we set the vehicle speed f for all vehicles to be 1.0 for simplification. However, our method is capable of coping with different speeds which is verified in MS-HCVRP. Pertaining to MSHCVRP, most of settings are the same as the MM-HCVRP except for the vehicle speeds, which are inversely proportional to their capacities. In doing so, it avoids that only vehicle with the largest capacity is selected to serve all customers to minimize total travel time. Particularly, the speeds are set to 1 4 , 1 5 , and 1 6 for V3, and 1 4 , 1 5 , 1 6 , 1 7 , and 1 8 for V5, respectively.

The hyperparameters are shared to train the policy for all problem sizes. Similar to [22], the training instances are randomly generated on the fly with iteration size of 1,280,000 and are split into 2500 batches for each iteration. Pertaining to the number of iterations, normally more iterations lead to better performance. However, after training with an amount of iterations, if the improvement in the performance is not much significant, we could stop the training before full convergence, which still could deliver competitive performance although not the best. For example, regarding the model of V5-C160 (5 vehicles and 160 customers) with min-max objective trained for 50 iterations, 5 more iterations can only reduce the gap by less than 0.03%, then we will stop the training. In our experiments, we use 50 iterations for all problem sizes to demonstrate the effectiveness of our method, while more iterations could be adopted for better performance in practice. The features of nodes and vehicles are embedded into a 128dimensional space before fed into the vehicle selection and node selection decoder, and we set the dimension of hidden layers in decoder to be 128 [19], [21], [22]. In addition, Adam optimizer is employed to train the policy parameters, with initial learning rate 10 -4 and decaying 0.995 per iteration for convergence. The norm of all gradient vectors are clipped to be within 3.0 and α in Section IV-C is set to 0.05. Each iteration consumes average training time of 31.61m (minutes), 70.52m (with single 2080Ti GPU), 93.02m, 143.62m (with two GPUs) and 170.14m (with three GPUs) for problem size of 40, 60, 80, 100 and 120 regarding V3, and 105.25m, 135.49m, (with two GPUs) 189.15m, 264.45m and 346.52m (with three GPUs) for problem size of 80, 100, 120, 140 and 160 regarding V5. Pertaining to testing, 1,280 instances are randomly generated for each problem size from the uniform distribution, and are fixed for our method and the baselines. Our DRL code in PyTorch is available. 1

## B. Comparison Analysis of HCVRP

For the MM-HCVRP, it is prohibitively time-consuming to find optimal solutions, especially for large problem size. Therefore, we adopt a variety of improved classical heuristic methods as baselines, which include: 1) Slack Induction by String Removals (SISR) [56], a state-of-the-art heuristic method for CVRP and its variants; 2) Variable Neighborhood Search (VNS), a efficient heuristic method for solving the consistent VRP [57]; 3) Ant Colony Optimization (ACO), an improved version of ant colony system for solving HCVRP with time windows [58], where we run the solution construction for all ants in parallel to reduce computation time; 4) Firefly Algorithm (FA), an improved version of standard FA method for solving the heterogeneous fixed fleet vehicle routing problem [59]; 5) the state-of-the-art DRL based attention model (AM) [22], learning a policy of node selection to construct a solution for TSP and CVRP. We adapt the objectives and relevant settings of all baselines so that they share the same one with MM-HCVRP. We have fine tuned the parameters of the conventional heuristic methods using the grid search [60] for adjustable parameters in their original works, such as the number of shifted points in shaking process, the discounting rate of the pheromones and the scale of the population, and report the best ones in Table I. Regarding the iterations, we linearly increase the original ones for VNS, ACO and FA as the problem size scales up for better performance, while the original settings adopt identical iterations across

TABLE II: DRL Method v.s. Baselines for Three Vehicles (V3).

| Method           | V3-C40   | V3-C40   | V3-C40   | V3-C60   | V3-C60   | V3-C60   | V3-C80   | V3-C80   | V3-C80   | V3-C100   | V3-C100   | V3-C100   | V3-C120   | V3-C120   | V3-C120   |
|------------------|----------|----------|----------|----------|----------|----------|----------|----------|----------|-----------|-----------|-----------|-----------|-----------|-----------|
|                  | Obj.     | Gap      | Time     | Obj.     | Gap      | Time     | Obj.     | Gap      | Time     | Obj.      | Gap       | Time      | Obj.      | Gap       | Time      |
| SISR             | 4.00     | 0%       | 245s     | 5.58     | 0%       | 468s     | 7.27     | 0%       | 752s     | 8.89      | 0%        | 1135s     | 10.42     | 0%        | 1657s     |
| VNS              | 4.17     | 4.25%    | 115s     | 5.80     | 3.94%    | 294s     | 7.57     | 4.13%    | 612s     | 9.20      | 3.49%     | 927s      | 10.81     | 3.74%     | 1378s     |
| ACO              | 4.31     | 7.75%    | 209s     | 6.18     | 10.75%   | 317s     | 8.14     | 11.97%   | 601s     | 10.05     | 13.05%    | 878s      | 11.79     | 13.15%    | 1242s     |
| FA               | 4.49     | 12.25%   | 168s     | 6.30     | 12.90%   | 285s     | 8.32     | 14.44%   | 397s     | 10.11     | 13.72%    | 522s      | 11.98     | 14.97%    | 667s      |
| AM(Greedy)       | 4.85     | 21.25%   | 0.37s    | 6.57     | 17.74%   | 0.54s    | 8.32     | 14.44%   | 0.82s    | 9.98      | 12.26%    | 1.07s     | 11.63     | 11.61%    | 1.28s     |
| AM(Sample1280)   | 4.36     | 9.00%    | 0.88s    | 5.99     | 7.39%    | 1.19s    | 7.73     | 6.33%    | 1.81s    | 9.36      | 5.29%     | 2.51s     | 10.94     | 4.99%     | 3.37s     |
| AM(Sample12800)  | 4.31     | 7.75%    | 1.35s    | 5.92     | 6.09%    | 2.46s    | 7.66     | 5.36%    | 3.67s    | 9.28      | 4.39%     | 5.17s     | 10.85     | 4.13%     | 6.93s     |
| DRL(Greedy)      | 4.45     | 11.25%   | 0.70s    | 6.08     | 8.96%    | 0.82s    | 7.82     | 7.57%    | 1.11s    | 9.42      | 5.96%     | 1.44s     | 10.98     | 5.37%     | 1.94s     |
| DRL(Sample1280)  | 4.17     | 4.25%    | 1.25s    | 5.77     | 3.41%    | 1.43s    | 7.48     | 2.89%    | 2.25s    | 9.07      | 2.02%     | 3.42s     | 10.62     | 1.92%     | 4.52s     |
| DRL(Sample12800) | 4.14     | 3.50%    | 1.64s    | 5.73     | 2.69%    | 2.97s    | 7.44     | 2.34%    | 4.56s    | 9.02      | 1.46%     | 6.65s     | 10.56     | 1.34%     | 8.78s     |
| Exact-solver     | 55.43*   | 0%       | 71s      | 78.47*   | 0%       | 214s     | 102.42*  | 0%       | 793s     | 124.61*   | 0%        | 2512s     | -         | -         | -         |
| SISR             | 55.79    | 0.65%    | (254s)   | 79.12    | 0.83%    | (478s)   | 103.41   | 0.97%    | (763s)   | 126.19    | 1.27%     | (1140s)   | 149.10    | 0%        | (1667s)   |
| VNS              | 57.54    | 3.81%    | 109s     | 81.44    | 3.78%    | 291s     | 106.18   | 3.67%    | 547s     | 129.32    | 3.78%     | 828s      | 152.56    | 2.32%     | 1217s     |
| ACO              | 60.11    | 8.44%    | 196s     | 86.05    | 9.66%    | 302s     | 113.75   | 11.06%   | 593s     | 140.61    | 12.84%    | 859s      | 166.50    | 11.67%    | 1189s     |
| FA               | 59.94    | 8.14%    | 164s     | 85.36    | 8.78%    | 272s     | 112.81   | 10.14%   | 388s     | 138.92    | 11.48%    | 518s      | 164.53    | 10.35%    | 653s      |
| AM(Greedy)       | 66.54    | 20.04%   | 0.49s    | 91.19    | 16.21%   | 0.83s    | 117.22   | 14.45%   | 1.01s    | 141.14    | 13.27%    | 1.23s     | 164.57    | 10.38%    | 1.41s     |
| AM(Sample1280)   | 60.95    | 9.96%    | 0.92s    | 85.74    | 9.26%    | 1.17s    | 111.78   | 9.14%    | 1.79s    | 135.61    | 8.83%     | 2.49s     | 159.11    | 6.71%     | 3.30s     |
| AM(Sample12800)  | 60.26    | 8.71%    | 1.35s    | 84.96    | 8,27%    | 2.31s    | 110.94   | 8.32%    | 3.61s    | 134.72    | 8.11%     | 5.19s     | 158.19    | 6.10%     | 6.86s     |
| DRL(Greedy)      | 58.99    | 6.42%    | 0.61s    | 83.06    | 5.85%    | 1.02s    | 108.44   | 5.88%    | 1.11s    | 131.75    | 5.73%     | 1.56s     | 154.56    | 3.66%     | 1.96s     |
| DRL(Sample1280)  | 57.05    | 2.92%    | 1.18s    | 80.46    | 2.54%    | 1.49s    | 105.29   | 2.80%    | 2.34s    | 128.63    | 3.23%     | 3.38s     | 151.23    | 1.43%     | 4.61s     |
| DRL(Sample12800) | 56.84    | 2.54%    | 1.65s    | 79.92    | 1.85%    | 2.99s    | 104.63   | 2.16%    | 4.63s    | 128.19    | 2.87%     | 6.74s     | 150.73    | 1.09%     | 9.11s     |

() The mark () indicates that the time is computed based on JAVA implementation which is publicly available. For VNS, ACO and FA, we re-implement them in Python since their original codes are not available, where C++ or JAVA was used in the original papers. So the reported time might be different from the ones in the original papers. *

The mark * indicates that all instances are solved optimally.

TABLE III: DRL Method v.s. Baselines for Five Vehicles (V5).

|         | Method           | Obj.    | V5-C80 Gap   | Time   | Obj.    | V5-C100 Gap   | Time    | Obj.   | V5-C120 Gap   | Time    | Obj.   | V5-C140 Gap   | Time    | Obj.   | V5-C160 Gap   | Time    |
|---------|------------------|---------|--------------|--------|---------|---------------|---------|--------|---------------|---------|--------|---------------|---------|--------|---------------|---------|
| Min-max | SISR             | 3.90    | 0% (727s)    |        | 4.72    | 0%            | (1091s) | 5.48   | 0%            | (1572s) | 6.33   | 0%            | (1863s) | 7.16   | 0%            | (2521s) |
| Min-max | VNS              | 4.15    | 6.41%        | 725s   | 4.98    | 7.19%         | 1046s   | 5.81   | 6.02%         | 1454s   | 6.67   | 5.37%         | 2213s   | 7.53   | 5.17%         | 3321s   |
| Min-max | ACO              | 4.50    | 15.38%       | 612s   | 5.56    | 17.80%        | 890s    | 6.47   | 18.07%        | 1285s   | 7.52   | 18.80%        | 2081s   | 8.51   | 18.85%        | 2898s   |
| Min-max | FA               | 4.61    | 18.21%       | 412s   | 5.62    | 19.07%        | 541s    | 6.58   | 20.07%        | 682s    | 7.60   | 20.06%        | 822s    | 8.64   | 20.67%        | 964s    |
| Min-max | AM(Greedy)       | 4.84    | 24.10%       | 1.08s  | 5.70    | 20.76%        | 1.31s   | 6.57   | 19.89%        | 1.74s   | 7.49   | 18.33%        | 1.93s   | 8.34   | 16.48%        | 2.15s   |
| Min-max | AM(Sample1280)   | 4.32    | 10.77%       | 1.88s  | 5.18    | 8.75%         | 2.64s   | 6.03   | 10.04%        | 3.38s   | 6.93   | 9.48%         | 4.47s   | 7.75   | 8.24%         | 5.73s   |
| Min-max | AM(Sample12800)  | 4.25    | 8.97%        | 3.71s  | 5.11    | 8.26%         | 5.19s   | 5.95   | 8.58%         | 6.94s   | 6.86   | 8.37%         | 8.73s   | 7.69   | 7.40%         | 10.69s  |
| Min-max | DRL(Greedy)      | 4.36    | 11.79%       | 1.29s  | 5.20    | 10.17%        | 1.64s   | 5.94   | 8.39%         | 2.38s   | 6.78   | 7.11%         | 2.43s   | 7.61   | 6.28%         | 3.02s   |
| Min-max | DRL(Sample1280)  | 4.08    | 4.62%        | 2.66s  | 4.91    | 4.03%         | 3.66s   | 5.66   | 3.28%         | 5.08s   | 6.51   | 2.84%         | 6.48s   | 7.34   | 2.51%         | 8.52s   |
| Min-max | DRL(Sample12800) | 4.04    | 3.59%        | 5.06s  | 4.87    | 3.18%         | 7.20s   | 5.62   | 2.55%         | 9.65s   | 6.47   | 2.21%         | 10.93s  | 7.30   | 1.96%         | 13.76s  |
| Min-sum | Exact-solver     | 102.42* | 0%           | 1787s  | 124.63* | 0%            | 6085s   | -      | -             | -       | -      | -             | -       | -      | -             | -       |
| Min-sum | SISR             | 103.49  | 1.04%        | (735s) | 126.35  | 1.38%         | (1107s) | 149.18 | 0%            | (1580s) | 172.88 | 0%            | (1881s) | 196.51 | 0%            | (2539s) |
| Min-sum | VNS              | 109.91  | 7.31%        | 538s   | 133.28  | 6.94%         | 811s    | 156.37 | 4.82%         | 1386s   | 180.08 | 4.16%         | 2080s   | 203.95 | 3.79%         | 2896s   |
| Min-sum | ACO              | 118.58  | 15.78%       | 608s   | 146.51  | 17.56%        | 865s    | 171.82 | 15.18%        | 1269s   | 200.73 | 16.11%        | 1922s   | 229.64 | 16.86%        | 2803s   |
| Min-sum | FA               | 116.13  | 13.39%       | 401s   | 142.39  | 14.25%        | 532s    | 167.87 | 12.53%        | 677s    | 196.48 | 13.65%        | 801s    | 223.49 | 13.73%        | 955s    |
| Min-sum | AM(Greedy)       | 128.31  | 25.28%       | 0.82s  | 152.91  | 22.69%        | 1.28s   | 177.39 | 18.91%        | 1.45s   | 201.85 | 16.76%        | 1.69s   | 227.10 | 15.57%        | 1.81s   |
| Min-sum | AM(Sample1280)   | 119.41  | 16.59%       | 1.83s  | 144.23  | 15.73%        | 2.66s   | 168.95 | 13.25%        | 3.63s   | 193.65 | 12.01%        | 4.68s   | 218.67 | 11.28%        | 5.49s   |
| Min-sum | AM(Sample12800)  | 118.04  | 15.25%       | 3.74s  | 142.79  | 14.57%        | 5.20s   | 167.45 | 12.25%        | 7.02s   | 192.13 | 11.13%        | 8.93s   | 217.14 | 10.50%        | 11.01s  |
| Min-sum | DRL(Greedy)      | 108.43  | 5.87%        | 1.26s  | 131.90  | 5.83%         | 1.73s   | 154.71 | 3.71%         | 2.11s   | 178.78 | 3.41%         | 3.06s   | 202.87 | 3.24%         | 3.60s   |
| Min-sum | DRL(Sample1280)  | 105.54  | 3.05%        | 2.70s  | 128.63  | 3.21%         | 4.15s   | 151.39 | 1.48%         | 5.37s   | 175.29 | 1.39%         | 6.83s   | 199.16 | 1.35%         | 8.68s   |
|         | DRL(Sample12800) | 104.88  | 2.40%        | 5.38s  | 128.17  | 2.84%         | 7.61s   | 150.86 | 1.26%         | 9.24s   | 174.80 | 1.11%         | 11.30s  | 198.66 | 1.09%         | 13.83s  |

all problem sizes. For SISR, we follow its original setting, where the iterations are increased as the problem size grows up. To fairly compare with AM, we tentatively leverage two external strategies to select a vehicle at each decoding step for AM, i.e., by turns and randomly , since it did not cope with vehicle selection originally. The results indicate that vehicle selection by turns is better for AM, which is thereby adopted for both min-max and min-sum objectives in our experiments. Note that, we do not compare with OR-Tools as there is no built-in library or function that can directly solve MMHCVRP. Moreover, we do not compare with Gurobi or CPLEX either, as our experience shows that they consume days to optimally solve a MM-HCVRP instance even with 3 vehicles and 15 customers. For the MS-HCVRP, with the same three heuristic baselines and AM as used for the MM-HCVRP, we additionally adopted a generic exact solver for solving vehicle routing problems with min-sum objective [61]. The baselines including VNS, ACO, and FA are implemented in Python. For SISR, we adopt a publicly available version 2 implemented in

JAVA. Note that, the running efficiency of the same algorithm implemented in C++, JAVA, and Python could be considerably different, which will also be analyzed later for running time comparison 3 . All these baselines are executed on CPU servers equipped with the Intel i9-10940X CPU at 3.30 GHz. For those which consume much longer running time, we deploy them on multiple identical servers.

Regarding our DRL method and AM, we apply two types of decoders during testing: 1) Greedy, which always selects the vehicle and node with the maximum probability at each decoding step; 2) Sampling, which engenders N solutions by sampling according to the probability computed in Eq. (27) and Eq. (31), and then retrieves the best one. We set N to 1280 and 12800, and term them as Sample1280 and Sample12800, respectively. Then we record the performance of our methods and baselines on all sizes of MM-HCVRP and MS-HCVRP instances for both three vehicles and five vehicles in Table II

3 A program implemented in C/C++ might be 20-50 times faster than that of Python, especially for large-scale problem instances. The running efficiency of Java could be comparable to C/C++ with highly optimized coding but could be slightly slower in general.

Fig. 4: The converge curve of DRL method and SISR (V3).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000003_999b6edb32f677593336397f59e6e5c41b81a164d7683f6846f88fd277c85740.png)

Fig. 5: The converge curve of DRL method and SISR (V5).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000004_2c398ede0b7eaa30ad4be7d6602bba069509823b6834d3eb8695b605c4a9a8fa.png)

and Table III, respectively, which include average objective value, gap and computation time of an instance. Given the fact that it is prohibitively time-consuming to optimally solve MMHCVRP, the gap here is calculated by comparing the objective value of a method with the best one found among all methods.

From Table II, we can observe that for the MS-HCVRP with three vehicles, the Exact-solver achieves the smallest objective value and gap and consumes shorter computation time than heuristic methods on V3-C40 and V3-C60. However, its computation time grows exponentially as the problem size scales up. We did not show the results of the Exact-solver for solving instances with more than 100 customers regarding both three vehicles and five vehicles, which consumes prohibitively long time. Among the three variants of DRL method, our DRL(Greedy) outperforms FA and AM(Greedy) in terms of objective values and gap. Although with slightly longer computation time, both Sample1280 and Sample12800 achieve smaller objective values and gaps than Greedy, which demonstrates the effectiveness of sampling strategy in improving the solution quality. Specifically, our DRL(Sample1280) can outstrip all AM variants and ACO. It also outperforms VNS in most cases except for V3-C40, where our DRL(Sample1280) achieves the same gap with VNS. With Sample12800, our DRL further outperforms VNS in terms of objective value and gap and is slightly inferior to the state-of-the-art heuristic, i.e., SISR and the Exact-solver. Pertaining to the running efficiency, although the computation time of our DRL method is slightly longer than that of AM, it is significantly shorter (at least an order of magnitude faster) than that of conventional methods, even if we eliminate the impact of different programming language via roughly dividing the reported running time by a constant (e.g., 30), especially for large problem sizes. Regarding the MM-HCVRP, similarly, our DRL method outperforms VNS, ACO, FA and all AM variants, which performs slightly inferior to SISR but consumes much shorter running time. Among all heuristic and learning based methods, the state-of-the-art method SISR achieves lowest objective value and gap, however, the computation time of SISR grows almost exponentially as the problem scale increases and our DRL(Sample12800) grows almost linearly, which is more obvious in large-scale problem sizes.

In Table III, similar patterns could be observed in comparison with that of three vehicles, where the superiority of DRL(Sample12800) to VNS, ACO, FA and AM becomes more obvious. Meanwhile, our DRL method is still competitive to the state-of-the-art method, i.e., SISR, on larger problem sizes in comparison with Table II. Combining both Tables, our DRL method with Sample12800 achieves better overall performance than conventional heuristics and AM on both the MM-HCVRP and MS-HCVRP, and also performs competitively well against SISR, with satisfactory computation time.

To further investigate the efficiency of our DRL method against SISR, we evaluate their performance with a bounded time budget, i.e., 500 seconds. It is much longer than the computation time of our method, given that our DRL computes a solution in a construction fashion rather than the improvement fashion as in SISR. In Fig. 4, we record the performance of our DRL method and SISR for MS-HCVRP with three vehicles on the same instances in Table II and Table III, where the horizontal coordinate refers to the computation time and the vertical one refers to the objective value. We depict the computation time of our DRL method using hollow circle, and then horizontally extend it for better comparison. We plot the curve of SISR over time since it improves an initial yet complete solution iteratively. We also record the time when SISR achieves same objective value as our method using filled circle. We can observe that SISR needs longer computation time to catch up the DRL method as the problems size scales up. When the computation time reaches 500 seconds for SISR, our DRL method achieves only slightly inferior objective values with much shorter computation time. For example, the DRL method only needs 9.1 seconds for solving a V3C120 instance, while SISR needs about 453 seconds to achieve the same objective value. In Fig. 5, we record the results of our DRL method and SISR for five vehicles also with 500 seconds. Similar patterns could be observed to that of three vehicles, where the superiority of our DRL method is more obvious, especially for large-scale problem sizes. For example, on V5-C140 and V5-C160, our DRL method with 11.3 and 13.84 seconds even outperforms the SISR with 500 seconds, respectively. Combining all results above, we can conclude that with a relatively short time limit, our DRL method tends to achieve better performance than the state-of-the-art method, i.e., SISR, and the superiority of our DRL method is more obvious for larger-scale problem sizes. Even with time limit

Fig. 6: Generalization performance for MM-HCVRP (V3).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000005_41373e8491fbd2521880c6e7c12b08827234054b4f53d9cd5e0205254f4cf027.png)

Fig. 7: Generalization performance for MS-HCVRP (V3).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000006_ce1a9b9d7428eb55058667a8104bdbb6cda641ce8aa6d38a9b08cb45e00edbe2.png)

much longer than 500 seconds, our DRL method still achieves competitive performance against SISR.

## C. Generalization Analysis of HCVRP

To verify the generalization of our method, we conduct experiments to apply the policy learnt for a customer size to larger ones since generalizing to larger customer sizes is more meaningful in real-life situations, where we mainly focus on Sample12800 in our method.

In Fig. 6, we record the results of the MM-HCVRP for the fleet of three vehicles, where the horizontal coordinate refers to the problems to be solved, and the vertical one refers to the average objective values of different methods. We observe that for each customer size, the corresponding policy achieves the smallest objective values in comparison with those learnt for other sizes. However, they still outperform AM and those classical heuristic methods except for V3-C40 in solving problem sizes larger than or equal to 80, where V3-C40 is comparable with the best performed baseline (i.e., VNS), if we refer to Table II. Moreover, we also notice that most of the policies learnt for proximal customer sizes tend to perform better than that of more different ones, e.g., the policies for V3-C80 and V3-C100 perform better than that of V3-C40 and V3-C60 in solving V3-C120. The rationales behind this observation might be that proximal customers sizes may lead to similar distributions of customer locations. In Fig. 7, we record the results of the MS-HCVRP for the fleet of three vehicles, where similar patterns could be found in comparison with that of the MM-HCVRP, and the policies learnt for other customer sizes outperform all the classical heuristic methods and AM.

Fig. 8: Generalization performance for MM-HCVRP (V5).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000007_c3f96cabe8b8de55cd04a4f1efbea51888ba85df3c3a34edf30a9afab7132789.png)

Fig. 9: Generalization performance for MS-HCVRP (V5).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000008_9ef74a5d93f59f5ed4c688aac24eed1491f475cf7d8a2445bd5787f03cdd1fe0.png)

In Fig. 8 and Fig. 9, we record the generalization performance of the MM-HCVRP and MS-HCVRP for five vehicles, respectively. Similar patterns to the three vehicles could be observed, where for each customer size, the corresponding policy achieves smaller objective value in comparison with those learnt for other sizes. However, they still outperform most classical heuristic methods and AM in all cases, and are only slightly inferior to the corresponding policies.

TABLE IV: Our Method v.s. Baselines on CVRPLib.

| Dist.       | Instance   | Opt.   | Ours   | VNS   | SISR   |
|-------------|------------|--------|--------|-------|--------|
| Uniform     | P-n60-k10  | -      | 306    | 308   | 293    |
| Uniform     | A-n61-k9   | -      | 319    | 307   | 299    |
| Uniform     | E-n76-k7   | -      | 372    | 375   | 362    |
| Uniform     | A-n80-k10  | -      | 795    | 813   | 776    |
| Uniform     | E-n101-k8  | -      | 446    | 455   | 428    |
| Uniform     | Avg. Gap   | -      | 4.11%  | 4.49% | 0%     |
| Non-Uniform | B-n41-k6   | -      | 385    | 371   | 359    |
| Non-Uniform | B-n51-k7   | -      | 392    | 378   | 369    |
| Non-Uniform | B-n63-k10  | -      | 564    | 558   | 540    |
| Non-Uniform | M-n101-k10 | -      | 419    | 401   | 391    |
| Non-Uniform | CMT11 1    | -      | 878    | 869   | 858    |
| Non-Uniform | Avg. Gap   | -      | 5.48%  | 2.59% | 0%     |
| Uniform     | P-n60-k10  | 4009*  | 4045   | 4265  | 4013   |
| Uniform     | A-n61-k9   | 3984*  | 4041   | 4252  | 3995   |
| Uniform     | E-n76-k7   | 4740*  | 5035   | 5222  | 4947   |
| Uniform     | A-n80-k10  | 11149* | 11454  | 11466 | 11186  |
| Uniform     | E-n101-k8  | 5653*  | 5972   | 6114  | 5727   |
| Uniform     | Avg. Gap   | 0%     | 2.08%  | 5.51% | 1.28%  |
| Non-Uniform | B-n41-k6   | 4948*  | 5327   | 5015  | 4948   |
| Non-Uniform | B-n51-k7   | 5235*  | 5434   | 5363  | 5236   |
| Non-Uniform | B-n63-k10  | 7706*  | 7805   | 7825  | 7727   |
| Non-Uniform | M-n101-k10 | 5443*  | 5707   | 5687  | 5507   |
| Non-Uniform | CMT11 1    | -      | 12526  | 12183 | 11910  |
| Non-Uniform | Avg. Gap   | 0%     | 4.55%  | 2.42% | 0.29%  |

1 CMT11 has 1 depot and 120 customers (121 nodes).

## D. Discussion

To comprehensively evaluate the performance of our DRL method, we further apply our trained model to solve the instances randomly selected from the well-known CVRPLib 4 benchmark, half of which follow uniform distribution regarding the customer locations, and the remaining half do not.

In Table IV, we record the comparison results on CVRPlib, where the Exact-solver is used to optimally solve the MSHCVRP. Regarding the DRL method, we directly exploit the trained models as in Table II and Table III to solve the CVRPLib instances, where the model with the closest size to the instances is adopted. For example, we use the model trained for V3-C60 to solve B-n63-k10. We select SISR and VNS as baselines for both MM-HCVRP and MS-HCVRP, which perform better than other heuristics in previous experiments. Each reported objective value is averaged over 10 independent runs with different random seeds. Note that it is prohibitively long for the Exact-solver to solve MS-HCVRP with more than 100 customers (i.e., CMT11). From Table IV, we can observe that our DRL method tends to perform better than VNS on uniformly distributed instances, and slightly inferior on the instances of non-uniform distribution for both MMHCVRP and MS-HCVRP. Although inferior to SISR, our DRL method is able to engender solutions of comparable solution quality with much shorter computation time. For example, SISR consumes 1598 seconds to solve the CMT11, while our DRL method only needs 9.0 seconds. We also notice that, our DRL method tends to perform better on uniform distributed instances than that of non-uniform ones if we refer to the gap between our method and the exact method for MS-HCVRP and the SISR for MM-HCVRP.

This observation about different distributions indeed makes sense especially given that as described in Section V-A, the customer locations in all training instances follow the uniform distributions, the setting of which is widely adopted in this line of research (e.g., [19], [21]-[23], [51]). Since our DRL model is a learning method in nature, it does have favorable potential to deliver superior performance when both the training and testing instances are from the same (or similar) uniform distribution. It also explains why our DRL method outperform most of the conventional heuristic methods in Table II and Table III. When it comes to non-uniform distribution for testing, this superiority does not necessarily preserve, as indicated by the results in Table IV. However, it is a fundamental out-ofdistribution challenge to all learning methods, including our DRL method. The purpose of Table IV is to reveal when our DRL method may perform inferior to others. Considering that addressing the out-of-distribution challenge is not in the scope of this paper, we will investigate it in future.

## VI. CONCLUSION AND FUTURE WORK

In this paper, we cope with the heterogeneous CVRP for both min-max and min-sum objectives. To solve this problem,

4 CVRPLib (http://vrp.atd-lab.inf.puc-rio.br/index.php/en/) is a well-known online benchmark repository of VRP instances used for algorithm comparison in VRP literature. More details of CVRPLib can be found in [55]. In our experiment, we select 10 instances and adapt them to our MM-HCVRP and MS-HCVRP settings by adopting their customer locations and demands.

we propose a learning based constructive heuristic method, which integrates deep reinforcement learning and attention mechanism to learn a policy for the route construction. In specific, the policy network leverages an encoder, a vehicle selection decoder and a node selection decoder to pick a vehicle and a node for this vehicle at each step. Experimental results show that the overall performance of our method is superior to most of the conventional heuristics and competitive to the state-of-the-art heuristic method, i.e., SISR with much shorter computation time. With comparable computation time, our method also significantly outperforms the other learning based method. Moreover, the proposed method generalizes well to problems with larger number of customers for both MM-HCVRP and MS-HCVRP.

One major purpose of our work is to nourish the development of deep reinforcement learning (DRL) based methods for solving the vehicle routing problems, which have emerged lately. Following the same setting adopted in this line of works [19], [21]-[23], [37], we randomly generate the locations within a square of [0,1] for training and testing. The proposed method works well for HCVRP with both min-max and min-sum objectives, but may perform inferior for other types of VRPs, such as VRP with time window constraint and dynamic customer requests. Taking into account the above concerns and other potential limitations that our method may have, in future, we will consider and study the following aspects, 1) time window constraint, and dynamic customer requests or stochastic traffic conditions; 2) generalization to different number of vehicles; 3) evaluation with other classical or realistic benchmark datasets with instances of different distributions (e.g., http://mistic.heigvd.ch/taillard/problemes.dir/vrp.dir/vrp.html); and 4) improvement over SISR by integrating with active search [19] or other improvement approaches (e.g., [62]).

## VII. ACKNOWLEDGEMENT

This work is supported by the National Natural Science Foundation of China (Grant No. 61803104, 62102228), and Young Scholar Future Plan of Shandong University (Grant No. 62420089964188).

## REFERENCES

- [1] B. Golden, A. Assad, L. Levy, and F. Gheysens, 'The fleet size and mix vehicle routing problem,' Computers &amp; Operations Research , vol. 11, no. 1, pp. 49-66, 1984.
- [2] C ¸ . Koc ¸, T. Bektas ¸, O. Jabali, and G. Laporte, 'A hybrid evolutionary algorithm for heterogeneous fleet vehicle routing problems with time windows,' Computers &amp; Operations Research , vol. 64, pp. 11-27, 2015.
- [3] X. Wang, B. Golden, E. Wasil, and R. Zhang, 'The min-max split delivery multi-depot vehicle routing problem with minimum service time requirement,' Computers &amp; Operations Research , vol. 71, pp. 110-126, 2016.
- [4] S. Duran, M. A. Gutierrez, and P. Keskinocak, 'Pre-positioning of emergency items for care international,' Interfaces , vol. 41, no. 3, pp. 223-237, 2011.
- [5] L. Bertazzi, B. Golden, and X. Wang, 'Min-max vs. min-sum vehicle routing: A worst-case analysis,' European Journal of Operational Research , vol. 240, no. 2, pp. 372-381, 2015.
- [6] X. Ma, Y. Song, and J. Huang, 'Min-max robust optimization for the wounded transfer problem in large-scale emergencies,' in Chinese Control and Decision Conference , pp. 901-904, 2010.

- [7] C. Alabas-Uslu, 'A self-tuning heuristic for a multi-objective vehicle routing problem,' Journal of the Operational Research Society , vol. 59, no. 7, pp. 988-996, 2008.
- [8] E. K. Hashi, M. R. Hasan, and M. S. U. Zaman, 'Gis based heuristic solution of the vehicle routing problem to optimize the school bus routing and scheduling,' in International Conference on Computer and Information Technology (ICCIT) , pp. 56-60, 2016.
- [9] X. Liu, H. Qi, and Y. Chen, 'Optimization of special vehicle routing problem based on ant colony system,' in International Conference on Intelligent Computing , pp. 1228-1233, 2006.
- [10] M. Haimovich and A. H. Rinnooy Kan, 'Bounds and heuristics for capacitated routing problems,' Mathematics of Operations Research , vol. 10, no. 4, pp. 527-542, 1985.
- [11] J. Lysgaard, A. N. Letchford, and R. W. Eglese, 'A new branch-and-cut algorithm for the capacitated vehicle routing problem,' Mathematical Programming , vol. 100, no. 2, pp. 423-445, 2004.
- [12] W. Y. Szeto, Y. Wu, and S. C. Ho, 'An artificial bee colony algorithm for the capacitated vehicle routing problem,' European Journal of Operational Research , vol. 215, no. 1, pp. 126-135, 2011.
- [13] X. Wang, S. Poikonen, and B. Golden, 'The vehicle routing problem with drones: Several worst-case results,' Optimization Letters , vol. 11, no. 4, pp. 679-697, 2017.
- [14] R. Baldacci and A. Mingozzi, 'A unified exact method for solving different classes of vehicle routing problems,' Mathematical Programming , vol. 120, no. 2, pp. 347-380, 2009.
- [15] P. M. Franc ¸a, M. Gendreau, G. Laporte, and F. M. M¨ uller, 'The mtraveling salesman problem with minmax objective,' Transportation Science , vol. 29, no. 3, pp. 267-275, 1995.
- [16] N. Mostafa and A. Eltawil, 'Solving the heterogeneous capacitated vehicle routing problem using k-means clustering and valid inequalities,' in Proceedings of the International Conference on Industrial Engineering and Operations Management , 2017.
- [17] X. Li, P. Tian, and Y. Aneja, 'An adaptive memory programming metaheuristic for the heterogeneous fixed fleet vehicle routing problem,' Transportation Research Part E: Logistics and Transportation Review , vol. 46, no. 6, pp. 1111-1127, 2010.
- [18] E. Yakıcı, 'A heuristic approach for solving a rich min-max vehicle routing problem with mixed fleet and mixed demand,' Computers &amp; Industrial Engineering , vol. 109, pp. 288-294, 2017.
- [19] I. Bello, H. Pham, Q. V. Le, M. Norouzi, and S. Bengio, 'Neural combinatorial optimization with reinforcement learning,' in International Conference on Learning Representations , 2017.
- [20] L. Xin, W. Song, Z. Cao, and J. Zhang, 'Multi-decoder attention model with embedding glimpse for solving vehicle routing problems,' in Proceedings of 35th AAAI Conference on Artificial Intelligence , 2021.
- [21] M. Nazari, A. Oroojlooy, L. Snyder, and M. Tak´ ac, 'Reinforcement learning for solving the vehicle routing problem,' in Advances in Neural Information Processing Systems , pp. 9839-9849, 2018.
- [22] W. Kool, H. van Hoof, and M. Welling, 'Attention, learn to solve routing problems!,' in International Conference on Learning Representations , 2018.
- [23] X. Chen and Y. Tian, 'Learning to perform local rewriting for combinatorial optimization,' in Advances in Neural Information Processing Systems , pp. 6278-6289, 2019.
- [24] J. Li, L. Xin, Z. Cao, A. Lim, W. Song, and J. Zhang, 'Heterogeneous attentions for solving pickup and delivery problem via deep reinforcement learning,' IEEE Transactions on Intelligent Transportation Systems , 2021.
- [25] A. Vaswani, N. Shazeer, N. Parmar, J. Uszkoreit, L. Jones, A. N. Gomez, Ł. Kaiser, and I. Polosukhin, 'Attention is all you need,' in Advances in Neural Information Processing Systems , pp. 5998-6008, 2017.
- [26] C. Prins, 'Efficient heuristics for the heterogeneous fleet multitrip vrp with application to a large-scale real case,' Journal of Mathematical Modelling and Algorithms , vol. 1, no. 2, pp. 135-150, 2002.
- [27] L. Feng, L. Zhou, A. Gupta, J. Zhong, Z. Zhu, K.-C. Tan, and K. Qin, 'Solving generalized vehicle routing problem with occasional drivers via evolutionary multitasking,' IEEE Transactions on Cybernetics , pp. 1 14, 2019.
- [28] G. B. Dantzig and J. H. Ramser, 'The truck dispatching problem,' Management Science , vol. 6, no. 1, pp. 80-91, 1959.
- [29] Y. Tian, X. Zheng, X. Zhang, and Y. Jin, 'Efficient large-scale multiobjective optimization based on a competitive swarm optimizer,' IEEE Transactions on Cybernetics , vol. 50, no. 8, pp. 3696 - 3708, 2019.
- [30] R. Cheng and Y. Jin, 'A competitive swarm optimizer for large scale optimization,' IEEE Transactions on Cybernetics , vol. 45, no. 2, pp. 191204, 2014.
- [31] R. Cheng, Y. Jin, M. Olhofer, et al. , 'Test problems for large-scale multiobjective and many-objective optimization,' IEEE Transactions on Cybernetics , vol. 47, no. 12, pp. 4108-4121, 2016.
- [32] J. Xiao, T. Zhang, J. Du, and X. Zhang, 'An evolutionary multiobjective route grouping-based heuristic algorithm for large-scale capacitated vehicle routing problems,' IEEE Transactions on Cybernetics , pp. 1 14, 2019.
- [33] K. S. V. Narasimha and M. Kumar, 'Ant colony optimization technique to solve the min-max single depot vehicle routing problem,' in Proceedings of the American Control Conference , pp. 3257-3262, 2011.
- [34] K. V. Narasimha, E. Kivelevitch, B. Sharma, and M. Kumar, 'An ant colony optimization technique for solving min-max multi-depot vehicle routing problem,' Swarm and Evolutionary Computation , vol. 13, pp. 63-73, 2013.
- [35] J. F. Sze, S. Salhi, and N. Wassan, 'The cumulative capacitated vehicle routing problem with min-sum and min-max objectives: An effective hybridisation of adaptive variable neighbourhood search and large neighbourhood search,' Transportation Research Part B: Methodological , vol. 101, pp. 162-184, 2017.
- [36] B. L. Golden, G. Laporte, and E. D. Taillard, ´ 'An adaptive memory heuristic for a class of vehicle routing problems with minmax objective,' Computers &amp; Operations Research , vol. 24, no. 5, pp. 445-452, 1997.
- [37] O. Vinyals, M. Fortunato, and N. Jaitly, 'Pointer networks,' in Advances in Neural Information Processing Systems , pp. 2692-2700, 2015.
- [38] J. M. Vera and A. G. Abad, 'Deep reinforcement learning for routing a heterogeneous fleet of vehicles,' in IEEE Latin American Conference on Computational Intelligence (LA-CCI) , pp. 1-6, 2019.
- [39] W. Qin, Z. Zhuang, Z. Huang, and H. Huang, 'A novel reinforcement learning-based hyper-heuristic for heterogeneous vehicle routing problem,' Computers &amp; Industrial Engineering , vol. 156, 2021.
- [40] D. Liu, X. Yang, D. Wang, and Q. Wei, 'Reinforcement-learning-based robust controller design for continuous-time uncertain nonlinear systems subject to input constraints,' IEEE Transactions on Cybernetics , vol. 45, no. 7, pp. 1372-1385, 2015.
- [41] H. Modares, I. Ranatunga, F. L. Lewis, and D. O. Popa, 'Optimized assistive human-robot interaction using reinforcement learning,' IEEE Transactions on Cybernetics , vol. 46, no. 3, pp. 655-667, 2015.
- [42] H. Wang, T. Huang, X. Liao, H. Abu-Rub, and G. Chen, 'Reinforcement learning for constrained energy trading games with incomplete information,' IEEE Transactions on Cybernetics , vol. 47, no. 10, pp. 3404-3416, 2016.
- [43] W. Bai, Q. Zhou, T. Li, and H. Li, 'Adaptive reinforcement learning neural network control for uncertain nonlinear system with input saturation,' IEEE Transactions on Cybernetics , vol. 50, no. 8, pp. 3433 3443, 2019.
- [44] Y. Wen, J. Si, A. Brandt, X. Gao, and H. H. Huang, 'Online reinforcement learning control for the personalization of a robotic knee prosthesis,' IEEE Transactions on Cybernetics , vol. 50, no. 6, pp. 23462356, 2019.
- [45] T. T. Nguyen, N. D. Nguyen, and S. Nahavandi, 'Deep reinforcement learning for multiagent systems: A review of challenges, solutions, and applications,' IEEE Transactions on Cybernetics , vol. 50, no. 9, pp. 3826 - 3839, 2020.
- [46] R. Bellman, 'A markovian decision process,' Journal of Mathematics and Mechanics , pp. 679-684, 1957.
- [47] G. Li, L. Zhu, P. Liu, and Y. Yang, 'Entangled transformer for image captioning,' in Proceedings of the IEEE International Conference on Computer Vision , pp. 8928-8937, 2019.
- [48] J. Yu, J. Li, Z. Yu, and Q. Huang, 'Multimodal transformer with multiview visual representation for image captioning,' IEEE transactions on circuits and systems for video technology , vol. 30, no. 12, pp. 44674480, 2019.
- [49] F. Sun, J. Liu, J. Wu, C. Pei, X. Lin, W. Ou, and P. Jiang, 'Bert4rec: Sequential recommendation with bidirectional encoder representations from transformer,' in Proceedings of the 28th ACM International Conference on Information and Knowledge Management , pp. 1441-1450, 2019.
- [50] Q. Chen, H. Zhao, W. Li, P. Huang, and W. Ou, 'Behavior sequence transformer for e-commerce recommendation in alibaba,' in Proceedings of the 1st International Workshop on Deep Learning Practice for HighDimensional Sparse Data , pp. 1-4, 2019.
- [51] L. Xin, W. Song, Z. Cao, and J. Zhang, 'Step-wise deep learning models for solving routing problems,' IEEE Transactions on Industrial Informatics , vol. 17, no. 7, pp. 4861-4871, 2020.
- [52] K. He, X. Zhang, S. Ren, and J. Sun, 'Deep residual learning for image recognition,' in Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition , pp. 770-778, 2016.

- [53] S. Ioffe and C. Szegedy, 'Batch normalization: Accelerating deep network training by reducing internal covariate shift,' in International Conference on Machine Learning , pp. 448-456, 2015.
- [54] O. Vinyals, S. Bengio, and M. Kudlur, 'Order matters: Sequence to sequence for sets,' arXiv preprint arXiv:1511.06391 , 2015.
- [55] E. Uchoa, D. Pecin, A. Pessoa, M. Poggi, T. Vidal, and A. Subramanian, 'New benchmark instances for the capacitated vehicle routing problem,' European Journal of Operational Research , vol. 257, no. 3, pp. 845-858, 2017.

[56] J. Christiaens and G. Vanden Berghe, 'Slack induction by string removals for vehicle routing problems,' Transportation Science , vol. 54, no. 2, pp. 417-433, 2020.

- [57] Z. Xu and Y. Cai, 'Variable neighborhood search for consistent vehicle routing problem,' Expert Systems with Applications , vol. 113, pp. 66-76, 2018.
- [58] A. Palma-Blanco, E. R. Gonz´ alez, and C. D. Paternina-Arboleda, 'A two-pheromone trail ant colony system approach for the heterogeneous vehicle routing problem with time windows, multiple products and product incompatibility,' in International Conference on Computational Logistics , pp. 248-264, Springer, 2019.

[59] P.-P. Matthopoulos and S. Sofianopoulou, 'A firefly algorithm for the heterogeneous fixed fleet vehicle routing problem,' International Journal of Industrial and Systems Engineering , vol. 33, no. 2, pp. 204-224, 2019.

[60] J. A. Brito, F. E. McNeill, C. E. Webber, and D. R. Chettle, 'Grid search: an innovative method for the estimation of the rates of lead exchange between body compartments,' Journal of Environmental Monitoring , vol. 7, no. 3, pp. 241-247, 2005.

- [61] A. Pessoa, R. Sadykov, E. Uchoa, and F. Vanderbeck, 'A generic exact solver for vehicle routing and related problems,' Mathematical Programming , vol. 183, no. 1, pp. 483-523, 2020.
- [62] Y. Wu, W. Song, Z. Cao, J. Zhang, and A. Lim, 'Learning improvement heuristics for solving routing problems,' IEEE Transactions on Neural Networks and Learning Systems , 2021.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000009_5aaa3b329e90c81e9a632abb5e657136a67f89dfc80a5e38873a2ca72ad18159.png)

Jingwen Li received the B.E. degree in computer science from University of Electronic Science and Technology of China, China, in 2018. She is currently pursuing the Ph.D. degree with the department of Industrial Systems Engineering and Management, National University of Singapore (NUS). Her research interests include deep reinforcement learning for combinatorial optimization problems, especially for vehicle routing problems.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000010_ee4dd2d4aba86c000e67c23b0f475453a89f38b4bb532383ea0752cd74918143.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000011_6c85e5f8d51b656ca8274a7ec5b20cb1319f6c384e44534811e8bb8fd3caeed0.png)

Yining Ma received the B.E. degree in computer science from South China University of Technology (SCUT), Guangzhou, China, in 2019. He is currently pursuing the Ph.D. degree with the department of Industrial Systems Engineering and Management, National University of Singapore (NUS). His research interests include deep reinforcement learning, combinatorial optimization, and swarm intelligence.

Ruize Gao received the B.E. degree from Shanghai Jiao Tong University (SJTU), Shanghai, China, in 2020. He is currently pursuing the Ph.D. degree in the Department of Computer Science and Engineering, the Chinese University of Hong Kong (CUHK). His research interests include trustworthy machine learning, especially for adversarial robustness.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000012_c7be995b6c4ff9113792fedb37f06215699bc3af969c3f0285ade65af237ab63.png)

Zhiguang Cao received the Ph.D. degree from Interdisciplinary Graduate School, Nanyang Technological University. He received the B.Eng. degree in Automation from Guangdong University of Technology, Guangzhou, China, and the M.Sc. degree in Signal Processing from Nanyang Technological University, Singapore, respectively. He was a Research Assistant Professor with the Department of Industrial Systems Engineering and Management, National University of Singapore, and a Research Fellow with the Future Mobility Research Lab,

NTU. He is currently a Scientist with the Singapore Institute of Manufacturing Technology (SIMTech), Agency for Science Technology and Research (A*STAR), Singapore. His research interests currently focus on neural combinatorial optimization.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000013_3a8ea14a39a89063db2d408304d80272551aba57f04964bba2a0fead0cf73bf8.png)

Andrew Lim received the Ph.D. degree in computer science in 1992 from the University of Minnesota, Minneapolis. He is currently a Professor with the school of Computing and Artificial Intelligence, Southwest Jiaotong University, China. His works have been published in key journals such as Operations Research and Management Science, and disseminated via international conferences and professional seminars. Before Andrew was recruited by NUS under The National Research Foundation's Returning Singaporean Scientists Scheme in 2016, he spent more than a decade in Hong Kong where he held professorships in The Hong Kong University of Science and Technology and City University of Hong Kong.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000014_68a07010662c6f5546c5d09bfa0c14296beef257722b7f9c5d769ba79a68e048.png)

Wen Song received the B.S. degree in automation and the M.S. degree in control science and engineering from Shandong University, Jinan, China, in 2011 and 2014, respectively, and the Ph.D. degree in computer science from the Nanyang Technological University, Singapore, in 2018. He was a Research Fellow with the Singtel Cognitive and Artificial Intelligence Lab for Enterprises (SCALE@NTU). He is currently an Associate Research Fellow with the Institute of Marine Science and Technology, Shandong University. His current research interests include artificial intelligence, planning and scheduling, multi-agent systems, and operations research.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[li2022]_Deep_reinforcement_learning_for_solving_the_heterogeneous_capacitated_vehicle_routing_problem_artifacts/image_000015_caf5fd75118ab704e9cad5237b60395ca21d29c7225938d881a7932978615c4e.png)

Jie Zhang received the Ph.D. degree from the Cheriton School of Computer Science, University of Waterloo, Canada, in 2009. He is currently an Associate Professor with the School of Computer Science and Engineering, Nanyang Technological University, Singapore. He is also an Associate Professor at the Singapore Institute of Manufacturing Technology. During his Ph.D. study, he held the prestigious NSERC Alexander Graham Bell Canada Graduate Scholarship rewarded for top Ph.D. students across Canada. He was also a recipient of the Alumni Gold

Medal at the 2009 Convocation Ceremony. The Gold Medal is awarded once a year to honour the top Ph.D. graduate from the University of Waterloo. His papers have been published by top journals and conferences and received several best paper awards. He is also active in serving research communities.