![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000000_690f18385eb2247308aabc4ebc318ef05d7b3c2e9d499fe0d4f02f8cfd68e55d.png)

Contents lists available at ScienceDirect

## Journal of Cleaner Production

j o urnal homepage: www.elsevier.com/locate/jclepro

## Collaborative two-echelon multicenter vehicle routing optimization based on state e space e time network representation

Yong Wang a, b, * , Yingying Yuan a , Xiangyang Guan c, ** , Maozeng Xu a, *** , Li Wang d , Haizhong Wang e , Yong Liu a

- a School of Economics and Management, Chongqing Jiaotong University, Chongqing, 400074, China
- b School of Management and Economics, University of Electronic Science and Technology, Chengdu, 610054, China
- c Department of Civil and Environmental Engineering, University of Washington, Seattle, WA, 98195, USA
- d School of Humanities and Social Science, Shanxi Medical University, Taiyuan, 030001, China
- e School of Civil and Construction Engineering, Oregon State University, Corvallis, OR, 97330, USA

## a r t i c l e i n f o

## a b s t r a c t

Article history:

Received 1 July 2019

Received in revised form

4 January 2020

Accepted 14 February 2020

Available online 18 February 2020

Handling editor: Hua Cai

Keywords:

State e space e time network Synchronization Resource sharing Multicenter vehicle routing problem Cost gap allocation

Collaboration among service providers in a logistics network can greatly increase their operation ef /uniFB01 -ciencies and reduce transportation emissions. This study proposes, formulates and solves a collaborative two-echelon multicenter vehicle routing problem based on a state e space e time (CTMCVRP-SST) network to facilitate collaboration and resource sharing in a multiperiod state e space e time (SST) logistics network. The CTMCVRP-SST aims to facilitate collaboration in logistics networks by leveraging the spatial-temporal properties of logistics demands and resources to optimize the distribution of logistics resources in space and time to meet logistics demands. A three-component solution framework is proposed to solve CTMCVRP-SST. First, a bi-objective linear programming model based on resource sharing in a multiperiod SST network is formulated to minimize the number of vehicles and the total cost of the collaborative operation. Second, an integrated algorithm consisting of SST-based dynamic programming (DP), improved K -means clustering and improved non-dominated sorting genetic algorithm-II (Im-NSGAII) is developed to obtain optimal routes. Third, a cost gap allocation model is employed to design a collaborative mechanism that encourages cooperation among logistics service providers. Using this solution framework, the coalition sequences (i.e., the order of each logistics provider joining a collaborative coalition) are designed and the stability of the coalitions based on pro /uniFB01 t allocations is studied. Results show that the proposed algorithm outperforms existing algorithms in minimizing the total cost with all other constraints being the same. An empirical case study of a logistics network in Chongqing suggests that the proposed collaboration mechanism with SST network representation can reduce costs, improve transportation ef /uniFB01 ciency, and contribute to ef /uniFB01 cient and sustainable logistics network operations.

© 2020 Elsevier Ltd. All rights reserved.

## 1. Introduction

Given economic and environmental constraints, our society can

* Corresponding author. School of Economics and Management, Chongqing Jiaotong University, Chongqing, 400074, China.

** Corresponding author.

*** Corresponding author.

E-mail addresses: yongwx@cqjtu.edu.cn (Y. Wang), yingyingyuan156@163.com

(Y. Yuan), guanxy@uw.edu (X. Guan), xmzzrxhy@cqjtu.edu.cn (M. Xu), research99@sxmu.edu.cn (L. Wang), Haizhong.Wang@oregonstate.edu (H. Wang), liuyongcs@cqjtu.edu.cn (Y. Liu).

mobilize only a limited amount of logistics resources (e.g., facilities and trucks). How to ef /uniFB01 ciently utilize these resources to meet the ever-increasing customer demands is a critical question (Mahmoudi and Zhou, 2016; Mahmoudi et al., 2019). Collaboration among logistics service providers is a promising approach to increase logistics operation ef /uniFB01 ciency by sharing the limited logistics resources (Wang et al., 2019b, 2020). To effectively facilitate collaboration and resource sharing in order to achieve optimal resource allocation both spatially and temporally, it is necessary to consider the state (e.g., which customer demands to be served by a truck from a logistics facility), time (e.g., the arrival time for each customer) and space (e.g., available space on a truck) characteristics

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000001_90aa996f451aac21c3a8faf7c1ad349932e21f7bd5ddc014b4b62d360af8dd2b.png)

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000002_d30812d6516d93fc16a32244714f4d1339656bc69dfe8eaea1bca269482f8961.png)

of logistics network operation, as well as customer demand characteristics (e.g., customers ' time windows).

The traditional two-echelon multicenter vehicle routing problem aims to connect depots to customers through a /uniFB02 eet of vehicles on the designed routes (Wang et al., 2015; Zhou et al., 2017). Customers on each route can only be served by a single vehicle, which returns to its departure depot after serving customers (Belgin et al., 2018). The collaborative two-echelon multicenter vehicle routing problem (CTMCVRP) is an extension of the two-echelon multicenter vehicle routing problem and considers a collaborative mechanism among distribution centers (DCs). The CTMCVRP emphasizes collaboration, which means coordinating the nodes (i.e., customers and DCs) of the multicenter logistics network (Wang et al., 2017a). In the CTMCVRP, customers adjacent to a DC can be assigned together and served by a single vehicle. Customer demands served by DCs may be reorganized by transferring goods among DCs using trucks. Studying this problem can decrease the total operational costs of the entire logistics network.

With the advent of new technologies such as big data and electronic commerce, freight demands are increasing in number and diversity. In 2018, the total amount of online retail sales reached 13 million dollars, which represents a 24% increase over 2017 and maintains a high growth rate (CD, 2018). The soaring demand increases the complexity of the logistics network and triggers strict requirements for the timeliness and quality of services, which pose challenges for express delivery. The decision on whether to deliver (state), the available space on a vehicle (space) and the service time for logistics facilities and customers (time) affect the quality of delivery services. Logistics networks operating under different state e space e time (SST) conditions thus need different operation strategies. This setup of contemporary logistics operation emphasizes the need to study the CTMCVRP based on the state e space e time (CTMCVRP-SST) network. Different from the traditional CTMCVRP, the CTMCVRP-SST improves the ef /uniFB01 ciency of collaboration by considering sharing logistics resources and allocating pro /uniFB01 ts. The CTMCVRP-SST aims to /uniFB01 nd a collaborative mechanism among multiple logistics facilities with the objective of minimizing the operational cost.

The CTMCVRP-SST usually includes multiple logistics centers (LCs), multiple DCs, and a large number of customers. In such a complicated logistics network, trucks are used for transportation in the /uniFB01 rst echelon between LCs and DCs. The transport routes depend on the SST characteristics of the logistics network. The customers need to be served by appropriate DCs at the expected time (within their time windows) in order to reduce the total operational cost. However, in real situation, DCs usually operate their own distribution systems independently to deliver goods to customers and thus cut off the connections among various distribution areas. In this scenario, the logistics resources are not shared and utilized rationally, resulting in low synergy of SST, such as underutilized vehicle capacities. In addition, DCs ' service quality varies, and DCs with low service quality reduce the ef /uniFB01 ciency of the entire network. Establishing an ef /uniFB01 cient collaborative mechanism through the CTMCVRP-SST can improve the service quality and optimize the utilization of logistics resources, such as distribution vehicles and logistics facilities, thus enhancing the ef /uniFB01 ciency of the entire network.

In the present study, a collaborative mechanism is proposed to coordinate the nodes of a two-echelon multidepot logistics network to improve logistics operation ef /uniFB01 ciency. The effects of SST on the collaborative mechanism are studied. The collaborative mechanism facilitates the sharing of truck space by reasonably assigning each customer to an adjacent DC. This optimization problem seeks the near-optimal routes to minimize the total transportation cost through an integrated algorithm. Customer clustering, dynamic programming (DP), and improved nondominated sorting genetic algorithm-II (Im-NSGAII) are employed to address the computational complexity and help /uniFB01 nd the best solution of the CTMCVRP-SST. A pro /uniFB01 t allocation strategy is then proposed to allocate pro /uniFB01 ts fairly. The orders in which DCs join a coalition are considered in the pro /uniFB01 t allocation strategy.

The remaining sections of this paper are organized as follows. In Section 2, the relevant literature is reviewed. In Section 3, the CTMCVRP-SST is established and elucidated. In Section 4, a biobjective optimization model is formulated to redesign the vehicle routes and minimize the total cost in the CTMCVRP-SST. In Section 5, an integrated algorithm incorporating DP, improved Kmeans clustering, and Im-NSGAII is presented to obtain the optimal routes in the CTMCVRP-SST. The pro /uniFB01 t allocation and monotonic path selection strategy are then introduced. In Section 6, a realworld case study is then conducted to test the applicability of the model and the proposed solution methodology. In Section 7, the related results are summarized, and possible future research directions are presented.

## 2. Literature review

With increasing customer demands, well-designed logistics networks become increasingly critical to the improvement of distribution ef /uniFB01 ciency. Several researchers have studied the multidepot vehicle routing problem (MDVRP) to minimize transportation cost and improve the ef /uniFB01 ciency of logistics networks (Tu et al., 2014; Oliveira et al., 2016). Given customers ' requirements for timeliness, the MDVRP with time windows (MDVRPTW) has also been studied (Afshar-Nadja /uniFB01 ., 2017; Sazonov et al., 2018). The development of e-commerce makes collaboration among multiple participants (e.g., DCs) considerably easy as it can reduce the operational costs of the entire logistics network while improving the operation ef /uniFB01 ciency (Feng et al., 2017). However, the CTMCVRP-SST is an extension of the MDVRPTW, which can be regarded as an application of SST in CTMCVRP on the basis of resource sharing and fair collaboration mechanism. Relevant literature pertaining to collaborative two-echelon MDVRPTW is presented in Section 2.1, SST network optimization is proposed in Section 2.2, and relevant solution methods and objective functions for CTMCVRP-SST are introduced in Section 2.3.

## 2.1. Collaborative two-echelon MDVRPTW

The traditional MDVRPTW aims to /uniFB01 nd the optimal routes for serving a set of customers among multiple depots under time window constraints (Bae and Moon, 2016; Wang et al., 2018b). Bae and Moon (2016) presented a hybrid genetic algorithm (GA) to obtain a near-optimal solution to the MDVRPTW by minimizing the /uniFB01 xed costs associated with the depots and expenses related to delivery distances. Afshar-Nadja /uniFB01 (2017) considered the effects of departure time on the total transportation cost for solving the MDVRPTW and aimed to minimize the total heterogeneous /uniFB02 eet cost. Wang et al. (2019a) developed a two-stage multi-objective evolutionary algorithm to solve the MDVRPTW. Among multiple entities, collaboration is an alternative to reduce logistics costs and increase pro /uniFB01 ts and thus provides many potential bene /uniFB01 ts to logistics participants (Molenbruch et al., 2017; Wang et al., 2018a, 2020; Vaziri et al., 2019). Lozano et al. (2013) merged transportation needs to achieve collaboration among participants; the merging was effective in reducing the participants ' expenses. Gansterer et al. (2018) studied the collaborative pickup and delivery problem, and redistributed customer requests to optimize the logistics network. Tang et al. (2019) developed a mixed linear programming model and designed a

collaborative multiperiod distribution network to cut distribution costs. Researchers have also developed different models to optimize the two-echelon logistics network (Belgin et al., 2018; Li et al., 2019). Wang et al. (2015) proposed and implemented a novel algorithm combining particle swarm optimization and GA to solve a region partition problem in a two-echelon logistics network. A similar problem with consideration of collaboration strategies in a two-echelon logistics network was solved by Wang et al. (2017b). Guimaraes et al. (2019) presented a mathematical ~ formulation incorporating pickups, deliveries and routing improvements to minimize the total cost in a two-echelon multidepot logistics network. Different types of package deliveries exist in real life, and a reasonable conversion among different types of packages can improve the distribution ef /uniFB01 ciency in logistics network (Nordin and Selke, 2010).

In a multilevel logistics network, establishing a collaborative mechanism is critical. Bene /uniFB01 ts arising from collaboration need to be reasonably allocated to participants. A fair pro /uniFB01 t allocation strategy is important for the stability of the logistics network. Cruijssen (2010) /uniFB01 rst raised the idea of supplier-initiated logistics operation to coordinate shippers to achieve synergies. Dai and Chen (2012) addressed two issues in a collaborative logistics network, namely sharing of service requests and pro /uniFB01 t allocation, and compared the performances of different pro /uniFB01 t allocation mechanisms. Kumoi and Matsubayashi (2014) proposed a cooperative game to analyze normatively stable and fair pro /uniFB01 t allocations to fairly allocate a grand coalition s ' pro /uniFB01 t. A method based on cooperative game theory was used to allocate additional pro /uniFB01 t to promote consumers ' participation in the distributed energy network (Wu et al., 2017). Yu et al. (2017) calculated the exact Shapley value to distribute the pro /uniFB01 t generated in collaborative pickup and delivery networks. A game theoretic approach was developed to maximize productivity while ensuring fair pro /uniFB01 t allocation in collaborative multi-echelon supply chains (Liu and Papageorgiou, 2018). Wang et al. (2018c) proposed a cooperation strategy to minimize carbon emissions in pickup and delivery processes, and designed a fair method of pro /uniFB01 t distribution based on cooperative game theory to stabilize alliances.

## 2.2. SST network optimization

The effects of different time windows and vehicle space constraints on the two-echelon vehicle routing problem were studied by Soysal et al. (2015). Yang et al. (2015) presented the train trajectory through a space e time path in a space e time network to use resources ef /uniFB01 ciently. Mahmoudi and Zhou (2016) studied the vehicle routing problem with pickup and delivery service time windows on the basis of vehicles ' carrying states within space e time transportation networks. Liu and Zhou (2016) proposed a new alternative modeling framework based on a timediscretized space e time network to obtain a viable and limited sets of space e time path alternatives. Yang and Zhou (2017) solved a path/uniFB01 nding problem in a stochastic transportation network by reformulating utility functions in a two-stage nonlinear stochastic programming model. Irie et al. (2019) used a time table to describe the time-evolution of each vehicle s states to obtain the track of the ' vehicles. The traditional MDVRP rarely considers the three aspects e state, space, and time e in collaborative two-echelon logistics networks with multiple service periods. In a large-scale transportation network optimization procedure, SST is introduced and can be used to study freight transportation trajectory.

Obtaining freight transportation trajectory based on SST networks can optimize complex logistics transportation networks and guide reasonable vehicle scheduling (Mahmoudi and Zhou, 2016; Boland et al., 2017; Xu et al., 2018; Mahmoudi et al., 2019; Wang et al., 2019a). Tong et al. (2015) developed a linear integer programming model to maximize transportation accessibility in the context of space e time networks to design an SST-optimized network. Wang et al. (2016) considered a time-dependent problem and formulated a 0 e 1 integer programming model in a space e time network to /uniFB01 nd paths within minimum expected travel time. Boland et al. (2017) proposed an iterative re /uniFB01 nement algorithm and utilized partially time-expanded networks to address continuous-time service network design problems. Xu et al. (2018) developed a model considering time windows and the tracking capacity in a three-dimensional SST network and aimed for a minimum cost of network operation. Boland and Savelsbergh (2019) presented dynamic discretization discovery using an integer programming technology to study time-dependent models on the traveling salesman problem with time windows. Lu et al. (2019) proposed an integer linear programming formulation based on a space e time network for location routing problems. The application of the SST model in optimizing CTMCVRP-SST networks needs to be further studied while the integer programming on time-expanded and time-dependent service network designs are important research directions.

In addition to tracking freight transportation trajectory, several researchers have studied resource sharing as an approach to reducing the operational cost and improving the ef /uniFB01 ciency of largescale logistics networks (Zhang et al., 2017; Deng et al., 2019; Wang et al., 2019b). Guedes and Borenstein (2018) studied the real-time vehicle rescheduling problem with heterogeneous /uniFB02 eet and devised a solution method with consideration of time e space networks. Wang et al. (2018a) designed a pro /uniFB01 t allocation strategy to stabilize corporations in a time e space logistics network with shared pickup and delivery services. Quintero e Araujo et al. (2019) studied an integrated routing problem by sharing resources such as facilities and /uniFB02 eets. The resource sharing problem, including the sharing of truck use space, logistics facility and time-staggered vehicle use, based on time and space conditions for multiperiod scenarios in collaborative two-echelon logistics networks has been inadequately addressed.

## 2.3. Relevant solution methods and objective functions for CTMCVRP-SST

Research solving corresponding problems provides a reference to the study of the collaborative MCVRP in a two-echelon network based on SST (Wang et al., 2015; Li et al., 2018a). Table 1 displays a comparison of relevant solution methods and objective functions for the CTMCVRP. The acronyms for the related research are as follow:

GLNC: Global logistics network con /uniFB01 gurations CLN: Collaborative logistics network optimization MDVRP: Multidepot vehicle routing problem MDVRPTW: Multidepot vehicle routing problem with time windows TSNF: Time-space network /uniFB02 ow optimization TCMDLN: Two-echelon collaborative multidepot logistics network optimization TSPTW: Traveling salesman problem with time windows CNDP: Continuous-time network design problem VRPPDTW-SST: Vehicle routing problem with pickup and delivery with time windows in state e space e time network 2E-CMCVRP: Two-echelon collaborative multicenter vehicle routing problem STSN: Space e time e state network optimization NDMCD: Network design for multiperiod collaborative distribution

Table 1 Comparison of relevant solution methods and objective functions for CTMCVRP.

| Reference in temporal order   | Acronym of problem studied   | Objective function                                            | Solution method                                          |
|-------------------------------|------------------------------|---------------------------------------------------------------|----------------------------------------------------------|
| Sheu and Lin (2012)           | GLNC                         | Minimize network con /uniFB01 guration costs                  | Statistics and analysis                                  |
| Hafezalkotob and Makui (2015) | CLN                          | Maximize /uniFB02 ow problem                                  | Game theory                                              |
| Wang et al. (2015)            | TCMDLN                       | Minimize total costs                                          | Extended partial swam optimization and genetic algorithm |
| Calvet et al. (2016)          | MDVRP                        | Minimize distribution distances and costs                     | Hybrid approach                                          |
| Li et al. (2016)              | MDVRPTW                      | Minimize total traveling costs                                | Hybrid GA                                                |
| Mahmoudi and Zhou (2016)      | VRPPDTW-SST                  | Optimize on-demand transportation systems                     | Lagrangian relaxation algorithm                          |
| Bae and Moon (2016)           | MDVRPTW                      | Minimize /uniFB01 xed costs of depots and delivery expenses   | Heuristic algorithms and GAs                             |
| Yuan and Tang (2017)          | TSNF                         | Improve the ef /uniFB01 ciency of warehouse operations        | Exact DP approach and approximate DP approach            |
| Boland et al. (2017)          | CNDP                         | Find an optimal continuous-time solution                      | Iterative re /uniFB01 nement algorithm                   |
| Wang et al. (2018d)           | 2E-CMCVRP                    | Minimize operational costs and reduce carbon dioxide emission | Im-NSGAII                                                |
| Boland and Savelsbergh (2019) | TSPTW                        | Solve time-dependent models                                   | Dynamic discretization discovery algorithms              |
| Shang et al. (2019)           | STSN                         | Passenger /uniFB02 ow state estimation                        | Lagrangian and Eulerian observations                     |
| Tang et al. (2019)            | NDMCD                        | Cut distribution costs                                        | Mixed integer linear programming formulation             |

Clustering customer demands before solving the vehicle routing problem can simplify the calculation and help optimize routes. Thus, customer clustering is an essential step in solving the CTMCVRP problem, and clustering methods can be used to reduce the complexity of large-scale logistics network optimization (Kuo et al., 2016; Defryn and Sorensen, 2017). € Yücenur and Demirel (2011) proposed a new clustering method based on GA that considers the distance between each customer and the cluster center to solve the MCVRP. Ho et al. (2012) proposed a GA-based K-means algorithm that can select which dimensions are best to be considered for each customer group during the process of searching for the optimal solution. Wang et al. (2014) proposed a fuzzy clustering algorithm to divide a large number of customers into multiple clusters to accelerate convergence in optimizing the logistics network. Zamar et al. (2017) proposed a constrained K-means algorithm that con /uniFB01 nes cluster centers to be at valid locations to optimize the bale collection problem. Feng et al. (2017) uni /uniFB01 ed Gaussian mixture modeling for clustering analysis with cluster capacity constraints. Generally, due to the inherent advantages of combination algorithms, many heuristic algorithms are widely adopted into the customer clustering process in logistics network optimization operations.

computations, which is often used to improve and combine other algorithms to increase the performance and stability of heuristics algorithms.

Heuristic algorithms are often used to optimize multicenter complex networks. Ho et al. (2008) incorporated the Clarke and Wright (CW) saving method with the nearest neighbor heuristic into hybrid GAs for the initialization step to solve the vehicle routing problem. Breunig et al. (2016) proposed a hybrid metaheuristic algorithm combining enumerative local searches with destroy-and-repair principles to optimize two-echelon routing problems. A cooperative coevolutionary algorithm was proposed to divide an MDVRP into smaller subproblems and solve them simultaneously (Oliveira et al., 2016). Zhang et al. (2017) introduced a novel vehicle routing problem considering time window and pallet loading constraints, and a hybrid approach combining tabu search and the arti /uniFB01 cial bee colony algorithm was developed to solve the new vehicle routing problem. Rabbani et al. (2017) proposed an NSGAII combined with a clustering algorithm to address a bi-objective location routing problem. Wang et al. (2018b) presented two NSGAII-based heuristics to solve a biobjective MDVRP with soft time windows. Wang (2018) solved a meal delivery problem under logistics resource sharing using two mainstream heuristic algorithms. In fact, NSGAII is a typical multi-objective evolutional algorithm for multi-objective

Exact algorithms are developed to obtain the optimal route in the optimization of the /uniFB01 rst echelon network with consideration of the second echelon. Many exact algorithms have been studied in the past (Baldacci et al., 2011; Ganguly et al., 2013). Contardo and Martinelli (2014) presented an exact algorithm for planning routes based on vehicle capacity and route length constraints. Tang et al. (2015) developed an exact algorithm to obtain the exact solution to the pickup and delivery problem in multitrip mode. Sundar and Rathinam (2016) solved a multi-depot traveling salesman problem by formulating an integer programming model and developing a branch-and-cut algorithm. Soysal and Çimenb (2017) considered transportation emissions and changing traveling time in a vehicle routing problem, and used a simulationbased restricted DP approach to solve it. Marinelli et al. (2018) presented a novel DP approach that divides the main problem into several subproblems and obtains the optimal routes in a twoechelon city logistics network. Sun et al. (2019) developed a computationally ef /uniFB01 cient algorithm based on partitioning, and applied it to minimize transportation emissions during pickup and delivery processes. In addition, the selection of suitable exact algorithms and the combination with heuristic algorithms can divide a complex problem into multiple subproblems in logistics network optimization, and then some good feasible solutions can be obtained.

In summary, the main contributions of the current study are as follows: (1) constructing a collaborative two-echelon SST logistics network based on multiple service periods, which accounts for collaborations through resource sharing including truck space sharing, logistics facility sharing and time-staggered vehicle resource sharing; (2) formulating a bi-objective optimization model considering multiple service periods, resource sharing and synchronization between trucks and other vehicles to minimize the number of vehicles and the total cost of the collaborative SST logistics operation; (3) designing a an integrated solution method including DP, improved K-means clustering and an improved heuristic algorithm based on the synchronization between trucks and vehicles and transformation between large and small packages; and (4) studying an SST model to optimize collaborative CTMCVRP-SST.

## 3. Problem statement

Solving the CTMCVRP-SST can improve the ef /uniFB01 ciency of a logistics network by enabling resource sharing (Mahmoudi and Zhou, 2016; Mahmoudi et al., 2019). The CTMCVRP-SST includes two parts. The /uniFB01 rst echelon deals with the transportation from LCs to DCs, and the second echelon addresses the delivery from DCs to customers. As a mean of transportation, trucks are used from LCs to DCs in the /uniFB01 rst echelon, whereas vehicles are operated by DCs to serve customers in the second echelon. Two transportation cycles in the /uniFB01 rst echelon are modeled with consideration of the varying time requirements (morning or afternoon) of customers. Fig. 1 shows the changes in the /uniFB01 rst echelon before and after the CTMCVRP-SST optimization. In reality, logistics facilities increase with the development of society. Those logistics facilities that /uniFB01 rst existed were also responsible for serving customers at long distances due to the lack of logistics facilities in the initial logistics network. This situation may persist due to customer loyalty and preferential policies of logistics facilities even if new and recent logistics facilities are established, leaving the logistics network in suboptimal states. Fig. 1 illustrates the problem in an SST logistics network.

leaves LC1 and arrives at LC2 after serving corresponding DCs, and then the same truck is loaded in LC2 and returns to LC1 after completing the distribution task on the next trip. Compared with the noncollaboration SST network, the collaborative SST network is organized and ordered, and the number of trucks needed is decreased. An example trip sequence considering the SST of a fourbin truck is shown in Fig. 2.

Fig. 2 shows the sequence of visited places by a truck within two service periods. A fully loaded truck leaves LC1 after completing the loading operation within a time unit and /uniFB01 rst visits DC3. [1 1 1 1] describes the loading state of the truck in Fig. 2 and indicates that the four bins are full. The state value of a bin is set to 0 after serving the corresponding DCs. Therefore, the state value of the truck changes from [1 1 1 1] to [1 1 0 0] after it serves DC3 by the end of time 4. Similarly, the state value of the truck changes each time the service for succeeding DCs is completed.

In Fig. 1(a), each DC is served twice in a working period (e.g., /uniFB01 ve working days in one week). The routes of LCs serving DCs are different for the two operations, whereas trucks always return to the LC where they departed from. Each LC serves the DCs independently. The resulting noncollaboration SST logistics network is complex with many incurred long-haul deliveries. Fig. 1(b) shows the collaborative SST network, where LCs act as coordinators to integrate the entire logistics network to achieve cooperation among DCs under the prerequisite of ensuring that each DC is served twice within the expected time windows. The loaded truck

Fig. 3 shows the changes in the second echelon before and after the CTMCVRP-SSToptimization. In Fig. 3(a), the DCs only serve their own customers and have no connection with one another in the noncollaboration SST network. This condition leads to a complex and disordered network with long-distance and cross-space transportations. In addition, customers ' requirements for delivery time result in a high number of vehicles in need for delivery. In Fig. 3 (b), the collaboration among DCs enables them to share the logistics resources and customer service. Thus, each DC only needs to serve nearby customers. Customers with different delivery time requirements can be served by the same vehicle, and thus, the number of vehicles used for delivery is greatly reduced. Unlike that in the noncollaborative SST network, the phenomenon of missing time windows and the number of vehicles used are reduced in the collaborative SST network. Each DC in the collaborative SST network needs to serve a smaller area than that served in the

Fig. 1. Illustration of CTMCVRP-SST optimization in the /uniFB01 rst echelon.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000003_342bf9787738f5137f3b5d1d702babf6b07a55c65d01d91dd6bfb3a5d73d00d3.png)

Fig. 2. Shortest path of the /uniFB01 rst echelon in SST network.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000004_c9df8b0801b47a83b8cca6e748921fbdbe5c633650ed139034fed292c338a722.png)

Fig. 3. Illustration of CTMCVRP-SST optimization in the second echelon.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000005_bd0d9a684e70c48e76eeb2f5700c357f5c54aaf886bffd60d1587b317a0ddded.png)

Table 2

Comparison before and after CTMCVRP-SST optimization.

| Cases                           |   Distribution cost ($) |   Penalty cost ($) |   Total cost ($) |   Number of vehicles |
|---------------------------------|-------------------------|--------------------|------------------|----------------------|
| Before CTMCVRP-SST optimization |                    2820 |                520 |             3340 |                   14 |
| After CTMCVRP-SST optimization  |                    1530 |                  0 |             1530 |                    5 |

## noncollaborative SST network.

Table 2 shows a comparison of the costs and numbers of vehicles in the noncollaborative and collaborative cases. The assumption includes $30 per unit time for transportation cost and $20 per unit time for penalty cost (earliness and delay penalties), and one-time unit is used to serve a customer. The transportation costs are $2820 before optimization and $1530 after optimization. The numbers of vehicles used for serving customers are 14 before optimization and 5 after optimization. The results show a signi /uniFB01 cant reduction in the total delivery cost and the number of vehicles in the collaborative SST logistics network. In other words, resource sharing can reduce the operational costs of the entire distribution network considering multiple service periods.

## 4. Related de /uniFB01 nitions and model formulation

## 4.1. De nitions /uniFB01

The CTMCVRP-SST proposed in this study is an extension of the traditional MDVRP in a collaborative SST network; it consists of multipleLCs,severalDCs,andanumberofcustomers(Mahmoudiand Zhou, 2016). Shared trucks are employed to complete the service in

the /uniFB01 rst echelon and shared vehicles are utilized to serve the customers in the second echelon within multiple working periods. Reasonable assumptions for the CTMCVRP-SST to make the collaborative SST network structure realistic can be expressed as follows:

Assumption 1. Customers ' demands are known and relatively stable within one working period.

Assumption 2. On the basis of the basic assumptions of vehicle routing in the traditional MDVRP (Tu et al., 2014), each vehicle starts from one DC and /uniFB01 nally returns to the same DC.

Assumption 3. The quantity of goods in the /uniFB01 rst echelon facilities is relatively large, and a relatively stable service time needs to be considered to realize the unloading of the goods (Mahmoudi and Zhou, 2016; Li et al., 2018b); by contrast, the quantity of goods for customers is relatively small, and the service time is relatively short; without loss of generality, the service time for each customer is considered 0.

In order to formulate CTMCVRP-SST into a mathematical model for analytical solutions, some relevant variables are de /uniFB01 ned in Table 3 as follows:

## 4.2. Mathematical model

The traditional MDVRPTW has extensively explored the cost savings of transportation networks obtained by the routing optimization (Oliveira et al., 2016). However, the optimization models that do not consider resource sharing including facility and vehicle sharing, greatly reduce the cost savings in noncollaborative logistics networks. A relatively independent logistics operation mode also causes signi /uniFB01 cant resource underutilization and environ-

F 1 and total number of vehicles F 2, is established to realize the optimization of the proposed CTMCVRP-SST with /uniFB02 exible distribution. In Eq. (1), the total cost F 1 includes the cost of the /uniFB01 rst echelon ( TC 1) and the cost of the second echelon ( TC 2). Eq. (2) expresses the total number of vehicles by /uniFB01 nding the maximum number of shared vehicles in multiple working periods.

$$\text{vehicle} \quad \min F _ { 1 } = T C _ { 1 } + T C _ { 2 }$$

$$\text{cilities} \quad F _ { 2 } = \max _ { \varphi \in \phi } \left \{ \min _ { \sum _ { \nu \in V } } v _ { \nu } ^ { \varphi } \sum _ { d \in D u \in U } x _ { d u \nu } ^ { \varphi } \right \} \quad \text{(2)} \\ \text{di and} \quad \text{On the basic of the non-of minimizing to 1-to-to-end} \, \text{the}$$

On the basis of the goal of minimizing total costs and the number of vehicles, carbon emission is calculated to explore the environmental effect of the proposed optimization strategy. The total carbon emissions can be calculated as CE ¼ min P i 2 I ∪ Dd P P P 2 I ∪ Dk 2 K 4 2 f ε , s id , f k , E k , x 4 idk þ P P d 2 D U ∪ u 2 D U ∪ P P v 2 V 4 2 f ε ,

$$\text{ned in} \quad \begin{array} { c } s _ { d u } { \cdot } f _ { v } { \cdot } E _ { v } { \cdot } X _ { d u v } ^ { \varphi } \end{array} \right ). \, The cost of the first echelon ( T C _ { 1 } ) consists of the \quad \begin{array} { c } \\ \cdot \end{array} \cdot \quad \begin{array} { c } \\ \cdot \end{array} \cdot \quad \begin{array} { c } \\ \cdot \end{array} \cdot \quad \begin{array} { c } \\ \cdot \end{array} \cdot \quad \begin{array} { c } \\ \cdot \end{array} \cdot \quad \begin{array} { c } \\ \cdot \end{array} \cdot \quad \begin{array} { c } \\ \cdot$$

following components: c 4 k i d t r wo ; ; ; ; ; ; wd x 4 k i d t r wo ; ; ; ; ; ; wd is the general transportation cost without transferring. c k i d t r w ; ; ; ; ; o ; wd x 4 iud is the transfer cost. mo 4 , Q d 4 , x 4 idk is the operational service cost of DC d . max 4 2 f fj DTK 4 jg , MK Nt is the maintenance cost of trucks. F d is the /uniFB01 xed cost for DC d . a , max f e 4 d /C0 AT k d 4 ; 0 g and b , max f AT k d 4 /C0 l 4 d ; 0 g are the penalty costs of earliness and delay for trucks, respectively. Gd 4 y d 4 refers to the /uniFB01 nancial subsidies when facility d participates in the collaboration coalition.

$$+ \sum _ { d \in D i \in I k \in K \rho \in \phi } \sum _ { d } m _ { \phi } ^ { \phi } \cdot Q _ { d } ^ { \phi } \cdot x _ { i d k } ^ { \rho } + \max _ { \phi \in \phi } \{ [ D T K _ { \phi } ] \cdot \frac { M _ { K } } { N _ { t } } + \sum _ { k \in K d \in D \rho \in \phi } \sum _ { \phi } \alpha \cdot \max \{ e _ { d } ^ { \rho } - A T _ { d \rho \rho } ^ { k }, 0 \}$$

$$T C _ { 1 } & = \sum _ { k \in K ( k, i, d t, r, w _ { w }, w _ { d } ) \in S _ { \phi } \in \phi } \sum _ { k \in K } \left ( \zeta _ { k, i, d t, r, w _ { w }, w _ { d } } ^ { \circ } x _ { k, i, d t, r, w _ { w }, w _ { d } } ^ { \circ } ) + \sum _ { k \in K } \sum _ { k ( k, i, d t, r, w _ { w }, w _ { d } ) \in S _ { \phi } } \sum _ { u \in U _ { \phi } \in \phi } ( c _ { k, i, d t, r, w _ { w }, w _ { d } } X _ { i u d } ^ { \circ } ) \\ & \quad \quad \quad$$

mental pollution and limits the /uniFB02 exibility of the logistics distribution system (Wang et al., 2018d; Gansterer et al., 2017). Therefore, in consideration of resource sharing and a collaborative SST network among different facilities, this study proposes a threecomponent solution framework (Fig. 4).

In model formulation, a mixed integer programming model with two objective functions, namely, the minimization of total cost

The cost of the second echelon ( TC 2) consists of the following components: c 4 du x 4 du v is the delivery cost for serving customers; mt 4 , Q u 4 , x 4 du v is the service cost in the delivery process. a , max f e u 4 /C0 AT u v 4 ; 0 g and b , max f AT u v 4 /C0 l 4 u ; 0 g are the penalty costs of earliness and delay for vehicles, respectively; and max 4 2 f fj UTV 4 jg , M v Nt is the maintenance cost of vehicles.

$$T C _ { 2 } = \sum _ { v \in V u \in U u \partial d \in D u \varphi \varphi } \sum _ { v \in V u \in U u d \in D u \varphi \in \phi } \sum _ { p } m _ { t } ^ { \phi } \cdot Q _ { u } ^ { \phi } \cdot _ { d u r } ^ { \phi } + \sum _ { v \in V u \in U u d \in D u \varphi \in \phi } m _ { t } ^ { \phi } \cdot Q _ { u } ^ { \phi } \cdot _ { d u r } ^ { \phi } + \max _ { \varphi \in \phi } \{ [ U T V _ { \phi } ] \cdot \frac { M _ { p } } { N _ { t } } + \\ \sum _ { v \in V u \in U u \varphi \in \phi } \alpha \cdot \max \{ e _ { u } ^ { \phi } - A T ^ { v } _ { u \varphi } \cdot 0 \} + \sum _ { v \in V u \in U u \varphi \in \phi } \sum _ { p } \beta \cdot \max \{ A T ^ { v } _ { u \varphi } - I ^ { v } _ { u } \cdot 0 \}$$

## Table 3

Symbols and description used in the CTMCVRP-SST model.

| Set De /uniFB01 nition            | Set De /uniFB01 nition                                                                                                                                                                                                     |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S K                               | Set of SST arcs in truck k ' s distribution network.                                                                                                                                                                       |
| K V                               | Set of trucks for between-DC transportation. Set of delivery vehicles for delivery from DCs to customers.                                                                                                                  |
| D                                 | Set of DCs.                                                                                                                                                                                                                |
| I                                 |                                                                                                                                                                                                                            |
|                                   | Set of LCs.                                                                                                                                                                                                                |
| U                                 | Set of customers.                                                                                                                                                                                                          |
| P Z                               | Set of packages to be delivered. Set of links from distributions to customers.                                                                                                                                             |
| v f                               | Set of transportation service periods within one working period, f ¼ f 1 ; 2 ; 3 ; :::; 4 g .                                                                                                                              |
| DTK                               | Set of trucks for serving DCs within the 4 th service period, 4 2 f .                                                                                                                                                      |
| 4 UTV 4                           | Set of vehicles for serving customers within the 4 th service period, 4 2 f .                                                                                                                                              |
| p p ; k                           | Set of links truck k traverses in transporting package p .                                                                                                                                                                 |
| Parameters                        | Parameters                                                                                                                                                                                                                 |
| w o                               | State of distribution in the origin.                                                                                                                                                                                       |
| w d                               | State of distribution in the destination.                                                                                                                                                                                  |
| ð i ; t ; w o Þ                   | A truck departs from DC i at time t with the state of w o . .                                                                                                                                                              |
| ð d ; r ; w d Þ 4                 | A truck arrives at DC d at time r with the state of w d                                                                                                                                                                    |
| c k ; i ; d ; t ; r ; w o ; w d   | Cost of transportation from LC or DC to DC d during the time interval [t, r ] by truck k in service period 4 , d 2 D ; i 2 I ; k 2 K .                                                                                     |
| c 4                               | Cost of delivery from DC or customer d to customer u within the 4 th service period.                                                                                                                                       |
| du DTK 4                          | Number of trucks for serving DCs within the 4 th service period, 4 2 f .                                                                                                                                                   |
| j j j UTV 4 j                     | Number of delivery vehicles for serving customers in 4 th service period, 4 2 f .                                                                                                                                          |
| G d 4                             | Subsidies provided to DC d if it agrees to cooperate in the two-echelon logistics network within the 4 th service period, d 2 D ; 4 2 f .                                                                                  |
| L K                               | Loading capacity of truck k .                                                                                                                                                                                              |
| L v                               | Loading capacity of delivery vehicle v .                                                                                                                                                                                   |
| M K                               | Annual maintenance cost of truck k.                                                                                                                                                                                        |
| M v                               | Annual maintenance cost of delivery vehicle v.                                                                                                                                                                             |
| NN                                | A very large number.                                                                                                                                                                                                       |
| a                                 | Penalty cost coef /uniFB01 cient for arriving early.                                                                                                                                                                       |
| b                                 | Penalty cost coef /uniFB01 cient of arriving late.                                                                                                                                                                         |
| N t Q 4                           | Working periods within one year. Demand from customer u in terms of the number of small packages within the 4 th service period.                                                                                           |
| u S id                            | Distance from logistics facility i to d .                                                                                                                                                                                  |
| S                                 |                                                                                                                                                                                                                            |
| du                                | Distance from DC or customer d to customer u .                                                                                                                                                                             |
| f k f                             | Fuel consumption rate of truck k per 100 miles (gallon/100miles). Fuel consumption rate of vehicle v per 100 miles (gallon/100miles).                                                                                      |
| v E                               | Emission coef /uniFB01 cient for truck k per 100 miles (gallon/100miles).                                                                                                                                                  |
| k                                 | Emission coef /uniFB01 cient for vehicle v per 100 miles (gallon/100miles).                                                                                                                                                |
| E v                               | Conversion factor from gallon to milliliter.                                                                                                                                                                               |
| ε Q 4                             | Distribution demand for DC d in terms of the number of large packages service period.                                                                                                                                      |
| d 4                               | within the 4 th Distribution quantity from DC i to DC d as the number of large packages within the 4 th service period. period.                                                                                            |
| Q id                              | Operational service cost of per demand unit within the 4 th service                                                                                                                                                        |
| m 4 o 4                           | Service cost per demand unit within the 4 th service period.                                                                                                                                                               |
| m t                               | Available quantities of small packages within the 4 th service                                                                                                                                                             |
| Q 4 small Q 4                     | period. Available quantities of large packages within the 4 th service period.                                                                                                                                             |
| big d                             | Conversion coef /uniFB01 cient between large and small packages                                                                                                                                                            |
| e 4 ; l 4                         | Operation time window for DC d within the 4 th service period.                                                                                                                                                             |
| ½ d d /C138 ½ e 4 u ; l 4 u /C138 | Service time window for customer u within the 4 th service                                                                                                                                                                 |
| 4 4                               | period. Acceptable delivery time windows for customer u within the 4 th service period.                                                                                                                                    |
| ½ el u ; ll u /C138 k             | Truck k ' s arrival time at DC d within the 4 th service period.                                                                                                                                                           |
| AT d 4 v                          | Vehicle v ' s arrival time at customer u within the 4 th service                                                                                                                                                           |
| AT u 4                            | period. Departure time of truck k form the LC or DC d within the 4 th period, K .                                                                                                                                          |
| DT k d 4                          | d 2 I ∪ D ; k 2 time of delivery vehicle v form DC i within the 4 th period, d 2 D ; v 2                                                                                                                                   |
| DT v d 4                          | Departure V . Travel time of truck k from LC or DC i to DC d within the 4 th service period, d 2 D ∪ I , i 2 I ∪ D ; k 2                                                                                                   |
| TT 4 idk TT 4                     | K . Travel time of delivery vehicle v from DC or customer d to DC or customer u within the 4 th service period, d ; u 2 D ∪ U , k 2 K . time of truck k for logistics facility d within the 4 th period, d 2 I D ; k 2 K . |
| du v                              | ∪                                                                                                                                                                                                                          |
| ST 4 dk                           | Service Service time of delivery vehicle v for customer u within the 4 th period, u 2 U ; v 2 V .                                                                                                                          |
| ST 4 u v 4 /C12                   | Number of customers served by delivery vehicle v within the 4 th service period, v 2 V ; 4 2                                                                                                                               |
| /C12 N v                          | f . Fixed cost of DC d.                                                                                                                                                                                                    |
| /C12 F d                          | d .                                                                                                                                                                                                                        |
| /C12 z d '                        | Vehicle number limit in                                                                                                                                                                                                    |
|                                   | DC                                                                                                                                                                                                                         |
| Decision variables 4 4 0.         | Decision variables 4 4 0.                                                                                                                                                                                                  |
| x 4 k ; i ; d ; t ; r ; w o ; w   | If a truck k ' s state changes from ð i ; t ; w o Þ to ð d ; r ; w d Þ within the 4 th service period, x k ; i ; d ; t ; r ; w o ; w d ¼ 1, otherwise x k ; i ; d ; t ; r ; w o ; w d ¼                                    |
| x 4 idk                           | If truck k travels from logistics facility i to d within the 4 th service period, x 4 idk ¼ 1, otherwise x 4 idk ¼ 0, i ; d 2 D ; k 2 K . 4 4                                                                              |
| x 4 du v                          | If delivery vehicle v serves customer u within the 4 th service period, x du v ¼ 1, otherwise x du v ¼ 0, d 2 D ; u 2 D ∪ U ; v 2 V .                                                                                      |
| x 4 iud                           | If customer u originally (i.e. before optimization) served by DC i changes to be served by DC d within the 4 th service period, set x 4 iud ¼ 1; otherwise,                                                                |
| 4                                 | d 2 D ; u 2 U . If the link ( i , u ) is on a delivery unit starting from DC d within the 4 th service period, set z 4 iud ¼ 1; otherwise z 4 iud ¼ 0, i ; d 2 D ; u 2 U .                                                 |
| z iud x 4 ; t ; r ; w po ; w      | pd If the state of package p in truck k changes from ð i ; t ; w o Þ to th service period, set x 4 k ; i ; d ; t ; r ; w po ; w pd ¼ 1, d 2 D ; 4 2 f ; otherwise,                                                         |
| k ; i ; d                         | ð d ; r ; w d Þ within the 4 0, ð k ; i ; d ; t ; r ; w po ; w pd Þ 2 p p ; k . If DC d agrees to cooperate in vehicle routing optimization within the 4 th                                                                |
| y d 4                             | service period, y d 4 ¼ 1, d 2 D ; 4 2 f , otherwise, set y d 4 ¼ 0. If vehicle v is selected to serve customers within the 4 th service period, set y 4 v ¼ 1, v 2 V ; 4 2 f ; otherwise, y 4 v ¼ 0.                      |
| y 4 v                             |                                                                                                                                                                                                                            |

Fig. 4. Schematic of CTMCVRP-SST optimization framework.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000006_56a688b48d858d7ec0aa9205ae88f009d8be65df3edb5ba87e11a2ff19f675be.png)

First echelon constraints:

Flow conservation at truck k ' s origin vertex:

$$& D T ^ { k } _ { d \varphi } + T T ^ { \varphi } _ { i d k } + S T ^ { \varphi } _ { d k } + N N ( 1 - x ^ { \varphi } _ { i d k } ) \\ & \quad \geq A T ^ { k } _ { d \varphi }, \forall i, d \in I \cup D, k \in K, \varphi \in \phi$$

$$& \sum _ { ( k, i, d, t, r, w _ { o }, w _ { d } ) \in S _ { k } \varphi \in \phi } x _ { k, i, d, t, r, w _ { o }, w _ { d } } ^ { \varphi } = 1, \ i = o _ { k } ^ { \prime }, t = e _ { k }, w _ { o } = w _ { d } \\ & \quad = w _ { 0 }, \forall k \in K$$

Flow conservation at truck k ' s destination vertex:

$$& \sum _ { ( k, i, d, t, r, w _ { o }, w _ { d } ) \in S _ { k } \varphi \in \phi } x _ { k, i, d, t, r, w _ { o }, w _ { d } } ^ { \varphi } = 1, \, d = d ^ { \prime } _ { k }, r = l _ { k }, w _ { o } = w _ { d } \\ & \quad = w _ { 0 }, \, \forall k \in K$$

(6)

Flow conservation at transportation network intermediate vertex:

$$= 0, \, ( i, t, w _ { 0 } ) \not \in \{ ( o ^ { \prime } _ { k }, e _ { k }, w _ { 0 } ), ( d ^ { \prime } _ { v }, e _ { k }, w _ { 0 } ) \}, \forall k \in K \quad \ \ ( 7 ) \, \stackrel { \ \ \ \ \ \ \ } } { \ \ \ \ \ } \text{$c_{con}$}$$

X X ð d r w ; ; ' d Þ 4 2 f x 4 k i d t r wo ; ; ; ; ; ; w d ' /C0 X X ð d r ' ; ' ; wd Þ 4 2 f x 4 k d ; ' ; i r ; ' ; t wo ; ; wd /C8/C0 /C1 /C0 /C1/C9 Delivery request constraints:

$$\sum _ { k \in K ( k, i, d, t, r, w _ { p o }, w _ { p d } ) \in \pi _ { p, k } } x _ { k, i, d, t, r, w _ { p o }, w _ { p d } } ^ { \varphi } = 1, \forall p \in P, \varphi \in \phi \quad ( 1 0 ) \quad \text{the arri} \\ \text{at DCs} \\ \text{large p}.$$

$$\sum _ { d \in D } \left ( Q _ { d } ^ { \varphi } \sum _ { i \in I \cup D d \in D \cup I } x _ { i d k } ^ { \varphi } \right ) \leq L _ { k }, \forall k \in K, \varphi \in \phi & \quad \text{(11)} \quad \text{ferring} \quad \text{one $se$} \\ & \text{variable}$$

$$D T ^ { \nu } _ { d \varphi } \geq A T ^ { k } _ { d \varphi } + S T ^ { k } _ { d \varphi } + N N ( 1 - x ^ { \varphi } _ { i d k } ), \forall i, d \in D, \nu \in V, k \in K, \varphi \in \phi$$

$$( 5 ) \quad Q _ { b i g } ^ { \varphi } = \delta Q _ { s m a l l } ^ { \varphi }, \varphi \in \phi$$

$$Q _ { i d } = \sum _ { u \in U } x _ { i u d } Q _ { u }, i, d \in D & & ( 1 6 ) \\ ^ { \prime } _ { d }$$

$$x _ { k, i, d, t, r, w _ { o }, w _ { d } } ^ { \varphi } = \{ 0, 1 \}, \, \forall \, ( k, i, d, t, r, w _ { o }, w _ { d } ) \in S _ { K }, \varphi \in \phi \quad \ \ ( 1 7 )$$

$$( 6 ) \quad x _ { k, i, d, t, r, w _ { p o }, w _ { p d } } ^ { \varphi } = \{ 0, 1 \}, \forall \left ( k, i, d, t, r, w _ { p o }, w _ { p d } \right ) \in \pi _ { p, k }, \varphi \in \phi \quad \\ \text{indiate}$$

$$x _ { i d k } ^ { \varphi } = \{ 0, 1 \}, \forall i \in I \cup D, d \in I \cup D, k \in K, \varphi \in \phi$$

$$y _ { d \varphi } = \{ 0, 1 \}, d \in D, \varphi \in \phi$$

Constraint (10) ensures that each package is transported by one and only one truck within a service period. Constraint (11) limits truck load to the truck s capacity. Constraints (12) ' e (13) formulate the arrival time of the truck k /uniFB01 rst departing from an LC and arriving at DCs within one service period. Constraint (14) ensures that the large packages in the trucks can be reorganized into small package and be loaded on to the delivery vehicles before their departures within one service period. Constraint (15) describes the transferring relationship from large packages to small packages within one service period. Constraints (16) e (20) are binary decision variables.

Second echelon constraints:

$$& D T ^ { k } _ { d \varphi } + T T ^ { \varphi } _ { i d k } + S T ^ { \varphi } _ { d k } - N N ( 1 - x ^ { \varphi } _ { i d k } ) \\ & \quad \leq A T ^ { k } _ { d \varphi }, \, \forall i, d \in I \cup D, k \in K, \, \varphi \in \phi$$

$$( 1 2 ) \sum _ { v \in V d \in D \cup U } x _ { d u v } ^ { \varphi } = 1, \forall u \in U, \varphi \in \phi$$

$$\sum _ { u \in U } \left ( Q _ { u } ^ { \varphi } \sum _ { d \in D \cup U u \in D \cup U } x _ { d u v } ^ { \varphi } \right ) \leq L _ { v }$$

$$\sum _ { d \in D \cup U } x _ { d u v } ^ { \varphi } - \sum _ { j \in D \cup U } x _ { u j v } ^ { \varphi } = \ 0, \forall u \in U, v \in$$

$$\sum _ { d, u \in D \cup U } x _ { d u v } ^ { \varphi } \leq | N _ { v } | - 1, \forall v \in V, \varphi \in \phi \quad & \quad \quad \quad$$

$$\sum _ { ( i, u ) \in \mathcal { Z } _ { r } } z _ { i u d } ^ { \varphi } \leq z _ { d } ^ { \prime }, \forall d \in D, \varphi \in \phi & &$$

$$\sum _ { u \in U v \in V } Q _ { u } ^ { \varphi } x _ { d u v } ^ { \varphi } \leq Q _ { d } ^ { \varphi }, \forall \varphi \in \phi, d \in D & ( 2 6 ) \quad \text{when a} \\ \quad \text{Therefc}$$

$$D & T ^ { \nu } _ { d \varphi } + T ^ { \varphi } _ { d u v } + S T ^ { \varphi } _ { u v } - N N ( 1 - x ^ { \varphi } _ { d u v } ) & \quad \text{SST-bas} \\ & \leq A & T ^ { \nu } _ { u \varphi }, \forall u, d \in D \cup U, v \in V, \varphi \in \phi & ( 2 7 ) & \quad \text{solve t} \\ & \quad \text{the cu}.$$

$$& D T ^ { \nu } _ { d \varphi } + T T ^ { \varphi } _ { d u \nu } + S T ^ { \varphi } _ { u \nu } + N N ( 1 - x ^ { \varphi } _ { d u \nu } ) \\ & \quad \geq A T ^ { \nu } _ { u \varphi }, \forall u, d \in D \cup U, \nu \in V, \varphi \in \phi$$

$$\geq A T _ { u \varphi } ^ { v }, \forall u, d \in D \cup U, v \in V, \varphi \in \phi & & ( 2 8 ) \quad \ e \in \Omega _ { A S \, \colon }$$

$$D T ^ { \nu } _ { d \varphi } + T T ^ { \varphi } _ { d u \nu } \leq l l ^ { \varphi } _ { u }, \forall u, d \in D \cup U, \nu \in V, \varphi \in \phi \quad \ \ ( 2 9 ) \quad \text{windov}$$

$$D T ^ { \nu } _ { d \varphi } + T T ^ { \varphi } _ { d u \nu } \geq e l ^ { \varphi } _ { u }, \forall u, d \in D \cup U, \nu \in V, \varphi \in \phi \quad \quad ( 3 0 ) \quad \text{the ne} \, \text{e}$$

$$x _ { d u v } ^ { \varphi } = \{ 0, 1 \}, \forall d \in D \cup U, u \in D \cup U, \varphi \in \phi & & ( 3 1 ) \quad \text{ method} \quad$$

$$z _ { j u d } ^ { \varphi } = \{ 0, 1 \}, \forall j, u \in U, d \in D & & ( 3 2 ) \quad \text{CTMCV} \\ & \text{dance} \, \upsilon$$

Constraint (21) ensures that each customer is served by only one DC within one service period. Constraint (22) limits delivery vehicles ' loads to their capacities. Constraint (23) ensures the conservation of goods /uniFB02 ow at the delivery process. Constraint (24) guarantees that delivery vehicles leaving from DCs return to DCs

Fig. 5. CTMCVRP-SST optimization procedure.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000007_606eede97d673ea8dc95ac7176dae118bc039820797732c3dbfa277486752ad8.png)

after visiting customers. Constraint (25) ensures the number of vehicles utilized is not more than the available vehicles. Constraint (26) ensures that the total demand from customers is no more than the total amount of goods delivered from DCs. Constraints (27) e (28) formulate the arrival time of delivery vehicle v at the customers. Constraints (29) e (30) ensure that the customers are served in their acceptable time windows. Constraints (31) e (32) are binary decision variables.

## 5. Solution methodology

## 5.1. Optimization method for CTMCVRP-SST

Many integrated algorithms are used to obtain the shortest path under time window constrains in a transportation network and show a good performance in solving the MDVRPTW and CTMCVRP (Yücenur and Demirel, 2011; Du et al., 2017; Wang et al., 2017b). Therefore, an integrated algorithm, including clustering algorithm, SST-based DP method, and Im-NSGAII algorithm is proposed to solve the CTMCVRP-SST. The clustering algorithm is used to cluster the customers, the DP method is used to optimize the transportation network that connects LCs and DCs, and the Im-NSGAII is used to optimize the delivery network between DCs and customers. The CTMCVRP-SST optimization procedure is shown in Fig. 5.

As shown in Fig. 5, /uniFB01 rst, the customer clustering is performed. The customers are clustered according to their locations and time windows. Customers geographically close to one another and with similar time windows are placed in the same cluster and served by the nearest DC. Second, Im-NSGAII is used to obtain the optimal vehicle routes and compute the total operational cost and number of vehicles in the second echelon of the CTMCVRP-SST. Third, a DP method based on SST is employed to obtain the truck routes and compute the total transportation cost in the /uniFB01 rst echelon of the CTMCVRP-SST. The trucks ' transportation routes are set in accordance with the space state of the truck and the time windows and customer demands for the DCs. Finally, the costs in the /uniFB01 rst and second echelons are summed and the optimal cost of the collaborative SST network is obtained.

## 5.1.1. DP method based on SST for the /uniFB01 rst echelon of CTMCVRP

For the proposed model based on SST networks, many effective methods are employed to optimize transportation networks (Tang et al., 2015; Sundar and Rathinam, 2016), and they include the iterative re /uniFB01 nementalgorithmbasedontime-expandednetworkstosolve the optimization of service networks (Boland et al., 2017) and the dynamic discretization discovery approach to solve the traveling salesman problem with time windows (Boland and Savelsbergh, 2019). The approach based on the DP algorithm has been proved to perform well in solving the routing optimization problem based on the SST network (Mahmoudi and Zhou, 2016; Yuan and Tang, 2017). The CTMCVRP-SST is proposed in this study to optimize the /uniFB01 rstechelon transportation networks within multiple service periods. It can be decomposed to several subproblems and solved by the DP method. Therefore, a DP method is employed to regulate the DC sequence of each cluster after customer clustering to minimize the cost and the number of trucks in the /uniFB01 rst echelon of CTMCVRP-SST. H ¼ f H k i t w 4 ; ; ; /C12 /C12 /C12 i ¼ 1 2 3 ; ; ; :::; h ; 4 2 f g is set as the states of trucks within one working period. H k i t w 4 ; ; ; means truck k leaving DC i with state w within 4 th service period. LE 4 ð i k ; t k ; w Þ expresses that truck k leaves from node i at time t withstate w within the 4 thservice period. j ¼ f H E B ; ; g denotestheroadnetwork. E representsthesetoflinksof the road network, and B represents the corresponding link lengths of theroadnetwork. U denotesthevisitedDCsbefore H k d r w 4 ; ; ; 0 within 4 th service period, with H k d r w 4 ; ; ; 0 2 H and /C12 /C12 /C12 S 4 i d t r w w ; ; ; ; ; 0 /C12 /C12 /C12 being the shortest distance between DCs i and d with departure time t and arrival time r

asthetruck sstate changesfrom ' w to w ' withinthe 4 thservice period. H k 4 ; 0 ; t w ; corresponds to the case inwhich truck k is at the LC within the 4 th service period. f 4 h ; k i d t r w w ; ; ; ; ; ; 0 is the optimal function of the h th phase of the DP method within the 4 th service period. ½ e k ; l k /C138 is the limiteddeparturetimeandarrivaltimeoftruck k . The speci /uniFB01 cprocess of the DP method is described as follows.

Step 1: Possible schemes for visiting all logistics facilities based on truck state, space, and customer demands are listed.

Step 2: LE 4 ð o 0 k ; e k ; wo Þ ¼ 0 means that truck k departs at the earliest time possible with an empty state within the 4 th service period. At time t k 2 ½ e k ; l k /C138 , downstream state w 0 is derived on the basis of the possible state transition on link ( , i d ) and the arrival time r k ¼ t k þ TT i k ð ; d k ; t k Þ ; if LE 4 ð i k ; t k ; w Þ þ t 4 ð k i d t ; ; ; ; r ; w w ; 0 Þ &lt; LE 4 ð d k ; r k ; w 0 Þ , then the downstream state is updated to LE 4 ð d k ; r k ; w 0 Þ ¼ LE 4 ð i k ; t k ; w Þ þ t 4 ð k i ; ; d t ; ; r ; w w ; 0 Þ .

Step 3: Step 2 is repeated until all links are visited, and the total time spent by truck k is computed in according with the following equation.

from [1,1] to [1,0] after serving DC1. Thus, the label for departure from DC1 is ð DC 1 3 ; ; ½ 1 0 ; /C138Þ , assuming the service time is one-time unit. DC4 is served through activity ð DC 1 ; DC 4 3 7 ; ; ; ½ 1 0 ; /C138 ; ½ 0 0 ; /C138Þ and the departure label changes to ð DC 4 ; 7 ; ½ 0 0 ; /C138Þ . This process repeats until the service period ends. The total time used is computed using Eq. (33), which yields 21.

## 5.1.2. Customer clustering

The clustering algorithm is generally utilized to group customers into different clusters, which is necessary prior to constructing the initial route (Feng et al., 2017; Wang et al., 2014, 2018c). Customer similarity can be judged and calculated on the basis of customers ' geographic adjacencies and service time windows to establish a 4D space e time coordinate system consisting of customers ' geographical coordinates and service time windows. Thus, an extended K -means clustering algorithm can be applied to obtain the clustering results (Wang et al., 2018c; Zamar et al., 2017). Fig. 7 shows the customer distribution in a 4D space e time coordinate. Customers can be /uniFB01 rst clustered on the basis of the index

$$\int _ { n, k, i, d, t, w, w } \left ( H _ { k, d, t, w } ^ { n }, \Omega \right ) = \min _ { d \in \Omega } \left \{ \int _ { n - 1, k, i, d, t, w, w } \left ( H _ { k, d, t, w, w } ^ { n }, \Omega \right / \left \{ H _ { k, d, t, w } ^ { n } \right \} \right ) + \left | S _ { l, d, t, w, w } ^ { n } \right | \right \}, \eta = 1, 2, 3, \dots, h$$

Step 4: The shortest path for all trucks are computed and obtained.

time range considering the division of service periods. Then the customers can be grouped into different clusters on the basis of their geographic adjacencies and service time windows within each service period.

Fig. 6 shows an example to illustrate the proposed DP method based on SST. Each truck is assumed to carry two bins. The demand at each DC is one bin, and the service time at each facility is onetime unit. {[DC1, DC2], [DC3, DC4]} marks the scenario in which DC1 and DC2 are grouped on the same route and DC3 and DC4 are grouped on the same route in accordance with the truck space. First, on the basis of the method mentioned above, all possibilities are listed as follows: {[DC1, DC2], [DC3, DC4]}, {[DC1, DC3], [DC2, DC4]}, {[DC1, DC4], [DC2, DC3]}. Second, set LE 4 ð o 0 k ; e k ; wo Þ ¼ 0. The departure label ð LC 1 1 ; ; ½ 1 1 ; /C138Þ means that the fully loaded truck leaves LC1 at time 1. ð LC 1 ; DC 1 1 2 ; ; ; ½ 1 1 ; /C138 ; ½ 1 0 ; /C138Þ is a label for the logistics activity that indicates that the fully loaded truck leaves LC1 at time 1 and arrives at DC1 at time 2, and the truck state changes

In Fig. 7, customers are assigned to receive distribution services within two service periods consisting of [t1, t2] and [t2, t3]. In each service period, a modi /uniFB01 ed Manhattan distance is set as the distance function for calculation in a 4D space e time coordinate system. The coordinates of customers C1 and C2 are supposed to be ð a 1 ; b 1 Þ and ð a 2 ; b 2 Þ , and the service time windows of customers C1 and C2 are ð TS1 e ; TS1 l Þ and ð TS2 e ; TS2 l Þ , respectively, as shown in Fig. 7. The modi /uniFB01 ed Manhattan distance is utilized and calculated through the equation duj ¼ j a 1 /C0 a 2 j þ j b 1 /C0 b 2 j þ g 1 /C2 j TS1 e /C0 TS2 e j þ g 2 /C2 j TS1 l /C0 TS2 l j in the 4D space-time coordinate system, where g 1 and g 2 express the conversion coef /uniFB01 cient between distance and time, respectively. The extended K-means clustering algorithm is detailed in Table 4 as follows:

Fig. 6. Illustration for the DP method based on SST.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000008_2f26f594de8ce01e6e81320dc0eac2253d7e2d92069ade5b03991ec06bf72ba5.png)

Fig. 7. Illustration for extended K -means clustering algorithm in a 4D space e time network.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000009_26662123965fb640a3bb48c83a6b3cc8373f1559a4e86287c2b1012475e248f5.png)

Table 4 Extended K -means clustering algorithm procedure.

## Extended K -means clustering algorithm:

| Input:                                   | The location and number of DCs, the customers' geographical coordinates and service time windows, the conversion coefficient between distance and time   |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Output: Clustering results               | Output: Clustering results                                                                                                                               |
| 1: Step 2:                               | 1: Import customers' corresponding data and convert data matrix to data vector within each service period.                                               |
| 3: Step 4:                               | 2: Define n as the number of clusters for each service period.                                                                                           |
| 5: Step period.                          | 3: Randomly choose n cluster centers for each service6:                                                                                                  |
| 7: For customers in each service period: | 7: For customers in each service period:                                                                                                                 |
| 8: 9: 10:                                | Step 4: (Re-)assign each customer to a cluster whose cluster center is closest to this customer within the corresponding service period.                 |
| 11: 12:                                  | Step 5: Update the center of each cluster in each service period.                                                                                        |
| 13:                                      | Repeat Steps 4-5 until all customers' cluster membership no longer changes.                                                                              |
| 14:                                      |                                                                                                                                                          |
| 15: End                                  | 15: End                                                                                                                                                  |
| 16: Step 6: Report the results.          | 16: Step 6: Report the results.                                                                                                                          |

## 5.1.3. Im-NSGAII

The NSGAII algorithm is an improvement of the NSGA and was proposed by Deb et al. (2002). A fast non-dominated sorting method in NSGAII reduces the computational complexity of the algorithm. In this algorithm, parent and child populations are combined to compete for producing the new generation. The combination is conducive to keeping the good individuals in the next generation. The proposed Im-NSGAII combines the basic NSGAII with CW to minimize the total cost and the number of delivery vehicles in the

CTMCVRP-SST. First, CW is used to generate initial routes among DCs and from DCs to customers, which are better than randomly generated routes in the original NSGAII. Second, the partial-mapped crossover (PMC) and polynomial mutation are employed to generate offspring population and thus greatly expand the scope of the search. Finally, the non-optimal solution can be eliminated in each Pareto solution calculated using non-dominated sorting and crowding distance (Chan et al., 2016; Wang et al., 2018c). The Im-NSGAII process is described in Table 5.

Im-NSGAII algorithm procedure.

## Im-NSGAII algorithm:

Input: POP no\_nodes  gen  no\_runs , , , , Pm Pc Sh , , , nh , nl

Return the best solution and optimal routes

Output:

1: Initialize parameters. Set the proper population size ( POP ), the number of customers ( no\_nodes ) and the number of generations ( gen );

- 2: # Set the mutation probability ( Pm ), the crossover probability ( Pc ) and the maximum number of runs ( no\_runs ).
- 3: for run = 1:no\_runs
- 4: Generate the initial population by CW (Ho et al., 2008; Wang et al., 2019b)
- 5: for = 1: POP
- 6: Calculate the distance between DCs d and a customer u du b
- 7: Calculate the distance between DCs d and a new customer ' du b ' u
- 8: Calculate the total distance savings by ' ' ( ) s du du uu b b b b
- 9: if all nodes are visited then
- 10: Maintain the customer sequence with the maximum distance saving as the sub-optimal solution and output the current population and its parent population

11:

end if

- 12: Objective function evaluation (Mahmoudi and Zhou, 2016; Wang et al., 2020)
- 13: end for
- 14: for t = 1: gen
- 15 Merge the population with its parent population to form
- 16 Perform non-dominated sorting (Rabbani et al., 2017; Wang et al., 2018b)
- 17 Output a series of non-dominated sets and calculate the crowding distance F
- 18: if | |&lt;| POP | then F
- 19: will be filled with t P F
- 20: end if
- 21: if | | | POP | then F
- 22: The individuals in are ranked by crowding distances F
- 23: Take the with the highest rank from and insert it into F +1 F
- 24: end if

25:

Genetic operation.

26:

Carry out Partial-Mapped Crossover (PMC) and polynomial mutation

27:

end for

- 28: Obtain the optimal solution of the run.
- 29: end for

Fig. 8. Non-dominated ranking procedure.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000010_63c51a21e68560b6e97613eed39966826fb6f4d797b0f486551ddb03292c99d7.png)

Some operations shown in Table 5 are elucidated as follows:

5.1.3.1. Fast non-dominated sorting algorithm. Given an individual h in population POP Sh , is the set of individuals dominated by individual h in population POP nh . and nl represent the number of individuals dominated by h and l , respectively; and h l ; 2 S h ; h s l . q expresses the front counter, and ℝ denotes the set for storing the individuals for the ð q þ Þ 1 th front. The non-dominated-ranking concept (Kumar et al., 2009) is shown in Fig. 8. The nondominated set F q is a Pareto front, and the peer solutions in the Pareto front possess the same optimization degree. All the solutions in different Pareto fronts are calculated by objectives f 1 and f 2 . The main steps are as follows.

Step 1: S h ¼ ∅ and nh ¼ 0 are initialized, for each individual l in POP , if h dominates l , then l is added to set S h ¼ S h ∪ f g l ; otherwise, the domination counter for h is increased, and nh ¼ nh þ 1. Step 2 : All individuals with nh ¼ 0 in population POP are found and updated in the current set F1 , F1 as the /uniFB01 rst rank.

Step 3: The front counter q ¼ 1, and ℝ ¼ ∅ are initialized. Step 4: For each individual h in front F q and for each individual l in Sh , nl ¼ nl -1, which means a decrement in the domination count for individual l. If nl ¼ 0, and l ¼ q þ 1, and ℝ is updated with individual l , then ℝ ¼ ℝ ∪ l , and F l ¼ ℝ ; otherwise, individual l is discarded. Fl becomes the second rank.

Step 5: The above operations in Step 4 are repeated and continued until the entire population is ranked.

5.1.3.2. Selection. The tournament selection strategy takes a certain number of individuals from the parent population each time and then selects the best one to enter the child generation population. This operation is repeated until the child population size reaches the parent population size. The speci /uniFB01 c steps are as follows:

Step 1: n individuals are randomly selected from the parent population with probability ps (each subject has the same probability of being selected).

Step 2: The individual with the /uniFB01 tness value is selected to enter the progeny population.

Step 3: Step 2 is repeated until the selected individuals constitute a new generation.

5.1.3.3. PMC. The PMC is selected as the crossover operator. The PMC process can be explained as follows and the speci /uniFB01 c process is illustrated in Fig. 9.

Step 1: The starting and ending positions of a gene sequence in a pair of chromosomes (parents) are randomly selected.

Step 2: The two selected sets of genes on the two chromosomes are exchanged, respectively.

Step 3: Con /uniFB02 ict detection is conducted, and a mapping relationship based on the exchanged genes is established. The mapping relationship of 9-4 is taken as an example in Fig. 9. Two genes 9 in pareto-child 1 are found at the second step. The one outside of the selected gene sequence is transformed into gene 4 in accordance with the mapping relationship. The process is repeated until no con /uniFB02 ict occurs.

Step 4: Whether the new pair of progeny genes has no con /uniFB02 ict is checked, and the /uniFB01 nal results is obtained.

5.1.3.4. Polynomial mutation. The mutation operator is an auxiliary operator because of its local search ability. The mechanism of a mutation operator is to change the gene values at certain loci of individual strings in the population (Bandyopadhyay and Bhattacharya, 2014). The method of polynomial mutation is employed in this study to prevent local convergence during evolution. Polynomial mutation is a method proposed by Deb (Deb and Goyal, 1996), and it is a mutation operator mainly used by researchers in multi-objective optimization. The steps of polynomial mutation are as follows. First, a chromosome from the paternal genes is randomly selected with probability Pm. Second, the two genes are exchanged and regenerated between them. Other repeating genes are then removed and regenerated to make them different from the original genes on the parental chromosome.

5.2. Pro /uniFB01 t allocation and monotonic path selection strategy

Cost gap allocation (CGA) is a method for solving the pro /uniFB01 t allocation problem (Wang et al., 2018a, 2018c). The CGA is based on the assumption that each participant can receive certain bene /uniFB01 ts from an economic interaction. The model consists of two key concepts, namely, marginal cost and cost gap. For example, A ¼ f DC 1 ; DC 2 ; DC 3 ; DC 4 ; DC 5 ; DC 6 g represents all participants (DCs), and S is a subset of A in the collaborative multicenter SST logistics network. Participants who agree to cooperate form a coalition, and 2 6 /C0 1 cases exist, excluding the empty coalition. A single member can also form a coalition. In a pro /uniFB01 t allocation strategy, V A ð Þ is the /uniFB01 nal pro /uniFB01 t of alliance A . m c d represents the

Fig. 9. PMC procedure.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000011_5a6f789e9345226513fa6acc35df2917836f9e8f09e9fb646aa2401d9fb5d0e7.png)

marginal cost of participant d when joining coalition A g c . ð S Þ is the cost gap in coalition S. They can be calculated by Eqs. (34) e (36).

$$V ( S ) = ( 1 - \sigma ) \max \left \{ \sum _ { d \in S } C _ { 0 } ( d ) - C ( S ), 0 \right \}, S \subseteq A \quad \text{(34)} \quad \text{(2014)} \quad \text{coalitio} \quad \text{added} \quad \text{caltio}$$

$$m _ { d } ^ { c } = C ( A ) - C ( A - \{ d \} ), \forall d \in A & & ( 3 5 ) \quad \text{followi}$$

$$g ^ { c } ( S ) = V ( S ) - \sum _ { d \in S } m _ { d } ^ { c }, \, S \equiv A & ( 3 6 ) \quad \text{Step} \\ \sim \, \overbrace { \lambda \, \dots \, \dots \, \lambda \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \dots \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \dots \, \dots \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \dots \, \delta \, \delta \, \delta \, \dots \, \delta \, \delta \, \delta \, \dots \, \delta \, \delta \, \delta \, \dots \, \delta \, \delta \, \delta \, \dots \, \delta \, \delta \, \delta \, \delta \, \dots \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \dots \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \, \delta \$$

C 0 ð d Þ denotes the cost of participant d without coalition; C A ð Þ is the total cost saving when forming grand coalition A (i.e., the coalition including all participants); and s is the synergy requirement, which expresses the bene /uniFB01 ts that the alliance organizer receives. V S ð Þ represents the cost-saving of forming coalition S . The pro /uniFB01 t for each participant can be calculated in accordance with Eq. (37).

$$Z _ { d } = m _ { d } ^ { c } + \frac { \min ^ { c } ( S ) } { \sum _ { d \in A } \min ^ { c } ( S ) } g ^ { c } ( A ), \, \forall d \in A & ( 3 7 ) & \text{the} \\ C G A \, \text{can distribute profits to each participant, and its simplicity} & \text{diag}$$

CGA can distribute pro /uniFB01 ts to each participant, and its simplicity helps control the pro /uniFB01 t allocation process to ensure the coalition stability.

Participants may join a coalition in different orders, resulting in different pro /uniFB01 t allocation scenarios in the collaborative multicenter SST logistics network. The cost reduction percentage can be de /uniFB01 ned

Table 6

Description of instances.

| Instance   |   Number of customers | Number of DCs   |
|------------|-----------------------|-----------------|
| 1 e 4      |                    90 | 8,6,4,2         |
| 5 e 8      |                   110 | 8,6,4,2         |
| 9 e 12     |                   130 | 10,8,6,4        |
| 13 e 16    |                   150 | 10,8,6,4        |
| 17 e 20    |                   200 | 12,10,8,6       |

as the percentage of the original operational cost that is reduced by participating in a coalition. A strictly monotonic path (SMP) strategy can be described as follows (Cruijssen, 2010; Wang et al., 2017a, 2017b): the cost reduction percentages of all existing members in a coalition must monotonically increase when new participants are added to the coalition. The optimal sequence of adding DCs to a coalition based on the SMP strategy can be identi /uniFB01 ed by the following step:

## Step 1 : The cost reduction percentage matrices are calculated for all possible sequential coalitions based on SMP.

Step 2 : The minimum diagonal value of the cost reduction percentage matrix is selected as the characteristic value of this cost reduction percentage matrix (and thus the corresponding sequential coalition). The characteristic values of all possible sequential coalitions are compared, and the sequential coalition with the highest characteristic value is selected as the optimal sequential coalition. If two or more sequential coalitions tie, their characteristic values are de /uniFB01 ned as the second minimum diagonal values of their cost reduction percentage matrices, and the comparison is redone.

Step 3 : step 2 is repeated for second, third, and fourth minimum diagonal values until the optimal sequential coalition is identi/uniFB01 ed or all diagonal values are compared. In the latter case, any sequential coalition can be selected as the optimal sequential coalition strategy.

## 6. Empirical analyses

## 6.1. Algorithm comparison

The proposed Im-NSGAII, the multi-objective particle swarm optimization (MOPSO) algorithm (Lin et al., 2018) and the ant colony optimization (ACO) algorithm (Narasimha et al., 2013) are tested using 20 different datasets to assess the applicability of the proposed algorithm to CTMCVRP optimization; their settings are shown in Table 7. The optimal total cost of logistics operation, optimal number of delivery vehicles and computation time are

Table 7 Algorithms optimization results comparison.

| Instance   | Im-NSGAII   | Im-NSGAII   | Im-NSGAII   | ACO   | ACO     | ACO   | MOPSO   | MOPSO   | MOPSO   |
|------------|-------------|-------------|-------------|-------|---------|-------|---------|---------|---------|
| Instance   | Cost        | Vehicle     | Time        | Cost  | Vehicle | Time  | Cost    | Vehicle | Time    |
| 1          | 8211        | 12          | 205.2       | 9169  | 12      | 206.3 | 14104   | 12      | 221.6   |
| 2          | 9206        | 13          | 171.3       | 9179  | 13      | 174.1 | 10655   | 14      | 166.1   |
| 3          | 11406       | 11          | 132.2       | 10951 | 11      | 128.3 | 12117   | 11      | 143.3   |
| 4          | 21756       | 13          | 87.2        | 25856 | 11      | 85.2  | 27022   | 11      | 82.1    |
| 5          | 10171       | 15          | 209.3       | 12088 | 15      | 212.1 | 13080   | 15      | 213.2   |
| 6          | 11512       | 12          | 176.4       | 12347 | 12      | 178.1 | 12436   | 13      | 157.5   |
| 7          | 14065       | 12          | 151.1       | 20372 | 12      | 152.2 | 20059   | 12      | 138.2   |
| 8          | 32002       | 15          | 95.1        | 36823 | 13      | 94.3  | 36469   | 12      | 89.7    |
| 9          | 11898       | 17          | 283.2       | 12446 | 17      | 282.2 | 13531   | 19      | 269.3   |
| 10         | 11902       | 17          | 247.1       | 12942 | 17      | 249.4 | 14094   | 18      | 238.6   |
| 11         | 16610       | 15          | 204.3       | 24809 | 15      | 203.2 | 18574   | 16      | 177.9   |
| 12         | 22941       | 13          | 141.2       | 25298 | 13      | 139.6 | 28699   | 15      | 135.9   |
| 13         | 12460       | 19          | 270.4       | 13431 | 19      | 272.3 | 15014   | 19      | 239.7   |
| 14         | 14725       | 17          | 232.5       | 15159 | 17      | 234.2 | 15847   | 18      | 218.3   |
| 15         | 22275       | 16          | 195.2       | 26106 | 16      | 194.3 | 27402   | 17      | 179.2   |
| 16         | 29403       | 15          | 151.3       | 34664 | 15      | 154.1 | 37555   | 16      | 137.5   |
| 17         | 22701       | 23          | 372.3       | 23942 | 24      | 369.5 | 25411   | 25      | 352.4   |
| 18         | 21299       | 22          | 304.6       | 22908 | 22      | 306.1 | 23863   | 23      | 319.7   |
| 19         | 21852       | 22          | 282.9       | 23368 | 21      | 282.4 | 23411   | 22      | 271.5   |
| 20         | 25503       | 22          | 250.1       | 27090 | 22      | 251.3 | 27770   | 23      | 243.3   |
| Average    | 17595       | 16          | 208.1       | 19947 | 16      | 208.5 | 20856   | 17      | 199.8   |

t -test p

4.5578

/C0

-value

2.15E-04

/C0

6.93099

1.32E-06

Fig. 10. Spatial distribution of Logistics facilities and customers.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000012_c5d57510868d595b59cb32e65d695c76e139aff3669ed679b0a0433830fdffdc.png)

compared among the three algorithms to evaluate their effectiveness. The algorithm will be terminated when no more improvement can be found over the best-known solution within k consecutive iterations to compare the speed of convergence in three different algorithms. Parameters in the algorithms are set as follows: population size Psize ¼ 150, maximum generation number gmax ¼ 1000, crossover ratio Crosp ¼ 0 9, mutation ratio : Mutp ¼ 0 1, vehicle capacity : Q ¼ 180, travel speed TS ¼ 40, M v ¼ 100, a ¼ 0 05, : b ¼ 0 1, and : k ¼ 15. The optimal solutions from the three algorithms on the 20 datasets are presented in Table 6.

Table 7 shows the optimal solution and the computation time returned by each algorithm for each data instance. The t -test and p -value results for comparing the minimum distribution costs in the ACO and MOPSO algorithms show a signi /uniFB01 cant difference. For cost minimization, Im-NSGAII performs better than the ACO and MOPSO algorithms in most cases. Im-NSGAII also outperforms the two other algorithms in terms of the average cost of 20 instances. For example, Narasimha et al. s average cost is ' F 1 ¼ $19947, Lin et al. s ' average cost is F 1 ¼ $20856, and the average cost of the proposed Im-NSGAII is F 1 ¼ $17595. For vehicle number minimization, the results indicate that the three algorithms have similar levels of optimization effectiveness. For example, the average number of vehicles from Narasimha et al. (2013) is 16, the average number of vehicles from Lin et al. (2018) is 17, and the average number of vehicles from the proposed Im-NSGAII is 16. For computation time, the ACO algorithm tends to take longer to converge than the two other algorithms. Im-NSGAII has a slightly higher computation time than the MOPSO algorithm. Therefore, compared with the two other algorithms, the proposed Im-NSGAII can obtain better optimization solutions. With an increase in the number of customers and DCs, the successful rate of /uniFB01 nding the best optimization solution is still high.

## 6.2. Data description

Acase study of CCTMCVRP-SST is employed in Chongqing, China to test the applicability of the proposed optimization model and algorithm in the real world. In the actual logistics network of the city which has a complicated and non-optimal structure, 2 LCs, 6 DCs and 180 customers are selected to demonstrate the practical signi /uniFB01 cance of the CTMCVRP-SST optimization. Fig. 10 shows the geographical distribution of the network components. Triangles refer to DC1 and the customers it serves. Squares, stars, diamonds, circles and crosses are the corresponding symbols for DC2, DC3, DC4, DC5 and DC6, respectively. Fig. 11 shows the main transport network consisting of LCs (LC1 and LC 2), DCs (DC1, DC2, DC3, DC4, DC5, and DC6), and transportation nodes (S1, S2, S3, S4, S5 and S6). Different transportation times exist among different logistics facilities (i.e., LCs, DCs and transportation nodes). Table 8 shows the characteristics of all logistics facilities. Logistics services between LCs and DCs, and between DCs and customers exist in the original logistics network. Therefore, potential improvements to the network can be made by the CTMCVRP-SST optimization based on the collaborative mechanism.

## 6.3. Optimization results

On the basis of actual surveys and related references (Li et al., 2016; Wang et al., 2018c), the optimization model is established with the following parameter values: (1) capacity for trucks and vehicles: L k ¼ 1500, L v ¼ 200; (2) maintenance cost for trucks and vehicles: Mk ¼ 1500, M v ¼ 500; (3) /uniFB01 xed cost for each DC: x DC 1 ¼ 1500, x DC 2 ¼ 1800, x DC 3 ¼ 1050, and x DC 4 ¼ 1200, x DC 5 ¼ 1500, x DC 6 ¼ 1500 (4) emission coef /uniFB01 cient: E k ¼ 2 6, : E v ¼ 2 5.The gov-: ernment provides each DC to join the coalition with a certain amount of incentives to encourage DCs to cooperate, as shown as follows: GDC 1 ¼ 1080, GDC 2 ¼ 986, GDC 3 ¼ 623, GDC 4 ¼ 650, GDC 5 ¼ 654 and GDC 6 ¼ 649. The total cost savings if all the facilities form the grand coalition is C ¼ 4642.

In this study, /uniFB01 ve working days are viewed as one working period and each working day includes two service periods. DC1 and DC3 have a collaborative relationship at the beginning, and this relationship persists through the coalition procedure. The Im-NSGAII is utilized to reassign customers to the DCs and compute the total cost in one working period. Cost savings need to be allocated to each participant in the coalition by the CGA model. Table 9 shows the optimization results from all initial allempty coalitions, and the implementation of our proposed integration algorithm effectively reduces the total operational cost, number of vehicles, and carbon emissions. Fig. 12 shows the customers served by each DC in the initial non-optimized logistics network. Table 10 shows the transportation routes from LCs to DCs, and Fig. 13 shows the optimal customer assignment in the grand coalition.

In Table 10, compared with the initial transportation route, the optimal transportation route can serve more DCs in one truck transportation route due to the collaboration between LC1 and LC2 in the /uniFB01 rst echelon collaborative SST logistics network. For example, LC1 needs two distribution trips within service period 1 in the non-optimal network (e.g., LC1 / DC1 / DC3 / DC5 / LC2 / DC4 / DC6 / DC2 / LC1), whereas only one is needed in the optimal logistics network (e.g., LC2 / DC4 / DC5 / DC1 / LC1 / DC2 / DC3 / DC6 / LC2) within service period 1. Resource sharing, which consists of truck spaces and logistics facilities, is conducted in the /uniFB01 rst echelon collaborative SST logistics network to signi /uniFB01 cantly reduce the total operational cost. In Fig. 13, compared with the initial customer assignment, each DC has a different number of customers that should be served in the collaborative network, and the distribution routes are optimized with the time-staggered vehicle resource sharing in the CTMCVRP-SST. Resource sharing can reduce the route crossover, improve the degree of synchronization between trucks and vehicles, and realize an on-demand operation of the logistics network. These changes will greatly reduce the number of vehicles, travel distances and

Fig. 11. First echelon Transportation network.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000013_9f565ec0557474939dcc106816bee52fc277982e7cee9d26e5be4fbf09220eaa.png)

Table 8 Characteristics of logistics facilities.

| Facilities   | Number of allocated customers   | Symbol   | Time windows   | Time windows   |
|--------------|---------------------------------|----------|----------------|----------------|
|              |                                 |          | Period 1       | Period 2       |
| LC1          | /                               |          | [0, 23]        | [22, 45]       |
| LC2          | /                               |          | [0, 23]        | [22, 45]       |
| DC1          | 27                              |          | [1, 2]         | [26, 31]       |
| DC2          | 29                              |          | [16, 21]       | [30, 35]       |
| DC3          | 32                              |          | [4, 6]         | [33, 38]       |
| DC4          | 30                              |          | [9, 14]        | [22, 25]       |
| DC5          | 31                              |          | [5, 7]         | [23, 28]       |
| DC6          | 31                              |          | [11, 17]       | [41, 42]       |

transportation costs. Figs. 14 and 15 show the shortest paths of a four-bin truck during service periods 1 and 2, respectively.

assumed to be one bin, and the truck state can be changed from [1,1, 1, 0] to [1,1, 0, 0] after visiting DC3. If the truck can wait in logistics facilities during the transportation procedure without violating the time window of the next facilities to be visited, different transportation schemes will be generated, but a waiting cost is incurred.

## 6.4. CGA model application and sequential coalition selection

The CTMCVRP-SST, the pro /uniFB01 ts and cost savings should be rationally allocated to each DC to ensure long-term collaboration and the stability of the coalition. In this study, the synergy requirement parameter s ¼ 0, which means that the coalition organizer takes no pro /uniFB01 t generated by the coalition and that all the pro /uniFB01 ts are shared by the DCs. All nonempty coalition scenarios from the combinations of DCs are shown in Table 11.

As shown in Figs. 14 and 15, truck space resource sharing can be conducted among logistics facilities including LCs and DCs, and the route of the truck in service period 1 is LC1 / DC1 / / S2 DC3 / DC5 / / / S4 S6 LC2. The truck state is changed after serving DCs; for example, the demand of DC3 is

Table 11 shows the total cost savings in each coalition, and how the cost savings are allocated to the DCs in each coalition based on the CGA model. The same DC may bene /uniFB01 t differently from different coalition scenarios. For example, the cost saving for DC2 to cooperate with DC4 is $4637. However, when cooperating with DC5, the cost savings for DC2 becomes $3534. In some cases, the bene /uniFB01 ts for existing members in a coalition are reduced if new members join.

Table 9 Comparison between initial and optimized network for cost, number of vehicles and carbon emission.

| Coalition       | Cost ($)   | Cost ($)   | Num. of vehicles   | Num. of vehicles   | CO 2 (kg)   | CO 2 (kg)   | Coalition                   | Cost ($)   | Cost ($)   | Num. of vehicles   | Num. of vehicles   | CO 2 (kg)   | CO 2 (kg)   |
|-----------------|------------|------------|--------------------|--------------------|-------------|-------------|-----------------------------|------------|------------|--------------------|--------------------|-------------|-------------|
|                 | Ini.       | Opt.       | Ini.               | Opt.               | Int.        | Opt.        | Coalition                   | Ini.       | Opt.       | Ini.               | Opt.               | Int.        | Opt.        |
| {DC1}           | 10799      | 10259      | 4                  | 4                  | 38          | 38          | {DC2,DC4,DC6}               | 38311      | 25198      | 14                 | 10                 | 140         | 93          |
| {DC2}           | 12328      | 11712      | 5                  | 5                  | 36          | 36          | {DC2,DC5,DC6}               | 38397      | 27181      | 13                 | 9                  | 147         | 93          |
| {DC3}           | 12472      | 11848      | 3                  | 3                  | 41          | 41          | {DC4,DC5,DC6}               | 39080      | 28129      | 12                 | 9                  | 157         | 90          |
| {DC4}           | 13012      | 12361      | 4                  | 4                  | 46          | 46          | {{DC1,DC3},DC2,DC4}         | 48610      | 30875      | 16                 | 13                 | 161         | 88          |
| {DC5}           | 13097      | 12442      | 3                  | 3                  | 53          | 53          | {{DC1,DC3},DC2,DC5}         | 48696      | 30274      | 15                 | 12                 | 168         | 97          |
| {DC6}           | 12971      | 12323      | 5                  | 5                  | 58          | 58          | {{DC1,DC3},DC2,DC6}         | 48570      | 30751      | 17                 | 13                 | 173         | 93          |
| {DC2,DC4}       | 25340      | 17249      | 9                  | 5                  | 82          | 62          | {{DC1,DC3},DC4,DC5}         | 49380      | 32361      | 14                 | 12                 | 178         | 89          |
| {DC2,DC5}       | 25425      | 18970      | 8                  | 5                  | 89          | 71          | {{DC1,DC3},DC4,DC6}         | 49254      | 32074      | 16                 | 12                 | 183         | 101         |
| {DC2,DC6}       | 25299      | 18616      | 10                 | 7                  | 94          | 67          | {{DC1,DC3},DC5,DC6}         | 49339      | 34300      | 15                 | 12                 | 190         | 100         |
| {DC4,DC5}       | 26109      | 19495      | 7                  | 5                  | 99          | 69          | {DC2,DC4,DC5,DC6}           | 51408      | 35474      | 17                 | 13                 | 193         | 118         |
| {DC4,DC6}       | 25983      | 19630      | 9                  | 6                  | 104         | 101         | {{DC1,DC3},DC2,DC4,DC5}     | 61708      | 36585      | 19                 | 15                 | 214         | 109         |
| {DC5,DC6}       | 26069      | 22704      | 8                  | 5                  | 111         | 88          | {{DC1,DC3},DC2,DC4,DC6}     | 61582      | 38902      | 21                 | 16                 | 219         | 114         |
| {{DC1,DC3},DC2} | 35599      | 23851      | 12                 | 9                  | 115         | 64          | {{DC1,DC3},DC2,DC5,DC6}     | 61667      | 38418      | 20                 | 15                 | 226         | 115         |
| {{DC1,DC3},DC4} | 36282      | 24775      | 11                 | 9                  | 125         | 70          | {{DC1,DC3},DC4,DC5,DC6}     | 62351      | 40285      | 19                 | 15                 | 236         | 127         |
| {{DC1,DC3},DC5} | 36368      | 26236      | 10                 | 9                  | 132         | 78          | {{DC1,DC3},DC2,DC4,DC5,DC6} | 74679      | 44774      | 24                 | 18                 | 272         | 138         |
| {{DC1,DC3},DC6} | 36242      | 26020      | 12                 | 9                  | 137         | 82          |                             | 37340      | 25618      | 12                 | 10                 | 136         | 83          |
| {DC2,DC4,DC5}   | 38437      | 25704      | 12                 | 9                  | 135         | 82          | Average                     |            |            |                    |                    |             |             |

| DC1   | DC1                                                                                                |
|-------|----------------------------------------------------------------------------------------------------|
| •     | D1 D2 D3 D4 D5 D6 D7 D8 D9 D10 D11 D12 D13 D14 D15 D16 D17 D18 D19 D20 D21 D22 D23 D24 D25 D29 D30 |

| DC2   | DC2                                                                                                                 |
|-------|---------------------------------------------------------------------------------------------------------------------|
| •     | D31 D32 D33 D34 D35 D36 D37 D38 D39 D40 D41 D42 D43 D44 D45 D46 D47 D48 D49 D50 D51 D52 D53 D54 D55 D56 D57 D58 D59 |

DC3

• D26 D60

D61 D62 D63

D64 D65 D66

D67 D68 D69

D70 D71 D72

D73 D74 D75

D76 D77 D78

D79 D80 D81

D82 D83 D84

D85 D86 D87

D88 D89 D90

Fig. 12. Initial assignment of customers.

| DC4                                                                                                                                            |
|------------------------------------------------------------------------------------------------------------------------------------------------|
| • D91 D92 D93 D94 D95 D96 D97 D98 D99 D100 D101 D102 D103 D104 D105 D106 D107 D108 D109 D110 D111 D112 D113 D114 D115 D116 D117 D118 D119 D120 |

Table 10 Initial and optimal transportation routes in service period 1 and service period 2.

| LC                                                                                            | Service period 1                      | Service period 2                                    |
|-----------------------------------------------------------------------------------------------|---------------------------------------|-----------------------------------------------------|
| Initial transportation route                                                                  | Optimal transportation route          | Initial transportation route Optimal transportation |
| LC1 LC1 / DC1 / DC5 / LC1 LC1 / DC4 / LC1 LC1 / DC1 / DC3 / DC5 / LC2 / DC4 / DC6 / DC2 / LC1 | LC1 / DC1 / DC5 / LC1 LC1 / DC6 / LC1 | LC2 / DC4 / DC5 / DC1 / LC1 / DC2 / DC3 / DC6 / LC2 |
| LC2 LC2 / DC6 / DC2 / LC2 LC2 / DC3 / LC2                                                     | LC2 / DC3 / DC2 / LC2 LC2 / DC4 / LC2 | LC2 / DC4 / DC5 / DC1 / LC1 / DC2 / DC3 / DC6 / LC2 |

Fig. 13. optimized assignment of customers in the grand coalition.

| DC1   | DC1                                                                                                                                                                      |
|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| •     | D1 D2 D3 D4 D7 D8 D9 D13 D25 D26 D27 D28 D30 D42 D43 D44 D56 D73 D74 D75 D76 D84 D85 D86 D105 D108 D114 D117 D118 D119 D136 D138 D140 D145 D165 D166 D171 D173 D176 D180 |

| DC2                                                                                                                                                 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| • D10 D16 D20 D21 D31 D32 D33 D34 D35 D36 D50 D51 D52 D53 D54 D58 D69 D78 D79 D82 D83 D110 D111 D112 D134 D137 D141 D167 D168 D172 D174 D14 D23 D22 |

| DC3                                                                                                             |
|-----------------------------------------------------------------------------------------------------------------|
| • D5 D12 D19 D24 D29 D38 D39 D40 D41 D45 D49 D55 D59 D61 D62 D63 D64 D65 D66 D67 D68 D81 D139 D163 D113 D89 D37 |

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000014_576d2ddf9fea2a47affd3e9db8131504126015df2a9170c5fde318334563dce1.png)

| DC6   | DC6                                                                                                                               |
|-------|-----------------------------------------------------------------------------------------------------------------------------------|
| •     | D17 D47 D48 D60 D70 D71 D72 D87 D98 D99 D109 D131 D133 D148 D150 D151 D152 D153 D154 D155 D156 D157 D158 D159 D161 D162 D178 D179 |

For example, after DC4 and DC6 form a coalition, the addition of DC5 changes the cost saving for DC6 from $2240 to $2212. A comprehensive coalition strategy therefore requires that new members must guarantee that the bene /uniFB01 ts of other members not compromised. In other words, the coalition stability depends on the changes in members ' pro /uniFB01 ts before and after a new member joins. Fig. 16 shows the percentage of cost saving in the process of forming a grand coalition. The original collaborative relationship between DC1 and DC3, and the route satisfying SMP are shown in Fig. 16.

possible combinations, the SMP-based optimal coalition sequence is p 1 ¼ f DC 1 ; DC 3 ; DC 4 ; DC 2 ; DC 5 ; DC 6 . All possible coalition seg quences satisfying the SMP principles are listed in Table 12. The optimal collaboration sequence based on SMP principle is shown in Table 13.

The sequence of joining a coalition is essential to the pro /uniFB01 t allocation strategy and to participants ' willingness to become participants. In other words, the order in which participants are added to the coalition affects the allocation of pro /uniFB01 ts and the satisfaction of SMP principles (Wang et al., 2014). Following the test of all

In Table 13, in the process of forming the grand coalition, DC1 and DC3 join the coalition /uniFB01 rst with the cost reduction percentages of 32.69% and 17.68%, respectively. DC4 joins as the third member, and the percentages of the operational cost reduced for DC1, DC3 and DC4 are 40.13%, 24.12%, and 32.01%, respectively. DC2 is the fourth member joining the coalition, increasing the cost reduction percentages for DC1, DC3, DC4 and DC2 to 42.17%, 25.89%, 33.70% and 45.17% respectively. The /uniFB01 nal sequence of the grand coalition {DC1, DC3, DC4, DC2, DC5, DC6} yields the cost reduction percentage of {48.34%, 34.05%, 38.82%, 53.43%, 42.92%, 24.50%},

DC5

• D27 D28

D121 D122

D123 D124

D125 D126

D127 D128

D129 D130

D131 D132

D133 D134

D135 D136

D137 D138

D139 D140

D141 D142

D143 D144

D145 D146

D147 D148

D149

| DC6                                                                                                                                                          |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| • D150 D151 D152 D153 D154 D155 D156 D157 D158 D159 D160 D161 D162 D163 D164 D165 D166 D167 D168 D169 D170 D171 D172 D173 D174 D175 D176 D177 D178 D179 D180 |

Fig. 14. Shortest path of four-bins truck during service period 1 in SST network.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000015_0eb2404426495ea21018bbb43480c2b33831cbe2205ddcb323ce8dae74bf157f.png)

respectively. Table 14 shows the changes in different types of cost and the total number of trucks and delivery vehicles.

## is shown in Table 15.

In Table 14, the other cost includes operational cost, /uniFB01 xed cost, transfer service cost and government subsidies. The total cost consists of transportation, penalty, maintenance and other costs; it is decreased by solving the CTMCVRP-SST. For example, the total cost before the CTMCVRP-SST optimization presented in Table 13 is $79258, whereas the total cost is $50943 after the CTMCVRP-SST optimization. The proposed CTMCVRP-SST solution methodology can achieve resource sharing, including trucks and vehicles, among multiple service periods in the collaborative two-echelon SST logistics network. For example, the number of vehicles is 24 before optimization, whereas it is 18 after optimization; the number of vehicles is decreased by 6, and the number of trucks is decreased by 2. The research includes the synchronization between trucks and vehicles and the transformation between large and small packages, both of which can improve the operational ef /uniFB01 ciency of the collaborative two-echelon SST logistics network. Although a collaboration cost is incurred in the collaborative SST network, the total cost is still less than that without collaboration.

The above case involves four bins in a truck. However, six-bin and eight-bin trucks can also be considered to study the CTMCVRP-SST based on the above methods. Figs. 17 and 18 only display the paths and truck states in service period 1 on the basis of the two types of trucks considering the similarity between service period 1 and 2 during the optimization procedures. The comparison among the three scenarios of using four-, six- and eight-bin trucks

In Fig. 17, the six-bin truck can serve four DCs during the travel from LC1 to LC2 and two DCs from LC2 to LC1. Fig. 18 shows that six DCs can be served by one eight-bin truck in service period 1. In Table 15, all cost components of using four-bin trucks are lower than those of the other two types of trucks. The numbers of trucks are the same in the three scenarios. The transportation costs of using eight-bin trucks ($27466) and six-bin trucks ($26538) are higher than those using four-bin trucks ($25471) due to the relatively high unit transportation cost. Although the unit transportation cost of eight-bin trucks is much higher, eight-bin trucks can serve DCs with great demands. Therefore, four-bin trucks are most ef /uniFB01 cient in meeting the current demands, and eight-bin trucks are potentially selected if /uniFB02 uctuations exist in customer demands.

## 6.5. Coalition stability

In this section, the accuracy of the CGA model is tested in identifying the optimal pro /uniFB01 t allocation mechanism. Four different pro /uniFB01 t allocation methods are employed to calculate the pro /uniFB01 t to each DC; they are the improved CGA model, the Shapley value model, the minimum cost remaining savings model (MCRS), and the equal pro /uniFB01 t method (EPM) model. The result of each pro /uniFB01 t allocation mechanism is compared to the core center to compare the performance of the methods (Frisk et al., 2010). Snowball theory (Lozano et al., 2013; Frisk et al., 2010) states that the mechanism

Fig. 15. Shortest path of four-bins truck during service period 2 in SST network.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000016_9f6174399e0f6b20d6ca53d868d085fa2f118e00772497ac67cee1246e670f16.png)

Table 11 Pro /uniFB01 t allocation in CTMCVRP-SST.

| S               |   V(t) | 4 /uniFF08 s ; v /uniFF09   | S                           |   V(t) | 4 /uniFF08 s ; v /uniFF09       |
|-----------------|--------|-----------------------------|-----------------------------|--------|---------------------------------|
| {DC1}           |   1080 | (1080,0,0,0,0,0)            | {DC2,DC4,DC5}               |  12733 | (0,4843,0,3660,4230,0)          |
| {DC2}           |    986 | (0,986,0,0,0,0)             | {DC2,DC4,DC6}               |  13113 | (0,5758,0,4575,0,2780)          |
| {DC3}           |    624 | (0,0,624,0,0,0)             | {DC2,DC5,DC6}               |  11215 | (0,4962,0,0,4348,1905)          |
| {DC4}           |    651 | (0,0,0,651,0,0)             | {DC4,DC5,DC6}               |  10952 | (0,0,0,4085,4655,2212)          |
| {DC5}           |    655 | (0,0,0,0,655,0)             | {{DC1,DC3},DC2,DC4}         |  17736 | (4554,5568,3229,4385,0,0)       |
| {DC6}           |    649 | (0,0,0,0,0,649)             | {{DC1,DC3},DC2,DC5}         |  18422 | (4572,5586,3291,0,4973,0)       |
| {DC2,DC4}       |   8091 | (0,4637,0,3454,0,0)         | {{DC1,DC3},DC2,DC6}         |  17819 | (4972,5986,3647,0,0,3214)       |
| {DC2,DC5}       |   6456 | (0,3534,0,0,2921,0)         | {{DC1,DC3},DC4,DC5}         |  17019 | (4686,0,3150,4307,4877,0)       |
| {DC2,DC6}       |   6683 | (0,4870,0,0,0,1814)         | {{DC1,DC3},DC4,DC6}         |  17180 | (5270,0,3823,4980,0,3107)       |
| {DC4,DC5}       |   6614 | (0,0,0,3022,3592,0)         | {{DC1,DC3},DC5,DC6}         |  15039 | (4493,0,3168,0,4895,2483)       |
| {DC4,DC6}       |   6353 | (0,0,0,4113,0,2240)         | {DC2,DC4,DC5,DC6}           |  15934 | (0,5197,0,4014,4584,2140)       |
| {DC5,DC6}       |   3365 | (0,0,0,0,2904,461)          | {{DC1,DC3},DC2,DC4,DC5}     |  25123 | (5190,6017,3677,4834,5404,0)    |
| {{DC1,DC3},DC2} |  11748 | (6824,5034,2694,0,0,0)      | {{DC1,DC3},DC2,DC4,DC6}     |  22679 | (5110,6037,3698,4854,0,2981)    |
| {{DC1,DC3},DC4} |  11507 | (4334,0,3009,4165,0,0)      | {{DC1,DC3},DC2,DC5,DC6}     |  23249 | (5024,6038,3734,0,5425,3028)    |
| {{DC1,DC3},DC5} |  10132 | (3685,0,2360,0,4087,0)      | {{DC1,DC3},DC4,DC5,DC6}     |  22066 | (4951,0,4070,4783,5353,2910)    |
| {{DC1,DC3},DC6} |  10222 | (4530,0,3204,0,0,2488)      | {{DC1,DC3},DC2,DC4,DC5,DC6} |  29905 | (5220,6587,4247,5052,5622,3178) |

with a pro /uniFB01 t allocation result closest to the core is the best. Table 16 shows the pro /uniFB01 t allocation result for each DC under the /uniFB01 ve allocation mechanisms. Fig. 19 illustrates the core position and its distance to the result of each allocation method.

In Fig. 19, the four borders correspond to the pro /uniFB01 t allocation resulting from the four scenarios. The numbers in parentheses are the allocations of pro /uniFB01 t for DC1, DC2, DC3, DC4, DC5 and DC6. The

CGA model result is the closest to the core center, and the best pro /uniFB01 t allocation scheme among DCs is (5220, 6587, 4247, 5052, 5623, 3178). Thus, the CGA model is selected to be the most appropriate pro /uniFB01 t allocation strategy in this case study. The result indicates that individual pro /uniFB01 ts should not be regarded as the most important factor in the design of a collaborative SST logistics network. Decision makers should focus on the selection of

Fig. 16. Operational cost reduction percentages based on SMP.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000017_4dc2558d83819c399cf45a8ad0321abc30f22fa54e3a38f08e9044b2f4503e7c.png)

collaboration mode and the strategy of resource sharing in the collaborative two-echelon SST logistics network.

resource and cost savings for urban transport administration and logistics companies.

## 6.6. Implications

The CTMCVRP optimization based on a multiperiod SST logistics network provides a good organizational framework by incorporating resource sharing and collaboration among multiple logistics facilities. The proposed three-component solution framework consists of a bi-objective mathematical model and an integrated algorithm and thus offers research insights for further theoretical and empirical research. Logistics companies and governments can improve transportation resource con /uniFB01 guration by referring to the collaborative mechanism and encourage collaboration as a sustainable and environment-friendly strategy. The managerial implications of the proposed methodology can be summarized as follows:

- (1) The design of SST network makes the entire collaborative logistics distribution system /uniFB02 exible on the basis of resource sharing and fair pro /uniFB01 t allocation. The proposed model and the integration algorithm provide a research breakthrough for the application of SST in collaborative logistics networks. Trucks with different capacities have different transportation costs. The selection of trucks with different space capacities and the design of transformation between large and small packages improve the utilization rate of resource sharing, strengthen the collaboration among different logistics facilities, and boost the operational ef /uniFB01 ciency of the entire logistics network. Therefore, an effective resource sharing and fair pro /uniFB01 t allocation mechanism with the collaborative satespace-time logistics network can contribute to social
- (2) The effective collaboration mechanism of the collaborative SST logistics network can decrease the complexity of the transportation system and propel the construction of an intelligent logistics system. Resource sharing and synchronization among logistics facilities in the collaborative SST logistics network can contribute to reducing delivery trips and improving the ef /uniFB01 cient utilization of transportation resources. Original collaborative coalition relationships may exist among several logistics enterprises, and the study on the establishment of a grand coalition consists of the original collaborative coalition sequence in the collaborative SST logistics network and is of important theoretical and practical signi /uniFB01 cance. Consequently, a reasonable collaboration mechanism can be utilized to improve the reliability and stability of the collaborative SST logistics network, and further encourage local government and logistics companies to accept environment- and resource-friendly social services.

## 7. Conclusions

This study evaluates the effects of collaboration among DCs in SST logistics networks. The coalition strategies are studied to analyze each participant s willingness to cooperate to minimize the ' total cost and the total number of trucks and delivery vehicles. Different results for using four-, six-, and eight-bin trucks are compared to explain the importance of considering different space states. A three-component solution framework is proposed to optimize the total cost and number of delivery vehicles in the SST logistics network. At the /uniFB01 rst stage, a bi-objective programming model is established to minimize the number of delivery vehicles and the total cost for the collaborative SST logistics network. An

Table 12 Feasible cooperation sequence based on SMP principle.

| Participant i                                        | p 1 ¼ {[DC1, DC3], DC4, DC2, DC5,DC6}                | p 1 ¼ {[DC1, DC3], DC4, DC2, DC5,DC6}                | p 1 ¼ {[DC1, DC3], DC4, DC2, DC5,DC6}                | p 1 ¼ {[DC1, DC3], DC4, DC2, DC5,DC6}                | p 1 ¼ {[DC1, DC3], DC4, DC2, DC5,DC6}                | p 1 ¼ {[DC1, DC3], DC4, DC2, DC5,DC6}   | Participant i                           | 2 ¼ {[DC1,DC3], DC4, DC5,DC2,DC6}       | 2 ¼ {[DC1,DC3], DC4, DC5,DC2,DC6}       | 2 ¼ {[DC1,DC3], DC4, DC5,DC2,DC6}       | 2 ¼ {[DC1,DC3], DC4, DC5,DC2,DC6}       | 2 ¼ {[DC1,DC3], DC4, DC5,DC2,DC6}       | 2 ¼ {[DC1,DC3], DC4, DC5,DC2,DC6}       |
|------------------------------------------------------|------------------------------------------------------|------------------------------------------------------|------------------------------------------------------|------------------------------------------------------|------------------------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|
| Participant i                                        | DC1                                                  | DC3                                                  | DC4                                                  | DC2                                                  | DC5                                                  | DC6                                     | Participant i                           | DC1                                     | DC3                                     | DC4                                     | DC5                                     | DC2                                     | DC6                                     |
| h ð i ; p ; 2 Þ                                      | 32.69%                                               | 17.68%                                               |                                                      |                                                      |                                                      |                                         | h ð i ; p ; 2 Þ                         | 32.69%                                  | 17.68%                                  |                                         |                                         |                                         |                                         |
| h ð i ; p ; 3 Þ                                      | 40.13%                                               | 24.12%                                               | 32.01%                                               |                                                      |                                                      |                                         | h ð i ; p ; 3 Þ                         | 40.13%                                  | 24.12%                                  | 32.01%                                  |                                         |                                         |                                         |
| h i ; p ; 4                                          | 42.17%                                               | 25.89%                                               | 33.70%                                               | 45.17%                                               |                                                      |                                         | h i ; p ; 4                             | 43.39%                                  | 25.26%                                  | 33.10%                                  | 37.23%                                  |                                         |                                         |
| ð Þ h ð i ; p ; 5 Þ                                  | 48.06%                                               | 29.49%                                               | 37.15%                                               | 48.81%                                               | 41.26%                                               |                                         | ð Þ h ð i ; p ; 5 Þ                     | 48.06%                                  | 29.49%                                  | 37.15%                                  | 41.26%                                  | 48.81%                                  |                                         |
| h ð i ; p ; 6 Þ                                      | 48.34%                                               | 34.05%                                               | 38.82%                                               | 53.43%                                               | 42.92%                                               | 24.5%                                   | h ð i ; p ; 6 Þ                         | 48.34%                                  | 34.05%                                  | 38.82%                                  | 42.92%                                  | 53.43%                                  | 24.5%                                   |
| p 3 ¼ {[DC1, DC3], DC5, DC2, DC4,DC6}                | p 3 ¼ {[DC1, DC3], DC5, DC2, DC4,DC6}                | p 3 ¼ {[DC1, DC3], DC5, DC2, DC4,DC6}                | p 3 ¼ {[DC1, DC3], DC5, DC2, DC4,DC6}                | p 3 ¼ {[DC1, DC3], DC5, DC2, DC4,DC6}                | p 3 ¼ {[DC1, DC3], DC5, DC2, DC4,DC6}                | p 3 ¼ {[DC1, DC3], DC5, DC2, DC4,DC6}   |                                         | p 4 ¼ {[DC1, DC3], DC5, DC4,DC2, DC6}   | p 4 ¼ {[DC1, DC3], DC5, DC4,DC2, DC6}   | p 4 ¼ {[DC1, DC3], DC5, DC4,DC2, DC6}   | p 4 ¼ {[DC1, DC3], DC5, DC4,DC2, DC6}   | p 4 ¼ {[DC1, DC3], DC5, DC4,DC2, DC6}   | p 4 ¼ {[DC1, DC3], DC5, DC4,DC2, DC6}   |
| Participant i                                        | DC1                                                  | DC3                                                  | DC5                                                  | DC2                                                  | DC4                                                  | DC6                                     | Participant i                           | DC1                                     | DC3                                     | DC5                                     | DC4                                     | DC2                                     | DC6                                     |
| h ð i ; p ; 2 Þ                                      | 32.69%                                               | 17.68%                                               |                                                      |                                                      |                                                      |                                         | h ð i ; p ; 2 Þ                         | 32.69%                                  | 17.68%                                  |                                         |                                         |                                         |                                         |
| h ð i ; p ; 3 Þ h i ; p ; 4                          | 34.13% 42.34%                                        | 18.92% 45.31%                                        | 31.20% 25.89%                                        | 37.79%                                               |                                                      |                                         | h ð i ; p ; 3 Þ h i ; p ; 4             | 34.13% 43.39%                           | 18.92% 25.26%                           | 31.20% 37.23%                           | 33.10%                                  |                                         |                                         |
| ð Þ h ð i ; p ; 5 Þ                                  | 48.06%                                               | 29.49%                                               | 35.20%                                               | 48.81%                                               | 31.03%                                               |                                         | ð Þ h i ; p ; 5                         | 48.06%                                  | 29.49%                                  | 41.26%                                  | 37.15%                                  | 48.81%                                  |                                         |
| h ð i ; p ; 6 Þ                                      | 48.34%                                               | 34.05%                                               | 42.92%                                               | 53.43%                                               | 38.82%                                               | 24.5%                                   | ð Þ h ð i ; p ; 6 Þ                     | 48.34%                                  | 34.05%                                  | 42.92%                                  | 38.82%                                  | 53.43%                                  | 24.5%                                   |
| {[DC1, DC3], DC5, DC6, DC2,DC4}                      | {[DC1, DC3], DC5, DC6, DC2,DC4}                      | {[DC1, DC3], DC5, DC6, DC2,DC4}                      | {[DC1, DC3], DC5, DC6, DC2,DC4}                      | {[DC1, DC3], DC5, DC6, DC2,DC4}                      | {[DC1, DC3], DC5, DC6, DC2,DC4}                      |                                         | p 6 ¼ {[DC1, DC3], DC5, DC6, DC4,DC2}   | p 6 ¼ {[DC1, DC3], DC5, DC6, DC4,DC2}   | p 6 ¼ {[DC1, DC3], DC5, DC6, DC4,DC2}   | p 6 ¼ {[DC1, DC3], DC5, DC6, DC4,DC2}   | p 6 ¼ {[DC1, DC3], DC5, DC6, DC4,DC2}   | p 6 ¼ {[DC1, DC3], DC5, DC6, DC4,DC2}   | p 6 ¼ {[DC1, DC3], DC5, DC6, DC4,DC2}   |
| Participant i                                        | DC1                                                  | DC3                                                  | DC5                                                  | DC6                                                  | DC2                                                  | DC4                                     | Participant i                           | DC1                                     | DC3                                     | DC5                                     | DC6                                     | DC4                                     | DC2                                     |
| h ð i ; p ; 2 Þ                                      | 32.69%                                               | 17.68%                                               |                                                      |                                                      |                                                      |                                         | h i ; p ; 2                             | 32.69%                                  | 17.68%                                  |                                         |                                         |                                         |                                         |
| h ð i ; p ; 3 Þ                                      | 34.13%                                               | 18.92%                                               | 31.20%                                               |                                                      |                                                      |                                         | ð Þ h i ; p ; 3                         | 34.13%                                  | 18.92%                                  | 31.20%                                  |                                         |                                         |                                         |
| h ð i ; p ; 4 Þ                                      | 41.61%                                               | 25.40%                                               | 37.37%                                               | 10.06%                                               |                                                      |                                         | ð Þ h ð i ; p ; 4 Þ                     | 41.61%                                  | 25.40%                                  | 37.37%                                  | 10.06%                                  |                                         |                                         |
| h ð i ; p ; 5 Þ                                      | 46.52%                                               | 29.94%                                               | 41.42%                                               | 23.34%                                               | 48.98%                                               |                                         | h ð i ; p ; 5 Þ                         | 45.85%                                  | 32.63%                                  | 40.87%                                  | 22.43%                                  | 36.76%                                  |                                         |
| h ð i ; p ; 6 Þ                                      | 48.34%                                               | 34.05%                                               | 42.92%                                               | 24.5%                                                | 53.43%                                               | 38.82%                                  | h ð i ; p ; 6 Þ                         | 48.34%                                  | 34.05%                                  | 42.92%                                  | 24.5%                                   | 38.82%                                  | 53.43%                                  |
| p 7 {[DC1, DC3], DC6, DC5, DC2, DC4}                 | p 7 {[DC1, DC3], DC6, DC5, DC2, DC4}                 | p 7 {[DC1, DC3], DC6, DC5, DC2, DC4}                 | p 7 {[DC1, DC3], DC6, DC5, DC2, DC4}                 | p 7 {[DC1, DC3], DC6, DC5, DC2, DC4}                 | p 7 {[DC1, DC3], DC6, DC5, DC2, DC4}                 |                                         |                                         | p 8 ¼ {[DC1, DC3], DC6, DC5, DC4, DC2}  | p 8 ¼ {[DC1, DC3], DC6, DC5, DC4, DC2}  | p 8 ¼ {[DC1, DC3], DC6, DC5, DC4, DC2}  | p 8 ¼ {[DC1, DC3], DC6, DC5, DC4, DC2}  | p 8 ¼ {[DC1, DC3], DC6, DC5, DC4, DC2}  | p 8 ¼ {[DC1, DC3], DC6, DC5, DC4, DC2}  |
| Participant i                                        | DC1                                                  | DC3                                                  | DC6                                                  | DC5                                                  | DC2                                                  | DC4                                     | Participant i                           | DC1                                     | DC3                                     | DC6                                     | DC5                                     | DC4                                     | DC2                                     |
| h ð i ; p ; 2 Þ                                      | 32.69%                                               | 17.68%                                               |                                                      |                                                      |                                                      |                                         | h i ; p ; 2                             | 32.69%                                  | 17.68%                                  |                                         |                                         |                                         |                                         |
| h ð i ; p ; 3 Þ                                      | 41.95%                                               | 25.69%                                               | 19.18%                                               |                                                      |                                                      |                                         | ð Þ h i ; p ; 3                         | 41.95%                                  | 25.69%                                  | 19.18%                                  |                                         |                                         |                                         |
| h ð i ; p ; 4 Þ                                      | 41.61%                                               | 25.40%                                               | 10.06%                                               | 37.37%                                               |                                                      |                                         | ð Þ h i ; p ; 4                         | 41.61%                                  | 25.40%                                  | 10.06%                                  | 37.37%                                  |                                         |                                         |
| h ð i ; p ; 5 Þ                                      | 46.52%                                               | 29.94%                                               | 23.34%                                               | 41.42%                                               | 48.98%                                               |                                         | ð Þ h i ; p ; 5                         | 45.85%                                  | 32.63%                                  | 22.43%                                  | 40.87%                                  | 36.76%                                  |                                         |
| h ð i ; p ; 6 Þ                                      | 48.34%                                               | 34.05%                                               | 24.5%                                                |                                                      |                                                      | 38.82%                                  | ð Þ h ð i ; p ; 6 Þ                     | 48.34%                                  | 34.05%                                  | 24.5%                                   | 42.92%                                  | 38.82%                                  | 53.43%                                  |
| 42.92% 53.43% p 9 ¼ {DC2, DC5, DC6, DC4, [DC1, DC3]} | 42.92% 53.43% p 9 ¼ {DC2, DC5, DC6, DC4, [DC1, DC3]} | 42.92% 53.43% p 9 ¼ {DC2, DC5, DC6, DC4, [DC1, DC3]} | 42.92% 53.43% p 9 ¼ {DC2, DC5, DC6, DC4, [DC1, DC3]} | 42.92% 53.43% p 9 ¼ {DC2, DC5, DC6, DC4, [DC1, DC3]} | 42.92% 53.43% p 9 ¼ {DC2, DC5, DC6, DC4, [DC1, DC3]} |                                         | p 10 ¼ {DC2, DC5, DC4, DC6, [DC1, DC3]} | p 10 ¼ {DC2, DC5, DC4, DC6, [DC1, DC3]} | p 10 ¼ {DC2, DC5, DC4, DC6, [DC1, DC3]} | p 10 ¼ {DC2, DC5, DC4, DC6, [DC1, DC3]} | p 10 ¼ {DC2, DC5, DC4, DC6, [DC1, DC3]} | p 10 ¼ {DC2, DC5, DC4, DC6, [DC1, DC3]} | p 10 ¼ {DC2, DC5, DC4, DC6, [DC1, DC3]} |
| Participant i                                        | DC2                                                  | DC5                                                  | DC6                                                  | DC4                                                  | DC1                                                  | DC3                                     | Participant i                           | DC2                                     | DC5                                     | DC4                                     | DC6                                     | DC1                                     | DC3                                     |
| h i ; p ; 1                                          | 8.00%                                                |                                                      |                                                      |                                                      |                                                      |                                         | h i ; p ; 1                             | 8.00%                                   |                                         |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 2                                      | 28.67%                                               | 22.30%                                               |                                                      |                                                      |                                                      |                                         | ð Þ h i ; p ; 2                         | 28.67%                                  | 22.30%                                  |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 3                                      | 40.25%                                               | 33.20%                                               | 14.69%                                               |                                                      |                                                      |                                         | ð Þ h i ; p ; 3                         | 39.29%                                  | 32.30%                                  | 28.13%                                  |                                         |                                         |                                         |
| ð Þ                                                  |                                                      |                                                      | 16.50%                                               | 30.85%                                               |                                                      |                                         | ð Þ h i ; p ; 4                         | 42.15%                                  | 35.00%                                  | 30.85%                                  | 16.50%                                  |                                         |                                         |
| h ð i ; p ; 4 Þ h ð i ; p ; 6 Þ                      | 42.15% 53.43%                                        | 35.00% 42.92%                                        | 24.5%                                                | 38.82%                                               | 48.34%                                               | 34.05%                                  | ð Þ h ð i ; p ; 6 Þ                     | 53.43%                                  | 42.92%                                  | 38.82%                                  | 24.5%                                   | 48.34%                                  | 34.05%                                  |
| p 11 ¼ {DC2, DC6, DC5, DC4, [DC1, DC3]}              | p 11 ¼ {DC2, DC6, DC5, DC4, [DC1, DC3]}              | p 11 ¼ {DC2, DC6, DC5, DC4, [DC1, DC3]}              | p 11 ¼ {DC2, DC6, DC5, DC4, [DC1, DC3]}              | p 11 ¼ {DC2, DC6, DC5, DC4, [DC1, DC3]}              | p 11 ¼ {DC2, DC6, DC5, DC4, [DC1, DC3]}              |                                         | p 12 ¼ {DC4, DC5, DC2, DC6, [DC1, DC3]} | p 12 ¼ {DC4, DC5, DC2, DC6, [DC1, DC3]} | p 12 ¼ {DC4, DC5, DC2, DC6, [DC1, DC3]} | p 12 ¼ {DC4, DC5, DC2, DC6, [DC1, DC3]} | p 12 ¼ {DC4, DC5, DC2, DC6, [DC1, DC3]} | p 12 ¼ {DC4, DC5, DC2, DC6, [DC1, DC3]} | p 12 ¼ {DC4, DC5, DC2, DC6, [DC1, DC3]} |
| Participant i                                        | DC2                                                  | DC6                                                  | DC5                                                  | DC4                                                  | DC1                                                  | DC3                                     | Participant i                           | DC4                                     | DC5                                     | DC2                                     | DC6                                     | DC1                                     | DC3                                     |
| h i ; p ; 1                                          | 8.00%                                                |                                                      |                                                      |                                                      |                                                      |                                         | h i ; p ; 1                             | 5.00%                                   |                                         |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 2                                      | 39.50%                                               | 13.98%                                               |                                                      |                                                      |                                                      |                                         | ð Þ h ð i ; p ; 2 Þ                     | 23.22%                                  | 27.42%                                  |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 3                                      | 40.25%                                               | 14.69%                                               | 33.20%                                               |                                                      |                                                      |                                         | h i ; p ; 3                             | 28.13%                                  | 32.30%                                  | 39.29%                                  |                                         |                                         |                                         |
| ð Þ h i ; p ; 4                                      | 42.15%                                               | 16.50%                                               | 35.00%                                               | 30.85%                                               |                                                      |                                         | ð Þ h ð i ; p ; 4 Þ                     | 30.85%                                  | 35.00%                                  | 42.15%                                  | 16.50%                                  |                                         |                                         |
| ð Þ h ð i ; p ; 6 Þ                                  | 53.43%                                               | 24.5%                                                | 42.92%                                               | 38.82%                                               | 48.34%                                               | 34.05%                                  | h ð i ; p ; 6 Þ                         | 38.82%                                  | 42.92%                                  | 53.43%                                  | 24.5%                                   | 48.34%                                  | 34.05%                                  |
| p 13 ¼ {DC5, DC6, DC2, DC4, [DC1, DC3]}              | p 13 ¼ {DC5, DC6, DC2, DC4, [DC1, DC3]}              | p 13 ¼ {DC5, DC6, DC2, DC4, [DC1, DC3]}              | p 13 ¼ {DC5, DC6, DC2, DC4, [DC1, DC3]}              | p 13 ¼ {DC5, DC6, DC2, DC4, [DC1, DC3]}              | p 13 ¼ {DC5, DC6, DC2, DC4, [DC1, DC3]}              |                                         | p 14 ¼ {DC2, DC4, DC5, DC6, [DC1, DC3]} | p 14 ¼ {DC2, DC4, DC5, DC6, [DC1, DC3]} | p 14 ¼ {DC2, DC4, DC5, DC6, [DC1, DC3]} | p 14 ¼ {DC2, DC4, DC5, DC6, [DC1, DC3]} | p 14 ¼ {DC2, DC4, DC5, DC6, [DC1, DC3]} | p 14 ¼ {DC2, DC4, DC5, DC6, [DC1, DC3]} | p 14 ¼ {DC2, DC4, DC5, DC6, [DC1, DC3]} |
| Participant i                                        | DC5                                                  | DC6                                                  | DC2                                                  | DC4                                                  | DC1                                                  | DC3                                     | Participant i                           | DC2                                     | DC4                                     | DC5                                     | DC6                                     | DC1                                     | DC3                                     |
| h i ; p ; 1                                          | 5.00%                                                |                                                      |                                                      |                                                      |                                                      |                                         | i ; p ; 1                               | 5.00%                                   |                                         |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 2                                      | 22.17%                                               | 3.55%                                                |                                                      |                                                      |                                                      |                                         | h ð Þ h i ; p ; 2                       | 37.61%                                  | 26.54%                                  |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 3                                      | 33.20%                                               | 14.69%                                               | 42.15%                                               |                                                      |                                                      |                                         | ð Þ i p 3                               |                                         |                                         | 32.30%                                  |                                         |                                         |                                         |
| ð Þ                                                  |                                                      |                                                      |                                                      |                                                      |                                                      |                                         | h ð ; ; Þ                               | 39.29%                                  | 28.13%                                  |                                         |                                         |                                         |                                         |
| h ð i ; p ; 4 Þ h ð i ; p ; 6 Þ                      | 35.00% 42.92%                                        | 16.50% 24.5%                                         | 42.15% 53.43%                                        | 30.85% 38.82%                                        | 48.34%                                               | 34.05%                                  | h ð i ; p ; 4 Þ h ð i ; p ; 6 Þ         | 42.15% 53.43%                           | 30.85% 38.82%                           | 35.00% 42.92%                           | 16.50% 24.5%                            | 48.34%                                  | 34.05%                                  |
| 15 ¼ {DC4, DC2, DC5, DC6, [DC1, DC3]}                | 15 ¼ {DC4, DC2, DC5, DC6, [DC1, DC3]}                | 15 ¼ {DC4, DC2, DC5, DC6, [DC1, DC3]}                | 15 ¼ {DC4, DC2, DC5, DC6, [DC1, DC3]}                | 15 ¼ {DC4, DC2, DC5, DC6, [DC1, DC3]}                | 15 ¼ {DC4, DC2, DC5, DC6, [DC1, DC3]}                |                                         |                                         |                                         |                                         |                                         |                                         |                                         |                                         |
| Participant i                                        | DC4                                                  | DC2                                                  | DC5                                                  | DC6                                                  | DC1                                                  | DC3                                     |                                         |                                         |                                         |                                         |                                         |                                         |                                         |
|                                                      | 5.00%                                                |                                                      |                                                      |                                                      |                                                      |                                         |                                         |                                         |                                         |                                         |                                         |                                         |                                         |
| h ð i ; p ; 1 Þ h i ; p ; 2                          |                                                      | 37.61%                                               |                                                      |                                                      |                                                      |                                         |                                         |                                         |                                         |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 3                                      | 26.54%                                               | 39.29%                                               | 32.30%                                               |                                                      |                                                      |                                         |                                         |                                         |                                         |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 4                                      | 28.13%                                               |                                                      |                                                      | 16.50%                                               |                                                      |                                         |                                         |                                         |                                         |                                         |                                         |                                         |                                         |
| ð Þ h i ; p ; 6                                      | 30.85% 38.82%                                        | 42.15% 53.43%                                        | 35.00% 42.92%                                        | 24.5%                                                | 48.34%                                               | 34.05%                                  |                                         |                                         |                                         |                                         |                                         |                                         |                                         |

Table 13 Optimal collaborative sequence based on SMP principle.

| Participant i   | p 1 ¼ {DC1, DC3, DC4, DC2, DC5,DC6}   | p 1 ¼ {DC1, DC3, DC4, DC2, DC5,DC6}   | p 1 ¼ {DC1, DC3, DC4, DC2, DC5,DC6}   | p 1 ¼ {DC1, DC3, DC4, DC2, DC5,DC6}   | p 1 ¼ {DC1, DC3, DC4, DC2, DC5,DC6}   | p 1 ¼ {DC1, DC3, DC4, DC2, DC5,DC6}   |
|-----------------|---------------------------------------|---------------------------------------|---------------------------------------|---------------------------------------|---------------------------------------|---------------------------------------|
|                 | DC1                                   | DC3                                   | DC4                                   | DC2                                   | DC5                                   | DC6                                   |
| h ð i ; p ; 2 Þ | 32.69%                                | 17.68%                                |                                       |                                       |                                       |                                       |
| h ð i ; p ; 3 Þ | 40.13%                                | 24.12%                                | 32.01%                                |                                       |                                       |                                       |
| h ð i ; p ; 4 Þ | 42.17%                                | 25.89%                                | 33.70%                                | 45.17%                                |                                       |                                       |
| h ð i ; p ; 5 Þ | 48.06%                                | 29.49%                                | 37.15%                                | 48.81%                                | 41.26%                                |                                       |
| h ð i ; p ; 6 Þ | 48.34%                                | 34.05%                                | 38.82%                                | 53.43%                                | 42.92%                                | 24.50%                                |

integrated methodology including DP method, clustering algorithm and Im-NSGAII is proposed at the second stage to solve the biobjective optimization problem. The DP method is used to calculate the transportation cost of trucks in the /uniFB01 rst echelon. The clustering algorithm is employed to /uniFB01 nd the initial routes to simplify the computation. Im-NSGAII is presented to optimize the routes of delivery vehicles traveling from DCs to customers. At the third stage, a collaborative coalition strategy based on the CGA

Table 14

Comparison before and after the CTMCVRP-SST optimization with a four-bin truck.

| Cases                           |   Transportation cost ($) Penalty cost ($) |   Transportation cost ($) Penalty cost ($) |     |   Maintenance cost ($) Other |   cost ($) Total cost |   Number of vehicles |   Number of trucks |
|---------------------------------|--------------------------------------------|--------------------------------------------|-----|------------------------------|-----------------------|----------------------|--------------------|
| Before CTMCVRP-SST optimization |                                      37339 |                                      22403 | 846 |                        18670 |                 79258 |                   24 |                  4 |
| After CTMCVRP-SST optimization  |                                      25471 |                                      10188 | 596 |                        14688 |                 50943 |                   18 |                  2 |

Fig. 17.

Shortest path of six-bin truck during service period 1 in SST network.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000018_71c3b2b4def4c340361b785549c6d3d6579f17ff3a477d02f0ea09166b502d8c.png)

model is presented and the optimal pro /uniFB01 t allocation strategy in the collaborative SST logistics network is obtained. SMP theory is utilized for optimal coalition sequence selection, considering the different cost reduction percentages in different coalition sequences.

An empirical study is conducted on an SST logistics network in Chongqing, China, to test the collaborative mechanism and CTMCVRP-SST in real life. The changes in the total cost and the number of delivery vehicles from before to after implementing the collaborative mechanism are $29905 and 18, respectively. A comparison among the Im-NSGAII, ACO and MOPSO algorithms indicates that Im-NSGAII performs best among the three in terms of solution quality. Given the selection of the proper pro /uniFB01 t allocation strategies, the CGA approach can achieve better network-level performance than the Shapley value model, MCRS and EPM. An analysis of using four-, six- and eight-bin trucks for between-DC distribution shows that four-bin trucks can be recommended for use under the current logistics demands.

This study aims to investigate the CTMCVRP-SST based on an

Fig. 18. Shortest path of eight-bin truck during service period 1 in SST network.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000019_1a9a910b81ba9d5891a243cb97b61486ea6a8bef8a42426afec2e6eb7e9914f4.png)

Table 15

Comparison before and after the CTMCVRP-SST optimization among four-, six- and eight-bin truck.

| Load space   |   Transportation cost ($) |   Penalty cost ($) |   Maintenance cost ($) |   Other cost ($) |   Total cost ($) |   Number of vehicles |   Number of trucks |
|--------------|---------------------------|--------------------|------------------------|------------------|------------------|----------------------|--------------------|
| Four-bin     |                     25471 |              10188 |                    596 |            14688 |            50943 |                   18 |                  2 |
| Six-bin      |                     26538 |              10257 |                    611 |            14899 |            52305 |                   18 |                  2 |
| Eight-bin    |                     27466 |              10799 |                    615 |            15119 |            53999 |                   18 |                  2 |

Table 16

Pro /uniFB01 t allocation in accordance with CGA, MCRS, Shapley, EPM, and Core.

| Participants   |   CGA model |   MCRS |   Shapley value model |   EPM |   Core |
|----------------|-------------|--------|-----------------------|-------|--------|
| DC1            |        5220 |   4816 |                  5443 |   666 |   5256 |
| DC2            |        6587 |   7087 |                  6300 |  7168 |   6037 |
| DC3            |        4247 |   5706 |                  4421 |  5228 |   4236 |
| DC4            |        5051 |   4223 |                  5026 |  6504 |   5127 |
| DC5            |        5622 |   4315 |                  4600 |  5557 |   5566 |
| DC6            |        3178 |   3758 |                  4115 |  4782 |   3683 |

MCRS: (4816,7086,5706,4223,4315,3758)

Fig. 19. Core center and distance comparison diagram.

![Image](Papers_Converted/0_Artifacts/[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation_artifacts/image_000020_e3dac61558c8867902d9da223dd48842501c91b668e5e3a9baacd7a79073617a.png)

integrated optimization and collaboration framework, which existing research has not addressed. Future work can be conducted in the following four directions: (1) considering pickups during delivery activities based on resource sharing in a two-echelon SST logistics network, (2) considering the connection between traf /uniFB01 c congestion and vehicle departure time in the SST logistics network, (3) considering the collaborative mechanisms established for multiple service periods in multi-echelon logistics networks, and (4) considering the effect of the heterogeneity and stability of different coalitions on an on-demand logistics distribution network.

## Declaration of competing interest

The authors declare that they have no known competing /uniFB01 nancial interests or personal relationships that could have appeared to in /uniFB02 uence the work reported in this paper.

## CRediT authorship contribution statement

Yong Wang: Resources, Methodology, Conceptualization, Formal analysis, Investigation, Data curation, Visualization, Writing -original draft, Writing - review &amp; editing, Supervision, Project administration, Funding acquisition. Yingying Yuan: Resources, Software, Validation, Formal analysis, Investigation, Data curation, Writing - original draft, Writing - review &amp; editing. Xiangyang Guan: Resources, Methodology, Conceptualization, Formal analysis,

Investigation, Data curation, Writing - original draft, Writing - review &amp; editing. Maozeng Xu: Resources, Visualization, Supervision. Li Wang: Visualization. Haizhong Wang: Visualization. Yong Liu: Validation, Supervision.

## Acknowledgments

The authors would like to express our sincere appreciation for the valuable comments made by three anonymous reviewers, which helped us to improve the quality of this paper. This research is supported by National Natural Science Foundation of China (Project No. 71871035, 71971036), Humanity and Social Science Youth Foundation of Ministry of Education of China (18YJC630189), Science and Technology Research Project of ' Chongqing Municipal Education Commission (KJQN201800723), National Social Science Foundation of Chongqing of China (No. 2019YBGL054), and Postdoctoral Science Foundation (Project No. 2017T100692 and 2016M600735), 2017 Postdoctoral Science Foundation of Sichuan Province, China. This research is supported by 2018 Chongqing Liuchuang Plan Innovation Project (cx2018111), and Program for the Philosophy and Social Science Research of Higher Learning Institutions of Shanxi (201803066), and Major Program of Key Disciplines in Dalian (2019J11CY002).

## References

- Afshar-Nadja /uniFB01 , B., Afshar-Nadja /uniFB01 , A., 2017. A constructive heuristic for timedependent multi-depot vehicle routing problem with time-windows and heterogeneous /uniFB02 eet. J. King Saud Univ. Eng. Sci. 29 (1), 29 e 34.
- Bae, H., Moon, I., 2016. Multi-depot vehicle routing problem with time windows considering delivery and installation vehicles. Appl. Math. Model. 40 (13 e 14), 6536 e 6549.
- Baldacci, R., Bartolini, E., Mingozzi, A., 2011. An exact algorithm for the pickup and delivery problem with time windows. Oper. Res. 59 (2), 414 e 426.
- Bandyopadhyay, S., Bhattacharya, R., 2014. Solving a tri-objective supply chain problem with modi /uniFB01 ed NSGA-II algorithm. J. Manuf. Syst. 33 (1), 41 e 50.
- Belgin, O., Karaoglan, I., Altiparmak, F., 2018. Two-echelon vehicle routing problem with simultaneous pickup and delivery: mathematical model and heuristic approach. Comput. Ind. Eng. 115, 1 e 16.
- Boland, N., Hewitt, M., Marshall, L., Savelsbergh, M., 2017. The continuous-time service network design problem. Oper. Res. 65 (5), 1303 e 1321.
- Boland, N.L., Savelsbergh, M.W.P., 2019. Perspectives on integer programming for time-dependent models. Top 27 (2), 147 e 173.
- Breunig, U., Schmid, V., Hartl, R.F., Vidal, T., 2016. A large neighbourhood based heuristic for two-echelon routing problems. Comput. Oper. Res. 76, 208 e 225.
- Calvet, L., Ferrer, A., Gomes, M.I., Juan, A.A., Masip, D., 2016. Combining statistical learning with metaheuristics for the multi-depot vehicle routing problem with market segmentation. Comput. Ind. Eng. 94, 93 e 104.
- Chan, F.T.S., Jha, A., Tiwari, M.K., 2016. Bi-objective optimization of three echelon supply chain involving truck selection and loading using NSGA-II with heuristics algorithm. Appl. Soft Comput. 38, 978 e 987.
- Commerce Department (CD), 2018. China E-commerce annual development report. Retrieved from. https://new.qq.com/omn/20190618/20190618A0SCAJ.html?pc. March 14, 2018.
- Contardo, C., Martinelli, R., 2014. A new exact algorithm for the multi-depot vehicle routing problem under capacity and route length constraints. Discrete Optim. 12 (1), 129 e 146.
- Cruijssen, F., Borm, P., Fleuren, H., Hamers, H., 2010. Supplier-initiated outsourcing: a methodology to exploit synergy in transportation. Eur. J. Oper. Res. 207 (2), 763 e 774.
- Dai, B., Chen, H.X., 2012. Pro /uniFB01 t allocation mechanisms for carrier collaboration in pickup and delivery service. Comput. Ind. Eng. 62 (2), 633 e 643.
- Deb, K., Goyal, M., 1996. A combined genetic adaptive search (GeneAS) for engineering design. Comput. Sci. Inf. 26, 30 e 45.
- Deb, K., Pratap, A., Agarwal, S., Meyarivan, T., 2002. A fast and elitist multiobjective genetic algorithm: NSGA-II. IEEE Trans. Evol. Comput. 6 (2), 182 e 197.
- Defryn, C., Sorensen, K., 2017. A fast two-level variable neighborhood search for the € clustered vehicle routing problem. Comput. Oper. Res. 83, 78 e 94.
- Deng, Y.R., Zheng, Y., Li, J.P., 2019. Route optimization model in collaborative logistics network for mixed transportation problem considered cost discount based on GATS. J. Ambient Intell. Humanized Comput. 10 (1), 409 e 416.
- Du, J.M., Li, X., Yu, L., Dan, R., Zhou, J.D., 2017. Multi-depot vehicle routing problem for hazardous materials transportation: a fuzzy bilevel programming. Inf. Sci. 399, 201 e 218.
- Feng, F., Pang, Y.S., Lodewijks, G., Li, W.F., 2017. Collaborative framework of an intelligent agent system for ef /uniFB01 cient logistics transport planning. Comput. Ind. Eng. 112, 551 e 567.
- Frisk, M., Gothe-Lundgren, M., Jornsten, K., Ronnqvistab, M., 2010. Cost allocation in € € € collaborative forest transportation. Eur. J. Oper. Res. 205 (2), 448 e 458.
- Ganguly, S., Sahoo, N.C., Das, D., 2013. Multi-objective planning of electrical distribution systems using dynamic programming. Int. J. Electr. Power Energy Syst. 46, 65 e 78.
- Gansterer, M., Hartl, R.F., Salzmann, P.E.H., 2018. Exact solutions for the collaborative pickup and delivery problem. Cent. Eur. J. Oper. Res. 26 (2), 357 e 371.
- Guedes, P.C., Borenstein, D., 2018. Real-time multi-depot vehicle type rescheduling problem. Transp. Res. Part B Methodol. 108, 217 e 234.
- Guimaraes, T.A., Coelho, L.C., Schenekemberg, C.M., Scarpina, C.T., 2019. The two-~ echelon multi-depot inventory-routing problem. Comput. Oper. Res. 101, 220 e 233.
- Hafezalkotob, A., Makui, A., 2015. Cooperative maximum/uniFB02 ow problem under uncertainty in logistic networks. Appl. Math. Comput. 250, 593 e 604.
- Ho, G.T.S., Ip, W.H., Lee, C.K.M., Mou, W.L., 2012. Customer grouping for better resources allocation using GA based clustering technique. Expert Syst. Appl. 39 (2), 1979 e 1987.
- Ho, W., Ho, G.T.S., Ji, P., Lau, H.C.W., 2008. A hybrid genetic algorithm for the multidepot vehicle routing problem. Eng. Appl. Artif. Intell. 21 (4), 548 e 557.
- Irie, H., Wongpaisarnsin, G., Terabe, M., Miki, A., Taguchi, S., 2019. Quantum annealing of vehicle routing problem with time, state and capacity. Lect. Notes Comput. Sci. 11413, 145 e 156.
- Kuo, R.J., Mei, C.H., Zulvia, F.E., Tsai, C.Y., 2016. An application of a metaheuristic algorithm-based clustering ensemble method to app customer segmentation. Neurocomputing 205 (C), 116 e 129.
- Kumar, R., Izui, K., Yoshimura, M., Nishiwaki, S., 2009. Multi-objective hierarchical genetic algorithms for multilevel redundancy allocation optimization. Reliab. Eng. Syst. Saf. 94 (4), 891 e 904.
- Kumoi, Y., Matsubayashi, N., 2014. Vertical integration with endogenous contract leadership: stability and fair pro /uniFB01 t allocation. Eur. J. Oper. Res. 238 (1), 221 e 232.
- Li, H.Q., Liu, Y.Y., Jian, X.R., Lu, Y.R., 2018a. The two-echelon distribution system considering the real-time transshipment capacity varying. Transp. Res. Part B Methodol. 110, 239 e 260.
- Li, J., Li, Y., Pardalos, P.M., 2016. Multi-depot vehicle routing problem with time windows under shared depot resources. J. Combin. Optim. 31 (2), 515 e 532.
- Li, J., Wang, R., Li, T.T., Lu, Z.X., Pardalos, P.M., 2018b. Bene /uniFB01 t analysis of shared depot resources for multi-depot vehicle routing problem with fuel consumption. Transport. Res. Transport Environ. 59, 417 e 432.
- Li, Y.B., Soleimani, H., Zohal, M., 2019. An improved ant colony optimization algorithm for the multi-depot green vehicle routing problem with multiple objectives. J. Clean. Prod. 227, 1161 e 1172.
- Lin, Y.H., Huang, L.C., Chen, S.Y., Yu, C.M., 2018. The optimal route planning for inspection task of autonomous underwater vehicle composed of MOPSO-based dynamic routing algorithm in currents. Appl. Ocean Res. 75, 178 e 192.
- Liu, J.T., Zhou, X.S., 2016. Capacitated transit service network design with boundedly rational agents. Transp. Res. Part B Methodol. 93, 225 e 250.
- Liu, S.S., Papageorgiou, L.G., 2018. Fair pro /uniFB01 t distribution in multi-echelon supply chains via transfer prices. Omega: Int. J. Manag. Sci. 80, 77 e 94.
- Lozano, S., Moreno, P., Adenso-Díaz, B., Algaba, E., 2013. Cooperative game theory approach to allocating bene /uniFB01 ts of horizontal cooperation. Eur. J. Oper. Res. 229 (2), 444 e 452.
- Lu, G.Y., Zhou, X.S., Mahmoudi, M., Shi, T., Peng, Q.Y., 2019. Optimizing resource recharging location-routing plans: a resource-space-time network modeling framework for railway locomotive refueling applications. Comput. Ind. Eng. 127, 1241 e 1258.
- Mahmoudi, M., Zhou, X.S., 2016. Finding optimal solutions for vehicle routing problem with pickup and delivery services with time windows: a dynamic programming approach based on state-space-time network representations. Transp. Res. Part B Methodol. 89, 19 e 42.
- Mahmoudi, M., Chen, H.H., Shi, T., Zhang, Y.X., Zhou, X.S., 2019. A cumulative service state representation for the pickup and delivery problem with transfers. Transp. Res. Part B Methodol. 129, 351 e 380.
- Marinelli, M., Colovic, A., Dell Orco, ' M., 2018. A novel dynamic programming approach for two-echelon capacitated vehicle routing problem in city logistics with environmental considerations. Transport. Res. Procedia 30, 147 e 156.
- Molenbruch, Y., Braekers, K., Caris, A., 2017. Bene /uniFB01 ts of horizontal cooperation in dial-a-ride services. Transport. Res. E Logist. Transport. Rev. 107, 97 e 119.
- Narasimha, K.V., Kivelevitch, E., Sharma, B., Kumar, M., 2013. An ant colony optimization technique for solving min-max multi-depot vehicle routing problem. Swarm Evol. Comput. 13, 63 e 73.
- Nordin, N., Selke, S., 2010. Social aspect of sustainable packaging. Packag. Technol. Sci. 23 (6), 317 e 326.
- Oliveira, F.B.D., Enayatifar, R., Sadaei, H.J., Guimaraes, ~ F.G., Potvin, J., 2016. A cooperative coevolutionary algorithm for the multi-depot vehicle routing problem. Expert Syst. Appl. 43, 117 e 130.
- Quintero-Araujo, C.L., Gruler, A., Juan, A.A., Faulin, J., 2019. Using horizontal cooperation concepts in integrated routing and facility-location decisions. Int. Trans. Oper. Res. 26 (2), 551 e 576.
- Rabbani, M., Farrokhi-Asl, H., Asgarian, B., 2017. Solving a bi-objective location routing problem by a NSGA-II combined with clustering approach: application in waste collection problem. J. Ind. Eng. Int. 13 (1), 13 e 27.
- Sazonov, V.V., Skobelev, P.O., Lada, A.N., Mayorov, I.V., 2018. Application of multiagent technologies to multiple depot vehicle routing problem with time windows. Autom. Rem. Contr. 79 (6), 1139 e 1147.
- Shang, P., Li, R.M., Guo, J.F., Xian, K., Zhou, X.S., 2019. Integrating Lagrangian and

- Eulerian observations for passenger /uniFB02 ow state estimation in an urban rail transit network: a space-time-state hyper network-based assignment approach. Transp. Res. Part B Methodol. 121, 135 e 167.
- Sheu, J.B., Lin, Y.S., 2012. Hierarchical facility network planning model for global logistics network con /uniFB01 gurations. Appl. Math. Model. 36 (7), 3053 e 3066.
- Soysal, M., Bloemhof-Ruwaard, J.M., Bektas ¸ , T., 2015. The time-dependent twoechelon capacitated vehicle routing problem with environmental considerations. Int. J. Prod. Econ. 164, 366 e 378.
- Soysal, M., Çimenb, M., 2017. A simulation based restricted dynamic programming approach for the green time dependent vehicle routing problem. Comput. Oper. Res. 88, 297 e 305.
- Sun, W., Yu, Y., Wang, J.W., 2019. Heterogeneous vehicle pickup and delivery problems: formulation and exact solution. Transport. Res. E Logist. Transport. Rev. 125, 181 e 202.

Sundar, K., Rathinam, S., 2016. Generalized multiple depot traveling salesmen problem-polyhedral study and exact algorithm. Comput. Oper. Res. 70, 39 e 55.

- Tang, X., Lehuede, Fabien, Peton, Olivier, Pan, L., 2019. Network design of a multi/C19 /C19 /C19 period collaborative distribution system. Int. J. Mach. Learn. Cybern. 10 (2), 279 e 290.
- Tang, J.F., Yu, Y., Li, J., 2015. An exact algorithm for the multi-trip vehicle routing and scheduling problem of pickup and delivery of customers to the airport. Transport. Res. E Logist. Transport. Rev. 73, 114 e 132.
- Tong, L., Zhou, X.S., Miller, H.J., 2015. Transportation network design for maximizing space e time accessibility. Transp. Res. Part B Methodol. 81, 555 e 576.
- Tu, W., Fang, Z.X., Li, Q.Q., Shaw, S.L., Chen, B.Y., 2014. A bi-level voronoi diagrambased metaheuristic for a large-scale multi-depot vehicle routing problem. Transport. Res. E Logist. Transport. Rev. 61, 84 e 97.
- Vaziri, S., Etebari, F., Vahdani, B., 2019. Development and optimization of a horizontal carrier collaboration vehicle routing model with multi-commodity request allocation. J. Clean. Prod. 224, 492 e 505.
- Wang, L., Yang, L.X., Gao, Z.Y., 2016. The constrained shortest path problem with stochastic correlated link travel times. Eur. J. Oper. Res. 255, 43 e 57.
- Wang, J.H., Weng, T.Y., Zhang, Q.F., 2019a. A two-stage multiobjective evolutionary algorithm for multiobjective multidepot vehicle routing problem with time windows. IEEE Trans. Cybern. 49 (7), 2467 e 2478.
- Wang, J.W., Yu, Y., Tang, J.F., 2018a. Compensation and pro /uniFB01 t distribution for cooperative green pickup and delivery problem. Transp. Res. Part B Methodol. 113, 54 e 69.
- Wang, S.J., Wang, X.D., Liu, X., Yu, J.B., 2018b. A bi-objective vehicle-routing problem with soft time windows and multiple depots to minimize the total energy consumption and customer dissatisfaction. Sustainability 10, 4257.
- Wang, Y., Assogba, K., Fan, J.X., Xu, M.Z., Liu, Y., Wang, H.Z., 2019b. Multi-depot green vehicle routing problem with shared transportation resource: integration of time-dependent speed and piecewise penalty cost. J. Clean. Prod. 232, 12 e 29.
- Wang, Y., Ma, X.L., Lao, Y.T., Wang, Y.H., 2014. A fuzzy-based customer clustering approach with hierarchical structure for logistics network optimization. Expert Syst. Appl. 41, 521 e 534.
- Wang, Y., Ma, X.L., Li, Z., Liu, Y., Xu, M., Wang, Y., 2017a. Pro /uniFB01 t distribution in
- collaborative multiple centers vehicle routing problem. J. Clean. Prod. 144, 203 e 219.
- Wang, Y., Ma, X.L., Liu, M., Gong, K., Liu, Y., Xu, M., 2017b. Cooperation and pro /uniFB01 t allocation in two-echelon logistics joint distribution network optimization. Appl. Soft Comput. 56, 143 e 157.
- Wang, Y., Ma, X.L., Xu, M.Z., Liu, Y., Wang, Y.H., 2015. Two-echelon logistics distribution region partitioning problem based on a hybrid particle swarm optimization-genetic algorithm. Expert Syst. Appl. 42 (12), 5019 e 5031.
- Wang, Y., Peng, S.G., X, C.C., Assogba, K., Wang, H.Z., X, M.Z., Wang, Y.H., 2018c. Twoechelon logistics delivery and pickup network optimization based on integrated cooperation and transportation /uniFB02 eet sharing. Expert Syst. Appl. 113, 44 e 65.
- Wang, Y., Zhang, S.L., Assogba, K., Fan, J.X., Xu, M.Z., Wang, Y.H., 2018d. Economic and environmental evaluations in the two-echelon collaborative multiple centers vehicle routing optimization. J. Clean. Prod. 197, 443 e 461.
- Wang, Y., Zhang, S.L., Guan, X.Y., Peng, S.G., Wang, H.Z., Liu, Y., Xu, M.Z., 2020. Collaborative multi-depot logistics network design with time window assignment. Expert Syst. Appl. 140. Article 112910.
- Wang, Z., 2018. Delivering meals for multiple suppliers: exclusive or sharing logistics service. Transport. Res. E Logist. Transport. Rev. 118, 496 e 512.
- Wu, Q., Ren, H.B., Gao, W.J., Ren, J.X., Lao, C.S., 2017. Pro /uniFB01 t allocation analysis among the distributed energy network participants based on game-theory. Energy 118, 783 e 794.
- Xu, X.M., Li, C.L., Xu, Z., 2018. Integrated train timetabling and locomotive assignment. Transp. Res. Part B Methodol. 117, 573 e 593.
- Yang, L.X., Li, S.K., Gao, Y., Gao, Z.Y., 2015. A coordinated routing model with optimized velocity for train scheduling on a single-track railway line. Int. J. Intell. Syst. 30 (1), 3 e 22.
- Yang, L.X., Zhou, X.S., 2017. Optimizing on-time arrival probability and percentile travel time for elementary path /uniFB01 nding in time-dependent transportation networks: linear mixed integer programming reformulations. Transp. Res. Part B Methodol. 96, 68 e 91.
- Yücenur, G.N., Demirel, N.Ç., 2011. A new geometric shape-based genetic clustering algorithm for the multi-depot vehicle routing problem. Expert Syst. Appl. 38 (9), 11859 e 11865.
- Yu, Y., Lou, Q., Tang, J.F., Wang, J.W., Yue, X.H., 2017. An exact decomposition method to save trips in cooperative pickup and delivery based on scheduled trips and pro /uniFB01 t distribution. Comput. Oper. Res. 87, 245 e 257.
- Yuan, Y., Tang, L.X., 2017. Novel time-space network /uniFB02 ow formulation and approximate dynamic programming approach for the crane scheduling in a coil warehouse. Eur. J. Oper. Res. 262 (2), 424 e 437.
- Zhang, D.F., Cai, S.F., Ye, F.R., Si, Y.W., Nguyen, T.T., 2017. A hybrid algorithm for a vehicle routing problem with realistic constraints. Inf. Sci. 394 e 395, 167 e 182.
- Zamar, D.S., Gopaluni, B., Sokhansanj, S., 2017. A constrained K-means and nearest neighbor approach for route optimization in the bale collection problem. IFACPapersOnLine 50 (1), 12125 e 12130.
- Zhou, L., Baldacci, R., Vigo, D., Wang, X., 2017. A multi-depot two-echelon vehicle routing problem with delivery options arising in the last mile distribution. Eur. J. Oper. Res. 265 (2), 765 e 778.