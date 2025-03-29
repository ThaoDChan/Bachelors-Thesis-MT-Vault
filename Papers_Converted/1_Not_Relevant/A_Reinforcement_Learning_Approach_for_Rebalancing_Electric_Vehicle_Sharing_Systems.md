## A Reinforcement Learning Approach for Rebalancing Electric Vehicle Sharing Systems

Aigerim Bogyrbayeva, Sungwook Jang, Ankit Shah, Young Jae Jang , and Changhyun Kwon

Abstract -This paper proposes a reinforcement learning approach for nightly offline rebalancing operations in free-floating electric vehicle sharing systems (FFEVSS). Due to sparse demand in a network, FFEVSS requires relocation of electrical vehicles (EVs) to charging stations and demander nodes, which is typically done by a group of drivers. A shuttle is used to pick up and drop off drivers throughout the network. The objective of this study is to solve the shuttle routing problem to finish the rebalancing work in minimal time. We consider a reinforcement learning framework for the problem, in which a central controller determines the routing policies of a fleet of multiple shuttles. We deploy a policy gradient method for training recurrent neural networks and compare the obtained policy results with heuristic solutions. Our numerical studies show that unlike the existing solutions in the literature, the proposed methods allow solving the general version of the problem with no restrictions on the urban EV network structure and charging requirements of EVs. Moreover, the learned policies offer a wide range of flexibility, resulting in a significant reduction in the time needed to rebalance the network.

Index Terms -Shared mobility, reinforcement learning, neural combinatorial optimization, vehicle routing.

## I. INTRODUCTION

T HE advent of electric vehicles (EVs) and car-sharing services provides a sustainable option to move people and goods across dense urban areas. Car sharing services with EVs have the potential to increase the utilization of resources and offer a unique opportunity to the urban population in the form of free-floating EV sharing systems (FFEVSS). With the FFEVSS, examples of which include companies such as car2go [1] and WeShare [2], customers no longer need to own a vehicle and can conveniently pick up/drop off any EV, ondemand, from the parking lots of designated service areas. However, there are some critical operational challenges to bring this on-demand service into the mainstream.

Before the start of the day, an operating company needs to relocate EVs to the ideal demand locations to establish a supply-demand balance in the system. Furthermore, to provide

Manuscript received October 5, 2020; revised April 5, 2021; accepted May 18, 2021. The work of Changhyun Kwon was supported in part by the University of South Florida Nexus Initiative (UNI) Award. The Associate Editor for this article was F. Chu. (Corresponding author: Changhyun Kwon.) Aigerim Bogyrbayeva, Ankit Shah, and Changhyun Kwon are with the Department of Industrial and Management Systems Engineering, University of South Florida, Tampa, FL 33620 USA (e-mail: aigerimb@usf.edu; ankitshah@usf.edu; chkwon@usf.edu).

Sungwook Jang and Young Jae Jang are with the Department of Industrial and Systems Engineering, KAIST, Daejeon 34141, South Korea (e-mail: jedi829@kaist.ac.kr; yjang@kaist.ac.kr).

Digital Object Identifier 10.1109/TITS.2021.3085217

Fig. 1. The overview of the rebalancing problem of FFEVSS, with a single shuttle and 2 drivers. The numbers indicate the number of drivers in the shuttle. Solid and dashed lines represent the routes of the shuttle and EVs, respectively. Cases I and II refer to relocation of EVs without and with charging, respectively.

![Image](image_000000_65fca83cc2036c851424b658a664e6b18a3436e91d90f08330e5d9058e6e77f0.png)

a certain level of service, EVs need to be charged before they can be used by the customers. There are two major issues: (i) there exists a sparse demand in the service area network, and hence it is not trivial to find the ideal locations to relocate the EVs; and (ii) there needs to be an efficient routing plan to drop off the drivers for picking up the EVs and taking the EVs to the charging stations for charging, and then pick up the drivers from their respective locations [3]. It is evident that without efficient solutions for the above complex and costly operational challenges [4], the sustainable existence of the FFEVSS is uncertain.

We consider a static, nightly rebalancing problem similar to [5]-[8], where a group of drivers is used to relocate and recharge the EVs based on the predicted demand for the next day, assuming the utilization level of FFEVSS is minimal. As shown in Figure 1, shuttles are used to support the movements of drivers. In this setting, rebalancing operations require two key decisions to be made: (i) how to route shuttles to pick up and drop off the drivers (shuttle routing decision) and (ii) where to charge and relocate each of the EVs (EV relocation decision). In this paper, focusing on solving the shuttle routing decision problem, we propose a reinforcement learning (RL) approach, in which the EV relocation decisions are made by a rule-based approach.

The proposed RL approach possesses several advantages compared to optimization-based approaches. First, unlike solutions coming from the static optimization techniques such

1558-0016 ' 2021 IEEE. Personal use is permitted, but republication/redistribution requires IEEE permission.

See https://www.ieee.org/publications/rights/index.html for more information.

Fig. 2. Assign supplier-charger pairs or reuse charger nodes? Ch and Su denotes charger and supplier nodes respectively.

![Image](image_000001_70288f1d1abe8e027d650d04ce2245cbdf346d13c0337bba667f6428e7bd4eda.png)

Fig. 3. How to balance traveling time and waiting time trade-off? De , Ch and Su denotes demander, charger and supplier nodes respectively.

![Image](image_000002_e5e6fcb3a999f6c9f9928d3477ae919a4ff267864dc3c171e285978f216af71a.png)

![Image](image_000003_8f72a884110ec09ea2d8f8d38a0ae05334acdb1b20e11c24a56b070793b87853.png)

as [7], [8], which need to be re-solved each time an input changes, the RL agent learns robust solutions that can be applied to any input coming from the same distribution [9]. Second, while static optimization approaches can take significant time to solve a problem, a trained RL agent can be invoked to produce quality solutions instantaneously. Third, many practical considerations can be flexibly incorporated within the simulator in the training phase.

The shuttle routing to rebalance FFEVSS with its variety of trade-offs is not a trivial problem. For instance, as depicted in Figure 2, one may allow or disallow the reuse of charging stations in the derivation of solutions. The former choice offers more flexibility, but it also increases the complexity of exploring solutions. Therefore, the existing methods do not allow the reuse of the charging stations [8]. On the other hand, such a choice results in opportunity loss.

Another trade-off is depicted in Figure 3, where the first supplier node has an EV that needs to be recharged while the second supplier has an EV with a sufficient charging level. Then one needs to balance between traveling time and waiting time when routing a shuttle to supplier nodes. The complexity of such routing decisions increases with the network size, the network structure, and the number of shuttles and drivers deployed. Hence, it may not be possible to explore potential solutions with human-driven heuristics efficiently. With the proven ability of neural networks in recognizing patterns in graph-based representations, the utilization of neural network architecture with the proposed RL approach will provide better approximations and assist in obtaining efficient solutions that can be generalized.

In recent years, there has been a surge of studies that apply reinforcement learning to solve various traditional vehicle routing problems (VRPs) [10]-[12] with capacity constraints, time windows, or stochastic demand. The shuttle routing problem, taken under this study, possesses significant differences with traditional VRPs. First, in a VRP setting, nodes to visit (demand) are typically independent of the routing decisions. However, in the shuttle routing problem, the locations of drivers to be picked up are determined by preceding routing decisions. This highlights a strong interdependence between demand and routing. Second, unlike VRP, the shuttle routing problem is characterized by delayed rewards . As shown in Figure 4, the actual relocations of EVs from a node happen after the execution of shuttle routing to the node. As a result, we observe delayed rewards with respect to the shuttle routing decision only after EVs reach their designated nodes. Such differences require a new approach to finding solutions for the shuttle routing problem.

We consider two settings of rebalancing FFEVSS. In the first setting, we focus on a single shuttle problem, where we train a single agent to learn routing policies. In the second setting, we aim to train a fleet of shuttles through single-agent reinforcement learning, where a central controller is responsible for routing multiple shuttles. In both cases, we deploy policy gradient methods along with recurrent neural networks for training. The shuttle routing problem under both of the above-mentioned settings possesses significant challenges that prohibit the direct use of the existing solution methods. For instance, in routing a single shuttle, we must train an agent not only to find efficient routes, but at the same time maintain the feasibility of the solutions related to the precedence of the visiting nodes. As for routing the fleet of shuttles, we must promote learning policies to route multiple shuttles that will contribute to a common goal.

The main contributions of this study are as follows. First, to the best of our knowledge, this study is the first to present an RL-based approach for handling multiple vehicles explicitly in the context of VRPs, while focusing on the shuttle routing problem for rebalancing the FFEVSS. Second, within the RL framework, we propose the utilization of deep neural network architecture to process the complex and high dimensional observations from an urban service area network to help train the RL agent in its decision-making. In particular, we adopt sequence-to-sequence models with an attention mechanism to fit the unique challenges of the rebalancing FFEVSS. Third, we present a novel training algorithm to route efficiently a fleet of shuttles to rebalance FFEVSS by utilizing policy gradient methods. Our training algorithm does not require splitting an urban network into sub-clusters for each shuttle, but instead allows developing policies that efficiently utilize shuttles and drivers in a whole network. Fourth, we develop a simulator to mimic real-world FFEVSS, which serves as the environment for training an RL-agent and allows efficient exploration of joint actions of multiple shuttles.

Unlike the solutions obtained using the methods from the literature, the empirical results obtained from this study show that the proposed method allows solving the general version of the problem with no restrictions on the urban network structure and charging levels of EVs. The learned policies offer a wide range of flexibility, resulting in a significant reduction in the time needed to rebalance the network.

The remainder of the paper will proceed as follows. In Section II we provide an overview of relevant literature and outline the unique challenges of the rebalancing FFEVSS. In Section III we present the problem formulation. In Section IV we introduce the proposed reinforcement learning model. In Section V we demonstrate the results of our computational studies. Lastly, in Section VI we provide concluding remarks.

## II. RELATED WORK

Even though the problem of rebalancing FFEVSS has been recognized as essential for their sustainable existence in the literature [13], [14], most of the studies focus on high-level approaches to address the issue. One category of studies falls on incentive-based methods that aim to rebalance the system through influencing customer behavior [15]. Another set of papers study the deployment of personnel and offer rule-based high-level decision-making frameworks [16], [17]. There are only a few studies that specifically focus on the shuttle routing problem to rebalance FFEVSS, thus offering detailed solutions for day-to-day operational challenges.

One of such studies is [7], which aims to solve both EV relocation and shuttle routing problems jointly. However, the proposed model does not enforce relocation of EVs directly to demander nodes, but indeed permits leaving EVs in charger nodes. As a result, charger stations will be blocked and cannot be reused, requiring the postponing of charging for the remaining set of EVs. Similarly, a recent study [8] presents novel approaches in addressing EV relocation and shuttle routing problems simultaneously. Even though the study aims at relocating EVs directly to demander nodes, it assumes the abundance of charger stations in an urban network. Thus, again reusing charger stations is not considered, and the postponement of charging for EVs requiring it is allowed. Since charging infrastructure is often limited [18], the reuse of charging stations must be an integral part of solutions to rebalance FFEVSS in real-world urban networks.

Recently reinforcement learning approaches gained popularity to solve various problems in transportation, including fleet management and rebalancing in ride-hailing services [19]-[22]. However, none of the existing studies focus on FFEVSS specifically and do not address the unique issue of charging and relocation together. For solving VRPs, deep reinforcement learning has been first applied in [10], which utilizes sequence-to-sequence methods [23] and an attention mechanism [24]. Later [25] adopted the transformer model [26] to solve VRPs without recurrent neural networks. [12] proposes a novel model to solve online VRPs by utilizing neural combinatorial optimization and deep reinforcement learning. Similarly, [27] presents a hybrid model that combines local search with an attention mechanism. However, these studies focus on routing a single capacitated vehicle, where the main goal is to minimize the distance traveled. While multiple loops of a single capacitated vehicle can be interpreted as multiple vehicles, this paper is the first to present explicit modeling of multiple vehicles within an RL framework.

Although this study also adopts sequence-to-sequence models with an attention mechanism similar to [10], the significant differences in the nature of the rebalancing FFEVSS problem and VRP dictate the development of novel solution techniques. For instance, in the given problem, shuttles need to leave a depot, drop off, pick up drivers who relocate EVs, and return to the depot, highlighting two sets of constraints. First, the precedence of visited nodes needs to be maintained when charging stations are visited after nodes with EVs and nodes that require EVs are visited after either charging stations or nodes with EVs. Second, the capacity constraint must be satisfied when nodes with EVs are visited only when there is a driver in a shuttle and nodes with drivers are visited only if there is seating available for a driver in the shuttle. In addition to feasibility constraints, since both charging and relocations of EVs are involved in the shuttle routing problem, only considering factors that affect the total distance traveled is not sufficient. Moreover, the dynamics of an urban network due to routing a shuttle is more complex compared to the VRP due to the delayed movements of EVs relocation. Also, routing multiple shuttles requires a novel training algorithm. In particular, when several shuttles are present in an urban network and each of their movement influence the state of the network, we need a novel framework that enables the application of reinforcement learning tools based on Markov Decision Process (MDP).

## III. PROBLEM STATEMENT AND FORMULATIONS

## A. Network

Let us consider a network N consisting of N number of nodes and a depot. We define a node as a supplier if it has an excess EV and a demander if it requires an EV. The network also has charger nodes. Each node in the network can store at most one EV. Depending on the charging levels of EVs there are two possibilities of the EVs relocation. In Case I, EVs are relocated from supplier nodes directly to demander nodes. In Case II, EVs first need to be taken to charger nodes, and after charging is complete, they need to be relocated to the demander nodes, as shown in Figure 1. We consider discrete charging levels of EVs, where a threshold-based rule is applied to decide whether to charge an EV or not. Also, a driver may wait at a charging station until an EV is fully charged or may head for the next activity. We consider two settings of the problem when a single shuttle or a fleet of shuttles is deployed for rebalancing the system. We formulate the routing problem for a single shuttle as MDP and utilize a central controller to route a fleet of shuttles.

## B. Multi-Shuttle Routing as MDP

Even though it is possible to formulate the routing of a fleet of shuttles using a multi-agent reinforcement learning framework, such an approach suffers from several drawbacks. Firstly, in the presence of several shuttles, each of which is treated as an autonomous agent, the stationary assumption of MDP is no longer valid [28]. In particular, in the presence of other agents in the environment, the Markovian property, which states that reward and current state only depends on individual action and previous state, does not hold. Therefore, a multi-agent reinforcement learning framework works under

partially observable MDP [29]-[31], when each agent can observe only a local view of the network [32]. Then each agent can only visit nodes visible from its local view, which imposes significant restrictions on developing an efficient routing. Secondly, it is challenging to train autonomous agents without making strong assumptions about constant communication between agents. For instance, if at the current time step one agent selects a node to visit, then such information must be shared among other agents to avoid the presence of several agents at the same node. Lastly, under a static network, when the state of the network is constant and well-known, a centralized approach will help navigate a fleet of shuttles efficiently. Therefore, we formulate routing multiple shuttles to rebalance FFEVSS using a central controller responsible for making routing decisions of all shuttles. Then, we can formulate the problem using a single-agent reinforcement learning framework and MDP. We also note that the concept of multi-agent reinforcement learning and central controller is similar to decentralized control and centralized control in the transportation literature.

A fleet of shuttles with drivers leaves a depot and visits nodes in the network to relocate EVs from supplier nodes to demander nodes. Shuttles must return to a depot after fulfilling demand at all demander nodes and picking up all the drivers. These sequential decisions of a central controller for routing shuttles under uncertain demand (locations of drivers) can be formulated as a finite horizon MDP, where the future dynamics of the system depend only on the current state. We define the RL framework for the problem as tuple M = 〈 X , A , P , R T , 〉 representing states, actions, transition probabilities, reward function, and time horizon, respectively. The definitions are as follows :

- · I = { 1 , . . . , I } is the set of I shuttles that are controlled by a central controller;
- · State set X represents the network, where for each node it shows its location, the relative distance, the number of EVs, the number of drivers, the charging levels of EVs' and indicators for the expected transitions. We utilize binary vectors to indicate if there is an expected EV coming to a node. We denote state as xt at time t .
- · A is the set of joint actions such that A t = A 1 t × A 2 t × · · · × A I t , where A i t is the action set of shuttle i at time t and action a i t indicates a node number to be visited next by shuttle i . Then a central controller's action set consists of joint actions of all shuttles, A t , at time t .
- · Transition Probabilities function, P , determines state transitions probabilities p xt ( + | 1 xt , at ) at time t with respect to taken action at . In the given problem, transitions are deterministic but often delayed. After an action is taken, the relocations of EVs are scheduled. However, the actual state transitions related to the movements of EVs occur later, as shown in Figure 4.
- · All shuttles share a common reward R and immediate reward rt , which are assigned based on the joint actions of all shuttles at time t denoted by at and state xt ;
- · Instead of defining the specific time value of T , we define one episode rollout for the problem based on the exper-

## A SUMMARY OF VARIABLES

iment outcomes. One episode is terminated either if all demander nodes are fulfilled and all drives are picked up back to a depot or if the total number of time steps exceeds the predefined maximum time steps, the value of which is set based on the size of a network.

- · Each time step t is determined by the earliest fulfilled action among all shuttles. Thus, each time step starts when a central controller takes an action and finishes whenever any action is fully executed.

The list of variables used can be found in Table I.

## IV. REINFORCEMENT LEARNING MODEL

We adopt a policy gradient method, similar to those popularly used in routing problems [10], [12], [25], to learn the complex routing policies of shuttles directly . In general, policy gradient methods consist of two separate networks: an actor and a critic. The critic estimates a value function given a state according to which the actor's parameters are set to generate policies in the direction of improvement. We train an agent and a central controller to route a single shuttle and multiple shuttles in an urban network by simulating the FFEVSS environment. The simulator is developed to handle EV relocations through rule-based decisions and utilizing sequenceto-sequence models to generate policies. The overview of the model is shown in Figure 5.

## A. The FFEVSS Simulator

The main function of the FFEVSS simulator is to represent the dynamics in an urban network caused by movements of shuttles. There are immediate and delayed transitions related to routing shuttles. In an immediate update to the environment at each time step, we consider locations of shuttles, drivers, EVs, the number of drivers in a shuttle, and fulfillment of scheduled transitions either related to charging or relocation of EVs. Also, at each time step, we schedule transitions related to movements of EVs that have started but unfulfilled. In particular, starting at the current clock time t c = 0, we update the environment according to movements of a shuttle :

$$t _ { c } \leftarrow \begin{cases} t _ { c } + \tau$$

/negationslash where τ represents traveling time between nodes visited by a shuttle at time t -1 and t and w t denotes waiting time at node n . We define waiting time at node n as the difference between the time when a delayed transition at node n occurs

Fig. 5. An overview of the reinforcement learning model.

![Image](image_000004_e7a925773e5663676b053a2438f512825ee88eaba3bf5219dbd7dd77e87b9e6a.png)

and the time when a shuttle reaches node n . To account for delayed transitions, we introduce a time vector, which keeps track of remaining times until either EVs arrive at designated nodes or their charging completes. In the case of multiple shuttles, the environment is updated with the earliest movements of shuttles.

Another function of the FFEVSS simulator is to update a masking scheme according to the current state of the urban network. The masking scheme helps to maintain the feasibility of solutions related to the precedence of visited nodes and the number of drivers in a shuttle. Also, having an efficient masking scheme expedites the exploration of action space. We deploy the following masking scheme, where A t = ∅ stores the set of available nodes/actions to visit at time t and the rest of the nodes are masked. For each n ∈ N , we update :

$$\mathcal { A } _ { t } \leftarrow \begin{cases} \mathcal {$$

Here set E t denotes nodes with an EV or nodes with the expected EV due to delayed transitions, set D t denotes nodes with a driver or nodes with the expected drivers, and l t denotes the number of drivers in a shuttle at time t .

## B. EV Relocation Decisions

As described earlier, our focus in this study is to solve the shuttle routing problem. Hence, we are using a rule-based approach for EVs' relocation decisions. The rule-based approach is as follows: every time a supplier node with an EV has a driver, that EV is relocated to the nearest available either charger or demander node. The decision of whether to relocate an EV to a demander or charger node is predetermined in the settings of a simulator. We apply a threshold-based rule; that is, if the charging level of an EV exceeds the threshold, then it can be directly relocated to a demander node or must be charged, otherwise.

We maintain a binary vector in the simulator to indicate if a charger node is available or not. This representation helps in deciding the relocation of an EV from a supplier node to an available charger node. We determine the closest available charger node by multiplying the binary vector by a time matrix that indicates time to travel among any pair of nodes. To decide EVs' relocations from either supplier or charger nodes to demander nodes, we maintain a demand matrix that keeps track of demander nodes that still need an EV at time t . In particular, in the simulator, we store the time needed to move from all nodes to each demander node and increase those values to large numbers if a demander node is satisfied. Then, if an EV needs to be relocated to a demander node, we compute the minimum time from a node to the closest demander nodes.

## C. A Sequence-to-Sequence Model for the Shuttle Routing Problem

Motivated by [10], we propose using a sequence-tosequence model for rebalancing FFEVSS, which typically consists of an encoder and a decoder. Given urban network N , we aim to generate a sequence of nodes to be visited by either a shuttle or a fleet of shuttles. In other words, we are interested in learning the following conditional probability or parametrized policy πθ :

$$\pi _ { \theta } ( Y _ { T } | x _ { 0 } ) =$$

In (1), we let xt = { x t 1 , . . . , x N t } , where x n t denotes static and dynamic states of node n at time t . Unlike in machine translation, the state of nodes in the network status changes dynamically with shuttles movement; thus, we need to consider both static and dynamic states for each node. Also, we let yt denote a node to be visited at time t and Yt = { y 1 , . . . , yt } with Y 0 = ∅ . Then to select a next node to visit yt + 1, we are interested in learning φ( yt + | 1 xt , Yt ) .

However, a set of nodes in the network does not convey any sequential information. Therefore, it is common in literature [10], to omit recurrent neural network for encoding. Indeed, due to the sparse nature of networks, graph embedding is deployed in encoder to build their continuous vector representation as they suit better for statistical learning [33].

The following equation describes embedding for each n ∈ N :

$$\bar { x } _ { s } ^ { n } & = b ^ { s } + W$$

$$\begin{array} {$$

where, ¯ x n s and ¯ x n dt are embedded static and dynamic states of node n at time t and b , W represent the trainable parameters of a neural network. We denote by ¯ x n t = ( ¯ x n s ; ¯ x n dt ) concatenation of embedded static and dynamic states of nodes.

For decoding we use recurrent neural networks (RNN), that takes static state of the last visited node and stores the sequence as follows :

$$h _ { t } = W ^ { h } f ( h _ { t - 1 } ) + W ^ { x } \bar { x } _ { s } ^ { n } \, \quad \ \ ( 4 ) \, \text{ and } \tau$$

where ht is a memory state of RNN, f represents nonlinear transformation and x n s is a static state of node n visited at time t . Trainable weight matrices W h and W x represent connections between hidden state to hidden state and hidden state to an input respectively. Note in our implementations, we use a LSTM cell as RNN.

In addition to encoder and decoder, we also utilize content based attention mechanism as in [10]. Content based attention tries to mimic associative memory and is designed to handle cases when an input to the sequence-to-sequence model is a set [24]. In particular, the current state of an urban network is coupled with the memory state of RNNs about the sequence to calculate an alignment vector ct that assigns the probabilities of nodes to visit next :

$$u _ { t } ^ { n } & = v \tanh ( W ( \bar { x } _ { t } ^ { n } ; h _ { t } ) ) \ \forall n \in \mathcal { N } \quad \quad ( 5 ) \quad \text{which} \\ \varkappa - \text{cofit $\omega t u.\,\mu$}$$

$$c _ { t } = \text{softmax} ( u _ { t } ) \quad \quad \quad ( 6 ) \quad \text{estim} \\ \cdot \, \cdot \, \cdot \, \cdot \, \cdot \, \cdot \, \cdot \, \cdot \, \cdot \, \cdot$$

where v and W are trainable weight matrices.

For the problem under study, we define the static state of nodes as their location coordinates and the initial charging levels of EVs at supplier nodes. Even though the charging levels of EVs will change as EVs are taken to charging stations, only their initial values determine charging times. Therefore, we consider them as a static state of nodes. For a dynamic representation of the states of nodes, we use the number of EVs, the number of drivers in a shuttle, and the distance from the current node to other nodes. Our experimental studies show that passing distance information as a dynamic state of nodes substantially reduces training time. Figures 5 summarizes the sequence-to-sequence model of the shuttle routing problem used in the actor network. In routing a fleet of shuttles, we also deploy a single actor network, where a sequence of visited nodes Yt , includes nodes visited by all shuttles up to time t .

## D. Reward Function

Reward function along with sets of available actions reflects our aim to maintain the feasibility and efficiency of routing decisions. Since the shuttle routing problem considers both charging and relocation of EVs, reward function must not only reflect traveling times between nodes, but also include waiting times. Therefore, we define reward function as the negative of total time spent in the system starting when a shuttle or a fleet of shuttles leaves a depot and ending when all shuttles are returned back to the depot with all drivers after fulfilling all demander nodes. Then our aim is to maximize the negative of total time spent in the system denoted by R . More formally we define reward function as follows, using immediate rewards rt :

$$R = \sum _ { t = 1 } ^ { T } r _ { t }$$

where

/negationslash

$$r _ { t } = \begin{cases} - \tau ( n _ { t - 1 }, n _ { t } ) & \text{if $n_{t-1}\neq n_{t}$} \\ - w _ { t } & \text{if $n_{t-1}=n_{t}$} \end{cases} \\ \text{i.e.} \text{travolino} \limo \, \text{and} \, \text{$n$ is within} \, \text{time at this}$$

and τ t is traveling time and w t is waiting time at time t .

## E. Training Algorithm

In training, we are interested in finding policy parameters ¯ θ that maximize the total expected reward :

$$\bar { \theta } = \underset { \theta } { \arg \max } \mathbb { E } _ { \pi _ { \theta } } [ R ].$$

Given the state of network X , we can write :

$$J ( \theta | x ) = \mathbb { E } _ { \pi \sim p _ { \theta } ( \cdot | x ) } [ R ( \pi | x ) ]$$

and

$$\nabla _ { \theta } J ( \theta | x ) & = \mathbb { E } _ { \pi } [ A ^ { \pi } \nabla _ { \theta } \log p _ { \theta } ( \pi | x ) ] \quad \quad ( 1 0 ) \\ \Delta ^ { \pi } & = \, \mathcal { P } ( \pi | v \, \cdot \, - \, V ( v \lambda ) \quad \quad \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \ \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \, \ \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad\, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad\, \ \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \. \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \ } \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \cdot \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \$$

$$A ^ { \pi } = R ( \pi | x ) - V ( x _ { 0 } ). \quad \quad ( 1 1 ) \\. \quad \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{--} \, \text{$$

We use the REINFORCE algorithm with a baseline [34], which is the value of the initial state of an urban network estimated by a critic with trainable parameters θ c . Algorithm 1 represents our training procedure, where the actor network with trainable parameters θ a represents policy π . In a batch training setting, the batch of instances is generated by a data generator. Instances are passed through the simulator. Then, the actor network produces probabilities of nodes to be visited by shuttles at each time step, and the simulator is updated accordingly until the entire episode is finished. Then with the received total reward for the selected actions, the parameters of the actor and critic networks are updated. Unlike in the existing literature [8], the algorithm does not require splitting an urban network into sub-clusters for each shuttle, but instead deploys all shuttles to serve the whole network. Also, utilizing a central controller that observes the entire urban network state along with the masking scheme in the simulator allows efficiently exploring joint action of all shuttles. For instance, if a node has been assigned to be visited by a shuttle, then that node is masked for other shuttles.

## V. COMPUTATIONAL STUDIES

## A. Data Generation and Configurations

We consider a 1 × 1 square mile network consisting of demander, supplier, and charger nodes. We first specify the total number of nodes in the network and the number of demander and charger nodes. We sample x, y coordinate of each node from a uniform distribution with values ranging from 0 to 1. Similarly, we sample demander, charger, and supplier nodes from a uniform distribution. For each supplier

## Algorithm 1 Training Algorithm

- 1: Initialize network parameters θ a and θ c for actor and critic networks respectively. Set the maximum number of epochs, a batch size and the maximum number of steps denoted as M epochs, M epis and T respectively;
- 2: for epochs = 1 to M epochs do
- 3: Reset gradients d θ a , d θ c ;

4:

for

m

= 1 to

M

epis do

5:

6:

7:

8:

9:

10:

11:

12:

13:

14:

15:

16:

17:

18:

19:

20:

21:

22:

23:

24:

25:

26:

DataGenerator(

ρ

);

Add x m 0 to X 0, set R m = 0, set L to I ;

data ∼ x m 0 , A 0 = simulator.reset(data);

for t=0 to T do for each i ∈ L do

Store p i t in p m , remove a i t from A t ;

a i t , p i t = actor network( xt , A i t );

end for xt + 1, A t + 1, rt , t c = simulator.step( at );

Empty set

L

;

for each i ∈ I do if a i t is complete at t c then

add i to L

else a i

t

end if

+

=

end for

R m = R m + rt ;

end for calculate V m ( x m 0 ; θ ) c using critic

end for

d

θ

M

epis

a

=

(

R m

-

V m

(

x m

;

θ ))

27:

∑

d

θ

c

1

M

epis

m

1

0

c

from

θ

A

t

+

log

1

p m

;

=

∑

=

M

epis

m

1

M

epis

1

θ

c

(

R m

V m

(

a

x m

∇

=

∇

-

;

0

θ ))

c

;

28: end for node we set the initial charging levels of EVs randomly between 1 and 5. We assume that EVs do not need charging and can be directly taken to demander nodes if their charging levels exceed 3. Otherwise, EVs first need to be taken to charger nodes, where all of them are charged until the charging level of 5 is reached. For each charging level, we assign the charging time equal to the average traveling time between all pairs of nodes in the network. We do not consider discharging rates in the movements of EVs, while we assume the constant velocity for EVs equal to 45 miles/hour.

Computational experiments are conducted with 2 Intel Xeon E5-2630 2.2 GHz 20-Core Processors, 32 GB RAM, and the Ubuntu 18.04.4 LTS operating system. All implementations are done in Python 3.7 using PyTorch 1.5. Our implementations of the critic network have similarities to the actor network structure except for the use of LSTM. We first embed the initial static state of the urban network using 1D convolution networks and then pass it to the attention mechanism. We pass the output of the attention mechanism through a sequence of feed-forward networks to obtain the final estimate for a value function. Table II represents the hyperparameters used for training, which are the same as in [10]. We train RL agents on networks of various sizes and difficulty levels.

2

1

a i

t

and remove a i

t

## HYPERPARAMTER VALUES

For each problem class defined by the size of a network, we consider instances with three different levels of difficulty. Cases when there is an abundant presence of charging stations than the number of EVs requiring charging we call easy instances. Similarly, cases when there is an exact number of charging stations as the number of demander nodes we call medium difficulty instances. Finally, in cases when there is a less number of charging stations than the number of demander nodes, we call them hard instances. The descriptions of difficulty levels are found in Table III.

## B. RL Agents and Benchmarks

We train three types of agents using the proposed RL models. The first agent denoted as gen-RL is trained on all three difficulty levels, but on a fixed network size. The second agent denoted as net-RL is trained on networks of various sizes, but it is tailored to a specific difficulty level. The last agent denoted as RL is trained on a fixed network size and on a specific difficulty level. For our computational studies, we consider a benchmark from [8]. The benchmark model denotes as Sim represents a joint model that solves the EVs relocation and the shuttle routing problems simultaneously. To solve multi-shuttle routing problems, the heuristic splits an urban network into some clusters and solves a single-shuttle routing problem for each cluster. However, there are some limitations to the method. One of them is related to the inflexibility of the solutions when drivers that have been dropped off from one shuttle cannot be picked up by other shuttles. Another disadvantage is related to charger nodes. The heuristic can only handle cases when the number of charger nodes is not less than the number of EVs that must be charged.

## C. Results on Random Instances

Figure 6 shows training rewards for the multi-shuttle problems on the network with 23 nodes and 3 drivers. Overall, training time depends on the network size, its structure, and the features passed to the actor network. Using distance information from the current node to other nodes in the actor network results in better rewards compared to when not passing such information.

To compare different RL agents' performances, we conduct experiments on various network sizes and the degree of difficulty of instances and measure the mean of the total time spent in the system out of 128 instances. Table IV shows the experiments' results. In most instances, an RL agent trained on a specific size and a specific instance difficulty level tends to perform the best. We observe that net-RL agents, trained on various network sizes, tend to perform better on larger network sizes, while gen-RL agents, trained on various difficulty levels, can be competitive on medium-sized networks. As the network

DIFFICULTY LEVELS DESCRIPTION, WHERE De , Ch , Su , AND Su ′ DENOTE THE SET OF DEMANDERS, CHARGERS, SUPPLIERS, AND SUPPLIERS WITH EVs THAT REQUIRE CHARGING, RESPECTIVELY

TABLE IV

COMPARISONOF RL AGENTS IN TERMS OF TOTAL TIME SPENT IN THE SYSTEM, THE AVERAGE OF 128 TEST INSTANCES ARE REPORTED. IN BOLD ARE THE BEST RESULTS

![Image](image_000005_6e8bc51373d22557c0b9afd1f304dfd131d6c2b7414683ed5f487a01a59af891.png)

of 128 test instances. As shown in Table V the RL model performs better than the heuristic method in at least 50% of all instances, except one instance.

size increases, the results show that using net-RL and gen-RL agents can be beneficial. For the rest of the experiments, we use RL agents.

Table V illustrates the performance of the RL solutions with those of the heuristic optimization method, labeled Sim. The reinforcement learning approach can solve all instances of the problem, while the optimization method can handle only easy and medium cases. Moreover, for easy and medium cases measured in the mean of total time spent in the system, the RL solutions perform better than the heuristic optimization solutions. We also note that the derived RL solutions do not solve for optimal relocation of EVs and are only based on predefined rules, while the optimization heuristic solves for both the shuttle routing and EV relocation problems.

Table V shows the performance comparison of Sim and RL models in terms of percentages of winning instances. For instance, in an RL-Sim pair comparison, the value of cells under the column indicates the percentages of instances when the RL model performed at least equally to Sim model out

To show the generation of the instantaneous solutions using RL models, we measured computation time. Table V demonstrates the computation time it takes to derive a solution under Sim and RL models. We report an average time to solve an instance out of 128 instances in total. The difference in deriving solutions between Sim and RL models increases up to 585 times in the case of a single shuttle routing in a network with 100 nodes for Easy instances.

We also compare the effects of the number of drivers and difficulty levels on the trained models. In particular, we train models with a specific number of drivers on easy, medium, and hard instances on a fixed network size and check these models' performances against the models with varying a number of drivers and difficulty levels. For example, in Table VI rows indicate the problems' configurations in testing and columns indicate the problems' configurations in training datasets. The cells corresponding to a row and column show the percentages of instances when a trained model outperformed the model specifically trained for a test dataset. As we observe, models trained on specific difficulty levels tend to perform better on similar instances with a different number of drivers compared to on test models with the same number of drivers, but different difficulty levels.

The sample solution for a single-shuttle case, where 4 EVs in an urban network require charging, is shown in Figure 7. A shuttle with 3 drivers leaves the depot and visits supplier nodes first, followed by a charger node. By interchangeably visiting nodes thorough the network, the shuttle returns to the depot after picking up drivers from demander nodes. We can observe the versatility of the produced solutions by looking

RL MODEL VS. THE HEURISTIC OPTIMIZATION IN TERMS OF TOTAL TIME SPENT IN THE SYSTEM, THE PERCENTAGES OF WINNING INSTANCES AND COMPUTATIONAL TIME IN SECONDS, THE AVERAGE OF 128 TEST INSTANCES ARE REPORTED. IN BOLD ARE THE BEST RESULTS

![Image](image_000006_941f5fff40b326f500db7df408a83418e93758bea28aceb62c73275c0123976f.png)

TABLE VI THE NUMBER OF DRIVERS VS. DIFFICULTY LEVELS

![Image](image_000007_7fa09b1295c11567dd86afedc244d0657f53de2eeaca3ffcb0f46b3c9cee86cb.png)

TABLE VII

THE AMSTERDAMDATASET STRUCTURE AND RL MODEL VS. SIM ON THE DATASET IN TERMS OF TOTAL TIME SPENT IN THE SYSTEM

at the charging stations. For instance, a driver dropped off at the first visited supplier node relocates the EV to a charging station, waits there until the EV is charged, and then relocates it to a demander node. Only then the driver is picked up by a shuttle. In another example, the driver dropped off at the second visited supplier node is picked up immediately at a designated charging station by a shuttle. Similarly, Figure 8 represents the sample solution for the case with 2 shuttles. Each shuttle visits supplier nodes first until it runs out of drivers. Then each of them interchangeably visits charger, supplier, and demander nodes and returns to the depot. The flexibility of the produced solutions can be observed when a driver originally dropped at the second visited supplier node by the first shuttle is picked up at a charging station by the second shuttle.

## D. Results on the Amsterdam Cases

We also use real data of FFEVSS representing car2go operations in Amsterdam, the Netherlands, which was collected between May 5th and October 29th, 2016. From the

Fig. 8. Example solution for a multiple-shuttle cases, | N | = 23, | Dr | = 3 and | I | = 2.

![Image](image_000008_c6511d5bb8d9cfbe1f62abdfaeac033dd2e6989eafe0555ca7cb2798b2379348.png)

actual data, we collect locations of supplier, demander, and charger nodes and reduce the network by removing EVs that do not need relocation/charging. We also group the data into weekdays and weekends, which results in 14 and 12 instances for the respective groups that we use as test data for the experiments. To train an RL agent, we generate on-the-fly training data by sampling locations of nodes using weekdays data by extracting the CDF of the distribution for nodes' coordinates. We also assign node types randomly by following the similar structure observed in weekdays data. In particular, the training and testing data have 170 nodes with 71 supplier, 71 demander, and 27 charger nodes. Table VII presents the results of the RL agent performance on the Amsterdam dataset. In all instances with except one, the RL agent performs better as compared to the heuristic; overall, RL performs 6.17%

better. The poor performance of the RL agent in the instance with a single shuttle and 5 drivers on weekends may be improved by training with more weekend data.

## VI. CONCLUSION

This study solves the shuttle routing problem for FFEVSS. We consider a static network, in which a group of drivers is deployed to relocate EVs from supplier nodes to charger and demander nodes. We propose a reinforcement learning approach to learn routing policies for single-shuttle and multi-shuttle cases. The proposed solution methods allow solving the new class of problem instances while demonstrating improved results on instances solvable by existing methods in the literature. We also present several RL agents that generalize on various network structures or network sizes, and we demonstrate that the RL agent specifically trained on a network produces superior results. As future work, it is interesting to consider a dynamic network with the presence of customer demand to rebalance FFEVSS in the daytime.

## REFERENCES

- [1] (2021). The Electric Drive in Montreal . Accessed: Feb. 24, 2021. [Online]. Available: https://www.car2go.com/NA/en/nextgen/
- [2] (2021). I Have Got 1, 500 Cars Here in Berlin . Accessed: Feb. 24, 2021. [Online]. Available: https://www.volkswagenag. com/en/news/stories/2019/06/ive-got-1500-cars-here-in-berlin.html
- [3] (2021). Do You Know: What a 'Relocation' is and Which Car2gos are Relocated? Accessed: Feb. 24, 2021. [Online]. Available: https://blog.car2go.com/2017/06/26/know-relocation-car2gos-relocated/
- [4] Y. M. Chianese, A. Avenali, R. Gambuti, and L. Palagi, 'One-way free floating car-sharing: Applying vehicle-generated data to assess the market demand potential of urban zones,' in Proc. IEEE 41st Annu. Comput. Softw. Appl. Conf. (COMPSAC) , vol. 2, Jul. 2017, pp. 777-782.
- [5] D. Kypriadis, G. Pantziou, C. Konstantopoulos, and D. Gavalas, 'Minimum walking static repositioning in free-floating electric car-sharing systems,' in Proc. 21st Int. Conf. Intell. Transp. Syst. (ITSC) , Nov. 2018, pp. 1540-1545.
- [6] A. G. Santos, P. G. L. Cândido, A. F. Balardino, and W. Herbawi, 'Vehicle relocation problem in free floating carsharing using multiple shuttles,' in Proc. IEEE Congr. Evol. Comput. (CEC) , Jun. 2017, pp. 2544-2551.
- [7] C. A. Folkestad, N. Hansen, K. Fagerholt, H. Andersson, and G. Pantuso, 'Optimal charging and repositioning of electric vehicles in a freefloating carsharing system,' Comput. Oper. Res. , vol. 113, Jan. 2020, Art. no. 104771.
- [8] Z. Haider, H. Charkhgard, S. W. Kim, and C. Kwon, 'Optimizing the relocation operations of free-floating electric vehicle sharing systems,' Univ. South Florida, Tampa, FL, USA, Tech. Rep., 2019, doi: 10.2139/ssrn.3480725.
- [9] I. Bello, H. Pham, Q. V. Le, M. Norouzi, and S. Bengio, 'Neural combinatorial optimization with reinforcement learning,' 2016, arXiv:1611.09940 . [Online]. Available: http://arxiv.org/abs/1611.09940
- [10] M. Nazari, A. Oroojlooy, L. Snyder, and M. Takác, 'Reinforcement learning for solving the vehicle routing problem,' in Proc. Adv. Neural Inf. Process. Syst. , 2018, pp. 9839-9849.
- [11] A. K. Kalakanti, S. Verma, T. Paul, and T. Yoshida, 'RL SolVeR pro: Reinforcement learning for solving vehicle routing problem,' in Proc. 1st Int. Conf. Artif. Intell. Data Sci. (AiDAS) , Sep. 2019, pp. 94-99.
- [12] J. J. Q. Yu, W. Yu, and J. Gu, 'Online vehicle routing with neural combinatorial optimization and deep reinforcement learning,' IEEE Trans. Intell. Transp. Syst. , vol. 20, no. 10, pp. 3806-3817, Oct. 2019.
- [13] F. Schulte and S. Vo§, 'Decision support for environmental-friendly vehicle relocations in free-floating car sharing systems: The case of car2go,' Procedia CIRP , vol. 30, pp. 275-280, Jan. 2015.
- [14] S. Herrmann, F. Schulte, and S. Vo§, 'Increasing acceptance of freefloating car sharing systems using smart relocation strategies: A survey based study of car2go hamburg,' in Proc. Int. Conf. Comput. Logistics . Cham, Switzerland: Springer, 2014, pp. 151-162.
- [15] S. Weikl and K. Bogenberger, 'Relocation strategies and algorithms for free-floating car sharing systems,' IEEE Intell. Transp. Syst. Mag. , vol. 5, no. 4, pp. 100-111, Oct. 2013.
- [16] S. Weikl and K. Bogenberger, 'A practice-ready relocation model for free-floating carsharing systems with electric vehicles-Mesoscopic approach and field trial results,' Transp. Res. C, Emerg. Technol. , vol. 57, pp. 206-223, Aug. 2015.
- [17] M. Zhao, X. Li, J. Yin, J. Cui, L. Yang, and S. An, 'An integrated framework for electric vehicle rebalancing and staff relocation in one-way carsharing systems: Model formulation and Lagrangian relaxation-based solution approach,' Transp. Res. B, Methodol. , vol. 117, pp. 542-572, Nov. 2018.
- [18] L. He, G. Ma, W. Qi, and X. Wang, 'Charging an electric vehicle-sharing fleet,' Manuf. Service Oper. Manage. , vol. 23, no. 2, pp. 471-487, 2021.
- [19] J. Shi, Y. Gao, W. Wang, N. Yu, and P. A. Ioannou, 'Operating electric vehicle fleet for ride-hailing services with reinforcement learning,' IEEE Trans. Intell. Transp. Syst. , vol. 21, no. 11, pp. 4822-4834, Nov. 2020.
- [20] K. Lin, R. Zhao, Z. Xu, and J. Zhou, 'Efficient large-scale fleet management via multi-agent deep reinforcement learning,' in Proc. 24th ACM SIGKDD Int. Conf. Knowl. Discovery Data Mining , Jul. 2018, pp. 1774-1783.
- [21] J. Wen, J. Zhao, and P. Jaillet, 'Rebalancing shared mobility-on-demand systems: A reinforcement learning approach,' in Proc. IEEE 20th Int. Conf. Intell. Transp. Syst. (ITSC) , Oct. 2017, pp. 220-225.
- [22] N. Sadeghianpourhamami, J. Deleu, and C. Develder, 'Achieving scalable model-free demand response in charging an electric vehicle fleet with reinforcement learning,' in Proc. 9th Int. Conf. Future Energy Syst. , Jun. 2018, pp. 411-413.
- [23] I. Sutskever, O. Vinyals, and Q. V. Le, 'Sequence to sequence learning with neural networks,' in Proc. Adv. Neural Inf. Process. Syst. , 2014, pp. 3104-3112.
- [24] O. Vinyals, S. Bengio, and M. Kudlur, 'Order matters: Sequence to sequence for sets,' 2015, arXiv:1511.06391 . [Online]. Available: http://arxiv.org/abs/1511.06391
- [25] W. Kool, H. van Hoof, and M. Welling, 'Attention, learn to solve routing problems!,' 2018, arXiv:1803.08475 . [Online]. Available: http://arxiv.org/abs/1803.08475
- [26] A. Vaswani et al. , 'Attention is all you need,' in Proc. Adv. Neural Inf. Process. Syst. , 2017, pp. 5998-6008.
- [27] J. Zhao, M. Mao, X. Zhao, and J. Zou, 'A hybrid of deep reinforcement learning and local search for the vehicle routing problems,' IEEE Trans. Intell. Transp. Syst. , early access, Jul. 15, 2020, doi: 10.1109/TITS.2020.3003163.
- [28] L. Bu¸ soniu, R. Babuška, and B. De Schutter, 'Multi-agent reinforcement learning: An overview,' in Innovations in Multi-Agent Systems and Applications-1 . Berlin, Germany: Springer, 2010, pp. 183-221.
- [29] R. Lowe, Y. Wu, A. Tamar, J. Harb, O. P. Abbeel, and I. Mordatch, 'Multi-agent actor-critic for mixed cooperative-competitive environments,' in Proc. Adv. Neural Inf. Process. Syst. , 2017, pp. 6379-6390.
- [30] J. K. Gupta, M. Egorov, and M. Kochenderfer, 'Cooperative multi-agent control using deep reinforcement learning,' in Proc. Int. Conf. Auton. Agents Multiagent Syst. Cham, Switzerland: Springer, 2017, pp. 66-83.
- [31] J. N. Foerster, G. Farquhar, T. Afouras, N. Nardelli, and S. Whiteson, 'Counterfactual multi-agent policy gradients,' in Proc. 32nd AAAI Conf. Artif. Intell. , 2018, pp. 1-9.
- [32] F. A. Oliehoek and C. Amato, A Concise Introduction to Decentralized POMDPs , vol. 1. Cham, Switzerland: Springer, 2016.
- [33] B. Perozzi, R. Al-Rfou, and S. Skiena, 'DeepWalk: Online learning of social representations,' in Proc. 20th ACM SIGKDD Int. Conf. Knowl. Discovery Data Mining , Aug. 2014, pp. 701-710.
- [34] R. J. Williams, 'Simple statistical gradient-following algorithms for connectionist reinforcement learning,' Mach. Learn. , vol. 8, nos. 3-4, pp. 229-256, May 1992.

![Image](image_000009_fb74be328ac20897473cb94f13f58fd8f0f1b6b982ca723dc7705b92267c74a8.png)

Aigerim Bogyrbayeva received the B.S. degree in industrial and operations engineering from the University of Michigan. She is currently pursuing the Ph.D. degree with the Department of Industrial and Management Systems Engineering, University of South Florida. Her research interests include reinforcement learning and transportation.

![Image](image_000010_60d4e347c9a92737d779226f409032d5c70e43831de198e6df211e21e2f191cc.png)

Sungwook Jang received the B.S. degree from the Department of Mathematical Sciences, Korea Advanced Institute of Science and Technology (KAIST), in 2019, and the M.S. degree in industrial and systems engineering from KAIST in 2021, where he is currently pursuing the Ph.D. degree in industrial and systems engineering. His research interests include AMHS systems, vehicle routing problems, and reinforcement learning.

![Image](image_000011_427d20a77cf1963c9961424e35faa306a200bceb76aca8ed208ead9d139be4c4.png)

Ankit Shah received the B.S. degree in computer science from Florida Atlantic University, Boca Raton, FL, USA, in 2001, and the M.S. degree in operations research and the interdisciplinary Ph.D. degree in information technology from George Mason University, Fairfax, VA, USA, in 2016 and 2019, respectively. He is currently an Assistant Professor of industrial and management systems engineering with the University of South Florida (USF). He also holds a courtesy faculty appointment of an Assistant Professor with the

Computer Science and Engineering Department. He is also the Director of the Artificial Intelligence Research Laboratory for Secure Systems, USF, and a member of the USF Institute for Artificial Intelligence + X. He is also a USF I-Corps Fellow. Prior to joining USF, he was a Researcher in the field of reinforcement learning with the Lawrence Livermore National Laboratory in CA, USA, and the Center for Secure Information Systems in VA, USA. He has over nine years of industry experience in the information technology and security sector. His research interests lie at the intersection of computer science, operations research, and information technology, with a strong focus on the development of artificial intelligence frameworks for sequential decision-making problems under uncertainty. His current research works are in the area of deep reinforcement learning and adversarial machine learning for threat reduction and secure systems.

![Image](image_000012_e39d64dd00958af76d3468c5bf2b1816e8434e8aae2d3809530f1028ee513f0a.png)

Young Jae Jang received the B.S. degree in aerospace engineering from Boston University in 1997, and the double M.S. degree in mechanical engineering and operations research and the Ph.D. degree in mechanical engineering from the Massachusetts Institute of Technology (MIT) in 2001 and 2007, respectively. He is currently an Associate Professor with the Industrial and Systems Engineering Department, Korea Advanced Institute of Science and Technology (KAIST), South Korea. He is also an Associate Editor of Computers and Industrial

Engineering journal and IEEE TRANSACTIONS ON AUTOMATION SCIENCE AND ENGINEERING.

![Image](image_000013_1f0559a1c06000e6b0986fa77cc65b86297333ed0cdf896f61bc9e406023ad49.png)

Changhyun Kwon received the B.S. degree in mechanical engineering from the Korea Advanced Institute of Science and Technology (KAIST) in 2000 and the M.S. degree in industrial engineering and operations research and the Ph.D. degree in industrial engineering from The Pennsylvania State University in 2005 and 2008, respectively. He is currently an Associate Professor in industrial and management systems engineering with the University of South Florida. His research interests include transportation systems analysis and service opera- tions problems. He is an Active Member of INFORMS and TRB. He received the NSF CAREER Award in 2014. Before he joined the University of South Florida, he has been with University at Buffalo, where he received the UB Exceptional Scholar: Young Investigator Award in 2015.