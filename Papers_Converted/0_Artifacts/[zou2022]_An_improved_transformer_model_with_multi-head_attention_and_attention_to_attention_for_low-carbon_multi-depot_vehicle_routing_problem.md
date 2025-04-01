ORIGINAL RESEARCH

## An improved transformer model with multi-head attention and attention to attention for low-carbon multi-depot vehicle routing problem

Yang Zou 1 · Hecheng Wu 1 · Yunqiang Yin 2 · Lalitha Dhamotharan 3 · Daqiang Chen 4 · Aviral Kumar Tiwari 5

Accepted: 17 May 2022 / Published online: 20  June  2022

©The Author(s), under exclusive licence to Springer Science+Business Media, LLC, part of Springer Nature 2022

## Abstract

Low-carbon logistics is an emerging and sustainable development industry in the era of a low-carbon economy. The end-to-end deep reinforcement learning (DRL) method with an encoder-decoder framework has been proven effective for solving logistics problems. However, in most cases, the recurrent neural networks (RNN) and attention mechanisms are usedinencodersanddecoders,whichmayresultinthelong-distancedependenceproblemand the neglect of the correlation between query vectors. To surround this problem, we propose an improved transformer model (TAOA) with both multi-head attention mechanism (MHA) and attention to attention mechanism (AOA), and apply it to solve the low-carbon multi-depot vehicle routing problem (MDVRP). In this model, the MHA and AOA are implemented to solve the probability of route nodes in the encoder and decoder. The MHA is used to process different parts of the input sequence, which can be calculated in parallel, and the AOA is used to deal with the deficiency problem of correlation between query results and query vectors in the MHA. The actor-critic framework based on strategy gradient is constructed to train model parameters. The 2opt operator is further used to optimize the resulting routes. Finally, extensive numerical studies are carried out to verify the effectiveness and operation efficiency of the proposed TAOA, and the results show that the proposed TAOA performs better in solving the MDVRP than the traditional transformer model (Kools), genetic algorithm (GA), and Google OR-Tools (Ortools).

Keywords End-to-end deep reinforcement learning · Transformer model · Multi-head attention mechanism · Low-carbon multi-depot vehicle routing problem · GA algorithm

B

Yunqiang Yin yinyq@uestc.edu.cn

- 1 College of Economics and Management, Nanjing University of Aeronautics and Astronautics, Nanjing 210016, China
- 2 School of Economics and Management, University of Electronic Science and Technology of China, Chengdu 611731, China
- 3 University of Exeter Business School, University of Exeter, Exeter EX4 4PU, UK
- 4 School of Management and E-Business, Zhejiang Gongshang University, Hangzhou 310018, China
- 5 Rajagiri Business School (RBS), Kochi 682039, India

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000000_8f55984d3766c2d4981a227719100e689583ade256b607d43ef61dfb8da5f810.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000001_bbbf274096ac079e0e9db3ddac3eaa7bcefe910a972f6dc179595bab3ae81de3.png)

## 1 Introduction

With the rapid development of e-commerce, the increasing quantity of goods promotes the evolution of the logistics distribution system (Roy et al., 2020). However, the increasing carbon emission in logistics will result in severe environmental pollution. Under this circumstance, low-carbon transportation methods are urgently needed to reduce carbon emissions in the logistics field (Camacho-Vallejo &amp; López-Vera, 2021; Sbihi &amp; Eglese, 2010). Many problems have been investigated to realize low-carbon transportation, such as low-carbon multi-depot vehicle routing problem (MDVRP) (Marrekchi et al., 2021), heterogeneous fixed fleet vehicle routing problem (HFFVRP) (Penna et al., 2019), and vehicle routing problem with time windows (VRPWTW) (Brandão de Oliveira &amp; Vasconcelos, 2010). Among them, the MDVRP is much closer to real-world applications since it is essentially a study of multidepot distribution to customers (Galindres-Guancha et al., 2018).

The low-carbon MDVRP is a variant of MDVRP considering the factors such as fuel consumption and carbon emission. The MDVRP was initially proposed by Gillett et al. (1976). As a variant of VRP, the MDVRP has proven to be an NP-hard problem. Traditional methods including exact and heuristic algorithms are proposed to solve this problem. Exact algorithms can provide optimal solutions, but the corresponding calculation is complex and time-consuming. As a result, most researchers prefer to use heuristic algorithms for solving such problems (Kuo &amp; Wang, 2011; Li et al., 2018; Salhi et al., 2014). The heuristic algorithm is a method that searches solution space via heuristic rules. It can attain optimal solutions in some cases. However, it cannot guarantee the quality of solutions in general. At the same time, it suffers from high computational costs, especially in large-scale trials. Compared with these methods, deep reinforcement learning technology (DRL) (Mizutani &amp; Dreyfus, 2017) takes advantage of the fast speed and strong generalization ability. This is due to that the encoder-decoder process in DRL can contribute to avoiding double-counting when handling instances with the same probability distribution. Therefore, the encoder-decoder approach serves as an excellent candidate for solving the traveling salesman problem (TSP), vehicle routing problem (VRP), knapsack problem, and other combinatorial optimization problems. Although DRL can effectively solve the VRP, it still has some limitations. For example, the recurrent neural network (RNN) used in encoders-decoders cannot avoid longdistance dependence problems (Sayli &amp; Yılmaz, 2017). This will finally lead to gradient disappearance and explosion. Therefore, Bresson and Laurent (2021) proposed a vehicle route planning method based on the transformer model. In this model, an attention mechanism was implemented in the encoding and decoding process. Numerical results show that the gradient explosion can effectively alleviate and significantly reduce computational costs. However, the correlation between the encoding vector and the given attention query is not considered in the attentional mechanism, which will lead to the failure of the decoder. To surround this problem, we exploit the AOA (Huang, et al., 2019) in the transformer to take the correlation between attentional queries and results into consideration.

This paper aims at arousing people's attention to the feasibility and effectiveness of deep reinforcement learning in the context of the MDVRP. The main contributions are summarized as follows.

- 1. Based on the traditional transformer (Kools), we propose an improved transformer model with both MHA and AOA, which we referred to as TAOA, for solving the low-carbon MDVRP. The MHA and AOA are added to the encoder-decoder. The MHA takes advantage of its strong robust information extraction capability, and the AOA can efficiently

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000002_7cfe831845fb495a4af9a05d0cefc0d5555206e7271e9f7ed4cec6ac964144cb.png)

improve the decoder's accuracy by considering the correlation between query vectors and query results in MHA.

- 2. The actor-critic network is implemented in the transformer network training to improve the model's accuracy. The exponential moving average method is used to construct the critic network, reducing the computational complexity.
- 3. Numerical studies on real-world cases are conducted to verify the effectiveness and operation efficiency of the proposed TAOA by comparing it with some state-of-art methods, which provides a guide for constructing high-quality route planning of the low-carbon MDVRP.

This paper is organized as follows. In Sect. 2, a brief review of relevant work is given. In Sect. 3, the low-carbon MDVRP considering fuel consumption and carbon emissions is formally introduced. In Sect. 4, the details of the TAOA are presented. In Sect. 5, numerical studies are conducted. Finally, in Sect. 6, the conclusions and future work are presented.

## 2 Literature review

Minimum travel distance and minimum logistics cost are often set as optimization goals in most existing studies on the MDVRP. However, as more attention has been paid to energy conservation and emission reduction, reducing the flue consumption of vehicles and cutting down on carbon emission has become a hot topic in both academic circles and environmental protection agencies. In general, low-carbon MDVRP refers to an MDVRP considering the flue consumption of vehicles and carbon emission. Methods including exact and heuristic algorithms have been developed to solve the low-carbon MDVRP. However, these two kinds of algorithms suffer from high calculation costs for large-scale problems. As a result, an algorithm capable of fast calculation in practical applications is urgently needed.

Deep reinforcement learning (DRL) was implemented in solving the TSP for the first time in 2015. In this work, Vinyals et al. (Vinyals et al., 2015) proposed a direction-oriented network for solving the problem, where the encoder-decoder model entirely depended on the current input city coordinates and numbers. Instead of using a weighted sum to fuse the hidden layer vector of the encoder into the semantic vector of each decoder, the attention mechanism in this network was used as a pointer to select the elements in the input sequence as the output sequence. Following this framework, many methods based on pointer-network have been successfully used in solving the TSP, VRP, multi-objective TSP, and some other combinatorial optimization problems (Ma et al., 2019; Yang, 2021).

In the DRL-based encoder-decoder algorithm, limited by the supervised training method used by the pointer network, it's challenging to obtain a large number of sample labels used for supervised training. As a result, the pointer network is not suitable for larger-scale VRP. To surround this problem, Bello et al. (2019) proposed a method combining neural combinatorial optimization and DRL to solve large-scale TSP effectively. Nazari et al. ( 2018) further used the pointer network for solving the TSP with time-varying input elements. The authors divided the input into two parts. One is the static input (customer location/coordinate), while the other is the dynamic input (customer demand). Moreover, a one-dimensional convolution layer was used to encode, and the DRL was used for training. Compared with Bello's model, this model can efficiently balance optimization capability and training time.

Comparedwiththetraditionalencoder-decodermodel,thetransformermodelhasachieved great success in natural language processing in recent years (Vaswani et al., 2017). The transformer model discards the convolutional neural network (CNN) and the RNN. It is

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000003_8f55984d3766c2d4981a227719100e689583ade256b607d43ef61dfb8da5f810.png)

a network mechanism built on an attention mechanism, which can establish long-distance dependencies and realize parallel computing to speed up training (Huang et al., 2020). In addition, the MHA of the transformer can better extract the deep features of the problem. As a result, the transformer model has been applied to solve the VRP. Deudon et al. (2018) developed an improved pointer network model, where the feature vectors of nodes in the transformer model were calculated by the MHA, the DRL method was used for training the model, and the weight of attention was calculated by the traditional pointer network. Experiment results showed that this method could effectively improve the quality of the solution. Kool et al. (2019) proposed an MHA technique to enhance the traditional pointer network to solve the VRP. In this method, MHA was used in the coding layer, the selfattention calculation method of the transformer model was applied, and more calculation layers were added to improve the performance of the model. Experiment results showed that this model could effectively solve the TSP, capacitated VRP (CVRP), orienteering problem (OP), and prize collecting TSP (PCTSP) compared with other pointer-network-based models (Bello et al., 2019; Ma et al., 2019; Nazari et al., 2018; Vinyals et al., 2015; Yang, 2021). In addition, Kool et al. (2019) improved the reinforcement learning training algorithm. The authors designed a rollout baseline to replace the critic neural network, where the model with the best performance was selected as the baseline strategy, and the action selection was carried out in a greedy approach. Experiment results showed that the convergence ability of the training method is better than that of the traditional method, and the optimization performance of this method exceeds that of other end-to-end models.

Based on these observations, we propose a novel TAOA with a simple 2-opt local search for solving the low-carbon MDVRP. Based on the transformer introduced by Kool et al. (2019), the AOA mechanism is introduced into the encoder-decoder to calculate the correlation between attention results and query results, which is beneficial for considering more information dimensions in the decoder and improving the performance of the model. A simple 2-opt local search is performed based on the resulting solution, which can make up for the deficiency of decoder error decoding in the transformer and optimize the resulting solution of the low-carbon MDVRP.

## 3 Problem description and model formulation

/negationslash

Each depot d ∈ D is associated with a set of homogeneous vehicles Kd ={1,…,K } with d capacity Q . The vehicles are used to deliver goods to the customers. Each customer j ∈ N has a request of the goods with a total amount of q j . The distance between any nodes i and j ∈ N is denoted by di j .

The low-carbon MDVRP studied in this paper can be formally defined as follows. Given a set of depots D ={1, …,D} and a set of customers C ={1,…,C} with known geographical locations. Let N = D ∪ C ∪ { 0 d , ( n + 1 ) d } d ∈ D and A = { ( , i j ) : i , j ∈ N i , = j } be the node set and arc set, respectively, where 0 d and ( n + 1 ) d denote the original and end node of depot d ∈ D .

Assigning a vehicle for serving the customers incurs a fixed cost p . Travelling along each arc ( , i j ) ∈ A incurs a unit transportation cost ν , a unit fuel consumption cost λ , and a unit carbon emission cost µ . The fuel conversion factor is denoted by δ .

The objective is to determine the distribution route planning consisting of the vehiclecustomer assignment strategy and service routes of assigned vehicles to minimize the sum of the vehicle assignment cost, travel cost, fuel consumption cost, and carbon emission cost. A

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000004_f7b7feee35b594f1dcddf609872d0963384ed7b8916feb02895b4131918ec781.png)

solution to the low-carbon MDVRP is said to be feasible if (1) The number of vehicles used in each depot cannot exceed the number of vehicles available at the depot; (2) Each vehicle of a depot will start from its original node and return to the corresponding end node; (3) Each customer will be served by exactly one vehicle; (4) The total demand of customers in a route cannot exceed the maximum capacity of the vehicle.

## 3.1 The calculation of fuel consumption and carbon emissions

Fuel cost is a significant factor in logistics distribution. Proper distribution route planning can reduce the cost of fuel consumption (Sahin et al., 2009). In general, the carbon emission of vehicles is inevitable in driving. It is related to the vehicle speed, distance, and load. Xiao and Zhao (2012) established a fuel consumption model using statistical methods to consider vehicle mileage and loading capacity, which can accurately predict fuel consumption. In this paper, we use Xiao's method to calculate fuel consumption, which can be formulated as follows.

$$\rho ( Q _ { 1 } ) = \rho ^ { 0 } + \frac { \rho ^ { * } - \rho ^ { 0 } } { Q } Q _ { 1 }$$

where ρ ∗ and ρ 0 are fuel consumption rates of the vehicle under full load and empty load, Q denotes the maximum capacity of the vehicle. ρ( Q 1 ) gives the fuel consumption per unit distance (km) under the condition of vehicle load Q 1. Here we set ρ ∗ = 2 and ρ 0 = 1.

When a vehicle travels from customer i ∈ N to customer j ∈ N , denote by ρ i j = ρ ( hij ) the fuel consumption per unit mileage along arc ( , i j ) , where hij is the actual load of the vehicle along arc ( , i j ) . Then, the fuel consumption cost of the vehicle along arc ( , i j ) can be calculated using the following expression.

$$C _ { i j } ^ { f } = \lambda \rho _ { i j } d _ { i j } \, \left ( \, \right ) \, \right ) \, \right ) \, \right ) \, \right ) \, \left ( 2 \right )$$

The carbon emission of a vehicle is proportional to flue consumption (Facts, 2005). Thus, the carbon emission cost of the vehicle along arc ( , i j ) can be as follows.

$$C _ { i j } ^ { e } = \mu \delta \rho _ { i j } d _ { i j } \\. \quad \. \. \. \. \. \. \. \. \. \.$$

where λ is the unit fuel consumption cost, µ is the unit carbon emission cost, and δ is the fuel conversion factor. Here, as suggested in IPCC 2006 (Eggleston et al., 2006), we set λ = 40, µ =10, and δ = 2 73. .

## 3.2 Low-carbon MDVRP

To model the low-carbon MDVRP, we introduce the following decision variables.

xi jk : a binary variable equal to 1 if and only if vehicle k traverses arc ( , i j )

Lik : the total load of vehicle k after leaving node i

Using the above decision variables, we formulate the mathematical programming model of the low-carbon MDVRP as follows.

$$\min Z & = v \sum _ { d \in D } \sum _ { k \in K _ { d } } \sum _ { j \in C } x _ { 0 j k } + p \sum _ { d \in D } \sum _ { k \in K } \sum _ { i \in ( 0 _ { d } ) \cup C } \sum _ { j \in C \cup \{ ( n + 1 ) _ { d } \} } d _ { i j } x _ { i j k } \\ & \quad + ( \lambda + \mu \delta ) \sum _ { d \in D } \sum _ { k \in K } \sum _ { i \in ( 0 _ { d } ) \cup C } \sum _ { j \in C \cup \{ ( n + 1 ) _ { d } \} } d _ { i j } x _ { i j k } ( \rho ^ { 0 } + \frac { \rho ^ { * } - \rho ^ { 0 } } { Q } L _ { i k } ) \quad ( 4 )$$

$$\in D \overleftarrow { k \in K } \, i \in \overleftarrow { \{ 0 _ { d } \} \cup C } \, j \in C \cup \overleftarrow { \{ ( n + 1 ) _ { d } \} } \, \nu$$

subject to

$$\text{subject to} \\ \sum _ { d \in D } \sum _ { k \in C \cup [ ( n + 1 ) _ { d } ] } x _ { 0 j k } = \sum _ { j \in C _ { d } ] \cup C } x _ { i, n + 1, k } \leq 1, \forall i \in C & ( 5 ) \\ \sum _ { i \in C _ { d } ] \cup C } x _ { i j k } = \sum _ { j \in C _ { d } ] \cup C } x _ { j i k }, \forall k \in K _ { d }, d \in D & ( 6 ) \\ \sum _ { i \in C _ { 0 } ] \cup C } x _ { i j k } = \sum _ { i \in C \cup [ ( n + 1 ) _ { d } ] } x _ { j i k }, \forall k \in K _ { d }, d \in D, j \in C & ( 7 ) \\ L _ { i k } \leq L _ { j k } + q _ { j } + Q ( 1 - x _ { i j k } ), \forall k \in K _ { d }, d \in D, ( i, j ) \in A & ( 8 ) \\ x _ { i j k } \in \{ 0, 1 \}, L _ { i k } \geq 0, \forall k \in K _ { d }, d \in D, i, j \in C \cup \{ 0, ( n + 1 ) _ { d } \} & ( 9 ) \\ \text{The objective function (4)} is to minimize the sum of the vehicle assignment cost, travel
cost. \text{ fuel consumption cost and carbon emission cost. where the first term is the vehicle}$$

The objective function (4) is to minimize the sum of the vehicle assignment cost, travel cost, fuel consumption cost and carbon emission cost, where the first term is the vehicle assignment cost, the second term denotes the travel cost and the third term gives the sum of the fuel consumption cost and carbon emission cost. Constraints (5) guarantee that each customer is severed by exactly one vehicle. Constraints (6) and (7) are the flow-conservation constraints. Constraints (8) define the total load of a vehicle after leaving a node. Finally, constraints (9) define the ranges of the decision variables.

## 4 The improved transformer model with both multi-head attention and attention to attention (TAOA)

This section formally describes the encoder-decoder-based TAOA for solving the low-carbon MDVRP.Intheencoder,theMHAisfirstlyusedforextractionoftheinputfeaturevectorofthe instance and calculation of the weight of attention. Then, the AOA generates the information vector and attention gate according to the weight of the attention and attention query. By incorporating the attention gate with the information vector, the AOA attention results can thus be obtained. In the decoder, the current context vector is composed of the context vector of the last moment, the current input feature vectors, and the AOA attention results. The corresponding attention weight vector is obtained by the self-attention mechanism. The probability distribution of customer nodes is then calculated from the normalized weight. Afterward, the subsequent customer nodes are selected by a sampling strategy. Moreover, the TAOA is trained using the actor-critic (Haarnoja et al., 2018) in the DRL, which combines the advantages of the value function algorithm and strategy gradient algorithm. In the training process, actor and critic update parameters alternately according to gradient descent, which can enhance the algorithm performance. By designing a suitable transformer network, we can convert the low-carbon MDVRP into an "offline training, online decision" model. The overall encoder-decoder model structure is shown in Fig. 1.

## 4.1 Encoder with AOA

As illustrated in the left part of Fig. 1, the encoder maps the input sequence to a vector of higher dimensions. The encoder is composed of MHA, AOA, feed-forward, and add &amp; norm. The MHA is used to facilitate the neural network to capture richer feature information. The process of AOA mechanism is involved to reduce the possibility of error decoding in the decoding stage. Feed-forward is composed of two layers of full connection. It uses the activation function to learn more comprehensive information about the feature vector. Add &amp;

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000005_f7b7feee35b594f1dcddf609872d0963384ed7b8916feb02895b4131918ec781.png)

Fig.1 The overall structure of the TAOA

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000006_4138929d1beba727d373b2248af864207c6215b4ff7ef3c9900a3f27ac1c442c.png)

norm consists of add and norm. Add is a residual connection. It is implemented to make the network only focus on the current difference in the multi-layer network. Norm refers to layer normalization. It is used to convert the input of neurons in each layer into the same mean and variance so as to speed up the convergence. The details of the encoder are elaborated as follows.

## 4.1.1 Multi-head attention

TheMHAfocusesonessentialfeaturesintheinputsequence,whichcanaccuratelycapturethe characteristic information of the input sequence. As a result, resource allocation optimization canberealized. A MHAassignsdifferentweightstodifferent feature vectors in global features to extract local features. At the same time, the joint feature vector sequences, including global and local features, will be generated.

According to Fig. 1, we perform the linear transformation of the W S , W U and W V on the hidden layer of input sequence at first and obtain the query vector S , key vector U , and value vector V respectively, where W S , W U and W V are the matrix parameters of the

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000007_b2c4730f7da95a53e63d40a40d7f83e804bd7207c7d66ad7af337a8e22c233f5.png)

Fig. 2 Scaled-dot product attention mechanism and AOA mechanism. a Scaled-dot product attention mechanism. b AOA mechanism

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000008_f55e835c47a8293f9f82f8b958aef5bb3f0de0efee4db5aef8cec704ad4a6dc3.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000009_8079db7d63b1410f67a5648ac660562fa4e6d3da36d7a064650aa752620ea833.png)

corresponding vectors. Secondly, the similarity between S and V is calculated. The soft-max function is then used to normalize the similarity. We can obtain the final attention based on the weighted sum of the similarity and the corresponding V . Many calculation methods of attention weight, including the multilayer perceptron approach, bilinear approach, dot product and scaled-dot product, have been reported (Kalaivaani et al., 2021). Among them, the scaled-dot product method can effectively solve the gradient disappearance problem. Based on this observation, we calculate the weight in the MHA using the scaled-dot product method. The framework of the scaled-dot product method is shown in Fig. 2a. The weight vector can be calculated using the expression shown as follows.

$$\text{vector can be calculated using the expression shown as follows.} \\ a _ { i j } = \frac { s _ { i } \cdot u _ { j } } { \sqrt { d } }, \ \bar { a } _ { i j } \frac { \exp ( a _ { i j } ) } { \sum _ { j } \exp ( a _ { i j } ) }, \ \ s _ { i } \in S, u _ { j } \in U, \ i, j = 1, \dots, 8 \\ \text{head} _ { i } = \sum _ { j } a _ { i j } v _ { j }, \ \ v _ { j } \in V, \ i, j = 1, \dots, 8 \\ \text{MultiHead} ( s _ { i }, u _ { j }, v _ { j } ) = C o n t a c t ( \text{head} _ { 1 }, \dots, \text{head} _ { 8 } ) W ^ { S } \\ \text{where there are 8 heads of the MHA, s _ { i } \text{ is the ith query vector,} u _ { j } \text{ is the jth key vector, head} _ { i }$$

where there are 8 heads of the MHA, s i is the i th query vector, u j is the j th key vector, headi denotes the similarity between s i and u j , . and Multi Head ( s i , u j , v j ) splices these headi in columns and then performs linear transformation with a new weight matrix W S to get the final attention output.

## 4.1.2 Attention to attention

The scaled-dot product attention mechanism in the MHA returns a weighted vector about the input even if none of the input vectors satisfy the query vector. The AOA uses elementwise multiplication to integrate the attention gate ( G ) and information vector ( I ) to solve this unreasonable phenomenon, which can get the attention weight and the output. The structures of the scaled-dot product attention mechanism and AOA are shown in Fig. 2a, b, respectively.

As seen in Fig. 2a, the scaled-dot product attention mechanism is an addressing process by giving a task-related query vector S ; the attention value is obtained by calculating the

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000010_7cfe831845fb495a4af9a05d0cefc0d5555206e7271e9f7ed4cec6ac964144cb.png)

attention distribution of key vector U and S and attaching it to value vector V . From Fig. 2b, we observe that the AoA generates an "information vector" I and an "attention gate" G by performing two independent linear transforms to the weight results obtained in the MHA. They are calculated by the results of attention and query vectors as follows.

$$\cdot & \cdot & \cdot \\ I = w _ { s } ^ { i } s + w _ { v } ^ { i } v + b ^ { i }, w _ { s } ^ { i }, w _ { v } ^ { i } \in R ^ { D \times D } & & ( 1 1 ) \\ G = \tau ( w _ { s } ^ { g } s + w _ { v } ^ { g } v + b ^ { g } ), w _ { s } ^ { g }, w _ { v } ^ { g } \in R ^ { D \times D } \\ \text{the query vector in M} \Delta \text{ with dimension } D \text{ in is the attention result } \tau \text{ is the}$$

where s is the query vector in MHA with dimension D , v is the attention result, τ is the sigmoid activation function, w i s , w i v and b i are the matrix parameters in the "information vector", and w g s , w g v and b g are the matrix parameters in the attention gate, respectively.

In addition, the AOA uses element-by-element multiplication combined with the attention gate and information vector to obtain AOA attention results.

$$A O A ( v, S ) = \tau ( W _ { s } ^ { g } S + W _ { v } ^ { g } v + b ^ { g } ) \odot \tau ( W _ { s } ^ { i } S + W _ { v } ^ { i } v + b ^ { i } ) \quad \quad ( 1 2 )$$

where /circledot denotes the element-by-element multiplication that is calculated using the classical attention mechanism.

## 5 Feed-forward layer

The feed-forward layer fuses all the features of the input sequence. Given the feature vector of the customer node i in the l th sub-layer, the calculation formula is as follows.

$$h _ { i } ^ { l } = W ^ { x } \left [ x _ { i }, d _ { j }, D _ { i } \right ] + b ^ { x }, i = 1, \dots, n$$

where d j is the eigenvector of the j -th parking lot, Di is the demand for xi , and W x and b x are the trainable matrix parameters, respectively. The fully connected feed-forward layer needs to apply batch regularization and skip connection to the vector h l i , and then perform normalization processing. The calculation is as follows.

$$h _ { i } ^ { l } = B N ^ { l } ( F F ^ { s k i p } ( B N ^ { l } \left ( h _ { i } ^ { l - 1 } + A O A ( M H A ( h _ { 1 } ^ { l - 1 }, \dots, h _ { n } ^ { l - 1 }, m a s k ) ) \right ) ) ) \quad ( 1 4 )$$

where l denotes the number of encoder layers that do not share parameters, MHA is the MHAoperator of the sub-layer, in which we assume there are 8 heads corresponding to the batch normalization operation of the first layer. AOA processes the results of the attention mechanism and the correlation between queries. FF skip denotes the feed-forward operator and skip connection, which is the full connection in Fig. 1 and calculated as follows.

$$\text{FF} ^ { \text{skip} } ( h _ { i } ) = W _ { 2 } \max ( 0, W _ { 1 } h _ { i } + b _ { 1 } ) + b _ { 2 } + h _ { i }$$

As shown in Eq. (15), the feed-forward layer includes two full connections, where W 1, W 2, b 1, and b 2 are the network parameters. Its purpose is to use the activation function and linear transformation to obtain more comprehensive information about the feature vector, which can make the expression of the encoded feature vector more accurate.

## 5.1 Decoder with AOA

The decoder architecture is shown in the left part of Fig. 1. The decoder is used to export the corresponding node probability distribution at each time step. The decoder is composed

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000011_8f55984d3766c2d4981a227719100e689583ade256b607d43ef61dfb8da5f810.png)

of MHA, AOA, MHA, add &amp; norm, and feed-forward layer. The functions of AOA, feedforward, add &amp; norm are consistent with the encoder in Sect. 4.1. The difference between the decoder and encoder depends on the two MHA applied in the model. Masked operation is adopted in the first MHA layer so that the model can focus on the previously output information. The second MHA (self-attention) is used to calculate the weight relationship between the output context vector in the encoder and the output vector in the decoder at the last time. The decoder is introduced as follows.

## 5.1.1 Multi-head attention

The input of the decoder is composed of h graph , h last and l t . h graph is the average hidden layer of all nodes of the encoder, h last is the node embedding at the last time, and l t gives the total load and position of the vehicle at the current time t . Given the node coordinate sequence, the decoder employs a sampling strategy and provides the travel route with a lower total cost. We set a mask vector and mark the visited customer nodes to ensure that each node only appears once in the travel route. The weight of the selected nodes at the next time step is set to negative infinity. The expression of the context vector at any time t is as follows.

$$C _ { t } & = \begin{cases} \ f _ { c o n c a t } [ h _ { g r a p h } ; l _ { t } ; h _ { l a s t } ], t \geq 1 \\ f _ { c o n c a t } [ h _ { g r a p h } ; l _ { 0 } ; h _ { l a s t } ], t = 1 \end{cases} \\ \sum \text{concurrent the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of thesum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum ofthe sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sumof the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sums of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of each sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum of the sum at the moment.$$

where f concat represents the cascading function of the hidden layer. The context vector obtained is used in the MHA to get the weight vector. The calculation of the MHA is the same as that of the MHA in Sect. 4.1.

## 5.1.2 Attention to attention

The MHA calculates the weight vector of C t . To solve the unreasonable phenomenon of the MHA mentioned in Sect. 4.1, the AOA is used to calculate the weight between C t and the hidden layer vector of all nodes at time t in the decoder. Then, the calculated weight is transformed into a context vector and sent to the self-attention layer. The calculation method is shown as follows.

$$\overline { C } _ { t } = A O A ( M H A ( C _ { t }, h _ { t }, m a s k ) ) \quad \quad ( 1 7 ) \\ \cdot \dots \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot$$

where h t is the node hiding layer at time t , and mask is the mask vector marked for the visited nodes.

## 5.1.3 Self-attention

Self-attention is the attention mechanism for calculating the weights between C t internals. The scaled-dot product attention is used to calculate the weight between the query vector C t and the key vector h i . The dot product is employed in the calculation. Finally, the normalization is performed on the results. The calculation method is shown as follows.

/negationslash

$$u _ { t } ^ { i } & = \begin{cases} \frac { W ^ { q } \bar { C } _ { t } W ^ { k } h ^ { i } } { \sqrt { d _ { k } } }, i f i \neq \pi ( k ), k < i \\ - \infty, o t h e r w i s e \end{cases}, i \in N \quad \quad ( 1 8 ) \\. \cdots \cdots \quad. \quad. \quad. \cdots \cdots \cdots$$

where h i is the hidden vector of the customer node i , dk denotes the dimension of the hidden vector, and π ( k ) is the set of nodes that have been visited prior to customer k. The node k &lt; i

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000012_0fb022d941524a9b4c7bd317daacef63633e083be114596190f9e13105e15e1c.png)

means that node i is a customer node that has not been visited and can be regarded as the next node to be selected.

## 5.1.4 Feed-forward layer

The feed-forward layer mainly uses linear activation function and linear transformation to strengthen the expression ability of feature vectors. The detailed calculation method is the same as the one in the Eq. (15). The vector of the feed-forward output is normalized using the soft-max function. The next customer node to be visited is selected according to the probability distribution. The calculation method is shown as follows.

$$y _ { t } = s o f t m a x ( u _ { t } ) \quad \ \ \ \ \ ( 1 9 ) \\ - \ \dots \ - \ - \ \dots \ \dots \ \dots$$

where ut is the weights in Eq. (17). The common strategies for the selection of the next customernodeincludegreedy,exhaustivesearch,andsampling.Sufferingfromtheexhaustive strategy, which is used to find the optimal solution by listing all possible solutions, the calculation is usually time-consuming. A greedy strategy is used to search for the nodes with the lowest cost and connect them to form the optimum travel route. The sampling strategy is employed for finding the optimum solution randomly. Compared with the exhaustive strategy and the greedy strategy, the sampling strategy takes advantage of less calculation time and better performance. Therefore, we adopt a sampling strategy to select the next customer node to be visited in the decoding stage.

## 5.2 Actor-critic training process

It takes a lot of time for the TAOA to train network parameters in the training stage. However, when the model training is completed, the prediction results can be obtained quickly in the test stage. We adopt the advantage actor-critic (A2C) algorithm in intensive learning to train the model. Compared with the traditional reinforcement algorithm (Powell, 2016), the A2C algorithm can optimize the critic network and be used to solve the high variance problem. Thus, the quality of the solution can be effectively improved. The specific training process is shown in Algorithm 1.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000013_b2c4730f7da95a53e63d40a40d7f83e804bd7207c7d66ad7af337a8e22c233f5.png)

Here θ a and θ c are parameters that need to be trained in the actor-network and the critic network, respectively. epoch represents the iteration times, I is the generated problem instance, baselinei is the estimated value of critic network constructed by exponential moving average method, Li and ll i are the total cost and the node probability distribution of the actor-network output, respectively. Step 5 calculates the gradient of the actor-network parameter θ a and updates θ a according to the direction of gradient descent. Similarly, in step 6, the gradient of the comment network parameter θ c is calculated and updated according to the direction of gradient descent.

## 6 Experiments

This section conducts extensive numerical studies on six classes of randomly generated instances to verify the proposed model. We performed all the experiments on a PC equipped with six Intel Core i7-10,750 H CPUs at 2.60 GHz and 16 GB RAM, running in the PyCharm Community 2020.3 environment.

## 6.1 Experimental dataset

The test data in this paper comes from six classes of randomly generated instances. Each data class includes instance class, number of depots, number of customers, number of vehicles, maxcapacity,andcustomerdemand.Theseparametersareconsideredtoevaluatethediversity of the algorithm with different depots, different customers, and different vehicles, which are introduced in the following part. The number of depots in instance classes 1, 3 and 5 is set

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000014_733798cf69d0fff1547810539ed127ece703279a1e6a0847506a9a40226420bf.png)

Table 1 The characteristic of the randomly generated instance classes

|   Instance class |   Number of depots |   Number of customers |   Number of vehicles |   Max capacity | Customer demand   |
|------------------|--------------------|-----------------------|----------------------|----------------|-------------------|
|                1 |                  1 |                    20 |                    5 |              1 | [1/30,1/3]        |
|                2 |                  3 |                    20 |                    5 |              1 | [1/30,1/3]        |
|                3 |                  1 |                    50 |                    6 |              2 | [1/40,1/4]        |
|                4 |                  3 |                    50 |                    3 |              2 | [1/40,1/4]        |
|                5 |                  1 |                   100 |                    6 |              3 | [1/50,1/5]        |
|                6 |                  3 |                   100 |                    3 |              2 | [1/50,1/5]        |

to 1, and 3 for instance classes 2, 4 and 6. The number of customers is set to 20, 50 and 100 for every two sets of instance class. The customer demand is related to the maximum capacity of vehicles. The condition customer*demand &lt; vehicle*max-capacity should be met. According to the details mentioned above, for instance classes 1 and 2, the number of customers, the max capacity and the customer demand are set to 20, 1 and a random value between 1/30 and 1/3, respectively. For instance classes 3 and 4, the number of customers, the max capacity and the customer demand are set to 50, 2 and a random value between 1/40 and 1/4, respectively. While for instance classes 5 and 6, the number of customers and the customer demand are set to 100 and a random value between 1/50 and 1/5, respectively. The max capacities of vehicles in instance classes 5 and 6 are set to 3 and 2, respectively. Besides, the coordinates of the locations of the customers and depots are randomly chosen from [0,1]. The detailed information is shown Table 1.

As in Kool et al. (2019), to construct the learning model, we randomly generated 128,000 data for each instance class, where 128,000 data are used for training and 10,000 data are used for testing.

## 6.2 Parameter settings

The relevant parameters of the deep reinforcement learning model significantly affect the solution quality of the problem. Most of the parameters in our proposed model and training settings used in this work are similar to the ones in Kool et al. (2019), which have been proved to solve the VRP effectively. The hyperparameters of the TAOA and the search range of training parameters are listed in Table 2.

Table 2 Parameter settings of the proposed model

| Hyperparameters   | Value   | Training parameter   | Value   |
|-------------------|---------|----------------------|---------|
| Epoch             | 20      | tanh_clipping        | 10      |
| Batch size        | 512     | Learning rate        | 1e - 4  |
| Batch steps       | 2500    | Encode layers        | 3       |
| Seed              | 123     | warmup_beta          | 0.8     |
| Optimizer         | Adam    | embed_dim            | 128     |

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000015_b2c4730f7da95a53e63d40a40d7f83e804bd7207c7d66ad7af337a8e22c233f5.png)

As shown in Table 2, the hyperparameters are used to define the training method, e.g., batch size refers to the data size of each batch, batch steps refer to the number of batches, one epoch is equal to the product of batch size and batch steps, the seed is the parameter to generate training dataset, and the optimizer is used to update the network parameter of the model. The initial epoch, batch size, batch steps, and seed are set to 20, 512, 2500, and 123, respectively. Each batch contains 2500 data, and the batch steps are 512, so one epoch contains 1,280,000. The task of the optimizer is to calculate the gradient of the loss function in each epoch and update the parameters (Bock &amp; Wei, 2019). The Adam optimizer (Bock &amp; Wei, 2019) combines the advantages of both AdaGrad (Ward et al., 2018) and RMSProp (Kurbiel &amp; Khaleghian, 2017). It has a fast convergence speed. Therefore, we use the Adam optimizer to train the model. The training parameter corresponds to the setting of neural network parameters, e.g., the learning rate is used to control the model learning progress, encode layers represents the number of encoder layers in the model, embed\_dim is a dimension to extract feature information to hidden vector, warmup\_beta is a learning rate optimization method, tanh\_clipping is a gradient norm set in gradient clipping. The initial learning rate, encode layers, embed\_dim, warmup\_beta, and tanh\_clipping are set to 0.0001, 3,128,0.8, and 10, respectively.

## 6.3 Algorithm comparison

This section focuses on assessing the effectiveness and operation efficiency of the proposed TAOA through algorithm comparison.

## 6.3.1 Effectiveness of the TAOA

To verify the effectiveness of the TAOA, we compare it with the traditional transformer (Kools) (Kool et al., 2019), the traditional genetic algorithm (GA) (Potvin, 1996) and the Google OR-Tools (Ortools) (Kruk, 2018). The main characteristics of these algorithms are listed in Table 3.

For each compared algorithm, we present the average total cost for each instance class including 10 sets of data. In the evaluation of the performance of the TAOA and Kools under

Table 3 The characteristics of the compared algorithms

| Model   | Characteristics                                                                                                                                                                                                                                                                                                  |
|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kools   | 1. The model framework is transformer, which belongs to the DRL algorithm 2. Advantages: in the end-to-end model, the algorithm performance is more prominent, and it adopts offline training and online decision-making 3. Disadvantages: the attention mechanism does not consider the correlation between the |
| GA      | 1. A heuristic algorithm 2. Advantages: simple search process, good parallelism and scalability 3. Disadvantages: the coding process is complicated while the search speed is slow                                                                                                                               |
| Ortools | 1. Google's open-source optimization toolbox 2. Advantages: fast solving speed, suitable for large-scale problems 3. Disadvantages: it cannot be applied to problems with complex scenarios                                                                                                                      |

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000016_7cfe831845fb495a4af9a05d0cefc0d5555206e7271e9f7ed4cec6ac964144cb.png)

Table 4 The comparison results of average total cost

| IC   | Method   | Method   | Method     | Method    | Method    | Method       | Method   | Method   |
|------|----------|----------|------------|-----------|-----------|--------------|----------|----------|
|      | TAOA (S) | TAOA (G) | TAOA(mean) | Kools (S) | Kools (G) | Kools (mean) | GA       | Ortools  |
| 1    | 405.7    | 425.7    | 415.7      | 429.1     | 447.6     | 438.35       | 430.1    | 492.3    |
| 2    | 401.7    | 415.5    | 408.6      | 419.5     | 425.9     | 422.7        | 449.2    | 486.5    |
| 3    | 509.5    | 506.3    | 507.9      | 567.0     | 560.4     | 563.7        | 514.9    | 570.6    |
| 4    | 488.6    | 492.0    | 490.33     | 551.2     | 545.1     | 548.1        | 556.1    | 558.2    |
| 5    | 639.9    | 656.6    | 668.2      | 686.2     | 670.5     | 678.3        | 666.9    | 688.5    |
| 6    | 764.5    | 753.6    | 759.1      | 782.3     | 763.5     | 772.9        | 771.3    | 819.9    |

IC is the instance class, TAOA(G) and TAOA(S) denote that the greedy and Sample strategies are used to select nodes according to the probability distribution in decoding in the TAOA, respectively. Similar method is used for the Kools(G) and Kools(S). TAOA (mean) represents the average total cost of TAOA(G) and TAOA(S), and similarly, Kools(mean) is the same different strategies, greedy and sample strategies are used to select nodes according to the probability distribution in decoding. The comparison results are summarized in Table 4, where the minimum average total cost is marked in bold.

It is evident that the smaller the average total cost is, the better the optimization effect is. From Table 4, we can observe that the TAOA(S) and TAOA(G) perform the best in all instance classes compared to the Kools(G), Kools(S), Ortools and GA. Among them, the performance of the TAOA(S) is generally better than that of the TAOA(G). This is because the sampling strategy has great randomness, which can provide the opportunity for obtaining high-quality solutions. However, the greedy strategy tends to fall into a local optimum during the optimization process. As a result, the optimal solution may not be ideal. Furthermore, the TAOA (mean) performs better than the Kools (mean) in all instances. Experimental results show that the TAOA can effectively improve the performance of the traditional transformer (Kools). With the increasing number of client nodes, the advantages of the TAOA(G) over the Ortools become more obvious. Overall, these observations validate the effectiveness of the TAOA to address the low-carbon MDVRP.

To further analyze the cost of fuel and carbon emissions obtained from the TAOA, we compare the TAOA, Kools, Ortools and GA in terms of the average cost of fuel and carbon emissions, where TAOA and Kools adopt the sampling strategy. The comparison results are depicted in Fig. 3.

As can be seen from Fig. 3, the TAOA performs best among all instance classes. In addition, the number of depots and vehicles has a certain impact on the average cost of fuel and carbon emissions. According to the experimental results of the instance classes 1 and 2, it can be found that with the same vehicle capacity and same number of customers, the average cost of fuel and carbon emission in instance class 2 with 3 depots is significantly less than that in instance class 1. This is because the number of depots increases while they are widely distributed at the same time. It is more convenient and faster to allocate vehicles from the depots to serve customers, which can reduce the average cost of fuel and carbon emissions. Similarly, the results of instance classes 3 and 4 are the same. However, comparing the experimental results of instance classes 5 and 6, there are more depots in instance class 6 with smaller capacity, and the average cost of fuel and carbon emissions is also higher. This is because the fuel cost coefficient of the vehicles is in inverse proportion to the capacity.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000017_b2c4730f7da95a53e63d40a40d7f83e804bd7207c7d66ad7af337a8e22c233f5.png)

Fig. 3 The average cost of fuel and carbon emissions of TAOA and GA, Ortools, and Kools on instance classes 1, 2 3, 4, 5 and 6

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000018_c9d724a9211f8aa3e59bd7ec186b328f8a2f5f9e615943cd234c4fbd0e7dc570.png)

Figure 3 also shows that with the change in the customer number, the cost of the TAOA is always the smallest and the performance is relatively stable.

The above observations highlight the superiority of the TAOA for solving the low-carbon MDVRP, which yields more reliable solutions that achieve the best performance compared with the Kools, Ortools and GA. This further demonstrates that it is beneficial to use AOA to calculate the similarity between context vectors in decoding.

## 6.3.2 Operation efficiency of the TAOA

In this section, we verify the operation efficiency of the TAOA through three experiments. In the first experiment, we assess the validity of the 2opt operator for improving the obtained soultion. Since instance classes 1, 3, 6 and instance classes 2, 4, 5 are similar, we only report the comparison results on instance classes 1, 3 and 6 in Fig. 4.

The results in Fig. 4 clearly demonstrate that the 2opt strategy is markedly useful. Indeed, the 2opt strategy reduces the average total costs by 7.91%, 6.98% and 3.09% on instance classes 1, 3 and 6, respectively. Compared with instance classes 1 and 3, we can see that the 2opt strategy achieves more reduction in average total cost. The reason behind this may be that the area of the instances with 20 customers is smaller than that of the instances with 50 customers, and it is easier to find better solution by performing the 2-opt strategy.

The first experiment shows that the TAOA algorithm can be well combined with optimization strategies. Besides, the 2opt operator can achieve local optimal search for improving the performance of the TAOA. In the second experiment, we show how the parameter epoch of the TAOA affects the total cost. Similar to the first experiment, we report the total cost on instance classes 1, 3 and 6 along with the variation of the epoch in Fig. 5.

From Fig. 5, we can observe that no matter how many customers are, the total cost of the TAOA gradually even out. Especially when epoch = 0, the total cost is tremendous, demonstrating that the effect of the TAOA at this time is not very ideal. However, as the number of epochs increases, the total cost converges quickly, which evens out at about 20 epochs. This observation shows that the convergence of the TAOA is perfect. Furthermore, this demonstrates that the complexity of the TAOA will not increase with the growth of the problem scale, and it is suitable for large-scale problems.

Similar to the second experiment, in the third experiment, we show how the problem size in terms of the number of customers affects the average computation time of the TAOA by comparing it with the Kools, Ortools, and GA on instance classes 1, 3, and 6. Each instance class includes 10 sets of data. The comparison results are depicted in Fig. 6.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000019_f7b7feee35b594f1dcddf609872d0963384ed7b8916feb02895b4131918ec781.png)

Fig. 4 The total cost between the TAOA and TAOA + 2opt on instance classes 1, 3 and 6: The a -c are the experimental results of the TAOA; d f -are experimental results of the TAOA + 2opt

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000020_0affb76e198a5cf46dbfe39b71ad4ac1290093b6918bee27ab15a440a65f0cdb.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000021_3da7f2fdd884de6dff68d1a5c1ed9d278020fa2bd2a8da1512aad49468247899.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000022_a4b88f1708eea660a705fc482d3794f1f0aecb51ec4f1ff770feb9d4b04faa7f.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000023_30c32989b891cb16e67a34e13f0bed8799d19dede151334c1bf203320dc19e12.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000024_b49f9cbf76da03477bd9fbe69a6e171cba9279afb718a194a5b776975d7a39e6.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000025_da4c6655b392226150b99ae760c4503471fa13df64bd5c57b8415128ea3c1461.png)

Fig. 5 The total cost of the TAOA on instance classes 1, 3 and 6

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000026_755c96853004d7f1f69dc59c5ad1426bc8f8338fd168ce7370b86ae6795fef49.png)

As can be seen in Fig. 6, with the increase in the number of customers, the average computation time of the TAOA is much lower than that of the GA, nearly the same as that of the Kools, and a litter larger than that of the Ortools. In particular, the average computation time of the TAOA increases slowly as the number of customers increases, which demonstrates that the proposed TAOA can be applied for solving large-scale low-carbon MDVRP.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000027_b2c4730f7da95a53e63d40a40d7f83e804bd7207c7d66ad7af337a8e22c233f5.png)

Fig. 6 The average computation time of the TAOA and GA, Ortools, and Kools on instance classes 1, 3, and 6

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000028_cc07cb557ccd34a9921f0977769f4ace4d6bf145a81898a7780dba11701e0ad6.png)

## 7 Conclusion

In this paper, we develop a novel transformer framework-based TAOA for solving the lowcarbon MDVRP that takes the fuel consumption and carbon emission costs into account. In the developed TAOA, the AOA is added into the encoder and decoder, and the actorcritic network is used to train parameters in the transformer network, among which the critic network is constructed by the exponential moving average method. Extensively numerical studies are conducted to the effectiveness and efficiency of the proposed TAOA, and the results demonstrate that the proposed TAOA achieves the best performance by comparing with the Kools, Ortools and GA.

Several issues are interesting for future research. First, it is promising to develop an advanced transformer framework-based model to further improve the performance. Second, it is interesting to further extend our model to solve other variants of the MDVRP or other combinatorial optimization problems. Future research may also apply the developed TAOA for solving the multi-objective low-carbon MDVRP.

Acknowledgements This work was supported in part by the National Natural Science Foundation of China under grant number 71971041; in part by the Outstanding Young Scientific and Technological Talents Foundation of Sichuan Province under grant number 2020JDJQ0035; and in part by the Major Program of National Social Science Foundation of China under Grant 20&amp;ZD084.

## References

- Bello, I., Pham, H., Le, Q.V. et al. (2019). Neural combinatorial optimization with reinforcement learning. In 5th International conference on learning representations , ICLR 2017-Workshop track proceedings.

Bock,S., &amp; Wei, M. G. (2019, July). A proof of local convergence for the Adam optimizer. In 2019International joint conference on neural networks (IJCNN) (pp. 1-8).

Brandão de Oliveira, H. C., &amp; Vasconcelos, G. C. (2010). A hybrid search method for the vehicle routing problem with time windows. Annals of Operations Research, 180 , 125-144.

Bresson, X., &amp; Laurent, T. (2021). The transformer network for the traveling salesman problem . arXiv preprint arXiv:2103.03012.

Camacho-Vallejo, J. F., López-Vera, L., et al. (2021). A tabu search algorithm to solve a green logistics bi-objective bi-level problem. Annals of Operations Research, 12 (4), 1-27.

Deudon, M., Cournut, P., Lacoste, A. et al. (2018). Learning heuristics for the tsp by policy gradient. In International conference on the integration of constraint programming, artificial intelligence, and operations research (pp. 170-181).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000029_7cfe831845fb495a4af9a05d0cefc0d5555206e7271e9f7ed4cec6ac964144cb.png)

Eggleston, H. S., Buendia, L., Miwa, K. et al. (2006). 2006 IPCC guidelines for national greenhouse gas inventories .

Facts, E. (2005). Average carbon dioxide emissions resulting from gasoline and diesel fuel , United States Environmental Protection Agency, Seattle, Wash., USA

Galindres-Guancha, L. F., Toro-Ocampo, E. M., &amp; Rendón, R. A. (2018). Multi-objective MDVRP solution considering route balance and cost using the ILS metaheuristic. International Journal of Industrial Engineering Computations, 9 (1), 33-46.

Gillett, B. E., &amp; Johnson, J. G. (1976). Multi-terminal vehicle-dispatch algorithm. Omega, 4 (6), 711-718. Haarnoja, T., Zhou, A. et al. (2018). Soft actor-critic algorithms and applications . arXiv preprint arXiv:1812. 05905.

Huang, L. et al. (2019). Attention on attention for image captioning. In Proceedings of the IEEE/CVF international conference on computer vision .

Huang, Z., Liang, D., Xu, P. et al. (2020). Improve transformer models with better relative position embeddings . arXiv preprint arXiv:2009.13658.

Kalaivaani, P., Sathishkumar, V. E., Hatamleh, W. A., et al. (2021). Advanced lightweight feature interaction in deep neural networks for improving the prediction in click through rate. Annals of Operations Research, 11 , 1-15.

Kool, W., Hoof, H. V., &amp; Welling, M. (2019). Attention, learn to solve routing problems! In: 7th International conference on learning representations .

Kruk, S. (2018). Practical python AI projects: Mathematical models of optimization problems with Google OR-tools , Apress.

Kuo, Y., &amp; Wang, C. (2011). A variable neighborhood search for the multi-depot vehicle routing problem with loading cost. Expert Systems with Applications, 39 (8), 6949-6954.

Kurbiel, T., &amp; Khaleghian, S. (2017). Training of deep neural networks based on distance measures using RMSProp . arXiv preprint arXiv:1708.01911.

Li, J., Wang, R., Li, T., et al. (2018). Benefit analysis of shared depot resources for multi-depot vehicle routing problem with fuel consumption. Transportation Research Part d: Transport and Environment, 59 , 417-432.

Ma, Q., Ge, S., He, D. et al. (2019). Combinatorial optimization by graph pointer networks and hierarchical reinforcement learning . arXiv preprint arXiv:1911.04936.

Marrekchi, E., Besbes, W., Dhouib, D., et al. (2021). A review of recent advances in the operations research literature on the green routing problem and its variants. Annals of Operations Research, 304 , 529-574.

Mizutani, E., &amp; Dreyfus, S. (2017). Totally model-free actor-critic recurrent neural-network reinforcement learning in non-Markovian domains. Annals of Operations Research, 258 , 107-131.

Nazari, M., Oroojlooy, A., Takáˇ c, M. et al. (2018). Reinforcement learning for solving the vehicle routing problem. In: Advances in neural information processing systems .

Penna, P. H. V., Subramanian, A., Ochi, L. S., et al. (2019). A hybrid heuristic for a broad class of vehicle routing problems with heterogeneous fleet. Annals of Operations Research, 273 , 5-74.

Potvin, J. Y. (1996). Genetic algorithms for the traveling salesman problem. Annals of Operations Research, 63 , 337-370.

Powell, W. B. (2016). Perspectives of approximate dynamic programming. Annals of Operations Research, 241 , 319-356.

Roy, J., Pamuˇ car, D., &amp; Kar, S. (2020). Evaluation and selection of third party logistics provider under sustainability perspectives: An interval valued fuzzy-rough approach. Annals of Operations Research, 293 , 669-714.

Sahin, B., Yilmaz, H., Ust, Y., et al. (2009). An approach for analysing transportation costs and a case study. European Journal of Operational Research, 193 (1), 1-11.

Salhi, S., Imran, A., &amp; Wassan, N. A. (2014). The multi-depot vehicle routing problem with heterogeneous vehicle fleet: Formulation and a variable neighborhood search implementation. Computers &amp; Operations Research, 52 , 315-325.

Sayli, M., &amp; Yılmaz, E. (2017). Anti-periodic solutions for state-dependent impulsive recurrent neural networks with time-varying and continuously distributed delays. Annals of Operations Research, 258 , 159-185.

Sbihi, A., &amp; Eglese, R. W. (2010). Combinatorial optimization and green logistics. Annals of Operations Research, 175 (1), 159-175.

Vaswani, A., Shazeer, N., Parmar, N., et al. (2017). Attention is all you need. In: Advances in neural information processing systems (pp. 5998-6008).

Vinyals, O., Fortunato, M., &amp; Jaitly, N. (2015). Pointer networks .

- Ward, R., Wu, X., &amp; Bottou, L. (2018). from any initialization

arXiv preprint arXiv:1506.03134. Adagrad stepsizes: Sharp convergence over nonconvex landscapes, . arXiv preprint arXiv:1806.01811

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000030_8f55984d3766c2d4981a227719100e689583ade256b607d43ef61dfb8da5f810.png)

Xiao, Y., Zhao, Q., et al. (2012). Development of a fuel consumption optimization model for the capacitated vehicle routing problem. Computers &amp; Operations Research, 39 (7), 1419-1431.

- Yang, H. (2021). Extended attention mechanism for TSP problem. In 2 021 International joint conference on neural networks (IJCNN).

Publisher's Note Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[zou2022]_An_improved_transformer_model_with_multi-head_attention_and_attention_to_attention_for_low-carbon_multi-depot_vehicle_routing_problem_artifacts/image_000031_0fb022d941524a9b4c7bd317daacef63633e083be114596190f9e13105e15e1c.png)