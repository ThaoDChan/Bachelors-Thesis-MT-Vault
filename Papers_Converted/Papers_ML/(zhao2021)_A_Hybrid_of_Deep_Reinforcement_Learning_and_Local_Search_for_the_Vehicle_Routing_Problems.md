## A Hybrid of Deep Reinforcement Learning and Local Search for the Vehicle Routing Problems

Jiuxia Zhao , Minjia Mao, Xi Zhao, Senior Member, IEEE , and Jianhua Zou, Member, IEEE

Abstract -Different variants of the Vehicle Routing Problem (VRP) have been studied for decades. State-of-the-art methods based on local search have been developed for VRPs, while still facing problems of slow running time and poor solution quality in the case of large problem size. To overcome these problems, we first propose a novel deep reinforcement learning (DRL) model, which is composed of an actor, an adaptive critic and a routing simulator. The actor, based on the attention mechanism, is designed to generate routing strategies. The adaptive critic is devised to change the network structure adaptively, in order to accelerate the convergence rate and improve the solution quality during training. The routing simulator is developed to provide graph information and reward with the actor and adaptive cirtic. Then, we combine this DRL model with a local search method to further improve the solution quality. The output of the DRL model can serve as the initial solution for the following local search method, from where the final solution of the VRP is obtained. Tested on three datasets with customer points of 20, 50 and 100 respectively, experimental results demonstrate that the DRL model alone finds better solutions compared to construction algorithms and previous DRL approaches, while enabling a 5- to 40-fold speedup. We also observe that combining the DRL model with various local search methods yields excellent solutions at a superior generation speed, comparing to that of other initial solutions.

Index Terms -VRP, VRPTW, routing simulator, deep reinforcement learning, adaptive critic, local search.

## I. INTRODUCTION

I NTELLIGENT transportation system (ITS) is an essential issue in smart cities [1]. Nowadays, under the development of e-commerce, the delivery cost has become a top burden [2]. Moreover, Bräysy found out that distribution costs represent almost half of total logistic costs [3]. In food and beverage industry, distribution costs account for 70 percent of the additional cost of goods [4]. This indicates that ITS should pay attention to effective route planning. The vehicle routing problem (VRP) is a typical NP-hard problem in combinatorial optimization and has been studied for decades [5].

Manuscript received November 22, 2019; revised March 24, 2020; accepted June 3, 2020. This work was supported in part by the National Natural Science Foundation of China under Grant 91746111 and Grant 71702143 and in part by the Key Research and Development Program of Shaanxi Province under Grant 2020 ZDLGY 09-08. The Associate Editor for this article was H. A. Rakha. (Jiuxia Zhao and Minjia Mao contributed equally to this work.) (Corresponding author: Xi Zhao.)

Jiuxia Zhao and Jianhua Zou are with the School of Electronic and Information Engineering, Xi'an Jiaotong University, Xi'an 710049, China (e-mail: jiuxia@stu.xjtu.edu.cn).

Minjia Mao is with the School of Mathematics and Statistics, Xi'an Jiaotong University, Xi'an 710049, China.

Xi Zhao is with the School of Management, Xi'an Jiaotong University, Xi'an 710049, China (e-mail: zhaoxi1@mail.xjtu.edu.cn).

Digital Object Identifier 10.1109/TITS.2020.3003163

In ITS, applications of VRPs include express delivering [6], production planning [7] and airline scheduling [8], indicating wide practical significance.

VRPs are generally given an approximate solution by heuristic methods due to the complexity of the problem. Two main categories of heuristic algorithms are construction algorithms and local search algorithms [9]. Construction algorithms give solutions of VRPs through hand-crafted rules, sacrificing solution quality for high efficiency. For the second category, existing methods using local search include simulated annealing [10], tabu search [11] and large neighborhood search (LNS) [12]. Starting from an initial solution, local search uses different search operators to search for better solutions. In modern operation research, construction algorithms are usually used to generate initial solutions [13], [14]. However, the final iterative results of local search algorithms largely depend on the initial solutions especially for problems with large sizes [15], because the search space of local search algorithms is near the initial solution. Therefore, unsuitable initial solutions may lead to plenty of calculation time and local optimum.

Recently, deep reinforcement learning (DRL) has achieved appealing results in several challenging problems including Go [16], app recommendation [17] and combinatorial optimization [18]-[20]. More specifically, DRL-based models have been proposed to solve several NP-hard optimization problems, consisting of Maximum Cut (MC) [21], Travelling Salesman Problem (TSP) [22] and VRP [20], [23]. Two characteristics of the DRL model make it promising account for VRPs. First, DRL can estimate useful patterns which are difficult to find by manual heuristics especially in large scale problems [24]. Second, DRL has great potential for solving time-sensitive VRPs, due to the fast route generation (inference) process [23].

Nonetheless, several challenges arise when solving routing problems using DRL: (1) Although the inference speed is fast, the training process of the DRL model is time consuming [23]. Hence, it is a challenge to accelerate the convergence rate of DRL models. (2) An interactable environment and a great number of instances are required to train the DRL model. A simulator is needed for generating data and interacting with the model. (3) The solution quality of existing DRL based models is not comparable to state-of-the-art heuristic algorithms like LNS, representing the necessity of solution quality improvement.

To resolve the aforementioned challenges, we introduce a novel DRL model for generating routing strategies. The DRL model consists of an actor, an adaptive critic and a routing

1524-9050 ' 2020 IEEE. Personal use is permitted, but republication/redistribution requires IEEE permission. See https://www.ieee.org/publications/rights/index.html for more information.

simulator. The actor is designed to produce routing policies, the adaptive critic aims to optimize the parameters of the actor, and the routing simulator serves as the environment to assist the training and validation of the actor and critic. Specifically, attention mechanism [25] is used in the actor network, which is trained with the well known policy gradient method [26]. The adaptive critic is proposed to compute the baseline of the policy gradient method, in order to reduce the variance of training. The network structure of the adaptive critic changes adaptively as the training process moves on, in order to improve convergent performance and achieve better results. The simulator is devised to generate numerical instances for different variants of VRPs, update graph information after each node selection and calculate the reward of the given routing solution.

Moreover, to further obtain satisfactory solutions for large scale problems with a reasonable running time, we propose a two-stage framework for solving VRPs. The first stage is implemented by the aforementioned DRL model, while the second stage considers local search heuristic algorithms. As an end-to-end model, the DRL model can efficiently generate a solution when given the information of a new VRP instance, but the results are generally effective in quality. Using this solution as the initial solution in the second stage, the solution quality and calculation time can be enhanced by combining the local search method.

Our major contributions are summarized as follows:

- · We propose a DRL model to generate routing solutions for VRPs. In this model, an adaptive critic is devised to train the parameters of the neural network, which can accelerate the convergence rate and improve the performance of the DRL model simultaneously.
- · We develop a routing simulator served as the environment of the DRL model. It simulates the real world scenario, generating massive instances and interacting with the DRL model.
- · We propose a two-stage framework to obtain effective routing solutions. By combining the DRL model and the local search method, the framework is capable of producing satisfactory routing solutions for VRPs with large problem size at a reasonable calculation speed.
- · We validate the performance of the proposed DRL model and the two-stage framework. We also investigate the affect of the adaptive critic on the solution quality and convergence rate.

The rest of the paper is organized as follows. We first analyze the related work in Section 2. Section 3 elaborates the problem statement. We describe the detailed DRL model in Section 4. And the experimental results are presented in Section 5. Finally, we conclude this paper in Section 6.

## II. RELATED WORK

## A. Exact and Heuristic Algorithms

VRPs have been studied for decades and researchers have developed extensive solution approaches, consisting of exact algorithms and heuristic algorithms. Exact algorithms, including branch and bound method and column generation, can get the optimal solutions [27]. However, with the scale of the problem grows, the computational complexity increases exponentially, causing the problems unsolvable in a reasonable time [28].

Heuristic algorithms are widely employed in practice. VRPs can be solved by heuristic algorithms via designing hand-crafted features with expert knowledge. The local search method, often referred as neighborhood search, is an important category of heuristic algorithms. Starting from an initial solution, local search methods improve the solution quality by searching in space near the initial solution. Based on the original design, a variety of heuristic algorithms are derived including simulated annealing [10], tabu search [11] and LNS [12]. Local search algorithms have been adopted to different variants of the VRP, such as the Vehicle Routing Problem with Time Window (VRPTW) [29], split delivery VRP [30] and electric VRP [31]. However, the search procedure stops when there is no improvement of solutions in the neighborhood space, causing the algorithms falling into local optima [32].

## B. Neural Combinatorial Optimization

The first attempt to use neural network (NN) for combinatorial optimization problems can trace back to Hopfield, who solved the TSP of small instances with NN [33]. Recently, based on the sequence-to-sequence model [34], Vinyals et al. proposed the pointer network trained with supervised learning for TSP [35]. For combinatorial optimization, supervised learning based methods generally requires optimal solutions as labels. However, they are hard to obtain in practice. On the contrary, DRL learns from the reward signals, without requiring the label in advance. Bello et al. made use of the pointer network to solve the TSP and trained the networks with reinforcement learning techniques [18]. Experiments demonstrated its scalability in the KnapSack problem and TSP, compared with traditional algorithms. Based on the pointer network, Nazari et al. proposed an attention-based network to solve the VRP and split delivery VRP, leading outperformed results on medium-size problems compared with construction algorithms and Google OR-Tools [20].

On the other hand, Khalil et al. reformulated the nodes of graph structure by a struct2vec network and applied a deep Q-network to train the network [21]. Yu et al. combined the pointer network with the struct2vec network to generate routing strategies for online VRP [23], regarded as a guideline for future DRL-based routing problems. Moreover, Li employed guided tree search with the graph convolutional network for maximal independent set (MIS) problem, achieving satisfactory results [24].

## III. PROBLEM STATEMENT

In a typical VRP instance, a set of n customer points and one depot are denoted as a graph G = ( V , E ) . For each point v i , it has several characteristics including coordinate ( ci = ( xi , yi ) ), and demand ( qi ). Service time ( t s i , ) and time window ( [ t e i , , t l , i ] ) are added in a VRPTW instance. The detailed information of variables is shown in Table I. As depicted in Figure 1, several vehicles are needed to meet the customers according to the following requirements including capacity and time constraints:

- · The starting points and ending points of all vehicles are the depot v 0.

Fig. 1. An illustration of VRPTW. Three vehicles serve 10 customers in given time windows.

TABLE I VARIABLES

![Image](image_000000_592fc56e9da8b4cff482cd932aaf1354db7382c52d130ce3dd93c74b93f64de3.png)

- · All vehicles have a fixed maximum load Q , and the planning route cannot exceed Q .
- · The time windows are set hard, i.e., vehicles are not allowed to serve customers outside of the time windows. When arriving early t a i , &lt; t e i , , the vehicle should wait until the early time. Meanwhile, it is not allowed to arrive later than the late time t l , i .

Given π as the solution sequence, our goal is to minimize the total routing length L :

$$\text{$\text{$touing $length $L$} } \\ L ( \pi | G ) = \sum _ { i = 1 } ^ { | \pi | - 1 } \left \| c _ { \pi ( i ) } - c _ { \pi ( i + 1 ) } \right \| _ { 2 }, \quad & ( 1 ) \quad A. \ R o \\ \text{re } \left \| \cdot \left \| _ { 2 } \text{ is the $\ell_{2}$ norm and $|\pi|$ is the length of the $the$ high $c$} \right \|.$$

where ‖ · ‖ 2 is the /lscript 2 norm and | π | is the length of the sequence π .

To tackle this problem, we propose a two-stage framework, including the DRL stage and local search stage. As depicted in Figure 2, initial solutions obtained by DRL pass through the local search stage to get the final solutions.

Given a graph G , the proposed DRL model, parameterized by θ , needs to define a strategy p (π | G ) and select a sequence π . It is a stochastic policy and can be factorized as

$$p _ { \theta } ( \pi | G ) = \prod _ { t = 1 } ^ { | \pi | } p _ { \theta } ( \pi ( t ) | \pi ( < t ), G ). \quad \ \ ( 2 ) \\ n \, \text{order to minimize the objective in} \, E a ( 1 ). \, \text{we use the DRL}$$

In order to minimize the objective in Eq(1), we use the DRL model to compute every component in the right-hand side of Eq(2) and the basic elements of the DRL model are defined as follows.

## A. Agent

We consider an operating vehicle as an agent. At every time step t , the agent is in a given state of the environment and chooses an action π( ) t according to its policy. Receiving reward -L from the environment, the agent learns to generate satisfactory results.

## B. State

The state is divided into static state s and dynamic state dt . Static state, which stays unchangeable all the time, includes coordinates for VRP, considering service time ( t s i , ) and time windows [ t e i , , t l , i ] of customers for VRPTW. Dynamic state consists of demands of customers qi , load Q ′ and position of the operating vehicle at current time t .

## C. Action

The action π( ) t specifys the next destination at current time t and a joint action π instructs the routing strategy of operating vehicles for considered instance. π( ) t is sampled from the right-hand side of Eq(2) during training time and obtained by greedy or beam search strategies in the test process.

## D. Reward

The negative of the objective function Eq(1) is calculated as reward when all the demands are satisfied, since DRL is set to maximize the reward (minimize the tour length).

## IV. REINFORCEMENT LEARNING MODEL

The proposed DRL model can be depicted as Figure 2, including an actor, an adaptive critic and a routing simulator. The routing simulator (described in Section A) is used to be the environment of VRPs, returning new states and reward to the actor and adaptive critic. The actor (described in Section B) is designed to generate the routing policy of the given graph. The adaptive critic (described in Section C) aims to optimize the parameters of the actor via estimating the reward adaptively.

## A. Routing Simulator

A reliable environment is necessary for DRL. Due to the high complexity of real world problems, devising the simulator is a challenge. In this section, the design of the routing simulator is introduced. This simulator first generate massive VRP instances for training and validation. In addition to data generation, the simulator needs to interact with the vehicle as it moves. Once the vehicle chooses the next customer point, the simulator has two main aspects to update. One is to update the dynamic state, and the other is to mask the unavailable points at next time [18].

Fig. 2. The architecture of the model.

![Image](image_000001_eb0f59d357e5e38b5b793a5e574d16c1a3361610f255a59b5738f147547db8da.png)

1) Data Generation: Well-known datasets of VRPs include Augerat dataset [36] for VRP and Solomon benchmark [37] for VRPTW. However, the number of instances in existing datasets cannot meet the need of DRL. Hence the routing simulator firstly tackles the data generation problem. For the sake of brevity, we only discuss the data generation of the VRPTW, because VRP can be transferred from VRPTW by removing time-related elements.

According to the data distribution of Solomon benchmark, the routing simulator generates instances with feasible solutions. In a VRPTW instance, the simulator first creates n customers and one depot. Each customer has nine features which are divided mainly into two parts, static and dynamic elements. The static part consists of the constant variables over time including coordinates ( xi , yi ) , time windows [ t e i , , t l , i ] and service time t s i , . Both the service time t s i , and the length of time window /Delta1 t are randomly generated from a closed interval. Then the early time t e i , and the due time t l , i are calculated as follows :

$$& \text{canduce as borrows} \\ & t _ { e, i } = \frac { e _ { i } \times d _ { 0 i } } { v }, e _ { i } \in [ 1, \frac { T - t _ { s, i } - \Delta t _ { i } } { d _ { 0 i } } \times v - 1 ], \quad ( 3 ) \\ & t _ { l, i } = t _ { e, i } + \Delta t _ { i }, \quad & ( 4 ) \\ & \text{where $n$ is the seed of the vehicle. $d_{u}$ denotes the distance}$$

where v is the speed of the vehicle. d i 0 denotes the distance between depot v 0 and customer point v i . T is the whole time interval.

For dynamic elements, the variables change with time. The dynamic state includes the demand of each customer, current load, time and position of the vehicle. The demand qi of a customer point is an integer and the demand of depot is 0. The maximize load Q varys with the problem sizes n , reffering to Nazari [20].

2) Updating Dynamic: After choosing the next customer to serve, the state changes according to the movement of the operating vehicle. The static state remains unchanged, while the dynamic state updates as the vehicle moves forward. Once the vehicle decides to serve customer j , the position variable equals the index of the chosen customer. Meanwhile, the demand of the chosen customer becomes zero and the load of the vehicle updates by subtracting the demand value Q ′ = Q ′ -qj . In addition, the time is set to be the end of this service t ′ = max { t a , j , t e , j } + t s , j where t a , j is the time of arrival.

3) Updating Mask: Owing to the capacity and time constraints, there exist points that the vehicle can not reach at current time t . And we utilize the mask to present the available customer points of the operating vehicle. The customer points that violate the capacity and time constraints are masked and the already served points are also masked.

The different mask conditions are shown in Eq(5), Eq(6) and Eq(7) seperately. And the ultimate mask is a conbination of the above situations, shown in Eq(8):

$$C _ { t, j } ^ { q } = \begin{cases} 1 & q _ { j } \geq 1 \\ 0 & \text{otherwise} \end{cases}, \quad \quad ( 5 )$$

$$C _ { t, j } ^ { Q } = \begin{cases} 1 & q _ { j } \leq Q ^ { \prime } \\ 0 & \text{otherwise} \end{cases},$$

$$C ^ { t } _ { t, j } = \begin{cases} 1 & t _ {$$

$$\text{Mask} _ { j } ^ { t } = \overset { \bullet }$$

where /circledot means the element-wise product.

## B. Actor Network Architecture

1) Encoder: The encoder is designed to map the state of the current graph, making the agent understanding the representation of each vertex. Like previous network architecture [20],

the static state and the dynamic state are embedded respectively. In basic attention mechanism [25], the model is sensitive to the order of the input sequence. A graph embedding is used in this paper to overcome this problem. As shown in Eq(9) and Eq(10), the static and dynamic state are embedded into a D -dimensional space. The embeddings are sent as the reference information of the attention layer.

$$s _ { i } ^ { r } & = W ^ { s } s _ { i } +$$

$$d _ { i, t } ^ { r } = W ^ { d } d _ { i,$$

where si means the static state and di t , means the dynamic state of point i at step t .

2) Decoder: Taking the embedding of current vehicle information as input, the decoder decodes the present state into high dimensional hidden state sequentially. A Long Short-Term Memory (LSTM) layer [38] is used to generate hiddden state ht , which is the query in the attention layer.

3) Attention Layer: The attention layer is used to describe the similarity between the current state and each customer point. At step t , the hidden state ht of decoder is used as a query to calculate the relevance with the graph information ( s r , d r t ) , where ( s r , d r t ) is the output of the encoder. The relevant parameter at is set as

$$\begin{array} {$$

In VRPs, the network will take a mask to determine whether the next customer point meets the demand and time constraints. The mask is given by the routing simulator and shown as where [· ; ·] is the concatenation operator between vectors.

$$\text{now} \, \infty \\ \text{Mask} _ { i } ^ { t } = \begin{cases} 1, & \text{if node $i$ is valid at time $t$} \\ 0, & \text{otherwise} \end{cases} \,. \quad ( 1 3 ) & \text{an act} \\ \text{layer} \, s \\ \text{Than $1\nmid$} \end{cases}$$

At last, consider M to be a large constant, the probability of each next optional customer point can be represented as follows :

The detailed processing procedure of the proposed model is depicted in Algorithm 1.

$$p ( \pi ( t ) | ( \pi ( < t ), G ) = \text{softmax} \left ( a _ { t } + M \cdot Mask ^ { t } \right ). \quad ( 1 4 ) \quad \text{where} \quad \\ \text{The detailed processing procedure of the proposed model is} \quad \text{How} \quad \text{the diff}$$

## C. Adaptive Critic Training

Vinyals et al. used supervised learning to train the neural network for the TSP [35]. However, high quality labels are difficult to obtain but are the key to supervised learning. On the contrary, DRL only needs the reward signal which is the negative of the total tour length to train the network. By setting the DRL parameters as θ and providing the graph G , we can get the training objective of the network :

$$J ( \theta | G ) = \mathbb { E } _ { \pi \sim p _ { \theta } ( \cdot | G ) } L ( \pi | G ). \quad \quad ( 1 5 ) \quad \text{greed} \\ \cdot \quad \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \colon$$

The policy gradient is used to train the parameters [26]:

$$\nabla _ { \theta } J ( \theta | G ) = \mathbb { E } _ { \pi \sim \theta } \left [ ( L ( \pi | G ) - b ( G ) ) \nabla _ { \theta } \log p _ { \theta } ( \pi | G ) \right ], \ ( 1 6 ) \quad \text{The } \cdot \\ \text{other } b ( G ) \text{ means a baseline which demonstrates the routing} \quad \text{has a c} \\ \text{mark of the same in some and is used to add to each one} \, \text{the remaining} \quad \cdot \cdot \cdot \cdot \cdot \cdot$$

where b G ( ) means a baseline which demonstrates the routing length of different instances and is used to reduce the training variance.

## Algorithm 1 Processing Procedure of the DRL Model

Input Static state si for i ∈ { 1 , . . . , n } , Dynamic state di , 0 for i ∈ { 1 , . . . , n }

- 1: Compute embeddings of static state s r i for i ∈ { 1 , . . . , n } by Encoder.
- 2: Initialize current position π( ) 0 ← 0.
- 4: Compute embeddings of dynamic state d r i , t for i ∈ { 1 , . . . , n } by Encoder.
- 3: for t = 1 , . . . , N do
- 5: Compute the query ht according to s r π( t 1 ) by Decoder.
- 7: Compute the Mask Mask t i = C q t , i ∗ C Q t , i ∗ C t t , i for i ∈ { 1 , . . . , n } by Routing Simulator.
- -6: Compute the attention value at by Eq(11) and Eq(12).
- 8: Compute the probability of each point p (π( ) (π(&lt; t | t ), G ) = t
- softmax ( at + M · ) 9: π( ) t = Greedy ( p (π( ) (π(&lt; t | t ), G )) if choose greedy mode.

Mask

.

- 10: end for
- 11: return Joint action π = { π( ) t } N 1 to get the solution.

In practice, the expected value is replaced with the average of Monte Carlo sampling in a batch size B :

$$( 1 1 ) \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdclots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \cdots \quad \colon \\ \text{$two$}. \quad \nabla _ { \theta } J ( \theta ) \approx \frac { 1 } { B } \sum _ { i = 1 } ^ { B } \left ( L \left ( \pi _ { i } | G _ { i } \right ) - b \left ( G _ { i } \right ) \right ) \nabla _ { \theta } \log p _ { \theta } \left ( \pi _ { i } | G _ { i } \right ). \\ \text{$two$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$two$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \ \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \ \text{$three $}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three $}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \ \text{$three$}\quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}.. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}\quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \vec {$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{\$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text`}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three`}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$Three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$Three`}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$ Three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$ Three`}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$two` }. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$}. \quad \text{$three$$

In the actor-critic framework, the critic is set to predict the advantage function, which is exactly the b G ( ) . As depicted in Fig 2, the adaptive critic consists of a simple network and an actor network. During the early stage of training, a multilayer simple network is designed to formulate a normal critic. The loss of the normal critic is calculated as

$$\text{$me loss of use normal crurc is cacuarece as} \\ \mathcal { L } ( \theta ^ { C } ) = \frac { 1 } { B } \sum _ { i = 1 } ^ { B } \left \| L ( \pi _ { i } | G _ { i } ) - V _ { \theta ^ { C } } ( G _ { i } ) \right \| _ { 2 } ^ { 2 }, \quad ( 1 8 ) \\ \text{where $V_{\theta ^{C} } ( G _ { i } )$ is the output baseline of the normal critic.}$$

where V C θ ( Gi ) is the output baseline of the normal critic.

However, the simple network structure is hard to describe the difference between instances effectively. To improve the performance of critic, we borrow the idea of self critic employed in image captioning [39]. One problem of self critic is that in the early stage of training, the critic, as a copy of actor, cannot calculate the baseline accurately, since the actor itself performs bad at the early stage.

To overcome this problem, the adaptive critic copies the network structure and parameters of the actor when the actor is trained for several epochs. By denoting the parameters of the adaptive critic as θ c , the baseline is the route length of a greedy solution :

$$b ( G ) = L ( \pi | G ) | _ { \pi = \text{Greedy} ( \pi \sim p _ { 0 } c ( \cdot | G ) ) }. \quad \ \ ( 1 9 ) \\ \dots \quad. \ \cdot \ \cdot \ \cdot \ \cdot \ \cdot \ \cdot \ \cdot \ \cdot \.$$

The adaptive critic will reacquire the parameters of the actor θ only when the t -test of the two recent testing results has a confidence of 95%, ensuring that the parameters of the actor are updated in a positive direction. Detailed algorithm is depicted in Algorithm 2.

## Algorithm 2 Adaptive Critic Training

Input Batch size B , Training epoch E 1 , E 2, Number of sampling N

- 1: Initialize actor params θ 2: Initialize adaptive critic params θ C ← θ 3: for epoch = 1 , . . . , E1 + E2 do 4: Generate training set Me 5: Generate validation set Mt 6: for n = 1 , . . . , N do 7: Gi ∼ SampleInput ( Me ) for i ∈ { 1 , . . . , B } 8: π i ∼ SampleSolution ( p θ ( ·| Gi )) for i ∈ { 1 , . . . , B } 9: if e ≤ E 1 then 10: bi ← V θ C ( Gi ) for i ∈ { 1 , . . . , B } 11: g θ ← 1 B ∑ B i = 1 ( L (π i | Gi ) -bi ) ∇ θ log p θ (π i | Gi ) 12: g θ C ← 1 B ∑ B i = 1 ‖ L (π i | Gi ) -bi ‖ 2 2 13: θ ← ADAM (θ, g θ ) 14: θ C ← ADAM (θ C , g θ C ) 15: else 16: π C i ∼ Greedy ( p θ C ( ·| Gi ) ) for i ∈ { 1 , . . . , B } 17: bi ← L (π C i | Gi ) for i ∈ { 1 , . . . , B } 18: g θ ← 1 B ∑ B i = 1 ( L (π i | Gi ) -bi ) ∇ θ log p θ (π i | Gi ) 19: θ ← ADAM (θ, g θ ) 20: if T-TEST ( L (π | Gtest ) , bi ) &lt; 5% then 21: θ C ← θ 22: end if 23: end if 24: end for 25: end for

1) Simple Network Architecture: We explain the architecture of the simple network now, which aims to mapping the input graph into a baseline b G ( ) . The network includes three modules: 1) an encoder which has the same structure as the actor network, 2) a Recurrent Neural Network (RNN) block and 3) a two-layer linear network. The encoder is designed to embed the information into static state, dynamic state and hidden state h . Similar to Bello [18], the RNN block repeats to update the hidden state h by glimpsing at the static and dynamic state. After the input is embedded by the encoder and sent to a block, a two-layer linear network is employed to calculate the baseline.

## V. EXPERIMENTS

We conduct extensive computational experiments to investigate the performance of the proposed DRL model and two-stage framework, in terms of solution quality and computational time. We tested on two kinds of routing problems - VRP and VRPTW, with problem sizes of 20, 50 and 100, respectively. The test sets are generated based on the routing simulator, named cohort-20, cohort-50, cohort-100 for VRP and cohort-20TW, cohort-50TW and cohort-100TW for VRPTW. These cohorts are used only in the test procedure to ensure reliable comparison. Computational experiments are conducted on a server with a configuration of single GeForce GTX 1080 Ti and 4 Intel(R) Xeon(R) CPUs at 2.40GHz with 12 cores. We first present the experimental details in Section A. In Section B and C, we analyze the performance of the DRL model alone and two-stage framework, compared with other baselines. Finally we investigate the convergence rate and solution quality influenced by the proposed adaptive critic in Section D.

## A. Experimental Details

1) Testing Strategies: To further improve the results, we use two approaches during the testing process, greedy and beam search [34]. For each step, the greedy strategy selects the highest probability customer point as the action. However, given beam width k , beam search stores topk choices at a time and outputs the final results according to the maximum probability of the sequence.

2) Baselines: To evaluate the proposed DRL model, we compare the DRL model against different baselines: construction algorithms, ant colony optimization (ACO) [40], and the DRL method presented by Nazari et al. [20]. Construction algorithms include Clarke-Wreight Savings [41], Random Insertion, Nearest Insertion [42] and Nearest Neighbor [43].

To investigate the effectiveness of the proposed two-stage framework, we compute the results of the above baselines as initial solutions for local search. Google OR-Tools and LNS [12] are considered as the representatives of local search algorithms. The results with different initial solutions of each local search algorithm are presented in the second stage of comparison.

3) Data Generation Parameters: In this section, we describe the parameters of data generation in the routing simulator. For the VRPTW, the coordinate of the depot and customer points ( xi , yi ) are generated from within a range of [ 0 1 , ] × [ 0 1 . Both the service time , ] t s i , and the length of time window /Delta1 t are randomly generated from [ 0 15 . , 0 2 . . ] The whole time interval is set as T = 4 6 . and the vehicle speed is set as v = 1. The integer demand qi of a customer point is generated from 1 to 9. We set the maximize load Q = 30 when n = 20, Q = 40 when n = 50, Q = 50 when n = 100, refering to Nazari [20].

4) Hyperparameters: As for the DRL model, the encoder embeds the graph information into a 128-dimension vector, while the decoder is a single LSTM layer with 128 hidden units, of which the dropout is 0.1. We take batch size as 256 and 200 epochs for training, randomly sampling 512000 instances in each epoch. We clip the L2 norm of gradient to 2.0. After fine tuning, we set E 1 as 20, which will be described in Section D. The model is trained with Adam optimizer [44] with a learning rate of 10 -4 . When testing, we use beam search (BS) of size 10.

Since Google OR-Tools is functioned as one of the local search algorithms, the local\_search\_metaheuristic parameter is set to GUIDED\_LOCAL\_SEARCH , and the time limit for each instance is 30 ∗ n ms , where n is the problem size of this instance. For LNS algorithm, the number of customers to remove ( toRemo e v ) and the maximum number of iterations ( I t er ) vary with the problem sizes. The parameters are set as ( toRemo e v = 3 , I t er = 150) at problem size 20, ( toRemo e v = 6 , I t er = 200) at size 50, and ( toRemo e v = 10 , I t er = 250) at size 100. Except that LNS is implemented in Matlab, other code is conducted in Python.

TABLE II RESULTS WITHOUT LOCAL SEARCH

5) Running Time: The running time is hard to compare but important. Since the model is end-to-end, the training time is not considered when analyzing the overall performance of the proposed methods. The running time of each method is calculated by running 1,280 instances in total. For each two-stage algorithm, the running time of the overall process is listed in the table. All the reported calculation time is in seconds.

by other baselines mentioned above. Similar to the results without local search, Table III and Table IV report the average tour lengths, gap and total calculation time on each cohort of Google OR-Tools and LNS with different initial solutions, respectivley.

## B. Results Without Local Search

The proposed DRL model is compared with the baselines mentioned in Section A for VRP and VRPTW respectively in Table II. The first column reports the baseline adopted to solve the corresponding cohorts, in which 'DRL' is the DRL model proposed in Section 4. Columns with headers 'Aver', 'Gap' and 'Time' report the average tour lengths, gap and total calculation time on different cohorts. For the VRP, the best result is set as gap ( = 0%) since the optimal results is hard to obtain even for cohort-20, while the optimal solutions calculated by Gurobi is set as gap ( = 0%) for the VRPTW.

For both VRPs, the proposed DRL model can achieve better results than construction algorithms with significantly shorter computation time even in large problem size. Most importantly, our results outperform work of Nazari [20] both in greedy and beam search (beam size = 10) mode for all cohorts. The reason is that the highly effective adaptive critic is capable of giving better assessment of instances when training the model, compared with the normal critic.

## C. Results With Local Search

We compare the performance on enhancing the Google OR-Tools and LNS by our proposed DRL model with that

Using Google OR-Tools in the local search stage, the results are presented in Table III. We can observe that results enhanced by DRL methods, including our and Nazari's model, significantly outperform that enhanced by construction algorithms both in solution quality and calculation time. Taking cohort-100TW as an example, the results enhanced by DRL models achieve average solution quality at 27.37 with an average solution time of 486.40s, while the results enhanced by construction algorithms get the best solution quality at 27.67, with a solution time of 2337.23s. We focus on the comparison between OR-Tools with Nazari's DRL model and with our DRL model, in terms of solution quality and calculation time. Taking solution quality into consideration, OR-Tools with our DRL model becomes more competitive for the VRP at all problem size. While for the VRPTW, the proposed method only achieves better results for cohort-100TW and gets the same solution quality for cohort-20TW and cohort-50TW. For the calculation time, note that these two DRL models have the same characteristics of high inference speed, their computational time is similar.

We also test our framework by using LNS in the local search stage. The results are summarized in Table IV. We can see that the proposed framework (DRL + LNS) significatly surpasses others for all cohorts and gets much closer to optimal results (from around 7% to 4% for cohort-100TW and about 3% improvement of cohort-100). The results imply that the DRL model can enhance the local search method

TABLE III RESULTS WITH GOOGLE OR-TOOLS

TABLE IV RESULTS WITH LARGE NEIGHBORHOOD SEARCH (LNS)

like Google OR-Tools and LNS both in solution quality and calculation time. Moreover, the competitive calculation speed makes the proposed method having the potential to be enlarged to accommodate the growth of problem size.

We present a more detailed comparison about solution quality of VRPTW utilizing Google OR-Tools as local search algorithm. Figure 3 shows the result comparison for VRPTW cohorts in terms of their winning rate on instance level.

Each table represents the comparison result of the problem with a specific size. The value in the table is the proportion of total 1,280 instances in which the method on the row outperforms those on the column. For example, the value at the coordinate (1,2) in table (a), means 43.2% of total instances that OR-Tools outperforms CW + OR-Tools on cohort-20TW. We can observe that DRL(10) + OR-Tools outperforms OR-Tools with other initial solutions, e.g., at a

![Image](image_000002_d2400ac14676663336f7f4bcb38fb4987261392998ef97a79cd8140dcbdfd7bf.png)

![Image](image_000003_1a9ca9fb77ad229953ed3bd15fdd975a9a125ba1639cba91e3486dd4e6939e2d.png)

![Image](image_000004_1211c747412023bd877a4b7927cc055df9ba4e7047b8370776adf7c48c831e8c.png)

for

Fig. 3. The proportion of all instances which the result of the method on the row outperforms the method on the column.

winning rate above 56% in cohort-100TW. From this perspective, the proposed method achieves consistent performance across different instance to exceed other approaches, rather than a stochastic event.

## D. Adaptive Critic

As mentioned above, a suitable baseline helps to reduce the training variance. In this paper, we introduce the adaptive critic which is an combination of self critic [22] and normal critic, conducted by simple network structure. One hyperparameter of the adaptive critic is the transition point, determining when to change the network structure from normal critic to self cirtic. It is defined as the percentage of epochs at the time of the transition, and symbolized as Et . Note that when Et equals 100%, the adaptive critic degenerates into a simple network; when Et equals 0%, it is exactly the same as the self critic [22]. Table V reports a comparison of the average tour length and training time per epoch on different set of Et , when testing on cohort-20. We can observe that the best solution is achieved when Et equals 10%, and the self critic and normal critic have a consistent result under this situation.

Fig. 4. Different convergence rates using different critics.

![Image](image_000005_ab2da2de91f741058f648d6e4310f34e91e8b57f669e89a62ca26be94f6e3ca8.png)

The convergence rates of the DRL model with adaptive critic and normal critic are compared in Figure 4. The Y-axis is the validation optimality gap and the X-axis is the training epochs of corresponding methods. It can be observed that the convergence rates and the solution quality of the DRL model are largely improved with adaptive critic, since this design makes use of the actor with inference mode to estimate the baseline after training for several epochs. On the contrary, normal critic is likely to be confused with different instances, and the self critic does not perform well at the beginning since the actor is poorly trained at start.

## VI. CONCLUSION

In this paper, we first propose a novel DRL model for solving VRPs, consisting of an actor to generate routing solutions and an adaptive critic to train the parameters of the actor. Like previous work [20], the actor is implemented

by the attention mechanism together with graph embbeding to construct vehicle routing tours iteratively. To reduce the training variance, an adaptive critic is devised to optimize the parameters of the actor. In addition, a routing simulator is designed to help the training and evaluation of the DRL model. Moreover, a two-stage framework consisting of DRL and local search is proposed for solving VRPs. The DRL method is capable of finding out strong initial solutions for local search to enhance the solution quality, achieving near optimal solution.

The simulation results show that the proposed DRL model outperforms the construction algorithms and other DRL model, and the adaptive critic can effectively accelerate the convergence rate and improve the solution quality. We conduct comprehensive experiments to evaluate the performance of the framework, using the Google OR-Tools and LNS as classical local search algorithms. Experimental results indicate that the proposed framework can generate outstanding routing strategies, comparing to local search with initial solutions produced by other construction algorithms. Besides the improvement of solution quality, the route generation of the proposed framework costs short computing time, making the framework promising for VRPs with large sizes. Furthermore, using DRL results as an initial solution can be a general approach to combining DRL with heuristic methods to solve combinatorial optimization problems.

The future research can be highlighted as follows. On one hand, adavanced techniques of reinforcement learning, e.g., multi-header attention, can be employed to improve the performance of the proposed DRL model. On the other, the proposed two-stage framework can be applied to more complicated routing problems and other combinatorial optimization problems.

## REFERENCES

- [1] L. Zhu, F. R. Yu, Y. Wang, B. Ning, and T. Tang, 'Big data analytics in intelligent transportation systems: A survey,' IEEE Trans. Intell. Transp. Syst. , vol. 20, no. 1, pp. 383-398, Jan. 2019.
- [2] M. Joerss, J. Schröder, F. Neuhaus, C. Klink, and F. Mann, Parcel Delivery: The Future of Last Mile . New York, NY, USA: McKinsey &amp; Company, 2016.
- [3] O. Bräysy and M. Gendreau, 'Vehicle routing problem with time windows, part I: Route construction and local search algorithms,' Transp. Sci. , vol. 39, no. 1, pp. 104-118, Feb. 2005.
- [4] L. R. Dopson and D. K. Hayes, Food and Beverage Cost Control Study Guide . Hoboken, NJ, USA: Wiley, 2015.
- [5] G. Kim, Y.-S. Ong, C. K. Heng, P. S. Tan, and N. A. Zhang, 'City vehicle routing problem (city VRP): A review,' IEEE Trans. Intell. Transp. Syst. , vol. 16, no. 4, pp. 1654-1666, Aug. 2015.
- [6] G. Perboli and M. Rosano, 'Parcel delivery in urban areas: Opportunities and threats for the mix of traditional and green business models,' Transp. Res. Part C: Emerg. Technol. , vol. 99, pp. 19-36, Feb. 2019.
- [7] Y. Li, F. Chu, C. Feng, C. Chu, and M. Zhou, 'Integrated production inventory routing planning for intelligent food logistics systems,' IEEE Trans. Intell. Transp. Syst. , vol. 20, no. 3, pp. 867-878, Mar. 2019.
- [8] B. D. Brouer, J. F. Alvarez, C. E. M. Plum, D. Pisinger, and M. M. Sigurd, 'A base integer programming model and benchmark suite for liner-shipping network design,' Transp. Sci. , vol. 48, no. 2, pp. 281-312, May 2014.
- [9] D. C. Paraskevopoulos, G. Laporte, P. P. Repoussis, and C. D. Tarantilis, 'Resource constrained routing and scheduling: Review and research prospects,' Eur. J. Oper. Res. , vol. 263, no. 3, pp. 737-754, 2017.
- [10] S. Kirkpatrick, C. D. Gelatt, and M. P. Vecchi, 'Optimization by simulated annealing,' Science , vol. 220, no. 4598, pp. 671-680, 1983.
- [11] F. Glover, 'Tabu search-Part I,' ORSA J. Comput. , vol. 1, no. 3, pp. 190-206, 1989.
- [12] P. Shaw, 'Using constraint programming and local search methods to solve vehicle routing problems,' in Proc. Int. Conf. Princ. Pract. Constraint Program. , 1998, pp. 417-431.
- [13] P. Grangier, M. Gendreau, F. LehuØdØ, and L.-M. Rousseau, 'An adaptive large neighborhood search for the two-echelon multiple-trip vehicle routing problem with satellite synchronization,' Eur. J. Oper. Res. , vol. 254, no. 1, pp. 80-91, Oct. 2016.
- [14] I. Žulj, S. Kramer, and M. Schneider, 'A hybrid of adaptive large neighborhood search and tabu search for the order-batching problem,' Eur. J. Oper. Res. , vol. 264, no. 2, pp. 653-664, Jan. 2018.
- [15] M. Zirour, 'Vehicle routing problem: Models and solutions,' J. Qual. Meas. Anal. , vol. 4, no. 1, pp. 205-218, 2008.
- [16] D. Silver et al. , 'Mastering the game of go with deep neural networks and tree search,' Nature , vol. 529, no. 7587, p. 484, 2016.
- [17] Z. Shen, K. Yang, W. Du, X. Zhao, and J. Zou, 'DeepAPP: A deep reinforcement learning framework for mobile application usage prediction,' in Proc. 17th Conf. Embedded Netw. Sensor Syst. , Nov. 2019, pp. 153-165.
- [18] I. Bello, H. Pham, Q. V. Le, M. Norouzi, and S. Bengio, 'Neural combinatorial optimization with reinforcement learning,' 2016, arXiv:1611.09940 . [Online]. Available: http://arxiv.org/abs/1611.09940
- [19] H. Dai, B. Dai, and L. Song, 'Discriminative embeddings of latent variable models for structured data,' in Proc. Int. Conf. Mach. Learn. , 2016, pp. 2702-2711.
- [20] M. Nazari, A. Oroojlooy, L. Snyder, and M. Takac, 'Reinforcement learning for solving the vehicle routing problem,' in Proc. Adv. Neural Inf. Process. Syst. , 2018, pp. 9839-9849.
- [21] E. Khalil, H. Dai, Y. Zhang, B. Dilkina, and L. Song, 'Learning combinatorial optimization algorithms over graphs,' in Proc. Adv. Neural Inf. Process. Syst. , 2017, pp. 6348-6358.
- [22] W. Kool and M. Welling, 'Attention, learn to solve routing problems!' in Proc. Int. Conf. Learn. Represent. , 2019, pp. 1-25.
- [23] J. J. Q. Yu, W. Yu, and J. Gu, 'Online vehicle routing with neural combinatorial optimization and deep reinforcement learning,' IEEE Trans. Intell. Transp. Syst. , vol. 20, no. 10, pp. 3806-3817, Oct. 2019.
- [24] Z. Li, Q. Chen, and V. Koltun, 'Combinatorial optimization with graph convolutional networks and guided tree search,' in Proc. Adv. Neural Inf. Process. Syst. , 2018, pp. 539-548.
- [25] A. Vaswani et al. , 'Attention is all you need,' in Proc. Adv. Neural Inf. Process. Syst. , 2017, pp. 5998-6008.
- [26] R. J. Williams, 'Simple statistical gradient-following algorithms for connectionist reinforcement learning,' Mach. Learn. , vol. 8, nos. 3-4, pp. 229-256, May 1992.
- [27] G. Laporte, 'The vehicle routing problem: An overview of exact and approximate algorithms,' Eur. J. Oper. Res. , vol. 59, no. 3, pp. 345-358, Jun. 1992.
- [28] G. Laporte, M. Gendreau, J.-Y. Potvin, and F. Semet, 'Classical and modern heuristics for the vehicle routing problem,' Int. Trans. Oper. Res. , vol. 7, nos. 4-5, pp. 285-300, Sep. 2000.
- [29] D. Pisinger and S. Ropke, 'Large neighborhood search,' in Handbook of Metaheuristics . Boston, MA, USA: Springer, 2010, pp. 399-419.
- [30] W. Gu, D. Cattaruzza, M. Ogier, and F. Semet, 'Adaptive large neighborhood search for the commodity constrained split delivery VRP,' Comput. Oper. Res. , vol. 112, 2019, Art. no. 104761.
- [31] J. Hof, M. Schneider, and D. Goeke, 'Solving the battery swap station location-routing problem with capacitated electric vehicles using an AVNS algorithm for vehicle-routing problems with intermediate stops,' Transp. Res. B, Methodol. , vol. 97, pp. 102-112, Mar. 2017.
- [32] T. Erdeli· c and T. Cari· c, 'A survey on the electric vehicle routing problem: Variants and solution approaches,' J. Adv. Transp. , vol. 2019, pp. 1-48, May 2019.
- [33] J. J. Hopfield and D. W. Tank, 'Neural computation of decisions in optimization problems,' Biological , vol. 52, no. 3, pp. 141-152, 1985.
- [34] I. Sutskever, O. Vinyals, and Q. V. Le, 'Sequence to sequence learning with neural networks,' in Proc. Adv. Neural Inf. Process. Syst. , 2014, pp. 3104-3112.
- [35] O. Vinyals, M. Fortunato, and N. Jaitly, 'Pointer networks,' in Proc. Adv. Neural Inf. Process. Syst. , 2015, pp. 2692-2700.
- [36] P. Augerat, J. M. Belenguer, E. Benavent, A. CorberÆn, and D. Naddef, 'Separating capacity constraints in the CVRP using tabu search,' Eur. J. Oper. Res. , vol. 106, nos. 2-3, pp. 546-557, Apr. 1998.
- [37] M. M. Solomon, 'Algorithms for the vehicle routing and scheduling problems with time window constraints,' Oper. Res. , vol. 35, no. 2, pp. 254-265, Apr. 1987.

[38] S. Hochreiter and J. Schmidhuber, 'Long short-term memory,' Neural Comput. , vol. 9, no. 8, pp. 1735-1780, 1997.

- [39] S. J. Rennie, E. Marcheret, Y. Mroueh, J. Ross, and V. Goel, 'Selfcritical sequence training for image captioning,' in Proc. IEEE Conf. Comput. Vis. Pattern Recognit. , 2017, pp. 7008-7024.

[40] M. Dorigo and M. Birattari, 'Ant colony optimization,' in Encyclopedia of Machine Learning . Boston, MA, USA: Springer, 2011, pp. 36-39.

[41] G. Clarke and J. W. Wright, 'Scheduling of vehicles from a central depot to a number of delivery points,' Oper. Res. , vol. 12, no. 4, pp. 568-581, Aug. 1964.

- [42] S. Joshi and S. Kaur, 'Nearest neighbor insertion algorithm for solving capacitated vehicle routing problem,' in Proc. Int. Conf. Comput. Sustain. Global Develop. , 2015, pp. 86-88.
- [43] R. He, W. Xu, Y. Wang, and W. Zhan, 'A route-nearest neighbor algorithm for large-scale vehicle routing problem,' in Proc. 3rd Int. Symp. Intell. Inf. Technol. Secur. Inform. , Apr. 2010, pp. 390-393.
- [44] D. P. Kingma and J. Ba, 'Adam: A method for stochastic optimization,' 2014, arXiv:1412.6980 . [Online]. Available: http:// arxiv.org/abs/1412.6980

![Image](image_000006_95fea8436a6229d5aa9e37bf060a4f78bf334e13bc0e40811fd9c0ce8bd3d08d.png)

![Image](image_000007_8a9b355e55f0c41a063d9fc168c20289048fb58e3fe99b7e247bc95fa49898d9.png)

Jiuxia Zhao received the bachelor's degree in the field of management from Xi'an Jiaotong University, Xi'an, China, in 2018, where she is currently pursuing the master's degree with the School of Automation Science and Technology.

Her research interests include the data mining and deep reinforcement learning for combinatorial optimization problems, especially for vehicle routing problems.

Minjia Mao received the B.Eng. degree from Tsinghua University in 2018. He is currently pursuing the master's degree with the School of Mathematics and Statistics, Xi'an Jiaotong University.

His research interests include deep learning and reinforcement learning, especially on applying learning methods for optimization problems.

![Image](image_000008_b740ba77f73a839ff2e06541213b42e2e588ba2675779f020d21c1221b28549c.png)

Xi Zhao (Senior Member, IEEE) received the Ph.D. degree (Hons.) in computer science from the École Centrale de Lyon, Lyon, France, in 2010.

He is currently a Professor with the School of Management, Xi'an Jiaotong University. His research interests include deep reinforcement learning, behavior analysis, and big data. He has published over 70 articles indexed by SCI/ EI, and has been PI for more than 15 grants in these fields.

![Image](image_000009_a153d9b287ebbfd1e6e84781a34f6bfa0d8d6c98cbf83f673416d5d1bf76ed59.png)

Jianhua Zou (Member, IEEE) received the Ph.D. degree with the Institute of Plasma Physics, Chinese Academy of Sciences, Beijing, China, in 1991.

He is the Vice Dean of the School of Electronics and Information Engineering, Xi'an Jiaotong University, Xi'an, China. He conducted the post-doctoral research with the Huazhong University of Science and Technology, Wuhan, China, and Xi'an Jiaotong University from 1991 to 1993 and from 1993 to 1995, respectively. He became a Professor with the System Engineering Institute, Xi'an

Jiaotong University. He was the Principle Investigator of two projects funded by the National Science Foundation of China, about ten key projects supported by industry and military. As a Senior Visiting Scholar, he has established partnerships with Eindhoven University, Eindhoven, The Netherlands, and Philips Company, Amsterdam, The Netherlands. He has published over 60 academic articles. His current research interests include intelligent control, computer vision and pattern recognition, data mining, and knowledge discovery.