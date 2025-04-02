## ORIGINAL PAPER

## Combining variable neighborhood search and machine learning to solve the vehicle routing problem with crowd-shipping

![Image](image_000000_6597957ba5bd263c0852aa230af8859a457cb8b4087a32e7636b9fb06227e719.png)

Luigi Di Puglia Pugliese 1 · Daniele Ferone 3 · Paola Festa 2 · Francesca Guerriero 3 · Giusy Macrina 3

Received: 21 April 2020 / Accepted: 26 November 2021 / Published online: 17 January 2022 ©The Author(s), under exclusive licence to Springer-Verlag GmbH Germany, part of Springer Nature 2021

## Abstract

Crowd-shipping is an innovative delivery model, based on the sharing economy concept. In this framework, delivery operations are carried out by using existing underused resources, i.e., ordinary people who usually travel on the roads with their own vehicles and have empty space to share, in addition to the company's conventional vehicles. Werefer to these non-professional couriers as 'occasional drivers'. Occasional drivers are not company's employees: they are common people who may decide to perform a delivery service during their free time, for a small compensation. Usually, this process is possible thanks to a crowd-shipping platform, which connects the company, the occasional drivers, and the customers. In this paper, we tackle the crowd-shipping model, by developing an approach inspired to variable neighborhood search (VNS) approach, where several machine learning techniques are used to explore the most promising areas of the search space. VNS is a well-known meta-heuristic already used in crowd-shipping applications. In this paper, the learning strategies embedded into the framework have shown to improve the effectiveness of the basic framework.

B

Francesca Guerriero francesca.guerriero@unical.it

Luigi Di Puglia Pugliese luigi.dipugliapugliese@icar.cnr.it

Daniele Ferone daniele.ferone@unina.it

Paola Festa paola.festa@unina.it

Giusy Macrina giusy.macrina@unical.it

- 1 Istituto di Calcolo e Reti ad Alte Prestazioni, Consiglio Nazionale delle Ricerche, Rende, Italy
- 2 Department of Mathematics and Applications, University of Napoli Federico II, Napoli, Italy
- 3 Department of Mechanical, Energy and Management Engineering, University of Calabria, Rende, Italy

![Image](image_000001_222b3cab9b27e729dc41a160a3209d72599fef903a193fcddb70b43400f04ef8.png)

Keywords Vehicle routing · Crowd-shipping · Variable neighborhood search · Machine learning · Reinforcement learning

## 1 Introduction

The growth in e-commerce has nowadays completely changed people's shopping habits. The number of customers who prefer to buy on-line is increasing as well as the number of e-shops, and hence, the e-commerce landscape is growing more competitive. Offering high quality products and providing an excellent on-line experience could be not enough to satisfy the customers if the last step, i.e., the delivery process, is not efficient and effective. As a matter of fact, customers' expectations are very high in terms of rapidity and quality of the deliveries.

For this reason, on-line retailers have started to propose and develop innovative transportation solutions to strengthen same-day and last-mile delivery. Among them, crowd-shipping is a promising model that is growing in popularity. The main idea of crowd-shipping is to apply the concept of 'sharing economy', which has created several successful business models for the delivery process (such as Airbnb Uber , , and Hubble ). In crowd-shipping, non-professional couriers (i.e., ordinary people) who have space in their own vehicles, decide to make a deviation from their ordinary routes for carrying items to other people (i.e., customers), for a small compensation.

In 2015, Amazon proposed Amazon Flex , a crowd-shipping platform, which connects company, non-professional drivers, and customers. This service allows common people to transport same-day delivery packages to Amazon' customers Bensinger [10]. Amazon Flex soon became very common, and nowadays, it is used in more than 30 cities in the world. In 2013, also Walmart introduced the possibility for in-store customers to deliver items to other customers Barr and Wohl [9]. More recently, in 2018, Walmart, partnered with a delivery logistics company called Bringg, proposed Spark Delivery , its crowd-shipping platform, which is very similar to Amazon Flex .

Other big on-line retailers proposed several crowd-shipping platforms and pilot crow-sourced delivery services, such as DHL with MyWays Landa [24], Slabinac [37], as well as many startups have been launched in the last decade. For a complete review on crowd-sourced delivery platforms, the reader is referred to Alnaggar et al. [4].

There are several features that have to be taken into account for introducing crowdshipping in transportation process and building a coherent crowd-sourcing platform: the presence of two types of vehicles, i.e., classical trucks and occasional drivers, and their own characteristics, the time windows of both customers and occasional drivers, the number of the depots, the policy of service, the compensation scheme for occasional drivers and so on.

In this work, we consider a single depot problem, where a company has to serve a set of customers, within their own time windows, in an urban area, i.e., we focus on lastmile delivery. The delivery is made by using the company's own fleet of capacitated vehicles as well as some occasional drivers. The occasional drivers are people who deviate from their routes, make a stop to the company's central depot, and deliver one or morepackagestosomecustomers.Eachvehicle,bothconventionalandoccasional,can

![Image](image_000002_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

perform at most one trip, but it may serve more than one customer. The conventional vehicles start and end their routes from/to the main depot. The occasional drivers start their routes from the main depot and end them to their own destinations. Each occasional driver has his/her own capacity and his/her own time window in which he/she is available for making deliveries; the compensation scheme is based on the distance travelled.

To handle the problem under study, we propose an enhanced version of the variable neighborhood search approach proposed in [30]. The main idea is to use some learning mechanisms to explore the most promising areas of the search space.

In more details, the learning mechanisms have a dual purpose. On the one hand, they bias the selection of the local searches in order to favor the ones that show the best results. On the other hand, another mechanism influences the shaking procedure to generate good solutions using a criterion that is different from the cost, in order to drive the algorithm toward unexplored, but promising space search areas.

The rest of the paper is organized as follows. Sect. 2 briefly summarizes the stateof-art related to our work. Section 3 describes the proposed solution approaches, and the experimental results are shown in Sect. 4. Finally, Sect. 5 reports the conclusions and highlights some possible future work directions.

## 2 State of the art

## 2.1 State of the art on the vehicle routing with crowd-shipping

Ourcontribution has been inspired by Archetti et al. [6] and Macrina et al. [29]. Archetti et al. [6] for the first time modelled the problem of managing the crowd-shipping in logistics networks, by extending the classical vehicle routing problem (VRP). They named the problem VRP with occasional drivers (VRPOD). In the VRPOD, the company has to serve a set of customers in an urban area, by using a fleet composed of both their own conventional vehicles and some occasional drivers (ODs), who in this context are freelance couriers willing to make a deviation from their ordinary routes, in order to deliver parcels to other customers. In their framework, each OD may serve at most one customer. The objective is to minimize the overall costs, given by the routing of conventional vehicles plus the compensation of the ODs. They studied how the compensation scheme for ODs may impact on the solution cost and proposed a metaheuristic that combines a variable neighborhood search (VNS) with a Tabu Search (TS). The VRPOD was extended by Macrina et al. [29], who proposed and modelled two variants of the original problem. The first one introduced the time windows for both customers and ODs, and multiple deliveries for ODs (VRPODTWmd), i.e., ODs may visit more than one customer in the same trip. The second variant extends the first one by considering the split and delivery policy. Since Macrina et al. [29] proposed only a mathematical model and solved it with a commercial solver, which finds solutions for networks with at most 15 customers, Macrina [26] proposed a VNS for the VRPODTWmd, which solves instances with up to 100 customers. Recently, an enhanced version of the VNS of Macrina [26] was proposed by Macrina et al. [28]. Dahle et al. [15] extended the VRPOD of Archetti et al. [6] by introducing pickup and

![Image](image_000003_cb89a238a74f9801a530731ec2702a5cb33f1eed9ed4826c1f50cd365856c1ea.png)

delivery policy and time windows. They proposed a load and a flow formulation for the problem and focused on the study of compensation schemes for the ODs. Macrina et al. [30] introduced the VRPOD with time windows and transshipment nodes. Transshipment nodes are depots closer to the urban area than the main depot. The authors modelled the problem as a variant of the well known two-echelon VRP, then implemented a matheuristic approach, based on the VNS.

All the aforementioned works consider 'static' versions of the problem. In fact, they suppose that all the information are known a-priori. Recently, several authors focused on the stochastic/dynamic version of the VRPOD, in which information about customers and/or ODs are not completely known in advance or are subjected to uncertainty. (see, [5,8,14,16,19]) For other works addressing some specific issues related to crowd-shipping, such as ODs' behavior, the use of public transportation, and the impact of crowd-shipping on the environment, the reader is referred to Serafini et al. [36], Abu Al Hla et al. [1], Allahviranloo and Baghestani [3], Macrina and Guerriero [31] and Macrina et al. [27]. For a survey of VRPOD contributions in operational research, the reader is referred to Alnaggar et al. [4].

## 2.2 State of the art on machine learning applied to routing and VNS

The use of machine learning (ML) approaches is gaining more and more popularity in operations research (OR) applications, due to their success when dealing with realistic problems. These applications arise in various and numerous contexts, such as production, telecommunication, transportation, and logistics. Focusing on transportation, several learning techniques have been implemented and used for solving routing problems.

Calvet et al. [11] reviewed the existing literature on the combination of metaheuristics with ML techniques for OR problems. In addition, they presented new types of hybrid algorithms, the learn-heuristics, that aim at solving combinatorial optimization problems with dynamic inputs. The authors proposed a new framework of learn-heuristics which hybridizes a heuristic-based constructive procedure with ML. They tested their approach on the capacitated VRP, in which each customer's demand depends on the visiting order.

Arnau et al. [7] applied the learn-heuristic framework to the VRP with dynamic inputs. They studied the VRP with dynamic traveling times and assumed that these inputs need to be dynamically re-calculated as the solution is being constructed. Then, they proposed a learn-heuristic-based approach, which integrates a regression model within a multi-start heuristic to predict the future inputs based on historical data.

Nazari et al. [33] used reinforcement learning Sutton and Barto [39] to solve the VRP and proposed a policy-gradient approach. They considered the Markov decision process formulation of the problem, in which the optimal solution can be viewed as a sequence of decisions. This allowed to use the reinforcement learning to increase the probability of finding good quality sequences, hence finding near-optimal solutions. They tested their approach on a particular variant of the VRP, in which one vehicle with a limited capacity has to serve a set of customers with finite demands. The vehicle has a limited load capacity and must return to the depot to refill once the

![Image](image_000004_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

load runs out. Vera and Abad [42] extended the model proposed in Nazari et al. [33] for solving the VRP with fixed fleet size. The authors used deep neural networks to encode the position of the vehicles and customers' demand and to estimate, for each vehicle, the next customers to be served. As Nazari et al. [33], Kool et al. [23] used a policy-gradient approach. They proposed an attention approach to solve a classic traveling salesman problem (TSP), two variants of the VRP, the orienteering problem, and the (stochastic) prize collecting TSP. Their computational results highlighted that the proposed approach leads to significant improvement over recent learned heuristics for the TSP as well as to contribute to additionally learned strong (single construction) heuristics for the other routing problem variants.

Chen et al. [13] used a deep Q-learning approach to solve same-day delivery VRP variants with drones, in which customers' demands arrive over the course of the day and have to be dynamically served by a fleet composed of vehicles and drones within a given deadline. Q-learning approach is a form of reinforcement learning that seeks to learn the value of state-action pairs Watkins and Dayan [43]. Their computational results showed that the use of reinforcement learning techniques may lead to more suitable solutions in terms of number of satisfied customers requests.

ML has also been applied to improve the performance of the meta-heuristics. In particular, VNS has been extended through different ML techniques applied to either the shaking procedure or to the local search selection.

The main purpose of the VNS is to explore several neighborhoods within a descent phase to find a local optimum and in a perturbation phase to escape from the corresponding valley Mladenovi· c and Hansen [32]. Therefore, at each iteration, a VNS uses a shaking procedure to perturb a current solution and to obtain a new starting point for the local search phase, in which it explores different neighborhood structures.

ML can be used to improve both these two phases. In particular, it improves the local search phase biasing the selection of which neighborhood structure to explore, and the shaking procedure to generate good solutions in unexplored search space areas.

To the best of our knowledge, Queiroz Dos Santos et al. [34] presented the first approach in this direction. In this work, at each iteration, Q-learning is used to select the next local search for the intensification process. Sze et al. [40] and Chen et al. [12] proposed a similar strategy, that uses the reinforcement learning in the local search phase to guide the search, adjusting the probabilities of the operators being adaptively invoked, according to the change of the feasibility and the quality of the generated solutions. Thevenin and Zufferey [41] used the concept of attractiveness to bias the random selection of the local search moves in the shaking step. At each iteration, the attractiveness of the moves is updated through a learning process; and the moves with high attractiveness values have more chance to be performed.

## 3 Solution approaches

To solve the VRPODTWmd, we propose a few approaches derived from the VNS framework presented in Macrina et al. [28]. VNS is a well-known metaheuristic, introduced by Mladenovi· c and Hansen [32] and applied to solve various combinatorial and global optimization problems Hansen et al. [20]. VNS aims at finding an optimal

or a sub-optimal solution by sequentially changing neighborhood structures during the search. We divided the algorithm into three main steps to simplify the description: shaking, local search, and neighborhood change step. To obtain an initial feasible solution, we use a simple insertion heuristic with the objective of serving the farther customers by using the ODs.

The shaking and the local search phases are iterated for MaxIterations times, (see Algorithm 1). The shaking phase aims at perturbing the current solution through random moves, in order to better explore the neighborhoods and escape possible local optima, and it allows to accept solutions of lower quality than the incumbent solution. The local search phase aims at improving the current solution. Usually, several local search moves are applied to find more effective solutions. Actually, the search through multiple neighborhoods is known as Variable Neighborhood Descent (VND), the deterministic variant of VNS Hansen et al. [21]. Finally, the neighborhood change step is responsible for selecting neighborhoods to use for shaking and local search, with the goal of better exploring the solutions space.

Algorithm 1: Pseudo-code of the VNS-Inspired algorithm.

![Image](image_000005_c02575a8a93fb0a8ae70c9f759065917f8d185f1bbb282e929d73d46db357e11.png)

In our solution approaches, we use six different neighborhoods, based on the local search moves proposed in Macrina et al. [28] and reported in what follows for the sake of clarity and completeness. In the description below, if not otherwise specified, we refer to routes r and r ′ both as traditional and ODs routes.

/negationslash

Move Node.

A node i is removed from a route r and is inserted in a route r ′ in a feasible position; r ′ can be equal to r .

2-Opt.

This operator removes two arcs ( , i j ) and ( u , v) in the same route r or in two distinct routes r and r ′ , and reconnects the path(s) created using arcs ( , i u ) and ( j , v) .

Swap Inter-Route.

This operator removes one node i from a route r and one node j from another route r ′ , r = r ′ , and inserts i into r ′ and j into r in the best feasible positions.

![Image](image_000006_f790e5f75c7350a4533c6bfc45ea5c44d2305f39c1bf5ebfad7db008f5030468.png)

Swap Intra-Route.

This operator swaps the position of two nodes i and j in a given route r .

New Route best.

A new traditional route r ′ is initialized only if this choice leads to a less expensive solution. A node i belonging to given route r is removed from r and inserted in r ′ .

New Route.

Similar to the previous one, but the move is performed also if it leads to a worse solution.

We report in Algorithm 1 our basic algorithm (VNS-I), which is inspired by the VNS. Indeed, our algorithm shares some similarities with the VNS; in particular, it presents a shaking phase (line 5), a local search, through different neighborhoods structures (line 8), and a neighborhood change step (lines 9-13). Nevertheless, the algorithm presents some differences: while in Basic VNS the shaking depends on the neighborhood index and the local search is performed in a single neighborhood, our algorithm performs the shaking by using a strategy based on two attributes (Sect. 3.1) and the local search is performed in all the neighborhood structures.

We implemented several enhanced versions by introducing ML strategies in the shakingandinthelocalsearchphases.Inparticular,thestrategies proposed in Thevenin and Zufferey [41], Chen et al. [12], and Queiroz Dos Santos et al. [34] have been adapted to address the VRPODTWmd. In what follows, the implemented strategies are described in details.

## 3.1 Learning for the shaking phase

In this section, we describe how we adapted the learning strategy proposed by Thevenin and Zufferey [41] to handle the VRPODTWmd. The main idea of the shaking step is to modify solutions selecting random moves, biasing the selection process to prefer those moves that lead towards a 'good' mix of solution attributes.

Clearly, the goodness of a feasible solution is reflected by lower costs. In our implementation, we used two different types of solution attributes: (i) customer-customer (C-C) matching, i.e., when two customers are often served by the same vehicle in good quality solutions; (ii) customer-OD (C-OD) association, i.e., when good quality solutions frequently serve a given customer with a specific OD.

To reflect the goodness of associations (i) and (ii) in the shaking phase, we make use of a trail system similar to the approach that characterizes the Ant Colony algorithms Dorigo et al. [18]. Such trail values - associated to each couple of either C-C or COD - are updated through the search process, and take larger values if the couple is featured in good-quality solutions.

Let C and K be the set of customers and occasional drivers, respectively, then, the trail values are denoted as Tr n ( 1 , n 2 ) , with n 1 ∈ C and n 2 ∈ C ∪ K .

To avoid any initial bias, all the trails are initialized to 0 and are updated at the end of each iteration by using s ′′ (see Algorithm 1). Equation 1 gives the trail updating rule, where ρ ∈ ( 0 , 1 ) is an evaporation factor:

$$T r ( n _ { 1 }, n _ { 2 } ) = \rho \cdot T r ( n _ { 1 }, n _ { 2 } ) + \Delta T r _ { ( s ^ { \prime \prime } ) } ( n _ { 1 }, n _ { 2 } ). \quad \ ( 1 )$$

If n 1 and n 2 are associated in s ′′ , i.e., either they are two customers served by the same vehicle or n 2 is the OD serving n 1, the better s ′′ in terms of solution cost, the higher the reinforcement quantity /Delta1 Tr s ( ′′ ) ( n 1 , n 2 ) . It is computed through Equation 2, where M is a large integer number (in our implementation, it is the sum of the cost of all the edges of the graph):

$$\Delta T r _ { ( s ^ { \prime \prime } ) } ( n _ { 1 }, n _ { 2 } ) = \begin{cases} \frac { M } { \cosh ( s ^ { \prime \prime } ) }, & \text{if $n_{1}$ and $n_{2}$ are customers served by the same vehicle in $s^{\prime\prime}$;} \\ \frac { M } { \cosh ( s ^ { \prime \prime } ) }, & \text{if $n_{2}$ is the OD serving the customer $n_{1}$ in $s^{\prime\prime}$;} \\ 0, & \text{otherwise.} \end{cases}$$

(2)

Given the trail values of each couple, it is possible to define a global trail of the solution, and the resulting attractiveness of a move m applied on a solution s .

The global trail of s , named Tr s ( ) , is the sum of the trails of all the couples. Let R be the set of routes performed by traditional vehicles, and let Pk be the customers served by the OD k , for a given solution s . Tr s ( ) is computed as follows:

$$T r ( s ) = \sum _ { r \in R } \left ( \sum _ { \substack { n _ { 1 }, n _ { 2 } \in r \\ n _ { 1 } \neq n _ { 2 } } } T r ( n _ { 1 }, n _ { 2 } ) \right ) + \sum _ { k \in K } \left ( \sum _ { n _ { 1 } \in P _ { k } } T r ( n _ { 1 }, k ) \right ). \quad \ \ ( 3 )$$

/negationslash

Let s ′ be the solution obtained by applying the move m to the solution s . The attractiveness of s ′ , named A m ( s ′ ) , is computed as

$$\mathcal { A } _ { m } ( s ^ { \prime } ) = T r ( s ^ { \prime } ) - T r ( s ).$$

The attractiveness of each feasible move is computed during the shaking phase. Then, the moves are sorted with respect to their attractiveness, and one of the moves is randomly selected following a geometric distribution with parameter β , that prefers the moves with a high attractiveness.

## 3.2 Reinforcement learning for local search selection

The second learning strategy is used to guide the selection of the local search moves in the intensification phase and can be viewed as an adaptation for the VRPODTWmd of the procedure presented in Chen et al. [12].

Given a solution s ′ , the procedure associates to each neighborhood N i ( s ′ ) of s ′ a weight Wi , that is used to determine the probability that the neighborhood is selected. Each time a local search in neighborhood N i is performed, the algorithm updates the corresponding weight, in order to refer neighborhoods that lead to good solutions. Algorithm 2 depicts the framework of this local search strategy, named ProbLS.

The algorithm gives as output the best found solution, taking as input the following parameters:

123

![Image](image_000007_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

- · s ∗ and s ′ are the best feasible solution found so far and the current solution, respectively;
- · W is the vector of the weights (in our implementation, we initialized it with all the weights equal to 10);
- · L max indicates the maximum number of iterations without improvement;
- · de v is the maximum deviation from s ∗ to accept a pejorative solution as current solution.

Algorithm 2: Local search ProbLS pseudo-code.

![Image](image_000008_52fa90b60fbc74290a3367303d300dba9b2966388e485dcaf7a3d76e3ca88c56.png)

While L is lower than L max, the selection probability of each neighborhood is calculated considering its weight (Line 5). If the obtained solution s ′′ is better than the incumbent one s ∗ , then both s ∗ and the current solution s are updated, the weight is increased, and L is reset to 0. Otherwise, if the gap of s ′′ with respect to s ∗ is lower than a given threshold, then s ′′ is accepted as the current solution. In any other cases, the weight associated to the neighborhood is decremented.

## 3.3 Q-learning for local search selection

Similarly to the ProbLS approach given in Algorithm 2, the learning strategy described in this paragraph is used to guide the local search moves selection. It can be viewed as an adaption of the Q-learning algorithm proposed in Queiroz Dos Santos et al. [34], and successfully used also in Alicastro et al. [2]. In this approach, an agent (the learner) interacts with its environment and selects the actions to be applied according to its current state and the reinforcement (i.e., rewards) it collects. The main goal of the agent is to maximize the reward, thus it provides to the learning algorithm feedback about the effect of the taken actions.

![Image](image_000009_7f5935cd69993f5f623e7e046c37982ed7a64fc946fcc055a007f54340667b0f.png)

Let Q t ( , a ) be an estimation of the expected total reward obtained by performing action a on state t , named action-value function . The learning process consists of a sequence of epochs ( 0 , 1 , . . . , n , . . . ) . In epoch n , the agent is in state t and performs action a receiving a reward re w ard t ( , a ) , then, it moves to state t ′ , and it updates Q on the basis of the following condition:

$$Q ( t, a ) = Q ( t, a ) + \alpha \left [ r e w a r d ( t, a ) + \gamma \max _ { a ^ { \prime } \in \mathcal { A } } Q ( t ^ { \prime }, a ^ { \prime } ) - Q ( t, a ) \right ] \quad ( 5 )$$

$$Q ( t, a ) & = Q ( t, a ) + \alpha \left [ r e w a r d ( t, a ) + \gamma \max _ { a ^ { \prime } \in \mathcal { A } } Q ( t ^ { \prime }, a ^ { \prime } ) - Q ( t, a ) \right ] \quad ( 5 ) \\ & = ( 1 - \alpha ) \ Q ( t, a ) + \alpha \cdot r e w a r d ( t, a ) + \alpha \gamma \max _ { a ^ { \prime } \in \mathcal { A } } Q ( t ^ { \prime }, a ^ { \prime } ), \quad ( 6 )$$

where γ ∈ [ 0 , 1 ] is a discount factor, α ∈ [ 0 , 1 ] is the learning rate, and A is the set of possible actions.

Note that Q t ( , a ) is the sum of three factors:

- · ( 1 -α) Q t ( , a ) : the current value weighted by 1 minus the learning rate. Values of the learning rate near to 1 made faster the changes in Q ;
- · α · re w ard t ( , a ) : the reward (weighted by learning rate) of the action performed;
- · αγ max a ′ ∈ A Q t ( ′ , a ′ ) : the maximum reward that can be obtained from state t ′ . It is weighted by learning rate and discount factor, that determines the importance of future rewards. A factor of 0 will make the agent 'myopic' by only considering current rewards, while a factor approaching 1 will make it strive for a long-term high reward.

/negationslash

Algorithm 3: Local search with Q-learning pseudo-code.

![Image](image_000010_bf9198f3f4f323968835cbdc393e19e14050856d381365fecfd9d81289a4c980.png)

For the considered problem, the neighborhood structures can be seen as actions as well as states. In particular, the current state is the last applied local search, the action,

instead, is the next local search to be applied. The local search procedure based on the Q-learning approach is depicted in Algorithm 3.

In order to increase the exploration factor, the algorithm makes use of a parameter /epsilon1 ∈ ( 0 , 1 . With a probability ) /epsilon1 , the algorithm selects the next action (local search) in a totally random manner. On the contrary, with probability 1 -/epsilon1 , the action a that maximizes Q t ( , a ) is selected as next action to be applied.

## 3.4 VNS-I variants

Given the learning strategies described above, we define the following five VNS-I versions, that differ one to each other in the way the learning strategies are applied in the shaking and in the local search phases.

ProbVNS ( PVNS ):

in this variant, Algorithm 2 is used as local search procedure; the shaking procedure uses two neighborhoods randomly selected among those presented in Sect. 3;

QLearningVNS ( QL ):

in this version, the local search phase is imple- mented by using the Q-learning approach of Algorithm 3; the shaking procedure adopts the same approach of ProbVNS;

TheveninVNS ( T ):

in this variant, the local search phase is based on the selection of a different neighborhood struc- ture in a round-robin way following the order of presentation reported in Sect. 3; the shaking pro- cedure is implemented as described in Sect. 3.1; in this version, the shaking procedure of Sect. 3.1 is combined with the local search phase of Prob- VNS;

Thevenin+ProbVNS ( T P ):

Thevenin+QLearningVNS ( T QL

): in this variant, the shaking procedure is the one used by TheveninVNS, the local search is per- formed as in QLearningVNS.

More detailed pseudo-codes of the algorithms can be found at https://bit.ly/3u4sIla.

## 4 Computational results

In this section, we describe the computational results that we carried out in order to assess the performance of the proposed algorithms. All the algorithms have been implemented in C++ with gcc 8.3.0 using the flags -std=c++17 -O3 . We carried out all the experiments on an Intel Core i7-4510U CPU @ 2.00GHz × 4 with 8GB of RAM under Linux (Ubuntu 18.04).

The remainder of this section is organized as follows. Firstly, we describe the instances we used to perform our experiments in Sect. 4.1, and the experimental settings used to validate the proposed approaches in Sect. 4.2. Then, we show the results in Sects. 4.3 and 4.4. In particular, we divided the study into two phases. In the

Table 1 Parameters setting of the instances belonging to S 1 ([28])

|   C |   P |   K |   Q | Q k      |
|-----|-----|-----|-----|----------|
|   5 |   3 |   3 |  80 | [10, 25] |
|  10 |   3 |   3 |  80 | [10, 30] |
|  15 |   3 |   5 |  80 | [15, 35] |
|  25 |   5 |  10 | 100 | [20, 40] |
|  50 |   8 |  15 | 200 | [20, 40] |
| 100 |  10 |  30 | 400 | [20, 40] |

first phase, we compared our results with those available in the literature (see Macrina et al. [28]); in the second phase, we carried out additional tests, on new classes of instances, to compare the performance of the proposed solution approaches.

All the local searches have been implemented following a best-improvement strategy, and all the methods have been executed setting MaxIterations = 150. Moreover, each method has been executed 30 times on each instance, and we report the average values.

## 4.1 Instances

We carried out our computational experiments on different classes of test problems. The first class contains the VRPODTWmd instances proposed in Macrina et al. [28], based on the VRPTW benchmarks of Solomon [38]. Solomon's instances are divided into three main categories C, R, and RC. Instances in C have clustered customers; those in R have customers location generated uniformly randomly over a square and, finally, instances in RC have a combination of clustered and randomly located customers. In addition, for each category, there are two types of instances: type 1 and type2. Instances of type 1 have narrow time windows and small vehicle capacity. Instances of type 2 have large time windows and large vehicle capacity. Thus, there are six different problem types: C1, C2, R1, R2, RC1, RC2.

Starting from these instances, Macrina et al. [28] explain how they generated VRPODTWmd instances following the idea of Archetti et al. [6]. Thus, they generated: 1) 36 small size instances of type C1, C2, R1, R2, RC1, RC2, with 5, 10, and 15 customers, 2) 30 medium size instances of type C1, R1, RC1, with 25 and 50 customers and 3) 15 big size instances of type C1, R1, RC1 with 100 customers. We named this class S1 , whose characteristics in terms of number of customers (C), number of classical vehicles (P), number of ODs (K), capacity of classical vehicles (Q), capacity of ODs (Q k ), are reported in Table 1.

Following the rules described in Macrina et al. [28], we have generated a second class of instances, starting from the Solomon's benchmarks. In particular, we have generated (1) 30 medium size instances of type C2, R2, RC2, with 25 and 50 customers and (2) 15 big size instances of type C2, R2, RC2 with 100 customers. We maintained the original capacity of trucks. We named this class S2 . Table 2 reports the characteristics of these instances.

![Image](image_000011_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

| Table 2 Parameters setting for the generation of the instances   | C   | P   | K   | Q           | Q k      |
|------------------------------------------------------------------|-----|-----|-----|-------------|----------|
| belonging to S 2                                                 | 25  | 5   | 10  | [700, 1000] | [20, 40] |
|                                                                  | 50  | 8   | 15  | [700, 1000] | [20, 40] |
|                                                                  | 100 | 10  | 30  | [700, 1000] | [20, 40] |
| Table 3 Parameters setting for the generation of the instances   | C   | P   | K   | Q           | Q k      |
| belonging to S 3                                                 | 200 | 25  | 40  | 500         | [25, 50] |
|                                                                  | 400 | 50  | 60  | 500         | [30, 60] |

We have also generated a third class of instances, named S3 , which contains 30 very large instances, with 200 and 400 customers. This set has been generated starting from the test problems introduced in Homberger and Gehring [22]. These instances are classified as the Solomon's ones. Thus, we generated instances of types C1, C2, R1, R2, RC1, RC2, whose main characteristics are given in Table 3. All the instances can be found at https://bit.ly/3u4sIla.

## 4.2 Parameters tuning

We performed a fine tuning process to assign a value to each parameter used by the proposed solution approaches. Firstly, we constructed a training set randomly selecting the 30% of the instances for each size (number of nodes). Once the set has been constructed, we performed an iterated racing procedure, implemented in irace package López-IbÆæez et al. [25], to automatically find the best configuration of the parameters. 1

TheveninVNS approach needs two parameters: the evaporation factor ρ , and the parameter of the geometric distribution β . ProbVNS needs two parameters: L max and de v , that are the maximum number of internal iterations without improvement and the threshold used to accept a pejorative solution, respectively. To conclude, QLearningVNS needs three parameters: the threshold for random selection /epsilon1 , the learning rate α , and the discount factor γ .

Table4showstheparametersconfigurationsforeachapproach.Inparticular,Table4 has four columns: the first one reports the approach, the second one the name of the parameter, the third column shows the input domain of the parameter given to irace ; the fourth one the final value.

## 4.3 Computational results on set S1

In this section, we discuss about the results obtained for instances belonging to S 1, i.e., the benchmark set proposed in Macrina et al. [28]. Hence, we compare our results with those reported in Macrina et al. [28].

1 Irace has been launched using the default parameters.

![Image](image_000012_7f5935cd69993f5f623e7e046c37982ed7a64fc946fcc055a007f54340667b0f.png)

Table 4 Parameter configurations

| Approach     | Parameter   | Input               | Final value   |
|--------------|-------------|---------------------|---------------|
| TheveninVNS  | ρ           | [ 0 . 85 , 0 . 95 ] | 0 . 91        |
| ProbVNS      | β L max     | [ 0 , 1 ] 3 , 5 , 7 | 0 . 27 5      |
|              | de v        | { } [ 0 , 0 . 20 ]  | 0 . 11        |
| QLearningVNS | /epsilon1   | [ 0 , 0 . 10 ]      | 0 . 02        |
|              |             | 0 . 85 , 0 . 95     | 0 . 87        |
|              | α           | [ ]                 |               |
|              | γ           | [ 0 . 85 , 0 . 95 ] | 0 . 93        |

Table 5 Results on instances with 5 customers

| Instance   | CPLEX    | VNS-I    | PVNS     | QL       | T        | T P      | T QL     |
|------------|----------|----------|----------|----------|----------|----------|----------|
| C101C5     | 145.40   | 145.40   | 145.40   | 145.40   | 145.40   | 145.40   | 145.40   |
| C103C5     | 111.60   | 111.60   | 111.60   | 111.60   | 111.60   | 111.60   | 111.60   |
| C206C5     | 159.60   | 159.60   | 159.60   | 159.60   | 159.60   | 159.60   | 159.60   |
| C208C5     | 131.90   | 131.90   | 131.90   | 131.90   | 131.90   | 131.90   | 131.90   |
| R104C5     | 107.20   | 107.20   | 107.20   | 107.20   | 107.20   | 107.20   | 107.20   |
| R105C5     | 143.10   | 143.10   | 143.10   | 143.10   | 143.10   | 143.10   | 143.10   |
| R202C5     | 129.50   | 129.50   | 129.50   | 129.50   | 129.50   | 129.50   | 129.50   |
| R203C5     | 170.10   | 170.10   | 170.10   | 170.10   | 170.10   | 170.10   | 170.10   |
| RC105C5    | 143.10   | 143.10   | 143.10   | 143.10   | 143.10   | 143.10   | 143.10   |
| RC108C5    | 164.00   | 164.00   | 164.00   | 164.00   | 164.00   | 164.00   | 164.00   |
| RC204C5    | 107.00   | 107.00   | 107.00   | 107.00   | 107.00   | 107.00   | 107.00   |
| Average    | 136 . 45 | 136 . 45 | 136 . 45 | 136 . 45 | 136 . 45 | 136 . 45 | 136 . 45 |

## 4.3.1 Experimental results on small size instances

In this section, we present the computational results obtained on small size instances, i.e., those with 5, 10, and 15 customers beloging to the set S 1. Tables 5 6 and 7 summarize the related results.

Each table has eight columns, the first one shows the name of the instance, the second reports the optimal values obtained by CPLEX in Macrina et al. [28], the third shows the results with the VNS-I approach proposed in Macrina et al. [28]. The remaining columns depict the results for each approach described in Sect. 3.4. All the optimal values are reported in boldface.

Since all the algorithms terminate in less than 0 5 seconds, we do not report the . computational times.

We want to recall that VNS-I finds optimal solutions for all the instances with 5 customers, instead it solves the instances with 10 and 15 customers with gaps of 1.04% and 1.92%, respectively.

Looking at Table 5, it is evident that all our approaches find the same solutions as the VNS-I , hence, they solve this class of instances to the optimality. The results

![Image](image_000013_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

Table 6 Results on instances with 10 customers

| Instance   | CPLEX    | VNS-I    | PVNS     | QL       | T        | TP       | TQL      |
|------------|----------|----------|----------|----------|----------|----------|----------|
| C101C10    | 283 . 2  | 286.56   | 284.48   | 283.68   | 285.12   | 283.68   | 284.00   |
| C104C10    | 242 . 40 | 242.63   | 242 . 40 | 242 . 40 | 243.55   | 242 . 40 | 242 . 40 |
| C202C10    | 175 . 40 | 175 . 40 | 175 . 40 | 175 . 40 | 175 . 40 | 175 . 40 | 175 . 40 |
| C205C10    | 184 . 70 | 184 . 70 | 184 . 70 | 184 . 70 | 184 . 70 | 184 . 70 | 184 . 70 |
| R102C10    | 166 . 40 | 178 . 61 | 166 . 40 | 166 . 40 | 175 . 84 | 166 . 40 | 166 . 40 |
| R103C10    | 154 . 00 | 154 . 00 | 154 . 00 | 154 . 00 | 154 . 00 | 154 . 00 | 154 . 00 |
| R201C10    | 185 . 20 | 187.28   | 185 . 20 | 185.40   | 185 . 20 | 185 . 20 | 185 . 20 |
| R203C10    | 132 . 90 | 136.75   | 136.44   | 132 . 90 | 138.66   | 132 . 90 | 132 . 90 |
| RC102C10   | 331 . 80 | 335.20   | 331 . 80 | 331 . 80 | 334.20   | 332.00   | 331 . 80 |
| RC108C10   | 330 . 10 | 332.79   | 330.26   | 330 . 10 | 334.53   | 330.41   | 330.57   |
| RC201C10   | 231 . 10 | 231 . 10 | 231 . 10 | 231 . 10 | 231 . 10 | 231 . 10 | 231 . 10 |
| RC205C10   | 260 . 30 | 260 . 30 | 260 . 30 | 260 . 30 | 260 . 30 | 260 . 30 | 260 . 30 |
| Average    | 223 . 12 | 225.44   | 223.54   | 223.18   | 225.22   | 223.21   | 223.23   |
| %-gap      |          | 1.04     | 0.19     | 0.03     | 0.94     | 0.04     | 0.05     |

Table 7 Results on instances with 15 customers

| Instance   | CPLEX    |   VNS-I | PVNS     | QL       | T        | TP       | TQL      |
|------------|----------|---------|----------|----------|----------|----------|----------|
| C103C15    | 206 . 5  |  220.07 | 211.06   | 211.12   | 219.79   | 209.57   | 209.36   |
| C106C15    | 161 . 90 |  162.16 | 161 . 90 | 161 . 90 | 161 . 90 | 161 . 90 | 161 . 90 |
| C202C15    | 332 . 90 |  341.32 | 337.15   | 337.17   | 341.23   | 336.89   | 336.16   |
| C208C15    | 304 . 70 |  305.33 | 304 . 70 | 304 . 70 | 305.70   | 304 . 70 | 304 . 70 |
| R102C15    | 297 . 80 |  303.75 | 299.74   | 300.05   | 299.02   | 298.27   | 298.16   |
| R105C15    | 215 . 80 |  218.08 | 216.53   | 215 . 80 | 217.57   | 216.01   | 215 . 80 |
| R202C15    | 324 . 40 |  330.53 | 326.79   | 326.51   | 330.04   | 326.41   | 327.17   |
| R209C15    | 239 . 40 |  244.97 | 239 . 40 | 239 . 40 | 241.97   | 239 . 40 | 239 . 40 |
| RC103C15   | 341 . 60 |  342.61 | 341.92   | 341.76   | 342.32   | 341 . 60 | 341 . 60 |
| RC108C15   | 248 . 60 |  253.16 | 252.65   | 252.65   | 253.15   | 252.63   | 252.60   |
| RC202C15   | 356 . 30 |  366.08 | 360.37   | 359.82   | 366.14   | 359.78   | 358.88   |
| RC204C15   | 341 . 10 |  347.83 | 342.78   | 342.86   | 348.03   | 341.66   | 342.29   |
| Average    | 280 . 92 |  286.32 | 282.92   | 282.81   | 285.57   | 282.40   | 282.33   |
| %-gap      |          |    1.92 | 0.71     | 0.67     | 1.66     | 0.53     | 0.50     |

in Table 6 shows that PVNS , QL T P , , and T QL find the optimal solution for the majority of the instances. Furthermore, the gaps with respect to CPLEX are very low. In fact, VNS-I presents a gap equal to 1 04%, meanwhile all the other approaches . have gaps less than 1%. Table 7 shows similar trends, underling that T QL is the best approach showing a gap of 0.50%, followed by T P with a gap of 0.53%. In addition, PVNS and QL perform similarly with gaps near to 0.70%. T and VNS-I behave the worst; indeed, the gaps are about 1.66% and 1.92%, respectively.

Table 8 Summary of results on small instances

|               |   VNS-I |   PVNS |     QL |      T | TP       | TQL      |
|---------------|---------|--------|--------|--------|----------|----------|
| All instances |  216.07 | 214.3  | 214.14 | 215.74 | 214.02   | 214 . 00 |
| Category C    |  205.56 | 204.19 | 204.13 | 205.49 | 203.98   | 203 . 93 |
| Category R    |  191.99 | 189.53 | 189.2  | 191.02 | 189 . 04 | 189.08   |
| Category RC   |  250.67 | 249.18 | 249.12 | 250.73 | 249.04   | 249 . 01 |
| Type 1        |  213.56 | 211.53 | 211.45 | 213.08 | 211.29   | 211 . 28 |
| Type 2        |  218.59 | 217.07 | 216.85 | 218.41 | 216.75   | 216 . 73 |

Table 8 summarizes the average results, grouped by categories and types. In particular, the first row shows the average results for all the instances, the second, third, and fourth rows report the average results grouped by customers location, while in the last two rows we grouped the results by type 1 and 2, respectively.

Looking at results in Table 8, it is clear that all the approaches show similar behavior, on different categories and types. Focusing on our proposed approaches, T QL generally shows the best performances on all the classes of small size instances.

## 4.3.2 Computational results on medium and large size instances

In this section, we analyze the computational results on medium and large instances, i.e., those with 25, 50, and 100 customers belonging to the set S 1. We report in Table 9 the average numerical results obtained on medium instances, for each category of instances. Since the problem is NP -hard, for most of these instances, CPLEX does not find a feasible solution within the time limit, often due to memory overflow. Therefore, the values of the optimal costs for these classes of instances are not available, and we report in Table 9 the percentage average gap in terms of cost for each approach. More specifically, let I be the set of instances, the percentage average gap is determined as follows

$$\frac { 1 } { | \mathcal { I } | } \sum _ { i \in \mathcal { I } } \frac { 1 0 0 ( O b j _ { H } ( i ) - O b j _ { V N S } ( i ) ) } { O b j _ { V N S } ( i ) },$$

where ObjH ( ) i is the cost of the solution determined by the approach H on instance i .

A detailed accounting of the computational results collected on these classes of instances is given in Tables A.14-A.16 available online at https://bit.ly/3u4sIla.

Looking at Table 9, it is clear that all the proposed approaches outperform the VNS-I in terms of effectiveness. Indeed, on average the improvement ranges from -2 21% . for T S to -6 13% for . T QL .

The improvements decrease as the increasing size of the instances. In particular, our approaches show the better performances for instances with 25 customers, in fact, the percentage average gap is equal to -5 53%. Meanwhile, the average improvement . decreases to -4 48% and . -3 84% for instances with 50 and 100 nodes, respectively. .

![Image](image_000014_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

Table 9 Summary of results on medium instances

|               | PVNS   | QL     | T      | TP       | TQL      | Average   |
|---------------|--------|--------|--------|----------|----------|-----------|
| 25 customers  | - 4.56 | - 5.30 | - 3.15 | - 7 . 33 | - 7.30   | - 5.53    |
| 50 customers  | - 3.93 | - 4.49 | - 2.14 | - 5.90   | - 5 . 94 | - 4.48    |
| 100 customers | - 3.62 | - 4.02 | - 1.35 | - 5.08   | - 5 . 15 | - 3.84    |
| Category C    | - 4.35 | - 5.18 | - 2.19 | - 6 . 83 | - 6.76   | - 5.06    |
| Category R    | - 3.72 | - 4.16 | - 2.14 | - 5.43   | - 5 . 54 | - 4.20    |
| Category RC   | - 4.04 | - 4.46 | - 2.31 | - 6.04   | - 6 . 09 | - 4.59    |
| All           | - 4.04 | - 4.60 | - 2.21 | - 6.10   | - 6 . 13 | - 4.62    |

Moreover, focusing on the numerical results grouped by categories (i.e., C, R, and RC),all the approaches are more performing on clustered instances (C) with an average improvement of -5 06%, their effectiveness is reduced on random (R) and random . clustered instances (RC), with an average improvement of -4 20% and . -4 59%, . respectively.

To summarize the overall results, we can state that all the proposed approaches improve the state of the art in terms of effectiveness. T is the least effective strategy. Nevertheless, combining T with PV NS and QL leads to the best results. In fact, T P and T QL algorithms show the best performance in terms of effectiveness, with a moderate increase in computational times (Tables A.14-A.16) 2 . This behavior was expected, since T works only on the shaking phase. However, the shaking procedure of T , taking advantages by the use of the attractiveness concept, guides the research towards the promising region of the search space. Hence, the PV NS and QL can be used to smartly investigate the neighborhood structures.

## 4.4 Experimental results on sets S2 and S3

In this section we discuss the results obtained for the new sets, named S2 and S3, that we generated for the VRPODTWmd and described in Sect. 4.1. Tables A.17-A.19, available online at https://bit.ly/3u4sIla, show the detailed results on these classes.

## 4.4.1 Experimental results on set S2

Table 10 summarizes the results collected for instances belonging to S2. For each approach, we report two gap values G 1 and G 2, in bold the best ones. In what follows we describe how we calculated their values.

Let us introduce the following quantities, named B i ( ) and S i ( ) , respectively:

$$B ( i ) = \min _ { H \in \{ P, Q L, T, T P, T Q L \} } O b j _ { H } ( i ) ; \\ S ( i ) = \min _ { H \in \{ P, Q L, T, T P, T Q L \} } T i m e _ { H } ( i ).$$

$$S ( i ) = \min _ { H \in \{ P, Q L, T, T P, T Q L \} } T i m e _ { H } ( i ).$$

2 https://bit.ly/3u4sIla.

Table 10 Summary of results on set S2

|               | PVNS   | PVNS   | QL   | QL   | T    | T      | TP   | TP   | TQL    | TQL   |
|---------------|--------|--------|------|------|------|--------|------|------|--------|-------|
|               | G1     | G2     | G1   | G2   | G1   | G2     | G1   | G2   | G1     | G2    |
| 25 customers  | 0.36   | 2.19   | 0.17 | 1.55 | 1.44 | 1 . 00 | 0.08 | 3.47 | 0 . 04 | 2.50  |
| 50 customers  | 1.85   | 2.76   | 1.39 | 1.76 | 3.38 | 1 . 00 | 0.27 | 3.85 | 0 . 06 | 2.58  |
| 100 customers | 2.02   | 2.35   | 1.59 | 1.63 | 4.50 | 1 . 00 | 0.28 | 3.51 | 0 . 12 | 2.39  |
| Category C    | 0.73   | 2.57   | 0.58 | 1.60 | 2.52 | 1 . 00 | 0.09 | 3.98 | 0 . 04 | 2.66  |
| Category R    | 2.18   | 2.27   | 1.61 | 1.62 | 3.73 | 1 . 00 | 0.34 | 3.34 | 0 . 08 | 2.36  |
| Category RC   | 1.32   | 2.46   | 0.96 | 1.72 | 3.08 | 1 . 00 | 0.20 | 3.51 | 0 . 10 | 2.44  |
| All           | 1.41   | 2.43   | 1.05 | 1.65 | 3.11 | 1 . 00 | 0.21 | 3.61 | 0 . 07 | 2.49  |

We calculated G 1 and G 2 as follows

$$G _ { 1 } = \frac { 1 } { | \mathcal { I } | } \sum _ { i \in \mathcal { I } } \frac { 1 0 0 ( O b j _ { H } ( i ) - B ( i ) ) } { B ( i ) } ;$$

$$G _ { 2 } = \frac { 1 } { | \mathcal { I } | } \sum _ { i \in \mathcal { I } } \frac { T i m e _ { H } ( i ) } { S ( i ) } ;$$

$$\mathfrak { z } _ { 1 } & = \frac { 1 } { | \mathcal { I } | } \sum _ { i \in \mathcal { I } } \frac { 1 0 0 ( O b j _ { H } ( i ) - B ( i ) ) } { B ( i ) } ; \\ \mathfrak { z } _ { 2 } & = \frac { 1 } { | \mathcal { I } | } \sum _ { i \in \mathcal { I } } \frac { T i m e _ { H } ( i ) } { S ( i ) } ; \\ \dots \quad \dots \quad \dots \quad \dots \quad \dots$$

where I is the set of considered instances. In particular, G 1 is the average percentage gap with respect to the most effective solution found by all the approaches, and G 2 is the ratio with respect to the fastest approach.

On the one hand, it is evident that T QL has the best performances in terms of effectiveness, with an average gap of 0 07%, followed by . T P with an average gap equal to 0 21%. On the other one, both . T QL and T P are the most time consuming among the proposed approaches (6.75 and 8.93 seconds on average, respectively).

It is worth to note that, although T is the fastest approach, it has the worst performance in terms of effectiveness. Meanwhile T P and T QL clearly improve the effectiveness of the solutions; as well as QL is more effective than PV NS .

Hence, we can conclude that combining T with QL improves the local search selection and, as a result, it leads to a better performance, with respect to combining T and PV NS .

Tofurther analyze the behavior of the algorithms, we performed also a time-to-target test for multiple instances Reyes and Ribeiro [35]. The test shows the probability that an algorithm will find a solution at least as good as a given target value for a given problem instance, within a given running time. In other words, it is useful to study the converge speed to a solution with a given threshold, and it can be seen as a plot of success rate over time where the success is the reaching of the target solution in the given time.

Let us consider n instances I j and the corresponding targets look 4 . For j j = 1 , . . . , n , let X j ≥ 0 be a random variable representing the time taken by the heuristic H to find a solution as good as the target value (e.g., look 4 ), for instance j I j . In addition, let FXj ( x ) = P X j ( ≤ x ) and f X j ( x ) be the cumulative distribution function

![Image](image_000015_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

Fig. 1 Time-to-target for instance set S2

![Image](image_000016_32189595e925bf31557188a2bdb6c09052c5887cc27ea4bea0ff848de91482d0.png)

Table 11 Adjusted p -values of the Friedman test on instances set S2

|      | Q                | T                | T P              | T QL             |
|------|------------------|------------------|------------------|------------------|
| PVNS | 8 . 95 · 10 - 02 | 4 . 13 · 10 - 02 | 1 . 16 · 10 - 03 | 1 . 93 · 10 - 06 |
| Q    |                  | 8 . 80 · 10 - 07 | 6 . 68 · 10 - 01 | 5 . 20 · 10 - 02 |
| T    |                  |                  | 1 . 45 · 10 - 10 | 5 . 32 · 10 - 15 |
| T P  |                  |                  |                  | 6 . 42 · 10 - 01 |

and the probability density function of X j , respectively. Then, our goal is to generate a sample of the cumulative distribution function FX 1 +···+ Xn ( x ) = P X ( 1 +···+ Xn ≤ x ) . The results are shown in Fig. 1. The time [sec] is represented on the abscissa axis, while the ordinate axis shows the probability that the algorithm reaches solutions as good as the targets in that time (summed over all instances).

The convergence rate confirms the trends previously observed. In particular, T QL has the highest converge rate, followed by T P , QL PVNS , , and finally T .

Lastly, following the example of Derrac et al. [17], we performed a Friedman test to study the statistical significance of the different results of the algorithms, considering a significance level equal to α = 0 05. The test provides a . p -value equal to 1 577 . · 10 -14 . Since the p -value is less than α , we can conclude that overall the results obtained with the approaches have different distribution. We performed Pairwise Signed Ranks as post-hoc procedure in order to evaluate the adjusted p -values for each pair of the approaches. The results are reported in Table 11. This test reveals that the differences

Table 12 Summary of results on set S3

|               | PVNS   | PVNS   | QL   | QL   | T    | T      | TP     | TP   | TQL    | TQL    |
|---------------|--------|--------|------|------|------|--------|--------|------|--------|--------|
|               | G1     | G2     | G1   | G2   | G1   | G2     | G1     | G2   | G1     | G2     |
| 200 customers | 1.55   | 2.33   | 0.85 | 1.58 | 4.27 | 1 . 00 | 0.47   | 3.16 | 0 . 29 | 2.42   |
| 400 customers | 1.68   | 2.21   | 0.88 | 1.54 | 3.85 | 1 . 00 | 0 . 05 | 2.79 | 0.24   | 2.12   |
| Category C    | 1.74   | 2.34   | 1.06 | 1.57 | 4.12 | 1 . 00 | 0 . 30 | 3.05 | 0.43   | 2.32   |
| Category R    | 1.44   | 2.15   | 0.80 | 1.56 | 3.74 | 1 . 00 | 0.30   | 2.89 | 0 . 13 | 2.23   |
| Category RC   | 1.68   | 2.32   | 0.73 | 1.55 | 4.31 | 1 . 00 | 0 . 19 | 3.00 | 0.22   | 2.26   |
| Type 1        | 1.47   | 2.26   | 0.86 | 1.55 | 3.89 | 1 . 00 | 0.34   | 2.96 | 0 . 20 | 2.22   |
| Type 2        | 1.77   | 2.28   | 0.87 | 1.57 | 4.23 | 1 . 00 | 0 . 18 | 3.00 | 0.33   | 2.32   |
| All           | 1.62   | 2.27   | 0.86 | 1.56 | 4.06 | 1 . 00 | 0 . 26 | 2.98 | 0 . 26 | 2 . 27 |

Table 13 Adjusted p -values of the Friedman test on instances set S3

|          | Q                | T                | T P                               | T QL                              |
|----------|------------------|------------------|-----------------------------------|-----------------------------------|
| PVNS Q T | 2 . 43 · 10 - 03 | 5 . 56 · 10 - 04 | 1 . 36 · 10 - 12 2 . 43 · 10 - 03 | 1 . 53 · 10 - 13 8 . 24 · 10 - 16 |
|          |                  | 7 . 09 · 10 - 14 |                                   | 04                                |
|          |                  |                  | 2 . 22 · 10 - 16                  | 2 . 22 · 10 -                     |
| T P      |                  |                  |                                   | 9 . 98 · 10 - 01                  |

between Q T P , , and T QL are not statistically relevant. However, all of them are statistically different from PVNS and T .

## 4.4.2 Experimental results on set S3

Table 12 summarizes the results collected for very large instances belonging to S3 (Tables A.18 and A.19). 3 As for the set S2, for each approach we report the two gap values G 1 and G 2.

In this case, T P and T QL share the primacy showing both a G 1 equal to 0 26. . T QL performs the best for instance with 200 customers, category R, and type 1.

We performed the Friedman test also on S3. Overall, the p -value is 2 20 . · 10 -16 . Also in this case the p -value is less than α = 0 05. Table 13 reports the adjusted . p -values for each pair of the approaches. This results confirm that the difference between T P and T QL are not statistically relevant. However, for all the other pairs, the results are statistically different.

## 5 Conclusions

In this work, we studied the vehicle routing problem with occasional drivers, time windows, and multiple deliveries (VRPODTWmd). Solving this problem to the opti-

3 https://bit.ly/3u4sIla.

![Image](image_000017_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

mality in acceptable CPU times may be difficult, especially for large size instances. In particular, CPLEX solves instances up to 25 customers. Therefore, we have proposed three different learning strategies to integrate in a variable neighborhood search (VNS) framework for solving the VRPODTWmd. In particular, we have implemented five versions of the VNS using different learning techniques. To evaluate the performance of the proposed approaches, we compared them with the state-of-the-art results Macrina et al. [28]. The results have shown that our approaches exhibit high performance in terms of effectiveness and efficiency. Moreover, we have introduced two new sets of instances with up to 400 customers, and we have carried out additional computational tests to study the behavior of the proposed approaches.

Supplementary Information The online version contains supplementary material available at https://doi. org/10.1007/s11590-021-01833-x.

## References

- 1. Al Hla, Y.A., Othman, M., Saleh, Y.: Optimising an eco-friendly vehicle routing problem model using regular and occasional drivers integrated with driver behaviour control. J. Clean. Prod. 234 , 984-1001 (2019)
- 2. Alicastro, M., Ferone, D., Festa, P., Fugaro, S., Pastore, T.: A reinforcement learning iterated local search for makespan minimization in additive manufacturing machine scheduling problems. Comput. Oper. Res. 131 , 105272 (2021). https://doi.org/10.1016/j.cor.2021.105272
- 3. Allahviranloo, M., Baghestani, A.: A dynamic crowdshipping model and daily travel behavior. Transport. Res. Part E: Logist. Transport. Rev. 128 , 175-190 (2019)
- 4. Alnaggar, A., Gzara, F., Bookbinder, J.H.: Crowdsourced delivery: a review of platforms and academic literature. Omega 98 , 102139 (2019). https://doi.org/10.1016/j.omega.2019.102139
- 5. Archetti, C., Guerriero, F., Macrina, G.: The online vehicle routing problem with occasional drivers. Comput. Oper. Res. 127 , 105144 (2020)
- 6. Archetti, C., Savelsbergh, M., Speranza, M.G.: The vehicle routing problem with occasional drivers. Eur. J. Oper. Res. 254 , 472-480 (2016). https://doi.org/10.1016/j.ejor.2016.03.049
- 7. Arnau, Q., Juan, A., Serra, I.: On the use of learnheuristics in vehicle routing optimization problems with dynamic inputs. Algorithms 11 , 208 (2018). https://doi.org/10.3390/a11120208
- 8. Arslan, A.M., Agatz, N., Kroon, L., Zuidwijk, R.: Crowdsourced delivery-a dynamic pickup and delivery problem with ad hoc drivers. Transport. Sci. 53 , 222-235 (2019). https://doi.org/10.1287/trsc. 2017.0803
- 9. Barr, A., Wohl, J.: Exclusive: Wal-Mart may get customers to deliver packages to online buyers. REUTERS -. Business. (2013)
- 10. Bensinger, G.: Amazon's next delivery drone: You. , Wall Street Journal (2015)
- 11. Calvet, L., Armas, J.D., Masip, D., Juan, A.A.: Learnheuristics: hybridizing metaheuristics with machine learning for optimization with dynamic inputs. Open Math. 15 , 261-280 (2017). https:// doi.org/10.1515/math-2017-0029
- 12. Chen, B., Qu, R., Bai, R., Laesanklang, W.: A variable neighborhood search algorithm with reinforcement learning for a real-life periodic vehicle routing problem with time windows and open routes. RAIRO-Oper. Res. (2019a). https://doi.org/10.1051/ro/2019080
- 13. Chen, X., Ulmer, M.W., Thomas, B.W.: Deep Q-learning for same-day delivery with a heterogeneous fleet of vehicles and drones. (2019b) arXiv:1910.11901
- 14. Dahle, L., Andersson, H., Christiansen, M.: The vehicle routing problem with dynamic occasional drivers. In: Bekta, s, T., Coniglio, S., Martinez-Sykora, A., V oß, S. (eds.) Computational Logistics, pp. 49-63. Springer International Publishing, Cham (2017)
- 15. Dahle, L., Andersson, H., Christiansen, M., Speranza, M.G.: The pickup and delivery problem with time windows and occasional drivers. Comput. Oper. Res. 109 , 122-133 (2019). https://doi.org/10. 1016/j.cor.2019.04.023

![Image](image_000018_7f5935cd69993f5f623e7e046c37982ed7a64fc946fcc055a007f54340667b0f.png)

- 16. Dayarian, I., Savelsbergh, M.: Crowdshipping and same-day delivery: employing in-store customers to deliver online orders. Optimiz. Online 2011 , 07-6142. https://pdfs.semanticscholar.org/ cf5d/9c4c79ce5470e20ce6872403c2929c1bd446.pdf (2017)
- 17. Derrac, J., García, S., Molina, D., Herrera, F.: A practical tutorial on the use of nonparametric statistical tests as a methodology for comparing evolutionary and swarm intelligence algorithms. Swarm Evol. Comput. 1 , 3-18 (2011). https://doi.org/10.1016/j.swevo.2011.02.002
- 18. Dorigo, M., Maniezzo, V., Colorni, A.: Ant system: optimization by a colony of cooperating agents. IEEE Trans. Syst. Man Cybern. Part B (Cybernetics) 26 , 29-41 (1996). https://doi.org/10.1109/3477. 484436 Part B (Cybernetics) 26 , 29-41 (1996). https://doi.org/10.1109/3477.484436
- 19. Gdowska, K., Viana, A., Pedroso, J.P.: Stochastic last-mile delivery with crowdshipping. Transport. Res. Procedia 30 , 90-100 (2018). https://doi.org/10.1016/j.trpro.2018.09.011
- 20. Hansen, P., Mladenovi· c, N., Brimberg, J., PØrez, J. A.: Variable neighborhood search. In: Handbook of Metaheuristics, pp. 57-97. Springer International Publishing, Cham (2019)
- 21. Hansen, P., Mladenovi· c, N., Todosijevi· c, R., Hanafi, S.: Variable neighborhood search: basics and variants. EURO J. Comput. Optim. 5 , 423-454 (2017). https://doi.org/10.1007/s13675-016-0075-x
- 22. Homberger, J., Gehring, H.: A two-phase hybrid metaheuristic for the vehicle routing problem with time windows. Eur. J. Oper. Res. 162 , 220-238 (2015)
- 23. Kool, W., van Hoof, H., Welling, M.: Attention, learn to solve routing problems. In: 7th International Conference on Learning Representations, ICLR 2019 (2018) arXiv:1803.08475
- 24. Landa, R.: Thinking Creatively in the Digital Age, 1st edn. Nimble, Blue Ash, Ohio (2015)
- 25. López-IbÆæez, Dubois-Lacoste., J., CÆceres, L.P., Birattari, M., Stützle, T.: The irace package: Iterated racing for automatic algorithm configuration. Oper. Res. Persp. 3 , 43-58 (2016). https://doi.org/10. 1016/j.orp.2016.09.002
- 26. Macrina, G.: Green logistics and crowd-shipping: challenges and opportunities. University of Calabria (2018). Ph.D. thesis
- 27. Macrina, G., Di Puglia Pugliese, L., Guerriero, F.: Crowd-shipping: a new efficient and eco-friendly delivery strategy. Procedia Manufac. 42 , 483-487 (2020a)
- 28. Macrina, G., Pugliese, L.D.P., Guerriero, F.: A variable neighborhood search for the vehicle routing problem with occasional drivers and time windows. In: Proceedings of the 9th International Conference on Operations Research and Enterprise Systems - Volume 1: ICORES, organizationINSTICC. SciTePress. pp. 270-277. (2020b) https://doi.org/10.5220/0009193302700277
- 29. Macrina, G., Di Puglia Pugliese, L., Guerriero, F., Laganà, D.: The vehicle routing problem with occasional drivers and time windows. In: Sforza, A., Sterle, C. (eds.) Optimization and Decision Science: Methodologies and Appliations, ODS, Sorrento, Italy, pp. 577-587. Switzerland, Springer, Cham (2017)
- 30. Macrina, G., Di Puglia Pugliese, L., Guerriero, F., Laporte, G.: Crowd-shipping with time windows and transshipment nodes. Comput. Oper. Res. 113 , 104086 (2020c). https://doi.org/10.1016/J.COR. 2019.104806
- 31. Macrina, G., Guerriero, F.: The green vehicle routing problem with occasional drivers. In: Daniele, P., Scrimali, L. (eds.) New Trends in Emerging Complex Real Life Problems. Springer, New York LLC (2018)
- 32. Mladenovi· c, M., Hansen, P.: Variable neighborhood search. Comput. Oper. Res. 24 , 1097-1100 (1997)
- 33. Nazari, M., Oroojlooy, A., Snyder, L., TakÆc, M.: Reinforcement learning for solving the vehicle routing problem. Advances in Neural Information Processing Systems , pp. 9839-9849 (2018)
- 34. Queiroz Dos Santos, J.P., De Melo, J.D., Duarte Neto, A.D., Aloise, D.: Reactive search strategies using reinforcement learning, local search algorithms and Variable Neighborhood Search. Expert Syst. Appl. 41 , 4939-4949 (2014). https://doi.org/10.1016/j.eswa.2014.01.040
- 35. Reyes, A., Ribeiro, C.C.: Extending time-to-target plots to multiple instances. Int. Trans. Oper. Res. 25 , 1515-1536 (2018). https://doi.org/10.1111/itor.12507
- 36. Serafini, S., Nigro, M., Gatta, V., Marcucci, E.: Sustainable crowdshipping using public transport: a case study evaluation in Rome. Transport. Res. Procedia 30 , 101-110 (2018). https://doi.org/10.1016/ j.trpro.2018.09.012
- 37. Slabinac, M.: Innovative solutions for a 'last-mile' delivery-a european experience, In: 15th international scientific conference 'Business Logistics in Modern Management', Croatia. p. 111-129 (2015)
- 38. Solomon, M.M.: Algorithms for the vehicle routing and scheduling problems with time window constraints. Oper. Res. 35 , 254-265 (1987). https://doi.org/10.1287/opre.35.2.254
- 39. Sutton, R.S., Barto, A.G.: Reinforcement Learning: An Introduction. The MIT Press, Cambridge (2018)

![Image](image_000019_4f7f589c5adffb620bdbb4ee17a2aef0f24b05f670ab91353abcec298e074ab7.png)

- 40. Sze, J.F., Salhi, S., Wassan, N.: A hybridisation of adaptive variable neighbourhood search and large neighbourhood search: application to the vehicle routing problem. Expert Syst. Appl. 65 , 383-397 (2016). https://doi.org/10.1016/j.eswa.2016.08.060
- 41. Thevenin, S., Zufferey, N.: Learning Variable Neighborhood Search for a scheduling problem with time windows and rejections. Discr. Appl. Math. 261 , 344-353 (2019). https://doi.org/10.1016/j.dam. 2018.03.019
- 42. Vera, J.M., Abad, A.G.: Deep reinforcement learning for routing a heterogeneous fleet of vehicles. (2019) arXiv:1912.03341
- 43. Watkins, C.J.C.H., Dayan, P.: Q-learning. Mach. Learn. 8 , 279-292 (1992). https://doi.org/10.1007/ bf00992698

Publisher's Note Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

![Image](image_000020_7f5935cd69993f5f623e7e046c37982ed7a64fc946fcc055a007f54340667b0f.png)