## Comparison of Attention Mechanisms in Machine Learning Models for Vehicle Routing Problems

V. S. Krishna Munjuluri Vamsi, Yashwanth Reddy Telukuntla,

Parimi Sanath Kumar, and Georg Gutjahr

## 1 Introduction

The Vehicle Routing Problem (VRP) is a classic NP-Hard problem and one of the most studied combinatorial optimization problems. In a VRP, the objective is to select a set of minimum-cost vehicle routes through customer locations so that each route starts and ends at a common location (depot). There are different variants for VRP; in this manuscript, we consider the Capacitated Vehicle Routing Problem (CVRP), in which each of the vehicles is restricted to a certain load capacity.

VRPs are very general in the way they are defined and can aptly represent a wide variety of combinatorial optimization problems. The VRP is a very abstract problem and many real-world problems can be modeled using the idea of tour planning. It may include vaccination drives (as in [8]), catastrophe relief distribution (as in [9]), and farming and cultivation community commutes (as in [2]). Multiple methods have been proposed that take various approaches in solving VRPs [3, 14, 15, 18].

To solve VRPs, a large number of exact and heuristic algorithms have been proposed in the literature. Some of the classic exact algorithms are Dijkstra's method, Branch-and-Bound, and Dynamic Programming. Latest advancements in the indus-

Department of Computer Science and Engineering, Amritapuri, India

V. S. Krishna Munjuluri Vamsi ( B ) · P. S. Kumar e-mail: munjulurivkrishna@am.students.amrita.edu

## P. S. Kumar

e-mail: psanathkumar@am.students.amrita.edu

## Y. R. Telukuntla

Department of Electronics and Communication, Amritapuri, India e-mail: yashwanthreddy@am.students.amrita.edu

## G. Gutjahr

Center for Research in Analytics &amp; Technologies for Education, Amrita Vishwa Vidyapeetham,

Amritapuri, India

e-mail: georgcg@am.amrita.edu

'The Author(s), under exclusive license to Springer Nature Singapore Pte Ltd. 2023

P. Singh et al. (eds.), Machine Learning and Computational Intelligence Techniques for Data Engineering , Lecture Notes in Electrical Engineering 998, https://doi.org/10.1007/978-981-99-0047-3\_53

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[vamsi2023] Comparison of Attention Mechanisms in Machine Learning Models for Vehicle Routing Problems_artifacts/image_000000_fc4fb0fc4aeb2219a19ef0ff2070455128c221a7c2ce55e411fb49bbcfe9d9fb.png)

629

try saw big data-aided heuristics being used to solve the VRPs [23] and a few have also proposed a better constraint relaxation strategy to improve the robustness of heuristics and meta-heuristics to solve the CVRP [12], and we guide the readers to Sect. 2 for the traditional CVRP constraints. Although the exact algorithms provide a guarantee that the optimal solution is obtained, such algorithms are computationally very demanding and will usually not scale to problem instances of realistic size. Therefore, heuristic algorithms are often the only available option to achieve acceptable running times in practical applications.

With the recent improvements in deep learning methods such as end-to-end neural networks, it has become possible to use machine learning techniques to solve the VRP problem. Such algorithms find a solution as a permutation of nodes at any instance by making a sequence of decisions. In this way, solving the VRP can be represented by a Markov Decision Process (MDP). MDPs, in general, require To solve such an MDP, we need a framework where the natural idea is to make one decision after the other. Reinforcement Learning (RL) [16] is one such framework where an 'agent' is constantly interacting with some 'environment' and gains 'reward' based on its 'actions'. In our context, the agent will be the actual vehicle that goes and serves the customers, the environment can be the customers and their demands, and we define our reward will be the inverse of the distance traveled by taking that action. And by actions, we mean the steps taken by the vehicle to serve its customers.

Now, for the RL agent to perform better, it needs to keep track of its current environment and make informed decisions about its customers. This is where the Attention Mechanisms come in. The vehicle attends to different customer nodes differently based on how the attention layers modify the node embeddings. After a route is complete, Attention Mechanism re-computes the node embeddings because the graph structure changes considerably. So, for choosing customers to be served next, the vehicle has the best possible representation of the current distribution of customer nodes. Each attention layer has multiple heads and we can have multiple such layers (discussed in detail in Sect. 4) which naturally brings the need for optimization of these hyperparameters of the model.

The remaining of the paper is organized as follows. Section 2 gives a problem definition of the VRP. Section 3 gives some background on VRPs and how a VRP can be solved by methods from machine learning. Section 4 describes AMs in more detail and the need for comparison. In the Results section, Sect. 5, we talk about the comparison that has been performed and our findings from it. In conclusion, we summarized our findings.

## 2 Problem Definition

VRPs can be modeled in different ways. One of the common models is the vehicle flow formulation where an integer value is used to count the number of times a particular edge is traversed by the vehicle. Two indices are used to represent the edges and therefore the formulation is known as a two-index formulation. Additionally,

three-index formulations also exist where a third index is used to identify which vehicle is used by a particular edge. VRPs can also be modeled as set partitioning problems.

A two-index formulation extended from the Dantzig et al. [7] is given. Let V be the vertex set or the set of customer locations. Let the cost of going from node i to node j be represented by ci j . Here, xi j is a binary variable that has value 1 if the arc going from i to j is considered as part of the solution and 0 otherwise. Let the number of available vehicles be denoted by K and r ( S ) corresponds to the minimum number of vehicles needed to serve a set S ⊆ V of customers. Also assume that 0 ∈ V is the depot node. VRP can be formulated as

$$\text{Minimize } \sum _ { i \in V } \sum _ { j \in V } c _ { i j } x _ { i j }$$

subject to

$$\sum _ { i \in V } x _ { i j } = 1 & \quad \forall \ j \in V \ \{ 0 \} \\ \Sigma \,.. \quad \, \begin{matrix} 0 \\ 0 \\ 0 \end{matrix} \quad \begin{matrix} 0 \\ 0 \end{matrix} 0 \\ 0 \end{matrix}$$

$$\frac { } { \epsilon }$$

$$\sum _ { j \in V } x _ { i j } = 1 \ \forall \, i \in V \, \smallsetminus \{ 0 \}$$

$$\sum _ { i \in V } x _ { i 0 } & = K \\ \Sigma & = v$$

$$\sum _ { j \in V } x _ { 0 j } = K & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & && & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & &  & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & &
 & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & \\ & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & ; & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & \ & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & &. & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & _ & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & } & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & - & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & &, & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & + & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & = & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & ^ & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & b & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & | & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & &

 & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & & &$$

GLYPH&lt;6&gt;

$$\sum _ { i \notin S } \sum _ { j \in S } x _ { i j } \geq r ( S ), \ \forall S \subseteq V \ \{ 0 \}, S \neq \emptyset$$

Constraints (2) and (3) ensure that exactly one arc enters and exactly one leaves each vertex associated with a customer. Constraints (4) and (5) ensure that the number of vehicles leaving the depot is equal to the number entering. Constraint (6) ensures that the routes should be connected and that the demand on each route should not exceed the capacity of the vehicle. The capacity constraint (6) can be converted into a subtour elimination constraint (7) to get an alternative formulation. This new constraint ensures that at least r ( s ) edges leave each customer set S .

$$\sum _ { i \in S } \sum _ { j \in S } x _ { i j } \leq | S | - r ( s )$$

As discussed, VRPs can be solved by many sophisticated methods (e.g., solving VRPs by using the learning mechanism provided by neural networks), but solving VRPs was all started with a classic algorithm [11]. The algorithm proposed by them iteratively matches vertices to form a set of vehicle routes, where each iteration is called a 'stage of aggregation' (This algorithm was only able to solve small

problem instances). Over the years many heuristics have been proposed to solve large problem instances (for example, the edge exchanges method by Kindervater and Savelsbergh [11]). Meta-heuristics are also methods for solving VRPs, which eventually overtook problem-specific solution methods.

Another approach that will be considered in this paper is to represent the VRP as a sequence-to-sequence problem in a pointer network, where the input is a sequence of nodes and the output is a permutation of this sequence. Methods from machine learning are used to learn how to permute sequences in an optimal way. This approach was originally developed by Google for use in machine translation and improved by Google in 2020, which is very much used in Google's recent chatbot Meena [1]. We will now discuss this approach in more detail.

## 3 Sequence-to-Sequence Model for Solving VRPs

Pointer networks [21] free us with the limitation of output length generally depending on the input given. The labels assigned by the approximate solver to the sequence-tosequence models [19] are trained in a supervised manner. But the performance of the pointer network is only as good as the quality of the training dataset, which in turn might give us expensive solutions. To train a policy modeled by PN without supervised signals, we can use reinforcement learning (RL) algorithms. Element-wise projections will be replacing Long short-term memory (LSTM) [13] of the pointer network, which are invariant to the input order and will not introduce redundant sequential information.

Recurrent Neural Networks (RNN) is a type of Neural Network, where the output of the previous layer is taken as input to the current step/layer. In an encoderdecoder architecture, both encoder and decoder use multiple RNN layers [22] each. At first, Encoder takes the entire input sequence and churns out a fixed-length internal representation/vector; this helps the model to understand more about the context and internal dependencies associated with it. This fixed-length internal representation/vector is then passed to the decoder's first layer. The decoder layer's role is to convert the fixed-length internal representation/vector to an output sequence. The Encoder-Decoder architecture (where we encode the problem to a fixed dimension vector in some way and then decode the vector while solving the problem) generally fails to remember (short-term memory) if the given input sequence is long enough. This implies that carrying information from the previous step to the next step is challenging, and during back-propagation, RNN suffers from the vanishing gradient problem (where Gradients are used to update a neural network's weights in the learning process). The vanishing gradient problem occurs when the gradient shrinks/becomes extremely small (during back-propagation); it doesn't contribute to efficient learning. So because of this, the RNN's layers stop learning for longer sequences, thus short-term memory.

To overcome the above limitation, we can try implementing an LSTM network, where every LSTM node has a cell state/vector, which gets passed down like a

chain. The state of the cell can be modified (both add and remove). From both RNNs Sequence-to-Sequence model and Pointer Networks and LSTM's (encoderdecoder/vanilla architecture), we expect them to have the capability to save/access the information seen thus far, which in turn means that the final encoder should be having the whole input data/sequence and which might cause data loss because of having the complete input sequence as a compressed vector. Passing that single compressed vector to a decoder for deciphering is in itself a highly complex task and which again is a bottleneck. This is where Attention comes in. Every hidden state from each encoder node in attention [16] at every time step makes predictions after deciding which one is more informative and passes it on to the decoder.

## 4 Attention Mechanisms

As discussed in Sect. 1, we need a more robust way of selecting the next customers. The attention mechanism can be thought of as follows. The vehicle, after servicing a customer, needs to decide where to go next for the overall optimality of the entire fleet. At the serviced customer node, the vehicle creates a query asking its neighbors. The neighbors reply to the vehicle by passing their key and value vectors (The computation of these vectors is explained in the later paragraphs). Now, the vehicle compares the product of the query and key to the value vector, the idea of query, key and values is derived from [20]. Based on the resulting distribution, the vehicle decides which customer to serve next. This is essentially the 'neural message passing mechanism [10]' which is a good way of thinking about the attention mechanisms.

In the architecture proposed by [17], the attention mechanism is included in the ENCODE (refer Fig. 1) step of the encoder-decoder architecture and it helps in computing the node embeddings effectively (The Encoder has been explained at a

## Encoder

Fig. 1 The Encoder (figure adapted from [10])

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[vamsi2023] Comparison of Attention Mechanisms in Machine Learning Models for Vehicle Routing Problems_artifacts/image_000001_ac5508c18f873c4fcc8e5042e14e0650d6715468461084ff4b75a7538d2eeeb8.png)

greater depth in Sect. 3). These final node embeddings are used, later in the DECODE stage, to compute the context vector. Now, the keys are generated according to the embeddings (just as the case with the encoding), but the queries are based on the compatibilities (computed from the context vector). In this work, we restrict ourselves to discussing the specifics of the Attention Mechanism in the ENCODE step.

The basic building block of the Multi-Head Attention (MHA) is the scaled dotproduct attention. Each head of the MHA learns a different type of information from the graph embeddings and the node embeddings (updated by the previous layer of MHA). The scaled-dot-product compares how similar the query ( Q ) is relative to the key ( K ) of the neighboring node, and the output vector is compared against the values ( V ) vector of the same neighbor. And to achieve this, we generate the Q K V , , for the current node and, from that, calculate the compatibility vector. This compatibility vector, aptly named, tells us how compatible the neighbor is with respect to the query that is generated. From the compatibility vector, we take a softmax of the compatibility vector (scaling) and multiply it with V (dot-product). Essentially, this is the scaled-dot-product attention. Now, for MHA, we do this as many times as the number of heads in the model and concatenate all the outputs to finally project it back onto the embedding space.

Each attention layer consists of an MHA and a fully connected Feed Forward (FF) layer. And we can have many layers of attention but it can quickly get really expensive in terms of computing power. Notionally, the more the number of heads and the number of layers, the better the model should perform because it has a better chance to take into account the surrounding nodes and design routes based on REINFORCE policy (a reference to RL algorithm).

In the equations that follow, we just give the formulation for the ideas that are described in the above paragraphs.

$$q _ { i m } ^ { ( l ) } = W _ { m } ^ { Q } h _ { i } ^ { ( l - 1 ) }, k _ { i m } ^ { ( l ) } = W _ { m } ^ { K } h _ { i } ^ { ( l - 1 ) }, v _ { i m } ^ { ( l ) } = W _ { m } ^ { V } h _ { i } ^ { ( l - 1 ) },$$

$$u _ { i j m } ^ { ( l ) } = ( q _ { i m } ^ { ( l ) } ) ^ { T } k _ { j m } ^ { l },$$

$$a _ { i j m } ^ { ( l ) } = \frac { e ^ { u _ { i j m } ^ { ( l ) } } } { \Sigma _ { j ^ { \prime } = 0 } ^ { h } e ^ { u _ { i j ^ { \prime } m } ^ { ( l )$$

$$\text{MHA} _ { i } ^ { ( l ) } ( h _ { 0 } ^ { ( l - 1 ) }, \dots, h _ { n } ^ { ( l - 1 ) } ) = \sum _ { m = 1 } ^ { M } W _ { m } ^ { O } h _ {$$

$$h _ { i m } ^ { ^ { \prime } ( l ) } = \sum _ { j = 0 } ^ { - j = \upsilon ^ { \prime } } a _ { i j m } ^ { ( l ) } v _ { j m } ^ { ( l ) }, \quad \quad \quad$$

$$\hat { h } _ { i } ^ { ( l ) } = \tanh ( h _ { i } ^ { ( l - 1 ) } + \text{MHA} _ { i } ^ { ( l ) } ( h _ { 0 } ^ { ( l - 1 ) }, \dots, h _ { n }$$

$$F F ( \hat { h } _ { i } ^ { ( l ) } ) = W _ { 1 } ^ { F } \text{ReLu} ( W _ { 0 } ^ { F } \hat { h } _ { i } ^ { ( l ) } + b _ { 0 } ^ { F } ) + b _$$

$$\hat { h } _ { i } ^ { ( l ) } = \tanh ( h _ { i } ^ { ( l - 1 ) } + F F ( \hat { h } _ { i } ^ { ( l ) } ) ).$$

In Eq. (8), we compute query, keys, and values at node i based on the node embeddings provided by the previous layer ( l -1 . Here, ) W Q , W K , and W V are the parameters that learn how to compute queries, keys, and values, respectively. And, these parameters stay independent (different for different layers of attention).

In Eq. (9), u i jm ( ) l is the compatibility vector that is obtained by looking at how similar the query generated by node i at the m th head in the l th layer (which is q im ( ) l ) is to the values with respect to the key of the neighboring node j at the m th head in the l th layer (which is k l j m ). This computes compatibility to all the surrounding nodes j in some neighborhood of i and can hence be expensive.

In Eq. (10), the attentions, for the nodes j in some neighborhood of node i at the m th head in l th layer, a i j m ( ) l are obtained by taking the softmax of the compatibilities.

In Eq. (11), h im GLYPH&lt;9&gt; ( ) l is obtained by taking the product of the attentions with the values vector v ( ) l j m of the node j at the m th head in the l th layer.

In Eq. (12), the final MHA output is obtained by taking the product with the weights matrix W O m with the output of the previous equation. This is the step where all the learnings from the heads are merged and projected back onto the embedding space that the customer nodes are in.

As we have seen in Eqs. (8) through (12), we can have as many layers and as many heads as we like. This is the Multi-Head Attention sublayer. In MHA, one way of thinking about the heads is that each of the heads gets better at identifying a certain aspect of the neighborhood. And we concatenate all of this and project the vector back onto the embedding space. And each of the layers refines the learnings from the previous layer because the next layer takes its output and computes all the compatibilities, attentions, and products using a different set of weights for each layer. From this, we can already see that with these two variables m and l , there are already a lot of possible architectures for our Attention Mechanism. And apparently, the more their number is, the better the Attention Mechanism is at attending to its neighboring nodes.

However, when applying this approach with real data, the number of heads and layers is limited. This is because the number of parameters in the layers increases exponentially, and after a certain point, training the network becomes infeasible. Also, the more parameters we have, the less time can be spend on finding optimal values for them. So, there is a need to compare and look for optimal values of m and l .

While we mainly worked on MHA, the Attention Mechanism is incomplete without the FF sublayer which is described in Eqs. (13) through (15). This constitutes the ENCODE part which tries to get the best node embeddings by stacking multiple attention layers.

As it was already established, there is a trade-off when selecting the number of layers and heads: the more layers and heads we use, the more complex our model becomes and the harder it gets to train the model. This is also related to the so-called bias-variance trade-off in machine learning, where increasing the flexibility of a modelwilldecrease the bias but will increase the variance. And since the performance of the model can usually be represented as a combination of bias and variance, there

exists an optimal level of complexity for the model where neither the bias nor the variance is too large.

In other words, when designing attention mechanisms for the VRP, multiple topologies are possible and it is not a prior obvious which of them is best. The different topologies can be described in terms of the numbers m and l and we would like to explore which values for m and l lead to good performance.

## 5 Simulation Results

Since we are trying to explore the performance of the models if we about the same time to train them, we fix the training epochs to be a constant for each of the models. For this comparison, the number of epochs is fixed to be 400 in batches of 128 for all of the models irrespective of the number of customer nodes. We trained the models to solve CVRPs of small (approximately 20 and 30 customer nodes) and medium (50 customer nodes) sizes. For each problem size, we trained the models with 1 head, 4 heads, and 8 heads. The testing data is the benchmark datasets from Christofides [5, 6] and Augerat [4]. Even though the models can handle dynamic inputs (i.e., with changing demands and the customer locations), the datasets are static and don't change with time. In other words, all the customer locations and their demands are known beforehand.

All the simulations were done on Google's Colab Notebooks with one Tesla K80 GPU. The implementation was done in Python. Tensorflow and Keras were the primarily used frameworks.

The results are shown in Table 1. The comparison is done with constant epochs for models with 1 head, 4 heads, and 8 heads. The cost of the solution generated by the models is recorded in the table and the optimal cost is shown next to it (the optimal cost is known for the selected benchmark datasets [4-6]).

## 6 Discussion and Conclusion

The OR community is showing much interest in solving VRPs. This is done to utilize the workforce maximally while reducing the costs of travel or transportation in a variety of applications. Usually, it is not feasible to apply exact methods that take too much time which should be started from scratch even if the problem instance changes slightly. Machine learning methods have much potential to approximate the costs of the routes efficiently and to get good generalizations if the distribution of the customers at testing is similar to that of the customers at training. In particular, research has shown that RNNs and LSTMs on their own are too naive for the job and that introducing RL can lead to better solutions.

To improve those RL models, advanced attention mechanisms have been proposed. We have motivated how there are different topologies possible for the AMs

Table 1 Comparison results

| DataSet                       |   1 Head |   4 Heads |   8 Heads |   Optimal cost |   Nodes |
|-------------------------------|----------|-----------|-----------|----------------|---------|
| Augerat Set P (P-n21-k8)      |      369 |       400 |       267 |            211 |      20 |
| Chirstofides Set E (E-n22-k4) |      722 |       589 |       583 |            375 |      21 |
| Augerat Set P (P-n22-k2)      |      359 |       397 |       270 |            216 |      21 |
| Augerat Set P (P-n22-k8)      |      862 |       747 |       666 |            603 |      21 |
| Augerat Set B (B-n31-k5)      |      751 |       971 |       814 |            672 |      30 |
| Augerat Set A (A-n33-k6)      |     1217 |      1157 |      1079 |            742 |      32 |
| Chirstofides Set E (E-n33-k4) |     1237 |      1201 |      1285 |            835 |      32 |
| Augerat Set P (P-n51-k10)     |     1009 |      1220 |      1068 |            741 |      50 |
| Christofides Set E (E-n51-k5) |      771 |       863 |       813 |            521 |      50 |
| Augerat Set B (B-n51-k7)      |     1466 |      1242 |      1294 |           1032 |      50 |
| Chirstofides et al. (CMT 1)   |      771 |       863 |       813 |            524 |      50 |
| Augerat Set B (B-n52-k7)      |     1419 |      1620 |      1917 |            747 |      51 |
| Augerat Set P (P-n55-k7)      |      887 |      1037 |       826 |            568 |      54 |

and why there is a need for a thorough comparison. In our work, we have tested the model by varying different hyperparameters like the number of heads and the number of attention layers that make up the entire AM. We observe that small CVRP instances are already benefiting from more heads. The larger instances do not show a clear trend, possibly due to the limited training we could provide. The smaller CVRP instances section is a clear demonstration of the potential benefits the Attention Mechanisms can add to the Deep Reinforcement Learning-based solutions to VRPs, in general.

In the future, we would like to perform a more exhaustive testing that takes into account a lot more values for the number of heads and for the number of layers. Depending upon the availability, we would love to test the model for CVRP instances with 100 customer nodes and above, which is where the more advanced architectures is expected to significantly outperform more simple models.

## References

- 1. Adiwardana D, Luong MT, So DR, Hall J, Fiedel N, Thoppilan R, Yang Z, Kulshreshtha A, Nemade G, Lu Y et al (2020) Towards a human-like open-domain chatbot. arXiv:2001.09977
- 2. Ajayan S, Dileep A, Mohan A, Gutjahr G, Sreeni K, Nedungadi P (2019) Vehicle routing and facility-location for sustainable lemongrass cultivation. In: 2019 9th international symposium on embedded computing and system design (ISED). IEEE, pp 1-6
- 3. Anbuudayasankar S, Ganesh K, Lee TR (2011) Meta-heuristic approach to solve mixed vehicle routing problem with backhauls in enterprise information system of service industry. In: Enterprise information systems: concepts, methodologies, tools and applications. IGI Global, pp 1537-1552
- 4. Augerat P (1995) Approche polyèdrale du problème de tournées de véhicules. PhD thesis, Institut National Polytechnique de Grenoble-INPG
- 5. Christofides N, Eilon S (1969) An algorithm for the vehicle-dispatching problem. J Oper Res Soc 20(3):309-318
- 6. Christofides N, Mingozzi A, Toth P (1981) Exact algorithms for the vehicle routing problem, based on spanning tree and shortest path relaxations. Math Program 20(1):255-282
- 7. DantzigG,FulkersonR,JohnsonS(1954)Solutionofalarge-scaletraveling-salesmanproblem. J Oper Res Soc Am 2(4):393-410
- 8. Gutjahr G, Krishna LC, Nedungadi P (2018) Optimal tour planning for measles and rubella vaccination in Kochi, South India. In: 2018 international conference on advances in computing, communications and informatics (ICACCI). IEEE, pp 1366-1370
- 9. Gutjahr G, Viswanath H (2020) A genetic algorithm for post-flood relief distribution in Kerala, South India. In: ICT systems and sustainability. Springer, Berlin, pp 125-132
- 10. Kool W, Van Hoof H, Welling M (2018) Attention, learn to solve routing problems! arXiv:1803.08475
- 11. Laporte G (2009) Fifty years of vehicle routing. Transp Sci 43(4):408-416
- 12.
- Letchford AN, Salazar-González JJ (2019) The capacitated vehicle routing problem: stronger bounds in pseudo-polynomial time. Eur J Oper Res 272(1):24-31
- 13. Libovick' y J, Helcl J (2017) Attention strategies for multi-source sequence-to-sequence learning. arXiv:1704.06567
- 14. Malairajan R, Ganesh K, Punnniyamoorthy M, Anbuudayasankar S (2013) Decision support system for real time vehicle routing in Indian dairy industry: a case study. Int J Inf Syst Supply Chain Manag (IJISSCM) 6(4):77-101
- 15. Mohan A, Dileep A, Ajayan S, Gutjahr G, Nedungadi P (2019) Comparison of metaheuristics for a vehicle routing problem in a farming community. In: Symposium on machine learning and metaheuristics algorithms, and applications, Springer, Berlin, pp 49-63
- 16. Nazari M, Oroojlooy A, Snyder LV, Takáˇ c M (2018) Reinforcement learning for solving the vehicle routing problem. arXiv:1802.04240
- 17. Peng B, Wang J, Zhang Z (2019) A deep reinforcement learning algorithm using dynamic attention model for vehicle routing problems. In: International symposium on intelligence computation and applications. Springer, Berlin, pp 636-650
- 18. Rao TS (2019) An ant colony and simulated annealing algorithm with excess load VRP in a FMCG company. In: IOP conference series: materials science and engineering, vol 577. IOP Publishing, p 012191
- 19. Sutskever I, Vinyals O, Le QV (2014) Sequence to sequence learning with neural networks. In: Advances in neural information processing systems, pp 3104-3112
- 20. Vaswani A, Shazeer N, Parmar N, Uszkoreit J, Jones L, Gomez AN, Kaiser Ł, Polosukhin I (2017) Attention is all you need. In: Advances in neural information processing systems, pp 5998-6008
- 21. Vinyals O, Fortunato M, Jaitly N (2015) Pointer networks. arXiv:1506.03134
- 22. Yu Y, Si X, Hu C, Zhang J (2019) A review of recurrent neural networks: Lstm cells and network architectures. Neural Comput 31(7):1235-1270
- 23. Zheng S (2019) Solving vehicle routing problem: A big data analytic approach. IEEE Access 7:169565-169570