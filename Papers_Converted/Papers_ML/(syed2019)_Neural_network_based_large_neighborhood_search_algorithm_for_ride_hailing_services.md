See discussions, stats, and author profiles for this publication at: https://www.researchgate.net/publication/335521123

## Neural Network Based Large Neighborhood Search Algorithm for Ride Hailing Services

Chapter in Lecture Notes in Computer Science · August 2019

![Image](image_000000_425462ce9648eef157fe14391cbf46d9dded623a7fa364a3902057039174a8e2.png)

1

## Neural Network based Large Neighborhood Search Algorithm for Ride Hailing Services

Arslan Ali Syed , Karim Akhnoukh , Bernd Kaltenhaeuser , and Klaus 1 2 3 Bogenberger 4

Mobility Services, Bavairan Motor Works, Germany arslan-ali.syed@bmw.de

2

Technical University of Munich, Germany karim.akhnoukh@tum.de

3

Baden-Wuerttemberg Cooperative State University, Germany bernd.kaltenhaeuser@dhbw-vs.de

- 4 University of Federal Armed Forces Munich, Germany klaus.bogenberger@unibw.de

Abstract. Ride Hailing (RH) services have become common in many cities. An important aspect of such services is the optimal matching between vehicles and customer requests, which is very close to the classical Vehicle Routing Problem with Time Windows (VRPTW).

With the emergence of new Machine Learning (ML) techniques, many researches have tried to use them for discreet optimization problems. Recently, Pointer Networks (Ptr-Net) have been applied to simpler Vehicle Routing Problem (VRP) with limited applicability [14]. We add fixed slots to their approach to make it applicable to RH scenario. The number of slots can vary without retraining the network. Furthermore, contrary to reinforcement learning in [14], we use supervise learning for training. We show that the presented architecture has the potential to build good vehicle routes for RH services. Furthermore, looking at the effectiveness of Large Neighbourhood Search(LNS) for VRPTW, we combine the approach with LNS by using the trained network as an insertion operator. We generate examples from New York Taxi data and use the solutions generated from LNS for training. The approach consistently produces good solutions for problems of sizes similar to the ones used during training, and scales well to unseen problems of relatively bigger sizes.

Keywords: Long Short Term Memory Networks · Large Neighborhood Search · Machine Learning · Discrete Optimization · Ride Hailing

## 1 Introduction

The large scale availability of high speed wireless internet and the widespread usage of smartphones has marked the beginning of many new and innovative transportation services. A lot of focus is now put on viewing people's mobility as a service. Among such services, Ride Hailing (RH) has gained a lot of attention, where a user can order a ride on short notice via a mobile app.

An important aspect in RH is how to match the vehicles to customer requests such that various user and business oriented goals are optimized. Traditionally,

this optimization problem comes under the window of Vehicle Routing Problem with Time Windows (VRPTW) or more specifically Dial a Ride Problem (DARP), for which currently Large Neighborhood Search (LNS) based algorithms provide state of the art solutions [10][9]. The dynamic addition of new requests and the limited calculation time provide the biggest challenge for an efficient real time matching algorithm in RH. We suggest that we can benefit from past runs of similar RH problems using Machine Learning (ML) techniques, and get better quick assignment solutions.

Recently, a similar approach has been applied to Vehicle Routing Problem (VRP) with limited applicability using Pointer Networks (Ptr-Net) [14]. We extend their approach to suit for small static RH scenarios or more generally to VRPTW, where all the vehicle positions and customer requests are known in advance. We also present an innovative approach of combining LNS algorithm with NN, by using the trained network as an insertion operator inside LNS. The training and validation problems are taken from the open source New York City (NYC) Taxi data [1].

## 1.1 Literature Review

The growing market of RH has caused a resurgence of research interests into the VRPTW and DARP [10]. DARP is a special case of pickup and delivery problem with time windows, where both customer convenience and business goals are considered. It is very similar to matching vehicles to customer requests in RH and Ride Sharing (RS) services. However, in DARP vehicles start and end at fixed depots while in RH and RS each vehicle start and end at its own specific location. Therefore, to adopt DARP for RH scenario, each vehicle can be thought of having its own unique start depot, which drastically increases the problem complexity.

DARP has been applied to a wide range of applications. Various exact methods and metaheuristics exist in literature for solving multiple variants of DARP. Usually, the exact methods can solve only small size problems. Therefore, the major focus in literature has been on finding good solutions in a reasonable computation time using various metaheuristics [10][5]. Multiple approaches based on Adaptive Large Neighborhood Search (ALNS)[9][16], Variable Neighborhood Search (VNS)[15][8], Genetic Algorithm (GA) [6] and their Hybrids [12] are generally utilized for this purpose. Among these, the reported current and previous state of the art approaches for static DARP are presented in [9][12], that are based on ALNS and Hybrid Genetic Algorithm (HGA), respectively [10].

Beside the DARP literature, there have been some researches in transportation that simulated the RH scenarios, but many of them did not benefit from the available DARP procedures. The main focus has been to study the impacts and potential of ODM services or an autonomous fleet. Some used commercial microsimulators like Aimsun or MATSim [7] [11] for this purpose while others simulated only ODM vehicles inside self built simulation environment [3]. They either used simple heuristics for vehicle dispatching or developed their own procedures for continuous optimization of the problem. However, they did not target

the real operation of actual ODM services, where the services get very little time for optimization.

Both VRPTW and DARP have not been directly approached before in literature using Deep Neural Networks (DNN). However, there has been some attempts to solve combinatorial optimization problems with the help of DNN. The first of which is Ptr-Net introduced by [18]. They used it to solve optimization problems like Convex Hull, Delaunay Triangulation and Traveling Salesman Problem (TSP). It is based on supervised learning to generate semi optimal solutions for small sized problems which can not be solved by optimal solvers. They used beam search to consider only valid solutions during the test time.

A modification to the loss function of Ptr-Net was proposed in [13]. Since it depended on supervised learning technique and the solutions used for training were sub optimum, the predicted solutions might outperform the training solutions. Therefore, they added another layer to the loss function that could choose between the current prediction and the approximated supervised solution. During training, it checked if the difference between the predicted solution and the ground truth is less than zero or not; and it treats the loss to be equivalent to zero if it is less than zero. This is a very simple yet effective approach since it makes sure that we do not over train the network with bad solutions.

A Reinforcement Learning (RL) approach based on Ptr-Net was introduced in [4]. They showed the effectiveness of their model by solving combinatorial optimization problems such as TSP and the knapsack problem. A further attempt was made by [14]. They used RL and a simplified version of the Ptr-Net to solve TSP and Capacitated Vehicle Routing Problem (CVRP). They tested their model with one vehicle and up to 100 requests and used beam search in the decoder to obtain their best solutions. They used static and dynamic features for the requests and a mask function to eliminate the set of infeasible solutions on every time step. Additionally, they replaced the Long Short Term Memory (LSTM) network at the encoder with an embedding layer that maps the input to a higher dimensional vector space since the input data is not sequentially dependant. Therefore, the updated embeddings at every time step can be efficiently updated, which decreases the training time significantly. Our implementation is based on this model which is further extended to allow for more vehicles and time constraints.

## 1.2 Problem Formulation

Consider a fleet V of size m and a set R of l requests. For an RH scenario, we assume that each vehicle can serve only one request at a time. For each request r ∈ R , we have a pickup location r p , a drop-off location r d and the time t r when the request wants to be picked up. The time it takes to travel from r p to r d is given as T r . For each vehicle v ∈ V , let P v ⊆ R be a set of requests assigned to v , and T v be the total duration that v will take to serve the requests in P v . We assume that after serving all requests in P v the vehicle stays at the drop-off location of the last request. Let d max be a constant indicating the maximum delay allowed for picking up a request. Then, for each request r , the allowed

pickup time window is given as [ t r , t r + d max ]. For each vehicle v ∈ V , let A rv be the vehicle arrival time at the pickup location of the request r . The delay for the customer pickup can be calculated as delay rv = max(0 , A rv -t r ). Lastly, let c r be 1 if request r is not covered by any vehicle, otherwise 0. With the above notations, the multi-objective function for the matching problem is given by:

$$\min \alpha \sum _ { v \in V } T _ { v } + \beta \sum _ { v \in V } \sum _ { r \in P _ { v } } d e l a y _ { r v } + \delta \sum _ { r \in R } c _ { r }$$

where the three terms represent the amount of time traveled by vehicles, the pickup delay of the served customers and the number of unserved customers, respectively. α and β are constant weights for the relative importance of the total travel time of all the vehicles and the total delay to the customers, respectively. δ is a relatively heavy weight for serving the maximum number of customer requests.

## 2 Methodology

In this section, we present the overall methodology used. We start with the preprocessing step of RV graph, then a brief description of LNS then finally detailed description of our NN.

## 2.1 Pairwise Request-Vehicle (RV) Graph

We first preprocess an RV graph to reduce the complexity of the RH matching problem and help in the implementation of the algorithms. It is motivated by the ideas of forward time slack in [17] and the RV graph in [3]. We compute a pairwise directed graph for vehicles and requests, with negative edges representing pairwise time slack and positive edges representing the pairwise delay for the pickup. Consider a directed RV graph G N,E ( ) with set N = V ∪ R as nodes and edges E defined as follows:

For a vehicle v ∈ V and a request r ∈ R , the edge ( v, r ) ∈ E if T vr + t s -t r ≤ d max , with a weight e = T vr + t s -t r ; where T vr is the traveling time from the vehicle location to the pickup point of r and t s is the start time of the vehicle. This implies that starting from the current location, the vehicle can pick up the request with a delay less than d max .

Similarly for two requests r 1 and r 2 , the edge ( r , r 1 2 ) ∈ E , with weight e = T r 1 2 r + T r 1 + t r 1 -t r 2 , if T r 1 2 r + T r 1 + t r 1 -t r 2 ≤ d max ; where T r 1 is the travel time from pickup to drop-off location of r 1 and T r 1 2 r is the distance from drop-off of r 1 to pickup of r 2 (And vice versa for edge ( r , r 2 1 ) ∈ E ). This means that an imaginary vehicle starting at the pickup point of r 1 , drops off r 1 first, and then goes and picks up r 2 such that the delay for picking up r 2 is less than d max .

Algorithm 1 Large Neighbourhood Search (LNS) function

```
Algorithm 1 Large Neighbourhood Search
    function SOLVE(sol \in solutions)
        sbest \getsol
        while stop-criterion not fulfilled do
                \s \getsol
               remove q requests from \s
               reinsert removed requests into \s
               if f(\s) < f(sbest) then
                   sbest \getsol \s
               if accept(\s,sol) then
                   sol \getsol \s
        return sbest
```

## 2.2 Large Neighborhood Search (LNS)

LNS is based on the idea of exploring such a large neighborhood of the solution that it's almost impossible to explore it explicitly. In terms of VRPTW, this corresponds to removing q percent requests and inserting them back using a variety of removal and insertion operators respectively, as shown in Algorithm 1. The initial solution is first built using an insertion operator. Ropke and Pisinger [16] first showed LNS effectiveness for VRPTW with pickup and drop off. They introduced a roulette wheel based adaptive scheme for dynamically selecting greedy and (2-, 3-, 4- and k -) regret insertions, and random, shaw and worst removals, and thus termed it as ALNS.

We use the same LNS operators as suggested by Ropke and Pisinger [16], and its solution is used for training NN. However, to adapt the algorithm to RH scenario, we consider that every vehicle has its own station or start location, as mentioned in section 1.1. Furthermore, we introduce a trained NN based insertion operator for final testing as follows:

- LSTM based Insertion: The basic aim of using LSTM based insertion inside LNS is to benefit from the past runs on similar problems. We expect that assigning a request to a vehicle path, one at a time, is like a sequence. The traditional insertion operators inside LNS look at various details of the request vehicle combinations and assign a suitable request to a vehicle's path. We expect that an LSTM based network can learn to make such a decision by looking at various features. The details of the used LSTM network architecture will be discussed later.

## 2.3 Neural Network Architecture

The proposed NN architecture is based on the model mentioned in [14] using dynamic features that are updated before every decoder time step. We also follow the same approach of replacing the Recurrent Neural Network (RNN) encoder presented in the Ptr-Net with an embedding layer for mapping the input to a D-dimensional vector space since the input is not sequentially dependant.

Fig. 1: The network architecture used for the assignment.

![Image](image_000001_8a1260fae22c9c4fe950d40a7d6a312b6cad9ee831773afab700d5b93830f578.png)

The architecture is generalized to cater for multiple vehicles and the pickup time windows of each customer request. For this purpose, every vehicle v j ∈ V is assigned a fixed number of slots n , representing the path of the vehicle. n can be adjusted for different problem sizes without the need of retraining. Our model also improves the vehicle path at every decoding time step in order to minimize the overall objective cost. However, in this path correction it guarantees that the request r i would still be served by v j (to which it was first assigned) but it can be assigned to a later slot s k ∈ S of v j .

Model overview Our input consists mainly of three distinct sets which are: requests, vehicles and slots. These sets are permuted together to generate every possible combination using RV graph. Each cell of the permuted sets has its own dynamic features that change at every decoding step. Then the input cells are mapped to a higher dimensional space using an embedding layer as shown in Fig. 1. At the output, a greedy decoder chooses one specific input (of highest probability) which represents an assignment of a request at a slot within a vehicle's path. The output at every time step is dependant on all the input permutations X t at the current time step and the outputs of the previous time steps y ....y 1 t -1 . Probabilities over different inputs are calculated with the same attention mechanism presented in [14].

Input features Each input cell is composed of four features, all of which are dynamic i.e change every time step depending on the assignment at t -1. Contrary to [14], we did not use any static feature. The dynamic features are:

- 1. The cost of assignment: This feature is similar to the incremental cost ∆f rv used in the basic greedy insertion operator for LNS. However, unlike basic greedy, the cost of insertion for r at all the possible insertion positions within v is used. Since all the vehicles are assumed to be empty at the beginning, initially only the cost of assigning requests in the first slot to vehicles is calculated. When a request r i is assigned to vehicle v j , the cost of assignment to v j is recalculated for all requests, that would include the first slot before r i and the second slot after

- it. These possible insertion slots keep increasing as more requests are assigned to v j . While inserting r i in v j at an intermediate slot s k it is made sure that the requests previously assigned to v j at s a where a ≥ k would be served in their predefined time window. Otherwise, s k would not be considered as an insertion option for r i in v j and would be masked out. Costs are further normalized by dividing them by the penalty term of neglecting a request (see section 1.2).
- 2. Number of outgoing edges in RV Graph: This feature takes a look one step further in time. For a cell of r v s i j k , it considers the number of requests from the set of unmatched requests U that can still be added to the path of v j at s b where b ∈ [1 , n ], while satisfying the time constraints for the set of requests P v j already in v j path. The number of outgoing edges for each input cell is divided by the total number of requests in the problem to be in the range [0 , 1].
- 3. Number of available vehicles: It considers the total number of vehicles that are able to serve r i considering the current path of the vehicles and the time window of the r i . This feature is request specific such that r v s i j k is the same ∀ j ∈ [1 , m ] &amp; k ∈ [1 , n ]. In other words, this feature does not change for same request and different vehicle. The value of this feature is divided by the total number of vehicles in the problem for normalization.
- 4. Regret value: This is also a request specific feature. It's based on the 2regret insertion concept as used in [16]. Intuitively, it keeps track of the difference between the best and second best insertion cost for a given request. For a request that has just one insertion option available, its regret value is calculated by subtracting its insertion cost from the penalty weight δ of neglecting a request.

Mask function The Mask function keeps track of the assignments that are infeasible according to the time window constraints. These infeasible assignments are guaranteed to have a probability of zero and thus they would never be selected by the decoder. If all the inputs have zero probability, then the decoding stops. This also makes sure that there could be no more valid assignments, and thus, the remaining requests (if any) will remain unassigned.

Loss function We want to train our neural network to generate solutions whose total cost is minimized. Therefore, the total objective cost of the solutions from the network should be as close as possible to the training solutions. Since these training solutions are not solved up to optimality, we add the argmax block from [13] to make sure that the network will not be over trained with bad solutions. The number of unassigned requests per solution are multiplied by the penalty term δ . Then, the loss is further divided by the ground truth solution to account for different magnitudes when the problem size is different. We base our loss function on the one used in [4] by applying the modifications mentioned above.

$$L ( \theta ) = \frac { 1 } { B } \sum _ { b = 0 } ^ { B } \frac { 1 } { C _ { G T } ^ { b } } \max \left ( \left ( ( \sum _ { t = 0 } ^ { T ^ { b } } c _ { t } ^ { b } ) + \left ( ( l ^ { b } - T ^ { b } ) \times \delta \right ) - C _ { G T } ^ { b } \right ), [ 0 ] \times B \right ) \cdot \sum _ { t = 0 } ^ { T ^ { b } } \log p _ { \theta } ( y _ { t } ^ { b } )$$

where θ represents the learnable network parameters. B is the batch size, C b GT is the ground truth objective cost (approximated here) for the whole problem b , l b is the total number of requests for the problem and T b is the number of output time steps from the decoder. c t is the cost of the assignment chosen at decoding time step t and y t is the probability of the output at this time.

## 3 Computational Experiments

We used NYC Taxi data for generating requests and vehicles [1]. Vehicles were generated by choosing the trip start positions randomly from the NYC data. Requests were generated from January 2016 for different working days in the morning. Only the trip start time, start location and end location were used for generating the requests. Then Open Source Routing Machine (OSRM) was used for calculating the travel time between different locations [2]. We generated multiple problem instances for the training with the number of vehicles fixed to 10 and varying only the number of requests between { 10 20 30 40 , , , } . We denote each problem size by m l a b , where a and b represent the number of used vehicles and requests in the problem instance, respectively. For each problem size, 1000 samples were generated for training and 300 for testing. Since it was difficult to solve the problem instances upto optimality for the mentioned problem sizes and time windows, LNS with 2-regret insertion and shaw removal operator was used for finding the solutions. LNS was run for 2000 iterations for each of the problem instance and the solutions were used as ground truth for our training process. We trained three different models, which are:

- 1. Specific model with n &gt; 1 : This model was trained separately on each problem instance until convergence using transfer learning. It was first trained on m l 10 10 , then starting from the same weights on m l 10 20 , then m l 10 30 and finally on m l 10 40 . We use number of slots n = 6.
- 2. General model: This model was trained on all the problem instances at once. Each batch contains problems of different sizes. The number of slots n = 6 here as well.
- 3. General model with n = 1 : In this model, the requests are always appended at end of a vehicle's path, i.e. n = 1. This eases the model as the features have to be calculated for just one position.This model is specifically important when we want to build vehicle paths incrementally without rescheduling the already assigned requests. Ideally, when we have perfect features for the insertion of unassigned request, such a model should be able to build vehicle path optimally by just looking at the dynamic features in the current decoder time step with much less computation time. Therefore, we include such a model in our analysis to test how much close our features are to such an ideal scenario.

The encoder hidden size is set to 128 and the number of layers in the LSTM decoder network to 2. We use a batch size of 64 samples and train for 500 epochs which are enough for convergence. The GPU device used for training is Nvidia quadro M4000 with 8GB memory. Each epoch containing instances of all the

problem sizes for the general model with n &gt; 1 takes around 10.4 minutes while it takes around 7.2 minutes for the model with n = 1.

## 4 Results and Discussion

In this section we discuss the results of the computational experiments.

## 4.1 Initial solution comparison

In this section we compare the performance of the three models against the 2-regret and basic greedy insertion heuristics of [16] for building the initial solution. Furthermore, since we are also testing a model that only inserts the new requests at the last position, i.e. Generic Model with n = 1, we found it reasonable to also test the approaches against modified versions of 2-regret and basic greedy insertions that only inserts the unassigned requests at the last position of vehicle's path. We represent these modified 2-regret and basic greedy by putting n = 1.

Fig. 2 compares the initial solutions' mean relative objective cost difference to 2000 iteration of LNS and the number of unassigned requests for various insertion strategies. In the simple problem of m l 10 20 , all the methods provide very similar solutions. However, when the problem becomes more complicated our general model with n &gt; 1 provides better solutions. Additionally, for m 10 and l i where i ∈ { 20 30 40 , , } the general model's performance almost same as the specific model which is trained on every problem size individually. This is very important to ensure that the loss function used is invariant to the magnitude of the problem size. Furthermore, this shows that the general model has already

Fig. 2: Comparison of initial solution building using NN models and other LNS insertion heuristics. The problem sizes on the right side of the dashed line correspond to bigger problem sizes on which the NN was never trained.

![Image](image_000002_6be00fdb576798c2fa28d22865d0b08d9a8b75b98bcbf0cfd622e59218bc8178.png)

![Image](image_000003_5d8045bcee8dc822cf7f7d4b8fb7a94d1a71364bac1907131314e12fe5d2d8b1.png)

learned what to do in each time step regardless of the size of the problem. This is also confirmed when the model is tested on instances of more than double the sizes it was trained on (right side of the dashed line) where the overall cost for the general model is better than the other heuristics. This verifies the fact that the model can generalize and scale well to larger problem instances without retraining.

Besides inside actual LNS, the initial solution approach can be used in a realtime dynamic environment to provide the user with a quick reply when there is no time available for batch optimization.

## 4.2 Trained Neural Network as insertion operator inside LNS

As introduced in section 2.2, we analyze the performance of the trained NN when used inside LNS as an insertion operator. For this analysis we use only the general model with n &gt; 1, as they gave best solution quality for initial solution. We compare it against the ALNS scheme of [16], as multiple LNS operators are almost always better with the adaptive scheme. Additionally, we also test the trained NN along with other operators in ALNS.

We run each problem instance multiple times with each of the used procedures. For this experiment, we select 10 problems for each problem size. Each problem is solved 5 times for each of the procedure with 2000 iterations. We take mean of the 5 solutions and compare it against the best found solution for each of the problems. Hence, the formula used to calculate the solution quality for a problem i using procedure j is given by:

$$( s o l u t i o n \ q u a l i t y ) _ { i } ^ { j } = \frac { \frac { 1 } { 5 } \sum _ { k = 1 } ^ { 5 } \cos t _ { k } ^ { j } } { \cos t _ { b e s t } ^ { i } }$$

where cost i best is the objective cost of the best solution found for problem i regardless of the procedure used and cost j k is the objective cost of the solution obtained using j procedure and k th run.

Fig. 3 compares the mean solution quality of the NN based LNS, ALNS with NN and traditional ALNS. All procedures are run for 2000 iterations. For small sized problems m l 10 20 all the procedures provide similar quality as the the problem is already simple to solve. However, for increasing problem complexity, the NN based approaches are continuously seen to be performing better than the traditional ALNS. For finding good solutions with limited number of iterations and increasing complexity, it is crucial that good requests insertion decisions are taken earlier. Our trained network is interestingly able to take such decisions even for the problem sizes for which the network has never been trained, i.e. ( m l 15 60 , m l 20 80 and m l 25 100 ). This shows that, as mentioned in section 2.2, trained model learned how to pick better insertion requests based on seen solutions. Ideally, if we had perfect set of features and trained model, then we would expect the LNS to converge in a single iteration. However, in the absence of that, we expect the solution to converge earlier to best solution if we use better features.

Fig. 3: Comparison of solution quality of trained NN as sole insertion operator inside LNS, trained NN inside ALNS along with other operators and ALNS with only traditional insertion operators. The problem sizes on the right side of the dashed line correspond to bigger problem sizes on which the NN was never trained. The shaded region mark the minimum and the maximum solution quality for each problem size.

![Image](image_000004_8ccf0ef03b4f599f739f9af0493732975fff206c16ee17960471484ad05aa085.png)

## 5 Conclusion

In this paper, we introduced the concept of fixed number of slots n for generalizing the NN model of [14] for RH scenario. The modification can be used with any VRPTW with minor changes. The experiments show that the network learned how to make good request insertion decisions.The general model with n &gt; 1 provided the best solution quality, even for the problem sizes on which it was never trained. Ideally a perfect NN with perfect features set would build solution with just n = 1, which can be further looked into in the future.

We also used the trained NN inside LNS and ALNS as an insertion operator which showed an improvement over the traditional ALNS. Such a NN based insertion has great potential inside LNS, as various information that is generally used by individual insertion operators separately for making insertion decisions, can be combined together inside a single NN.

In the future, we propose to try other strategies in the decoder rather than using the greedy decoder directly. This can lead the model to reach a better solution faster inside LNS. We also suggest to use the suggested NN inside an actual dynamic RH simulation environment for quick assignment of vehicles.

## References

- 1. https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page
- 2. http://project-osrm.org/

## 12 A.A. Syed, K. Akhnoukh et al.

- 3. Alonso-Mora, J., Samaranayake, S., Wallar, A., Frazzoli, E., Rus, D.: On-demand high-capacity ride-sharing via dynamic trip-vehicle assignment. Proceedings of the National Academy of Sciences 114 (3), 462-467 (2017)
- 4. Bello, I., Pham, H., Le, Q.V., Norouzi, M., Bengio, S.: Neural combinatorial optimization with reinforcement learning. arXiv preprint arXiv:1611.09940 (2016)
- 5. Cordeau, J.F., Laporte, G.: The dial-a-ride problem: models and algorithms. Annals of operations research 153 (1), 29-46 (2007)
- 6. Cubillos, C., Urra, E., Rodr´ ıguez, N.: Application of genetic algorithms for the darptw problem. International Journal of Computers Communications &amp; Control 4 (2), 127-136 (2009)
- 7. Dandl, F., Bracher, B., Bogenberger, K.: Microsimulation of an autonomous taxisystem in munich. In: 2017 5th IEEE International Conference on Models and Technologies for Intelligent Transportation Systems (MT-ITS). pp. 833-838. IEEE (2017)
- 8. Detti, P., de Lara, G.Z.M.: Variable neighborhood search algorithms for the multidepot dial-a-ride problem with heterogeneous vehicles and users. arXiv preprint arXiv:1611.05187 (2016)
- 9. Gschwind, T., Drexl, M.: Adaptive large neighborhood search with a constant-time feasibility test for the dial-a-ride problem. Transportation Science (2019)
- 10. Ho, S.C., Szeto, W., Kuo, Y.H., Leung, J.M., Petering, M., Tou, T.W.: A survey of dial-a-ride problems: Literature review and recent developments. Transportation Research Part B: Methodological 111 , 395-421 (2018)
- 11. H¨ orl, S., Ruch, C., Becker, F., Frazzoli, E., Axhausen, K.W.: Fleet control algorithms for automated mobility: A simulation assessment for zurich. Transportation Research. Part C, Emerging Technologies (2018)
- 12. Masmoudi, M.A., Braekers, K., Masmoudi, M., Dammak, A.: A hybrid genetic algorithm for the heterogeneous dial-a-ride problem. Computers &amp; operations research 81 , 1-13 (2017)
- 13. Milan, A., Rezatofighi, S.H., Garg, R., Dick, A., Reid, I.: Data-driven approximations to np-hard problems. In: Thirty-First AAAI Conference on Artificial Intelligence (2017)
- 14. Nazari, M., Oroojlooy, A., Snyder, L., Tak´c, M.: Reinforcement learning for solva ing the vehicle routing problem. In: Advances in Neural Information Processing Systems. pp. 9839-9849 (2018)
- 15. Parragh, S.N., Doerner, K.F., Hartl, R.F.: Variable neighborhood search for the dial-a-ride problem. Computers &amp; Operations Research 37 (6), 1129-1138 (2010)
- 16. Ropke, S., Pisinger, D.: An adaptive large neighborhood search heuristic for the pickup and delivery problem with time windows. Transportation science 40 (4), 455-472 (2006)
- 17. Savelsbergh, M.W.: The vehicle routing problem with time windows: Minimizing route duration. ORSA journal on computing 4 (2), 146-154 (1992)
- 18. Vinyals, O., Fortunato, M., Jaitly, N.: Pointer networks. In: Advances in Neural Information Processing Systems. pp. 2692-2700 (2015)