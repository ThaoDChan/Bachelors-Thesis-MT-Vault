## Learning to Iteratively Solve Routing Problems with Dual-Aspect Collaborative Transformer

Yining Ma , Jingwen Li , Zhiguang Cao 1 1 2 , ∗ , Wen Song 3 , ∗ , Le Zhang , 4 Zhenghua Chen , Jing Tang 5 6

1

National University of Singapore 2 Singapore Institute of Manufacturing Technology, A*STAR 3 Institute of Marine Science and Technology, Shandong University 4 University of Electronic Science and Technology of China 5 Institute for Infocomm Research, A*STAR 6 The Hong Kong University of Science and Technology {yiningma, lijingwen}@u.nus.edu, zhiguangcao@outlook.com, wensong@email.sdu.edu.cn, zhangleuestc@gmail.com, chen0832@e.ntu.edu.sg, jingtang@ust.hk

## Abstract

Recently, Transformer has become a prevailing deep architecture for solving vehicle routing problems (VRPs). However, it is less effective in learning improvement models for VRP because its positional encoding (PE) method is not suitable in representing VRP solutions. This paper presents a novel Dual-Aspect Collaborative Transformer (DACT) to learn embeddings for the node and positional features separately, instead of fusing them together as done in existing ones, so as to avoid potential noises and incompatible correlations. Moreover, the positional features are embedded through a novel cyclic positional encoding (CPE) method to allow Transformer to effectively capture the circularity and symmetry of VRP solutions (i.e., cyclic sequences). We train DACT using Proximal Policy Optimization and design a curriculum learning strategy for better sample efficiency. We apply DACT to solve the traveling salesman problem (TSP) and capacitated vehicle routing problem (CVRP). Results show that our DACT outperforms existing Transformer based improvement models, and exhibits much better generalization performance across different problem sizes on synthetic and benchmark instances, respectively.

## 1 Introduction

Vehicle Routing problems (VRPs), such as the Traveling Salesman Problem (TSP) and the Capacitated Vehicle Routing Problem (CVRP) which consider finding the optimal route for a single or fleet of vehicles to serve a set of customers, have ubiquitous real-world applications [1, 2]. Despite being intensively studied in the Operations Research (OR) community, VRPs still remain challenging due to their NP-hard nature [3]. Recent studies on learning neural heuristics are gathering attention as promising extensions to traditional hand-crafted ones (e.g., [4-14]), where reinforcement learning (RL) [15] is usually exploited to train a deep neural network as an efficient solver without hand-crafted rules. A salient motivation is that deep neural networks may learn better heuristics by identifying useful patterns in an end-to-end and data-driven fashion.

Solutions to VRPs, i.e., routes, are sequences of nodes (customer and depot locations). Naturally, deep models for Natural Language Processing (NLP), which deal with sequence data as well, are ideal

∗ Zhiguang Cao and Wen Song are the corresponding authors.

Figure 1: Transformer frameworks for VRPs. (a) Wu et al. [11] (the original one); (b) DACT (ours).

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000000_b07b888f269019226fb13ebe01b375b79fdb90ae411573d7eaf3b992b9e064e4.png)

choices for encoding VRP solutions. Given its remarkable performance in NLP tasks, Transformer [16] is standing at the forefront in the learning based methods for VRPs (e.g., [5, 7, 8, 11-13, 17]). The original Transformer encodes a sentence, i.e., a sequence of words, into a unified set of embeddings by injecting word positional information into its word embeddings through positional encoding (PE). When it comes to VRPs, while is not required in construction models, positional information is critical for deep models that learn improvement heuristics since the input are solutions to be improved.

Although some success has been achieved, learning improvement heuristics for VRPs based on the original Transformer encoder is yet lacking from our perspective. Firstly, directly applying addition operation on PE vectors and the embeddings in absolute PE method (i.e., Figure 1(a)) could limit the representation of the model [18], as the mixed correlations 2 existing in the self-attention can bring unreasonable noises and random biases to the encoder (details in Appendix A). Secondly, existing PE methods tend to fuse the node and positional information into one unified representation. NLP tasks such as translation may benefit from this owing to the deterministic and instructive nature of the positional information. However, such design may not be optimal for routing tasks because the positional information therein can be non-deterministic and sometimes even random. This may cause disharmony or disturbance in the encoder and may thus deteriorate the performance. Finally, most VRPs seek the shortest loop of the nodes, making their solutions to be cyclic sequences. However, existing PE methods are only designated to encode linear sequences , which may fail to identify such 3 circular input. As will be shown in our experiments, this could severely damage the generalization performance, since the cyclic feature of VRP solutions is not correctly reflected by the encoder.

In this paper, we address the above issues and contribute to the line of using RL to learn neural improvement heuristics for VRPs. We introduce the Dual-Aspect Collaborative Transformer (DACT) , where we revisit the solution representations and propose to learn separated groups of embeddings for the node and positional features of a VRP solution as shown in Figure 1(b). Our DACT follows the encoder-decoder structure. In the encoder, each set of embeddings encodes the solution mainly from its own aspect, and at the same time exploits a cross-aspect referential attention mechanism for better perceiving the consistence and differentiation with respect to the other aspect. The decoder then collects action distribution proposals from the two aspects and synthesizes them to output the final one. Meanwhile, we design a novel cyclic positional encoding (CPE) method to capture the circularity and symmetry of VRP solutions, which allows Transformer to encode cyclic inputs, and also boost the generalization performance for solving VRPs. As the last contribution, we design a simple yet effective curriculum learning strategy to improve the sample efficiency. This further leads to faster and more stable convergence of RL training. Extensive experiments show that our DACT can outperform existing Transformer based improvement models with fewer parameters, and also generalizes well across different sizes of synthetic and benchmark instances, respectively.

2

The term correlation refers to the dot product between Query and Key in the self-attention module. The term mixed correlation refers to the case where Query and Key are projected from different types of embeddings. 3 The relative PE method seems to help, however, it is found to be even worse than the absolute PE method for VRPs in Wu et al. [11], partly due to the disharmony issue caused by learning the unified representation.

## 2 Related work

## 2.1 Positional encoding (PE) in Transformer.

The original Transformer adopted the absolute PE method to describe the absolute position of elements in the sequence [16], especially for NLP. As formulated in Eq. (1), each generated positional embedding p i ∈ R d is added together with the i -th word embedding x i in the first layer of the encoder,

$$\alpha _ { i, j } ^ { \text{Abs} } = \frac { 1 } { \sqrt { d } } ( ( x _ { i } + p _ { i } ) W ^ { Q } ) ( ( x _ { j } + p _ { j } ) W ^ { K } ) ^ { T }. \quad \quad \quad ( 1 )$$

The relative PE method was further proposed in Shaw et al. [19] to better capture the relative order information. On the basis of absolute PE, it introduces an inductive bias to the attention as follows,

$$\alpha _ { i, j } ^ { \text{Rel} } = \frac { 1 } { \sqrt { d } } ( ( x _ { i } + p _ { i } ) W ^ { Q } ) ( ( x _ { j } + p _ { j } ) W ^ { K } + a _ { j - i } ) ^ { T },$$

where a j -i ∈ R d is learnable parameters for encoding the relative position j -i . To avoid the mixed and noisy correlations between word semantics and positional information in the above two PEs, the Transformer with United Positional Encoding (TUPE) [18] was proposed for NLP which utilizes separated projection metrics W x and W p for each information as follows,

$$\alpha _ { i, j } ^ { T U P E } = \frac { 1 } { \sqrt { 2 d } } ( x _ { i } W _ { x } ^ { Q } ) ( x _ { j } W _ { x } ^ { K } ) ^ { T } + \frac { 1 } { \sqrt { 2 d } } ( p _ { i } W _ { p } ^ { Q } ) ( p _ { j } W _ { p } ^ { K } ) ^ { T } + b _ { j - i }.$$

However, as mentioned previously, existing PE methods are less effective for VRPs since they simply fuse the node and positional information into one unified set of embeddings during or after the calculation of the attention correlation α i,j . Meanwhile, they are also unable to properly encode and handle cyclic input sequences as in VRP solutions.

## 2.2 Deep models for VRP.

Various deep architectures such as Recurrent Neural Network (RNN), Graph Neural Network (GNN), and Transformer have been employed in solving VRPs.

RNN based models. As the pioneering work of neural VRP solvers, Pointer Network adopted RNN and supervised learning to solve TSP [20] (extended to RL in Bello et al. [21] and CVRP in Nazari et al. [22]). While the models in [20-23] learn construction heuristics, NeuRewriter [4] learns improvement heuristic for CVRP using LSTM to encode the positional information of a solution. In Hottung et al. [13], the conditional variational autoencoder was adopted to learn a continuous and latent search space for VRP, where high-quality solutions were taken as input and encoded by RNNs. However, recurrence structures in RNN are less efficient in both representation and computation [5].

GNN based models. In Dai et al. [24], GNN was combined with Q-learning for solving TSP. Based on supervised learning, Joshi et al. [6] used GNN to learn heatmaps that prescribe the probability of each edge appearing in the optimal TSP tour. This idea was extended in Fu et al. [25] with additional components such as graph sampling and heatmap merging to enable generalization to larger TSP instances. These models often require post-processing to construct feasible solutions from heatmaps (e.g., beam search [6], Monte-Carlo tree search [25], and dynamic programming [26]).

Transformer based models. The Attention Model (AM) by Kool et al. [5] was recognized as the first success of Transformer based models for VRPs. Based on AM, Xin et al. [7] proposed a MultiDecoder AM that learns multiple diverse policies for better performance. In Kwon et al. [8], the RL algorithm of AM was improved which leaded to a new solver, i.e., POMO (Policy Optimization with Multiple Optima), and achieved the state-of-the-art performance. However, POMO is still lacking in generalization. Besides these construction models, Transformer was also explored to learn improvement heuristics. Hottung and Tierney [27] learned first neural large neighborhood search algorithm for VRPs. Lu et al. [12] proposed the L2I model that learns to select local search operators from a pool of traditional ones. Both methods used a Transformer-style encoder, but the positional information is captured in the node features (information of previous and next nodes) instead of using PE methods. Though L2I was shown to outperform LKH3 [28], it is limited to CVRP and the required time is considerably long. Wu et al. [11] proposed a Transformer model which learns to pick node pair in each step to perform a pairwise local operator (e.g., 2-opt). However, it suffers from the inaccurate representation of positional information given the original Transformer encoder.

Figure 3: Architecture of our policy network, dual-aspect collaborative Transformer (DACT).

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000001_309eb357fc25f743b54b3f8716ac4e9a9627f86fffd4bb6a4db94620951f4c44.png)

## 3 Problem formulation

We define a VRP instance as a group of N nodes to visit, where the node feature x i of node i contains 2-dim coordinates and other problem-specific features (e.g., customer demand). A solution δ consists of a sequence of nodes visited in order where we denote p i to be the position (indices) of node i in the solution which is deemed as the positional feature of node i . The objective is to minimize the total travel distance D δ ( ) under certain problem-specific constraints.

Starting with an initial yet complete solution, our neural RL policy tries to improve the solution iteratively. At each step, the policy automatically selects a pair of nodes and locally adjusts the solution using a preset pairwise operator such as 2-opt , insert , or swap . As illustrated in Figure 2, given a node pair ( i, j ), the 2-opt operator adjusts a solution by reversing the segment between node i and node j ; the insert operator adjusts a solution by placing node i after node j ; and the swap operator

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000002_30b533ff6620e3ecc313660bfd1b475ea81888156c371be00cd536405a28baab.png)

Figure 2: Illustration examples of three pairwise operators for routing problems when node pair ( i = 2 , j = 1 ) is specified for operating. From left to right: 2-opt , insert , and swap .

adjusts a solution by exchanging the position of node i and node j . Such operation is repeated until reaching the step limit T and we model it in the form of Markov Decision Process (MDP) as follows.

State. For an instance with N nodes, a state describes current solution δ t using its node and positional features of each node, i.e., s t = Ψ( δ t ) = { x , ..., x t 1 t N , p t 1 , ..., p t N } .

Action. The action a t = ( i, j ) specifies a node pair ( i,j ) for the pairwise operator.

Reward. The reward function is defined as, r t = D δ ( * t ) -min D δ [ ( t +1 ) , D δ ( * t ) ] where δ * t is the best incumbent solution found until time t . It refers to the immediate reduced cost at each step with respects to the best incumbent solution, which ensures the cumulative reward equal to the total reduced cost over the initial solution. Hence the reward r t &gt; 0 if and only if a better solution is found.

Policy. The policy π θ is parameterized by the proposed DACT model with parameters θ . At each time step, the action ( i, j ) is obtained by sampling the stochastic policy for both training and inference.

Transition. The next state s t +1 is originated from s t by performing the preset pairwise operator on the given node pair (action). Our state transient is deterministic, in the sense that it always accepts the next solution as the next state (infeasible solutions will be masked), regardless of its objective value. With such simple rule, the RL agent is expected to automatically learn how to combine multiple steps of simple local movements to achieve better solutions, even if some of them may worsen the current solution. Note that the step limit T can be any user-specified value according to the allowed time budget. Hence, our MDP can have infinite horizon and we consider the reward discount factor γ&lt; 1 . Besides, we stipulate that if T r (a hyper-parameter) subsequent steps do not yield a better solution, we will reinitialize the search by assigning the best solution found thus far as the next solution to move on.

Figure 5: Comparison of our CPE method with absolute PE method on a TSP instance with 20 nodes. (a) the embedding vectors, (b) the correlations (dot products) between every two embeddings, and (c) the top two principal components after PCA (principal component analysis) projection.

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000003_fdf46fb6623020468b160040d9cace4b04e042c827189e0a0e1fd6fbab4f7125.png)

## 4 Dual-aspect collaborative Transformer model

We now present the details of our Dual-Aspect Collaborative Transformer (DACT). The concrete architecture of DACT is presented in Figure 3, where we take the TSP with N nodes as an illustration example. Our DACT leverages separate aspects of embeddings to encode a VRP solution. In the DAC encoder , the self-attention correlations are computed individually for each aspect, and a cross-aspect referential attention mechanism is proposed to enable one aspect to effectively exploit attention correlations from the other aspect as optional references. The DAC decoder then collects action distribution proposals from both aspects and synthesize them to the final one.

## 4.1 Dual-aspect solution representation

Specifically, we propose to learn two sets of embeddings, i.e., the node feature embeddings (NFEs) for node representation and the positional feature embeddings (PFEs) for positional representation.

NFEs. Following [5, 11], the NFE h i of node i is initialized as the linear projection of its node feature x i with output dimension 4 dim = 64 .

PFEs. The PFE g i of the positional feature p i is initialized as a real-valued vector ( dim = 64 ) by applying our cyclic positional encoding (CPE), which is designed based on cyclic Gray codes [29].

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000004_ecd6127ddf462e6a5bab0766885f2092c262139a1a7d86c1dc553792935815ec.png)

As illustrated in Figure 4, the cyclic Gray codes present a cyclic property ('1110' in the last column is adjacent to '1111' in the first column) and an adjacency similarity property (any codes in adjacent

Figure 4: An example of cyclic Gray code where 4 digits are used to encode N =16 nodes. The top left shows the base symmetry pattern '1001' in Gray code, and the top right plots its representation in our method.

columns only differ in one digit), both of which are desirable for cyclic sequences. To preserve these properties in designing our CPE, we follow two observed patterns: 1) each numerical digit contains a periodic cycle with reflectional symmetry, e.g., the '10 01' in the lowest digit; and 2) the | higher the numerical digit, the longer the period. Accordingly, we create similar patterns based on the sinusoidal functions in Eq. (4), where a periodic function with period 4 π ω d (induced by modulus) is used to generate one base symmetry pattern (the top right in Figure 4),

$$\overrightarrow { g } _ { i } ^ { \prime } ( d ) \coloneqq \begin{cases} s i n ( \omega _ { d } \cdot \left | ( z ( i ) \bmod \frac { 4 \pi } { \omega _ { d } } ) - \frac { 2 \pi } { \omega _ { d } } \right | ), \, \text{if $d$ is even} \\ c o s ( \omega _ { d } \cdot \left | ( z ( i ) \bmod \frac { 4 \pi } { \omega _ { d } } ) - \frac { 2 \pi } { \omega _ { d } } \right | ), \, \text{if $d$ is odd} \end{cases}$$

where z i ( ) = i N ω 2 π d ⌈ N 2 π/ω d ⌉ is to make N nodes linearly spaced in the generated pattern; the angular frequency ω d is decreasing along the dimension to make the wavelength longer within the range [ N 1 glyph[floorleft] dim/ 2 glyph[floorright] , N ] (see Appendix B for details). In Figure 5, we visualize the comparison between the absolute PE and our CPE for encoding a TSP instance of 20 nodes. Figure 5(a) demonstrates that our real-valued base symmetry pattern has a longer cyclic period as the digit grows. Figure 5(b) indicates that our method (blue) is able to correctly reflect the adjacency between the head and tail of the cyclic

4 Different from Wu et al. [11], we reduce the dimension of the embeddings from 128 to 64.

sequence whereas the PE method (red) fails to do so. Figure 5(c) verifies that our CPE vectors are well distributed in space with desired cyclic and adjacency similarity properties.

## 4.2 The encoder

The encoder consists of L =3 stacked DAC encoders. In each DAC encoder, we retain relatively independent encoding stream for NFEs and PFEs as in Eq. (5) and Eq. (6), respectively, each of which consists of a shared Dual-Aspect Collaborative Attention ( DAC-Att ) sub-layer and an independent feed-forward network ( FFN ) sub-layer. DAC-Att takes both sets of embeddings as input and then outputs their respective enhanced embeddings, i.e., NFEs { ˜ h } N i =1 and PFEs { ˜ g } N i =1 . Each sub-layer is followed by skip connection [30] and layer normalization [31] as same as the original Transformer.

$$h _ { i } ^ { ( l ) } = \mathrm L N \left ( h _ { i } ^ { \prime } + \mathrm F N _ { h } ^ { ( l ) } ( h _ { i } ^ { \prime } ) \right ), h _ { i } ^ { \prime } = \mathrm L N \left ( h _ { i } ^ { ( l - 1 ) } + \tilde { h } _ { i } ^ { ( l ) } \right ),$$

$$g _ { i } ^ { ( l ) } = & \mathrm L \mathrm N \left ( g _ { i } ^ { \prime } + \mathrm F \mathrm N _ { g } ^ { ( l ) } ( g _ { i } ^ { \prime } ) \right ) \,, g _ { i } ^ { \prime } = \mathrm L \mathrm N \left ( g _ { i } ^ { ( l - 1 ) } + \tilde { g } _ { i } ^ { ( l ) } \right ) \,.$$

DAC-Att. The DAC-Att sub-layer enhances each set of embedding from its own aspect, while leveraging attention correlations from the other aspect to achieve the synergy. Given the two sets of embeddings 5 , { h i } N i =1 and { g i } N i =1 , we first compute the self-attention correlation from both aspects,

$$\alpha _ { i, j } ^ { h } = \frac { 1 } { \sqrt { d _ { k } } } \left ( h _ { i } W _ { h } ^ { Q } \right ) \left ( h _ { j } W _ { h } ^ { K } \right ) ^ { T }, \quad \alpha _ { i, j } ^ { g } = \frac { 1 } { \sqrt { d _ { k } } } \left ( g _ { i } W _ { g } ^ { Q } \right ) \left ( g _ { j } W _ { g } ^ { K } \right ) ^ { T },$$

where independent matrices W ,W Q h K h , W Q g and W K g ∈ R dim × d k are used to calculate queries and keys . The obtained correlations are further normalized to ˜ α h i,j and ˜ α g i,j via Softmax. Note that the correlations are computed from their own aspect, which eliminates possible noises and conduces to correctly describe the incompatible node pair relationships in different aspects of VRP solutions.

We then exploit a cross-aspect referential attention mechanism, which allows computed correlations to be shared between each other, as additional references for both contradistinction and collaboration,

$$\text{out} _ { i } ^ { h } = & \text{Concat} \left [ \sum _ { j = 1 } ^ { N } \tilde { \alpha } _ { i, j } ^ { h } \left ( h _ { j } W _ { h } ^ { V } \right ), \sum _ { j = 1 } ^ { N } \tilde { \alpha } _ { i, j } ^ { g } \left ( h _ { j } W _ { h } ^ { V r e f } \right ) \right ],$$

$$\text{out} _ { i } ^ { g } = & \text{Concat} \left [ \sum _ { j = 1 } ^ { N } \tilde { \alpha } _ { i, j } ^ { g } \left ( g _ { j } W _ { g } ^ { V } \right ), \sum _ { j = 1 } ^ { N } \tilde { \alpha } _ { i, j } ^ { h } \left ( g _ { j } W _ { g } ^ { V r e f } \right ) \right ],$$

where W ,W V h V g ∈ R dim × d v are trainable parameter matrices for formulating values in each aspect; and W Vref h , W Vref g ∈ R dim × d v are parameter matrices for each aspect to generate referential values . We finally use the multi-head attention to get NFEs ˜ h i and PFEs ˜ g i as follows,

$$\tilde { h } _ { i }, \tilde { g } _ { i } = \text{DAC-Att} \left ( W ^ { Q }, \ W ^ { K }, W ^ { V }, W ^ { V _ { \alpha } }, W ^ { O } \right ), \\ \tilde { h } _ { i } = \text{Concat} \left [ \text{head} ^ { h } _ { i, 1 }, \dots, \text{head} ^ { h } _ { i, m } \right ] W ^ { O } _ { h }, \ \tilde { g } _ { i } = \text{Concat} \left [ \text{head} ^ { g } _ { i, 1 }, \dots, \text{head} ^ { g } _ { i, m } \right ] W ^ { O } _ { g },$$

where head h i,k = out h i,k , head g i,k = out g i,k , and W ,W O h O g ∈ R 2 md v × dim are trainable parameter matrices. In our model, we adopt m = 4 and d k = d v = 16 .

FFN. Our FFN sub-layer has only one hidden layer with 64 hidden unites and adopts the ReLU activation function. The parameters of FFN h and FFN g are different for each group of embeddings.

## 4.3 The decoder

In the DAC decoder, the two sets of embeddings { h ( L ) i } N i =1 and { g ( L ) i } N i =1 are first passed through a Max-pooling sub-layer and a multi-head compatibility (MHC) sub-layer to independently generate

5 We omit the encoder index l for better readability.

diversified node-pair selection proposals from their own aspect, which are then aggregated through a feed-forward aggregation ( FFA ) sub-layer for output.

Max-pooling . For each set of embeddings, we adopt the max-pooling sub-layer in Wu et al. [11] to aggregate the global representation of all N embeddings into each respective one . 6

MHC . The compatibility sub-layer computes the attention correlations for each embedding pair, where the obtained correlations with size N × N will be deemed as a proposal distribution for node pair selection. Our correlations are computed based on multiple heads for diversity. And we calculate separated attention score matrices Y h k , Y g k ∈ R N × N (of head k ) from the two aspects independently. Accordingly, the action distribution proposals would be different due to their aspect-specific focus and cognitions of the current solution, which will provide the subsequent FFA layer with a rich pool of proposals and allow our model to be more flexible and robust.

FFA . Once all proposals from two aspects are collected, a FFN with four layers (dimensions are 2 m , 32, 32 and 1, respectively) and ReLU activation is used to aggregate them,

$$\tilde { Y } _ { i, j } & = \text{FFA} \left ( Y _ { i, j, 1 } ^ { g }, \dots, Y _ { i, j, m } ^ { g }, Y _ { i, j, 1 } ^ { h }, \dots Y _ { i, j, m } ^ { h } \right ), & & ( 1 1 )$$

where m =4 is the number of heads; and the output ˜ Y i,j is a scalar indicating the likelihood of selecting node pair ( i, j ) as an action. Afterwards, we apply ˆ Y ij = C · Tanh ( ˜ Y i,j ) with C = 6 to control the entropy, and mask 7 the infeasible node pairs ( i , j ′ ′ ) as ˆ Y i j ′ ′ = -∞ . Lastly, the likelihoods are normalized using Softmax function to obtain the final action distribution P i,j .

## 4.4 Reinforcement learning algorithm

We adopt the proximal policy optimization [32] with n -step return estimation for training (details are given in Appendix C), and design a curriculum learning (CL) strategy for better sample efficiency.

Curriculum learning strategy. The strategy in Wu et al. [11] sets a maximum of T train steps for training and estimates future returns by bootstrapping [33]. However, due to the concern of training cost, T train is usually much smaller than actual T for inference (e.g., 200 v.s. 10k), which may leave the agent a poor chance of observing high-quality solutions (states) during training. Consequently, it may cause high variance for bootstrapping because the value function is mostly fitted on low-quality solutions and may render it less knowledgeable in estimating long-term future returns accurately. In this paper, we tackle this issue by a simple yet efficient strategy which gradually prescribes higher-quality solutions as the initial states for training. In doing so, 1) it increases the probability for the agent to observe better solutions and thus reduce the variance of the value function; 2) it increases the difficulty of the learning task (higher-quality solutions are harder to improve) in a gradual manner and achieves better sample efficiency [34]. In practice, those higher-quality solutions can be easily achieved by improving the randomly generated ones using the current policy for a few T init steps, where T init could be slightly increased as the epoch grows.

## 5 Experiments

We evaluate our DACT model on two representative routing problems, i.e., TSP and CVRP [5, 8, 11]. For each problem, we abide by existing conventions to randomly generate instances on the fly for three sizes, i.e., N = 20, 50 and 100. Initial experiments with three operators including 2-opt , swap and insert show that 2-opt performs best for both TSP and CVRP (with insert better than swap ), hence we report results of our method based on 2-opt . Following [4, 11, 27] we use randomly generated initial solutions for training and the solutions generated by the greedy algorithm for inference. Since each problem has its own constraints and node features, we adjust the input, feasibility masks, and problem-dependent hyperparameters for each problem, the details of which are provided in Appendix D and E. The DACT models and baselines are tested on a server equipped with RTX 2080 TI GPU cards and Intel Xeon E5-2680 CPU @ 2.40GHz. Our code in PyTorch are available here 8 .

6 E.g., for ˆ h h , ˆ i = h ( L ) i W Local h +max [ { h ( L ) i } N i =1 ] W Global h , where W Local h ,W Global h ∈ R 64 × 64 are parameters.

7 Besides, we also mask all the diagonal elements since they are not meaningful to the pair-wise operators, and the node pair selected at the last step to forbid possible dead loops [11].

8 https://github.com/yining043/VRP-DACT

Table 1: Comparison with various baselines on TSP and CVRP.

|     | Method                   |        |          |                |         |          |                  | N=100   | N=100   | N=100            |
|-----|--------------------------|--------|----------|----------------|---------|----------|------------------|---------|---------|------------------|
|     | Method                   | Obj.   | N=20 Gap | Time ∗         | Obj.    | N=50 Gap | Time ∗           | Obj.    | Gap     | Time ∗           |
|     | Concorde [35]            | 3.83   | -        |                | 5.70    | -        | -                | 7.76    | -       | -                |
|     | LKH [36]                 | 3.83   | 0.00%    | - (6m)         | 5.70    | 0.00%    | (1.5h)           | 7.76    | 0.00%   | (6h)             |
|     | OR-Tools [37]            | 3.86 ‡ | 0.94% ‡  | (1m) ‡         | 5.85 ‡  | 2.87% ‡  | (5m) ‡           | 8.06 ‡  | 3.86% ‡ | (23m) ‡          |
|     | Neural-2-Opt [23] (T=3k) | 3.83   | 0.00%    | (47m)          | 5.70    | 0.10%    | (1h)             | 7.81    | 0.61%   | (1.5h)           |
|     | Neural-2-Opt [23] (T=6k) | 3.83   | 0.00%    | (1.5h)         | 5.70    | 0.06%    | (2h)             | 7.79    | 0.39%   | (3.5h)           |
|     | Wu et al. [11] (T=1k)    | 3.83 ‡ | 0.03% ‡  | (12m) ‡        | 5.74    | 0.82%    | (16m)            | 8.02    | 3.31%   | (24m)            |
|     | Wu et al. [11] (T=5k)    | 3.83 ‡ | 0.00% ‡  | (1h) ‡         | 5.71    | 0.24%    | (1.5h)           | 7.88    | 1.54%   | (2h)             |
|     | DACT (T=1k)              | 3.83   | 0.04%    | (2m) [ 40s ]   | 5.70    | 0.14%    | (7m) [ 2m ]      | 7.89    | 1.62%   | (20m) [ 5m ]     |
| TSP | DACT (T=5k)              | 3.83   | 0.00%    | (10m) [ 3m ]   | 5.70    | 0.02%    | (32m) [ 8m ]     | 7.81    | 0.61%   | (1.5h) [ 25m     |
| TSP | DACT (T=10k)             | 3.83   | 0.00%    | (21m) [ 7m ]   | 5.70    | 0.01%    | (1h) [ 17m ]     | 7.79    | 0.37%   | ] (3.5h) [ 50m ] |
| TSP | DACT × 4 augment         | 3.83   | 0.00%    | (1.5h) [ 28m ] | 5.70    | 0.00%    | (4h) [ 1h ]      | 7.77    | 0.09%   | (14h) [ 3.5h ]   |
|     | GCN-BS [6]               | 3.84 ‡ | 0.01% ‡  | (12m) ‡        | 5.70 ‡  | 0.01% ‡  | (18m) ‡          | 7.87 ‡  | 1.39% ‡ | (40m) ‡          |
|     | AM-sampling [5]          | 3.84 ‡ | 0.08% ‡  | (5m) ‡         | 5.73 ‡  | 0.52% ‡  | (24m) ‡          | 7.94 ‡  | 2.26% ‡ | (1h) ‡           |
|     | MDAM-BS [7]              | 3.84 ‡ | 0.00% ‡  | (3m) ‡         | 5.70 ‡  | 0.03% ‡  | (14m) ‡          | 7.79 ‡  | 0.38% ‡ | (44m) ‡          |
|     | POMO [8]                 | 3.83 ‡ | 0.04% ‡  | (1s) ‡         | 5.70 ‡  | 0.21% ‡  | (2s) ‡           | 7.80    | 0.46%   | (12s)            |
|     | POMO × 8 augment [8]     | 3.83 ‡ | 0.00% ‡  | (3s) ‡         | 5.69 ‡  | 0.03% ‡  | (16s) ‡          | 7.78    | 0.19%   | (2m)             |
|     | CVAE-Opt-DE [13]         | -      | ≈ 0.00%  | ( ≈ 1d)        | -       | ≈ 0.02%  | ( ≈ 2d)          | -       | ≈ 0.34% | ( ≈ 6d)          |
|     | DPDP [26] (100k)         | -      | -        | -              | -       | -        | -                | 7.77 ‡  | 0.00% ‡ | (3h) ‡           |
|     | LKH [28]                 | 6.14   | 0.00%    | (18h)          | 10.38   | 0.00%    | (3d)             | 15.65   | 0.00%   | (6d)             |
|     | OR-Tools [37]            | 6.46 ‡ | 5.68% ‡  | (2m) ‡         | 11.27 ‡ | 8.61% ‡  | (13m) ‡          | 17.12 ‡ | 9.54% ‡ | (46m) ‡          |
|     | NeuRewriter [4]          | 6.15 ‡ | -        | (22m) ‡        | 10.51 ‡ | -        | (35m) ‡          | 16.10 ‡ | -       | (1h) ‡           |
|     | NLNS [27] (T=1k)         | 6.19   | 0.85%    | (9m) #         | 10.56   | 1.82%    | (14m) #          | 16.09   | 2.84%   | (31m) #          |
|     | NLNS [27] (T=10k)        | 6.17   | 0.60%    | (1.5h) #       | 10.49   | 1.12%    | (2.5h) #         | 15.88   | 1.49%   | (5h) #           |
|     | Wu et al. [11] (T=1k)    | 6.16 ‡ | 0.90% ‡  | (23m) ‡        | 10.68   | 2.89%    | (50m)            | 16.39   | 4.76%   | (1h)             |
|     | Wu et al. [11] (T=5k)    | 6.12 ‡ | 0.39% ‡  | (2h) ‡         | 10.54   | 1.63%    | (4h)             | 16.17   | 3.31%   | (5h)             |
|     | DACT (T=1k)              | 6.14   | 0.04%    | (4m) [ 1m ]    | 10.55   | 1.73%    | (16m) [ 3m ]     | 16.20   | 3.52%   | (41m) [ 5m ]     |
|     | DACT (T=5k)              | 6.13   | -0.07%   | (20m) [ 6m ]   | 10.46   | 0.80%    | (1.5h) [ 13m     | 15.95   | 1.92%   | (3.5h) [ 26m ]   |
|     | DACT (T=10k)             | 6.13   | -0.07%   | (40m) [ 12m ]  | 10.43   | 0.58%    | ] (2.5h) [ 25m ] | 15.88   | 1.51%   | (7h) [ 52m ]     |
|     | DACT × 6 augment         | 6.13   | -0.08%   | (4h) [ 1h ]    | 10.38   | 0.08%    | (16h) [ 2.5h ]   | 15.74   | 0.57%   | (41h) [ 5h ]     |
|     | AM-sampling [5]          | 6.25 ‡ | 1.87% ‡  | (6m) ‡         | 10.62 ‡ | 2.40% ‡  | (28m) ‡          | 16.23 ‡ | 3.72% ‡ | (2h ‡ )          |
|     | MDAM-BS [7]              | 6.14 ‡ | 0.18% ‡  | (5m) ‡         | 10.48 ‡ | 0.98% ‡  | (15m) ‡          | 15.99 ‡ | 2.23% ‡ | (1h) ‡           |
|     | POMO [8]                 | 6.17 ‡ | 0.82% ‡  | (1s) ‡         | 10.49 ‡ | 1.14% ‡  | (4s) ‡           | 15.84   | 1.21%   | (21s)            |
|     | POMO × 8 augment [8]     | 6.14 ‡ | 0.21% ‡  | (5s) ‡         | 10.42 ‡ | 0.45% ‡  | (26s) ‡          | 15.75   | 0.69%   | (2m)             |
|     | CVAE-Opt-DE [13]         | ≈ 6.14 | -        | ( ≈ 2d)        | ≈ 10.40 | -        | ( ≈ 5d)          | ≈ 15.75 | -       | ( ≈ 11d)         |
|     | DPDP [26] (100k)         | -      | -        | -              | -       | -        | -                | 15.69 ‡ | 0.31% ‡ | (6h) ‡           |

∗ We report the total time using one 2080 TI GPU or one CPU in (·), and the total time using multiple GPUs (4 for TSP, 8 for CVRP) in [·].

‡ The results (i.e., obj. values, gaps, and time) are adopted from its original paper or from Wu et al. [11].

# The run time of NLNS is based on one GPU and ten CPUs (in parallel) following its default setting.

## 5.1 Comparison studies

In Table 1, we compare our DACT with, (1) learning based improvement methods, including Wu et al. [11], Neural-2-Opt [23] (TSP only), NeuRewriter [4] (CVRP only), NLNS [27] (CVRP only), (2) learning based construction methods, including AM-sampling [5], GCN-BS [6] (TSP only), MDAM-BS [7], POMO [8], (3) conventional optimization algorithms equipped with learning based component(s), including DPDP [26], CVAE-Opt-DE [13], and (4) strong conventional solvers including Concorde [35], LKH [28, 36], and OR-Tools [37]. We do not include L2I [12] as a baseline since it requires a prohibitively longer inference time than others . 9 All results are averaged over 10,000 randomly generated instances except for CVAE-Opt-DE (with ≈ ) which only infers 1,000 instances, and we report the metrics of objective values, (optimality) gaps and run time. We test strong baselines POMO (TSP100, CVRP100), Neural-2-Opt, Wu et al. (except for size 20), and NLNS on our machine based on the pre-trained models. Other results (with ‡ ) are adopted from their original papers or from Wu et al. [11]. For TSP, Concorde is adopted to get the optimal solutions; CVRP is harder to be solved optimally, and the gaps are based on objective values of LKH3. We report the total time using one GPU (for neural solvers) or one CPU (for conventional solvers) in (·), and the total time using multiple GPUs (for DACT) in [·]; for NLNS (with # ), we use one GPU and ten CPUs (in parallel) following its default setting. Note that the run time (for CPU solvers, and those with ‡ and # ) may not be directly comparable with ours and hard to compare due to various factors (e.g., GPU v.s. CPU, Python v.s. C, different devices) and thus we focus more on gaps.

Pertaining to TSP, our DACT with inference steps 1,000 (T=1k) and 5,000 (T=5k) significantly outperforms the traditional solver OR-Tools and the baseline Wu et al. [11] which directly adopted

9 L2I needs 167 days for 10,000 CVRP100 instances, as estimated from 24min/instance in its original paper.

Table 2: Generalization performance. (a) DACT v.s. baselines on benchmark datasets (up to 200 customers, see Appendix E.4 for detailed results and discussion); (b) PE v.s. CPE on different sizes.

| Method         | TSPLIB   | CVRPLIB   |
|----------------|----------|-----------|
| OR-Tools [37]  | 3.34%    | 8.06%     |
| POMO [8]       | 10.06%   | 6.10%     |
| Wu et al. [11] | 4.17%    | 5.20%     |
| DACT-Avg       | 1.90%    | 3.85%     |
| DACT-Best      | 1.01%    | 2.97%     |
|                | (a)      |           |

| Method                | N=20   | N=20   | N=100   | N=100   |
|-----------------------|--------|--------|---------|---------|
|                       | Obj.   | Gap    | Obj.    | Gap     |
| DACT-PE (T=5k)        | 3.84   | 0.21%  | 8.38    | 7.93%   |
| DACT-CPE (T=5k)       | 3.83   | 0.10%  | 7.99    | 2.98%   |
| Wu et al. [11] (T=5k) | 3.91   | 2.14%  | 9.03    | 16.37%  |
| OR-Tools [37]         | 3.83   | 0.00%  | 8.06    | 3.87%   |

(b)

Table 3: Dual v.s. single aspect representation

| Steps   | Method   | # Params    | N=50                  | N=100             |
|---------|----------|-------------|-----------------------|-------------------|
| T=1k    | SA-T     | 0.37M       | 0.35% (2m)            | 3.49% (4m)        |
| T=1k    | DACT     | 0.29M       | 0.14% (2m)            | 1.62% (5m)        |
| T=5k    | SA-T     | 0.37M 0.29M | 0.05% (7m) 0.02% (8m) | 1.55% (22m) 0.61% |
| T=5k    | DACT     |             |                       | (25m)             |

the original Transformer encoder. The DACT (T=5k) also deliveries lower gaps than construction methods including AM-sampling and GCN-BS on TSP100. With larger steps T=10k, our DACT further boosts the solution qualities and outperforms other construction methods including MDAMBS (with beam search), and POMO (the current state-of-the-art). Besides, our DACT consistently outperforms improvement method Neural-2-Opt with shorter or similar run time. To further reduce the gaps, we also leverage the data augmentation technique in POMO (which considers flipping node coordinates without changing the optimal solution) to solve same instances multiple times in different ways. Although the inference time increases (we run data augmentation in serial), our DACT with 4 augments not only outstrips POMO with 8 augments but also achieves the lowest objective values and gaps among all purely learning based models. In particular, we achieve the gap of 0.09 % on TSP100, which is superior to most of the recent neural solvers. Pertaining to CVRP, our DACT with T=5k achieves better objective values than that of NeuRewriter. It also consistently outperforms the important baseline Wu et al. [11] in all cases. Compared with the NLNS solver that learned to control the large neighborhood search heuristics, the gaps achieved by DACT (T=10k) are smaller on CVRP20 and CVRP50, and highly comparable on CVRP100. With T=10k and 6 augments, our DACT exhibits even better performance than the highly specialized heuristic solver LKH on CVRP20 and delivers the smallest gap of 0.57% on CVRP100 against other neural solvers including POMO with 8 augments. Compared with hybrid solver CVAE-Opt-DE, despite that it is averaged over 1,000 instances and integrated with search algorithm like differential evolution, our objective values are still lower on both TSP and CVRP. Nevertheless, our DACT is inferior to a more advanced hybrid solver DPDP which leverages learnt heat-maps and traditional dynamic programming to search solutions.

In terms of the inference time, our DACT is highly competitive against most of the neural solvers except for POMO which learns a construction model by sampling diverse trajectories. However, when it comes to the generalization performance on benchmark datasets, i.e., TSPLIB [38] and CVRPLIB [39] in Table 2(a), DACT produces significantly lower average gaps than the POMO with 8 augments, which indicates that our DACT is more advantageous in practice despite its longer inference time. On the other hand, it is possible to adopt a similar diverse rollout strategy for DACT to find better solutions earlier, or explore other model compression techniques such as the knowledge distillation [40] to learn a lighter DACT model for faster inference. Since our focus is to ameliorate Transformer for neural improvement solvers, we will investigate these possibilities in the future.

## 5.2 Ablation studies

Dual-aspect representation. In Table 3, we evaluate the effectiveness of our dual-aspect representation against the single-aspect one (SA-T) on TSP50 and TSP100, where SA-T mainly follows the Transformer in Wu et al. [11] but equipped with the CPE, multi-head attentions and CL strategy for fair comparison. We observe that our DACT with fewer parameters consistently outperforms SA-T, which verifies the effectiveness of the dual-aspect representation.

TSP20 Policy tested  tested

TSP20 Policy tested  tested on TSP50 (PE) 50 (PE) on TSP20 (CPE)  (CPE)on TSP50 (CPE)  (CPE)

0

5

10

15

20

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000005_7e6d37421a1ea2a95477ce858dee79b3592a27c465a11462ce05e3b7ce869a69.png)

40 3 50 40

0 0

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000006_fae467dd03b7ee5c186583f7ab6a3473c3a285b2c026edad8bdd5b59b6ed7454.png)

5

Figure 6: Visualization of the (multi-head) attention scores for the encoder when a trained model is used to solve instances with a larger size. (a) using PE method; (b) using CPE method (ours).

Cyclic positional encoding. Here we show that CPE significantly improves the generalization performance across different problem sizes. In Table 2(b), we record the results of our DACT with PE and CPE, and Wu et al. [11], when the model trained on TSP50 is directly used to solve instances from TSP20 and TSP100 with T=5k. We see that even with PE, our DACT outperforms Wu et al. [11]. Further equipped with CPE, DACT outstrips DACT-PE and OR-Tools on TSP100. We continue to compare the two DACT variants by visualizing their attention scores. As depicted in Figure 6(a), although the absolute PE is designed for linear sequences, it did attempt to capture the circularity of

Figure 7: Training curves of PPO with and without CL on CVRP20 (random seeds 1-5).

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000007_88ea7153f62bee34d095131d8d2a00bd808d91be1496e51913636c43c4e26c1f.png)

VRP solutions (as highlighted in the green boxes) after training. However, the ability to perceive such properties significantly drops when generalizing over different problem size, which instead engenders random attention scores when generalizing to larger size (see right side of Figure 6(a)). In contrast, our DACT with CPE is able to capture the circularity as depicted in Figure 6(b), which verifies the effectiveness of CPE in representing cyclic sequences (i.e., VRP solutions).

Curriculum learning (CL) strategy. In Figure 7, we plot training curves of PPO algorithm with and without our CL strategy for solving CVRP, where the results are averaged over 5 independent runs with 90% confidence intervals. It shows that our CL strategy significantly improves the sample efficiency while reducing the variance of training, which aligns with our analysis in Section 4.4.

## 6 Conclusions and future work

In this paper, we present a novel DACT model for routing problems. It learns separate groups of embeddings for the node and positional features, and is equipped with cyclic positional encoding (CPE) to capture the circularity and symmetry of VRP solutions. A curriculum learning (CL) strategy is also exploited to improve the RL training efficiency. Extensive experiments on both synthetic and benchmark datasets justified the effectiveness of DACT in terms of both inference and generalization. A potential limitation is that DACT is more useful for learning improvement models at present. In the future, we will investigate how to extend DACT to construction models, and how to speed up the DACT through diverse rollouts or model compression techniques. It is also interesting to apply the proposed CPE to develop Transformer based model for other tasks where the cyclic property is also important, e.g., encoding circular DNA/RNA structures in computational biology [41, 42].

## Acknowledgments and Disclosure of Funding

This work was supported in part by the National Natural Science Foundation of China under Grant 61803104 and Grant 62102228, in part by the Young Scholar Future Plan of Shandong University under Grant 62420089964188, and in part by the A*STAR CyberPhysical Production System (CPPS) - Towards Contextual and Intelligent Response Research Program, under the RIE2020 IAF-PP Grant A19C1a0018, and Model Factory@SIMTech.

## References

| [1]   | Paolo Toth and Daniele Vigo. Vehicle routing: problems, methods, and applications . SIAM press, 2014.                                                                                                                                                          |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [2]   | Michael Schneider, Andreas Stenger, and Dominik Goeke. The electric vehicle-routing problem with time windows and recharging stations. Transportation Science , 48(4):500-520, 2014.                                                                           |
| [3]   | Jan Karel Lenstra and AHG Rinnooy Kan. Complexity of vehicle routing and scheduling problems. Networks , 11(2):221-227, 1981.                                                                                                                                  |
| [4]   | Xinyun Chen and Yuandong Tian. Learning to perform local rewriting for combinatorial optimization. In Advances in Neural Information Processing Systems , volume 32, pages 6281-6292, 2019.                                                                    |
| [5]   | Wouter Kool, Herke van Hoof, and Max Welling. Attention, learn to solve routing problems! In International Conference on Learning Representations , 2018.                                                                                                      |
| [6]   | Chaitanya K Joshi, Thomas Laurent, and Xavier Bresson. An efficient graph convolutional network technique for the travelling salesman problem. arxiv preprint arxiv:1906.01227, ArXiV, 2019.                                                                   |
| [7]   | Liang Xin, Wen Song, Zhiguang Cao, and Jie Zhang. Multi-decoder attention model with embedding glimpse for solving vehicle routing problems. In Proceedings of 35th AAAI Conference on Artificial Intelligence , pages 12042-12049, 2021.                      |
| [8]   | Yeong-Dae Kwon, Jinho Choo, Byoungjip Kim, Iljoo Yoon, Youngjune Gwon, and Seungjai Min. POMO: Policy optimization with multiple optima for reinforcement learning. In Advances in Neural Information Processing Systems , volume 33, pages 21188-21198, 2020. |
| [9]   | Liang Xin, Wen Song, Zhiguang Cao, and Jie Zhang. Step-wise deep learning models for solving routing problems. IEEE Transactions on Industrial Informatics , 17(7):4861-4871, 2020.                                                                            |
| [10]  | Cong Zhang, Wen Song, Zhiguang Cao, Jie Zhang, Puay Siew Tan, and Xu Chi. Learning to dispatch for job shop scheduling via deep reinforcement learning. In Advances in Neural Information Processing Systems , volume 33, pages 1621-1632, 2020.               |
| [11]  | Yaoxin Wu, Wen Song, Zhiguang Cao, Jie Zhang, and Andrew Lim. Learning improvement heuristics for solving routing problems. IEEE Transactions on Neural Networks and Learning Systems , 2021.                                                                  |
| [12]  | Hao Lu, Xingwen Zhang, and Shuang Yang. A learning-based iterative method for solving vehicle routing problems. In International Conference on Learning Representations , 2019.                                                                                |
| [13]  | André Hottung, Bhanu Bhandari, and Kevin Tierney. Learning a latent search space for routing problems using variational autoencoders. In International Conference on Learning Representations , 2021.                                                          |
| [14]  | Jingwen Li, Yining Ma, Ruize Gao, Zhiguang Cao, Andrew Lim, Wen Song, and Jie Zhang. Deep rein- forcement learning for solving the heterogeneous capacitated vehicle routing problem. IEEE Transactions on Cybernetics , 2021.                                 |
| [15]  | Richard S Sutton and Andrew G Barto. Reinforcement learning: An introduction . MIT press, 2018.                                                                                                                                                                |
| [16]  | Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N Gomez, Łukasz Kaiser, and Illia Polosukhin. Attention is all you need. In Advances in Neural Information Processing Systems , volume 30, pages 6000-6010, 2017.               |
| [17]  | Jingwen Li, Liang Xin, Zhiguang Cao, Andrew Lim, Wen Song, and Jie Zhang. Heterogeneous attentions for solving pickup and delivery problem via deep reinforcement learning. IEEE Transactions on Intelligent Transportation Systems , 2021.                    |
| [18]  | Guolin Ke, Di He, and Tie-Yan Liu. Rethinking the positional encoding in language pre-training. In International Conference on Learning Representations , 2020.                                                                                                |
| [19]  | Peter Shaw, Jakob Uszkoreit, and Ashish Vaswani. Self-attention with relative position representations. In North American Chapter of the Association for Computational Linguistics: Human Language Technologies , pages 464-468, 2018.                         |
| [20]  | Oriol Vinyals, Meire Fortunato, and Navdeep Jaitly. Pointer networks. In Advances in Neural Information Processing Systems , volume 28, pages 2692-2700, 2015.                                                                                                 |
| [21]  | Irwan Bello, Hieu Pham, Quoc V Le, Mohammad Norouzi, and Samy Bengio. Neural combinatorial optimization with reinforcement learning. In International Conference on Machine Learning (Workshop) ,                                                              |

| [22]   | Mohammadreza Nazari, Afshin Oroojlooy, Martin Takáˇ, c and Lawrence VSnyder. Reinforcement learning for solving the vehicle routing problem. In Advances in Neural Information Processing Systems , pages 9861-9871, 2018.                           |
|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [23]   | Paulo R d O Costa, Jason Rhuggenaath, Yingqian Zhang, and Alp Akcay. Learning 2-opt heuristics for the traveling salesman problem via deep reinforcement learning. In Asian Conference on Machine Learning pages 465-480, 2020.                      |
| [24]   | Hanjun Dai, Elias BKhalil, Yuyu Zhang, Bistra Dilkina, and Le Song. Learning combinatorial optimization algorithms over graphs. In Advances in Neural Information Processing Systems , pages 6351-6361, 2017.                                        |
| [25]   | Zhang-Hua Fu, Kai-Bin Qiu, and Hongyuan Zha. Generalize a small pre-trained model to arbitrarily large TSP instances. In AAAI Conference on Artificial Intelligence , 2021.                                                                          |
| [26]   | Wouter Kool, Herke van Hoof, Joaquim Gromicho, and Max Welling. Deep policy dynamic programming for vehicle routing problems. arXiv preprint arXiv:2102.11756 , 2021.                                                                                |
| [27]   | André Hottung and Kevin Tierney. Neural large neighborhood search for the capacitated vehicle routing problem. In European Conference on Artificial Intelligence , 2020.                                                                             |
| [28]   | Keld Helsgaun. LKH-3 (version 3.0.6), 2019. URL http://webhotel4.ruc.dk/~keld/research/ LKH-3/ .                                                                                                                                                     |
| [29]   | Wikipedia. Gray code, 2021. URL: https://en.wikipedia.org/wiki/Gray_code . Last visited on 2020/05/19.                                                                                                                                               |
| [30]   | Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. Deep residual learning for image recognition. In IEEE conference on computer vision and pattern recognition , pages 770-778, 2016.                                                            |
| [31]   | Lei Jimmy Ba, Jamie Ryan Kiros, and Geoffrey E. Hinton. Layer normalization. Corr: abs/1607.06450, ArXiV, 2016.                                                                                                                                      |
| [32]   | John Schulman, Filip Wolski, Prafulla Dhariwal, Alec Radford, and Oleg Klimov. Proximal policy optimization algorithms. arxiv preprint arxiv:1707.06347, ArXiV, 2017.                                                                                |
| [33]   | Fabio Pardo, Arash Tavakoli, Vitaly Levdik, and Petar Kormushev. Time limits in reinforcement learning. In International Conference on Machine Learning , pages 4045-4054, 2018.                                                                     |
| [34]   | Yoshua Bengio, Jérôme Louradour, Ronan Collobert, and Jason Weston. Curriculum learning. In International Conference on Machine Learning , pages 41-48, 2009.                                                                                        |
| [35]   | David L Applegate, Robert E Bixby, Vašek Chvátal, and William J Cook. Concorde TSP Solver, 2020. URL http://www.math.uwaterloo.ca/tsp/concorde/ .                                                                                                    |
| [36]   | Keld Helsgaun. LKH (version 2.0.9), 2018. URL http://webhotel4.ruc.dk/~keld/research/ LKH/ .                                                                                                                                                         |
| [37]   | Laurent Perron and Vincent Furnon. OR-Tools (version 7.2), 2019. URL https://developers.google. com/optimization/ .                                                                                                                                  |
| [38]   | Gerhard Reinelt. TSPLIB-A traveling salesman problem library. ORSA journal on computing , 3(4): 376-384, 1991.                                                                                                                                       |
| [39]   | Eduardo Uchoa, Diego Pecin, Artur Pessoa, Marcus Poggi, Thibaut Vidal, and Anand Subramanian. New benchmark instances for the capacitated vehicle routing problem. European Journal of Operational Research , 257(3):845-858, 2017.                  |
| [40]   | Geoffrey Hinton, Oriol Vinyals, and Jeff Dean. Distilling the knowledge in a neural network. arXiv preprint arXiv:1503.02531 , 2015.                                                                                                                 |
| [41]   | Chun-Ying Yu, Tung-Cheng Li, Yi-Ying Wu, Chan-Hsien Yeh, Wei Chiang, Ching-Yu Chuang, and Hung-Chih Kuo. The circular rna circbirc6 participates in the molecular circuitry controlling human pluripotency. Nature communications , 8(1):1-15, 2017. |
| [42]   | Chengyu Liu, Yu-Chen Liu, Hsien-Da Huang, and Wei Wang. Biogenesis mechanisms of circular rna can be categorized through feature extraction of a machine learning model. Bioinformatics , 35(23):4867-4870, 2019.                                    |
| [43]   | Logan Engstrom, Andrew Ilyas, Shibani Santurkar, Dimitris Tsipras, Firdaus Janoos, Larry Rudolph, and Aleksander Madry. Implementation matters in deep policy gradients: A case study on PPO and TRPO. In                                            |

## Learning to Iteratively Solve Routing Problems with Dual-Aspect Collaborative Transformer (Appendix)

## A Issues in existing Transformer-based model for VRP

We investigate the issues of mixed correlations and noisy biases when the absolute PE method of Transformer is directly used to learn improvement heuristics in Wu et al. [11]. By fusing the node feature embedding h i and the positional feature embedding g i through an addition operator, four attention query terms from input i to j exist during the self-attention of the encoder as follows,

$$\begin{array} { c } \text{:ntion query terms from input } i \, to \, j \, \text{exist during the self-attention of the encoder as follows,} \\ \alpha _ { i, j } ^ { A b s } & = \frac { 1 } { \sqrt { d _ { k } } } \left ( ( h _ { i } + g _ { i } ) W ^ { Q } \right ) \left ( ( h _ { j } + g _ { j } ) W ^ { K } \right ) ^ { T } \\ & = \frac { 1 } { \sqrt { d _ { k } } } \left ( h _ { i } W ^ { Q } \right ) \left ( h _ { j } W ^ { K } \right ) ^ { T } + \frac { 1 } { \sqrt { d _ { k } } } \left ( g _ { i } W ^ { Q } \right ) \left ( g _ { j } W ^ { K } \right ) ^ { T } \\ & + \frac { 1 } { \sqrt { d _ { k } } } \left ( h _ { i } W ^ { Q } \right ) \left ( g _ { j } W ^ { K } \right ) ^ { T } + \frac { 1 } { \sqrt { d _ { k } } } \left ( g _ { i } W ^ { Q } \right ) \left ( h _ { j } W ^ { K } \right ) ^ { T }, \\ \text{ere we call them } \, \ n o d e { \text{-to-node, position-to-position, node-to-position, \, \ n o d e { \text{-position-to-node, \, \ n o d e { \text{-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-sb-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-t-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-sl-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s- s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-S-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-n-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-ss-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-g-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-gs-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-ns-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-l-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-ls-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-ds-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-d-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-r-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-m-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s-s$$

where we call them node-to-node , position-to-position , node-to-position , and position-to-node , respectively. Obviously, they all share the same projection matrices W Q and W K , which might be unreasonable since they are used to represent correlations of different information [18]. Furthermore, the last two terms are essentially computing the mixed correlations across different information. Intuitively, queries from the location of a node (node feature) to the index of another node (positional feature) would be meaningless and vice versa. Such design may further bring noisy biases to routing problems. To verify this, we visualize the above four attention terms using a pre-trained model of Wu et al. [11] on a sampled batch of instances for TSP20. As shown in Figure 8, the last two correlations (node-to-position and position-to-node) seem to unreasonably present some random patterns across different node pairs, e.g., all nodes tend to have strong correlations with the ones appeared close to the end of the solution. This may yield biased attention, and thus affect the accuracy and the performance of the learned heuristics. In contrast, our DACT avoids such mixed correlations by learning feature embeddings for two aspects separately without fusing them into a unified representation.

Figure 8: Visualizations of correlations on a trained model of Wu et al. [11]. From left to right: correlation for node-to-node, position-to-position, node-to-position, and position-to-node, respectively.

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000008_5a51236872ddc82c6a697d2a6f93f631cde215eb530d9fe2db75e4b18f0f8750.png)

## B Details of CPE

We let the angular frequencies ω d = 2 π λ d decrease along the vector dimension by increasing the wavelength λ d according to Eq. (13). Empirically, we fix the last half of the wavelength to N (the largest value) to better preserve the desired cyclic and adjacency similarity properties. Regarding the generalization of CPE, we explored two strategies: 1) directly use the target size N new to generate the above frequencies, 2) reuse the frequencies generated based on N old in pre-trained model. We find that the former is often better when the difference between N new and N old is not too large, and conversely, the latter could be better. In this paper, we report the best performance under both strategies.

$$\lambda _ { d } = & \begin{cases} \frac { \cdot \cdot } { d / 3 \, | + 1 } ( N - N \frac { 1 } { [ d / 1 / 2 ] } ) + N \frac { 1 } { [ d / 1 / 2 ] }, \text{if} \, d < \left [ \frac { d i m } { 2 } \right ] \\ N, & \text{otherwise} \end{cases}$$

## C Training algorithm

## Algorithm 1 n -step PPO with curriculum learning strategy

Input: initial policy network parameters θ ; initial value function parameters φ ; clipping threshold ε ; initial learning rate η θ , η φ , learning rate decay β .

- 1: for epoch = 1 to E do 2: for b = 1 to B do 3: Randomly generate a batch of training instances D b ; 4: Initialize random solutions { δ i } to D b ; 5: CL: Improve { δ i } to { δ ′ i } by iterating T init = S e ( ) -S (0) S E ( ) -S (0) ξ CL E steps with the current policy network (DACT) π θ , where S epoch ( ) = 1 / ( 1 + e -κ epoch ( -E/ 2) ) ; 6: Set initial state s 0 = { δ ′ i } , t ← 0 ; 7: while t &lt; T train do 8: Collect experience { ( s t ′ , a t ′ , r t ′ ) } t + n t ′ = t by running policy π θ for n time steps where a t ′ ∼ π θ ( a t ′ | s t ′ ) ; 9: Set t ← t + n π , old ← π θ , v old ← v φ ; 10: for k = 1 to K do 11: ˆ R t +1 = v φ ( s t +1 ) ; 12: for t ′ ∈ { t, t -1 , ..., t -n } do 13: ˆ R t ′ ← r t ′ + γR ˆ t ′ +1 ; 14: ˆ A t ′ ← ˆ R t ′ -v φ ( s t ′ ) ; 15: end for 16: Compute PPO-Clip objective J PPO ( θ ) using Eq. (14) and clipped value function loss L BL ( φ ) using Eq. (16); 17: θ ← θ + η θ ∇ J PPO ( θ ) ; 18: φ ← φ -η φ ∇ L BL ( φ ) ; 19: end for 20: end while 21: end for 22: η θ ← βη θ , η φ ← βη φ ; 23: end for

As presented in Algorithm 1, our training algorithm for DACT is adapted from the proximal policy optimization (PPO) [32], which is a prevailing RL algorithm. In particular, we follow the actor-critic variant of PPO which considers subtracting a baseline v φ ( s t ) (i.e., value function) in the objective function (line 14) to reduce the variance. Our v φ is similar to the one in Wu et al. [11] as follows, (1) it takes the concatenation of node and positional embeddings as input, and then enhances them by a normal multi-head attention layer (with 6 heads); and (2) the enhanced embeddings are passed through a mean-pooling layer (similar to the max-pooling layer in the DAC decoder) and then processed by a four-layer feed forward network (with 128 and 64 hidden units) to get the output value.

We train π θ and v φ for E epochs and B batches per epoch. For each batch, we generate training instances D b on the fly (line 3) and use the proposed curriculum learning strategy to initialize the state (line 4 to 6). We exploit the n -step return estimation to attain a satisfactory trade-off between one step temporal difference (TD) method and Monte Carlo (MC) method [11] (line 8 to 15). Afterwards, PPO performs K epochs of updates on D b with its objective clipped by a threshold ε to penalize large policy variances that move the probability ratio π θ ( a t | s t ) π old ( a t | s t away from 1 (as shown in Eq. (14)),

$$J _ { P P O } ( \theta ) = \frac { 1 } { | \mathcal { D } _ { b } | n } \sum _ { \mathcal { D } _ { b } } \sum _ { t ^ { \prime } = t } ^ { t + n } m i n \left ( \frac { \pi _ { \theta } ( a _ { t ^ { \prime } } | s _ { t ^ { \prime } } ) } { \pi _ { o l d } ( a _ { t ^ { \prime } } | s _ { t ^ { \prime } } ) } \hat { A } _ { t ^ { \prime } }, \, c l i p \left [ \frac { \pi _ { \theta } ( a _ { t ^ { \prime } } | s _ { t ^ { \prime } } ) } { \pi _ { o l d } ( a _ { t ^ { \prime } } | s _ { t ^ { \prime } } ) }, 1 - \varepsilon, 1 + \varepsilon \right ] \hat { A } _ { t ^ { \prime } } \right ).$$

We also clip the estimated value ˆ( v s t ) around the previous one as shown in Eq. (15) for better performance [43] and define the baseline loss in Eq. (16). The parameters of our two networks are updated by the Adam Optimizer (line 17,18) with a decaying learning rate (line 22),

$$v _ { \phi } ^ { c l i p } ( s _ { t ^ { \prime } } ) = c l i p \left [ v _ { \phi } ( s _ { t ^ { \prime } } ), v _ { o l d } ( s _ { t ^ { \prime } } ) - \varepsilon, v _ { o l d } ( s _ { t ^ { \prime } } ) + \varepsilon \right ],$$

$$L _ { B L } ( \phi ) = \frac { 1 } { | \mathcal { D } _ { b } | n } \sum _ { \mathcal { D } _ { b } } \sum _ { t ^ { \prime } = t } ^ { t + n } m a x \left ( \left | v _ { \phi } ( s _ { t ^ { \prime } } ) - \hat { R } _ { t ^ { \prime } } \right |, \ \left | v _ { \phi } ^ { c l i p } ( s _ { t ^ { \prime } } ) - \hat { R } _ { t ^ { \prime } } \right | \right ) ^ { 2 }.$$

For our curriculum learning strategy, we adopt κ = 0 2 . and adjust the maximum CL step limit coefficient ξ CL according to the difficulty level and the problem sizes of the routing problems. Ideally, the selected ξ CL should satisfy the following conditions: (1) it is able to considerably boost the sample efficiency of training compared with the smaller ones, and (2) if a larger value is adopted, it may not be able to further bring a significant improvement. In practice, we recommend to determine the value by performing preliminary short training (around 10 epochs) with different ξ CL .

## D Problem-specific description

## D.1 Travelling salesman problem (TSP)

Problem setup. ATSP instance considers to find the shortest loop that visits N nodes exactly once, and finally returns to the original one. We follow Kool et al. [5] to generate the coordinates of N nodes in the unit square [0 , 1] × [0 , 1] with a uniform distribution.

State feature representations. The node feature x i of node i in the state is represented by its location. Let c i denote the 2-dim coordinates of node i . We define x i = c i .

## D.2 Capacitated vehicle routing problem (CVRP)

Problem setup. On the basis of TSP, we add another node with index 0 as the depot, and let the original N nodes be customers. A CVRP instance considers to find the minimum total travel distance to serve all customers with multiple vehicles. Hence the solution to CVRP may consist of multiple sub-routes, each of which is a loop of nodes visited by one vehicle, i,e., departing from the depot, visiting the customers on this sub-route, and finally returning to the depot. The constraints of CVRP include: 1) the total demands of a sub-route cannot exceed the vehicle capacity Q , and 2) all customers must be visited exactly once. Similarly, we follow Kool et al. [5] to generate the coordinates of all nodes in the unit square [0 , 1] × [0 , 1] with the uniform distribution, and sample the demand of each customer uniformly from { 1 2 , , ..., 9 } . The Q is set to be 30, 40 and 50 for CVRP20, CVRP50 and CVRP100, respectively.

The length of CVRP solution might be larger than N +1 since the depot could be visited for multiple times. It may also vary even for the same instance, since different solutions may contain different numbers of sub-routes. For example, both δ 0 = 0 1 2 0 4 3 0 { , , , , , , } and δ 1 = 0 1 2 3 4 0 { , , , , , } with different lengths could be feasible solutions to CVRP with 4 customers. Such varying lengths render it much hard for parallel batch training. We thus add multiple dummy depots to the end of initial solutions following Wu et al. [11], where the number of dummy depots could be also considered as the maximum number of available vehicles. In the aforementioned example, after adding dummy depots (we index the depots as (1),(2),(3)), the solution δ ′ 1 = 0 { (1) , 1 2 3 4 0 , , , , (2) , 0 (3) } will have the same length with δ 0 . In doing so, 1) it guarantees the same length of solutions for a instance batch; and 2) it allows the policy to automatically learn the number of sub-routes and their lengths in a solution. E.g., δ ′ 1 = 0 { (1) , 1 2 3 4 0 , , , , (2) , 0 (3) } at step t could be changed to { 0 (1) , 1 2 0 , , (2) , 4 3 0 , , (3) } (equivalent to δ 0 ) at step t +1 using the 2-opt operator given action (3 , 0 (2) ) . In our experiments, we empirically set 10 (dummy) depots for CVRP20, and 20 (dummy) depots for CVRP50 and CVRP100, respectively.

State feature representations. For CVRP, the node feature x i for node i in the state is represented as a 7-dimensional vector [11], which contains, 1) the 2-dim coordinates of node i , i.e., c i ; 2) the distance from node i to its preceding neighbour; 3) the distance from node i to its succeeding neighbour; 4) sum of demands of the corresponding route before node i ; 5) the demand of node i ; and 6) sum of demands of the corresponding route after node i (including node i ). Due to the circularity and symmetry of VRP solutions, we consider the succeeding neighbour of the last node in a solution to be the first node, and the preceding neighbour of the first node to be the last node.

Figure 9: Box plots of the objective values obtained by our DACT model (without augments) for 10 independent runs of 10,000 testing instances (with random seeds 1-10). (a) TSP100; (b) CVRP100.

![Image](Papers_Converted/0_Artifacts/[ma2022]_Learning_to_iteratively_solve_routing_problems_with_dualaspect_collaborative_transformer_artifacts/image_000009_fd6667c7a4246726c3eb14badd13ff48d55a51128a7fd3fc9fcb9eef2903afd9.png)

## E More discussion on the experiments

## E.1 Hyperparameters

We set ξ CL as 0.25, 2, 10 for TSP 20, TSP50, and TSP100; 1, 4, 12.5 for CVRP20, CVRP50, and CVRP100, respectively. To avoid exploding gradients, we follow [5, 7, 17] to clip the gradient norm of each model parameter to be within 0.04, 0.2, and 0.45 for both TSP and CVRP of the three sizes, respectively. We set the reward discount factor γ = 0 999 . for both problems. Regarding T r , we use 250 for inference; 10 (TSPLIB) and 20 (CVRPLIB) for generalization on benchmark datasets.

## E.2 Training

We train our model with E =200 epochs and B =20 batches per epoch with batch size 600 for TSP and CVRP. We use 512 for CVRP100 due to the limited GPU memory. Regarding the n -step PPO algorithm, we set n =4 , T train =200 for TSP, and n =5 , T train =250 for CVRP. PPO performs K =3 mini-batch updates per batch with its objective function clipped by a threshold glyph[epsilon1] =0 1 . . We adopt the Adam optimizer with a learning rate η θ =10 -4 for π θ and η φ =3 × 10 -5 for v φ , both of which are decayed with β =0 985 . per epoch for convergence. We use pretrained models for TSP50, CVRP20, and CVRP50 to train TSP100, CVRP50, and CVRP100 for faster convergence, while for others the model is initialized randomly. We enable all state transitions of the MDP and the masking for feasibility of a batch to be performed in parallel on GPU for higher efficiency. Training time depends on the problem and varies with problem sizes. For example, the training time for CVRP sizes 20, 50, and 100 are around 4 days (1 GPU), 1 week (2 GPUs), and 2 weeks (4 GPUs), respectively.

## E.3 Stability analysis of our DACT

We study the stability of our DACT model (without augments) during inference stage. Figure 9 depicts the box plots of objective values on TSP100 and CVRP100 for 10 independent runs of 10,000 testing instances, where the minimum, lower quartile, mean, upper quartile, maximum, and possible outlets of results are depicted. In each sub-plot, we show the results for three step limit T , i.e., T = 1k, 5k, and 10k as per the settings in Table 1. For all cases, the range of box plots are within only 0.005 for both TSP and CVRP. These results show that our DACT has desirable stability for inference.

## E.4 Generalization on benchmark datasets

We now evaluate the generalization performance of DACT by applying the trained models in Section 5 to solve instances from two well-known benchmark datasets, i.e., TSPLIB [38] and CVRPLIB [39], respectively. Note that these instances may follow completely different distributions from ours, such as clustered customer locations, corner depot location, etc. We report the results on instances with size between 50 and 200 for TSPLIB; and size between 100 and 200 for CVRPLIB.

As recorded in Table 4 and 5, we first compare our DACT with Wu et al. [11] in the first group of columns to verify the superior performance of DACT to the existing Transformer based improvement model. In the second group of columns, we report the performance of several strong baselines

Table 4: Generalization of DACT (10 runs) v.s. baselines on TSPLIB benchmark dataset.

| Instance               | Wu et al. [11] (T=3k)   | DACT(T=3k) Best Avg   | DACT(T=3k) Best Avg   | OR-Tools   | AM-sampling (N=10k)   | POMO × 8 augment   | Wu et al. [11] (T=3k, M=1k)   | DACT(T=10k) Best Avg   | DACT(T=10k) Best Avg   | DACT( × 4 augment) Best Avg   | DACT( × 4 augment) Best Avg   |
|------------------------|-------------------------|-----------------------|-----------------------|------------|-----------------------|--------------------|-------------------------------|------------------------|------------------------|-------------------------------|-------------------------------|
| eil51                  | 2.82%                   | 0.23%                 | 1.06%                 | 2.35%      | 2.11%                 | 0.00%              | 1.17%                         | 0.23%                  | 0.87%                  | 0.23%                         | 0.26%                         |
| berlin52               | 6.34%                   | 0.00%                 | 0.94%                 | 5.34%      | 1.67%                 | 0.03%              | 2.57%                         | 0.00%                  | 0.53%                  | 0.00%                         | 0.00%                         |
| st70                   | 4.59%                   | 0.00%                 | 0.21%                 | 1.19%      | 2.22%                 | 0.30%              | 0.89%                         | 0.00%                  | 0.13%                  | 0.00%                         | 0.00%                         |
| eil76                  | 6.88%                   | 0.74%                 | 1.90%                 | 4.28%      | 3.35%                 | 1.49%              | 4.65%                         | 0.00%                  | 1.39%                  | 0.00%                         | 0.72%                         |
| pr76                   | 1.40%                   | 0.03%                 | 0.75%                 | 2.72%      | 2.84%                 | 19.97%             | 1.37%                         | 0.00%                  | 0.59%                  | 0.00%                         | 0.59%                         |
| rat99                  | 17.18%                  | 1.98%                 | 3.48%                 | 1.73%      | 9.50%                 | 7.51%              | 8.51%                         | 0.74%                  | 2.58%                  | 0.74%                         | 1.29%                         |
| KroA100                | 18.39%                  | 0.43%                 | 2.08%                 | 0.78%      | 79.49%                | 4.45%              | 2.08%                         | 0.41%                  | 1.13%                  | 0.00%                         | 0.21%                         |
| KroB100                | 19.97%                  | 0.26%                 | 1.17%                 | 3.91%      | 9.30%                 | 5.83%              | 5.78%                         | 0.26%                  | 0.67%                  | 0.26%                         | 0.38%                         |
| KroC100                | 22.14%                  | 0.39%                 | 3.55%                 | 4.02%      | 8.04%                 | 6.55%              | 3.17%                         | 0.39%                  | 2.56%                  | 0.00%                         | 0.77%                         |
| KroD100                | 16.33%                  | 0.57%                 | 2.69%                 | 1.61%      | 10.02%                | 8.74%              | 5.00%                         | 0.45%                  | 2.23%                  | 0.45%                         | 0.64%                         |
| KroE100                | 21.91%                  | 1.10%                 | 1.91%                 | 2.40%      | 3.10%                 | 5.97%              | 3.29%                         | 0.46%                  | 1.14%                  | 0.28%                         | 0.60%                         |
| rd100                  | 0.06%                   | 0.00%                 | 1.47%                 | 3.53%      | 1.93%                 | 0.00%              | 0.06%                         | 0.00%                  | 0.99%                  | 0.00%                         | 0.59%                         |
| eil101                 | 4.61%                   | 1.91%                 | 2.94%                 | 5.56%      | 3.97%                 | 2.07%              | 4.61%                         | 1.59%                  | 2.23%                  | 1.27%                         | 1.75%                         |
| lin105                 | 26.53%                  | 3.24%                 | 7.23%                 | 3.09%      | 32.13%                | 12.00%             | 2.48%                         | 2.69%                  | 6.37%                  | 0.00%                         | 0.89%                         |
| pr107                  | 19.76%                  | 4.04%                 | 4.97%                 | 1.74%      | 43.26%                | 5.66%              | 3.87%                         | 2.87%                  | 4.33%                  | 2.87%                         | 4.07%                         |
| pr124                  | 11.82%                  | 0.66%                 | 1.04%                 | 5.91%      | 4.41%                 | 0.29%              | 2.97%                         | 0.65%                  | 0.75%                  | 0.00%                         | 0.30%                         |
| bier127                | 20.65%                  | 2.98%                 | 4.91%                 | 3.76%      | 1.71%                 | 60.56%             | 3.48%                         | 2.01%                  | 3.39%                  | 1.71%                         | 2.43%                         |
| ch130                  | 16.53%                  | 0.77%                 | 3.64%                 | 2.85%      | 2.96%                 | 0.25%              | 4.89%                         | 0.41%                  | 2.72%                  | 0.41%                         | 1.09%                         |
| pr136                  | 9.14%                   | 2.99%                 | 6.71%                 | 5.62%      | 4.90%                 | 1.06%              | 6.33%                         | 1.45%                  | 5.65%                  | 0.58%                         | 4.58%                         |
| pr144                  | 21.30%                  | 2.30%                 | 2.93%                 | 1.28%      | 8.77%                 | 0.80%              | 1.40%                         | 2.07%                  | 2.63%                  | 0.44%                         | 1.16%                         |
| ch150                  | 21.26%                  | 0.74%                 | 1.86%                 | 3.08%      | 3.45%                 | 0.83%              | 3.55%                         | 0.58%                  | 1.77%                  | 0.87%                         | 1.23%                         |
| KroA150                | 17.80%                  | 3.32%                 | 6.12%                 | 4.03%      | 9.98%                 | 13.15%             | 4.51%                         | 1.74%                  | 4.21%                  | 0.66%                         | 5.15%                         |
| KroB150                | 20.20%                  | 2.84%                 | 4.09%                 | 5.52%      | 9.87%                 | 11.72%             | 5.40%                         | 1.64%                  | 3.22%                  | 1.64%                         | 2.45%                         |
| pr152                  | 16.20%                  | 1.84%                 | 4.45%                 | 2.92%      | 13.47%                | 4.11%              | 2.17%                         | 1.57%                  | 3.10%                  | 0.77%                         | 2.23%                         |
| u159                   | 21.97%                  | 2.50%                 | 6.08%                 | 8.79%      | 7.38%                 | 2.19%              | 7.67%                         | 1.64%                  | 5.13%                  | 1.64%                         | 2.69%                         |
| rat195                 | 25.40%                  | 4.00%                 | 4.87%                 | 2.84%      | 16.57%                | 29.06%             | 9.90%                         | 2.02%                  | 3.92%                  | 2.02%                         | 3.28%                         |
| d198                   | 13.83%                  | 7.05%                 | 9.73%                 | 1.16%      | 331.58%               | 45.98%             | 4.99%                         | 6.60%                  | 8.62%                  | 4.89%                         | 6.31%                         |
| KroA200                | 22.44%                  | 4.46%                 | 6.98%                 | 1.27%      | 15.64%                | 20.00%             | 7.01%                         | 3.05%                  | 5.43%                  | 2.33%                         | 3.01%                         |
| KroB200                | 23.69%                  | 7.61%                 | 10.90%                | 3.67%      | 18.54%                | 21.06%             | 7.05%                         | 5.46%                  | 8.27%                  | 5.23%                         | 6.39%                         |
| Avg. Gap for [50,100)  | 6.53%                   | 0.50%                 | 1.39%                 | 2.93%      | 3.61%                 | 4.88%              | 3.19%                         | 0.16%                  | 1.01%                  | 0.16%                         | 0.48%                         |
| Avg. Gap for [100,150) |                         | 1.54%                 | 3.37%                 |            | 15.29%                | 8.16%              | 3.53%                         | 1.12%                  | 2.63%                  | 0.59%                         | 1.39%                         |
|                        | 9.69%                   |                       |                       | 3.29%      |                       |                    |                               |                        |                        |                               |                               |
| Avg. Gap for [150,200] | 12.76%                  | 3.82%                 | 6.12%                 | 3.70%      | 47.39%                | 16.45%             | 5.81%                         | 2.70%                  | 4.85%                  | 2.23%                         | 3.64%                         |

Table 5: Generalization of DACT (10 runs) v.s. baselines on CVRPLIB benchmark dataset.

| Instance                   | Depot Type                 | Node Type                  | Wu et al. [11] (T=5k)   | DACT(T=5k) Best Avg   | DACT(T=5k) Best Avg   | OR-Tools   | AM-sampling (N=10k)   | POMO × 8 augment   | Wu et al. [11] (T=5k, M=100)   | DACT(T=10k) Best Avg   | DACT(T=10k) Best Avg   | DACT( × 6 augment) Best Avg   | DACT( × 6 augment) Best Avg   |
|----------------------------|----------------------------|----------------------------|-------------------------|-----------------------|-----------------------|------------|-----------------------|--------------------|--------------------------------|------------------------|------------------------|-------------------------------|-------------------------------|
| X-n101-k25                 | R                          | R                          | 7.70%                   | 1.86%                 | 3.49%                 | 6.57%      | 32.95%                | 3.64%              | 5.60%                          | 1.68%                  | 3.11%                  | 1.32%                         | 1.86%                         |
| X-n106-k14                 | E                          | C                          | 4.86%                   | 2.33%                 | 3.00%                 | 3.72%      | 6.78%                 | 1.85%              | 2.83%                          | 2.28%                  | 2.77%                  | 1.72%                         | 2.09%                         |
| X-n110-k13                 | C                          | R                          | 6.39%                   | 0.41%                 | 2.64%                 | 7.87%      | 3.15%                 | 2.05%              | 4.40%                          | 0.37%                  | 2.38%                  | 0.00%                         | 1.10%                         |
| X-n115-k10                 | C                          | R                          | 13.32%                  | 1.23%                 | 1.80%                 | 4.50%      | 7.52%                 | 3.49%              | 5.19%                          | 0.08%                  | 1.43%                  | 0.02%                         | 0.68%                         |
| X-n120-k6                  | E                          | RC                         | 16.16%                  | 3.52%                 | 6.32%                 | 6.83%      | 4.54%                 | 2.12%              | 5.56%                          | 3.49%                  | 6.01%                  | 2.71%                         | 4.00%                         |
| X-n125-k30                 | R                          | C                          | 8.79%                   | 5.50%                 | 6.71%                 | 5.63%      | 35.16%                | 7.14%              | 4.71%                          | 4.76%                  | 6.01%                  | 4.76%                         | 5.51%                         |
| X-n129-k18                 | E                          | RC                         | 11.01%                  | 4.06%                 | 5.72%                 | 8.37%      | 4.00%                 | 0.97%              | 4.63%                          | 3.98%                  | 5.28%                  | 3.48%                         | 3.91%                         |
| X-n134-k13                 | R                          | C                          | 16.06%                  | 4.85%                 | 7.17%                 | 21.61%     | 20.13%                | 4.22%              | 8.88%                          | 3.77%                  | 6.40%                  | 2.32%                         | 3.28%                         |
| X-n139-k10                 | C                          | R                          | 14.99%                  | 1.82%                 | 3.47%                 | 12.02%     | 4.30%                 | 2.28%              | 4.90%                          | 1.65%                  | 3.05%                  | 0.53%                         | 1.13%                         |
| X-n143-k7                  | E                          | R                          | 20.20%                  | 4.59%                 | 5.86%                 | 11.27%     | 8.88%                 | 2.79%              | 6.61%                          | 3.92%                  | 5.15%                  | 0.89%                         | 3.37%                         |
| X-n148-k46                 | R                          | RC                         | 16.38%                  | 4.09%                 | 5.21%                 | 7.80%      | 79.53%                | 19.88%             | 3.60%                          | 3.61%                  | 4.75%                  | 2.60%                         | 4.24%                         |
| X-n153-k22                 | C                          | C                          | 22.94%                  | 6.10%                 | 8.98%                 | 8.01%      | 78.11%                | 12.16%             | 4.53%                          | 4.63%                  | 7.62%                  | 3.22%                         | 4.26%                         |
| X-n157-k13                 | R                          | C                          | 17.15%                  | 3.24%                 | 3.78%                 | 2.57%      | 16.30%                | 2.79%              | 3.60%                          | 2.70%                  | 3.36%                  | 2.09%                         | 2.60%                         |
| X-n162-k11                 | C                          | RC                         | 19.16%                  | 2.53%                 | 3.98%                 | 6.31%      | 6.37%                 | 4.77%              | 5.26%                          | 1.82%                  | 2.68%                  | 1.10%                         | 1.57%                         |
| X-n167-k10                 | E                          | R                          | 18.52%                  | 5.03%                 | 6.36%                 | 9.34%      | 8.41%                 | 4.05%              | 8.27%                          | 4.36%                  | 5.75%                  | 4.05%                         | 4.83%                         |
| X-n172-k51                 | C                          | RC                         | 12.06%                  | 4.44%                 | 6.50%                 | 10.74%     | 85.37%                | 21.99%             | 4.36%                          | 3.66%                  | 5.42%                  | 3.37%                         | 3.78%                         |
| X-n176-k26                 | E                          | R                          | 19.49%                  | 8.53%                 | 13.23%                | 8.99%      | 20.39%                | 10.27%             | 6.16%                          | 7.74%                  | 10.91%                 | 7.74%                         | 9.13%                         |
| X-n181-k23                 | R                          | C                          | 6.27%                   | 3.27%                 | 3.84%                 | 2.94%      | 6.45%                 | 2.08%              | 2.08%                          | 3.05%                  | 3.65%                  | 1.89%                         | 2.51%                         |
| X-n186-k15                 | R                          | R                          | 17.71%                  | 7.40%                 | 9.15%                 | 7.75%      | 6.01%                 | 2.15%              | 7.65%                          | 6.80%                  | 8.01%                  | 5.01%                         | 6.10%                         |
| X-n190-k8                  | E                          | C                          | 18.64%                  | 7.00%                 | 8.69%                 | 6.53%      | 46.61%                | 9.25%              | 6.78%                          | 6.36%                  | 7.86%                  | 5.81%                         | 6.42%                         |
| X-n195-k51                 | C                          | RC                         | 17.04%                  | 6.78%                 | 8.39%                 | 13.76%     | 79.26%                | 9.23%              | 4.47%                          | 6.60%                  | 7.74%                  | 5.14%                         | 6.25%                         |
| X-n200-k36                 | R                          | C                          | 9.60%                   | 6.43%                 | 7.26%                 | 4.15%      | 26.25%                | 5.01%              | 4.26%                          | 6.30%                  | 7.08%                  | 5.54%                         | 6.07%                         |
| Avg. Gap for [100,150)     | Avg. Gap for [100,150)     | Avg. Gap for [100,150)     | 12.35%                  | 3.11%                 | 4.67%                 | 8.74%      | 18.81%                | 4.58%              | 5.17%                          | 2.69%                  | 4.21%                  | 1.85%                         | 2.83%                         |
| Avg. Gap for [150,200]     | Avg. Gap for [150,200]     | Avg. Gap for [150,200]     | 16.24%                  | 5.52%                 | 7.29%                 | 7.37%      | 34.50%                | 7.61%              | 5.22%                          | 4.91%                  | 6.37%                  | 4.09%                         | 4.87%                         |
| Avg. Gap for all instances | Avg. Gap for all instances | Avg. Gap for all instances | 14.29%                  | 4.32%                 | 5.98%                 | 8.06%      | 26.66%                | 6.10%              | 5.20%                          | 3.80%                  | 5.29%                  | 2.97%                         | 3.85%                         |

including, 1) OR-Tools [37], 2) AM-sampling [5], 3) POMO × 8 augment [8], the state-of-the-art neural construction solver, and 4) the enhanced variant of Wu et al. [11], which samples M actions to produce multiple solutions at each step and retrieves the best one as the next state. We present the performance of our DACT with and without augments in the last group of columns. For TSPLIB, we infer the first 5 instances (size &lt; 99 ) using DACT model trained on TSP50 and the remaining ones using model trained on TSP100. For CVRPLIB, we infer all instances using DACT model trained on CVRP100 since all the sizes are larger than 100. For AM-sampling and POMO, we use the trained models of our sizes which are provided by the authors. The results of Wu et al. [11] and OR-Tools are adapted from Wu et al. [11]. The gaps are calculated based on the optimal solutions provided in the datasets. And we test our DACT for 10 runs and report the average and the best gaps. We also list the average gaps for instances in different problem size intervals, i.e., [50, 100), [100, 150) and [150, 200] for TSPLIB; and [100, 150) and [150, 200] for CVRPLIB.

TSPLIB Pertaining to TSPLIB in Table 4, our DACT (T=3k) significantly outperforms Wu et al. [11] (T=3k) for all instances expect for 'rd100'. It also performs better than the two neural con-

struction solvers AM-sampling (N=10k) and POMO × 8 augment for all three problem size intervals in terms of the average gaps, where the superiority to them becomes more obvious as the problem size increases. With larger steps (T=10k), our DACT continues improving the solution qualities and outstrips all the baselines including OR-Tools and Wu et al. [11] (T=3k, M=1k), in terms of the overall average gap. Further empowered by 4 augments, our DACT consistently reduces the gaps and achieves the best performance on most instances with the lowest overall average gap.

CVRPLIB Pertaining to CVRPLIB in Table 5, the depot and the nodes follow various distributions. Though trained on uniform distribution, our DACT (T=5k) outperforms Wu et al. [11] (T=5k), ORTools, AM-sampling (N=10k), and POMO × 8 augment in terms of gap on all instances. With T=10k and 6 augments, it further reduces the overall average gap to the lowest one. This further verifies that our DACT generalizes well on real-world instances with various sizes and distributions.

Remarks It is worth noting that although POMO × 8 augment previously achieved the state-of-theart performance on synthetic instances according to Kwon et al. [8], it is still lacking in generalization on benchmark instances, whose underlying core model is AM (which showed the worst generalization performance in Table 1). Meanwhile, we can also infer from the results that the neural improvement models including Wu et al. [11] and our DACT have much better generalization capability than the neural construction ones including AM-sampling and POMO. Given the advantages of our DACT model, it achieves the new state-of-the-art generalization performance among all existing Transformer based models on these benchmark instances from TSPLIB and CVRPLIB.

## E.5 Licenses for used assets

We list the used existing assets in Table 6. All of them are open-sourced assets for academic usage. Our code is released under the MIT open-source license.

Table 6: List of used assets and the licenses.

| Asset                                                                                                               | Type                                               | License                                                                                                                                                                                                 |
|---------------------------------------------------------------------------------------------------------------------|----------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| OR-Tools [37] LKH, LKH3 [28, 36] AM[5] NLNS [27] Neural-2-Opt [23] Wu et al. [11] POMO [8] TSPLIB [38] CVRPLIB [39] | Code Code Code Code Code Code Code Dataset Dataset | Apache License, Version 2.0 Available for academic research use MIT License GPL-3.0 License No License MIT License MIT License Available for any non-commercial use Available for academic research use |
| Python numpy PyTorch tdqm tensorboard                                                                               | Code Code Code Code Code                           | Python Software Foundation License BSD License BSD License MIT License Apache License 2.0                                                                                                               |