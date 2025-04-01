## Machine Learning to Solve Vehicle Routing Problems: A Survey

Aigerim Bogyrbayeva , Meraryslan Meraliyev , Taukekhan Mustakhov, and Bissenbay Dauletbayev

Abstract -This paper provides a systematic overview of machine learning methods applied to solve NP-hard Vehicle Routing Problems (VRPs). Recently, there has been great interest from both the machine learning and operations research communities in solving VRPs either through pure learning methods or by combining them with traditional handcrafted heuristics. We present a taxonomy of studies on learning paradigms, solution structures, underlying models, and algorithms. Detailed results of state-of-the-art methods are presented, demonstrating their competitiveness with traditional approaches. The survey highlights the advantages of the machine learning-based models that aim to exploit the symmetry of VRP solutions. The paper outlines future research directions to incorporate learning-based solutions to address the challenges of modern transportation systems.

Index Terms -Reinforcement learning, supervised learning, neural combinatorial optimization, vehicle routing.

## I. INTRODUCTION

C OST-EFFECTIVE logistics systems define the competitiveness of companies, and the relation of logistics expenditure to GDP indicates the effectiveness of business operations in a country. For instance, in 2018, USA businesses spent 10.4% of their revenue on transportation costs alone, while the overall logistics expenditure constituted 8% of GDP [1]. Along with increased fuel prices, transportation costs are mainly influenced by last-mile delivery, defined as transporting goods from a warehouse to a customer's location. With the increased demand for online sales, the last-mile delivery system's effectiveness has become essential as it comprises 50% of the total transportation costs [2]. Also, the carbon dioxide footprint is another emerging concern in logistics systems. To overcome the above-outlined challenges, efficiently solving vehicle routing problems has been of great interest to both practitioners and researchers.

The Vehicle Routing Problem, in general, is defined on either a directed or undirected graph G V E ( , ) consisting of a set of nodes V = { v , . . . , v 0 n } and a set of edges or arcs.

Manuscript received 10 October 2022; revised 23 May 2023 and 8 September 2023; accepted 8 November 2023. Date of publication 2 January 2024; date of current version 31 May 2024. This work was supported by the funding for Young Scientists' Scientific and Technical Projects for 2023-2025, provided by the Ministry of Science and Higher Education of the Republic of Kazakhstan under Grant IRN AP19575607. The Associate Editor for this article was M. A. Khan. (Corresponding author: Meraryslan Meraliyev.)

The authors are with the Department of Computer Science, SDU University, 040900 Kaskelen, Kazakhstan (e-mail: aigerim.bogyrbayeva@sdu.edu.kz; meraryslan.meraliyev@sdu.edu.kz; taukekhan.mustakhov@sdu.edu.kz; b.dauletbayev@sdu.edu.kz).

Digital Object Identifier 10.1109/TITS.2023.3334976

We focus on an undirected graph with a set of edges E = { (v , v i j ) : i &lt; j , v i , v j ∈ V } . VRP aims to construct routes with minimal total cost for M identical vehicles leaving from a depot, v 0, to visit all nodes, V \ v 0, representing customers with non-negative demand d v , and return to the depot, where each customer is visited only once. Different constraints are added to VRP to reflect the realistic routing challenges faced by delivery companies [3]. In the Capacitated Vehicle Routing Problem (CVRP), each vehicle is subjected to a maximum capacity Qm such that the total demand of visited customers in its route does not exceed Qm . In Vehicle Routing Problems with Time Windows (VRPTW), vehicles must arrive at the customer location within specified time windows. However, all the above variations of the VRP belong to vertex routing problems. Some applications, such as mail delivery and bus routing, are concerned with visiting arcs called arc routing problems (ARPs). The key difference between vertex and arc routing problems is that the former involves finding a path between a set of vertices, while the latter involves finding a path between a set of arcs or edges.

As generalizations of the Traveling Salesman Problem (TSP), VRPs belong to the family of Combinatorial Optimization (CO) problems and are known to be NP-hard [4]. Therefore, approximation or heuristic algorithms have been developed to solve large-size VRPs [5], [6], [7], while Mixed Integer Programming (MIP) is used to find exact solutions on small instances. The handcrafted heuristics are built on the experts' domain knowledge about the specifics of the problem's structure. Even though such heuristics often produce solutions in a short amount of time, they fail to generalize to slight changes in the inputs and must be solved from scratch [8].

On the other hand, machine learning methods have shown great success across many applications due to advancements in algorithms and hardware. Machine Learning, in general, can be viewed as applied statistics, where the computational power of computers is used to learn approximate functions instead of explicitly writing computer programs. The main advantage of machine learning models is their ability to generalize to input problems coming from identical distributions and produce solutions instantaneously [9]. This can be invaluable in practice to solve online routing problems with dynamically changing inputs. Therefore, there has been a surge of studies in recent years aiming to develop novel models and training algorithms for routing problems using machine learning methods to achieve near-optimal solutions.

1558-0016 © 2024 IEEE. Personal use is permitted, but republication/redistribution requires IEEE permission. See https://www.ieee.org/publications/rights/index.html for more information.

There are two main learning paradigms used for solving VRPs. In supervised learning (SL), a model is trained on a training dataset consisting of input problems and corresponding solutions. In Reinforcement Learning (RL), VRPs are sequential decision-making problems and rely on the Markov Decision Process (MDP) to construct solutions. However, pure learning-based heuristics have disadvantages such as generalization, poor solution quality, and scalability, leading to new methods that combine traditional heuristics with learning methods. An increasingly growing literature has attracted the attention of both machine learning and operations research communities [10]. Therefore, this survey aims to systematically present the recent advancements in learning methods to solve routing problems and discuss the challenges and opportunities for future research. We believe the survey will benefit researchers interested in solving VRPs either using pure learning methods or hybrid methods that combine learning and construction heuristics.

There are a few studies that focus on machine learning methods used to solve VRPs. For instance, [10] presents general methodological frameworks to merge machine learning and optimization methods to solve CO problems, omitting problem-specific approaches for VRPs. Similarly, [11] and [12] focus on only Deep Reinforcement Learning (DRL) approaches to tackle a set of well-known CO problems, while [13] and [14] discuss machine learning approaches to solve CO problems in networking and manufacturing, respectively. Reference [15] limit their survey to Graph Neural Networks applications to solve CO problems, and [16] focuses only on TSP. Reference [17]vprovides an overview of data analytics and machine learning approaches used for modeling and optimization of VRPs. With the heavy focus on hybrid methods,the paper does not provide any computational studies to compare recently proposed models and demonstrate stateof-the-art results. Although [18] incorporates experimental analysis, it falls short of encompassing the recent studies. In contrast, this study includes a comprehensive computational study, including a large number of models, their variants, and different problem types using well-known datasets. Also, we provide the detailed modeling of single and multiple VRPs used in many realistic VRP variants that have also been missed in [18]. Nevertheless, we consider our work to be complementary to the existing studies.

In total, our contributions can be summarized as follows: i) this survey comprehensively summarizes the most recent machine learning approaches to solve various VRPs, including both pure end-to-end learning and hybrid methods; ii) we present the computational studies to compare the surveyed paper results on random and well-known benchmark instances to demonstrate the state-of-the-art.

The survey indicates the benefits of learning-based models to solve medium-sized graphs consisting of 100 nodes, to produce comparable solutions with the optimization methods with less computational time. Within learning-based models, both pure end-to-end and hybrid models are comparable in terms of solution quality. In both methods, the studies focused on the symmetry of VRP solutions perform better as compared to the studies that ignore such properties. However, the majority of the studies focus on routing a single vehicle with fully known inputs. This is in contrast with real-world applications of VRPs that often involve routing a fleet of vehicles with dynamic inputs changing over time, indicating challenging yet important future research directions.

We have the following outline for the paper: in Section II, we present the central taxonomy and discuss end-to-end learning methods; in Section III, we focus on hybrid methods and present different solution structures; in Section IV we present single and multiple VRP formulations; in Section V, we present our experimental results; in Section VI, we discuss the future research directions; in Section VII, we finalize with the concluding remarks.

## II. TAXONOMY OF LEARNING METHODS FOR ROUTING PROBLEMS

It is challenging to classify the wide range of learning methods used to solve routing problems using a single classification. However, one useful classification is based on the structure of the solutions, which distinguishes between solutions that use only learning methods and those that combine learning methods with existing non-learning methods. In endto-end learning methods, either SL or RL is used to solve a problem from the beginning until the final solution. In hybrid methods, learning methods are used either as a primary method to construct feasible and efficient solutions, which will later be further improved with construction heuristics, or learning methods facilitate solving the inner problems of the existing non-learning methods to solve routing problems.

One way to classify routing problems is based on the number of vehicles involved, either single-vehicle or multi-vehicle. Single-vehicle routing problems (SVRPs) involve finding the optimal route for a single vehicle to visit a set of locations, while multi-vehicle problems require addressing the challenge of routing multiple vehicles. A central controller is commonly employed to manage a fleet of heterogeneous or homogeneous vehicles. Since most studies focus on routing a single vehicle, the underlying problems are commonly referred to as TSP, CVRP, and VRPTW, while mTSP, mCVRP, and mVRPTW are used to indicate the presence of multiple vehicles in corresponding problems.

## A. End-to-End SL for VRPs

In end-to-end models, purely learning methods construct a solution without any assistance from non-learning methods. SL requires having high-quality VRP solutions as labels that guide a model to find efficient solutions. Furthermore, depending on how the solutions are produced, we can divide them into autoregressive (AR) and non-autoregressive (NAR) categories. In AR SL methods, the routes are constructed step by step, one node at a time, while in NAR methods, the routes are constructed in a zero-shot manner.

In the case of TSP, to produce an NAR solution, the HeldKalp [19] algorithm or well-known solvers such as Concorde [20] and LKH3 [21] are used to generate optimal solutions. Thus, for a given graph G V E ( , ) in the two-dimensional Euclidean space, the output of the solver, π ∗ , is decomposed

TABLE I THE SUMMARY OF END-TO-END SL METHODS WITH AUTOREGRESSIVE (AR) AND NONE-AUTOREGRESSIVE (NAR) SOLUTIONS

into the adjacency matrix, where for each ei , j ∈ E we can compute the probability pi , j :

samples of the tuple { St , At , Rt t , + n , St + } n , where Rt t , + n = ∑ n k = 0 rt + k :

$$p _ { i, j } = \begin{cases} 1, & \text{if } e _ { i, j } \text{ is in } \pi ^ { * } \\ 0, & \text{otherwise.} \end{cases} \quad ( 1 )$$

Following that, a deep learning model with a set of parameters θ , given the graph, is trained to produce an adjacency matrix with probabilities ˆ pi , j ∀ ei , j ∈ E . The loss is then defined as a binary cross-entropy :

$$\mathcal { L } ( \theta ) = p _ { i, j } \log \hat { p } _ { i, j } - ( 1 - p _ { i, j } ) \log ( 1 - \hat { p } _ { i, j } ) \quad ( 2 ) \ \text{param} \mathfrak { \ } \Phi _ { \Phi \cdot \Phi \cdot }$$

The adjacency matrix produced by the model can either follow a greedy search by picking edges with the most significant probabilities or be coupled with beam search by preserving some number of candidate tours to be selected as the final solution.

On the other hand, an exact or high-quality approximation solution π ∗ = [ v , . . . , v 0 n ] is fed to a deep learning model to produce partial tours to maximize the conditional probability given below in AR SL methods :

$$\theta = \arg \max \log p ( \pi | \pi ^ { * }, \theta ) \text{ \quad \ \ } ( 3 )$$

$$p ( \pi | \pi ^ { * }, \theta ) = \prod _ { i = 1 } ^ { n } p ( v _ { i } | v _ { 0 }, \dots, v _ { i - 1 }, \pi ^ { * }, \theta ). \quad ( 4 ) \quad \text{where} \\ \quad \text{to learn}$$

Both methods utilize advanced deep learning models such as Graph Neural Networks (GNNs) [22] and Graph Convolution Networks [23] to extract features of a graph and deploy Memory Augmented Neural Networks [24] and Recurrent Neural Networks (RNNs) [25] to pass sequential information. Both methods require training separate sets of parameters for different graph sizes to produce near-optimal solutions for TSP [26], [27]. Table I summarizes studies focused on end-to-end SL.

## B. End-to-End DRL for VRPs

While tabular RL methods are mostly used in the hybrid setting, in end-to-end methods, DRL is a popular choice due to the combinatorial nature of the action space of VRPs. We can categorize deep RL methods applied to VRPs as value-based and policy-based.

In value-based methods, such as Deep Q-learning for VRPs, neural networks are used to approximate the Q-function for each state and action pair, from which an optimal policy is derived. However, instead of using one-step updates, n-step TD is used as a target to consider delayed rewards. Further, experience replay is used to update Q-values with a batch of

$$\hat { Q } _ { \theta } ( S _ { t }, A _ { t } ) \leftarrow \hat { Q } _ { \theta } ( S _ { t }, A _ { t } ) + \alpha [ R _ { t, t + n } + \gamma \max _ { a } \hat { Q } _ { \theta } \\ \times ( S _ { t + n }, a _ { t + n } ) - \hat { Q } _ { \theta } ( S _ { t }, A _ { t } ) ]. \ ( 5 )$$

Experience replay is a technique used in RL, where a sample of the agent's experiences is stored and replayed during the learning process. Reference [28] points out that sampling a batch of stored experiences to update the neural network parameters results in fast convergence to solve combinatorial optimization problems, which is a general case when neural networks are used as approximation functions [29], [30].

Even though value-based models are generally sample-efficient as compared to policy-gradient methods, the routing policies are complex. Therefore, in most studies, VRP policies are directly learned through policy-gradient methods. In particular, a parametrized policy πθ is approximated using the conditional probabilities and chain rule :

$$\pi _ { \theta } ( a | s ) = \prod _ { t = 0 } ^ { T } p ( a _ { t + 1 } | s _ { t }, Y _ { t } ) \, \text{ \quad \ \ } ( 6 )$$

where Yt is a set of visited nodes. In practice, the actor-critic structure is often used to represent two sets of neural networks to learn πθ and V θ ( S 0 ) respectively, where V θ ( S 0 ) serves as a baseline. Mean Square Error is used to update the parameters of the critic θ c with batch size B using total reward R :

$$\theta _ { c } \leftarrow \theta _ { c } + \alpha [ \frac { 1 } { B } \sum _ { j = 1 } ^ { B } \nabla _ { \theta _ { c } } ( R ^ { j } - \hat { V } _ { \theta _ { c } } ^ { j } ( S _ { 0 } ^ { j } ) ) ^ { 2 } ] \quad \quad ( 7 )$$

Accordingly, the actor parameters are updated using the REINFORCE algorithm with baseline :

$$\theta _ { a } \leftarrow \theta _ { a } + \alpha [ \frac { 1 } { B } \sum _ { j = 1 } ^ { B } \nabla _ { \theta _ { a } } \log \pi _ { \theta _ { a } } ( R ^ { j } - \hat { V } _ { \theta _ { c } } ^ { j } ( S _ { 0 } ) ) ] \quad ( 8 )$$

Some studies [31] use a greedy rollout of the actor with the current set of parameters to avoid the complexities of training the critic network for Equation (7).

To learn stochastic policy πθ using an actor network, an encoder-decoder structure originated from machine translation has shown its effectiveness due to the similar nature of the problems in both fields [32]. For instance, both machine translation and VRPs aim to construct a sequence of words or nodes, given the initial set of words or nodes. The only difference is that the input set of words in machine translation also has a sequential nature. Therefore, an encoder used in

modeling VRPs differs from its original setting in machine translation and serves as a graph embedding responsible for passing the graph structures efficiently to a decoder [9].

Encoders maybe static and dynamic in nature. Static encoders embed a given initial graph only once, embedding both static v s and dynamic v d , elements of the nodes :

$$h _ { v } ^ { s } = F ^ { s } ( v _ { s } ), \ \ h _ { d } ^ { v } = F ^ { d } ( v _ { d } ) \ \forall v \in \mathbb { V }. \quad \ ( 9 ) \ \text{decode} \ \.$$

where F s , F d are series of nonlinear functions, and h s v and h d v are the embeddings of the static and dynamic elements of node v . On the other hand, dynamic encoders embed the graph at each time t reflecting the changes due to the routing decisions on the dynamic parts of the nodes :

$$h _ { v } ^ { d _ { t } } = F ^ { d } ( v _ { d _ { t } } ) \ \forall v \in \mathbb { V }. \quad \quad ( 1 0 ) \ \underset { \text{and} } { \text{move} }$$

The final embedding of a graph ˆ ht is computed as the mean of embedding of all nodes in a graph at time t :

$$\hat { h } _ { t } = \frac { 1 } { | \mathbb { V } | } \sum _ { v \in \mathbb { V } } h _ { v } ^ { t } \quad \quad \quad ( 1 1 ) \quad \stackrel { m } { \text{the} } ^ { t } \text{ and} \\ \text{estif}$$

where h t v is the final embedding of node v . The graph embedding is the same for static encoders across all t . Therefore, static encoders are computed only once per problem instance, while dynamic encoders are invoked at each time step, resulting in considerable computational time. Research conducted in [33] and [34] has demonstrated the effectiveness of dynamic encoders in enhancing the results.

The static and dynamic features of nodes are problem-specific and may include the coordinates of the nodes, the customer demand at each node, and others. Regardless of the types of RL algorithms used, VRPs require learning efficient graph representations that transform discrete and complex information between nodes and edges in a graph into a continuous vector space. Various models have been proposed to learn graph embeddings. For instance, [32] uses RNNs as an encoder as a part of the Pointer Network, which [9] shows to be unnecessary due to the absence of the sequential relationship in the given initial set of nodes in a graph. Independently, [35] proposed a novel graph representation called Structure2Vec (S2V) that can encode both the graph and the partial solution at any time step. Reference [31] proposes fully attention-based encoder, introducing transformers [36] to solve VRPs, while [27], [37], and [38] uses GNNs, a deep learning model dedicated to learn graph information. In return, other studies adopt the proposed encoders, including Pointer Networks [39], multi-head attention [40], [41], [42], [43], [44], recurrent neural networks (RNNs) [45], S2V [46], [47], [48], and others. However, no studies focused on investigating which models are the most efficient in graph embedding to solve VRPs.

A decoder is a dynamic part of the actor-network invoked at each time step that incorporates the current state of a problem with the graph embedding to sample the next action. In particular, the decoder computes compatibility ut at time step t that aims to extract relevant features from the inputs, which are later passed through softmax to produce the probabilities of selecting the next action :

$$u _ { t } = F ( \ddot { h } _ { t }, F ( s _ { t } ) ), \ \pi _ { \theta } ( a | s ) = \text{softmax} ( u _ { t } ) \quad ( 1 2 )$$

where F can be a series of nonlinear transformations. RNNs, which enable the passing of information about the sequence of the partial solution, have been a common choice for a decoder that is followed by additive attention [49] to compute the alignment score between the graph embedding and the current state [9], [32], [50]. A fully attention-based model is another popular design choice for a decoder, where multi-head attention is used to compute the compatibility between the context node representing the state and the graph embedding [31], [33], [41], [51], [52], [53]. Recently, [54] introduced a novel idea of a feature embedding refiner between the encoder and decoder to boost existing models.

Table II summarizes studies with end-to-end RL methods, where due to the episodic nature of VRPs REINFORCE algorithm with a baseline is used. However, the choice of the baseline varies across studies. For instance, [9], [32], [46] and others train a separate neural network called the critic to estimate the baseline, total expected reward, given the initial state of a graph. On the other hand, [31], [42], and others use a training model in a greedy setting to estimate the baseline value. Other policy gradient methods such as Deterministic Policy Gradient and Actor-critic methods have been proposed in [45] and [55]. However, the greedy rollout resulting from a trained end-to-end RL method may not be competitive against handcrafted heuristics. Therefore, it is common to use different search methods with the trained models. One of them is active search , which aims to adopt the trained model parameters to the specifics of an instance, thus improving the solution quality [56]. Another search method is random sampling , indicating the number of total solutions to be sampled, among them, we select the best solution for a reward.

## III. HYBRID METHODS

Hybrid methods are a combination of both learning and non-learning methods to address complex VRPs. Based on the structure of the hybrid method solutions, they can be classified into two groups. The first group involves using learning methods as a supporting tool to address the internal subproblems of non-learning methods. This approach can enhance the efficiency of non-learning methods by leveraging the learning methods to tackle specific subproblems. In the second group, learning methods are the primary tool to produce a solution, which is then improved with construction heuristics. Additionally, optimization layers can be added to learn the costs [63], [64] for training machine learning models. By combining learning methods with construction heuristics, hybrid methods can produce efficient solutions for VRPs. Overall, hybrid methods are a promising approach for addressing the challenges of VRPs by taking advantage of the strengths of both learning and non-learning methods.

## A. Learning Methods for Facilitating Non-Learning Methods

Combining Q-learning with meta-heuristics dates back to [65], which proposed to use ant-colony in a distributed setting

THE SUMMARY OF STUDIES WITH END-TO-END RL METHODS

CSP-COVERING SALESMAN PROBLEM, DVRP-DYNAMIC VRP, PCTSP-PRIZE COLLECTING TSP, SDVRP-SPLIT DELIVERY VRP, SPCTSPSTOCHASTIC PRIZE COLLECTING TSP, OP-ORIENTEERING PROBLEM, MCVRP-MULTIPLE CVRP, MVRPSTW- MULTIPLE VRP WITH SOFT TIME WINDOWS, MAMP-MULTI-AGENT MAPPING PROBLEM, MOTSP-MULTI-OBJECTIVE TSP, MCVRPTW-MULTIPLE CVRP WITH TIME WINDOWS, EVRPTW-ELECTRICAL VEHICLE ROUTING PROBLEM WITH TIME WINDOWS, PDP-PICKUP AND DELIVERY PROBLEM, HCVRP-HETEROGENEOUS CVRP, DU-VRP-DYNAMIC AND UNCERTAIN VRP

(Continued.) THE SUMMARY OF STUDIES WITH END-TO-END RL METHODS

CSP-COVERING SALESMAN PROBLEM, DVRP-DYNAMIC VRP, PCTSP-PRIZE COLLECTING TSP, SDVRP-SPLIT DELIVERY VRP,

SPCTSP-STOCHASTIC PRIZE COLLECTING TSP, OP-ORIENTEERING PROBLEM, MCVRP-MULTIPLE CVRP, MVRPSTW-

MULTIPLE VRP WITH SOFT TIME WINDOWS, MAMP-MULTI-AGENT MAPPING PROBLEM, MOTSP-MULTI-OBJECTIVE

TSP, MCVRPTW-MULTIPLE CVRP WITH TIME WINDOWS, EVRPTW-ELECTRICAL VEHICLE ROUTING PROBLEM

WITH TIME WINDOWS, PDP-PICKUP AND DELIVERY PROBLEM, HCVRP-HETEROGENEOUS CVRP,

DU-VRP-DYNAMIC AND UNCERTAIN VRP

to explore solutions for TSP. In particular, each ant represents an agent to construct solutions and fill a Q-table associated with moving from one city to another. At the same time, the reward function reinforces selecting tours with short lengths. Later, [66] further developed the idea of enhancing cooperation between ants/agents through the introduction of updated pheromone values assigned to each edge in the graph. The study has shown that combining the ant-colony system with local search achieves competitive results in solving TSP.

Reference [67] combines the genetic algorithm with RL to solve TSP, where the Q-learning algorithm is modified along with the mutation operation to produce efficient tours, which later can be enforced with local search. Similarly, [68] combines the genetic algorithm with multi-agent RL, where MARL is used to generate an initial solution which later will be improved by a genetic algorithm. Reference [69] integrated RL into the NSGA-II optimization algorithm by using the Q-learning algorithm for local search. This approach enables the algorithm to generate quality solutions by improving its search process. The authors showed that the use of Q-learning is able to achieve superior performance compared to the Q-learning-based multiobjective evolutionary algorithms with the randomly selected neighborhood. Reference [70] proposed an alternative approach to enhance evolutionary algorithms by utilizing a radial basis function network (RBFN) for population evaluation. The study highlights that a well-trained RBFN can provide a remarkable boost to the overall performance and can expedite the convergence of the proposed model.

LKH3, a unified hybrid genetic search. Also, [72] proposes using [9] model combined with Large Neighborhood Search (LNS) to solve VRPTW in ride-hailing services. They train the neural networks using SL for the insertion operation inside LNS. Reference [73] further enhances the idea of [74] using multi-agent systems to learn which meta-heuristic to apply to solve VRPTW and develops adaptive local search through Q-learning. Q-learning aims to determine the sequence of neighborhoods that need to be explored first to maximize the objective function by picking the actions as different operations available from LNS.

Reference [71] presents a model to use RL for neighborhood search (NS) to solve CVRP and split delivery VRP (SDVRP). In particular, a feasible solution is destroyed by a destroy operator in several locations. Next, a repair operator is trained using RL to connect the partial solutions. In particular, an encoder-decoder model is proposed to learn the embeddings of the end nodes of the partial solutions and produce the probabilities of connecting nodes to construct a complete solution. The results outperform [9], [31] for VRP and SDVRP for different node sizes and show competitive results with

Along with approximated DP methods used to solve VRPs [75], [76], the combination of RL with DP is an emerging research direction. For instance, [77] proposes to combine RL with constraint programming (CP) using a DP approach to solve TSP, VRP, and TSPTW. Deep Q-learning and Proximal Policy Optimization (PPO) are integrated into CP search algorithms. Reference [78] aims to mitigate the curse of dimensionality present in DP by restricting its search space to policies produced by learning methods. Identical to [23], the study generates the heatmap for the edges of a given graph that indicates the probabilities of entering those edges into the solution. Afterwards, this heatmap is used for branching in forwarding DP. References [79] and [80] use neural networks to approximate the value functions for DP, which expedites the solution time. References [81] utilizes a policy iteration algorithm to solve CVRP. The authors propose a simple model consisting of fully connected hidden layers and a ReLU activation function to estimate the value of each state defined as the set of unvisited nodes. Additionally, or policy evaluation, they used MIP to select actions, combining the learned value functions. The resulting MIP is solved using the branch-andcut approach of [82].

Furthermore, clustering algorithms have also been applied to routing problems. These algorithms aim to split the problem into smaller sub-problems and solve them using nonlearning methods. In particular, several recent studies [83], [84], [85], [86], [87] have utilized clustering algorithms to group customers. The resulting clusters are then provided to

non-learning methods to generate solutions considering certain constraints, such as time windows.

Another set of studies uses learning methods to pick heuristics to solve routing problems. For instance, [88], [89] aim to learn improved heuristics. They embed the nodes and positions in a given solution separately, wherein in the middle of the encoding, they are shared with each other, but at the end, the encoder produces different node features and position feature embedding that is passed to the decoder. The decoder produces the matrix of probabilities to select the pair of nodes to use for local operation. Reference [90] proposes the NeuRewriter model, that given an initial solution, picks the region to be modified and applies some learned rewriting rules to improve the solution. Both region-picker and rule-picker policies are trained using RL to solve TSP and CVRP. Similarly, [91] proposes to learn a general heuristic to solve VRP, VRPTW that given an initial solution destroys some part of the solution and learns to reconstruct an improved version of it. The study modifies GNNs to embed both the nodes and edges, which passed through GRU to define the order of the nodes of the newly repaired part for the solution. Also, [92] proposes to delegate subproblems of VRPs to black-box solvers and trains a subproblem generator model based on transformers and SL. In particular, they restrict the possible number of subproblems to solve given an initial solution by looking at only subsets of the visited nodes and restricting the number of routes to be considered by utilizing spatial locality.

## B. Improving Learning Methods With Heuristics

Along with sampling methods such as beam search, active search, and random sampling, trained learning methods can be combined with traditional construction heuristics. In particular, a greedy solution produced by a learning method is used as an initial solution. For instance, [93] solves TSP with the MHA-based encoder and the Pointer Network decoder along with a 2-opt local search. For solving VRP and VRPTW on a large scale, [94] used [9]' model with an adaptive critic later combined with local search algorithms such as OR-tools and LNS. Also, [95] introduces Graph Pointer Nets (GPNs) to solve TSP, where a graph encoder made of GNNs is combined with Pointer Networks. In light of this, the paper proposes a Hierarchical RL to solve TSPTW, where each layer of the neural network learns to solve TSP with some constraint. By combining GPNs with local search, they obtained comparable results with the construction heuristics. They also search real and random data instances of TSPTW and demonstrate comparable results with the construction heuristics. Another study by [96] solves TSP with Time Windows and Rejections, where the objective is to minimize the rejection and distance costs. The paper proposes to use the AM model to solve the regular TSP and deploy a heuristic that will check the feasibility of the produced solution according to Time Windows and determine if an order has been rejected. This will help compute the rewards function correctly according to TSPTWR and update the parameters of NNs using the baseline rollout. Recently, [97] updated their model [88] combined with NS to solve the Pickup and Delivery Traveling Salesman Problem

(PDTSP) more efficiently. They use Neural Neighborhood Search to tackle the precedence constraint quickly by allowing a pair of pickup-delivery nodes to be simultaneously operated in the neighborhood search through two customized removal and reinsertion decoders.

Learning methods can also be combined with prescriptive methods to solve complex routing problems. For instance, [98] discusses the problem of managing a large set of available homogeneous vehicles for online ride-sharing platforms. In this work, the authors propose Contextual DQN and Contextual Actor-Critic networks to learn a state-value function shared by all agents representing each vehicle. The learned state-value functions are used for efficient allocation solved by linear programming.

Another set of studies combines learning methods with Monte-Carlo Tree Search (MCTS). For instance, [99] proposes the idea of training TSP in a supervised manner in small instances using LKH as the optimal solution and then subsequently utilizing the trained model to solve large instances. First, the authors train a graph convolutional residual network with an attention mechanism on a graph with m nodes. Furthermore, they take a larger graph and split it into several graphs with size m , where each node can be present in several graphs. Consequently, they use the trained model for each subgraph to generate the heatmap that shows the probabilities of connecting nodes in the graph. To combine the heatmap and produce a single solution, they sum the probabilities for each arc computed across all the subgraphs, and this summed heatmap is used for MCTS to further improve the solution, similar to [100]. The study solves TSPs with up to 10,000 nodes in a reasonable time. Table III summarizes studies with hybrid approaches, encompassing a wide variety of learning methods, including supervising learning, Q-learning, clustering, and policy gradient methods in combination with MIPs, heuristics, and metaheuristics.

## IV. SINGLE VS. MULTIPLE VRP FORMULATIONS

The majority of VRPs in practice involve routing either multiple heterogeneous or homogeneous vehicles [115]. However, the straightforward application of learning methods to such VRPs is challenging due to the presence of several vehicles in a graph that all can influence the shared environment. This section presents the MDP formulations for routing a single vehicle using CVRP as an example. In addition to the formulation, we present the comparison results for TSP, CVRP, and VRPTW. Finally, we discuss several approaches present in the literature to formulate multiple VRPs.

## A. Single Vehicle Routing Problems

A single vehicle routing problem is defined in a graph G V E ( , ) consisting of a set of nodes V = { v , . . . , v 0 n } and a set of edges E = { (v , v i j ) : i &lt; j , v i , v j ∈ V } , where each node represents depot v 0 or customer's locations V \ v 0. The objective is to find a sequence of nodes to be visited by a vehicle to minimize the total costs, defined as the total distance traveled. Capacity constraints can be added by setting the maximum load l max to be carried by a vehicle, with d v

THE SUMMARY OF STUDIES WITH HYBRID METHODS DISCUSSED IN SECTION IV

CVRP-CAPACITATED VRP, CVRPTW-CVRP WITH TIME WINDOWS, MARL-MULTI-AGENT RL, SDVRP-SPLIT DELIVERY VRP, TSPTW-TSP WITH TIME WINDOWS, TSPTWR-TSP WITH TIME WINDOWS AND REJECTION, VRPTW-VRP WITH TIME WINDOWS, SDDPVD-SAMEDAY DELIVERY PROBLEM WITH VEHICLES AND DRONES, MVRPTW- MULTIPLE VRP WITH TIME WINDOWS, DVRP-DYNAMIC VRP, ATSP-ASYMMETRIC TSP, CMVRPTWMDP-COLLABORATIVE MULTICENTER VRP WITH TIME WINDOWS AND MIXED DELIVERIES AND PICKUPS, TDGVRPTW-TIME-DEPENDENT GREEN VRP WITH TIME WINDOWS, MOVRPSD-MULTI-OBJECTIVE VRP WITH STOCHASTIC DEMAND, FDTSP-FLEXIBLE DRONES TSP

(Continued.) THE SUMMARY OF STUDIES WITH HYBRID METHODS DISCUSSED IN SECTION IV

CVRP-CAPACITATED VRP, CVRPTW-CVRP WITH TIME WINDOWS, MARL-MULTI-AGENT RL, SDVRP-SPLIT DELIVERY VRP, TSPTW-TSP WITH TIME WINDOWS, TSPTWR-TSP WITH TIME WINDOWS AND REJECTION, VRPTW-VRP WITH TIME WINDOWS, SDDPVD-SAME-DAY DELIVERY PROBLEM WITH VEHICLES AND DRONES, MVRPTW- MULTIPLE VRP WITH TIME WINDOWS, DVRP-DYNAMIC VRP, ATSP-ASYMMETRIC TSP,

CMVRPTWMDP-COLLABORATIVE MULTICENTER VRP WITH TIME WINDOWS AND MIXED DELIVERIES AND PICKUPS, TDGVRPTW-

TIME-DEPENDENT GREEN VRP WITH TIME WINDOWS, MO-VRPSD-MULTI-OBJECTIVE VRP WITH STOCHASTIC DEMAND, FDTSP-FLEXIBLE DRONES TSP

representing the customer demand at node v . The problem naturally fits a single-agent RL, where a vehicle represents the agent, and the environment is a simulated CVRP. The agent is trained to select action At ∈ V at each time step t , representing the node index to be visited next given the current state St . A good state representation should fully capture the dynamics of the problem and make future decisions based only on the current state, thus preserving the Markov property. A common choice of a state for CVRP is defined by a tuple St = { at , l t , Yt } , representing the current location of a vehicle, the remaining load, and the set of served customers [9], [31]. At each time step t , we can update St + 1 given the current state St and selected action at .

After each decision at time t , we calculate intermediate reward rt , defined as the distance between the current location at and the node to be visited at + 1, from which we can derive the total reward R = ∑ T t = 0 rt . Single vehicle routing problems, involving routing decisions of only a single vehicle, may come with modifications, such as considering time window constraints [74], [94], [111], allowing split deliveries [9], [71], introducing dynamic demand [46], or generalizing to various forms of TSP [31], [41], [95], [96].

## B. Multi-Vehicle Routing

Multiple vehicle routing problems can also be modeled using the above single-agent RL formulation. For example, when routing M homogeneous vehicles to minimize the total distance traveled, the trained RL agent solution can be split into M vehicles, where each vehicle is allowed to return to the depot only once. However, in VRPs, all vehicles share a common goal of serving customers with the least total costs. Therefore, the idea of using a centralized controller , responsible for routing all vehicles in coordination to achieve a common goal, has been used in many studies.

In a centralized controller setting, the controller is the agent, taking actions for all vehicles at each time step. Formally, the central controller has action state A t + 1 = { A t 1 + 1 , . . . , A M t + 1 } , where M is the number of vehicles and A m t + 1 ∈ V for all m = 1 , . . . , M . The current state St = { A t , l t , Yt } consists of a tuple indicating the last selected actions of all vehicles, the vector of remaining loads for each vehicle, and the set of customers served by any vehicle. The ensuing step involves training the central controller to efficiently construct routes for all vehicles denoted as p = [ p 0 , . . . , pT ] , where pt = [ a t 1 , . . . , a M t ] . Subsequently, for each vehicle m , we have a route p m = [ a m 0 , . . . a m T ] . In case when the objective is to minimize the total time spent to serve all customers, the central controller aims to minimize the total reward defined as :

$$R = \min _ { p } \{ \max \{ c ^ { 1 } ( p ^ { 1 } ), \dots, c ^ { m } ( p ^ { m } ) \} \} \quad \ \ ( 1 3 )$$

where c m ( p m ) = ∑ T -1 t = 0 1 t m a m t , a m t + 1 , where 1 t m a m t , a m t + 1 is the traveling time of vehicle m from node a m t to node a m t + 1 . Unlike the MARL formulation, the central controller observes the entire environment and has full knowledge about the state of each vehicle, allowing it to promote coordinated routing for both homogeneous fleets and heterogeneous vehicles. For instance, all vehicles have common action space for multiple vehicles operating in a shared graph. Without coordination, several vehicles may visit the same customer. The central controller, which assigns actions for each vehicle, prevents such inefficiencies. Studies such as [8], [47], [50], [96], [105], [116], and [117] presented in Table IV use the centralized controller to solve mCVRP and mVRPTW with a homogeneous set of vehicles. Additionally, [46] and [98] propose deep RL to route multiple vehicles with uncertain demand. Another emerging topic is to route a set of heterogeneous vehicles such as trucks and drones [90], [118], also relying on the centralized controller.

THE SUMMARY OF STUDIES TO SOLVE MULTI-VEHICLE ROUTING PROBLEMS. DSCVRPTW - DYNAMIC AND STOCHASTIC CVRPTW, SCVRPTWSTOCHASTIC CVRPTW, MMHCVRP-MIN-MAX HETEROGENEOUS CVRP, MSHCVR-MIN-SUM HETEROGENEOUS CVRP

On the other hand, MARL aims to train fully independent agents which cooperate for a common goal. Formally, each agent m has its local observation denoted as o m t , which includes partial information about the environment. Each agent is trained to select action A m t + 1 given its observation o m t and its state s m t . To promote coordinated routing, communication messages are allowed between agents. Each agent is trained to construct its route p m = [ a m 0 , . . . , a m T ] , contributing to the common reward R = ∑ M m = 1 c m ( p m ), where c m ( p m ) is a cost function. The general framework to apply MARL for routing multiple vehicles is presented in [73]. MARL formulations are suitable in dynamic environments when customer demand is observed over time. For instance, in the case of online routing, vehicles trained in a MARL setting can make independent decisions to serve customers. An example is [57], presents the MARL formulation to solve Multi-agent Mapping Problem to satisfy demand in known locations with unknown quantities.

## V. COMPUTATIONAL STUDIES

In this section, we present the experimental results obtained from evaluating the performances of various models.

Data Generation: To compare the models presented to solve TSP and VRP in random data, we used the benchmark dataset presented in [31], where coordinates of the customer locations are generated using uniform distribution in a unit square. For CVRP, the customer demand at each node is an integer random variable sampled uniformly with values ranging from 1 to 9, and the capacity is set as 30, 40, and 50 for the graphs with 20, 50, and 100 nodes, respectively.

Baselines: We compare end-to-end learning methods [31], [41], [42], [43], hybrid methods [88], [89], [104], [121] in their different configurations to solve CVRP on different graph sizes and use LKH3 as baselines. The implementations and the pre-trained models are taken from the respective shared GitHub repositories. Even though a model-agnostic refiner proposed in [54] shows promising results in boosting [31], [43], in the experiments, we focus on the original models.

Run times: We run our experiments using a single GPU (RTX2080) and 16 cores of CPU (Intel Core i9-9900K 3.60GHz) on Ubuntu 20.04. We set the batch size equal to one in both greedy and sampling decoding strategies if the shared code of the models can support such a setting.

Results: Table V presents the performance comparisons, where the objective represents the average cost of solving 1,000 instances of CVRP. The gap is computed as follows: GAP = Z -Z ∗ Z ∗ , where Z ∗ is the cost of LKH3 algorithm, and Z is the cost of a method. We report the average of the gaps and the total time spent across all test instances. Among learning-based models, the hybrid model of [88] with data augmentation performs slightly better than the rest of the presented studies, but it requires a long running time. CVRP is a challenging problem, where the gaps between learning-based methods and optimization-based algorithms such as LKH3 still exists. However, [43] can solve instances on large sizes with 1.27% gap on average in a fraction of time compared to LKH3.

Even though there have been some studies to solve VRPTW [40], [74], [94], the settings of the problem are different for each study, preventing the comparisons between them. Therefore, in Table VI, we present the comparison between [31] and [51] on the well-known Solomon dataset [122] originally reported in [51]. Table VII, we present the generalization capacity of models on well-known CVRPLib dataset, while OR-Tools [123] is used as a benchmark method to represent the optimization methods. Since all the studies have been evaluated on the same instances, we present the comparison results based on the corresponding papers. In particular, the results of [31] and [43] are based on the report from [88], the results of [121] are based on the reports from [42] and we report the results of [42], [88], and [89] from the original studies. As shown in the tables, on average, [95] performs the best, outperforming OR-Tools and other learning-based methods.

## VI. THE FUTURE RESEARCH DIRECTIONS

Machine learning-based methods for VRPs can provide benefits, such as improved efficiency, real-time decision-making,

TABLE VI

THE PERFORMANCE COMPARISON TO SOLVE VRPTW ON THE SOLOMON BENCHMARK PROBLEMS. K IS THE AVERAGE NUMBER OF VEHICLES, AND TINF IS THE TIME TO GENERATE A SOLUTION

customized solutions, and scalability. However, based on the presented survey, we indicate a list of future research directions that aim to further reveal the potential of machine learning methods for VRPs.

## A. Generalization

The generalization of learning methods to problems of different graph sizes is one of the primary challenges. For instance, to produce competitive solutions, separate sets of models need to be trained from scratch for each graph size leading to a substantial total training time. Nevertheless, developing transfer learning from small-sized graphs to largesized graphs has not been studied extensively except in a few studies. Reference [99] proposes to learn heatmaps for TSP on small sizes and apply them to arbitrarily large instances. Also, [27] investigates the generalization capabilities of SL and RL methods in end-to-end settings on TSP. The paper has shown that RL models have better generalization capabilities as compared to SL models. The study demonstrates that models trained on various graph sizes generalize better than the models trained on solo-sized graphs.nNotably, the recent work by [124] on SL for route optimization with robustness guarantees is a promising step forward in this area. However, there is still much work to be done to improve the efficiency of generalized solutions. At the same time, the hybrid models have demonstrated the potential to handle large-scale routing

TABLE VII

## THE PERFORMANCE COMPARISON ON CVRPLIB

problems. For instance, [100] presents a model to solve TSP with 1,000 nodes. Reference [92] speeds up the current solvers to solve VRPs with 500 and up to 3,000 nodes. Reference [91]'s experiments indicate the competitiveness of their hybrid method with the construction heuristics for VRPTW with 400 nodes.

Presenting the performance of the proposed models on real-world datasets has become more common in recent years [8], [42], [89], [95], [118]. A particularly noteworthy advancements in this field are the recent studies conducted by [125], [126], and [127], all of which have exhibited promising outcomes. Despite these advancements, a significant amount of work is still required to enhance the efficiency of generalized solutions when applied to real-world datasets. Recently there have been some attempts [128] to establish well-accepted benchmark libraries and guidelines to compare the computational results of machine learning models with the operations research methods. However, the fair comparison of the learning-based models among themselves still remains a challenge.

## B. Improving Models and Solution Methods

Complex models equipped with advanced deep neural techniques and numerous parameters to train on expensive hardware have been proposed. However, the solutions produced by learning methods in the best cases are comparable to existing non-learning methods as reported in Tables V and VI. Learning methods require the development of simple yet powerful models that can compete with traditional methods. Currently, there are strong assumptions about the inputs to the models. For instance, both end-to-end and hybrid models work with complete graphs with known coordinates of the nodes. However, practically important VRPs focus on the characteristics of the road networks such as costs of edges [129] without complete information about a graph. This indicates a strong need to develop models that can generalize to edge features only. In addition to inputs, the central question of why the presented models work remains largely unanswered. The studies present the superiority of their proposed models based on the computational studies conducted mostly on random datasets. However, the discussions over their choice of deep learning models that fit the properties of VRPs are limited. Another important avenue to explore involves addressing the challenges associated with solving VRPs that come with complex constraints. Currently, many existing end-to-end learning methods rely on a technique known as 'masking' to represent feasibility constraints. However, this approach falls short of ensuring full satisfaction of constraints. Therefore, it is crucial to look into alternative approaches that can adequately handle VRPs with complex constraints.

## C. Incorporating Uncertainty and Online Routing

The power of learning models lies in their ability to generalize over data distribution. This advantage can be well exploited to incorporate dynamic elements in routing problems, which is not easy to do with the traditional methods [75]. For instance, online routing has become more critical with the increased demand for on-demand services such as parcel delivery, ridesharing. The development of green logistic systems, including electrical, autonomous vehicles, and drones with uncertain charging times, can also be incorporated into the development of learning-based routing problems. The majority of the surveyed studies focus on routing either a single vehicle or are designed to solve the problems with additive objectives such as distance. The fundamental problem of routing multiple vehicles that share a common action space requires developing novel or proper adjustments to existing algorithms, including the development of clustering methods.

## VII. CONCLUSION

This paper presents a survey of machine learning-based studies that can generalize to the inputs coming from the same distribution and produce quality solutions instantaneously. This potential improvement can lead to enhanced efficiency of solutions for static and dynamic VRPs in applications involving both offline and online decision-making. We present a broad taxonomy that includes both end-to-end learning and hybrid models, which use machine learning methods either to improve existing solutions or deploy them to produce initial solutions which are later improved by non-learning methods. We discuss in detail SL and RL methods for VRPs. We also present the hybrid methods for selecting heuristics, performing neighborhood searches, and estimating value and action-value functions to solve offline and online VRPs with single and multiple vehicles. In our computational studies, we compare the performances of the most recent models on random and well-known datasets.

Although there has been great progress in the machine learning literature to solve VRPs, there are many unanswered research questions, including the challenges of routing multiple vehicles in a dynamic environment and generalization of the trained models across different graph sizes. Most importantly, the development of novel models is needed that focus on the specifics of VRP solutions such as symmetry. Additionally, future research directions may include the integration of realtime data, the development of multi-objective optimization models, and the exploration of hybrid approaches combining machine learning with other optimization techniques. These advancements will be critical in enabling machine learning models to provide optimal routing solutions that consider real-world factors and can be easily implemented by logistics companies.

## APPENDIX BACKGROUND

This section discusses two learning paradigms applied to solve VRPs, namely SL and RL. Unlike the former, RL does not rely on given labels but instead focuses on learning through interactions in order to maximize a long-term reward. A goaloriented SL agent collects experiences by interacting with environment , where the interaction dynamics describe a task at hand. The agent aims to learn decisions that will lead to the most significant total reward through trial and error. We introduce Markov Decision Process and SL algorithms that solve sequential decision-making problems.

Fig. 1. Anumber of studies focused on solving VRPs using learning methods.

![Image](image_000000_bb5b5121ab6ae026b6002c8015c4dab2b31dbedf6571954368f80705a45a0d3b.png)

## A. SL

In SL, we are given a vector of inputs, X T = ( X 1 , X 2 , . . . , Xp ) consisting of p number of features and a vector of labels, Y . Subsequently, we can assemble a training set D = ( xi , yi ), i = 1 , . . . , N and xi ∈ R p . A SL algorithm takes input xi and produces the predicted values of output denoted as ˆ f ( xi ) . The algorithm is trained by adjusting its predictions to reduce the difference between the actual output values, yi , and its predicted values, ˆ f ( xi ) . Depending on the problem, we can define different loss functions to turn training parameters θ efficiently. After training, the learned function ˆ f ( x ) is used to predict the values of output with new values of x .

## B. Markov Decision Process (MDP)

Formally, SL can be described in detail with MDP. Finite MDP consists of a tuple M = ⟨ S A R , , , P ⟩ representing a finite set of states, actions, rewards, and a transition probability function, respectively. At time t , given the current state of environment St , an agent selects action At that yields a reward in the next step denoted by Rt + 1 and a new state St + 1. The state-transition probability function given in 14 defines the dynamics of a problem, where the next state of the environment is only determined by the current state and action showcasing the Markov property :

$$p ( s ^ { \prime } | s, a ) = P ( S _ { t + 1 } = s ^ { \prime } | S _ { t } = s, A _ { t } = a ). \quad ( 1 4 ) \ \text{provid}$$

Using the above state transition dynamics, we can determine the expected reward for the taken action a at state s with the consecutive state s ′ :

$$r ( s, a, s ^ { \prime } ) & = \mathbb { E } [ R _ { t + 1 } | S _ { t } = s, A _ { t } = a, S _ { t + 1 } = s ^ { \prime } ] \quad \text{$two$ such} \\ & = \frac { \sum _ { r \in R } r p ( s ^ { \prime }, r | a, s ) } { p ( s ^ { \prime } | s, a ) } \quad \text{$(15)} \quad \text{with $\alpha$} \\ & \quad Q ( S _ { t }, \, \iota )$$

In MDPs, a policy denoted as π( a s | ) maps from states to probabilities of selected actions as shown below :

$$\pi ( a | s ) = p ( A _ { t + 1 } = a | S _ { t } = s ). \quad \quad ( 1 6 ) \ \ a \ \text{tai}$$

The value function, v( ) s , relates states to long-term rewards. A value function under policy π is formally denoted as vπ ( s ) , which represents the expected total reward starting from state s and following policy π over a planning horizon of T and a discount factor of γ :

$$v _ { \pi } ( s ) = \mathbb { E } [ \sum _ { k = 0 } ^ { T - 1 } \gamma ^ { k } R _ { t + k + 1 } | S _ { t } = s ] \quad \quad ( 1 7 )$$

Similarly, action-value function q π ( s , a ) defines the expected reward when taking action a at state s under policy π :

$$q _ { \pi } ( s, a ) = \mathbb { E } [ \sum _ { k = 0 } ^ { T - 1 } \gamma ^ { k } R _ { t + k + 1 } | S _ { t } = s, \, A _ { t } = a. ] \quad ( 1 8 )$$

We are often interested in finding a policy that leads to the most significant expected total rewards. In other words, we solve for an optimal policy π ∗ that yields the optimal state-value function v ( ∗ s ) :

$$\i d a _ { \substack { v ^ { * } ( s ) = \max _ { \pi } v _ { \pi } ( s ) \\ \text{ing} } } = \sum _ { a } ^ { * } v _ { \pi } ( s ) \\ \text{th} } = \sum _ { a } \mathbb { E } [ R _ { t + 1 } + \gamma v ^ { * } ( S _ { t + 1 } ) | S _ { t } = s, A _ { t } = a ] \quad ( 1 9 )$$

or the optimal action-value function q ∗ ( s , a ) :

$$\ g \ i t s _ { \text{tput} } \quad q ^ { * } ( s, a ) & = \max _ { \pi } q _ { \pi } ( s, a ) \\ \intertext { n } \quad & = \max _ { a ^ { \prime } } \mathbb { E } [ R _ { t + 1 } + \gamma q ^ { * } ( S _ { t + 1 }, a ^ { \prime } ) | S _ { t } = s, A _ { t } = a ] \\ \text{ction} \\. \dots$$

Equations (19) -(20) are known as the Bellman optimality equations that are recursive.

## C. SL Algorithms

We can broadly define SL as any method that solves the above-defined MDPs. When Equations (15) and (16) are known, one can directly compute the future reward and apply planning methods. The methods that require complete knowledge about the dynamics of the problems are called model-based , and Dynamic Programming (DP) is one of such methods. Often Equations (15) and (16) are unknown, and one needs to learn the dynamics of the problem by learning from experience. The methods that focus directly on finding optimal state-value functions rather than requiring the complete knowledge of transition probabilities are called model-free . Figure 3 provides an overview of SL. Further, model-free methods can be categorized into value-based, actor-critic, and policy-based, depending on which function they aim to optimize.

Typically, in value-based methods, the goal is to find the action-value function, q ∗ . One of the widely used algorithms for such purpose is Q-learning that recursively updates the values of action and state pairs independently from a policy with α as a constant step size :

$$\stackrel { \dots } { \stackrel { Q } { \stackrel { Q } { ( S _ { t }, A _ { t } ) } } } \\ \text{states to } \quad \leftarrow Q ( S _ { t }, A _ { t } ) + \alpha [ R _ { t + 1 } + \gamma \max _ { a } Q ( S _ { t + 1 }, a ) - Q ( S _ { t }, A _ { t } ) ] \ ( 2 1 )$$

$$\ e s \ \tt o \quad \leftarrow Q ( S _ { t }, \, A _ { t } ) + \alpha [ R _ { t + 1 } + \gamma \max _ { a } Q ( S _ { t + 1 }, a ) - Q ( S _ { t }, \, A _ { t } ) ] \ ( 2 1 )$$

In the simplest form shown above, Q-learning takes as a target one step Temporal-Difference (TD), Rt + 1 + γ max a Q St ( + 1 , a ) . The algorithm uses policy π to select actions and states whose values will be updated; however, the updates of Q-values are done through maximization over actions. To balance exploration and exploitation, Q-learning

Fig. 2. The taxonomy of learning methods for VRPs.

![Image](image_000001_6ba0bdb0691fdc019a7d6a15b355bbc16052e595208c5a1d2c82cc8b02636b99.png)

Fig. 3. An overview of SL methods.

![Image](image_000002_4061cdee154794b6c02bb7c5b473f6f1f2873037c971c68067c9c46c52b7c84a.png)

THE PERFORMANCE COMPARISON TO SOLVE TSP ON RANDOM INSTANCES USED IN [31]. WE USED THE PRE-TRAINED MODELS FROM THE RESPECTIVE GITHUB REPOSITORIES

deploys ϵ -greedy policy that selects random actions with probability ϵ and follows the greedy policy concerning the current estimates of Q with probability 1 -ϵ .

Methods that directly focus on finding optimal policies are called policy-based methods. For instance, in policy-gradient methods, we directly aim to learn a parametrized policy πθ

## TABLE IX

## THE PERFORMANCE COMPARISON ON TSPLIB

with a set of parameters θ defined as follows :

$$\pi _ { \theta } ( a | s ) = P r ( A _ { t } = a | S _ { t } = s, \theta = \theta _ { t } ) \quad \ \ ( 2 2 ) \quad \ \cdots$$

To find the optimal values of parameters θ , an objective function J (θ) needs to be defined, which is then maximized. Consequently, we need to solve the optimization problem with the gradient ascent to update the values of θ with step size α :

$$\theta \leftarrow \theta + \alpha \nabla _ { \theta } J ( \theta ) \text{ \quad \ \ } ( 2 3 ) \text{ \quad } \text{ }$$

For instance, the objective function can be set to maximize the total reward at the end of the episode, RT then in accordance to the policy gradient theorem, we obtain :

$$\nabla _ { \theta } J ( \theta ) = \mathbb { E } [ \nabla _ { \theta } \log \pi _ { \theta } ( a | s ) R _ { T } ] \, \text{ \quad \ \ } ( 2 4 )$$

In fact, the REINFORCE algorithm widely adopted to solve VRPs, uses Monte-Carlo gradient methods to learn optimal policies. However, using the total reward of an episode results in high variance, which can be mitigated by the advantage function defined as the difference between the total reward and baseline. Accordingly, we can write the REINFORCE algorithm with baseline as follows :

$$\nabla _ { \theta } J ( \theta ) = \mathbb { E } [ \nabla _ { \theta } \log \pi _ { \theta } ( a | s ) ( R _ { T } - b ) ] \quad \ ( 2 5 ) \quad \ 1$$

In practice, actor-critic methods are often used, where an actor produces policy, and a critic is responsible for the policy evaluation. In contrast to Monte-Carlo gradient methods, TD is used to update values in actor-critic policy gradients.

## REFERENCES

- [1] M. A. Levans, '30th annual state of logistics report: What's next?' Logistics Manage. Highlands Ranch , vol. 58, no. 7, pp. 9-10, 2019.
- [2] Y. Yuan, D. Cattaruzza, M. Ogier, and F. Semet, 'Last mile delivery problem: The one-vehicle case,' in Proc. 7th Workshop Freight Transp. Logistics (Odysseus) , Cagliari, Italy, Jun. 2018. [Online]. Available: https://hal.science/hal-01951934
- [3] A. E. Rizzoli, R. Montemanni, E. Lucibello, and L. M. Gambardella, 'Ant colony optimization for real-world vehicle routing problems,' Swarm Intell. , vol. 1, no. 2, pp. 135-151, 2007.
- [4] J. K. Lenstra and A. H. G. R. Kan, 'Complexity of vehicle routing and scheduling problems,' Networks , vol. 11, no. 2, pp. 221-227, Jun. 1981.
- [5] G. Laporte, 'The vehicle routing problem: An overview of exact and approximate algorithms,' Eur. J. Oper. Res. , vol. 59, no. 3, pp. 345-358, Jun. 1992.
- [6] K. F. Doerner and V. Schmid, 'Survey: Matheuristics for rich vehicle routing problems,' in Proc. Int. Workshop Hybrid metaheuristics . Cham, Switzerland: Springer, 2010, pp. 206-221.
- [7] M. Asghari and S. M. J. M. Al-E-Hashem, 'Green vehicle routing problem: A state-of-the-art review,' Int. J. Prod. Econ. , vol. 231, Jan. 2021, Art. no. 107899.
- [8] A. Bogyrbayeva, S. Jang, A. Shah, Y. J. Jang, and C. Kwon, 'A reinforcement learning approach for rebalancing electric vehicle sharing systems,' IEEE Trans. Intell. Transp. Syst. , vol. 23, no. 7, pp. 8704-8714, Jul. 2022.
- [9] M. Nazari, A. Oroojlooy, L. Snyder, and M. Takác, 'Reinforcement learning for solving the vehicle routing problem,' in Proc. Adv. Neural Inf. Process. Syst. , vol. 31, 2018, pp. 1-11.
- [10] Y. Bengio, A. Lodi, and A. Prouvost, 'Machine learning for combinatorial optimization: A methodological tour d'horizon,' Eur. J. Oper. Res. , vol. 290, no. 2, pp. 405-421, Apr. 2021.
- [11] N. Mazyavkina, S. Sviridov, S. Ivanov, and E. Burnaev, 'Reinforcement learning for combinatorial optimization: A survey,' Comput. Oper. Res. , vol. 134, Oct. 2021, Art. no. 105400.
- [12] Y. Yan, A. H. F. Chow, C. P. Ho, Y.-H. Kuo, Q. Wu, and C. Ying, 'Reinforcement learning for logistics and supply chain management: Methodologies, state of the art, and future opportunities,' Transp. Res. E, Logistics Transp. Rev. , vol. 162, Jun. 2022, Art. no. 102712.
- [13] N. Vesselinova, R. Steinert, D. F. Perez-Ramirez, and M. Boman, 'Learning combinatorial optimization on graphs: A survey with applications to networking,' IEEE Access , vol. 8, pp. 120388-120416, 2020.
- [14] C. Zhang et al., 'A review on learning to solve combinatorial optimisation problems in manufacturing,' IET Collaborative Intell. Manuf. , vol. 5, no. 1, Mar. 2023, Art. no. e12072.
- [15] Q. Cappart, D. Chételat, E. Khalil, A. Lodi, C. Morris, and P. Veliˇ ckovi´, c 'Combinatorial optimization and reasoning with graph neural networks,' 2021, arXiv:2102.09544 .
- [16] U. Junior Mele, L. Maria Gambardella, and R. Montemanni, 'Machine learning approaches for the traveling salesman problem: A survey,' in Proc. 8th Int. Conf. Ind. Eng. Appl. (Europe) , Jan. 2021, pp. 182-186.
- [17] R. Bai et al., 'Analytics and machine learning in vehicle routing research,' Int. J. Prod. Res. , vol. 61, no. 1, pp. 1-27, Jan. 2023.
- [18] B. Li, G. Wu, Y. He, M. Fan, and W. Pedrycz, 'An overview and experimental study of learning-based optimization algorithms for the vehicle routing problem,' IEEE/CAA J. Autom. Sinica , vol. 9, no. 7, pp. 1115-1138, Jul. 2022.
- [19] M. Held and R. M. Karp, 'A dynamic programming approach to sequencing problems,' J. Soc. Ind. Appl. Math. , vol. 10, no. 1, pp. 196-210, Mar. 1962.
- [20] M. Hahsler and K. Hornik, 'TSPinfrastructure for the traveling salesperson problem,' J. Stat. Softw. , vol. 23, no. 2, pp. 1-27, 2007.
- [21] K. Helsgaun, 'An extension of the Lin-Kernighan-Helsgaun TSP solver for constrained traveling salesman and vehicle routing problems,' Roskilde Universitet, Tech. Rep., Dec. 2017.
- [22] M. Prates, P. H. Avelar, H. Lemos, L. C. Lamb, and M. Y. Vardi, 'Learning to solve Np-complete problems: A graph neural network for decision TSP,' in Proc. AAAI Conf. Artif. Intell. , vol. 33, 2019, pp. 4731-4738.
- [23] C. K. Joshi, T. Laurent, and X. Bresson, 'An efficient graph convolutional network technique for the travelling salesman problem,' 2019, arXiv:1906.01227 .
- [24] M. van Knippenberg, M. Holenderski, and V. Menkovski, 'Complex vehicle routing with memory augmented neural networks,' in Proc. IEEE Conf. Ind. Cyberphysical Syst. (ICPS) , vol. 1, Jun. 2020, pp. 303-308.
- [25] A. Milan, S. H. Rezatofighi, R. Garg, A. Dick, and I. Reid, 'Datadriven approximations to np-hard problems,' in Proc. 31st AAAI Conf. Artif. Intell. , 2017, pp. 1-7.
- [26] C. K. Joshi, T. Laurent, and X. Bresson, 'On learning paradigms for the travelling salesman problem,' 2019, arXiv:1910.07210 .
- [27] C. K. Joshi, Q. Cappart, L.-M. Rousseau, and T. Laurent, 'Learning the travelling salesman problem requires rethinking generalization,' Constraints , vol. 27, no. 1, pp. 70-98, Apr. 2022, doi: 10.1007/s10601022-09327-y.
- [28] H. Dai, E. B. Khalil, Y. Zhang, B. Dilkina, and L. Song, 'Learning combinatorial optimization algorithms over graphs,' Proc. Adv. Neural Inf. Process. Syst. , vol. 30, 2017, pp. 1-7.
- [29] V. Mnih et al., 'Human-level control through deep reinforcement learning,' Nature , vol. 518, no. 7540, pp. 529-533, 2015.
- [30] M. Riedmiller, 'Neural fitted Q iteration-first experiences with a data efficient neural reinforcement learning method,' in Proc. Eur. Conf. Mach. Learn. Cham, Switzerland: Springer, 2005, pp. 317-328.
- [31] W. Kool, H. van Hoof, and M. Welling, 'Attention, learn to solve routing problems!' 2019, arXiv:1803.08475 .
- [32] I. Bello, H. Pham, Q. V. Le, M. Norouzi, and S. Bengio, 'Neural combinatorial optimization with reinforcement learning,' 2016, arXiv:1611.09940 .
- [33] B. Peng, J. Wang, and Z. Zhang, 'A deep reinforcement learning algorithm using dynamic attention model for vehicle routing problems,' in Proc. Int. Symp. Intell. Comput. Appl. Cham, Switzerland: Springer, 2019, pp. 636-650.
- [34] L. Xin, W. Song, Z. Cao, and J. Zhang, 'Step-wise deep learning models for solving routing problems,' IEEE Trans. Ind. Informat. , vol. 17, no. 7, pp. 4861-4871, Jul. 2021.
- [35] P. R. de O. da Costa, J. Rhuggenaath, Y. Zhang, and A. Akcay, 'Learning 2-opt heuristics for the traveling salesman problem via deep reinforcement learning,' in Proc. Asian Conf. Mach. Learn. , 2020, pp. 465-480.
- [36] A. Vaswani et al., 'Attention is all you need,' in Proc. Adv. Neural Inf. Process. Syst. , 2017, pp. 5998-6008.
- [37] Y. Jiang, Y. Wu, Z. Cao, and J. Zhang, 'Learning to solve routing problems via distributionally robust optimization,' Proc. AAAI Conf. Artif. Intell. , Jun. 2022, vol. 36, no. 9, pp. 9786-9794.

- [38] Y. Senuma, Z. Wang, Y. Nakano, and J. Ohya, 'GEAR: A graph edge attention routing algorithm solving combinatorial optimization problem with graph edge cost,' in Proc. 10th ACM SIGSPATIAL Int. Workshop Analytics Big Geospatial Data , 2022, pp. 8-16.
- [39] K. Li, T. Zhang, R. Wang, Y. Wang, Y. Han, and L. Wang, 'Deep reinforcement learning for combinatorial optimization: Covering salesman problems,' IEEE Trans. Cybern. , vol. 52, no. 12, pp. 13142-13155, Dec. 2022.
- [40] K. Zhang, F. He, Z. Zhang, X. Lin, and M. Li, 'Multi-vehicle routing problems with soft time windows: A multi-agent reinforcement learning approach,' Transp. Res. C, Emerg. Technol. , vol. 121, Dec. 2020, Art. no. 102861.
- [41] L. Xin, W. Song, Z. Cao, and J. Zhang, 'Multi-decoder attention model with embedding glimpse for solving vehicle routing problems,' in Proc. 35th AAAI Conf. Artif. Intell. , 2021, pp. 12042-12049.
- [42] M. Kim et al., 'Learning collaborative policies to solve NP-hard routing problems,' in Proc. Adv. Neural Inf. Process. Syst. , vol. 34, 2021, pp. 10418-10430.
- [43] Y.-D. Kwon, J. Choo, B. Kim, I. Yoon, Y. Gwon, and S. Min, 'POMO: Policy optimization with multiple optima for reinforcement learning,' in Proc. Adv. Neural Inf. Process. Syst. (NeurIPS) , vol. 33, 2020, pp. 21188-21198.
- [44] Y. Liang Goh, W. Sun Lee, X. Bresson, T. Laurent, and N. Lim, 'Combining reinforcement learning and optimal transport for the traveling salesman problem,' 2022, arXiv:2203.00903 .
- [45] P. Emami and S. Ranka, 'Learning permutations with sinkhorn policy gradient,' 2018, arXiv:1805.07010 .
- [46] J. J. Q. Yu, W. Yu, and J. Gu, 'Online vehicle routing with neural combinatorial optimization and deep reinforcement learning,' IEEE Trans. Intell. Transp. Syst. , vol. 20, no. 10, pp. 3806-3817, Oct. 2019.
- [47] B. Lin, B. Ghaddar, and J. Nathwani, 'Deep reinforcement learning for the electric vehicle routing problem with time windows,' IEEE Trans. Intell. Transp. Syst. , vol. 23, no. 8, pp. 11528-11538, Aug. 2022.
- [48] W. Pan and S. Q. Liu, 'Deep reinforcement learning for the dynamic and uncertain vehicle routing problem,' Appl. Intell. , vol. 53, no. 1, pp. 405-422, 2023.
- [49] D. Bahdanau, K. Cho, and Y. Bengio, 'Neural machine translation by jointly learning to align and translate,' in Proc. 3rd Int. Conf. Learn. Represent. (ICLR), Conf. Track , Y. Bengio and Y. LeCun, Eds. San Diego, CA, USA, May 2015. [Online]. Available: http://arxiv.org/abs/1409.0473
- [50] J. M. Vera and A. G. Abad, 'Deep reinforcement learning for routing a heterogeneous fleet of vehicles,' in Proc. IEEE Latin Amer. Conf. Comput. Intell. (LA-CCI) , 2019, pp. 1-6.
- [51] J. K. Falkner and L. Schmidt-Thieme, 'Learning to solve vehicle routing problems with time windows through joint attention,' 2020, arXiv:2006.09100 .
- [52] K. Lei, P. Guo, Y. Wang, X. Wu, and W. Zhao, 'Solve routing problems with a residual edge-graph attention neural network,' Neurocomputing , vol. 508, pp. 79-98, Oct. 2022.
- [53] U. Gunarathna, R. Borovica-Gajic, S. Karunasekara, and E. Tanin, 'Solving dynamic graph problems with multi-attention deep reinforcement learning,' 2022, arXiv:2201.04895 .
- [54] J. Li et al., 'Learning feature embedding refiner for solving vehicle routing problems,' IEEE Trans. Neural Netw. Learn. Syst. , early access, Jun. 23, 2023, doi: 10.1109/TNNLS.2023.3285077.
- [55] K. Li, T. Zhang, and R. Wang, 'Deep reinforcement learning for multiobjective optimization,' IEEE Trans. Cybern. , vol. 51, no. 6, pp. 3103-3114, Jun. 2021.
- [56] A. Hottung, Y.-D. Kwon, and K. Tierney, 'Efficient active search for combinatorial optimization problems,' 2021, arXiv:2106.05126 .
- [57] Q. Sykora, M. Ren, and R. Urtasun, 'Multi-agent routing value iteration network,' in Proc. Int. Conf. Mach. Learn. , 2020, pp. 9300-9310.
- [58] Y. Xu, M. Fang, L. Chen, G. Xu, Y. Du, and C. Zhang, 'Reinforcement learning with multiple relational attention for solving vehicle routing problems,' IEEE Trans. Cybern. , vol. 52, no. 10, pp. 11107-11120, Oct. 2022.
- [59] I. Drori et al., 'Learning to solve combinatorial optimization problems on real-world graphs in linear time,' in Proc. 19th IEEE Int. Conf. Mach. Learn. Appl. (ICMLA) , Dec. 2020, pp. 19-24.
- [60] J. Li, L. Xin, Z. Cao, A. Lim, W. Song, and J. Zhang, 'Heterogeneous attentions for solving pickup and delivery problem via deep reinforcement learning,' IEEE Trans. Intell. Transp. Syst. , vol. 23, no. 3, pp. 2306-2315, Mar. 2022.
- [61] J. Li et al., 'Deep reinforcement learning for solving the heterogeneous capacitated vehicle routing problem,' IEEE Trans. Cybern. , vol. 52, no. 12, pp. 13572-13585, Dec. 2022.
- [62] L. Duan et al., 'Efficiently solving the practical vehicle routing problem: A novel joint learning approach,' in Proc. 26th ACM SIGKDD Int. Conf. Knowl. Discovery Data Mining , 2020, pp. 3054-3063.
- [63] F. Alesiani, 'BiGrad: Differentiating through bilevel optimization programming,' in Proc. AAAI Workshop Adversarial Mach. Learn. Beyond , 2022, pp. 1-11. [Online]. Available: https://openreview.net/ forum?id=HvRAM-dpmEv
- [64] M. V. Pogancic, A. Paulus, V. Musil, G. Martius, and M. Rolínek, 'Differentiation of blackbox combinatorial solvers,' in Proc. 8th Int. Conf. Learn. Represent. (ICLR) . Addis Ababa, Ethiopia. OpenReview.net, Apr. 2020. [Online]. Available: https://openreview.net/ forum?id=BkevoJSYPB
- [65] L. M. Gambardella and M. Dorigo, 'ANT-Q: A reinforcement learning approach to the traveling salesman problem,' in Proc. Mach. Learn. , 1995, pp. 252-260.
- [66] M. Dorigo and L. M. Gambardella, 'Ant colony system: A cooperative learning approach to the traveling salesman problem,' IEEE Trans. Evol. Comput. , vol. 1, no. 1, pp. 53-66, Apr. 1997.
- [67] F. Liu and G. Zeng, 'Study of genetic algorithm with reinforcement learning to solve the TSP,' Exp. Syst. Appl. , vol. 36, no. 3, pp. 6995-7001, Apr. 2009.
- [68] M. M. Alipour, S. N. Razavi, M. R. Feizi Derakhshi, and M. A. Balafar, 'A hybrid algorithm using a genetic algorithm and multiagent reinforcement learning heuristic to solve the traveling salesman problem,' Neural Comput. Appl. , vol. 30, no. 9, pp. 2935-2951, Nov. 2018.
- [69] R. Qi, J.-Q. Li, J. Wang, H. Jin, and Y.-Y. Han, 'QMOEA: A Qlearning-based multiobjective evolutionary algorithm for solving timedependent green vehicle routing problems with time windows,' Inf. Sci. , vol. 608, pp. 178-201, Aug. 2022.
- [70] Y. Niu, J. Shao, J. Xiao, W. Song, and Z. Cao, 'Multi-objective evolutionary algorithm based on RBF network for solving the stochastic vehicle routing problem,' Inf. Sci. , vol. 609, pp. 387-410, Sep. 2022.
- [71] A. Hottung and K. Tierney, 'Neural large neighborhood search for the capacitated vehicle routing problem,' 2019, arXiv:1911.09539 .
- [72] A. A. Syed, K. Akhnoukh, B. Kaltenhaeuser, and K. Bogenberger, 'Neural network based large neighborhood search algorithm for ride hailing services,' in Proc. EPIA Conf. Artif. Intell. Cham, Switzerland: Springer, 2019, pp. 584-595.
- [73] M. A. L. Silva, S. R. de Souza, M. J. F. Souza, and A. L. C. Bazzan, 'A reinforcement learning-based multi-agent framework applied for solving routing and scheduling problems,' Exp. Syst. Appl. , vol. 131, pp. 148-171, Oct. 2019.
- [74] F. C. Fernandes, S. R. de Souza, M. A. L. Silva, H. E. Borges, and F. F. Ribeiro, 'A multiagent architecture for solving combinatorial optimization problems through metaheuristics,' in Proc. IEEE Int. Conf. Syst., Man Cybern. , Oct. 2009, pp. 3071-3076.
- [75] M. W. Ulmer, J. C. Goodson, D. C. Mattfeld, and M. Hennig, 'Offlineonline approximate dynamic programming for dynamic vehicle routing with stochastic requests,' Transp. Sci. , vol. 53, no. 1, pp. 185-202, Feb. 2019.
- [76] M. W. Ulmer, D. C. Mattfeld, and F. Köster, 'Budgeting time for dynamic vehicle routing with stochastic customer requests,' Transp. Sci. , vol. 52, no. 1, pp. 20-37, Jan. 2018.
- [77] Q. Cappart, T. Moisan, L.-M. Rousseau, I. Prémont-Schwarz, and A. A. Cire, 'Combining reinforcement learning and constraint programming for combinatorial optimization,' in Proc. AAAI Conf. Artif. Intell. , vol. 35, no. 5, 2021, pp. 3677-3687.
- [78] W. Kool, H. van Hoof, J. Gromicho, and M. Welling, 'Deep policy dynamic programming for vehicle routing problems,' 2021, arXiv:2102.11756 .
- [79] S. Xu, S. S. Panwar, M. Kodialam, and T. Lakshman, 'Deep neural network approximated dynamic programming for combinatorial optimization,' in Proc. AAAI Conf. Artif. Intell. , vol. 34, no. 2, 2020, pp. 1684-1691.
- [80] F. Yang, T. Jin, T.-Y. Liu, X. Sun, and J. Zhang, 'Boosting dynamic programming with neural networks for solving Np-hard problems,' in Proc. Asian Conf. Mach. Learn. , 2018, pp. 726-739.
- [81] A. Delarue, R. Anderson, and C. Tjandraatmadja, 'Reinforcement learning with combinatorial actions: An application to vehicle routing,' in Proc. Adv. Neural Inf. Process. Syst. , vol. 33, 2020, pp. 609-620.
- [82] R. Anderson, J. Huchette, W. Ma, C. Tjandraatmadja, and J. P. Vielma, 'Strong mixed-integer programming formulations for trained neural networks,' Math. Program. , vol. 183, nos. 1-2, pp. 3-39, Sep. 2020.
- [83] F. Alesiani, G. Ermis, and K. Gkiotsalitis, 'Constrained clustering for the capacitated vehicle routing problem (CC-CVRP),' Appl. Artif. Intell. , vol. 36, no. 1, Dec. 2022, Art. no. 1995658.

- [84] Y. Wang, L. Ran, X. Guan, J. Fan, Y. Sun, and H. Wang, 'Collaborative multicenter vehicle routing problem with time windows and mixed deliveries and pickups,' Exp. Syst. Appl. , vol. 197, Jul. 2022, Art. no. 116690.
- [85] S.-H. Lu, R. J. Kuo, Y.-T. Ho, and A.-T. Nguyen, 'Improving the efficiency of last-mile delivery with the flexible drones traveling salesman problem,' Exp. Syst. Appl. , vol. 209, Dec. 2022, Art. no. 118351.
- [86] Y. Wang et al., 'Collaborative two-echelon multicenter vehicle routing optimization based on state-space-time network representation,' J. Cleaner Prod. , vol. 258, Jun. 2020, Art. no. 120590.
- [87] Y. Wang, S. Peng, X. Zhou, M. Mahmoudi, and L. Zhen, 'Green logistics location-routing problem with eco-packages,' Transp. Res. E, Logistics Transp. Rev. , vol. 143, Nov. 2020, Art. no. 102118.
- [88] Y. Ma et al., 'Learning to iteratively solve routing problems with dualaspect collaborative transformer,' in Proc. Adv. Neural Inf. Process. Syst. , vol. 34, 2021, pp. 11096-11107.
- [89] Y. Wu, W. Song, Z. Cao, J. Zhang, and A. Lim, 'Learning improvement heuristics for solving routing problems,' IEEE Trans. Neural Netw. Learn. Syst. , vol. 33, no. 9, pp. 5057-5069, Sep. 2022.
- [90] X. Chen and Y. Tian, 'Learning to perform local rewriting for combinatorial optimization,' in Advances in Neural Information Processing Systems 32: Annual Conference on Neural Information Processing Systems 2019, NeurIPS 2019, December 8-14, 2019, Vancouver, BC, Canada , H. M. Wallach, H. Larochelle, A. Beygelzimer, F. d'Alché-Buc, E. B. Fox, and R. Garnett, Eds., 2019, pp. 6278-6289. [Online]. Available: https://proceedings.neurips.cc/ paper/2019/hash/131f383b434fdf48079bff1e44e2d9a5-Abstract.html
- [91] L. Gao, M. Chen, Q. Chen, G. Luo, N. Zhu, and Z. Liu, 'Learn to design the heuristics for vehicle routing problem,' 2020, arXiv:2002.08539 .
- [92] S. Li, Z. Yan, and C. Wu, 'Learning to delegate for large-scale vehicle routing,' in Proc. Adv. Neural Inf. Process. Syst. , vol. 34, 2021, pp. 26198-26211.
- [93] M. Deudon, P. Cournut, A. Lacoste, Y. Adulyasak, and L.-M. Rousseau, 'Learning heuristics for the TSP by policy gradient,' in Proc. Int. Conf. Integr. Constraint Program., Artif. Intell., Oper. Res. Cham, Switzerland: Springer, 2018, pp. 170-181.
- [94] J. Zhao, M. Mao, X. Zhao, and J. Zou, 'A hybrid of deep reinforcement learning and local search for the vehicle routing problems,' IEEE Trans. Intell. Transp. Syst. , vol. 22, no. 11, pp. 7208-7218, Nov. 2021.
- [95] Q. Ma, S. Ge, D. He, D. Thaker, and I. Drori, 'Combinatorial optimization by graph pointer networks and hierarchical reinforcement learning,' 2019, arXiv:1911.04936 .
- [96] R. Zhang, A. Prokhorchuk, and J. Dauwels, 'Deep reinforcement learning for traveling salesman problem with time windows and rejections,' in Proc. Int. Joint Conf. Neural Netw. (IJCNN) , Jul. 2020, pp. 1-8.
- [97] Y. Ma et al., 'Efficient neural neighborhood search for pickup and delivery problems,' 2022, arXiv:2204.11399 .
- [98] K. Lin, R. Zhao, Z. Xu, and J. Zhou, 'Efficient large-scale fleet management via multi-agent deep reinforcement learning,' in Proc. 24th ACM SIGKDD Int. Conf. Knowl. Discovery Data Mining , 2018, pp. 1774-1783.
- [99] Z.-H. Fu, K.-B. Qiu, and H. Zha, 'Generalize a small pre-trained model to arbitrarily large TSP instances,' in Proc. AAAI Conf. Artif. Intell. , 2021, vol. 35, no. 8, pp. 7474-7482.
- [100] Z. Xing and S. Tu, 'A graph neural network assisted Monte Carlo tree search approach to traveling salesman problem,' IEEE Access , vol. 8, pp. 108418-108428, 2020.
- [101] J. Ragan-Kelley, C. Barnes, A. Adams, S. Paris, F. Durand, and S. Amarasinghe, 'Halide: A language and compiler for optimizing parallelism, locality, and recomputation in image processing pipelines,' ACM SIGPLAN Notices , vol. 48, no. 6, pp. 519-530, 2013.
- [102] X. Chen, M. W. Ulmer, and B. W. Thomas, 'Deep Q-learning for sameday delivery with vehicles and drones,' Eur. J. Oper. Res. , vol. 298, no. 3, pp. 939-952, May 2022.
- [103] R. Tyasnurita, E. Özcan, and R. John, 'Learning heuristic selection using a time delay neural network for open vehicle routing,' Proc. IEEE Congr. Evol. Comput. (CEC) , Oct. 2017, pp. 1474-1481.
- [104] A. Hottung, B. Bhandari, and K. Tierney, 'Learning a latent search space for routing problems using variational autoencoders,' in Proc. 9th Int. Conf. Learn. Represent. (ICLR) . Virtual Event, Austria: OpenReview.net, May 2021. [Online]. Available: https: //openreview.net/forum?id=90JprVrJBO
- [106] K. C. Tan, Y. H. Chew, and L. H. Lee, 'A hybrid multiobjective evolutionary algorithm for solving vehicle routing problem with time windows,' Comput. Optim. Appl. , vol. 34, no. 1, pp. 115-151, May 2006.
- [107] B. Ombuki, B. J. Ross, and F. Hanshar, 'Multi-objective genetic algorithms for vehicle routing problem with time windows,' Appl. Intell. , vol. 24, no. 1, pp. 17-30, 2006.
- [108] Y. Qi, Z. Hou, H. Li, J. Huang, and X. Li, 'A decomposition based memetic algorithm for multi-objective vehicle routing problem with time windows,' Comput. Oper. Res. , vol. 62, pp. 61-77, 2015.
- [109] D. Q. Wu, M. Dong, and H. Y. Li, 'Vehicle routing problem with time windows using multi-objective co-evolutionary approach,' Int. J. Simul. Model. , vol. 15, no. 4, pp. 742-753, Dec. 2016.
- [110] Z.-H. Fu, K.-B. Qiu, M. Qiu, and H. Zha, 'Targeted sampling of enlarged neighborhood via Monte Carlo tree search for TSP,' in Proc. ICLR , 2019, pp. 1-11.
- [111] L. Xin, W. Song, Z. Cao, and J. Zhang, 'NeuroLKH: Combining deep learning model with lin-kernighan-helsgaun heuristic for solving the traveling salesman problem,' in Proc. Adv. Neural Inf. Process. Syst. , vol. 34, 2021.
- [112] W. Joe and H. C. Lau, 'Deep reinforcement learning approach to solve dynamic vehicle routing problem with stochastic customers,' in Proc. Int. Conf. Automated Planning Scheduling , vol. 30, 2020, pp. 394-402.
- [113] Z. Zong, H. Wang, J. Wang, M. Zheng, and Y. Li, 'RBG: Hierarchically solving large-scale routing problems in logistic systems via reinforcement learning,' in Proc. 28th ACM SIGKDD Conf. Knowl. Discovery Data Mining , 2022, pp. 4648-4658.
- [114] J. Zheng, K. He, J. Zhou, Y. Jin, and C.-M. Li, 'Reinforced linkernighan-helsgaun algorithms for the traveling salesman problems,' Knowl.-Based Syst. , vol. 260, Jan. 2023, Art. no. 110144.
- [115] Z. Sun, M. Greiff, A. Robertsson, and R. Johansson, 'Feasible coordination of multiple homogeneous or heterogeneous mobile vehicles with various constraints,' in Proc. Int. Conf. Robot. Autom. (ICRA) , May 2019, pp. 1008-1013.
- [116] H. Van Hasselt, A. Guez, and D. Silver, 'Deep reinforcement learning with double Q-learning,' in Proc. AAAI Conf. Artif. Intell. , vol. 30, no. 1, 2016.
- [117] W. Qin, Z. Zhuang, Z. Huang, and H. Huang, 'A novel reinforcement learning-based hyper-heuristic for heterogeneous vehicle routing problem,' Comput. Ind. Eng. , vol. 156, Jun. 2021, Art. no. 107252.
- [118] A. Bogyrbayeva, T. Yoon, H. Ko, S. Lim, H. Yun, and C. Kwon, 'A deep reinforcement learning approach for solving the traveling salesman problem with drone,' Transp. Res. C, Emerg. Technol. , vol. 148, p. 103981, 2023.
- [119] G. Bono, J. S. Dibangoye, O. Simonin, L. Matignon, and F. Pereyron, 'Solving multi-agent routing problems using deep attention mechanisms,' IEEE Trans. Intell. Transp. Syst. , vol. 22, no. 12, pp. 7804-7813, Dec. 2021.
- [120] J. Banfi, A. Messing, C. Kroninger, E. Stump, S. Hutchinson, and N. Roy, 'Hierarchical planning for heterogeneous multi-robot routing problems via learned subteam performance,' IEEE Robot. Autom. Lett. , vol. 7, no. 2, pp. 4464-4471, Apr. 2022.
- [121] P. da Costa, J. Rhuggenaath, Y. Zhang, A. Akcay, and U. Kaymak, 'Learning 2-Opt heuristics for routing problems via deep reinforcement learning,' Social Netw. Comput. Sci. , vol. 2, no. 5, pp. 1-6, Sep. 2021.
- [122] M. M. Solomon, 'Algorithms for the vehicle routing and scheduling problems with time window constraints,' Oper. Res. , vol. 35, no. 2, pp. 254-265, Apr. 1987.
- [123] L. Perron and V. Furnon. (2013). OR-Tools. Google. [Online]. Available: https://developers.google.com/optimization/
- [124] T. Jacobs, F. Alesiani, and G. Ermis, 'Reinforcement learning for route optimization with robustness guarantees,' in Proc. IJCAI , 2021, pp. 2592-2598.
- [125] Y. Jiang, Z. Cao, Y. Wu, and J. Zhang, 'Multi-view graph contrastive learning for solving vehicle routing problems,' in Proc. Uncertainty Artif. Intell. , 2023, pp. 984-994.
- [126] J. Bi et al., 'Learning generalizable models for vehicle routing problems via knowledge distillation,' in Proc. Adv. Neural Inf. Process. Syst. , vol. 35, 2022, pp. 31226-31238.
- [127] J. Zhou, Y. Wu, W. Song, Z. Cao, and J. Zhang, 'Towards omnigeneralizable neural methods for vehicle routing problems,' 2023, arXiv:2305.19587 .
- [105] A. E. Gutierrez-Rodríguez, S. E. Conant-Pablos, J. C. Ortiz-Bayliss, and H. Terashima-Marín, 'Selecting meta-heuristics for solving vehicle routing problems with time windows via meta-learning,' Exp. Syst. Appl. , vol. 118, pp. 470-481, Mar. 2019.
- [128] L. Accorsi, A. Lodi, and D. Vigo, 'Guidelines for the computational testing of machine learning approaches to vehicle routing problems,' Oper. Res. Lett. , vol. 50, no. 2, pp. 229-234, Mar. 2022.
- [129] Á. Corberán, R. Eglese, G. Hasle, I. Plana, and J. M. Sanchis, 'Arc routing problems: A review of the past, present, and future,' Networks , vol. 77, no. 1, pp. 88-115, Jan. 2021.

Authorized licensed use limited to: Technische Universitaet Muenchen. Downloaded on January 12,2025 at 17:15:44 UTC from IEEE Xplore.  Restrictions apply.