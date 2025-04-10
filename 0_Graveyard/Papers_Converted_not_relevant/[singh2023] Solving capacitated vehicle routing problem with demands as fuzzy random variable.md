## OPTIMIZATION

## Solving capacitated vehicle routing problem with demands as fuzzy random variable

V. P. Singh 1 · Kirti Sharma 1 · Debjani Chakraborty 2

Accepted: 19 June 2023 / Published online: 15 July 2023

©The Author(s), under exclusive licence to Springer-Verlag GmbH Germany, part of Springer Nature 2023

## Abstract

Capacitated vehicle routing problem (CVRP) is a classical combinatorial optimization problem in which a network of customers with specified demands is given. The objective is to find a set of routes which originates as well as terminates at the depot node. These routes are to be traversed in such a way that the demands of all the customers in the network are satisfied and the cost associated with traversal of these routes come out to be a minimum. In real-world situations, the demand of any commodity depends upon various uncontrollable factors, such as season, delivery time, and market conditions. Due to these factors, the demand can always not be told in advance and a precise information about the demand is nearly impossible to achieve. Hence, the demands of the customers always experience impreciseness and randomness in real life. The decisions made by the customers about the demands may also have some scope of hesitation as well. In order to handle such demands of customers in the network, fuzzy random variables and intuitionistic fuzzy random variables are used in this work. The work bridges the gap between the classical version of CVRP and the real-life situation and hence makes it easier for the logistic management companies to determine the routes that should be followed for minimum operational cost and maximum profit. Mathematical models corresponding to CVRP with fuzzy stochastic demands (CVRPFSD) and CVRP with intuitionistic fuzzy stochastic demands (CVRPIFSD) have been presented. A two-stage model has been proposed to find out the solution for the same. To explain the working of the methodology defined in this work, different examples of networks with fuzzy and intuitionistic fuzzy demands have been worked out. The proposed solution approach is also tested on modified fuzzy stochastic versions of some benchmark instances.

Keywords Vehicle routing problem · Branch-and-bound algorithm · Discrete fuzzy random variable · Fuzzy stochastic demands · Graded mean integration representation

## 1 Introduction

CVRP (Toth and Vigo 2002), a very well-known problem in operational research, aims at finding a set of routes beginning and ending at source node in a way such that total cost incurred comes out to be a minimum. In CVRP, we are usu-

B

V. P. Singh

vpsingh@mth.vnit.ac.in

Kirti Sharma unik96kirti@gmail.com

Debjani Chakraborty debjani@math.iitkgp.ac.in

ally given a weighted graph in which the weights on the edges represent the time (or distance or cost) needed to traverse that edge and the vertices represent customers with some demands to be fulfilled. There is a depot(source) node from where the journey of the travelling salesman (Russell and Norvig 2016) starts with an aim to travel each customer exactly once, fulfil their demands and then return to the depot node. In conventional CVRP, customers' demands are known precisely in advance, which makes it easier for the travelling salesmantoplantheroutes.Indailylifesituations, sometimes the demand of the customers is not known clearly, whereas sometimes, the demands may not be known well in advance.

- 1 Department of Mathematics, Visvesvaraya National Institute of Technology, Nagpur, India
- 2 Department of Mathematics, Indian Institute of Technology, Kharagpur, India

## 1.1 Literature review

TheCVRPwasfirstproposedin1959byDantzigandRamser (1959), and because of the widespread applications of the

![Image](image_000000_fd744b63e8b5f6f59f700cfffbd71c3675b3603c25ed7b1e4663ef4d7f0fd5a3.png)

problem in logistic management, communication networks, urban solid waste collection, vehicle scheduling, etc., the problem has received close attention from the optimization community since then. CVRP is an extended version of the well-known travelling salesman problem (TSP) (Cormen et al. 2009), where the objective is to determine a minimum cost Hamiltonian circuit. Thus, many approaches may it be exact, random, approximate, heuristics based or metaheuristics based, which are used for finding a solution for CVRP inherited from the successful works done for finding the solution of TSP. The book edited by Toth and Vigo (2002) presented an overview of various exact and heuristic methods for solving a CVRP. Exact algorithms used for solving VRP are based on branch-cut-and-price algorithms (Gauvin et al. 2014). An interesting survey comprising of early exact methods for solving CVRP is given by Laporte and Nobert (1987).

VRP is known to be NP hard in nature; thus, all the exact algorithms used for solving the problem are only applicable for small instances only and face time complexity issues for big networks. The inability of the exact approaches to solve medium- and large-scale vehicle routing problem and the difficulty in evaluating objective function in real-life complex problems are two main reasons why heuristic and meta-heuristics are needed for solving such problems. One of the very early works in heuristics was presented by Clarke and Wright (1964), where a savings heuristics was designed to solve the problem. A heuristic method based on nearest neighbour algorithm was defined in Pop et al. (2011). Nearest neighbour algorithm is a greedy algorithm in which the nearest vertex is traversed first when given a set of vertices to be traversed. A tabu search algorithm was presented by Úbeda et al. (2014) for solving a green CVRP. In tabu search method, a local heuristic method is used to explore the solution space beyond optimality. Meta-heuristic methods based on genetic algorithm was presented in Mohammed et al. (2012) and Awad et al. (2018). Genetic algorithm mimics the natural evolution process as defined in theory of evolution by Charles Darwin. Meta-heuristic methods based on ant colony optimization and particle swarm optimization were presented in Dorigo et al. (2006) and Fang et al. (2007), respectively. Such method imitates the behaviour of ants and swarms, and a local optimal solution is obtained. Ahybrid genetic algorithm-bacteria foraging optimization algorithm, was defined in Barma et al. (2021). A hybrid particle swarm optimization and simulated annealing method was presented in Fang et al. (2007). Christofides heuristic method was extended by Frieze (1983) to solve a k-person TSP. A modified fuzzy C-means clustering approach was presented in Shalaby et al. (2020) for solving CVRP. The approach is based on cluster first route second method. In Shalaby et al. (2020), the first stage of problem solving corresponds to the formation of clusters, and in second phase, each cluster is solved independently as a TSP.

Recent years have witnessed various variants of CVRP based on the uncertainty and variability of different attributes of the problem. In the classical version of the problem, all the information is available well in advance and such a VRP is known as a dynamic VRP. If one or more parameters of the problem are not known well in advance, then such a problem is known as stochastic VRP (SVRP). The early works in SVRP were presented in Gendreau et al. (1996), Dror et al. (1989) and Laporte and Nobert (1987). The stochastic nature of the problem is generally found in two parameters, namely the demands of the customers and the travel times. Cordeau et al. (2007) studied the most basic and most studied version of SVRP, i.e. CVRP with stochastic demands (CVRPSD). In CVRPSD, the customer demands are not known well in advance and are only revealed when a vehicle arrives at a customer location. Two well-known accepted algorithms, namely branch-cut-and-price (Gauvin et al. 2014) and branch-and-price (Christiansen and Lysgaard 2007)algorithms, are used for solving CVRPSD. In Bernardo and Pannek (2018), a robust solution approach was presented for solving dynamic and stochastic vehicle routing problem. A two-stage stochastic program model with recourse is presented where the objective of first stage is to minimize the a priori routing plan and the second stage dealt with minimizingtherecoursecost.Alocalsearch-basedalgorithmwith the objective of minimizing the total number of vehicles used and total distance visited was developed in Kaur and Singh (2017) for solving CVRPSD. In Markovi« c et al. (2020), a case study of real-life problem of municipal waste collection in the city of Nis is considered and a solution approach based on heuristic and meta-heuristic methods was presented for solving CVRPSD. In Xia et al. (2021), a hybrid algorithm called DSMO-GA which combines genetic algorithm (GA) and discrete spider monkey optimization (DSMO) algorithm was proposed for solving CVRPSD.

When the customer demands are not known precisely but are given as uncertain quantities, i.e. as fuzzy numbers, then such a VRP is known as VRP with fuzzy demands. In Singh and Sharma (2020), CVRP with fuzzy demands given by interval type 2 triangular fuzzy numbers was presented. They used a modified Clarke and Wright algorithm to solve the problem. In Tordecilla et al. (2021), a hybrid approach combining simulations, meta-heuristics, and fuzzy logic has been used to generate near optimal strategies to large-scale NP hard problems. They used the simheuristic approach for optimizing VRP with fuzzy demands. In Werners and Drawe (2003), CVRP with fuzzy demands was handled by using fuzzy multi-criteria modelling approach. In Kuo et al. (2012), hybrid particle swarm optimization with a genetic algorithm for solving CVRP with fuzzy demands is proposed. In Mirzaei-khafri et al. (2020), a location arc routing

problem was solved by using a mixed integer programming model. In Mirzaei-khafri et al. (2020), the demand of each edge belongs to a bounded uncertainty set. The proposed model determines a subset of potential depots to be opened along with their allocated routes. In Dutta et al. (2022), a biobjective green vehicle routing problem with heterogeneous fleet is proposed. Along with the minimization of distance, the greenhouse gases emitted by the vehicles during the operation are also minimized. Multi-criteria optimization and VIKOR method are used to determine the best solution from the Pareto front.

In certain situations, the cost matrix stores the time taken to traverse edges in the network. In many practical problems, the travel time between two locations in a routing problem cannot be told precisely because of traffic congestion or road conditions. Such situations can be handled by using fuzzy numbers for representing travel times. A fuzzy approach for solving VRP with fuzzy travel times was presented in Brito et al. (2010). There also exist situations when the travel times cannot be told well in advance. Such a version of CVRP with stochastic travel times was presented in Oyola (2019). In Oyola (2019), a non-dominated sorting genetic algorithm (NSGA) together with a variable neighbourhood search (VNS) heuristic was proposed. The demands of the customers were assumed to be deterministic. In addition to stochastic travel time, a soft time window is also associated with every customer and there exists a penalty for serving the customer outside their specified time window. In Zulvia et al. (2012), a hybrid ant colony optimization and genetic algorithm are used to solve VRPTW (VRP with time windows) where the demands of the customers, as well as travel time between every pair of customers, are imprecise. In Gupta et al. (2022), a particular case of VRP where triangular fuzzy numbers are used to represent the fuzzy travel times and the deliveries are split into bags was modelled. In Gultom and Napitupulu (2020), an algorithm is based on Clarke and Wright savings heuristic where three criteria of the network, namely the distance of various edges, time required to travel these edges, and road quality, are considered as fuzzy. In Kondratenko et al. (2020), fuzzy and evolutionary algorithms wereusedforplanningtheroutes of tankers when the demand of fuel at various nodes in the network is presented by using triangular fuzzy numbers.

## 1.2 Novelty and contribution

Higher cost of precise information retrieval and random nature of demands of customers give rise to CVRPFSD. In CVRPFSD, the demands of the customers are neither presented precisely and are also not given well in advance. The decisions about the demands also face hesitation because of fluctuating demand-supply chain network. This work considers such fuzzy stochastic nature of customers' demands, and such nature of customer demands is handled by using fuzzy random variables. In this work, the basic version of CVRPSD has been extended and the current version deals with CVRPFSD and CVRPIFSD. In this paper, the edge weights denote the travel cost. The customers' demands are only revealed upon the arrival of the vehicle, and even then the demands of the customers are not told precisely. Various uncontrollable factors like delivery hours, market conditions, demand-supply chain fluctuations contribute to this imprecise and random nature of the customers' demands. In this work, two cases have been considered. The first case corresponds to the situation when the demands of customers are represented by using fuzzy random variables, whereas in the second case, the demands of the customers are represented by using intuitionistic fuzzy random variables. Various methods have been developed in the literature for solving CVRP with fuzzy demands and CVRPSD separately, but a combination of randomness and impreciseness has never been dealt in the literature. To the best of author's knowledge, such an amalgamation of randomness and impreciseness will bridge the gap between conventional and real-life problems.

In this work, CVRPFSD (CVRPIFSD) has been modelled as a two-level stochastic process. While solving the problem, the first task is to design a minimum cost path which traverse each node exactly once. Here, we used branch-andbound algorithm for finding a minimum cost Hamiltonian circuit. The branch-and-bound algorithm can only be used for symmetric networks of small sizes, and this comes up as a major limitation of this work. After designing, the execution of these routes takes place. Since the demands of the customers are not known in advance and are imprecise in nature, it may happen that upon reaching a customer, the salesman will realize his/her inability to serve the customer. This is the case when route failure occurs, and in such cases, the salesman is required to return to the depot node, replenish their vehicle, and then re-continue the planned route from the point of recent failure. In this work, the customer is served only in one visit of one vehicle, i.e. un-split delivery service policy is followed. The recourse policy followed in this work is reactive in nature, i.e. a vehicle will only return to depot when a route failure will occur. The results obtained by this method can only be used when the demands of the customers are represented by discrete fuzzy (intuitionistic fuzzy) random variables, where the demand of the customers and their respective probability are given by triangular (triangular intuitionistic) fuzzy numbers. The results of this work can be extended for scheduling problems faced by service agents since the service time windows are always stochastic and imprecise. The model can also be further extended to include the randomness and impreciseness of the network, and such networks can be represented by using neutrosophic graph theory (Kandasamy et al. 2015). The work can also be extended when the demands of the customers present in the

network are expressed by using linguistic environment using dual-connection numbers (Irvanizam et al. 2020). The use of dual-connection numbers for representing demands will help the customers to formulate the membership degree under certain and uncertain situations and thus help the customers in making feasible and rational judgements.

## 1.3 Applications of CVRPFSD and CVRPIFSD

Real-world applications of the CVRPFSD and CVRPIFSD include the distribution of perishable items from stockholders to retailers, who further sell it to the consumers. For this work, the retailers play the role of customers and the stockholders play the role of travelling salesman. Since perishable items decay after a period of time which usually depend upon the uncontrollable weather conditions. So, the demands of perishable items are always random in nature. Factors like the fluctuations of market supply and demand of any perishable item make it difficult for the retailers to precisely determine the demand. In such case, the demands are told randomly and imprecisely. Such real-life situations give rise to CVRPFSD. Psychologically, the experience of retailers also makes them hesitant because an unreasonable stock of such items would only either lead to decay and hence the loss (if more stock is bought), in terms of money, or it will lead to unsatisfied consumers (if less stock is bought), which will cost their market reputation. So, the decision of the retailer has some scope of hesitancy in addition to the randomness and impreciseness while entailing their demand. Such real-life situations give rise to CVRPIFSD. Another applications include distribution of cash to different automatic teller machines (ATMs) in the city. Other examples include the delivery of essential commodity (milk, oil) where daily customer consumption is random in nature but can be predicted with the use of discrete random variable.

This paper is structured as follows: Sect. 2 deals with the basic concepts and definitions of fuzzy set theory and intuitionistic fuzzy set theory. Branch-and-bound algorithm for solving TSP is also presented. In Sect. 3, two mathematical models for solving CVRP in an imprecise and random environment when the demands of the customers are represented by using triangular fuzzy numbers and triangular intuitionistic fuzzy number have been presented. Section4 deals with a procedure based on branch-and-bound algorithm to solve CVRPFSDandCVRPIFSD.Thissectioncomprisesofatwolevel algorithm and a two-level flow chart which presents the working of the two-level stochastic process defined in this paper. In Sect. 5, several numerical examples with customers having fuzzy stochastic demands and intuitionistic fuzzy stochastic demands have been presented. Section 5 also comprises of the comparison of the results obtained by the other methods and the method proposed in this work and thus explains the supremacy of the method proposed. The proposed algorithm is also tested on the modified version of some benchmark instances. Section6 comprises of the concluding remarks.

## 2 Preliminaries: concepts and definitions

Definition 1 Fuzzy number: A fuzzy set (Zimmermann 2011) ˜ A on R is said to be a fuzzy number, if the following three properties are satisfied:

- 1. Fuzzyset ˜ A mustbenormal,i.e. ∃ x suchthatsup µ ( ˜ A x ) = 1.
- 2. The support of fuzzy set, i.e. set of all the elements with nonzero degree of membership, must be bounded.
- 3. α level set, i.e. set of all the elements with membership degree greater than α , must be a closed interval for α ∈ [0,1].

Definition 2 Triangular fuzzy number: A generalized triangular fuzzy number is written as ˜ A = ( a , b , c ) , and the membership function of triangular fuzzy number ( a , b , c ) is given by Eq. (1).

<!-- formula-not-decoded -->

Definition 3 Symmetric triangular fuzzy number: A triangular fuzzy number is said to be symmetric if its left and right spreads are equal. If ˜ A = ( a , b , c ) is a triangular fuzzy number, then it is said to be symmetric if and only if ( b -a ) = ( c -b ) .

Example 1 ( 2 , 4 6 , ) is a symmetric triangular fuzzy number. The graph of the membership function of a symmetric triangular fuzzy number is given in Fig. 1.

Definition 4 Arithmetic operations on symmetric triangular fuzzy number: let ˜ A = ( a 1 , b 1 , c 1 ) and ˜ B = ( a 2 , b 2 , c 2 ) be two symmetric triangular fuzzy numbers, then

## 1. Addition:

( a 1 , b 1 , c 1 ) ⊕ ( a 2 , b 2 , c 2 ) = ( a 1 + a 2 , b 1 + b 2 , c 1 + c 2 ).

- 2. Symmetric image:

<!-- formula-not-decoded -->

- 3. Subtraction:

( a 1 , b 1 , c 1 ) /circleminus ( a 2 , b 2 , c 2 ) = ( a 1 -c 2 , b 1 -b 2 , c 1 -a 2 ).

Fig. 1 A symmetric triangular fuzzy number

![Image](image_000001_62340e3e3ed11ef3cf31781804fcc58f4616f0a03713bcd2ccc28cadfad5397f.png)

## 4. Multiplication:

<!-- formula-not-decoded -->

for ˜ A and ˜ B positive.

Definition 5 Triangular intuitionistic fuzzy number (TIFN): A triangular intuitionistic fuzzy number (TIFN) (Atanassov 2016) ˜ A I is an intuitionistic fuzzy number with the membership function and non-membership function given by Eqs. (2) and (3), respectively.

<!-- formula-not-decoded -->

where a ′ ≤ a ≤ b ≤ c ≤ c ′ . This TIFN is denoted by ( a , b , c ; a ′ , b , c ′ ) . Figure2 represents the membership and non-membership functions of TIFN defined above.

Definition 6 Arithmetic operations on TIFNs Atanassov (2016): Let ˜ A I = ( a , b , c ; a ′ , b , c ′ ) and ˜ B I = ( p , q , r ; p ′ , q , r ′ ) be two TIFNs, then

## 1. Addition:

A I

˜

B I

⊕ ˜

=

a

(

+

p

,

b

+

q

,

c

r

+ ;

a

′

+

p

′

,

b

+

q

,

c

′

+

r

′

)

## 2. Subtraction:

<!-- formula-not-decoded -->

## 3. Multiplication:

<!-- formula-not-decoded -->

where l 1 = min ap { , ar , cp , cr } , l ′ 1 = min a p { ′ ′ , a r ′ ′ , c p ′ ′ , c r ′ ′ } , l 2 = bq , l 3 = max ap ar { , , cp , cr } , and l ′ 3 = max a p { ′ ′ , a r ′ ′ , c p ′ ′ , c r ′ ′ } .

## 4. Scalar multiplication:

<!-- formula-not-decoded -->

## 5. Division:

<!-- formula-not-decoded -->

where a 0 , p &gt; 0

<!-- formula-not-decoded -->

Definition 7 Gradedmeanintegrationrepresentation: (Chiao 2015) let L -1 and R -1 be the inverse functions of L lef t ( ) and R right ( ) , respectively, then the graded mean integration representation of a generalized triangular fuzzy number ˜ A is given by Eq. (4).

<!-- formula-not-decoded -->

Thus, the graded mean integration representation (Chiao 2016) of a triangular fuzzy number ˜ A = ( a , b , c ) is given by Eq. (5).

<!-- formula-not-decoded -->

Definition 8 Accuracy function: let ˜ A I = ( a , b , c ; a ′ , b , c ′ ) be a triangular intuitionistic fuzzy number. The score function for membership function and non-membership function is denoted by S (µ ˜ A I ) and S (ϑ ˜ A I ) , respectively, where

<!-- formula-not-decoded -->

The accuracy function (Singh and Yadav 2018) of ˜ A I is denoted by f ( ˜ A I ) and is defined by Eq. (6).

<!-- formula-not-decoded -->

A triangular intuitionistic

Fig. 2 fuzzy number

![Image](image_000002_c25724656800377ecc6872149c674f2f77203363548347e660fe8c86468c9b03.png)

Definition 9 Fuzzy random variable: a fuzzy random variable (Puri et al. 1993) is a random variable whose value is not real, but a fuzzy number.

imization problem, an upper bound corresponding to every node is calculated and the branches with higher upper bounds are explored first since that will provide better solution.

Definition 10 Expectation of discrete fuzzy random variable: if ˜ X is a discrete fuzzy random variable, in such a way that P X ( ˜ = ˜ xi ) = ˜ pi , i=1, 2, 3, . . . , then the fuzzy expectation is given by

<!-- formula-not-decoded -->

## 2.1 Branch-and-bound algorithm

The branch-and-bound algorithm (Cormen et al. 2009) is used to find exact solution for a wide variety of combinatorial optimization problems like TSP, knapsack problem, job scheduling problem, and many more. The algorithm works by systematically enumerating all the candidates solutions and then discarding some useless candidates by using the estimated bound on the quantity being optimized. In branch-and-bound algorithm, a tree search strategy is used to enumerate all possible solutions of a given optimization problem and only those branches of tree are explored which satisfies a bound. Figure3 shows a branch-and-bound tree search strategy corresponding to a network with 4 nodes. To avoid unnecessary exploration of entire tree, the algorithm calculates a value (known as bound) corresponding to each node. The bound of any node represents the cost of best possible solution that can be obtained if a path through that node is traversed. If a better solution cannot belong to the sub tree rooted at a considered node, then the sub-tree need not be further explored. Otherwise, the process of exploration continues. For minimization problem, a lower bound corresponding to every node is calculated and the branches with lesser lower bounds are explored for better solution. For max-

## 3 Mathematical model

In this section, mathematical models for solving a CVRP with random and imprecise demands have been formulated. The mathematical models depicted in Sects. 3.1 and 3.2 represent the situation when the demands of the customers are given by discrete fuzzy random variables and impreciseness of customer demands is handled by using triangular fuzzy numbers and TIFNs, respectively.

In this work, some assumptions regarding the structure of the network, demands of the customers in the network, the recourse policy, and the service policy used have been made. The structure of the network is assumed to be symmetric, i.e. ci j = c ji , which means that the cost of traversal from node i to node j is same as the cost of traversal from node j to node i . The demands of the customers are assumed to be independent and positive. The demand of every individual customer is assumed to be less than the carrying capacity of the vehicle used to maintain the feasibility of the network. In this work, the factors like randomness and impreciseness have been used to represent the demands of the customer. The random(stochastic) nature of the customer demands means that the demands of the customers are only known when the vehicle arrives and this stochastic nature of demands leads to the failure of the route. The failure of route means that when a salesman arrives at a particular customer, they realize that they do not have enough capacity of goods to serve the customer. In this work, it has been assumed that the route failure may occur any number of times. Upon the failure of the route, the salesman is supposed to return to the depot, i.e. a reactive recourse policy is used. After returning to the

Fig. 3 A tree-based search strategy for TSP in a network with 4 nodes

![Image](image_000003_18e84d0fb2e9eb3381cb82beb29ee165406b3c1d3eb09bcd84326dc4b09360d2.png)

depot, the vehicles are supposed to replenish themselves and continue the service again. In this work, it has been assumed that each customer is visited exactly once and is served in that visit only, i.e. the delivery policy used is un-split in nature. The journey of the travelling salesman is always assumed to originate and terminate at depot node only. The fleet used for serving the customers is assumed to be homogeneous, i.e. all the vehicles have the same carrying capacity, and they all operate at the identical cost.

<!-- formula-not-decoded -->

Wewill now formulate the mathematical model for CVRP withfuzzystochasticdemands.Let G = ( N = C ∪{ s , s ′ } , A ) be a graph. The notation C is used for the set of customers, and s and s ′ represent the source node and its copy, respectively. ci j represents the cost of travelling from node i to node j . Let ˜ Di and ˜ D I i be the fuzzy random variable and intuitionistic fuzzy random variable representing the fuzzy and intuitionistic fuzzy demands of the customer at node i , respectively. Demand of source node is assumed to be 0 units.

Aroute is defined as a path of the form P = ( p 1 , p 2 , . . . , p P | | ) where p 1 = s and p P | | = s ′ with ph ∈ C for h ∈ { 2 , 3 4 , , . . . | k | -1 . Let } ˜˜ T D (µ ph , σ ˜ 2 ph ) = ∑ h i = 1 ˜ Dpi and ˜˜˜ T D I (µ I ph , σ 2 I ph ) = ∑ h i = 1 ˜ D I pi denote the total actual fuzzy cumulative demand and the total actual intuitionistic fuzzy cumulative demand at the customer ph for h ∈ { 2 , 3 4 , , . . . | P | -1 . }

Givenaroute P = ( p 1 , p 2 , . . . , p P | | ) , let ˜˜ EFC (µ ph , σ ˜ 2 ph ) and ˜ EFC I (µ ˜ I ph , σ ˜ 2 I ph ) denote the expected fuzzy and expected intuitionistic fuzzy failure cost at customer ph , respectively. It can be written as

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

P ( G ( ˜ T D (µ ˜ ph , σ ˜ 2 ph ) ) ≤ uQ ) therefore indicates the fuzzy probability of having the u th failure at ph customer when the demands are given by discrete fuzzy random variables with the condition that failure has yet not occurred on any other node which has been previously visited on the route. P ( f ( ˜ T D I (µ ˜ I ph -1 , σ ˜ 2 I ph -1 ) ) ≤ uQ ) -P ( f ( ˜ T D (µ ˜ I ph , ˜ σ 2 I ph ) ) ≤ uQ ) thus indicates the intuitionistic fuzzy probability of having the u th failure at ph customerunderthesame conditions as explained for the case when demands are given by fuzzy random variables.

## 3.1 Mathematical model for CVRPFSD

<!-- formula-not-decoded -->

Table 1 Description of symbols used in mathematical model

| Symbol                              | Description of the symbol                                                                                                 |
|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| C                                   | Set of customers in the network                                                                                           |
| c i j                               | Cost of traversal of edge i j                                                                                             |
| P = ( p 1 , p 2 , . . . , p | P | ) | A route starting and ending at p 1 and p | P | , respectively                                                             |
| ˜ D i                               | A fuzzy random variable representing the demands of customer at node p i                                                  |
| ˜ D I i                             | An intuitionistic fuzzy random variable representing the demands of the customer at node p i                              |
| P ( E )                             | Probability of an event E                                                                                                 |
| ˜ T D ( ˜ µ p h , ˜ σ 2 p h )       | Fuzzy cumulative demand at customer p h in the route p                                                                    |
| ˜ T D I ( ˜ µ I p h , ˜ σ 2 I p h ) | Intuitionistic Fuzzy cumulative demand at customer p h in the route p                                                     |
| Q                                   | Capacity of the vehicle                                                                                                   |
| G ( ˜ A )                           | GMIR representation of ˜ A                                                                                                |
| f ( ˜ A I )                         | Score function of TIFN ˜ A I                                                                                              |
| ˜ EFC I (µ ˜ I p h , σ 2 I p h )    | Fuzzy effective failure cost at customer p h of the route P                                                               |
| λ p                                 | A binary decision variable whose value is 1 when route P is traversed and 0 otherwise                                     |
| α i p                               | A binary decision variable whose entry is 1 when node i is traversed in route P and 1 when node i is traversed in route P |
| R                                   | Set of all the routes                                                                                                     |
| E [ ˜ X                             | Expected value of a fuzzy random variable X                                                                               |
| ] ϑ F ( n )(ϑ I F ( n ))            | Lower bound on the number of vehicles when demands are given by Fuzzy (intuitionistic fuzzy) random variable              |

subject to

<!-- formula-not-decoded -->

## 3.2 Mathematical model for CVRPIFSD

In the mathematical models proposed above, α i p and λ p are binary decision variables. The value of λ p is 1 if route p is chosen and 0 otherwise. α i p is also a binary decision variable whichtakes the value 1 if the node i is traversed while traversing the route p . R is the set of all feasible routes originating and terminating at the source node. The terms ϑ F ( n ) and ϑ I F ( n ) are lower bounds on the number of vehicles required when the service is performed by a fleet of vehicles or the number of times the vehicle may be required to return to source node because of failure in fulfilling the fuzzy and intuitionistic fuzzy demands of the customers, respectively, if only one vehicle is used. These can be easily computed by the formulas given in Eqs. (17 and (18), respectively.

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

subject to

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

where Q is the capacity of the vehicle.

The objectives of minimization of operation cost are represented by Eqs. (9) and (13) for CVRPFSD and CVRPIFSD, respectively. The condition of serving each customer exactly once is represented by Eqs. (10) and (14). The use of minimum number of vehicles needed for successful execution

(13)

Algorithm 1 An algorithm to calculate the a priori route by using Branch and Bound algorithm

## Input: C n [ ][ n ] .

## Output: The deterministic cost c .

- 1: Start.
- 2: C n [ ][ n ] ← cost matrix.
- 4: v i si t [ n ] ← source node.
- 3: Un isited n v [ ] ← Set of all customers .
- 5: for all i = 0 to n do

j 0 to n do

- 7: p i [ ] ← min C i ( [ ][ j ] )
- 6: for all =
- 8: C i [ ][ j ] ← C i [ ][ j ] -p i [ ];
- 9: end for
- 10: end for
- 11: for all j = 0 to n do

12:

13:

for all i = 0 to n do

14:

t [ j ] ← min C i ( [ ][ j ] ) ;

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

27:

28:

29:

30:

31:

32:

33:

34:

35:

36:

37:

38:

39:

40:

41:

42:

43:

44:

45:

46:

47:

48:

49:

50:

C i [ ][ j ] ← C i [ ][ j ] -t [ j ];

end for end for

C ← 0

j

] ←

C i

[

][

j

];

while

VC i [ ][ l , k , i ← 0

v

is not empty for all

Un isited n [ ] for k ∈ v i si t ed [ n ] do do

end for j = 0 to n C k [ ][ j ] ← ∞

end for for l ∈ Un isited n v [ ] do

C i [ ][ ] ← ∞ l for all i = 0 to n do

end for

C l [ ][ k ] ← ∞

for all i = 0 to n do

V ← VC l [ ][ k ]

for all j = 0 to n do

C i [ ][ j ] ← C i [ ][ j ] -p i [ ];

p i [ ] ← min C i ( [ ][ j ] ) ;

end for end for

for all j = 0 to n do t [ j ] ← min C i ( [ ][ j ] ) ;

for all i = 0 to n do

C i [ ][ j ] ← C i [ ][ j ] -t [ j ];

end for end for

for all

0

to n do

C l [ ] ← C + p i [ ] + t [ j ];

i = for all j = 0 to n do end for

end for l ← + l 1

end for

- 51: m ← min C l ( [ ] ) ;

53:

- 52: C ← m + c

Un isited n

v

v

[

] ←

i si t ed

n

] ←

- 56: end while
- 54: [ 55: k ← + ; k 1
- 57: return c

v

Un isited n

v

i si t ed

n

[

] ∪

l

;

;

[

do

] -

l

;

is governed by Eqs. (11) and (15) for CVRPFSD and CVRPIFSD, respectively. The constraint for route selection for CVRPFSD and CVRPIFSD is represented by Eqs. (12) and (16), respectively.

Algorithm 2 An algorithm for calculating the Total fuzzy effective failure cost

## Input:

- 1. Fuzzy random variable ˜ Di representing demands of every customer.
- 3. Capacity, Q of the vehicle.
- 2. r = ( d , r 1 , r 2 , . . . , r n , d ) .

## Output: The total effective failure cost

```
1: Start. 2: i ← 1 3: ˜ T E FC ← 0 4: ˜ Prob Cr ( 0 ) ← ( 1 1 1 , , ) 5: for i ≤ n do 6: ˜ Effective failure Prob ( Cr i ) ← ( 0 , 0 , 0 ) 7: ˜ ( Cum-dem ) [ r i ] = ˜ D r i [ - ] ⊕ 1 ˜ D r i [ ] 8: ˜ P Cr i ( ) = ˜ P Cr i ( -1 ) /circledot ˜ P Cr i ( ) 9: u ← 1 10: if G (( ˜ Cum-dem ) [ r i ] ) ≤ Qu then 11: ˜ Prob Cr i ( ) = P ( G ( ˜ Cum-dem [ r i ] ) ≤ Qu ) 12: ˜ ˜ Failure Prob ( Cr i ) = ˜ Prob ( Cr i -1 ) /circleminus ˜ Prob ( Cr i ) 13: Effective Failure Prob ( Cr 1 ) = ˜ Effective Failure Prob ( Cr i ) + ˜ Failure Prob ( Cr i ) 14: if u = ϑ F ( n ) + 1 then 15: ˜ ˜ Effective failure cost = 2 c 0 r i ˜ Effective failure Prob ( Cr i ) 16: ˜ T E FC = T E FC + ˜ Effective failure cost ( Cr i ) 17: else 18: u = u + 1 19: end if 20: else 21: u = u + 1 22: end if 23: end for 24: return ˜ T E FC
```

## 4 Methodology

In this work, a two-level stochastic process has been defined for solving CVRPFSD and CVRPIFSD. The first level corresponds to the determination of an a priori route and the cost associated with that route. Various algorithms used for solving a TSP can be used for this purpose. In this work, we have used branch-and-bound algorithm for this purpose because the method ensures an optimal solution. The second level corresponds to the execution of the a priori route found in the first level and serving the customers. While executing the a priori route, a route failure may occur and in the second stage, an effective fuzzy failure cost corresponding to every customer in the network is calculated. The sum of deterministic cost obtained in stage 1 and effective failure cost obtained in stage 2 gives the cost of operation. Table 1 comprises of the various symbols used in the algorithm and their descriptions.

## 4.1 Algorithm

The stage 1 of methodology is represented by Algorithm 1. The stage 2 of methodology for fuzzy and intuitionistic fuzzy demands is represented by Algorithms 2 and 3, respectively.

Algorithm3 Analgorithmfor calculating the Total Intuitionistic fuzzy effective failure cost

Input:

- 1. Intuitionistic Fuzzy random variable ˜ D I i representing demands of every customer.
- 2. r = ( d , r 1 , r 2 , . . . , r n , d ) .
- 3. Capacity, Q of the vehicle.

## Output: The total effective failure cost

```
1: Start. 2: i ← 1 3: ˜ T E FC I ← ( 0 , 0 , 0 ; 0 , 0 , 0 ) 4: ˜ Prob I ( Cr 0 ) ← ( 1 1 1 , , ; 1 1 1 , , ) 5: for i ≤ n do 6: ˜ Effective failure Prob I ( Cr i ) ← ( 0 , 0 , 0 ; 0 , 0 , 0 ) 7: ( ˜ Cum-dem I ) [ r i ] = ˜ D I [ r i - ] ⊕ 1 ˜ D I [ r i ] 8: ˜ P I ( Cr i ) = ˜ P I ( Cr i -1 ) /circledot ˜ P I ( Cr i ) 9: u ← 1 10: if f (( ˜ Cum-dem I ) [ r i ] ) ≤ Qu then 11: ˜ Prob I ( Cr i ) = ˜ ( P I f ( ˜ Cum-dem I [ r i ] ) ≤ Qu ) 12: ˜ ˜ Failure Prob I ( Cr i ) = ˜ Prob I ( Cr i -1 ) /circleminus ˜ Prob I ( Cr i ) 13: Effective Failure Prob I ( Cr 1 ) = ˜ Effective Failure Prob I ( Cr i ) ⊕ ˜ Failure Prob I ( Cr i ) 14: if u = ϑ I F ( n ) + 1 then 15: ˜ ˜ Effective failure cost I = 2 c 0 r i /circledot ˜ Effective failure Prob ( Cr i ) 16: ˜ T E FC I = T E FC I + ˜ Effective failure cost I ( Cr i ) 17: else 18: u = u + 1 19: end if 20: else 21: u = u + 1 22: end if 23: end for 24: return ˜ T E FC I
```

## 4.2 Flow chart

Figures 4 and 5 represent the flow chart for methodology of stage 1 and stage 2, respectively.

## 5 Result and discussion

## 5.1 Numerical examples for CVRPFSD

Example 2 To illustrate the working of the mathematical modelthathasbeenexplainedinSect.3.1,letusassumeanetwork with 7 customers waiting for the goods to be delivered. The depot node is denoted by Node-0, and the customers are waiting at the remaining nodes. The information about the

Fig. 4 Flow chart for stage 1

![Image](image_000004_286c23d4203e47347838199ce24eebfd240aaa3efec1474e4a0d345ec11ce205.png)

![Image](image_000005_013245c3d397a096800217797d2aa5eb65d69998b2373ddf023686194e40d84d.png)

cost of traversal between various nodes is given by C 1, where

```
C 1 =             ∞ 4 8 3 9 6 5 11 4 ∞ 9 6 10 3 4 6 8 9 ∞ 8 4 8 12 5 3 6 8 ∞ 9 8 7 3 9 10 4 9 ∞ 11 12 2 6 3 8 8 11 ∞ 8 15 5 4 12 7 12 8 ∞ 12 11 6 5 3 2 15 12 ∞            
```

The demand at the depot node is considered to be 0 units, and the fuzzy stochastic demands for the customers in the network are presented in Table 2. The carrying capacity of the vehicle is assumed to be 30 units.

In order to find the number of vehicles required, we calculate the expected fuzzy demands for the customers present in the network. The total actual cumulative demand of the network will be the sum of all expected fuzzy demands, i.e.

<!-- formula-not-decoded -->

Fig. 5 Flow chart for stage 2

![Image](image_000006_0f75bf44161afa4269df96478296812ce97ff9fa46f0ce9b661678b14d8b4758.png)

Table 2 Fuzzy stochastic demands for Example 2

| Node   | Fuzzy demands   | Fuzzy probability   |
|--------|-----------------|---------------------|
| 1      | (4, 6, 8)       | (0.45, 0.50, 0.55)  |
|        | (8, 10, 12)     | (0.45, 0.50, 0.55)  |
| 2      | (14, 16, 18)    | (0.75, 0.80, 0.85)  |
|        | (18, 20, 22)    | (0.15, 0.20, 0.25)  |
| 3      | (8, 10, 12)     | (0.30, 0.35, 0.40)  |
|        | (13, 15, 17)    | (0.60, 0.65, 0.70)  |
| 4      | (8, 10, 12)     | (0.25, 0.30, 0.35)  |
|        | (2, 4, 6)       | (0.65, 0.70, 0.75)  |
| 5      | (12, 14, 16)    | (0.35, 0.40, 0.45)  |
|        | (5, 7, 9)       | (0.55, 0.60, 0.65)  |
| 6      | (3, 5, 7)       | (0.58, 0.63, 0.68)  |
|        | (11, 13, 15)    | (0.22, 0.27, 0.32)  |
| 7      | (12, 15, 18)    | (0.40, 0.45, 0.50)  |
|        | (4, 7, 10)      | (0.50, 0.55, 0.60)  |

Defuzzifying the above triangular fuzzy number by using graded mean integration representation method gives

<!-- formula-not-decoded -->

In this example, the capacity of one vehicle is assumed to be 30 units and since there are chances of route failure, so it is better to approximate the number of times the vehicle should return to the source node and continue the service again. The number of times the vehicle should return to the source node can be obtained by dividing the total cumulative fuzzy demand by the capacity of the vehicle.

<!-- formula-not-decoded -->

Table 4 Fuzzy stochastic demands for Example 3

| Node   | Fuzzy demands   | Fuzzy probability   |
|--------|-----------------|---------------------|
| 1      | (8, 10, 12)     | (0.45, 0.50, 0.55)  |
|        | (13, 15, 17)    | (0.45, 0.50, 0.55)  |
| 2      | (18, 20, 22)    | (0.55, 0.60, 0.65)  |
|        | (8, 10, 12)     | (0.35, 0.40, 0.45)  |
| 3      | (23, 25, 27)    | (0.25, 0.30, 0.35)  |
|        | (28, 30, 32)    | (0.65, 0.70, 0.75)  |
| 4      | (20, 22, 24)    | (0.75, 0.80, 0.85)  |
|        | (13, 15, 17)    | (0.15, 0.20, 0.25)  |
| 5      | (8, 10, 12)     | (0.35, 0.40, 0.45)  |
|        | (16, 18, 20)    | (0.55, 0.60, 0.65)  |

Thus, in order to accomplish the demands of the customers present in the network, minimum 3 vehicles are required or a single vehicle is required to make three trips. In this model, the routes are designed at a priori with the help of branch-and-bound algorithm. Applying branch-and-bound algorithm on the cost matrix given by C , the path that the salesman should follow is given by 0 → 6 → 1 → 5 → 2 → 4 → 7 → 3 → 0 and the cost incurred on travelling this route comes out to be 32 units. Till now, the route which a travelling salesperson should take if he does not have any demands to fulfil has been figured out. But in the model explained above, there are two constraints in stage 2, one being the maximum carrying capacity of vehicle to be 30 units and second being the fuzzy and stochastic nature of the customers' demands, which can only be known while visiting a customer. In order to satisfy these two constraints, we find the effective failure cost at every node by using the formula given in Eq. (7). We note that effective failure cost at the depot node is 0. Table 3 comprises of comparison of method presented in this work with various other methods used for finding a priori route. Figure6 represents the comparison of total fuzzy cost obtained by using various methods. The com-

Table 3 Comparison of various methods for CVRPFSD presented in Example 2

| Name of the method          | Path              |   Path cost | Effective failure cost       | Total cost                   |
|-----------------------------|-------------------|-------------|------------------------------|------------------------------|
| Brute force approach        | 0-6-1-5-2-4-7-3-0 |          32 | ( - 103.92, 26.09, 156.11)   | ( - 71.92, 58.09, 188.11)    |
| Brute force approach        | 0-3-7-4-2-5-1-6-0 |          32 | ( - 91.879, 27.929, 147.738) | ( - 59.879, 59.929, 179.738) |
| Clark and Wright algorithm  | 0-4-7-2-5-1-6-3-0 |          41 | ( - 83.52, 22.688, 128.90)   | ( - 42.52, 63.68, 169.90)    |
| Christofides Algorithm      | 0-3-7-4-2-5-1-6-0 |          32 | ( - 91.879, 27.929, 147.738) | ( - 59.879, 59.929, 179.738) |
| Genetic algorithm           | 0-3-7-2-5-1-4-6-0 |          49 | ( - 85.69, 28.063, 141.824)  | (36.69, 77.06, 190.82)       |
| Bellman-Held-Karp algorithm | 0-6-1-5-2-4-7-3-0 |          32 | ( - 103.92, 26.09, 156.11)   | ( - 71.92, 58.09, 188.11)    |
| Bellman-Held-Karp algorithm | 0-3-7-4-2-5-1-6-0 |          32 | ( - 91.879, 27.929, 147.738) | ( - 59.878, 59.929, 179.738) |
| Nearest Neighbour           | 0-3-7-4-2-5-1-6-0 |          32 | ( - 91.879, 27.929, 147.738) | ( - 59.878, 59.929, 179.738) |
| Proposed method             | 0-6-1-5-2-4-7-3-0 |          32 | ( - 103.92, 26.09, 156.11)   | ( - 71.92, 58.09, 188.11)    |

![Image](image_000007_8127e59b031655297a7b2f1399753dbd9059a6440939103b21996f6e0f6da526.png)

Fig. 6 Total cost by using various methods for CVRPFSD presented in Example 2

![Image](image_000008_93918a6b89fee2a74a149fe365963897034f28f60cd48a5316c75883f11ce634.png)

Table 5 Comparison of various methods for CVRPFSD presented in Example 3

| Name of the method          | Path          |   Path cost | Effective failure cost      | Total cost                   |
|-----------------------------|---------------|-------------|-----------------------------|------------------------------|
| Brute force approach        | 0-3-5-4-1-2-0 |         130 | ( - 217.35, 148.09, 513.55) | ( - 87.35, 278.099, 643.55)  |
| Brute force approach        | 0-2-1-4-5-3-0 |         130 | ( - 246.22, 102.58, 451.39) | ( - 116.22, 232.58, 581.39)  |
| Clark and Wright algorithm  | 0-5-2-3-4-1-0 |         157 | ( - 214.63, 102.44, 419.52) | ( - 57.63, 259.448, 576.526) |
| Christofides algorithm      | 0-3-2-1-4-5-0 |         140 | ( - 205.15, 151.80, 508.76) | ( - 65.15, 291.80, 648.76)   |
| Genetic Algorithm           | 0-2-3-4-5-1-0 |         171 | ( - 205.72, 125.79, 457.30) | ( - 34.72, 296.79, 628.30)   |
| Bellman-Held-Karp algorithm | 0-3-5-4-1-2-0 |         130 | ( - 217.35, 148.09, 513.55) | ( - 87.35, 278.099, 643.55)  |
| Bellman-Held-Karp algorithm | 0-2-1-4-5-3-0 |         130 | ( - 246.22, 102.58, 451.39) | ( - 116.22, 232.58, 581.39)  |
| Nearest Neighbour           | 0-3-4-5-2-1-0 |         137 | ( - 184.02, 152.89, 489.90) | ( - 47.02, 289.89, 626.80)   |
| Proposed method             | 0-2-1-4-5-3-0 |         130 | ( - 246.22, 102.58, 451.39) | ( - 116.22, 232.58, 581.39)  |

parison has been done on the network given in the numerical Example 2.

Table 6 Fuzzy stochastic demands for Example 4

Example 3 Wenowassume a network with 5 customers. The depot node is denoted by Node-0. The demand at the depot node is considered to be 0 units. The information about the cost of traversal between various nodes is given by C 2, where

<!-- formula-not-decoded -->

The fuzzy stochastic demands for the customers in the network are presented in Table 4. The carrying capacity of the vehicle is assumed to be 40 units.

Table 5 comprises of the comparison of the solutions obtained by various conventional methods defined in the literature for solving a TSP for Example 3. Figure7 represents the comparison of total fuzzy cost obtained by using various methods.

Example 4 Wenowassume a network with 4 customers. The depot node is denoted by Node-0, and the customers are waiting at the remaining nodes. The information about the cost

| Node   | Fuzzy demands   | Fuzzy probability   |
|--------|-----------------|---------------------|
| 1      | (10, 12, 14)    | (0.45, 0.50, 0.55)  |
|        | (13, 15, 17)    | (0.45, 0.50, 0.55)  |
| 2      | (8, 10, 12)     | (0.58, 0.63, 0.68)  |
|        | (11, 13, 15)    | (0.22, 0.27, 0.32)  |
| 3      | (6, 8, 10)      | (0.35, 0.40, 0.45)  |
|        | (10, 12, 14)    | (0.55, 0.60, 0.65)  |
| 4      | (5, 7, 9)       | (0.75, 0.80, 0.85)  |
|        | (10, 12, 14)    | (0.15, 0.20, 0.25)  |

of traversal between various nodes is given by C 3, where

<!-- formula-not-decoded -->

The demand at the depot node is considered to be 0 units, and the fuzzy stochastic demands for the customers in the network are presented in Table 6. Let the carrying capacity of the vehicle be assumed to be 30 units.

Fig. 7 Total cost by using various methods for CVRPFSD presented in Example 3

![Image](image_000009_ccb0a87514b6e4006e9065035ed5d457ed8085850c75ae7a5ef1186e8ff0324b.png)

Table 7 Comparison of various methods for CVRPFSD presented in Example 4

| Name of the method          | Path        |   Path cost | Effective failure cost   | Total cost               |
|-----------------------------|-------------|-------------|--------------------------|--------------------------|
| Brute force approach        | 0-1-2-3-4-0 |          27 | ( - 29.31, 19.74, 68.79) | ( - 2.31, 46.74, 95.79)  |
|                             | 0-4-3-2-1-0 |          27 | ( - 38.81, 13.28, 65.37) | ( - 11.81, 40.28, 92.37) |
| Clark and Wright Algorithm  | 0-1-2-4-3-0 |          30 | ( - 36.90, 14.52, 65.94) | ( - 6.90, 44.52, 95.94)  |
| Christofides Algorithm      | 0-2-1-3-4-0 |          30 | ( - 30.45, 19.74, 69.95) | ( - 0.45, 49.74, 99.93)  |
| Genetic Algorithm           | 0-2-1-3-4-0 |          30 | ( - 30.45, 19.74, 69.95) | ( - 0.45, 49.74, 99.93)  |
| Bellman-Held-Karp algorithm | 0-1-2-3-4-0 |          27 | ( - 29.31, 19.74, 68.79) | ( - 2.31, 46.74, 95.79)  |
|                             | 0-4-3-2-1-0 |          27 | ( - 38.81, 13.28, 65.37) | ( - 11.81, 40.28, 92.37) |
| Nearest Neighbour           | 0-2-1-4-3-0 |          33 | ( - 38.04, 14.52, 67.08) | ( - 5.04, 47.52, 100.08) |
| Proposed method             | 0-4-3-2-1-0 |          27 | ( - 38.81, 13.28, 65.37) | ( - 11.81, 40.28, 92.37) |

Fig. 8 Total cost by using various methods for CVRPFSD presented in Example 4

![Image](image_000010_6604dedc8bb002f8a105daf861cb27d2047f6e2467ec33f497b0e29f57087da3.png)

Table 7 comprises of the comparison of the solutions obtained by various conventional methods defined in the literature for solving a TSP for Example 4. Figure8 represents the comparison of total fuzzy cost obtained by using various methods.

cost of traversal between various nodes is given by C 4, where

## 5.2 Numerical examples for CVRPIFSD

Example 5 To illustrate the working of the mathematical modelthathasbeenexplainedinSect.3.2,letusassumeanetwork with 5 customers waiting for the goods to be delivered. The depot node is denoted by Node-0, and the customers are waiting at the remaining nodes. The information about the

![Image](image_000011_9a83b13df228e1ba14b3887f66318fe194f49a06bd49406526982ff10fd36d64.png)

The demand at the depot node is considered to be 0 units, and the intuitionistic fuzzy stochastic demands for the customers in the network are presented in Table 8. The carrying capacity of the vehicle is assumed to be 30 units.

Table 8 Intuitionistic fuzzy stochastic demands for Example 5

|   Node | Intuitionistic fuzzy demands                      | Intuitionistic fuzzy probability                                          |
|--------|---------------------------------------------------|---------------------------------------------------------------------------|
|      1 | (7, 8, 9; 6, 8, 10)                               | (0.25, 0.30, 0.35; 0.20, 0.30, 0.40)                                      |
|      2 | (10, 12, 14; 8, 12, 16) (12, 14, 16; 10, 14, 18)  | (0.35, 0.40, 0.45; 0.30, 0.40, 0.50) (0.55, 0.60, 0.65; 0.50, 0.60, 0.70) |
|      3 | (6, 7, 8; 5, 7, 9) (10, 12, 14; 8, 12, 16)        | (0.40, 0.45, 0.50; 0.35, 0.45, 0.55) (0.50, 0.55, 0.60; 0.45, 0.55, 0.65) |
|      4 | (13, 15, 17; 11, 15, 19) (18, 20, 22; 16, 20, 24) | (0.45, 0.50, 0.55; 0.40, 0.50, 0.60) (0.45, 0.50, 0.55, 0.40, 0.50, 0.60) |
|      5 | (8, 10, 12; 6, 10, 14) (13, 15, 17; 11, 15, 19)   | (0.75, 0.80, 0.85; 0.70, 0.80, 0.90) (0.15, 0.20, 0.25; 0.10, 0.20, 0.30) |

Table 9 comprises of the comparison of the solutions obtained by various conventional methods for Example 5. Figure9 represents the comparison of total intuitionistic fuzzy cost obtained by using various methods.

in the network are presented in Table 12. Let the carrying capacity of the vehicle be 25 units.

Example 6 Wenowassume a network with 4 customers. The depot node is denoted by Node-0, and the customers are waiting at the remaining nodes. The information about the cost of traversal between various nodes is given by C 5, where

<!-- formula-not-decoded -->

The demand at the depot node is considered to be 0 units, and the intuitionistic fuzzy stochastic demands for the customers in the network are presented in Table 10. Let the carrying capacity of the vehicle be 30 units.

Table 11 comprises of the comparison of the solutions obtained by various conventional methods for the example described in Example 6. Figure10 represents the comparison of total intuitionistic fuzzy cost obtained by using various methods.

Example 7 Assume a network with 4 customers waiting for the goods to be delivered. The depot node is denoted by Node0, and the customers are waiting at the remaining nodes. The information about the cost of traversal between various nodes is given by C 6, where

<!-- formula-not-decoded -->

The demand at the depot node is considered to be 0 units, and the intuitionistic fuzzy stochastic demands for the customers

Table 13 comprises of the comparison of the solutions obtained by various conventional methods for Example 7. Figure11 represents the comparison of total intuitionistic fuzzy cost obtained by using various methods.

Tables 3, 5, 7, 9, 11 and 13 represent the comparison of various methods such as nearest neighbour algorithm, Clark and Wright algorithm, genetic algorithm, Christofides algorithm, and many more for finding the a priori route, and then, the effective failure cost for these routes is also calculated. The sum of the effective failure cost and the deterministic cost of the routes gives the cost of operation. It has been observed that other than branch-and-bound algorithm, only BellmanHeld-Karp algorithm and brute force approach give optimal solution. The time complexity of Bellman-Held-Karp algorithm is exponential in nature, which is same as that of branch-and-boundalgorithm.IncaseofBellman-Held-Karp algorithm, all possible permutations are explored in all cases and hence more space in memory is required as compared to branch-and-bound algorithm. The brute force approach gives optimal solution by exploring all possible permutations and thus has a time complexity of O(n!), which is worse than that of branch and bound. Branch-and-bound algorithm has time complexity of O( n 2 2 ), which is also exponential in nature, n but no algorithm with lesser time complexity than branch and bound gives optimal solution for TSP. The major advantage of the algorithm is that we can control the quality of the solution to be expected, even if it is not yet found. Only in worst case scenario, the exploration of all possible permutations is required. Other algorithms such as Christofides algorithm, nearest neighbour algorithm, Clark and Wright algorithm, and genetic algorithm have polynomial time complexity but the solution obtained by these method is not always guaranteed to be optimal. It can be clearly observed from all the tables that the cost of operation obtained by using the proposed method is significantly lesser than the cost of operation obtained by using any other method.

![Image](image_000012_0c6b9d1fc7f4a754a939dad0b199b2d7e1b583f3501c152ab12d8a37d9e3005a.png)

ôtal Cost

Fig. 9 Total cost by using various methods for CVRPIFSD presented in Example 5

Fig. 10 Total cost by using various methods for CVRPIFSD presented in Example 6

![Image](image_000013_254d49bc641ac2f0e12ed680c1c455f0dd55a890e1af07d14a22129da21f0171.png)

Table 9 Comparison of various methods for CVRPIFSD presented in Example 5

| Name of the method          | Path                                                | Path cost   | Effective failure cost                                                                                                                     | Total cost                                                                                                                                      |
|-----------------------------|-----------------------------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Brute force approach        | 0 - 1 - 3 - 5 - 2 - 4 - 0 0 - 4 - 2 - 5 - 3 - 1 - 0 | 32 32       | ( - 68 . 49 , 22 . 40 , 108 . 28 ; 166 . 84 , 22 . 40 , 191 . 37 ) ( - 63 . 1817 , 26 . 072 , 109 . 91 ; - 161 . 07 , 26 . 07 , 191 . 30 ) | ( - 36 . 49 , 54 . 40 , 140 . 28 ; - 134 . 84 , 54 . 40 , 223 . 37 ) ( - 31 . 1817 , 58 . 0728 , 141 . 9108 ; - 129 . 07 , 58 . 07 , 223 . 30 ) |
| Clark and Wright algorithm  | 0-1-2-3-5-4-0                                       | 38          | ( - 64 . 47 , 24 . 32 , 108 . 07 ; - 162 . 04 , 24 . 32 , 189 . 64 )                                                                       | ( - 26 . 47 , 62 . 32 , 146 . 07 ; - 124 . 04 , 62 . 32 , 227 . 64 )                                                                            |
| Christofides algorithm      | 0-5-2-3-1-4-0                                       | 34          | ( - 55 . 70 , 23 . 93 , 99 . 23 ; - 141 . 55 , 23 . 93 , 171 . 96 )                                                                        | ( - 21 . 70 , 57 . 93 , 133 . 23 ; - 107 . 55 , 57 . 93 , 205 . 96 )                                                                            |
| Genetic algorithm           | 0-4-2-1-3-5-0                                       | 37          | ( - 49 . 57 , 24 . 37 , 94 . 04 ; - 129 . 63 , 24 . 37 , 161 . 16 )                                                                        | ( - 12 . 57 , 61 . 37 , 131 . 04 ; - 92 . 63 , 61 . 37 , 198 . 16 )                                                                             |
| Bellman-Held-Karp algorithm | 0 - 1 - 3 - 5 - 2 - 4 - 0 0 - 4 - 2 - 5 - 3 - 1 - 0 | 32 32       | ( - 68 . 49 , 22 . 40 , 108 . 28 ; 166 . 84 , 22 . 40 , 191 . 37 ) ( - 63 . 1817 , 26 . 072 , 109 . 91 ; - 161 . 07 , 26 . 07 , 191 . 30 ) | ( - 36 . 49 , 54 . 40 , 140 . 28 ; - 134 . 84 , 54 . 40 , 223 . 37 ) ( - 31 . 1817 , 58 . 0728 , 141 . 9108 ; - 129 . 07 , 58 . 07 , 223 . 30 ) |
| Nearest neighbour           | 0-4-3-1-2-5-0                                       | 37          | ( - 68 . 185 , 25 . 404 , 113 . 79 ; - 170 . 914 , 25 . 404 , 200 . 59 )                                                                   | ( - 31 . 185 , 62 . 404 , 150 . 79 ; - 133 . 914 , 62 . 404 , 237 . 59 )                                                                        |
| Proposed method             | 0-1-3-5-2-4-0                                       | 32          | ( - 68 . 49 , 22 . 40 , 108 . 28 ; 166 . 84 , 22 . 40 , 191 . 37 )                                                                         | ( - 36 . 49 , 54 . 40 , 140 . 28 ; - 134 . 84 , 54 . 40 , 223 . 37 )                                                                            |

Table 10 Intuitionistic fuzzy stochastic demands for Example 6

|   Node | Intuitionistic fuzzy demands                                    | Intuitionistic fuzzy probability                                                                                |
|--------|-----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
|      1 | ( 14 , 15 , 16 ; 13 , 15 , 17 ) ( 19 , 20 , 21 ; 18 , 20 , 22 ) | ( 0 . 45 , 0 . 50 , 0 . 55 ; 0 . 40 , 0 . 50 , 0 . 60 ) ( 0 . 45 , 0 . 50 , 0 . 55 ; 0 . 40 , 0 . 50 , 0 . 60 ) |
|      2 | ( 11 , 12 , 13 ; 10 , 12 , 14 ) ( 13 , 14 , 15 ; 12 , 14 , 16 ) | ( 0 . 35 , 0 . 40 , 0 . 45 ; 0 . 30 , 0 . 40 , 0 . 50 ) ( 0 . 55 , 0 . 60 , 0 . 65 ; 0 . 50 , 0 . 60 , 0 . 70 ) |
|      3 | ( 6 , 7 , 8 ; 5 , 7 , 9 ) ( 10 , 12 , 14 ; 8 , 12 , 16 )        | ( 0 . 40 , 0 . 45 , 0 . 50 ; 0 . 35 , 0 . 45 , 0 . 55 ) ( 0 . 50 , 0 . 55 , 0 . 60 ; 0 . 45 , 0 . 55 , 0 . 65 ) |
|      4 | ( 9 , 10 , 11 ; 8 , 10 , 12 ) ( 14 , 15 , 16 ; 13 , 15 , 17 )   | ( 0 . 75 , 0 . 80 , 0 . 85 ; 0 . 70 , 0 . 80 , 0 . 90 ) ( 0 . 15 , 0 . 20 , 0 . 25 ; 0 . 10 , 0 . 20 , 0 . 30 ) |

Table 11 Comparison of various methods for CVRPIFSD presented in Example 6

| Name of the method          | Path                                        | Path cost   | Effective failure cost                                                                                                                    | Total cost                                                                                                                                |
|-----------------------------|---------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Brute force approach        | 0 - 2 - 1 - 3 - 4 - 0 0 - 4 - 3 - 1 - 2 - 0 | 32 32       | ( - 60 . 67 , 31 . 52 , 118 . 04 ; - 159 . 98 , 31 . 52 , 200 . 22 ) ( - 74 . 03 , 24 . 65 , 117 . 35 ; - 180 . 25 , 24 . 65 , 205 . 46 ) | ( - 28 . 67 , 63 . 53 , 150 . 03 ; - 127 . 98 , 63 . 53 , 232 . 22 ) ( - 42 . 03 , 56 . 66 , 149 . 35 ; - 148 . 25 , 56 . 66 , 237 . 46 ) |
| Clark and Wright algorithm  | 0-1-2-3-5-4-0                               | 38          | ( - 64 . 47 , 24 . 32 , 108 . 07 ; - 162 . 04 , 24 . 32 , 189 . 64 )                                                                      | ( - 26 . 47 , 62 . 32 , 146 . 07 ; - 124 . 04 , 62 . 32 , 227 . 64 )                                                                      |
| Christofides algorithm      | 0-2-4-3-1-0                                 | 35          | ( - 68 . 44 , 36 . 77 , 135 . 59 ; - 181 . 72 , 36 . 77 , 230 . 77 )                                                                      | ( - 33 . 44 , 71 . 77 , 170 . 89 ; - 146 . 72 , 71 . 77 , 265 . 77 )                                                                      |
| Genetic algorithm           | 0-1-4-3-2-0                                 | 32          | ( - 59 . 59 , 36 . 45 , 126 . 87 ; - 162 . 95 , 36 . 45 , 213 . 24 )                                                                      | ( - 25 . 59 , 70 . 46 , 160 . 87 ; - 128 . 95 , 70 . 46 , 247 . 24 )                                                                      |
| Bellman-Held-Karp algorithm | 0 - 2 - 1 - 3 - 4 - 0 0 - 4 - 3 - 1 - 2 - 0 | 32 32       | ( - 60 . 67 , 31 . 52 , 118 . 04 ; - 159 . 98 , 31 . 52 , 200 . 22 ) ( - 74 . 03 , 24 . 65 , 117 . 35 ; - 180 . 25 , 24 . 65 , 205 . 46 ) | ( - 28 . 67 , 63 . 53 , 150 . 03 ; - 127 . 98 , 63 . 53 , 232 . 22 ) ( - 42 . 03 , 56 . 66 , 149 . 35 ; - 148 . 25 , 56 . 66 , 237 . 46 ) |
| Nearest neighbour           | 0-4-2-1-3-0                                 | 39          | ( - 82 . 79 , 25 . 25 , 126 . 32 ; 161 . 43 , 64 . 25 , 261 . 84 )                                                                        | ( - 43 . 79 , 64 . 25 , 165 . 32 ; 161 . 43 , 64 . 25 , 261 . 84 )                                                                        |
| Proposed method             | 0-4-3-1-2-0                                 | 32          | ( - 74 . 03 , 24 . 65 , 117 . 35 ; - 180 . 25 , 24 . 65 , 205 . 46 )                                                                      | ( - 42 . 03 , 56 . 65 , 149 . 35 ; - 148 . 25 , 56 . 65 , 237 . 46 )                                                                      |

Table 12 Intuitionistic fuzzy demands for Example 7

|   Node | Intuitionistic fuzzy demands                                    | Intuitionistic fuzzy probability                                                                                |
|--------|-----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
|      1 | ( 10 , 12 , 14 ; 8 , 12 , 16 ) ( 13 , 15 , 17 ; 11 , 15 , 19 )  | ( 0 . 35 , 0 . 40 , 0 . 45 ; 0 . 30 , 0 . 40 , 0 . 50 ) ( 0 . 55 , 0 . 60 , 0 . 65 ; 0 . 50 , 0 . 60 , 0 . 70 ) |
|      2 | ( 9 , 10 , 11 ; 8 , 10 , 12 ) ( 12 , 13 , 14 ; 11 , 13 , 15 )   | ( 0 . 15 , 0 . 20 , 0 . 25 ; 0 . 10 , 0 . 20 , 0 . 30 ) ( 0 . 75 , 0 . 80 , 0 . 85 ; 0 . 70 , 0 . 80 , 0 . 90 ) |
|      3 | ( 7 , 8 , 9 ; 6 , 8 , 10 ) ( 8 , 10 , 12 ; 6 , 10 , 14 )        | ( 0 . 65 , 0 . 70 , 0 . 75 ; 0 . 60 , 0 . 70 , 0 . 80 ) ( 0 . 25 , 0 . 30 , 0 . 35 ; 0 . 20 , 0 . 30 , 0 . 40 ) |
|      4 | ( 18 , 20 , 22 ; 16 , 20 , 24 ) ( 15 , 17 , 19 ; 13 , 17 , 21 ) | ( 0 . 45 , 0 . 50 , 0 . 55 ; 0 . 40 , 0 . 50 , 0 . 60 ) ( 0 . 45 , 0 . 50 , 0 . 55 ; 0 . 40 , 0 . 50 , 0 . 60 ) |

## 5.3 Benchmark instances

The benchmark instances for CVRP given by Set P defined by Augerat (1995) (7 instances), Set A defined by Augerat (1995) (10 instances) and Set E defined by Christofides and Eilon (1969) (3 instances) are modified for the imprecise and random environment. The cost of traversal of edges in the network is taken as the distance which is calculated by using Euclidean distance norm. The coordinates of the vertices are taken as the same given in the benchmark instances.

The carrying capacity of the vehicle is considered same as that of benchmark instances. Two realizations for the fuzzy demand of the customers in the network are taken, where one realization is obtained by fuzzifying the demand as given in the benchmark instance and another realization is generated randomly in neighbourhood of first demand. The probability distribution function for customers' demand is also generated randomly. Table 14 comprises of the results of some benchmark instances obtained by using the proposed methodology.

Table 13 Comparison of various methods for CVRPIFSD presented in Example 7

| Name of the method          | Path                                        | Path cost   | Effective failure cost                                                                                                                  | Total cost                                                                                                                         |
|-----------------------------|---------------------------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Brute force approach        | 0 - 3 - 1 - 2 - 4 - 0 0 - 4 - 2 - 1 - 3 - 0 | 46 46       | ( - 41 . 61 , 44 . 00 , 125 . 55 ; - 132 . 47 , 44 . 00 , 204 . 12 ) ( - 28 . 93 , 36 . 22 , 97 . 68 ; - 98 . 49 , 36 . 22 , 156 . 10 ) | ( 4 . 38 , 90 . 00 , 171 . 55 ; - 86 . 47 , 90 . 00 , 250 . 12 ) ( 17 . 06 , 82 . 22 , 143 . 68 ; - 52 . 49 , 82 . 22 , 202 . 10 ) |
| Clark and Wright algorithm  | 0-2-4-3-1-0                                 | 56          | ( - 21 . 96 , 95 . 78 , 155 . 25 ; - 31 . 34 , 95 . 78 , 213 . 68 )                                                                     | ( 34 . 03 , 95 . 78 , 155 . 25 ; - 31 . 34 , 95 . 78 , 213 . 68 )                                                                  |
| Christofides algorithm      | 0-3-1-2-4-0                                 | 46          | ( - 41 . 61 , 44 . 00 , 125 . 55 ; - 132 . 47 , 44 . 00 , 204 . 12 )                                                                    | ( 4 . 38 , 90 . 04 , 171 . 55 ; - 86 . 47 , 90 . 04 , 250 . 12 )                                                                   |
| Genetic algorithm           | 0-3-1-4-2-0                                 | 47          | ( - 42 . 97 , 41 . 33 , 121 . 90 ; - 132 . 59 , 41 . 33 , 200 . 18 )                                                                    | ( 4 . 022 , 88 . 33 , 168 . 90 ; - 85 . 59 , 88 . 33 , 247 . 18 )                                                                  |
| Bellman-Held-Karp algorithm | 0 - 3 - 1 - 2 - 4 - 0 0 - 4 - 2 - 1 - 3 - 0 | 46 46       | ( - 41 . 61 , 44 . 00 , 125 . 55 ; - 132 . 47 , 44 . 00 , 204 . 12 ) ( - 28 . 93 , 36 . 22 , 97 . 68 ; - 98 . 49 , 36 . 22 , 156 . 10 ) | ( 4 . 38 , 90 . 00 , 171 . 55 ; - 86 . 47 , 90 . 00 , 250 . 12 ) ( 17 . 06 , 82 . 22 , 143 . 68 ; - 52 . 49 , 82 . 22 , 202 . 10 ) |
| Nearest neighbour           | 0-3-1-4-2-0                                 | 47          | ( - 42 . 97 , 41 . 33 , 121 . 90 ; - 132 . 59 , 41 . 33 , 200 . 18 )                                                                    | ( 4 . 022 , 88 . 33 , 168 . 90 ; - 85 . 59 , 88 . 33 , 247 . 18 )                                                                  |
| Proposed method             | 0-4-2-1-3-0                                 | 46          | ( - 28 . 93 , 36 . 22 , 97 . 68 ; 98 . 44 , 36 . 22 , 156 . 10 )                                                                        | ( 17 . 06 , 82 . 22 , 143 . 68 ; 52 . 49 , 82 . 22 , 202 . 10 )                                                                    |

-

-

![Image](image_000014_c81f3428b680f39037add9f5953df12223991cbb707bcc2acc9c683b4dbdc5f1.png)

otal Cost

Fig. 11 Total cost by using various methods for CVRPIFSD presented in Example 7

Table 15 comprises of the comparison of 20 modified versions of benchmark instances in various cases. The column entitled as optimal cost represents the cost when the demands are deterministic in nature and it presents the cost as given in VRP repository. The second column entitled as genetic algorithm represents the cost when the demands were deterministic in nature and the algorithm used for solving the CVRP was based on genetic algorithm. There are several algorithms which have been defined in the recent decades for solving CVRP with deterministic demands. The columns entitled as branch cut and price and branch and price represent the cost of operation when the demands of the customers are stochastic in nature. In both the methods, the demands of the customers are assumed to follow a Poisson distribution where the expected value of the demand at every customer is equal to the deterministic demand of the customer as given in the benchmark instances. As far as the knowledge of authors is concerned, no benchmark datasets have been defined in the literature for solving CVRP with stochastic demands or

CVRP with fuzzy stochastic demands. The modification of benchmark datasets, as provided for deterministic demands, is done and solved. The imprecise nature of demands has not received much attention by the researchers, and not many algorithms have been developed for the same. The column entitled Simheuristic approach comprises of the cost when the demands of the customers are fuzzy in nature. The fuzzy value of demands and remaining capacity of vehicles are defuzzified by using the centre of gravity method, which ultimately results in the lost of data representing the impreciseness as well as the shape of the fuzzy variables used. The last column entitled as proposed approach represents the expected cost of route traversal when the demands of the customers represent both attributes, namely impreciseness and randomness. In the proposed approach, no defuzzification techniques are used, as a result of which no important information about the demands and shape of the variables representing demands is lost. The output obtained is impre-

Table 14 Results for benchmark instances with modified fuzzy stochastic demands

| Instance   |   Deterministic cost | Effective failure cost                | Total cost                             |
|------------|----------------------|---------------------------------------|----------------------------------------|
| P-n16-k8   |              154.4   | ( - 5394.58, 56.83, 5508.26)          | ( - 5240.189, 211.236, 5662.662)       |
| P-n19-k2   |              171.98  | ( - 2839.51, 66.39, 2972.31)          | ( - 2667.53, 238.37, 3144.29)          |
| P-n20-k2   |              178.64  | ( - 3773.39, 44.56, 3866.01)          | ( - 3594.75, 223.20, 4044.65)          |
| P-n21-k2   |              180.83  | ( - 4236.45, 43.19, 4322.84)          | ( - 4055.62, 224.02, 4503.67)          |
| P-n22-k2   |              184.94  | ( - 4764.50, 43.056, 4850.62)         | ( - 4579.56, 227.99, 5035.56)          |
| P-n22-k8   |              278.43  | ( - 8708.64, 414.93, 9538.51)         | ( - 8430.21, 693.36, 9816.94)          |
| P-n23-k8   |              185.34  | ( - 10,607.27, 317.25, 11241.77)      | ( - 10,421.93, 502.59, 11427.114)      |
| E-n13-k4   |              142     | ( - 3178.58, 114.25, 3407.09)         | (3036.58, 256.254, 3549.092)           |
| E-n22-k4   |              278.43  | ( - 5090.354, 192.11, 5474.57)        | ( - 4811.92, 470.54, 5753.00)          |
| E-n23-k3   |              470.04  | ( - 13,039.81, 189.45, 13418.71)      | ( - 12,569.77, 659.49, 13,888.753)     |
| A-n32-K5   |              467.1   | ( - 47,732.89, 551.116, 48,868.295)   | ( - 47,265.78, 1018.216, 49,335.395)   |
| A-n33-K5   |              436.459 | ( - 41,746.490, 413.318, 42,572.911)  | ( - 41,310.0406, 849.76, 43,009.36)    |
| A-n33-K6   |              482.9   | ( - 39,738.115, 451.5131, 40,641.142) | ( - 39,255.2151, 934.4131, 4124.0424)  |
| A-n34-K5   |              490.29  | ( - 56,239.0028, 411.1219, 57,052.35) | ( - 55,748.7128, 901.411, 57,542.6493) |
| A-n36-K5   |              480.62  | ( - 73,853.27, 546.31, 74,936.68)     | ( - 73,372.66, 1026.92, 75,417.29)     |
| A-n37-K5   |              517.13  | ( - 49,392.35, 341.61, 50,075.65)     | ( - 48,875.25, 858.747, 50,592.78)     |
| A-n37-K6   |              510.46  | ( - 58,830.54, 688.66, 60,208.29)     | ( - 58,320.08, 1199.12, 60,718.75)     |
| A-n38-K5   |              469.54  | ( - 52,296.38, 393.42, 53,078.25)     | ( - 51,826.83, 862.96, 53,547.79)      |
| A-n39-K5   |              542.02  | ( - 76,493.56, 535.45, 77,559.47)     | ( - 75,951.54, 1077.47, 78,101.49)     |
| A-n39-K6   |              548.24  | ( - 60,740.21, 519.55, 61,777.35)     | ( - 60,191.97, 1067.79, 62,325.58)     |

Table 15 Comparison of various methods on modified benchmark datasets

| Instances   | Deterministic demands   | Deterministic demands                | Stochastic demands                        | Stochastic demands                                | Fuzzy demands                                  | Fuzzy stochastic demands               |
|-------------|-------------------------|--------------------------------------|-------------------------------------------|---------------------------------------------------|------------------------------------------------|----------------------------------------|
| Instances   | Optimal cost            | Genetic algorithm (Awad et al. 2018) | Branch cut and price (Gauvin et al. 2014) | Branch and price (Christiansen and Lysgaard 2007) | Simheuristic approach (Tordecilla et al. 2021) | Proposed approach                      |
| A-n32-K5    | 784                     | 784                                  | 853.6                                     | 890.13                                            | 1245.4                                         | ( - 47,265.78, 1018.216, 49,335.395)   |
| A-n33-K5    | 661                     | 661                                  | 704.2                                     | 722.99                                            | 988.8                                          | ( - 41,310.0406, 849.76, 43,009.36)    |
| A-n33-K6    | 742                     | 742.9                                | 793.9                                     | 816.58                                            | 990.8                                          | ( - 39,255.2151, 934.4131, 4124.0424)  |
| A-n34-K5    | 778                     | 782                                  | 826.9                                     | 839.95                                            | -*                                             | ( - 5,5748.7128, 901.411, 57,542.6493) |
| A-n36-K5    | 799                     | 808.8                                | 858.7                                     | 907.55                                            | -*                                             | ( - 73,372.66, 1026.92, 75,417.29)     |
| A-n37-K5    | 669                     | 673.2                                | 708.3                                     | 709.83                                            | 1044.3                                         | ( - 48,875.25, 858.747, 50,592.78)     |
| A-n37-K6    | 949                     | 949.7                                | 1030.7                                    | 1069.32                                           | -*                                             | ( - 58,320.08, 1199.12, 60,718.75)     |
| A-n38-K5    | 730                     | 731.2                                | 775.1                                     | 831.99                                            | 1118.5                                         | ( - 51,826.83, 862.96, 53,547.79)      |
| A-n39-K5    | 822                     | 823.5                                | 869.2                                     | 903.26                                            | -*                                             | ( - 75,951.54, 1077.47, 78,101.49)     |
| A-n39-K6    | 831                     | 832.5                                | 876.6                                     | 960.81                                            | -*                                             | ( - 60,191.97, 1067.79, 62,325.58)     |
| P-n16-K8    | 450                     | 450                                  | 512.8                                     | 512.82                                            | -*                                             | ( - 5240.189, 211.236, 5662.662)       |
| P-n19-K2    | 212                     | 212                                  | 224.1                                     | 229.68                                            | -*                                             | ( - 2667.53, 238.37, 3144.29)          |
| P-n20-K2    | 216                     | 216                                  | 233.1                                     | 233.05                                            | -*                                             | ( - 3594.75, 223.20, 4044.65)          |
| P-n21-K2    | 211                     | 211                                  | 219                                       | 218.96                                            | -*                                             | ( - 4055.62, 224.02, 4503.67)          |
| P-n22-K2    | 216                     | 216                                  | 231.3                                     | 231.26                                            | -*                                             | ( - 4579.56, 227.99, 5035.56)          |

Table 15 continued

| Instances   | Deterministic demands   | Deterministic demands                | Stochastic demands                        | Stochastic demands                                | Fuzzy demands                                  | Fuzzy stochastic demands           |
|-------------|-------------------------|--------------------------------------|-------------------------------------------|---------------------------------------------------|------------------------------------------------|------------------------------------|
|             | Optimal cost            | Genetic algorithm (Awad et al. 2018) | Branch cut and price (Gauvin et al. 2014) | Branch and price (Christiansen and Lysgaard 2007) | Simheuristic approach (Tordecilla et al. 2021) | Proposed approach                  |
| P-n22-K8    | 603                     | 603                                  | 681.8                                     | 707.8                                             | -*                                             | ( - 8430.21, 693.36, 9816.94)      |
| P-n23-K8    | 529                     | 529                                  | 619.5                                     | 662.31                                            | -*                                             | ( - 10421.93, 502.59, 11427.114)   |
| E-n13-K4    | 247                     | 247                                  | -*                                        | -*                                                | -*                                             | (3036.58, 256.254, 3549.092)       |
| E-n22-K4    | 375                     | 375                                  | 411.6                                     | 411.73                                            | 502.3                                          | ( - 4811.92, 470.54, 5753.00)      |
| E-n23-K4    | 569                     | 569                                  | -*                                        | -*                                                | -*                                             | ( - 12,569.77, 659.49, 13,888.753) |

*Operational costs about these modified instances are not available in repositories cise in nature which aligns well with the modified inputs provided to the benchmark instances.

## 6 Conclusion

services to the customers present at various nodes according to their specified time windows. The work can also be further extended to include randomness and impreciseness of arc lengths, and such case will be helpful when the cost matrix represents the time taken to cover various edges in the network.

In this work, two mathematical models for CVRPFSD and CVRPIFSDhavebeenpresented.Atwo-stagestochasticprocess for solving such problems has been presented. The first stage of the problem solving corresponds to an a priori route construction. In this work, branch-and-bound algorithm has been used for this purpose. The demands of the customers are dealt in the second stage where the rote obtained in first stage is traversed. The demands of the customers present in the network are random and imprecise in nature and hence are given by discrete fuzzy random variables. An effective failure cost corresponding to every customer is calculated in the second stage. In this work, each customer is traversed exactly once and the vehicle returns to depot only when a route failure occurs, i.e. when the vehicle reaches at a customer and realizes that the customer cannot be satisfied any more. The work can be used by logistic management companies to schedule the delivery of the seasonal/perishable items. Since the demands of seasonal/perishable items are stochastic in nature, various uncontrollable factors cause the impreciseness of customers' demands. This work shows new dimension of estimating the route cost in a mixed environment when information about the customer demands is based on the past experiences and includes factor of impreciseness and hesitation as well. A model for calculating the operation cost when a logical customer also includes the factor of hesitation while representing his/her demand is also provided in this work. Thus, the methods presented in this work deal with the modelling of randomness, impreciseness and hesitance, which a logical decision maker often encounters in real-world problems. The work can further be extended for service provider companies whose objective is to provide

![Image](image_000015_8127e59b031655297a7b2f1399753dbd9059a6440939103b21996f6e0f6da526.png)

Funding This research is supported by VNIT Nagpur.

Data availability The datasets generated during and/or analysed during the current study are available at http://vrp.atd-lab.inf.puc-rio.br/index. php/en/.

Codeavailability Codes are available from the corresponding author on reasonable request.

## Declarations

Conflict of interest The authors state that there is no conflict of interest.

Humanandanimalrights This article does not contain any studies with human participants or animals performed by any of the authors.

Informed consent Informed consent was obtained from all individual participants included in the study.

## References

Atanassov K (2016) Intuitionistic fuzzy sets. Int. J. Bioautom. 20(S1):S1-S6

Awad H, Elshaer R, AbdElmo'ez A, Nawara G (2018) An effective genetic algorithm for capacitated vehicle routing problem. In: Proceedings of the international conference on industrial engineering and operations management, pp 374-384

Barma PS, Dutta J, Mukherjee A, Kar S (2021) A hybrid GA-BFO algorithm for the profit-maximizing capacitated vehicle routing problemunderuncertainparadigm.JIntellFuzzySyst40(5):87098725

Bernardo M, Pannek J (2018) Robust solution approach for the dynamic and stochastic vehicle routing problem. J Adv Transp 2018(1):111

Brito J, Martínez FJ, Moreno JA, Verdegay JL (2010) Fuzzy approach for vehicle routing problems with fuzzy travel time. In: International conference on fuzzy systems. IEEE, pp 1-8

Chiao KP (2015) Ranking type 2 fuzzy sets by parametric embedded representation. In: 2015 international conference on machine learning and cybernetics (ICMLC), vol 1. IEEE, pp 371-376

Chiao KP (2016) Ranking interval type 2 fuzzy sets using parametric graded mean integration representation. In: 2016 international conference on machine learning and cybernetics (ICMLC), vol 2. IEEE, pp 606-611

Christiansen CH, Lysgaard J (2007) A branch-and-price algorithm for the capacitated vehicle routing problem with stochastic demands. Oper Res Lett 35(6):773-781

Clarke G, Wright JW (1964) Scheduling of vehicles from a central depot to a number of delivery points. Oper Res 12(4):568-581

Cordeau JF, Laporte G, Savelsbergh MW, Vigo D (2007) Vehicle routing. Handb Oper Res Manag Sci 14:367-428

Cormen TH, Leiserson CE, Rivest RL, Stein C (2009) Introduction to algorithms. MIT Press

Dantzig GB, Ramser JH (1959) The truck dispatching problem. Manage Sci 6(1):80-91

Dorigo M, Birattari M, Stutzle T (2006) Ant colony optimization. IEEE Comput Intell Mag 1(4):28-39

Dror M, Laporte G, Trudeau P (1989) Vehicle routing with stochastic demands: properties and solution frameworks. Transp Sci 23(3):166-176

Dutta J, Barma PS, Mukherjee A, Kar S, De T, Pamušar D, Šukeviˇ cius Š, Garbinˇ cius G (2022) Multi-objective green mixed vehicle routing problem under rough environment. Transport 37(1):51-63

Fang L, Chen P, Liu S (2007) Particle swarm optimization with simulated annealing for tsp. In: Proceedings of the 6th wseas international conference on artificial intelligence, knowledge engineering and data bases. Citeseer, pp 206-210

Frieze AM (1983) An extension of christofides heuristic to the k-person travelling salesman problem. Discret Appl Math 6(1):79-83

Gauvin C, Desaulniers G, Gendreau M (2014) A branch-cut-and-price algorithm for the vehicle routing problem with stochastic demands. Comput Oper Res 50:141-153

Gendreau M, Laporte G, Séguin R (1996) Stochastic vehicle routing. Eur J Oper Res 88(1):3-12

GultomP,Napitupulu N (2020) The development of algorithm for determining optimal route for distribution of goods based on distance, time, and road quality using fuzzy set and Clarke and algorithm Wright savings. In: Journal of physics: conference series, vol 1542. IOP Publishing, pp 1-9

Gupta P, Govindan K, Mehlawat MK, Khaitan A. (2022) Multiobjective capacitated green vehicle routing problem with fuzzy time-distances and demands split into bags. Int J Prod Res 60(8):2369-2385

Irvanizam I, Usman T, Iqbal M, Iskandar T, Marzuki M (2020) An extended fuzzy todim approach for multiple-attribute decision-making with dual-connection numbers. Adv Fuzzy Syst 2020(1):1-10

Kandasamy V, Ilanthenral K, Smarandache F (2015) Neutrosophic graphs: a new dimension to graph theory. Infinite Study

Kaur H, Singh H (2017) Local search based algorithm for CVRP with stochastic demands. Int J Adv Res Comput Sci 8(7):1087-1092

KondratenkoY,KondratenkoG,SidenkoI,TaranovM(2020)Fuzzyand evolutionary algorithms for transport logistics under uncertainty. In: International conference on intelligent and fuzzy systems. Springer, pp 1456-1463

Kuo R, Zulvia FE, Suryadi K (2012) Hybrid particle swarm optimization with genetic algorithm for solving capacitated vehicle routing problem with fuzzy demand-a case study on garbage collection system. Appl Math Comput 219(5):2574-2588

Laporte G, Nobert Y (1987) Exact algorithms for the vehicle routing problem. In: North-Holland mathematics studies, vol 132. Elsevier, pp 147-184

Markovi« c D, Petrov« c G, Cojbaši« Ž, Stankovi« c A (2020) The vehicle « c routing problem with stochastic demands in an urban area-a case study. Facta Univer, Ser: Mech Eng 18(1):107-120

Mirzaei-khafri S, Bashiri M, Soltani R et al (2020) A robust optimization model for a location-arc routing problem with demand uncertainty. Int J Ind Eng 27(2):288-307

Mohammed MA, Ahmad MS, Mostafa SA (2012) Using genetic algorithm in implementing capacitated vehicle routing problem. In: 2012 International conference on computer and information science (ICCIS), vol 1. IEEE, pp 257-262

Oyola J (2019) The capacitated vehicle routing problem with soft time windows and stochastic travel times. Rev Facult Ingen 28(50):1933

PopPC,ZelinaI, Lup¸ se V, Sitar CP, Chira C (2011) Heuristic algorithms for solving the generalized vehicle routing problem. Int J Comput Commun Control 6(1):158-165

Puri ML, Ralescu DA, Zadeh L (1993) Fuzzy random variables. In: Readings in fuzzy sets for intelligent systems. Elsevier, pp 265271

Russell SJ, Norvig P (2016) Artificial intelligence: a modern approach. Pearson Education Limited, Malaysia

Shalaby MAW, Mohammed AR, Kassem S (2020) Modified fuzzy cmeans clustering approach to solve the capacitated vehicle routing problem. In: 2020 21st international Arab conference on information technology (ACIT). IEEE, pp 1-7

Singh SK, Yadav SP (2018) Intuitionistic fuzzy multi-objective linear programming problem with various membership functions. Ann Oper Res 269(1-2):693-707

Singh V, Sharma K (2020) Capacitated vehicle routing problem with interval type-2 fuzzy demands. In: Advances in mechanical engineering. Springer, pp 83-89

Tordecilla RD, Martins LDC, Panadero J, Copado PJ, Perez-Bernabeu E, Juan AA (2021) Fuzzy simheuristics for optimizing transportation systems: dealing with stochastic and fuzzy uncertainty. Appl Sci 11(17):7950

Toth P, Vigo D (2002) The vehicle routing problem. SIAM

Úbeda S, Faulin J, Serrano A, Arcelus FJ (2014) Solving the green capacitated vehicle routing problem using a Tabu search algorithm. Lect Notes Manag Sci 6(1):141-149

Werners B, Drawe M (2003) Capacitated vehicle routing problem with fuzzy demand. In: Fuzzy sets based heuristics for optimization. Springer, pp 317-335

Xia X, Liao W, Zhang Y, Peng X (2021) A discrete spider monkey optimization for the vehicle routing problem with stochastic demands. Appl Soft Comput 111(1):1-13

Zimmermann HJ (2011) Fuzzy set theory-and its applications. Springer Zulvia FE, Kuo R, Hu TL (2012) Solving CVRP with time window, fuzzy travel time and demand via a hybrid ant colony optimization and genetic algorithm. In: 2012 IEEE congress on evolutionary computation. IEEE, pp 1-8

Publisher's Note Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

Springer Nature or its licensor (e.g. a society or other partner) holds exclusive rights to this article under a publishing agreement with the author(s) or other rightsholder(s); author self-archiving of the accepted manuscript version of this article is solely governed by the terms of such publishing agreement and applicable law.