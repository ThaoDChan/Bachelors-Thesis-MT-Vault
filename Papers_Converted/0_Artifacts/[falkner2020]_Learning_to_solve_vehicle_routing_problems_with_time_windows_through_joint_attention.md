## Learning to Solve Vehicle Routing Problems with Time Windows through Joint Attention

## Jonas K. Falkner

## Lars Schmidt-Thieme

Department of Computer Science University of Hildesheim 31141 Hildesheim, Germany falkner@ismll.uni-hildesheim.de

Department of Computer Science

University of Hildesheim 31141 Hildesheim, Germany

schmidt-thieme@ismll.uni-hildesheim.de

## Abstract

Many real-world vehicle routing problems involve rich sets of constraints with respect to the capacities of the vehicles, time windows for customers etc. While in recent years first machine learning models have been developed to solve basic vehicle routing problems faster than optimization heuristics, complex constraints rarely are taken into consideration. Due to their general procedure to construct solutions sequentially route by route, these methods generalize unfavorably to such problems. In this paper, we develop a policy model that is able to start and extend multiple routes concurrently by using attention on the joint action space of several tours. In that way the model is able to select routes and customers and thus learns to make difficult trade-offs between routes. In comprehensive experiments on three variants of the vehicle routing problem with time windows we show that our model called JAMPR works well for different problem sizes and outperforms the existing state-of-the-art constructive model. For two of the three variants it also creates significantly better solutions than a comparable meta-heuristic solver.

## 1 Introduction

The standard CVRP is a NP-hard combinatorial optimization problem which consists of finding the optimal set of tours for a fleet of capacitated vehicles which have to serve the varying demand of multiple customers. However, for many practical scenarios the problem model of the CVRP is oversimplified and leads to highly sub-optimal solutions since it only takes distances into account. Therefore an important extension introduces an additional time dimension with respective time windows. The CVRP-TW consequently has a high relevance for many practical applications and has been a subject of study for several decades in the Operations Research community [26, 4, 27].

Since the seminal work of Vinyals et al. [30] showing that deep learning methods are a valid approach to solve combinatorial optimization problems, the field has evolved in recent years. Apart from general graph related problems like maximum cut [15] or link prediction and graph partitioning [31], the Traveling Salesman Problem (TSP) was most extensively covered [1, 15, 6, 7, 33]. Meanwhile however, there exist also several approaches to solve VRPs [20, 17, 5, 18].

In the same way as traditional heuristics for routing problems can be categorized as construction or improvement heuristics [27], respective learned methods exhibit some similar characteristics. While the first approaches of Nazari et al. [20] and Kool et al. [17] construct a solution to the CVRP sequentially one node at a time, other work [5, 18] focuses on iteratively improving an existing solution. In general improvement approaches assume an existing start solution which they can iteratively improve. While such an initial solution might be easily found for simple problems like the TSP or CVRP, this is increasingly difficult for more constrained problems. In fact just finding a first feasible solution for the CVRP-TW given a fixed number of vehicles is a NP-hard problem all by

Preprint. Under review.

itself [22]. Another shortcoming of improvement approaches lies in their expensive iterative nature. Compared to construction methods which can produce a useful solution in one forward pass within seconds, iterative improvement requires a not negligible number of iterations to achieve a suitable performance. This number grows relative to the problem complexity, number of customers N and the quality of the initial solution while constructive approaches always require only N steps for greedy decoding. Finally an improvement method can always be used on top of the solution produced by constructive approaches. One drawback of existing sequential construction approaches in case of the CVRP-TW is the very limited information on which the next decision of the agent is based. While our results show that this is of minor concern for the CVRP, the highly restricted solution space of the CVRP-TW leads to large inefficiencies when tours are created sequentially without any information about the other tours at a given time. Most importantly this problem is not resolved by search and sampling methods often employed for constructive methods, since they represent a very inefficient way of exploring the space of sequentially constructed solutions.

## Contributions:

- 1. We show that existing constructive machine learning methods for other, less constrained discrete optimization problems cannot easily be generalized to the CVRP-TW and hypothesize that this is due to consecutively constructing single tours.
- 2. We propose a more expressive policy model JAMPR, that operates on several tours in parallel. To do so, we designed (i) a more comprehensive state and action space than existing methods, providing sufficient information about other tours based on an enhanced feature embedding for nodes, tours and vehicles, and (ii) a policy decoder model that jointly selects both, the next location to visit and the tour to which this location should be added.
- 3. In experiments on three variants of the CVRP-TW ( hard , partly-soft , soft ) we show that our method provides vastly better solutions than existing discrete optimization methods as well as state-of-the-art ML methods adapted to cope with time windows for two of these settings, while being on par with the discrete optimization methods for the third setting, still outperforming the adapted machine learning methods there.

## 2 Related Work

The first deep learning model for sequential solutions to VRPs was proposed by Nazari et al. [20] who adapted the Pointer Network (PtrNet) of Vinyals et al. [30] to work on the CVRP. The original PtrNet was based on a supervised learning approach later extended to reinforcement learning (RL) by Bello et al. [1], both only for the TSP. Nazari et al. [20] dropped the original RNN part of the encoder model completely and replaced it with a linear embedding layer with shared parameters. The decoder remained a RNN with pointer attention which sequentially produces a permutation of the input nodes. The more recent model of Kool et al. [17] replaced this architecture with an adapted transformer model employing self-attention [28] instead. Concurrently Deudon et al. [6] combined the PtrNet model with heuristic post-processing to tackle the TSP. This problem is also the focus of more recent works [7, 33] which employ Monte-Carlo Tree Search similar to Alpha-Go [25]. Chen and Tian [5] propose an RL based improvement approach that iteratively chooses a region of a graph representation of the problem and then selects and applies established local heuristics. This approach was further improved by a disruption operator introduced by Lu et al. [18]. Khalil et al. [15] propose a Q-learning based method for the TSP and other graph related problems while Kaempfer and Wolf [14] use a supervised approach involving permutation invariant pooling to tackle the Multiple TSP. Another emerging approach by Gasse et al. [9] uses ML to find exact combinatorial solutions.

First machine learning approaches to tackle the CVRP-TW (with hard TW) have only been proposed very recently. To the best of our knowledge up to date there exist only two (improvement-based) methods. Gao et al. [8] use an enhanced version of the graph attention network [29] to learn a heuristic for Very Large-scale Neighborhood Search [23] including improvement and destruction operators similar to [18]. While they are able to tackle large instances of up to 400 nodes, the performance gains compared to standard heuristic selection methods are only around 4-5%. Silva et al. [24] learn the sequence of 8 different neighborhood functions for a Variable Neighborhood Descent heuristic [19] with tabular Q-learning. However, they only learn on a per-instance basis re-initializing the Q-table with zeros for each new instance. Instead we focus on the general machine learning approach to learn a policy based on the whole distribution of problem instances and build our model on top of

the most advanced constructive method, the attention model [17]. A recent survey on combinatorial optimization with machine learning techniques was presented by Bengio et al. [2] and Guo et al. [10] while more traditional heuristic approaches are discussed e.g. in [27].

## 3 Problem setting and base model

## 3.1 Vehicle routing problems

glyph[negationslash]

glyph[negationslash]

CVRP The Capacitated Vehicle Routing Problem can be defined on a graph G = {V E , , q, c } with node set V consisting of N +1 nodes, one depot node 0 and N customer nodes 1 , ..., N . Each node i has attributes including its Euclidean coordinates r i ∈ R 2 and a demand q i &gt; 0 , i ∈ V ′ (where we define V ′ = V { \ 0 } ) that needs to be satisfied. For the depot we set q 0 = 0 . The edge weights c ij of edges E = { { i, j } | i, j ∈ V , i = j } are given by the transit costs from node i to node j . Furthermore there are K homogeneous vehicles with same capacity Q &gt; 0 representing the fleet. Without loss of generality we normalize demands ˜ = q i q i Q and set ˜ = 1 Q . The tour s k of vehicle k ∈ K is a sequence of indices w.r.t. a subset of all customers nodes representing the order in which vehicle k visits the respective nodes. Furthermore every tour implicitly starts and ends at the depot. The set of sequences S = { s , ..., s 1 K } constitutes a solution to the problem instance when (1) all customers are visited, i.e. ⋃ s ∈ S v s ( ) = V ′ , (2) no customer is visited more than once, i.e. v s ( k ) ∩ v s ( l ) = ∅ ∀ ; k, l ∈ K, k = l and (3) all tours respect the capacity constraint ∑ i ∈ v s ( k ) q i ≤ 1 , ∀ k ∈ K .

CVRP-TW This extension of the CVRP is concerned with customers which can only be served within a specific time window. Moreover the service at each customer i ∈ V ′ requires a specific service duration h i . A time window (TW) consists of a tuple [ a , b i i ] , a i ≤ b i where a i and b i are upper and lower bounds on the possible arrival time. Due times τ are included by setting b i = τ -h i . The TW of the depot [ a , b 0 0 ] constitutes the available planning horizon regarding earliest possible departure from and latest return to depot. Edge weights c ij represent transit costs in time units and without loss of generality include the service duration h i of the node i from which the vehicle departs. A more comprehensive overview of the VRP and its variants can be found in [27] and [4].

## 3.2 Learning-to-Optimize problem

While classical discrete optimization algorithms tackle each VRP from scratch and in isolation, the Learning-to-Optimize problem consists of finding solutions to a problem, given, we have seen and solved many such problems (from the same distribution of problems) already, without having access to the optimal solutions themselves. Given a sampler for problems ( c, q, a, b ) ∼ p and a cost function cost , learn a solver ˆ s that minimizes the cost of solutions for future problems from the same distribution

$$\min \mathbb { E } _ { ( c, q, a, b ) \sim p } ( \text{cost} ( \hat { s } ( c, q, a, b ) ) ) & & ( 1 )$$

## 3.3 Attention model for the sequential construction of VRP solutions

Constructive methods [20, 17] treat the Learning-to-Optimize problem as a sequential decision making problem which constructs the solution piece by piece. The most advanced model of this class, the attention model (AM) [17], constructs one route s at a time by treating all not yet visited locations i as actions. It learns a policy model π i ( ( ) t | c, q, a, b, s ( t -1) ; θ ) and consequently s ( ) t = [ s ( t -1) ; i ( ) t ] .

Encoder The encoder model takes the node-wise features x i = ( r , q i i ) ∈ R 3 , i ∈ V consisting of coordinates r i and demand q i as input and first applies a linear projection z 0 i = W x in i + b in to create an initial embedding z 0 i ∈ R d emb with embedding dimension d emb. The main part of the encoder consists of a stack of three self-attention blocks (SA) producing embeddings

$$\omega _ { i } ^ { \text{node} } = \text{SA} ( \text{ SA} ( \text{ SA} ( z _ { i } ^ { 0 }, Z ^ { 0 } ) ) ), \text{ } i \in \mathcal { V }$$

where Z 0 = ( z 0 0 , . . . , z 0 N ) is the sequence of initial embeddings. Each block consists of a multi-head attention layer (MHA) [28], an element-wise fully connected layer (FF), each in turn then followed by a residual connection (res) [11] and a batch normalization layer (BN) [13]:

$$z _ { i } ^ { l } = S A ( z _ { i } ^ { l - 1 }, Z ^ { l - 1 } ) = B N ( \ F F ^ { r e s } ( \ B N ( \ M H A ^ { r e s } ( z _ { i } ^ { l - 1 }, ( z _ { 0 } ^ { l$$

The multi-head attention layer is a linear combination of H = 8 single-head attention (SHA) layers each taking a slice of all the input elements (see appendix A for more details):

$$\text{MA} ( z, Z ; W ) = \sum _ { h = 1 } ^ { H } W _ { h } ^ { \text{head} } SHA ( z _ { \text{slice} ( h ) }, Z _ {, \text{slice} ( h ) } ; W ) \\ \cdot \cdot \cdot \cdot \$$

A single-head attention layer is a convex combination of linearly transformed elements with parametrized pairwise attention weights:

$$\text{SHA} ( z, Z ; W ) = \sum _ { j = 1 } ^ { | Z | } \text{att} ( z, Z ; W ^ { \text{query} }, W ^ { \text{key} } ) _ { j } W ^ { \text{value} } Z _ { j } \quad \quad \quad$$

$$\text{attn} ( z, Z ; W ^ { \text{query} }, W ^ { \text{key} } ) = \text{softmax} \left ( \frac { 1 } { \sqrt { d _ { \text{key} } } } z ^ { T } ( W ^ { \text{query} } ) ^ { T } W ^ { \text{key} } Z _ { j } \, | \, _ { j = 1, \dots, | Z | } \right ), \ \ z \in Z$$

Fully connected and residual layers are defined as usual:

$$\text{MHA} ^ { \text{res} } ( z _ { i }, Z ; W ) = z _ { i } + \text{MHA} ( z _ { i }, Z ; W )$$

$$\text{FF} ( z _ { i } ; W, b ) = \max ( 0, W z _ { i } + b )$$

$$\text{FF} ^ { \text{res} } ( z _ { i } ; W, b ) = z _ { i } + \text{FF} ( z _ { i } ; W, b ).$$

Decoder At decoding step t ∈ { 1 , ..., T } the decoder takes a context C ( ) t and selects the next node to add to the current tour by attending over the sequence M = ( ω node 0 , ω node 1 , ..., ω node N ) . The context C ( ) t consists of the concatenation of the graph embedding ω graph = 1 N +1 ∑ N i =0 ω node i , the remaining capacity of the current vehicle Q ( ) t f and the embedding ω last = ω node last( s ) of the preceding node (the depot in case of a new tour):

$$C ^ { ( t ) } = [ \omega ^ { \text{graph} } ; Q _ { f } ^ { ( t ) } ; \omega ^ { \text{last} } ]$$

where [ ; ] represents the concatenation operator. Its corresponding dimension is d C = 2 d node +1 with d node as the dimensions of the node embedding. The decoder consists of a multi-head attention layer and a subsequent layer with just attention weights operating on a masked input sequence, where values of alternatives, which are ruled out by hard constraints of the routing problems, are set to -∞ and thus yield weights of zero:

$$p _ { i } = \pi ( s _ { t } = i \, | \, x, s _ { 1 ; t - 1 }, \theta ) = \text{attn} ( M H A ( C ^ { ( t ) }, M ), \text{mask} ( M ) ) \quad \quad ( 1 1 )$$

## 4 Attending multiple routes (JAMPR)

Regarding the CVRP-TW, existing sequential construction approaches perform quite poorly because of the independent sequential construction of single tours and the very limited information on which each decision of the agent is based. We address these shortcomings in our Joint Attention Model for Parallel Route-Construction: JAMPR (pronounce "jumper") which introduces two major extensions to AM (described in section 3.3).

While the sequential construction of single tours is very efficient in terms of memory and computational complexity and was shown to work quite well for the CVRP, it faces some major challenges for more complex routing settings. Existing approaches appear to be sub-optimal since at time step t in the creation of a specific tour the internal state representation for the decoder given by context C ( ) t and M comprises very limited information about the other existing tours. Obviously in sequential decoding for the first tour there is no information about later tours. However, even for later tours the context only provides information about the preceding node. Nodes that are not available since they were already served by other tours are only considered by the masking scheme when calculating the compatibilities in the decoder, effectively pruning the action space but not providing any additional information.

## 4.1 Comprehensive context information

In order to provide sufficient information to the decoder at any time step we create a more comprehensive context by extending the state representation and adding two additional encoders for tours and vehicles. To simplify notation the whole section omits the index ( ) t of the current time step.

Figure 1: Illustration of the model architectures of JAMPR and AM [17] for one decoding step. Our model employs 2 additional encoders producing a comprehensive context C and an enhanced joint action space represented by ¯ M . OBS are the state observations of the problem at the current step.

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000000_71c7a495b5ee6b2c1575d4173ea9382bf34c6866ce3c38d3e5aaf10cd0810fe9.png)

First we consider a state representation including a tour plan S , represented by a K × L matrix consisting of K tours of maximal length L which is initialized with all zeros. When a node is added to a tour k we insert the corresponding index in place of the first zero, the second node index in place of the next zero and so on.

The additional encoders encode (i) vehicle features v k , consisting of the vehicle index k , the current return cost to the depot, the current position of the vehicle (given by the coordinates of the last served node), and the current time of each vehicle, as well as (ii) the tour itself, consisting of the nodes visted so far. The vehicle features are encoded by a feed forward NN g v , the tour by the average output of a second feed forward NN g s fed with the embeddings ω node i of the visited nodes i so far:

$$\omega _ { k } ^ { \text{vehicle} } = [ g _ { v } ( v _ { k } ) ; \frac { 1 } { L } \sum _ { i \in s _ { k } } g _ { s } ( \omega _ { i } ^ { \text{node} } ) ]$$

Our context also includes information about the whole fleet and the current active vehicles (see section 4.2), simply as the averages of their embeddings:

$$\omega ^ { \text{fleet} } = \frac { 1 } { K } \sum _ { k = 1 } ^ { K } \omega ^ { \text{vehicle} } _ { k }, \quad \omega ^ { \text{act} } = \frac { 1 } { K _ { \text{act} } } \sum _ { k \in K _ { \text{act} } } \omega ^ { \text{vehicle} } _ { k }, \quad \omega ^ { \text{last} } = \frac { 1 } { K } \sum _ { k = 1 } ^ { K } \omega ^ { \text{node} } _ { \text{last} ( s _ { k } ) }$$

where last( s k ) denotes the last visited node in tour s k . Together with the initial graph embedding ω graph and the embedding ω node 0 of the depot node, the context embedding then is defined as

$$C = [ \omega ^ { \text{graph} } ; \omega ^ { \text{fleet} } ; \omega ^ { \text{act} } ; \omega _ { 0 } ^ { \text{node} } ; \omega ^ { \text{last} } ].$$

with a dimension of d C = 3 d node +2 d vehicle where d node and d vehicle are the dimensions of the node and vehicle embedding respectively. The complete architecture of our model compared to the original AMis shown in figure 1.

## 4.2 Enhanced joint action space for parallel route construction

For highly constrained problems, we furthermore propose to construct several routes S in parallel, and treat all pairs of routes s k ∈ S and not yet visited locations i as actions, i.e., to learn a policy model

glyph[negationslash]

π k ( ( ) t , i ( ) t | c, q, a, b, S ( t -1) ; θ ) . Then S ( ) t k ( t ) = [ S ( t -1) k ( t ) ; i ( ) t ] and all other S ( ) t k = S ( t -1) k ( k = k ( ) t ) stay the same. Therefore we combine the node embeddings ω node with the vehicle embeddings ω vehicle creating the joint combinatorial space of all nodes and vehicles which represents the new M :

$$M = \{ \ g _ { a } ( \omega _ { k } ^ { \text{vehicle} }, \omega _ { i } ^ { \text{node} } ) \ | \ k = 1, \dots, K, i \in \mathcal { V } \}$$

with a NN g a : R d node + d vehicle → R d M that models the compatibility or affinity between a tour and a node according to

$$g _ { a } ( \omega _ { k } ^ { \text{vehicle} }, \omega _ { i } ^ { \text{node} } ) = \left ( W _ { 1 } \omega _ { i } ^ { \text{node} } + W _ { 2 } \omega _ { k } ^ { \text{vehicle} } + W _ { 3 } [ \omega _ { k } ^ { \text{vehicle} } \odot \omega _ { i } ^ { \text{node} } ; ( \omega _ { k } ^ { \text{vehicle} } ) ^ { T } \omega _ { i } ^ { \text{node} } ] \right ) \quad ( 1 4 )$$

the sum of the projected node and vehicle embedding and the projected concatenation of the elementwise ( glyph[circledot] ) and dot products with suitable weight vectors W to project the respective components to the shared dimension d M .

Since the size of M is subject to combinatorial explosion w.r.t. N and K we fix the number of active vehicles for which decisions can be done concurrently to m con (at any time there are exactly m con vehicles active). If a vehicle has no feasible nodes left (because of capacity or time constraints) or it selects the depot node, it returns to the depot and is set inactive. Then a new tour is initialized by activating the next available vehicle. We define M ( ) t which changes according to the currently active vehicles K ( ) t act at time step t but has a constant size | M ( ) t | = m con · N +1 as:

$$M ^ { ( t ) } = \{ g _ { a } ( \omega _ { k } ^ { \text{vehicle} }, \omega _ { i } ^ { \text{node} } ) \ | \ k \in K _ { \text{act} } ^ { ( t ) }, i \in \mathcal { V } \}$$

## 5 Experiments

## 5.1 Experiment setup

Problem Details We evaluate JAMPR for three different variants of the CVRP-TW, which differ in the way they handle the respective time windows (TW) and consequently are concerned with different application scenarios:

- 1. TW1 The first variant is the most well-known problem with hard TW. Vehicles can only serve customers within the TW given by [ a , b i i ] . When too early they wait at no additional cost until a i while too late vehicles are not able to serve the respective customer.
- 2. TW2 For the second type of problem the latest arrival time b i is a soft constraint which can be violated by paying a corresponding penalty. The late deviation of vehicle k is defined as δ k b i = max( T ik -b , i 0) where T ik is the arrival time of vehicle k at node i . Vehicles that are too early wait at no cost until a i .
- 3. TW3 The third variant involves soft constraints for both the upper and the lower bound of the TW. They can be violated by paying the respective penalty. The early deviation of vehicle k is defined as δ k a i = max( a i -T ik , 0) .

Respective penalties involve a suitable penalty function λ (e.g. λ x ( ) = x or λ x ( ) = x 2 ) which is monotonously increasing in R + . Finally the cost function is defined wr.t. the total time required by all vehicles and the incurred penalties according to:

$$\cos ^ { \mu } _ { k = 1 } \left ( \sum _ { ( i, j ) \in s _ { k } \varpi \ 0 } c _ { i j } + \alpha \lambda ( \delta _ { a _ { i } } ^ { k } ) + \beta \lambda ( \delta _ { b _ { i } } ^ { k } ) \right )$$

where α, β ≥ 0 are respective weights, j is the index following i in the corresponding sequence s k glyph[unionmulti] 0 where the 0 index is included at start and end to take care of departure and return to the depot.

Data Generation For the CVRP-TW we sample suitable problem instances from a distribution based on the statistics of the R201 instance of the well-known benchmark problem set of Solomon [26]. Capacities are set to Q 20 = 500 and Q 50 = 750 for problem size 20 and 50 respectively. The full time horizon is [ a 0 = 0 , b 0 = 1000] while the service durations h i are uniformly set to 10. More details can be found in the supplementary material. For the generation of instances for the CVRP we follow the sampling procedure of [17] and use the same vehicle capacities of Q 20 = 30 and Q 50 = 40 as well as the same validation and test sets.

Training and Hyper-Parameters We train our model with the policy gradient based on the established REINFORCE algorithm [32] with the rollout baseline proposed in [17]. We employ the Adam optimizer [16] with a smooth learning rate decay schedule according to η t = ( 1 1+ γt ) η t -1 at epoch t with decay factor γ = 0 001 . and an initial learning rate of 10 -4 . Our node encoder consists of 3 SA blocks with dim=128, the tour and vehicle encoder both have a hidden dimension of 64. The tour encoder has 2 layers while the vehicle encoder uses 1 layer for the CVRP and 3 layers for the CVRP-TW. The embedding dimensions for all problems are given by d node = d vehicle = 128 . For the decoder we use a hidden dimension of 256. Furthermore we clip the norm of the gradients to 1. The model specific hyper-parameter m con controlling the number of concurrently constructed tours is tuned with a grid search between 1 and 4 on the validation set. We train the models for 50 epochs, each epoch involving 1,024,000 training instances with a problem size dependent batch size of BS 20 = 512 and BS 50 = 128 . We find that our models are mostly converged after 50 epochs, but train them nevertheless for 100 epochs on the CVRP to be comparable to related work. Our code will be made available with the published paper on github: https://github.com/AnonymousAuthors.

Baseline models We compare our model to the current learning based state-of-the-art model for sequential route construction AM [17] which we adapt for the CVRP-TW by including the current time of the vehicle in the context and an updated masking scheme considering TW (AM +TW ). Furthermore we compare against the Google OR-Tools (GORT) library [21] which frequently serves as established (meta-)heuristic baseline in the related work. We apply it with two different configurations regarding the underlying local search heuristic, for which we select either automatic selection (AU) or guided local search (GLS) . Additionally we compare against related work for the standard CVRP including the VRP extension of the PointerNet (PtrNet) [20], the two improvement methods NeuRewriter [5] and Learning to improve (L2I) [18], and LKH3 [12] and GORT as heuristic approaches (from [18]).

Inference Setup We report results for greedy decoding and sampling where 1280 solutions are sampled from the stochastic policy π (as one batch) and we report the best one. For AM +TW we compare results for the same number of samples (1280) and for approximately the same inference time assuming sequential processing of sample batches (10240 samples). The reported inference times are the approx. time required to solve a single instance (BS=1) averaged over the test set. Since run times are hard to compare because of different batch size and parallelization capability we argue for this practical approach focusing on the most common use case where one single problem instance is solved at a time. However, the learned models are much better in leveraging parallelization via mini-batch processing on a GPU to quickly solve larger amounts of instances. Consequently because of the varying evaluation protocols in the related work most inference times are not comparable and the respective number k of used vehicles is not known (marked by "-"). We learn our models on a single GPU (Nvidia 1080TI) while GORT is run on CPU (Intel Xeon E5-2670v2).

## 5.2 Results

Table 1 shows the results of our experiments on the three described variants of the CVRP-TW. We report the average cost on the test set (10000 instances). According to [4] the main objective in most CVRP-TW is to minimize the number of required vehicles to serve all nodes while respecting all constraints. The minimization of distance or time is usually only a secondary objective. For VRP these objectives have been shown to be conflicting so that the reduction in the number of used vehicles k often causes an increase in total distance [3]. For this reason we report the average k as well.

JAMPR outperforms AM +TW on all CVRP-TW by a large margin. Compared to GORT we also achieve significantly better results on TW1 and TW3. For TW2 our model finds solutions of similar quality while being much faster overall. Although inference for AM +TW for greedy decoding and sampling is faster, the results are much worse. Taking the speed into consideration we perform inference for AM +TW with 8 times as many samples ( t 10240 ). While this improves the results, it is still significantly worse than JAMPR for all CVRP-TW and for GORT for all but TW3. For the standard CVRP our model performs slightly worse than AM but still achieves reasonable results showing that the model is also useful for the vanilla CVRP without TW. Although it is no explicit part of our model or objective function, we find that our model generally creates solutions with a much lower average k than AM +TW as well as GORT.

In general it seems that for bigger problem instances requiring a larger total number of vehicles, a higher concurrency controlled by m con is advantageous. In contrast for small instances that only

Table 1: Comparison of results for CVRP and the CVRP-TW variants. Best results for each problem are bold . Results taken from related work are marked with † .

| Model              | N=20    | N=20   | N=20   | N=50     | N=50   | N=50   |
|--------------------|---------|--------|--------|----------|--------|--------|
|                    | cost    | k      | t inf  | cost     | k      | t inf  |
| GORT - AU          | 2577.08 | 4.18   | 0.22s  | 4344.88  | 6.30   | 1.76s  |
| GORT - GLS         | 2522.85 | 4.11   | 8.00s  | 4213.23  | 6.15   | 8.06s  |
| AM +TW (greedy)    | 3766.88 | 5.29   | 0.05s  | 7189.43  | 9.42   | 0.12s  |
| AM +TW (sampl.)    | 3041.24 | 5.74   | 0.12s  | 7327.09  | 11.92  | 0.38s  |
| AM +TW ( t 10240 ) | 2750.06 | 5.27   | 0.95s  | 6878.84  | 11.28  | 3.08s  |
| JAMPR (greedy)     | 1862.40 | 2.25   | 0.10s  | 3055.94  | 5.42   | 0.24s  |
| JAMPR (sampl.)     | 1716.60 | 2.29   | 0.86s  | 2691.55  | 4.03   | 3.07s  |
| GORT - AU          | 635.06  | 4.23   | 0.37s  | 1123.82  | 6.72   | 3.81s  |
| GORT - GLS         | 619.57  | 4.14   | 8.30s  | 1119.07  | 6.67   | 8.09s  |
| AM +TW (greedy)    | 7615.69 | 2.00   | 0.05s  | 40245.40 | 2.00   | 0.12s  |
| AM +TW (sampl.)    | 1572.31 | 6.56   | 0.11s  | 7712.35  | 9.34   | 0.34s  |
| AM +TW ( t 10240 ) | 1387.30 | 6.46   | 0.90s  | 6730.55  | 9.99   | 2.70s  |
| JAMPR (greedy)     | 674.72  | 4.32   | 0.11s  | 1273.20  | 6.02   | 0.25s  |
| JAMPR (sampl.)     | 620.68  | 4.19   | 0.92s  | 1116.76  | 5.64   | 2.32s  |
| GORT - AU          | 1317.81 | 4.07   | 1.07s  | 2707.72  | 6.12   | 20.63s |
| GORT - GLS         | 1312.71 | 4.11   | 8.00s  | 2753.66  | 6.18   | 8.02s  |
| AM +TW (greedy)    | 3101.21 | 2.00   | 0.05s  | 26467.34 | 2.00   | 0.12s  |
| AM +TW (sampl.)    | 1412.16 | 3.42   | 0.12s  | 4161.24  | 7.15   | 0.34s  |
| AM +TW ( t 10240 ) | 1318.56 | 3.23   | 0.88s  | 3941.47  | 6.77   | 2.67s  |
| JAMPR (greedy)     | 1002.81 | 1.00   | 0.10s  | 3158.26  | 2.01   | 0.23s  |
| JAMPR (sampl.)     | 844.35  | 1.39   | 0.78s  | 1947.65  | 2.29   | 2.13s  |
| LKH3 †             | 6.14    | -      | -      | 10.38    | -      | -      |
| GORT †             | 6.43    | -      | -      | 11.31    | -      | -      |
| NeuRewriter †      | 6.16    | -      | -      | 10.51    | -      | -      |
| L2I †              | 6.12    | -      | -      | 10.35    | -      | -      |
| PtrNet † (greedy)  | 6.59    | -      | -      | 11.39    | -      | -      |
| PtrNet † (beam)    | 6.40    | -      | -      | 11.15    | -      | -      |
| AM(greedy)         | 6.40    | 5.90   | 0.03s  | 10.98    | 7.00   | 0.08s  |
| AM(sampl.)         | 6.25    | 5.22   | 0.05s  | 10.62    | 7.65   | 0.20s  |
| JAMPR (greedy)     | 6.47    | 4.06   | 0.11s  | 11.44    | 7.31   | 0.23s  |
| JAMPR (sampl.)     | 6.26    | 3.97   | 0.84s  | 10.84    | 7.13   | 2.81s  |

require 2-3 vehicles, m con = 1 or m con = 2 works better. Furthermore larger m con seems to be helpful for more constrained problems like TW1 with hard time windows while m con = 1 is sufficient for the TW3 where the only hard constraint is the capacity. As expected AM +TW performs quite poorly on all TW problems because of the limited information in the context and the simplistic action space. While large numbers of samples help to some extend to improve the AM +TW results for small problems ( t 10240 ), this effect seems to be diminished by the combinatorial explosion of the solution space for larger instances.

## 6 Conclusion

In this paper we introduced a new constructive model for solving highly constrained VRP. It is the first attempt for a learned constructive approach to tackle the CVRP-TW where our model shows strong results even outperforming a problem specific meta-heuristic approach. This contributes to the advancement of discrete optimization approaches based on learned models which this work successfully extends to much more difficult problem settings. In the future we plan to take a look at online optimization scenarios. In that case the comprehensive context should enable online decoding or respectively partial route construction, since the provided comprehensive information about the problem state transforms the decoding procedure into an approximate Markov Decision Process respecting the Markov property.

## References

| [1]   | Irwan Bello, Hieu Pham, Quoc V. Le, Mohammad Norouzi, and Samy Bengio. Neural Com- binatorial Optimization with Reinforcement Learning. arXiv:1611.09940 [cs, stat] , November 2016. URL http://arxiv.org/abs/1611.09940 . arXiv: 1611.09940.                                                                                                                                                                                                                                                       |
|-------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [2]   | Yoshua Bengio, Andrea Lodi, and Antoine Prouvost. Machine Learning for Combinatorial Optimization: a Methodological Tour d'Horizon. arXiv:1811.06128 [cs, stat] , November 2018. URL http://arxiv.org/abs/1811.06128 . arXiv: 1811.06128.                                                                                                                                                                                                                                                           |
| [3]   | Olli Bräysy. Local search and variable neighborhood search algorithms for the vehicle routing problem with time windows . Vaasan yliopisto, 2001.                                                                                                                                                                                                                                                                                                                                                   |
| [4]   | Olli Bräysy and Michel Gendreau. Vehicle routing problem with time windows, part i: Route construction and local search algorithms. Transportation science , 39(1):104-118, 2005.                                                                                                                                                                                                                                                                                                                   |
| [5]   | Xinyun Chen and Yuandong Tian. Learning to perform local rewriting for combinatorial optimization. In Advances in Neural Information Processing Systems , pages 6278-6289, 2019.                                                                                                                                                                                                                                                                                                                    |
| [6]   | Michel Deudon, Pierre Cournut, Alexandre Lacoste, Yossiri Adulyasak, and Louis-Martin Rousseau. Learning Heuristics for the TSP by Policy Gradient. In Willem-Jan van Hoeve, editor, Integration of Constraint Programming, Artificial Intelligence, and Operations Research volume 10848, pages 170-181. Springer International Publishing, Cham, 2018. ISBN 978-3- 319-93030-5 978-3-319-93031-2. doi: 10.1007/978-3-319-93031-2_12. URL http://link. springer.com/10.1007/978-3-319-93031-2_12 . |
| [7]   | Zhang-Hua Fu, Kai-Bin Qiu, Meng Qiu, and Hongyuan Zha. Targeted sampling of en- larged neighborhood via Monte Carlo tree search for TSP. September 2019. URL https: //openreview.net/forum?id=ByxtHCVKwB .                                                                                                                                                                                                                                                                                          |
| [8]   | Lei Gao, Mingxiang Chen, Qichang Chen, Ganzhong Luo, Nuoyi Zhu, and Zhixin Liu. Learn to design the heuristics for vehicle routing problem. arXiv preprint arXiv:2002.08539 , 2020.                                                                                                                                                                                                                                                                                                                 |
| [9]   | Maxime Gasse, Didier Chételat, Nicola Ferroni, Laurent Charlin, and Andrea Lodi. Exact combinatorial optimization with graph convolutional neural networks. In Advances in Neural Information Processing Systems , pages 15554-15566, 2019.                                                                                                                                                                                                                                                         |
| [10]  | Tiande Guo, Congying Han, Siqi Tang, and Man Ding. Solving combinatorial problems with machine learning methods. In Nonlinear Combinatorial Optimization , pages 207-229. Springer, 2019.                                                                                                                                                                                                                                                                                                           |
| [11]  | Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. Deep residual learning for image recognition. In Proceedings of the IEEE conference on computer vision and pattern recognition pages 770-778, 2016.                                                                                                                                                                                                                                                                                          |
| [12]  | Keld Helsgaun. An extension of the lin-kernighan-helsgaun tsp solver for constrained traveling salesman and vehicle routing problems. Roskilde: Roskilde University , 2017.                                                                                                                                                                                                                                                                                                                         |
| [13]  | Sergey Ioffe and Christian Szegedy. Batch normalization: Accelerating deep network training by reducing internal covariate shift. arXiv preprint arXiv:1502.03167 , 2015.                                                                                                                                                                                                                                                                                                                           |
| [14]  | Yoav Kaempfer and Lior Wolf. Learning the Multiple Traveling Salesmen Problem with Permutation Invariant Pooling Networks. arXiv:1803.09621 [cs, stat] , February 2019. URL http://arxiv.org/abs/1803.09621 . arXiv: 1803.09621.                                                                                                                                                                                                                                                                    |
| [15]  | Elias Khalil, Hanjun Dai, Yuyu Zhang, Bistra Dilkina, and Le Song. Learn- ing Combinatorial Optimization Algorithms over Graphs. In I. Guyon, U. V. Luxburg, S. Bengio, H. Wallach, R. Fergus, S. Vishwanathan, and R. Gar- nett, editors, Advances in Neural Information Processing Systems 30 , pages 6348- 6358. Curran Associates, Inc., 2017. URL http://papers.nips.cc/paper/ 7214-learning-combinatorial-optimization-algorithms-over-graphs.pdf .                                           |

- [16] Diederik P Kingma and Jimmy Ba. Adam: A method for stochastic optimization. arXiv preprint arXiv:1412.6980 , 2014.

| [17]   | Wouter Kool, Herke van Hoof, and Max Welling. Attention, learn to solve routing problems! In International Conference on Learning Representations , 2019. URL https://openreview. net/forum?id=ByxBFsRqYm .                                                                                                              |
|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [18]   | Hao Lu, Xingwen Zhang, and Shuang Yang. A Learning-based Iterative Method for Solving Vehicle Routing Problems. September 2019. URL https://openreview.net/forum?id= BJe1334YDH .                                                                                                                                        |
| [19]   | Nenad Mladenovi´ c and Pierre Hansen. Variable neighborhood search. Computers &operations research , 24(11):1097-1100, 1997.                                                                                                                                                                                             |
| [20]   | Mohammadreza Nazari, Afshin Oroojlooy, Lawrence Snyder, and Martin Takác. Reinforcement learning for solving the vehicle routing problem. In Advances in Neural Information Processing Systems , pages 9839-9849, 2018.                                                                                                  |
| [21]   | Laurent Perron and Vincent Furnon. Or-tools. URL https://developers.google.com/ optimization/ .                                                                                                                                                                                                                          |
| [22]   | Martin WP Savelsbergh. Local search in routing problems with time windows. Annals of Operations research , 4(1):285-305, 1985.                                                                                                                                                                                           |
| [23]   | Paul Shaw. Using constraint programming and local search methods to solve vehicle routing problems. In International conference on principles and practice of constraint programming , pages 417-431. Springer, 1998.                                                                                                    |
| [24]   | Maria Amélia Lopes Silva, Sérgio Ricardo de Souza, Marcone Jamilson Freitas Souza, and Ana Lúcia C Bazzan. A reinforcement learning-based multi-agent framework applied for solving routing and scheduling problems. Expert Systems with Applications , 131:148-171, 2019.                                               |
| [25]   | David Silver, Aja Huang, Chris J Maddison, Arthur Guez, Laurent Sifre, George Van Den Driessche, Julian Schrittwieser, Ioannis Antonoglou, Veda Panneershelvam, Marc Lanctot, et al. Mastering the game of go with deep neural networks and tree search. nature , 529(7587): 484, 2016.                                  |
| [26]   | Marius MSolomon. Algorithms for the vehicle routing and scheduling problems with time window constraints. Operations research , 35(2):254-265, 1987.                                                                                                                                                                     |
| [27]   | Paolo Toth and Daniele Vigo. Vehicle routing: problems, methods, and applications . SIAM, 2014.                                                                                                                                                                                                                          |
| [28]   | Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N Gomez, Łukasz Kaiser, and Illia Polosukhin. Attention is all you need. In Advances in neural information processing systems , pages 5998-6008, 2017.                                                                                    |
| [29]   | Petar Veliˇkovi´, c c Guillem Cucurull, Arantxa Casanova, Adriana Romero, Pietro Lio, and Yoshua Bengio. Graph attention networks. arXiv preprint arXiv:1710.10903 , 2017.                                                                                                                                               |
| [30]   | Oriol Vinyals, Meire Fortunato, and Navdeep Jaitly. Pointer Networks. In C. Cortes, N. D. Lawrence, D. D. Lee, M. Sugiyama, and R. Garnett, editors, Advances in Neural Information Processing Systems 28 , pages 2692-2700. Curran Associates, Inc., 2015. URL http:// papers.nips.cc/paper/5866-pointer-networks.pdf . |
| [31]   | Bryan Wilder, Eric Ewing, Bistra Dilkina, and Milind Tambe. End to end learning and optimization on graphs. In Advances in Neural Information Processing Systems , pages 4674- 4685, 2019.                                                                                                                               |
| [32]   | Ronald J Williams. Simple statistical gradient-following algorithms for connectionist reinforce- ment learning. Machine learning , 8(3-4):229-256, 1992.                                                                                                                                                                 |
| [33]   | Zhihao Xing and Shikui Tu. A Graph Neural Network Assisted Monte Carlo Tree Search Approach to Traveling Salesman Problem. September 2019. URL https://openreview. net/forum?id=Syg6fxrKDB .                                                                                                                             |

## A Attention Mechanism

A self-attention layer [28] transforms an input sequence Z := ( Z , Z , . . . , Z 1 2 J ) of vectors Z j ∈ R d in into an output sequence of vectors in R d out allowing to model dependencies between all input elements based on their values, but not on their position. A single-head attention layer (SHA) is a convex combination of linearly transformed elements (called values ):

$$\text{SHA} ( z, Z ; W ^ { \text{query} }, W ^ { \text{key} }, W ^ { \text{value} } ) = \sum _ { j = 1 } ^ { | Z | } \text{attn} ( z, Z ; W ^ { \text{query} }, W ^ { \text{key} } ) _ { j } W ^ { \text{value} } Z _ { j } \quad \ \ ( 1 7 )$$

with parametrized pairwise attention weights

$$\text{attn} ( z, Z ; W ^ { \text{query} }, W ^ { \text{key} } ) = \text{softmax} ( \frac { 1 } { \sqrt { d _ { \text{key} } } } z ^ { T } ( W ^ { \text{query} } ) ^ { T } W ^ { \text{key} } Z _ { j } \ | _ { j = 1, \dots, | Z | } ), \ \ z \in Z \ \ ( 1 8 )$$

$$W ^ { \text{value} } \in \mathbb { R } ^ { d _ { \text{out} } \times d _ { \text{in} } }, W ^ { \text{query} }, W ^ { \text{key} } \in \mathbb { R } ^ { d _ { \text{key} } \times d _ { \text{in} } }, d _ { \text{key} } \in \mathbb { N } \quad \quad ( 1 9 )$$

W query z is usually called the query , W key Z j the key for element j .

A multi-head attention layer (MHA) is a linear combination of several single-head attention layers (with ouput size d value ) each taking a slice of all the input elements:

$$\text{MHA} ( z, Z ; W ) = \sum _ { h = 1 } ^ { H } W _ { h } ^ { \text{head} } \text{SHA} ( z _ { \text{slice} ( h ) }, Z _ {., \text{slice} ( h ) } ; W _ { h } ^ { \text{query} }, W _ { h } ^ { \text{key} }, W _ { h } ^ { \text{value} } ) \quad \text{(20)}$$

$$\text{slice} ( h ; H, d _ { \text{in} } ) = ( 1 + h \Delta h, 2 + h \Delta h, \dots, \Delta h + h \Delta h ), \quad \Delta h \coloneqq \frac { d _ { \text{in} } } { H } \quad \quad ( 2 1 )$$

$$& W = ( W _ { h } ^ { \text{head} }, W _ { h } ^ { \text{query} }, W _ { h } ^ { \text{key} }, W _ { h } ^ { \text{value} } ) _ { h = 1 \colon H } & & ( 2 2 ) \\ & W ^ { \text{head} } _ { h } \in \mathbb { R } ^ { d _ { \text{out} } \times d _ { \text{value} } }, W _ { h } ^ { \text{value} } \in \mathbb { R } ^ { d _ { \text{value} } \times d _ { \text{in} } }, d _ { \text{value} } \in \mathbb { N }, & & ( 2 3 )$$

## B Batch Normalization

The batch normalization component (BN) [13] used in the encoder can be defined as

$$\text{BN} ( z _ { i } ; W, b ) = W \odot \frac { z _ { i } - \mu _ { B } } { \sqrt { \sigma _ { B } ^ { 2 } + \epsilon } } + b$$

with mean µ B and variance σ 2 B of mini-batch B , noise term glyph[epsilon1] and learnable parameters W and b .

## C Recovering decisions from joint action space embedding

To recover the original decision space regarding the selected vehicle k ∗ and selected node n ∗ from M ( ) t we use an index mapping ϕ : N → N 2 mapping the index of each embedding vector back to the corresponding decision pair ( k , n ∗ ∗ ) :

$$\varphi ( m ) = ( k ^ { * }, n ^ { * } )$$

where m ∈ | M | is the selected index from the combinatorial space of M ( ) t .

## D Considerations regarding computational efficiency

To make the proposed model computationally feasible we separate the embeddings into their respective static and dynamic components. The routing problems we are concerned with in this paper are deterministic and static and correspondingly the embeddings ω node and accordingly ω graph are completely static and can be easily pre-computed.

glyph[negationslash]

Furthermore we propose to construct several routes S in parallel, learning a policy model π k ( ( ) t , i ( ) t | c, q, a, b, S ( t -1) ; θ ) where S ( ) t k ( t ) = [ S ( t -1) k ( t ) ; i ( ) t ] and all other S ( ) t k = S ( t -1) k ( k = k ( ) t )

stay the same. Consequently ω vehicle only changes in the dimension of one specific vehicle k at each decoding step while it stays the same for all other vehicles. Therefore we only need to update the latent embedding of the currently selected vehicle k with the encoder models and then just recompute the means for ω fleet and ω act . The same argument holds for the static and dynamic components of M where ω node is static and can be pre-computed while ω vehicle as well as the interactions ω had and ω dot need to be updated respectively.

## E Reinforcement learning framework

Similar to previous approaches we employ a policy gradient based on the REINFORCE algorithm [32] with baseline. We adopt the rollout baseline proposed by Kool et al. [17] since it was shown to work robustly well for this type of problem. Based on small preliminary experiments with a moving exponential average baseline and a learned critic model we can confirm these findings. For this reason we employ the aforementioned rollout baseline which corresponds to a greedy rollout of the best model checkpoint found so far. The evaluation of the model checkpoint is done on a separate validation set which is re-sampled each episode to prevent over-fitting. The significance of the performance difference is checked with a paired t-test with α = 0 05 . . We configure the baseline with the same specification as [17] including a warm-up of 1 epoch via exponential average ( β = 0 8 . ).

## F Data generation for the CVRP-TW

Here we give details for the generation of the data for the CVRP-TW training, validation and test set. To implement a suitable data generation protocol we took a look on the well-known benchmark problem set of Solomon [26] (http://web.cba.neu.edu/~msolomon/problems.htm). It consists of CVRP-TW instances representing different setups regarding

- 1. Geographical Data: There are three different approaches to sample the location of customers and the depot: uniformly at random (R) , clustered (C) and both random and clustered (RC) .
- 2. Scheduling Horizon: The instances are sampled for two different scenarios, either a short horizon (1) for many vehicles serving small numbers of customers or a long horizon (2) where a smaller number of vehicles is able to serve a larger number of customers each.
- 3. TWwidth: The width of the TW effects the flexibility of planning the tours within a specific time interval.
- 4. TWdensity: The density of TW controls how many of the customers are constrained by hard TW, either 25, 50, 75 or 100%.

For our experiments we select the R201 instance of the benchmark as master sample since it suits our use case the best. It consists of randomly sampled geographical data with a long scheduling horizon and 100% of the customers are constrained by short to medium sized TW. All service durations are 10 time units and the standard capacity of all vehicles for problem size 100 is Q 100 = 1000 . The service horizon is given by the depot time window [ a 0 = 0 , b 0 = 1000] . The corresponding demands q i of the instance have a mean of 17.24 and standard deviation of 9.4175.

In order to sample similar instances we define the following constraints and sampling routines described below:

- · Locations: The locations of the customers and the depot are sampled uniformly from the interval [0, 100].
- · Demands: and then clamp its absolute value to integer

For the demand q i of customer i we sample from a normal distribution ˆ q ∼ N ( µ, σ ) with mean µ = 15 and standard deviation σ = 10 values between 1 and 42: q = min(42 max(1 , , glyph[floorleft]| ˆ q |glyph[floorright] )) .

· TW: In order to sample TW which are feasible regarding the travel time required from the depot to the corresponding customer, we first define a sample horizon h i for each customer i

according to h i = [ a sample = ˆ h , b i sample = b 0 -ˆ ] h i where ˆ h i = glyph[ceilingleft] d 0 i glyph[ceilingright] +1 and d 0 i is the L2 distance from the depot to customer i . Then we sample the TW start time a i uniformly from h i . The end time b i is calculated as

$$b _ { i } = \min ( \lfloor a _ { i } + 3 0 0 \epsilon \rfloor, b _ { \text{sample} } )$$

where glyph[epsilon1] is a noise term sampled from a standard normal distribution according to glyph[epsilon1] = max ( | ˆ glyph[epsilon1] | , 1 / 100) , ˆ glyph[epsilon1] ∼ N (0 , 1)

Furthermore we use the same capacity Q 100 = 1000 . For smaller problem instances we set the respective capacities to Q 20 = 500 and Q 50 = 750 . All service durations are 10 time units. Our validation and test sets as well as the generator code will be made available with the published paper.

## G Additional details regarding the experiment setup

Problems The problem specific weights for the penalty terms in the objective function we set specific to our use cases to:

- · TW1: α = 1 0 . , β = ∞ ,
- · TW2: α = 0 0 . , β = 0 5 . ,
- · TW3: α = 0 1 . , β = 0 5 .

with linear penalty functions λ x ( ) = x .

JAMPR For our model we allow a number of pre-mature returns for vehicles which do not use their full capacity in order to start new intermediate tours by activating vehicles. They are limited by the hyper-parameter m pre which we set to 3 for the CVRP and to 6 for the CVRP-TW.

AM For the CVRP-TW we increase the hidden dimension of the decoder model from 128 to 256.

GORT Since GORT is only able to process integer valued data we scale all attributes of the standard problem instances by 100 and round to the next integer values. For that reason we also use the un-normalized original demands q and capacity Q . The created solutions consist of the node indices in the corresponding tours and therefore can be evaluated in the same way as the learned models. Furthermore the GORT GLS model requires the specification of a suitable time limit for the local search procedure. We run the GORT GLS baseline with a local search time limit of 8s per instance. If no feasible solution is found within this time limit, it is consecutively doubled until a feasible solutions is found.

## H Detailed experiment results

While working on the benchmark (see section I) we experimented with an additional constraint for the GORT baseline, restricting waiting time to be less than an upper limit ˆ δ k a i . After trying different limits, we found that a ˆ δ k a i of 10min works best. This improved the results on the benchmark w.r.t to our cost function by a large margin. Therefore we re-run the adapted GORT baseline (marked as GORT* ) for our large sampled test set as well. The results can be found in table 2. While the adapted constraint improves the results in some cases by balancing the objectives of minimizing total distance and waiting times, the general finding is still the same, that JAMPR significantly outperforms GORT on TW1 and TW3. This is because JAMPR is balancing the cost regarding distance as well as the total duration and incurred waiting time of the tours, while GORT generally rather finds solutions with a smaller total distance but much higher total duration and waiting times. Finally to provide further comparison we here also include a random baseline ( random (1000) ) which just samples 1000 sequentially created random solutions for each instance and selects the best one.

Table 2: Comparison of results for CVRP-TW variants. Best results for each problem are bold . Results of GORT* which are better than the results of the original model are underlined.

| Model              | N=20    | N=20   | N=20    | N=20   | N=50     | N=50   | N=50    | N=50   |
|--------------------|---------|--------|---------|--------|----------|--------|---------|--------|
| Model              | cost    | k      | dist    | t inf  | cost     | k      | dist    | t inf  |
| GORT - AU          | 2577.08 | 4.18   | 643.77  | 0.22s  | 4344.88  | 6.30   | 1110.68 | 1.76s  |
| GORT - GLS         | 2522.85 | 4.11   | 617.77  | 8.00s  | 4213.23  | 6.15   | 1090.36 | 8.06s  |
| GORT* - AU         | 2873.56 | 5.34   | 909.09  | -      | 4025.13  | 6.74   | 1520.56 | -      |
| GORT* - GLS        | 2478.70 | 4.77   | 809.75  | -      | 3675.83  | 6.22   | 1433.05 | -      |
| random (1000)      | 3036.39 | 5.68   | 1200.37 | -      | 7297.53  | 11.80  | 2914.86 | -      |
| AM +TW (greedy)    | 3766.88 | 5.29   | 1264.40 | 0.05s  | 7189.43  | 9.42   | 2882.53 | 0.12s  |
| AM +TW (sampl.)    | 3041.24 | 5.74   | 1202.64 | 0.12s  | 7327.09  | 11.92  | 2921.39 | 0.38s  |
| AM +TW ( t 10240 ) | 2750.06 | 5.27   | 1163.58 | 0.95s  | 6878.84  | 11.28  | 2865.30 | 3.08s  |
| JAMPR (greedy)     | 1862.40 | 2.25   | 966.74  | 0.10s  | 3055.94  | 5.42   | 1733.24 | 0.24s  |
| JAMPR (sampl.)     | 1716.60 | 2.29   | 965.42  | 0.86s  | 2691.55  | 4.03   | 1811.06 | 3.07s  |
| GORT - AU          | 635.06  | 4.23   | 635.06  | 0.37s  | 1123.82  | 6.72   | 1123.82 | 3.81s  |
| GORT - GLS         | 619.57  | 4.14   | 619.21  | 8.30s  | 1119.07  | 6.67   | 1118.01 | 8.09s  |
| GORT* - AU         | 855.66  | 4.94   | 855.65  | -      | 1498.25  | 6.35   | 1498.25 | -      |
| GORT* - GLS        | 809.01  | 4.76   | 809.01  | -      | 1462.33  | 6.27   | 1462.32 | -      |
| random (1000)      | 1646.83 | 6.13   | 1202.66 | -      | 8368.49  | 8.65   | 2897.99 | -      |
| AM +TW (greedy)    | 7615.69 | 2.00   | 1094.39 | 0.05s  | 40245.40 | 2.00   | 2687.95 | 0.12s  |
| AM +TW (sampl.)    | 1572.31 | 6.56   | 1221.09 | 0.11s  | 7712.35  | 9.34   | 2953.65 | 0.34s  |
| AM +TW ( t 10240 ) | 1387.30 | 6.46   | 1170.49 | 0.90s  | 6730.55  | 9.99   | 2954.76 | 2.70s  |
| JAMPR (greedy)     | 674.72  | 4.32   | 626.80  | 0.11s  | 1273.20  | 6.02   | 1126.74 | 0.25s  |
| JAMPR (sampl.)     | 620.68  | 4.19   | 602.33  | 0.92s  | 1116.76  | 5.64   | 1076.79 | 2.32s  |
| GORT - AU          | 1317.81 | 4.07   | 637.15  | 1.07s  | 2707.72  | 6.12   | 1121.57 | 20.63s |
| GORT - GLS         | 1312.71 | 4.11   | 625.80  | 8.00s  | 2753.66  | 6.18   | 1192.61 | 8.02s  |
| GORT* - AU         | 1428.53 | 4.74   | 849.69  | -      | 2641.83  | 6.18   | 1480.75 | -      |
| GORT* - GLS        | 1388.88 | 4.74   | 810.66  | -      | 2671.93  | 6.28   | 1494.14 | -      |
| random (1000)      | 1409.35 | 3.34   | 953.48  | -      | 4407.58  | 6.92   | 2692.78 | -      |
| AM +TW (greedy)    | 3101.21 | 2.00   | 1094.39 | 0.05s  | 26467.34 | 2.00   | 2687.95 | 0.12s  |
| AM +TW (sampl.)    | 1412.16 | 3.42   | 951.92  | 0.12s  | 4161.24  | 7.15   | 2674.03 | 0.34s  |
| AM +TW ( t 10240 ) | 1318.56 | 3.23   | 899.83  | 0.88s  | 3941.47  | 6.77   | 2575.28 | 2.67s  |
| JAMPR (greedy)     | 1002.81 | 1.00   | 733.01  | 0.10s  | 3158.26  | 2.01   | 1347.72 | 0.23s  |
| JAMPR (sampl.)     | 844.35  | 1.39   | 660.48  | 0.78s  | 1947.65  | 2.29   | 1358.29 | 2.13s  |

## I Solomon CVRP-TW benchmark

In order to compare the performance of our models on some more established work, we evaluate GORT, JAMPR and AM +TW on two of the available location variants of the Solomon benchmark data set [26] which include random (R2) and partially random and clustered (RC2) instances of size 50 and 100 with hard TW.

In the detailed comparison for R201 and RC201 (table 3) the N=50 instance includes the depot and the first 50 nodes of the standard N=100 instance. For the aggregated results (table 4) we split the N=100 instances into two instances with 50 nodes, each including the depot and the first and the second half of the nodes respectively.

For the learned models a notable distinction has to be made for the benchmark set for N = 100 . Here we use models which were trained only on the described random instances of size 50 (similar to R201, see section F). For problem size 100 we evaluate the models which were trained on N = 50 to investigate the generalization capability similar to the approach in [30]. Furthermore we test our models on partially clustered data (RC2) which was no explicit part of the training distribution. Additionally the other R2 and RC2 instances vary also in the width of the TW and the number of customers which are constrained by TW (cp. section F) compared to the training instances.

Again we also include the adapted GORT baseline (explained in section H) with a waiting time constraint of ˆ δ k a i equal to 10min ( GORT* ). Furthermore we give the optimal results ( optimal* ) reported on the benchmark website 1 and the best known heuristic solution 2 ( BKH ). For the aggregated instances we also include the results of Silva et al. [24] ( ALS-Q ). Finally we run a random baseline ( random (1000) ) which just samples 1000 sequentially created random solutions for the instance and selects the best one.

The results are shown in more detail for the instances R201 and RC201 in table 3. Aggregated results for all R2 and RC2 instances respectively are shown in table 4. In general we see that JAMPR is able to sufficiently generalize to instances which are twice the size of the training data set. This can be seen in comparison to the GORT baseline which is run independently on the larger instances. A similar generalization capability is apparent for the results on the partially clustered data (RC2) and different settings for the TW.

The comparison to other related work is more difficult, since the respective models are normally trained and evaluated on a different objective. Especially the operations research literature focuses on minimizing the number of vehicles as main and the total distance as secondary objective:

$$\text{cost} & = \alpha K + \sum _ { k = 1 } ^ { K } \left ( \sum _ { ( i, j ) \in s _ { k } \Leftrightarrow 0 } c _ { i j } \right )$$

where α is an arbitrary large non-negative penalty factor for the number of vehicles K and c ij is the distance betwenn locations i and j .

In contrast we took a more holistic perspective also including waiting time (as well as soft TW) into the objective. In this regard we can conclude, that JAMPR finds a good trade-off between transit cost (total distance), waiting times and number of vehicles while taking considerably less inference time than standard heuristic approaches. In comparison the sampling behavior of AM +TW is close to random, which means that the model is not able to extract significant features and learn important relations. Greedy inference is better, but still significantly worse than GORT or JAMPR. Further analysis is provided in section J.

1 http://web.cba.neu.edu/~msolomon/problems.htm

2 https://www.sintef.no/projectweb/top/vrptw/solomon-benchmark/100-customers/

Table 3: Detailed results for the R201 and RC201 instance of the Solomon benchmark.

|       | Model              | N=50    | N=50   | N=50      | N=50   | N=100   | N=100   | N=100   | N=100   |
|-------|--------------------|---------|--------|-----------|--------|---------|---------|---------|---------|
|       |                    | cost    | k      | dist      | t inf  | cost    | k       | dist    | t inf   |
|       | optimal*           | -       | 6.00   | 791.90    | -      | -       | 8.00    | 1143.20 | -       |
|       | BKH                | -       | -      | -         | -      | -       | 4.00    | 1252.37 | -       |
|       | random (1000)      | 4060.06 | 10.00  | 2096.71   | -      | 7953.15 | 16.00   | 3423.72 | -       |
|       | GORT - AU          | 3807.16 | 7.00   | 822.99    | 0.78 s | 5570.30 | 9.00    | 1235.92 | 3.29s   |
|       | GORT - GLS         | 3696.14 | 7.00   | 804.77    | 8.00 s | 5432.42 | 9.00    | 1223.02 | 8.01s   |
|       | GORT* - AU         | 1722.53 | 5.00   | 969.36    | 1.12 s | 2785.88 | 7.00    | 1503.86 | 3.80s   |
|       | GORT* - GLS        | 1722.53 | 5.00   | 969.36    | 8.00 s | 3288.16 | 8.00    | 1455.68 | 8.00s   |
|       | AM +TW (greedy)    | 4468.44 | 8.00   | 1781.85   | 0.21 s | 6604.67 | 12.00   | 3121.32 | 0.31s   |
|       | AM +TW (sampl.)    | 4694.72 | 11.00  | 1877.49   | 0.49 s | 8924.56 | 20.00   | 3600.92 | 1.20s   |
|       | AM +TW ( t 10240 ) | 4461.07 | 11.00  | 2050.11   | 3.10 s | 8497.29 | 18.00   | 3327.01 | 8.63s   |
|       | JAMPR (greedy)     | 2279.10 | 4.00   | 1591.65   | 0.34 s | 3436.51 | 6.00    | 2396.45 | 0.59s   |
|       | JAMPR (sampl.)     | 1791.92 | 3.00   | 1289.35   | 3.08 s | 2682.39 | 5.00    | 2230.59 | 8.98s   |
|       | optimal*           | -       | 5.00   | 684.80    | -      | -       | 9.00    | 1261.80 | -       |
|       | BKH                | -       | -      |           | -      | -       | 4.00    | 1406.94 | -       |
|       | random (1000)      | 5012.32 | 13.00  | - 2579.74 | -      | 9417.33 | 23.00   | 4562.85 | -       |
|       | GORT - AU          | 3249.92 | 6.00   | 688.34    | 0.71 s | 5673.33 | 9.00    | 1443.32 | 3.85s   |
|       | GORT - GLS         | 3249.92 | 6.00   | 688.34    | 8.00 s | 6046.57 | 10.00   | 1426.65 | 8.00s   |
| RC201 | GORT* - AU         | 2968.59 | 8.00   | 1211.88   | 0.51 s | 3188.90 | 7.00    | 1594.82 | 4.67s   |
| RC201 | GORT* - GLS        | 2649.55 | 7.00   | 1142.04   | 8.00 s | 3188.90 | 7.00    | 1594.82 | 8.00s   |
| RC201 | AM +TW (greedy)    | 4544.19 | 9.00   | 1947.72   | 0.22 s | 7562.65 | 15.00   | 3999.01 | 0.32s   |
| RC201 | AM +TW (sampl.)    | 5039.80 | 12.00  | 2900.55   | 0.50 s | 9150.92 | 20.00   | 4765.43 | 1.21s   |
| RC201 | AM +TW ( t 10240 ) | 5000.00 | 13.00  | 2655.76   | 3.13 s | 8633.75 | 18.00   | 4647.16 | 8.72s   |
| RC201 | JAMPR (greedy)     | 1871.79 | 3.00   | 1501.17   | 0.34 s | 3711.35 | 6.00    | 3053.83 | 0.58s   |
| RC201 | JAMPR (sampl.)     | 1819.27 | 4.00   | 1250.00   | 3.15 s | 2904.33 | 6.00    | 2523.61 | 8.74s   |

Table 4: Aggregated average results over all R2 and RC2 instances of the Solomon benchmark

| Model              | N=50    | N=50   | N=50    | N=50   | N=100   | N=100   | N=100   | N=100   |
|--------------------|---------|--------|---------|--------|---------|---------|---------|---------|
| Model              | cost    | k      | dist    | t inf  | cost    | k       | dist    | t inf   |
| BKH                | -       | -      | -       | -      | -       | 2.73    | 951.03  | -       |
| ALS-Q              | -       | -      | -       | -      | -       | 3.00    | 956.61  | -       |
| random (1000)      | 3235.21 | 7.00   | 1917.63 | -      | 5855.62 | 14.00   | 3468.93 | -       |
| GORT - AU          | 2920.98 | 5.82   | 633.09  | 0.95 s | 4519.85 | 8.27    | 987.36  | 5.41s   |
| GORT - GLS         | 2763.58 | 5.77   | 610.45  | 8.00 s | 4468.01 | 8.18    | 977.93  | 8.00s   |
| GORT* - AU         | 2284.14 | 5.55   | 731.05  | 0.99 s | 2428.52 | 6.09    | 1091.08 | 5.78s   |
| GORT* - GLS        | 2231.35 | 5.50   | 704.01  | 8.00 s | 2476.82 | 6.18    | 1082.11 | 8.00s   |
| AM +TW (greedy)    | 3451.73 | 5.64   | 1556.63 | 0.11 s | 6149.99 | 9.36    | 3312.46 | 0.22s   |
| AM +TW (sampl.)    | 3287.95 | 7.82   | 1737.51 | 0.35 s | 6407.59 | 12.73   | 3448.46 | 1.00s   |
| AM +TW ( t 10240 ) | 2997.14 | 7.59   | 1700.02 | 2.70 s | 6009.37 | 12.27   | 3377.40 | 7.75s   |
| JAMPR (greedy)     | 1694.49 | 2.95   | 1106.25 | 0.24 s | 2536.46 | 4.27    | 1959.64 | 0.49s   |
| JAMPR (sampl.)     | 1566.42 | 3.59   | 1179.40 | 3.04 s | 2401.86 | 5.00    | 2068.57 | 8.85s   |
| BKH                | -       | -      | -       | -      | -       | 3.25    | 1119.24 | -       |
| ALS-Q              | -       | -      | -       | -      | -       | 3.38    | 1164.61 | -       |
| random (1000)      | 4833.41 | 12.00  | 2691.28 | -      | 8696.28 | 19.00   | 4800.89 | -       |
| GORT - AU          | 2794.08 | 5.56   | 618.30  | 0.89 s | 4477.10 | 8.38    | 1127.18 | 4.83s   |
| GORT - GLS         | 2756.02 | 5.56   | 602.51  | 8.00 s | 4535.50 | 8.50    | 1123.41 | 8.00s   |
| GORT* - AU         | 2244.97 | 5.63   | 837.89  | 0.87 s | 2981.47 | 6.88    | 1278.96 | 5.35s   |
| GORT* - GLS        | 2240.78 | 5.75   | 791.05  | 8.00 s | 2971.08 | 6.88    | 1274.59 | 8.00s   |
| AM +TW (greedy)    | 3865.95 | 6.38   | 1754.63 | 0.11 s | 7015.59 | 11.38   | 3877.71 | 0.23s   |
| AM +TW (sampl.)    | 3914.12 | 8.94   | 2175.67 | 0.36 s | 7632.03 | 15.25   | 4372.87 | 1.03s   |
| AM +TW ( t 10240 ) | 3601.40 | 8.69   | 2102.26 | 2.75 s | 7363.07 | 15.00   | 4356.63 | 8.03s   |
| JAMPR (greedy)     | 1766.32 | 3.06   | 1174.98 | 0.25 s | 3101.57 | 5.00    | 2484.69 | 0.50s   |
| JAMPR (sampl.)     | 1645.00 | 3.25   | 1207.42 | 3.05 s | 2774.64 | 5.00    | 2401.82 | 9.01s   |

## J Example solution plots

Weplot the solutions to the R201 and RC201 instances for both problem sizes ( N = 50 and N = 100 ) always for the best variant of each of the three compared methods GORT, AM +TW and JAMPR. The letters in the legend describe for each tour the number of customer (n), the length (l) i.e. total distance, the used capacity (q) and the total duration (t). Plots are best viewed in color. We want to remind the reader that these instances are similar to the TW1 variant with hard time windows and therefore tours are not only dependent on the geographical proximity, leading to tours which are visually less clearly separated.

Figure 2: GORT solution for R201-50 (left) and R201-100 (right).

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000001_e90d9aea3a7ee5c5705ae61f619015dcc4cbaebc5c4f87210bd9ff07db4f9435.png)

Figure 3: JAMPR solution for R201-50 (left) and R201-100 (right).

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000002_9e0b8b443cf9dc3b6ee3ee512960b645e3b453a47f50bdc7db6958675863d218.png)

As we can see from the solution plots, the tours of GORT are based more on geographical proximity, leading to tours with smaller total distance but a higher number of used vehicles with less used capacity and much more waiting times leading to longer tour duration. In contrast the tours created by JAMPR focus less on close neighbors and more on most efficiently using the available time and capacity, leading to tours with larger total distance but a smaller number of used vehicles with a tighter schedule. This generally leads to a shorter total duration. Finally AM +TW does not seem able to focus on any part of the objective and constraint space with close to random solutions. This can be

Figure 4: AM +TW solution for R201-50 (left) and R201-100 (right).

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000003_89f8c643901a9570384c690decb0cfd59f23e14e0cf0b5c634c1e7daa723b5e6.png)

Figure 5: GORT solution for RC201-50 (left) and RC201-100 (right).

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000004_b195a07e7b731bd75ec9be4fb50ab974fa8b247cbb1a5f3b210f43a63b130521.png)

seen especially in figure 7 for the partially clustered locations, where on the N = 50 instance e.g. tour R04 erratically visits customers in all clusters.

Figure 6: JAMPR solution for RC201-50 (left) and RC201-100 (right).

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000005_905557d5c504b5e43560f91fd8b0284a46de746b5c734db87e676bf2fc6d2d3c.png)

Figure 7: AM +TW solution for RC201-50 (left) and RC201-100 (right).

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000006_91fde0338ae3bd4d3313bfe03bd1aced029185a116a2d654fcead998a037661d.png)

## K Effect of max concurrency

Finally we show some learning curves for the CVRP-TW variant TW1 with hard time windows and the standard CVRP for different values of maximum concurrency controlled by m con (mc). For the less constrained CVRP a smaller number of m con = 1 (N=20, figure 10) or m con = 2 (N=50, figure 11) works best. In contrast the much more constrained CVRP-TW1 benefits from a higher m con . However, since most of the problem instances are solved by JAMPR with two to four vehicles on average, a larger m con does not have any additional advantage, but rather hurts performance by unnecessarily increasing learning complexity.

Figure 8: Learning curves on random sampled training data for the CVRP-TW1 of size N = 20 .

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000007_55073f5d43bd41d3bd05641c5cb07e2ce63beebe77b9bbedb3adb62fdb45b6b9.png)

Figure 9: Learning curves on random sampled training data for the CVRP-TW1 of size N = 50 .

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000008_d5c92d1006c2d9583c1ca242cd4c7fde74da4c3be1aa0ed55dcc6ce0582ccc2d.png)

Figure 10: Learning curves on random sampled training data for the CVRP of size N = 20 .

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000009_6f1c4f7ba4a1b5aed7d3178c3beeb6303b275939265928c6078a9fb77bb32634.png)

Figure 11: Learning curves on random sampled training data for the CVRP of size N = 50 .

![Image](Papers_Converted/0_Artifacts/[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention_artifacts/image_000010_08257be020a5030633ae6463f79b8c58881df8300431d5f02499e211e2442507.png)