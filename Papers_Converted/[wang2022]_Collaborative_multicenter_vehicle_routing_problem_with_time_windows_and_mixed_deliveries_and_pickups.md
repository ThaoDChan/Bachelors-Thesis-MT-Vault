Contents lists available at ScienceDirect

## Expert Systems With Applications

journal homepage: www.elsevier.com/locate/eswa

GLYPH&lt;0&gt;GLYPH&lt;20&gt;GLYPH&lt;0&gt;GLYPH&lt;28&gt;GLYPH&lt;0&gt;GLYPH&lt;26&gt;

GLYPH&lt;0&gt;℘GLYPH&lt;0&gt;GLYPH&lt;21&gt;GLYPH&lt;0&gt;GLYPH&lt;19&gt;GLYPH&lt;0&gt;GLYPH&lt;21&gt;GLYPH&lt;0&gt;GLYPH&lt;21&gt;GLYPH&lt;0&gt;2

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000000_d2897a5ec4962a85eeb33bbc6f69b499a96ef9e680d413ae96a6959537512223.png)

## Collaborative multicenter vehicle routing problem with time windows and mixed deliveries and pickups

Yong Wang a,* , Lingyu Ran a , Xiangyang Guan b,* , Jianxin Fan c,* , Yaoyao Sun a , Haizhong Wang d

- a School of Economics and Management, Chongqing Jiaotong University, Chongqing 400074, China
- b
- Department of Civil and Environmental Engineering, University of Washington, Seattle, WA 98195, USA
- c School of River and Ocean Engineering, Chongqing Jiaotong University, Chongqing 400074, China
- d School of Civil and Construction Engineering, Oregon State University, Corvallis, OR 97330, USA

## A R T I C L E  I N F O

## A B S T R A C T

## Keywords:

Vehicle routing problem with time windows and mixed deliveries and pickups Space time distance -Transportation resource sharing 3D k -means clustering algorithm Collaborative alliance

This  study  focuses  on  the  collaborative  multicenter  vehicle  routing  problem  with  time  windows  and  mixed deliveries and pickups (CMVRPTWMDP), which is a variant of the vehicle routing problem (VRP) with mixed deliveries and pickups, and VRP with simultaneous deliveries and pickups and time windows. Collaboration and transportation resource sharing are adopted to optimize vehicle routes in the CMVRPTWMDP, to integrate the delivery and pickup services with time windows, and to construct open -closed mixed vehicle routes. First, the CMVRPTWMDP is formulated as a mixed-integer programming model to minimize logistics operating costs. The effect of transportation resource sharing on reducing the number of needed vehicles and the maintenance cost is considered in the model formulation. Second, a two-stage hybrid algorithm combining customer clustering and vehicle routing optimization is designed to solve the CMVRPTWMDP. An improved 3D k -means clustering algorithm based on space -time distances and the customer demand is proposed to reassign customers to logistics facilities (e.g., delivery or pickup centers). Furthermore, a hybrid heuristic algorithm that combines the genetic algorithm (GA) and particle swarm optimization (PSO) algorithm, called GA -PSO, is designed to optimize the vehicle routes. A coordination operator between GA and PSO is designed to allow particles and chromosomes to interact, increasing the diversity of particle swarms and the possibility of finding a feasible solution. Third, the performance and effectiveness of the proposed approach are tested by comparing them with the CPLEX solver using 30 small-scale instances and other existing algorithms using 25 benchmark instances. Fourth, the minimum costs-remaining savings (MCRS) model is adopted to design a fair and reasonable profit allocation scheme for participants in the collaborative alliance and maintain alliance stability. Finally, the optimization results of a real-world case study from Chongqing, China, show that transportation resource misuse and logistics operating costs  are  significantly  reduced,  demonstrating  the  proposed  approach s  effectiveness  and  applicability.  This ' study provides insights for logistics enterprises and  transportation departments on effectively allocating and utilizing the transportation resources and optimizing the local logistics network.

## 1. Introduction

Reverse logistics (RL) is often combined with forward logistics to reduce the impact on the environment and minimize logistics operating costs (Govindan et al., 2016; Dutta et al., 2020). In previous studies, the vehicle  routing  problem  (VRP)  with  mixed  deliveries  and  pickups (VRPMDP) and the VRP with simultaneous deliveries and pickups and time  windows  (VRPSDPTW)  are  the  two  extensions  of  VRP  with backhaul in RL (Wang et al., 2015a; Shi et al., 2020). In logistics operations,  customers  can  be  divided  into  three  types  according  to  their demands: customers with only delivery demands, customers with only pickup  demands,  and  customers  with  both  delivery  and  pickup  demands. These three types of customers often simultaneously exist  in realistic logistics networks. Examples include the distribution of medical supplies and the recycling of medical waste at the same time, the distribution  of  bottled  beverages,  and  the  recycling  of  empty  bottles

* Corresponding authors.

GLYPH&lt;0&gt;GLYPH&lt;20&gt;GLYPH&lt;0&gt;GLYPH&lt;20&gt;GLYPH&lt;0&gt;GLYPH&lt;25&gt;GLYPH&lt;0&gt;GLYPH&lt;25&gt;GLYPH&lt;0&gt;GLYPH&lt;28&gt;GLYPH&lt;0&gt;GLYPH&lt;19&gt;

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000001_6dd40ec835d9c2df608754a6c426ba40dd71f76e4c3e2d3b5d019a284b50c6c1.png)

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000002_0b48e150c1b98dcdb41689e41befb6d42f680229f141d90db6afeb8b0abbcef6.png)

Y. Wang et al.

(Sellitto, 2018; Yu et al., 2020). In those cases, transportation resource sharing and collaboration are essential means to integrate the delivery and pickup services with time windows of vehicles (DPSTWV) and increase resource utilization (Marques et al., 2020; Wang et al., 2021). Therefore,  a  collaborative  multicenter  VRP  with  time  windows  and mixed  deliveries  and  pickups  (CMVRPTWMDP)  is  proposed  in  this study, which accounts for the three types of customers, collaboration, and resource sharing.

(Li et al., 2019; Liu et al., 2021a). Therefore, collaboration and profit allocation mechanisms are incorporated in optimizing CMVRPTWMDP.

A collaborative multicenter logistics network includes multiple logistics facilities [e.g., delivery centers (DCs) and pickup centers (PCs)] and a large number of customers. Compared with conventional depots, DCs and PCs have more versatile functions, either serving third-party logistics deliveries and pickups or being dedicated to self-owned logistics deliveries and pickups (Liu and Jiang, 2012). Vehicles owned by the third party logistics enterprises start distribution from the depot and do not necessarily return (i.e., open VRPs). On the contrary, personal logistics enterprises could be considered the standard VRP. However, a challenge in solving the CMVRPTWMDP is how to design the DPSTWV to meet customer demands (Liu et al., 2013; Qiu et al., 2018b; Wang et al., 2018c). If vehicles are restricted to provide only delivery or pickup services,  then  customers of the third type are visited multiple times, which inevitably generates additional costs and lowers customer satisfaction (Qiu et al., 2018a). Moreover, if facilities share vehicles, then the design of the origin and destination of vehicles plays a vital role in the coordination of DPSTWV (Goksal et al., 2013; Wang et al., 2021). In those cases, vehicle routes in the network may contain both closed and open routes, which are called open -closed mixed vehicle routes (OCMR) (Azadeh and Farrokhi-Asl, 2019). Thus, the coordination of DPSTWV can  be  combined  with  an  OCMR  design  to  solve  the  proposed CMVRPTWMDP, which advances the research of VRP with time windows and mixed deliveries and pickups (VRPTWMDP), and promotes the application of multicenter VRPTWMDP (MVRPTWMDP).

Collaboration  is  an  important  factor  in  optimizing  the  vehicle schedules  in  a  multicenter  logistics  network,  which  supports  sharing transportation resources, customer information, and facility capabilities (Verdonck et al., 2013; Angelopoulos et al., 2018; Szeto and Shui, 2018; Wang et al., 2018b). However, the formation and stability of a collaborative alliance (i.e., a group of logistics facilities that agree to collaborate)  are  easily  affected  by  the  profit  allocation  schemes  among  its participants (Gansterer et al., 2020). Accordingly, the analysis of the optimal  collaborative alliance structure (i.e., the composition  of collaborative alliance that benefits all participants), the optimal sequence  of  joining  collaborative  alliances,  and  the  profit  allocation scheme  are  an  indispensable  part  of  solving  CMVRPTWMDP.  Profit allocation mechanisms are often used to allocate the profits generated by collaboration, which serve as management tools for logistics facilities in deciding whether to collaborate and maintain the alliance s stability '

Table 1

The abbreviations of the research related to CMVRPTWMDP.

| Abbreviation of problem studied   | Type of problem                                                                                      | Abbreviation of problem studied   | Type of problem                                                   |
|-----------------------------------|------------------------------------------------------------------------------------------------------|-----------------------------------|-------------------------------------------------------------------|
| VRP                               | Vehicle routing problem                                                                              | MDVRPTW                           | Multidepot vehicle routing problem with time windows              |
| VRPMDP                            | Vehicle routing problem with mixed deliveries and pickups                                            | VRPPDTW                           | Vehicle routing problem with pickup and delivery and time windows |
| VRPSDPTW                          | Vehicle routing problem with simultaneous deliveries and pickups and time windows                    | GA-PSO                            | Genetic algorithm and particle swarm optimization algorithm       |
| DPSTWV                            | Delivery and pickup services with time windows of vehicles                                           | GA-TS                             | Genetic algorithm and tabu search                                 |
| CMVRPTWMDP                        | Collaborative multicenter vehicle routing problem with time windows and mixed deliveries and pickups | MOPSO                             | Multi-objective particle swarm optimization algorithm             |
| OCMR                              | Open-closed mixed vehicle routes                                                                     | IDKCA                             | Improved 3D k-means clustering algorithm                          |
| MVRPTWMDP                         | Multicenter vehicle routing problem with time windows and mixed deliveries and pickups               | MCRS                              | Minimum costs-remaining savings                                   |
| OVRP                              | Open vehicle routing problem                                                                         | SMP                               | Strictly monotonic path                                           |
| OCMVRP                            | Open-closed mixed vehicle routing problem                                                            |                                   |                                                                   |

In  this  work,  the  CMVRPTWMDP  incorporates  the  OCMR  and DPSTWV and can be formulated as a mixed-integer programming model to minimize logistics operating costs. In order to address the computational challenges, an  improved  3D k -means  clustering  algorithm (IDKCA) based on coordinates, time windows, and service demands of customers is proposed. All customers are simultaneously clustered with DCs and PCs as cluster centers. Specifically, customers in the intersection of multiple clusters are identified with a corresponding DC or PC after  clustering.  The  customers ' delivery  and  pickup  demands  are collaboratively  served  by  these  two  logistics  facilities.  In  addition,  a hybrid heuristic algorithm named GA-PSO that combines the genetic algorithm  (GA)  and  particle  swarm  optimization  algorithm  (PSO)  is designed to find the optimized vehicle routes in CMVRPTWMDP. The profits generated by collaboration are allocated based on the minimum costs-remaining savings (MCRS) method. The results of profit allocation are further analyzed to find the optimal alliance sequence and profit allocation scheme that maintain the stability of collaborative alliance.

The remaining sections of this paper are organized as follows. Section 2 summarizes related studies. Section 3 conceptually explains the CMVRPTWMDP  is.  Section  4  introduces  the  model  formulation  of CMVRPTWMDP. Section 5 demonstrates the methods used to solve the CMVRPTWMDP. Section 6 describes the analytical results of comparing instances and a real-word case and related management insights. Section 7 summarizes the conclusions of this study.

## 2. Literature review

Considering that the rapid growth of e-commerce has caused changes in the scale of logistics networks and customer demands, improving the utilization of transportation resources by collaborating to optimize the logistics network has been regarded as a vital issue (Long, 2016b; Liu et al., 2021c; Wang et al., 2021). The classical VRPMDP and VRPSDP, which coordinate the DPSTWV, have been widely studied (Wang et al., 2015a; Shi et al., 2020). Accordingly, the MVRPTWMDP can also be optimized by coordinating the DPSTWV (Liu et al., 2021b; Agra et al., 2021).  Table  1  lists  the  abbreviations  of  the  research  related  to  the CMVRPTWMDP.  In  this  study,  the  OCMR  is  combined  with  the DPSTWV, which increases the complexity of solving MVRPTWMDP and makes the CMVRPTWMDP practical. However, the origin and destination selection of vehicles means that the logistics facilities where the vehicles  depart  and  arrive  have  collaborated  (Brito  et  al.,  2015). Therefore, the integration of DPSTWV, the OCMR, collaboration, and resource  sharing  should  be  considered  as  a  whole  to  address  the CMVRPTWMDP.

As  a  fundamental  component  problem  element  of  the  proposed

Y. Wang et al.

CMVRPTWMDP, the VRPSDP has been studied by a few scholars and regarded as a special case of VRPMDP (Avci and Topaloglu, 2015). Liu et al. (2013) proposed a GA and Tabu Search (TS) hybrid algorithm to solve  the  two  established  mixed-integer  programming  models  for VRPSDP. Ashouri and Yousefikhoshbakht (2017) integrated the scanning algorithm and the local search algorithm into the ant colony algorithm to solve VRPSDP and the open vehicle routing problem (OVRP). Scholars further studied VRPSDP with time windows (VRPSDPTW) by considering the customers ' service time windows (Wang et al., 2015a; Shi  et  al.,  2020).  Moreover,  Sartori  and  Buriol  (2020)  studied  the VRPSDPTW  in  transportation  services  and  the  efficiency  of  their improved  metaheuristic  was  identified  by  providing  a  comparative analysis  of  the  existing  hybrid  algorithm.  By  focusing  on  solving VRPSDPTW optimization, Aziez et al. (2020) introduced two new formulations, improved an existing 3-index formulation, and solved the VRPSDPTW  via  a  branch-and-cut  algorithm.  Wang  et  al.  (2020c) formulated a multi-objective optimization model for a multidepot and multiperiod  VRPPDTW and designed a hybrid heuristic algorithm to solve it by considering the open-close mixed vehicle routes. In OVRP and open closed mixed VRP (OCMVRP), vehicles are flexibly used, which -improves the utilization of vehicles (Liu and Jiang, 2012). Some scholars have optimized VRPMDP through constructing the OCMR and considering resource sharing, thereby achieving superior results (Goksal et al., 2013; Qiu et al., 2018b). Brito et al. (2016) adopted variable neighborhood search to study the OCMVRP with time windows. Azadeh and Farrokhi-Asl  (2019)  studied  the  multidepot  vehicle  routing  problem (MDVRP) and the OCMVRP and proposed a new hybrid metaheuristic algorithm.

The MDVRPTW is a classical variant of MDVRP, and heuristic algorithms combining customer assignments and vehicle routing optimization are usually adopted to solve the large-scale MDVRPTW (Sever et al., 2018;  Wang et  al.,  2020a). K -means clustering  algorithm  and  fuzzybased clustering are two common customer assignment methods that allocate customers based on customers ' coordinates and time windows (Nagy and Salhi, 2005; Van Anholt et al., 2016; Rabbani et al., 2017). Wang et al. (2015) established a general mixed-integer programming model for the VRPSDPTW and designed a parallel simulated annealing algorithm including a residual capacity and radial surcharge insertionbased heuristic to obtain a feasible solution. Yanik et al. (2014) constructed a model for VRP with pickup and delivery and time windows (VRPPDTW) that considered multiple vendors and found a solution a hybrid GA and Clarke -Wright savings algorithm. Ahkamiraad and Wang (2018)  formulated  a  mixed-integer  linear  programming  model  for VRPPDTW and proposed a hybrid algorithm with GA and PSO.

Collaboration is regarded as an effective strategy to share resources (Gansterer et al., 2020; Yang et al., 2020). Customers, facility capability, and resources are shared among multiple facilities in a collaborative logistics network (Wang et al., 2017). In previous studies, collaboration has been widely adopted to optimize the logistics network. Fernandez ´ et al. (2018) proposed a new strategy to share customer services in a collaborative MDVRP and further analyzed the potential profits. The sharing strategies of vehicle, customer information, and facility capability have been combined by Wang et al. (2018c) to integrate transportation  resources  and  coordinate  the  delivery  and  pickup  services while solving a multicenter VRPSDPTW. Quintero-Araujo et al. (2019) studied the multidepot integrated routing and facility-location decisions and analyzed the effect of collaboration on the monetary and environmental  aspects.  Collaboration  among  multiple  facilities  has  been regarded  as  an  effective  strategy  widely  adopted  by  scholars  when addressing logistics network optimization (Ho and Szeto, 2017; Araghi et al., 2021).

Some profit allocation mechanisms in the concept of collaborative game theory have been widely adopted in optimizing the collaborative logistics  (Krajewska et al.,  2008; Wang et al., 2018c), such as Game Quadratic Programming (GQP), MCRS (Liu and Sui, 2016), Proportional Least Core (Sadegh and Kerachian, 2011), Shapley, Equal Profit Method

(Audy et al., 2011) and Nucleolus (Maschler et al., 1979). The core area based on the  snowball ' ' theory helps find the optimal profit allocation schemes,  which  are  related  to  other  profit  allocation  mechanisms (Shapley, 1971; Lozano et al., 2013). In addition, a selection strategy named  strictly  monotonic  path  (SMP)  is  used  to  find  the  optimal sequence of participants joining the collaboration (Blundell et al., 2014).

The previous research has provided rich references for the formulation, analysis, and solution of CMVRPTWMDP. However, the research related to CMVRPTWMDP  has  the  following  limitations. (1) A CMVRPTWMDP combining the collaborative multicenter VRPMDP and VRPSDPTW with various customer demands has not been sufficiently studied. (2) The OCMVRP and DPSTWV are often separately analyzed and  studied,  and  collaboration  and  transportation  resource  sharing based on open -closed mixed vehicle routes have not been significantly examined  on  the  resource  configuration  and  collaborative  logistics networks. (3) A mathematical formulation with the transshipped cargo requirements and transportation resource sharing in the CMVRPTWMDP is lacking. (4) The existing intelligent algorithms for solving MVRP and VRPMDP have limited applicability for solving the CMVRPTWMDP.

Compared with previous studies, the main contributions of this study are  as  follows.  (1)  A  collaborative  multicenter  logistics  network  is designed  to  serve  three  customer  demands  (i.e.,  delivery  demands, pickup  demands,  delivery  and  pickup  demands)  with  time  windows based on the open -closed mixed vehicle routes among same or different types of centers (i.e., DCs, PCs, DCs and PCs) to obtain the optimized logistics operating costs in comparison to the independent multicenter operation, which  advances  the  fields  of study  in VRPMDP  and VRPSDPTW. (2) Collaboration and transportation resource sharing are adopted  to  optimize  heterogeneous  vehicle  scheduling,  including semitrailer truck and open -closed mixed vehicle routes among multiple centers, and thereby reduce the number of vehicles, improve delivery and pickup service efficiencies, and optimize resource configuration. (3) A  mixed-integer  programming  model  with  transportation  resource sharing based on open-closed mixed vehicle routes and transshipped cargo  requirements  are  established  to  minimize  the  total  logistics operating costs in the CMVRPTWMDP. (4) A GA -PSO hybrid algorithm combined with improved 3D  -means clustering is proposed to solve the k CMVRPTWMDP model, find the optimized logistics operating costs and determine the open -closed mixed delivery and pickup vehicle routes in the collaborative multicenter logistics network.

## 3. Problem statement

In this study, the logistics network of interest contains multiple DCs, multiple  PCs  and  three  types  of  customers  (i.e.,  delivery  demands, pickup demands, and delivery and pickup demands) with time windows. Contrary to the conventional multidepot operation, third party and selfowned logistics delivery and pickup services usually exist in multiple centers (e.g., DCs, PCs). Furthermore, the DCs and PCs have more logistics  functions  (e.g.,  sorting  and  assembling)  and  serve  various customer  demands.  Furthermore,  collaboration  and  transportation resource  sharing  can  be  effectively  implemented  based  on  the  open--closed  mixed  vehicle  routes  among  the  same  or  different  types  of centers (i.e., DCs, PCs, DCS and PCs) for the CMVRPTWMDP. Each DC and  PC  separately  handle  customers ' delivery  and  pickup  demands before collaboration. The logistics network before and after optimization is shown in Fig. 1. The time window of each node (square bracket near the node) and the travel time of each road segment (near the arrow line) are assumed known.

In Fig. 1(a), each facility independently operates; thus, centralized transportation does not occur between facilities. Some phenomena that increase logistics operating costs are frequently observed in the nonoptimized  network.  First,  two  different  logistics  facilities  serve  each customer s delivery and pickup demands, meaning customers with both ' delivery and pickup demands are visited twice. Customers who have

Y. Wang et al.

Fig. 1. Illustration of the CMVRPTWMDP optimization.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000003_9fd3233a85820144eabe3912804fdda082ba197099e91f1d417c2bad0d20b3a4.png)

delivery and pickup demands often want to meet their demands on a single visit. Second, vehicles frequently arrive at customers beyond the requested time windows, which may cause customer dissatisfaction and affect the long-term profits of the logistics facilities. Third, each vehicle must return to its origin, even if the last customer visited is far away from its origin.

Moreover, the rental costs of vehicles and semitrailer trucks are $60 and $100, and the CC and FC of each facility are $50 and $200, respectively, and significant decreases in the TC, WT, and Nv can be achieved through resource sharing and collaboration. The TC after collaboration ($2380) is significantly  lower  than  without  collaboration  ($3780),  demonstrating that collaboration optimizes the logistics network.

In Fig. 1(b), long-distance transportation and violation of customer time windows are avoided in the optimized network. The merged and transshipped cargoes can be transported among DCs or PCs by a fleet of semitrailer trucks, and then semitrailer truck routes are generated. In addition, each DC and PC can serve the adjacent customers by a fleet of vehicles, and the demands of each customer are met on a single visit. The collaboration among facilities allows vehicles to be used by various logistics facilities. Specifically, vehicles that provide delivery services can move to the nearest DC or PC (open routes); the vehicles that perform delivery and pickup services or provide only pickup services must return to  the  nearest  PC  (closed  routes).  In  Fig.  1(b),  the  closed  and  open vehicle route numbers are four and seven, respectively. This study assumes that cargoes (and associated customer demands) can be transferred  between  facilities  by  semitrailer  trucks;  however,  no  transfer occurs between DCs and PCs. The gaps in the delivery and pickup time (DPT),  transportation  time  (TRT),  waiting  time  (WT),  government incentive  (GI),  delivery  and  pickup  costs  (DPC),  transportation  cost (TRC),  penalty  cost  (PCO),  collaborative  cost  (CC),  fixed  cost  (FC), numbers of vehicles (Nv) and semitrailer trucks (NT), lease cost of vehicles (VLC) and semitrailer trucks (TLC), and total cost (TC) of the logistics network in the noncollaborative and collaborative scenarios are shown in Table 2.

In Table 2 14 indicators of the logistics network performance in the noncollaborative  and  collaborative  scenarios  are  listed  in  Table  2. Assuming that the travel costs of vehicles and semitrailer trucks are $10 and $15 per unit time, respectively, the penalty cost for arriving early or late is $30 per unit time, the GI for each collaborating facility is $180.

## 4. Related definitions and model formulation

## 4.1. Model formulation

The logistics network in this study is composed of DCs, PCs, and customers with mixed delivery and pickup demands. The customer demands are served by vehicles, and the transfer volume between facilities is centralized by semitrailer trucks. Some reasonable assumptions are proposed to make the CMVRPTWMDP model more specific (Wassan and Nagy, 2014). Those assumptions are formulated as follows:

- · Assumption  1.  Customer  demands  and  service  time  windows  are known and relatively stable within a work period.
- · Assumption 2. The service time of each vehicle and semitrailer truck is zero.
- · Assumption 3. The centralized transportation among DCs and PCs is performed by a semitrailer truck.
- · Assumption 4. Each facility provides homogeneous vehicles, that is, the vehicle capacity of each facility is the same. In addition, homogeneous semitrailer trucks transfer the transportation requirements among  multiple  logistics  facilities  after  forming  a  collaborative alliance.

The related notations and definitions of the mathematical model are shown in Table 3. Readers interested in symbols and constraints can refer to the studies of Liu et al. (2013), Tu et al. (2014), Fernandez et al. ´

Table 2 Comparison between the noncollaborative and collaborative logistics networks.

| Scenario          |   DPT |   TRT |   WT |   Nv |   NT |   DPC($) |   TRC($) |   PCO($) |   GI($) |   CC($) |   FC($) |   VLC($) |   TLC($) |   TC($) | Gap of TC ($)   |
|-------------------|-------|-------|------|------|------|----------|----------|----------|---------|---------|---------|----------|----------|---------|-----------------|
| Non-Collaboration |   178 |     0 |   12 |   14 |    0 |     1780 |        0 |      360 |       0 |       0 |     800 |      840 |        0 |    3780 | 1400            |
| Collaboration     |   106 |    12 |    0 |   11 |    2 |     1060 |      180 |        0 |     720 |     200 |     800 |      660 |      200 |    2380 |                 |

GLYPH&lt;0&gt;↔GLYPH&lt;0&gt;𝒮GLYPH&lt;0&gt;𝒮GLYPH&lt;0&gt;ℴGLYPH&lt;0&gt;ℒ

Y. Wang et al.

Table 3 Notations and definitions in CMVRPTWMDP.

| Set          | Description                                                                                                                                                    |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| D            | Set of DCs , D = { d | d = 1,2,3, … , N d }, and N d is the total number of DCs                                                                                |
| P            | Set of PCs , P = { p|p = 1,2,3, … , N p }, and N p is the total number of PCs                                                                                  |
| F            | Set of DCs and PCs , F = P ∪ D                                                                                                                                 |
| I            | Set of customers with delivery demands , I = { i | i = 1,2,3, … , N i } and N i is the total number of this type of customers                                  |
| J            | Set of customers with pickup demands , J = { j | j = 1,2,3, … , N j } and N j is the total number of this type of customers                                    |
| S            | Set of customers with pickup and delivery demands , S = { s | s = 1,2,3, … , N s } and N s is the total number of this type of customers                       |
| U            | Set of all customers , U = I ∪ J ∪ S                                                                                                                           |
| V            | Set of vehicles , V = { v | v = 1,2,3, … , Nv }, and Nv is the total number of vehicles                                                                        |
| K            | Set of semitrailer trucks , K = { k | k = 1,2,3, … , N k }, and N k is the total number of semitrailer trucks                                                  |
| Parameters   |                                                                                                                                                                |
| Q h          | Delivery demand of customer h, h ∈ I ∪ S                                                                                                                       |
| G h          | Pickup demand of customer h, h ∈ J ∪ S                                                                                                                         |
| [ e h ,l h ] | Service time window of customer h, h ∈ U                                                                                                                       |
| [ e f ,l f ] | Service time window of DC or PC f, f ∈ F                                                                                                                       |
| d rh         | Travel distance between customer or facility r and customer h. r, h ∈ U ∪ F                                                                                    |
| d wf         | Travel distance between facilities w and f. f, w ∈ F                                                                                                           |
| t rh         | Travel time of a vehicle driving from facility or customer r to customer h. r, h ∈ U ∪ F                                                                       |
| ∂ e          | Waiting penalty coefficient of arriving early (unit: dollars/unit time)                                                                                        |
| ∂ l          | Tardiness penalty coefficient of arriving late (unit: dollars/unit time)                                                                                       |
| T            | Number of working periods in a year                                                                                                                            |
| T v          | Maximum travel time of vehicle v, v ∈ V                                                                                                                        |
| T k          | Maximum travel time of semitrailer truck k, k ∈ K                                                                                                              |
| f v          | Fuel consumption rate of vehicle v per km (unit: gallon/km)                                                                                                    |
| f k          | Fuel consumption rate of semitrailer truck k per km (unit: gallon/km)                                                                                          |
| P v          | Gasoline price (unit: dollars/gallon)                                                                                                                          |
| P k          | Diesel price (unit: dollars/gallon)                                                                                                                            |
| C f          | Capacity of DC or PC f, f ∈ F                                                                                                                                  |
| C v          | Capacity of vehicle v, v ∈ V                                                                                                                                   |
| C k          | Capacity of semitrailer truck k, k ∈ K                                                                                                                         |
| M v          | Annual maintenance cost of vehicle v, v ∈ V                                                                                                                    |
| M k          | Annual maintenance cost of semitrailer truck k, k ∈ K                                                                                                          |
| G f          | Government incentives if DC or PC f agrees to collaborate , f ∈ F                                                                                              |
| CC f         | Cooperation costs if DC or PC f agrees to collaborate, f ∈ F                                                                                                   |
| I f          | Fixed cost of facility f within one working period , f ∈ F.                                                                                                    |
| tm of        | Transportation quantity from DC o to f , o, f ∈ D                                                                                                              |
| tm fo        | Transportation quantity from PC f to o , o, f ∈ P                                                                                                              |
| Tm o         | Total transportation quantity from DC or PC o , o ∈ D                                                                                                          |
| nd vrh       | Remaining quantity of goods for delivery from customer r to customer h for vehicle v , v ∈ V, r, h ∈ U                                                         |
| np vrh dt    | Remaining quantity of goods for pickup from customer r to customer h for vehicle v , v ∈ V, r, h ∈ U Departure time of vehicle v from DC or PC f, v ∈ V, f ∈ F |
| vf at vh     | Arrival time of vehicle v at customer h , v ∈ V, h ∈ U                                                                                                         |
| LB           | Lower bound for the primal problem                                                                                                                             |
| UB           | Upper bound for the primal problem                                                                                                                             |
| RT           | Running times in seconds                                                                                                                                       |
| GAP_AL       | The gap percentage of the average objective function from LB                                                                                                   |
| GAP_UL       | value The gap percentage between UB and LB with respect to UB                                                                                                  |
| Variables    |                                                                                                                                                                |
| x vhf        | x vhf = 1 if customer h is served by vehicle v departing from DC or PC f ; otherwise, x vhf = 0, v ∈ V, h ∈ U, f ∈ F                                           |
| x vrh        | x vrh = 1 if vehicle v travels directly from facility or customer r to h ; otherwise, x vrh = 0, v ∈ V, r,h ∈ U ∪ F                                            |
| y hof        | y hof = 1 if the service relation of customer h changes from DC o to f or PC o to f ; otherwise, y hof = 0, h ∈ U, o,f ∈ F                                     |
| x kowf       | x kowf = 1 if semitrailer truck k departs from facility o and travels from facility w to f ; otherwise, x kowf = 0, k ∈ K, o, f, w ∈ F                         |
| x kof        | x kof = 1 if DC f or PC f served by semitrailer truck k departs from DC o or PC o ; otherwise, x kof = 0 , k ∈ K, o,f ∈ F.                                     |
| z f          | z f = 1 if DC f or PC f agrees to collaborate; otherwise, z f = 0, f ∈ F.                                                                                      |

## (2018), and Wang et al. (2018b).

min TC TCV TCK TCF

$$\min T C \, = \, T C _ { V } + T C _ { K } + T C _ { F }$$

## 4.2. Mathematical model

The  proposed  CMVRPTWMDP  is  formulated  as  a  mixed-integer mathematical model. The objective function TC presented in Eq.  (1) aims  to  minimize  the  total  operating  cost  within  a  working  period, which is composed of the cost of all vehicles, semitrailer trucks, and facilities.

Furthermore, TCV denotes  the  total  operating  cost  of  all  vehicles within  a  working  period,  which  is  composed  of  the  transportation, maintenance, and penalty costs. Mv T is the maintenance cost of vehicle v in one period. ∂ e × max {( eh GLYPH&lt;0&gt; atvh ) , 0 }+ ∂ l × max {( atvh GLYPH&lt;0&gt; l h ) , 0 } represents the penalty cost when the arrival time of vehicle v is  not  within the customer service window.

Y. Wang et al.

$$\tau _ { \nu } & = \sum _ { \infty } \left ( \sum _ { \infty \infty } x _ { \infty \infty } \times f _ { \tau } \times d _ { \infty \infty \infty } + \sum _ { \infty \infty \infty \infty } x _ { \infty \infty \infty \infty } \times f _ { \tau } \times d _ { \infty } \right ) + \sum _ { \infty \infty \infty \infty \infty \infty } \sum _ { \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \infty \outline } ^ { M _ { \tau } } \times x _ { \infty \infty }$$

TCk denotes the total cost of all semitrailer trucks within one working period,  which  are  composed  of  the  transportation  and  maintenance costs.

∑

Tmo

=

tmof

,

∀

o

∈

D

∪

P

(18)

f

∈

D P

∪

$$\tau _ { \kappa } = \sum _ { \mu } f _ { \ell }, P _ { \kappa } \left ( \sum _ { \mu } \sum _ { \mu } x _ { \mu \nu } d _ { \nu \nu } + \sum _ { \mu } \sum _ { \mu \nu } x _ { \mu \nu } d _ { \nu \nu } \right ) + \frac { M _ { \ell } } { T } \left ( \min _ { \infty } \left ( \left [ \frac { T m _ { \nu } } { C _ { \ell } } \right | \right ) \right )$$

$$T C _ { \kappa } = \sum _ { k \in \mathcal { K } } f _ { k } \cdot P _ { k } \cdot \left ( \sum _ { o \in P } \sum _ { w \in P } x _ { k o w f } d _ { w f } + \sum _ { o \in D } \sum _ { w \in D } x _ { k o w f } \cdot d _ { w f } \right ) + \frac { M _ { k } } { T } \cdot \left ( \max _ { o \in F } \left ( \left [ \frac { T m _ { o } } { C _ { k } } \right ] \right ) \right )$$

TCF denotes the remaining fixed costs of all facilities, which is offset by government incentives.

$$\text{is offset} \quad \sum _ { v \in V h \in J \colon s } x _ { v h f } \times Q _ { h } + \sum _ { o \in D h \in J \colon s } y _ { h o f } \times Q _ { h } \leqslant C _ { f }, \forall f \in D$$

$$\vec { \L } _ { T C _ { F } } = \sum _ { f \in F } ( I _ { f } + C C _ { f } - z _ { f } \cdot G _ { f } )$$

Subject to

$$\sum _ { v \in V } \overset { \kappa } { \sum _ { f \in F } } x _ { v h f } = 1, \forall h \in U$$

∑∑

xvrh

=

1

,

∀

v

∈

V

r

∈

F h

∈

U

$$\sum _ { h \in U } x _ { v r h } - \sum _ { h \in U } x _ { v h r } = 0, \forall v \in V, r \in U$$

∑

∑

xvrh

h

∈

U

∑

=

xvhn

h

∈

U

xkowf

=

F v

,

∈

xkofw

,

V

∀

k

∈

K

,

∀

o

∈

D

w

=

o f

,

∈

D

\{

o

}

w

=

o f

,

∈

D

\{

o

}

$$\sum _ { w = o, f \in P \langle o \rangle } x _ { k o w f } = \sum _ { w = o, f \in P \langle o \rangle } x _ { k o f w }, \forall k \in K, \forall o \in P$$

$$\sum _ { w \in P } x _ { k o w f } - \sum _ { w \in P } x _ { k o f w } = 0, \forall k \in K, o, f \in P$$

∑

∑

xkowf

GLYPH&lt;0&gt;

xkowf

=

0

,

∀

k

∈

K o f

,

,

∈

D

∈

w D

w D

∈

ndvrh + npvrh + xvrh ×( Gh GLYPH&lt;0&gt; Qh ) ⩽ Cv , ∀ v ∈ V r , ∈ U h , ∈ I ∪ J

∑

xkof

⋅

tmof

⩽

Ck

,

∀

k

∈

K o

,

∈

D

f

∈

D

$$\sum _ { f \in P } x _ { k o f } \cdot t m _ { o f } \leqslant C _ { k }, \forall k \in K, o \in P$$

$$\ t m _ { o f } = \sum _ { h \in I \cup S } y _ { h o f } \times Q _ { h }, \forall o, f \in D$$

$$\ t m _ { f o } = \sum _ { h \in J \cup S } y _ { h o f } \times G _ { h }, \forall o, f \in P$$

,

∀

r

,

n

∈

∑

$$\sum _ { v \in V \, h \in J \, \colon S } x _ { i h f } \times G _ { h } + \sum _ { o \in P \, h \in J \, \colon S } y _ { h a f } \times G _ { h } \leqslant C _ { f }, \forall f \in P$$

$$e _ { f } \leqslant d t _ { v f } \leqslant l _ { f }, \forall v \in V, f \in F$$

$$e _ { h } \times x _ { i v h } \leqslant ( a _ { i v r } + t _ { h } ) \times x _ { i v h } \leqslant l _ { h } \times x _ { i v h }, \forall v \in V, r \in U, h \in U \cup F \quad \quad ( 2 2 )$$

∑

xvrh

×

,

∀

∈

(6)

t

rh

⩽

Tv

v

V

(23)

,

r h

∈ ∪

F

U

$$\sum _ { r, h \in U, r \neq h } x _ { v h } \leq \sum _ { h \in U } x _ { v h f } - 1, \forall v \in V, f \in F$$

$$\sum _ { f, w \in D \langle \{ o \} } x _ { k o n f } \leqslant \sum _ { f \in D \langle \{ o \} } x _ { k o f } - 1, \forall k \in K, o \in D$$

$$\sum _ { f, w \in P \langle \{ o \} } x _ { k o n f } \leqslant \sum _ { f \in P \langle \{ o \} } x _ { k o f } - 1, \forall k \in K, o \in P$$

$$x _ { i f } = \{ 0, 1 \}, \forall v \in V, h \in U, f \in F$$

$$x _ { \nu h } = \{ 0, 1 \}, \forall v \in V, r, h \in U \cup F$$

$$y _ { h \neq f } = \{ 0, 1 \}, \forall h \in U, o, f \in F$$

$$x _ { k o n f } = \{ 0, 1 \}, \forall k \in K, o, w, f \in F$$

xkof

= {

0 1

,

} ∀

,

k

∈

K o f

,

,

∈

F

(31)

$$\mu \cup J \quad \ ( 1 3 ) \quad \ z _ { \gamma } = \{ 0, 1 \}, \forall f \in F$$

(14) (15) (16) (17) Constraint  (5)  ensures  that  every  customer  is  visited  just  once. Constraint (6) denotes that each vehicle departs from one facility and provides service to the customer. Constraints (7) -(8) ensure the flow balance of every vehicle. Constraints (9) -(12) assure that the semitrailer truck should leave the facility after it has completed the arranged service to this facility and return to its origin. Constraints (13) -(15) guarantee that vehicles and semitrailer trucks cannot exceed their loading capacity on their service routes. Additionally, constraints (16) -(18) calculate the amount of goods transported from facility o to f and the total amount of goods transported from facility o. Constraints (19) -(20) are restrictions on the capabilities of each facility. Constraints (21) -(22) are restrictions on the departure and arrival times of vehicles. Constraint (23) ensures

Y. Wang et al.

Fig. 2. Algorithm flowchart of CMVRPTWMDP.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000004_a0fdd5a2905ea395ad8a3baa7130f662763c37bfe7edd996ad2273b55acb24c0.png)

that the total travel time of each vehicle during service does not exceed their maximum travel time. Constraints (24) -(26) are used to eliminate the sub-tours of all vehicles and semitrailer trucks. Lastly, constraints (27) -(32) indicate that the decision variables used in our mathematical are binary.

## 5. Solution methodology

Cluster-first  and  route  second  heuristic  have  been  widely  used  to solve VRP and its variants (Miranda-Bront et al., 2017; Brandao, 2020; ˜ Sadati et al., 2021; Wang et al., 2021). In addition, hybrid algorithms can combine the advantages of multiple algorithms to handle MDVRP with high efficiency (Nagy and Salhi, 2005; Ray et al., 2014; Tu et al., 2014;  Wang  et  al.,  2017).  A  two-stage  hybrid  algorithm  including customer clustering and vehicle route optimization is designed to optimize the CMVRPTWMDP considering the two characteristics of multicenter and customer demands (Sever et al., 2018; Hintsch and Irnich, 2020; Wang et al., 2020a). First, an improved 3D k -means clustering algorithm is designed to cluster customers according to their coordinates and time windows. The second stage named GA -PSO, is mainly used to obtain  the  optimized  vehicle  routes  in  which  the  GA  and  PSO  are adopted to improve the algorithm performance (Goksal et al., 2013; Liu et al., 2013; Wang et al., 2015a; Bernardino and Paias, 2018).

The procedure of the proposed two-stage hybrid algorithm is illustrated in Fig. 2. In IDKCA, the two main criteria for performing clustering are the space -time distance and the demand type of customers. In GA PSO, the update formula  of  inertia  weight  and  the  coordination -operator between GA and PSO are the primary operations to improve the

Y. Wang et al.

Fig. 3. Clustering diagram of IDKCA.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000005_4c46bff886f18c3c9586f5d441c8d901dc1a32283a3329dbd3a8fc653ea4c5f4.png)

Fig. 4. Illustration of the chromosome, coding, and decoding of CMVRPTWMDP.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000006_def3d090a0edb296dde7fd34e5a7d59a7f9b5baedf8db4cb68ce44a6d86bf5c2.png)

Y. Wang et al.

quality of solutions. In addition, the MCRS and SMP selection strategies are adopted to allocate the profits generated by collaboration and find the optimal sequence for each member participating in the collaborative alliance. The parameters involved in Fig. 2 are defined as follows: gen is the current number of iterations, Maxgen denotes the maximum number of iterations, r1 is the current number of internal reoptimization runs between  IDKCA  and  GA -PSO, r2 is  the  current  number  of  internal reoptimization runs in the coordination operator between GA and PSO, R1 and R2 signify the corresponding maximum number of r1 and r2 , G is the number of chromosomes (particles), and GG is the number of worstfit chromosomes.

providing an opportunity to adapt to the IDKCA. In the first stage of IDKCA, customers are clustered with DCs and PCs, respectively. Fig. 3(b) demonstrates the clustering results of customers grouped with DCs as cluster centers; Fig. 3(c) shows the results of customers clustered with PCs as cluster centers. In the second stage of IDKCA, the purpose is to combine the clustering results of the first stage to obtain new clusters, which is illustrated  in  Fig.  3(d).  Each  customer  has  a  corresponding nearest DC or the closest PC in the final clusters.

## 5.2. GA PSO for CMVRPTEMDP -

## 5.2.1. Chromosome, coding and decoding

The GA has excellent robustness, and the PSO can avoid falling into the feasible local solution (Goksal et al., 2013; Wang et al., 2015b; Li et al., 2016a; Hannan et al., 2018). Accordingly, the GA -PSO hybrid algorithm is designed to optimize CMVRPTWMDP. An improved coding method of the chromosome is proposed to properly utilize the results of IDKCA, which is modified based on the method proposed by Wang et al. (2018b). Fig. 4 shows the encoding and decoding processes of this study.

In Fig. 4, the number of vehicles is four (marked as 18, 19, 20, and 21), and DC2 and PC2 are denoted as 0 and 1, respectively. The customers  are  labeled  as  2,  3,  4, … ,  17.  Chromosome  1  is  a  randomly generated initial solution and its genetic makeup and decoding result are shown in Fig. 4. In chromosome 1, the service routes of vehicles 1 and 4 are closed, while the service routes of vehicles 2 and 3 are open. Each customer s demands are met after the vehicle s first visit. ' '

## 5.2.2. Order crossover

The roulette wheel selection method is adopted to select individuals with large fitness values to perform the order crossover operation. The specific way of order crossover operation is illustrated in Fig. 4.

The first step in Fig. 5 is to determine that the crossover gene segments  in  the  parents  1  and  2  are  (7,  8,  9,  10)  and  (3,  9,  10,  13), respectively. Second, genes (7, 8, 9, 10) are placed into the forefront of parent 1, and genes (3, 9, 10, 13) are placed into the forefront of parent 2. Then, the duplicate and missing genes in parents 1 and 2 are determined. Finally, the missing gene is filled into the gene position wherein the duplicate gene appears for the second time in one chromosome. After the crossover, offsprings 1 and 2 are produced from parents 1 and 2, respectively.

## 5.1. Improved 3D k-means clustering algorithm

The  -means clustering algorithm is a common method for customer k clustering, which clusters customers based on their time windows, coordinates, and other characteristics (Van Anholt et al., 2016; Rabbani et al., 2017; Wang et al., 2020a). In this study, time and geographic distances based on customer time windows and location information are combined into space -time distance by weighting, which is regarded as the  basis  for  clustering.  Eqs.  (33) -(35)  show  the  specific  calculation method of the proposed space -time distance.

$$d _ { r h } ^ { S T } & = \wp _ { 1 } \cdot d _ { r h } ^ { s } + \wp _ { 2 } \cdot d _ { r h } ^ { T } & ( 3 3 ) & \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \ \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \vec { \text{$\theta$} } \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \ & \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} & \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$} \quad \text{$\theta$}$$

$$d _ { r h } ^ { T } & = \left | \frac { ( e _ { r } + l _ { r } ) } { 2 } \ - \ \frac { ( e _ { h } + l _ { h } ) } { 2 } \right | \cdot v e l o c i t y & \quad \text{are close}$$

$$\wp _ { 1 } + \wp _ { 2 } & = 1 & ( 3 5 ) & \stackrel { \lambda \lambda \lambda \lambda } { \stackrel { \lambda } { \quad T h e r \, t } } }$$

In Eqs. (33) and (35), ℘ 1 and ℘ 2 represent the geographic and time distance coefficients, respectively. drh S and drh T characterize the geographic and time distances between nodes r and  , respectively. h drh ST is the space -time distance between nodes r and  . Eq. (33) shows that the h space -time distance is a weighted combination of geographic and time distances. Eq. (34) symbolizes the calculation method of time distance, which mainly refers to the method proposed by Qi et al. (2011) and Wang  et  al.  (2020b).  Eq.  (35)  ensures  that  the  total  weight  of  the geographic and time distances does not exceed 1. Fig. 3 illustrates the proposed IDKCA in detail from the customer s perspective, which con-' taining two DCs, two PCs, and nine customers.

Fig.  3(a)  displays  the  initial  service  relationship  of  customers. Assuming that DC1, DC2, PC1, and PC2 agree to collaborate and form a collaborative  alliance,  their  customer  services  can  be  shared,  which

5.2.3. Position, velocity, and inertia weight update formula of PSO In PSO, the velocity, local feasible solution, global feasible solution,

Fig. 5. Diagram of the order crossover operation.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000007_fd78959720ca4413f3ab1bb6fd41a745047df531c84519cd2dfe5ef5f4ef7f8e.png)

Y. Wang et al.

Fig. 6. Flowchart of the coordination operator between GA and PSO.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000008_9e7365296d492a0dd11eb412c9aea1d1ebac472ca0d897a587add8627e057b50.png)

Table 4

Procedure of the two-stage hybrid algorithm.

## The two-stage hybrid algorithm.

- 1 : Input basic data sets including coordinates, time windows and demands of each logistics facility and its customers,

(Nickabadi et al., 2011; Chen et al., 2016; Yao et al., 2016). Assuming that the maximum number of iterations is Maxgen , the current iteration is marked gen , the current position of particle q is Xq gen , the local feasible position of particle q is Pq gen , and the global feasible position of particles is Gq gen . Therefore, the update mechanism of the position, velocity, and inertia weight designed in this study are shown in Eqs. (36) -(38).

- 2 : Select m DC and n PC as the initial clustering centers. Set the coefficient of geographic and time distances, Maxgen , R1 R2 G GG , , , .
- 3 : gen = 1
- 4 while : gen ≤ Maxgen do
- 5 : Calculate the space -time distance matrix from each customer to each center and set r1 = 1.
- 6 : Assign each customer to the closest DC and the closest PC.
- 7 : Update the clusters and calculate the new center points.
- 8 if : customers need to be adjusted among clusters then
- 9 : Re-calculate the distance matrix.
- 10 : Re-assign each customer to the appropriate DC and the appropriate PC.
- 11 : Re-update the clusters and calculate the new center points
- 12 end if :
- 13: Return : The cluster results
- 14: if r1 ≤ 1, then
- Initialize parameters and generate the initial population including G chromosomes.
- 15: end if
- 16: Calculate the objective function values and re-set r1 = 1, set r2 = 1.
- 17 : Execute the roulette wheel selection, order crossover, mutation, and local search operations
- 18 : Calculate and sort the objective function values
- 19 if : r2 ≤ R2 , then
- 20 : Substitute the GG best-fit chromosomes in GA for the GG worst-fit particles in PSO, and set r2 = 1
- 21 : Execute particle swarm operations for G particles
- 22 end if :
- 23 : Update the local and global feasible solutions
- 24 : gen = gen + 1, r1 = r1 + 1, r2 = r2 + 1
- 25 : Record the best known solution and the objective function values
- 26 if : gen ≤ Maxgen , then
- 27 if : r1 ≤ R1 , then
- 28 : Repeat the operations showed in lines 17 -27
- 29 end if :
- 30 : Repeat the operations showed in lines 7 -27.
- 31 end if :
- 32 : Obtain the new population and calculate the objective function values
- 33 : Output the best known feasible solution
- 34 End while :
- Return: The best known feasible solution

and inertia  weight are  the  four  key  factors  for  updating  the  particle position  (Ai  and  Kachitvichyanukul,  2009;  Li  et  al.,  2016b;  Hannan et al., 2018). The velocity is updated to order the particles to a new position. The value of inertia weight affects the algorithm s local and ' global search capabilities. If the value of the inertia weight is considerable, then the particle can search for a feasible solution in an ample space; if the value of the inertia weight is small, then the search space of the particle is small. Thus, adopting time-varying inertia weight strategies  to  update  the  weight  can  enhance  the  algorithm s  performance '

$$\stackrel { \text{-\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\ dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots \dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dts\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\sigma\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\finally\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\eldots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots\dots
\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{--\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{ -\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{	-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text$-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text	-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\tt{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\tt	-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\test{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text---\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\text{-\-$$

$$X _ { q } ^ { \text{gen} + 1 } = X _ { q } ^ { \text{gen} } + v _ { q } ^ { \text{gen} + 1 }$$

$$w _ { g e n + 1 } = w _ { g e n } + \sum _ { i = 1 } ^ { g e n } \left [ \frac { | g - a v e _ { g } | } { g _ { m a x } - \overline { g } } \right ] ^ { \left [ \frac { ( g e n x 2 ) } { M a q e n } \right ] + 1 }$$

In Eqs.(36) -(37), wgen signifies the inertia weight, a1 and a2 are the two random parameters for adjustment, and their values are between zero and one. Variables c1 and c2 are constants, and vq gen is the velocity of particle q in the gen th iteration. In Eq. (38), the inertia weight wgen is adaptively  adjusted  nonlinearly  to  adjust  the  algorithm s  local  and ' global  search  capabilities. aveg indicates  the  average  fitness  function value  of  all  particles,  and gmax is  the  global  feasible  fitness  function value.

## 5.2.4. Coordination operator between GA and PSO

In order to increase the population diversity and improve the possibility of finding a feasible solution (Wang et al., 2018a), the coordination operator between GA and PSO is designed, which is according to the principle of elite alteration (Chen and Hao, 2016; Li et al., 2016b; Rabbani et al.,  2017). The designed coordination operator execution process is demonstrated in Fig. 6.

In Fig. 6, first, the total number of G chromosomes is obtained by the GA  operation,  and,  the GG best-fit  chromosomes  are  selected  and retained based on the fitness function value of the G chromosomes. Next, the G chromosomes are treated as G particles and the particle swarm operation  is  performed  on  those G particles.  Then,  the GG best-fit chromosomes  substitute  the GG worst-fit  particles  in  the  particle swarm.  Finally,  a  new  particle  swarm  with G particles  is  formed. Therefore,  the  coordination  operator  improves  the  diversity  of  the population and increases the possibility of the algorithm determining a feasible solution.

## 5.2.5. Pseudocode of the two-stage hybrid algorithm

The proposed two-stage hybrid algorithm includes two parts: IDKCA and  GA -PSO.  The  pseudocode  of  this  two-stage  hybrid  algorithm  is illustrated in Table 4.

Y. Wang et al.

## 5.3. Profit allocation

## 5.3.1. MCRS

Many profit allocation mechanisms exist, including MCRS (Liu and Sui, 2016). In this method, the member s marginal income and mini-' mum income determine  that  member s  final  profit.  This  mechanism ' allocates the profit obtained by the alliance in a weighted manner. Eqs. (39) -(41) symbolize how to use the MCRS mechanism to calculate the alliance s profit allocation scheme. '

$$\mathfrak { O } _ { i } ( S, \varphi ) = p _ { i } ^ { \min } + \frac { p _ { i } ^ { \max } - p _ { i } ^ { \min } } { \sum _ { i \in S } ( p _ { i } ^ { \max } - p _ { i } ^ { \min } ) } \times \left ( \varphi _ { S } - \sum _ { i \in S } p _ { i } ^ { \min } \right ), i \in S \quad \ ( 3 9 ) \quad \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \
 \text{the locati} \quad \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \$$

$$p _ { i } ^ { \min } & = \left \{ p _ { 1 } ^ { \min }, p _ { 2 } ^ { \min }, \cdots, p _ { i } ^ { \min } \right \} & & \text{the device} \\ & \quad \quad \quad \,. \quad \,.$$

customers  (15  customers  with  delivery  demands,  15  customers  with pickup demands, and 10 customers with delivery and pickup demands), and the third 10 instances include 70 customers (25 customers with delivery  demands,  25  customers  with  pickup  demands,  and  20  customers  with  delivery  and  pickup  demands).  The  merged  and  transshipped cargoes among DCs or PCs by a fleet of semitrailer trucks need to be considered in CMVRPTWMDP, making the problem difficult to solve using a CPLEX solver. Therefore, only the open -closed mixed vehicle routes between one DC and PC are studied by the CMVRPTWMDP in these  30  small-scale  instances.  All  customers  in  each  of  these  30  instances 3 are assumed to be served by two centers (i.e., DC and PC), and the locations of two centers are randomly generated in the region where the delivery or pickup customers are located.

$$p _ { i } ^ { \max } & = \left \{ p _ { i } ^ { \max }, p _ { 2 } ^ { \max }, \cdots, p _ { i } ^ { \max } \right \} & \quad \text{the time} \\ & \quad \text{$(41)} \quad & \text{small-sc} i$$

In Eq. (39), ℧ i ( S, φ ) represents the profit allocated to member i in alliance S , and φ S is the total profit generated by alliance S . Eqs. (40) -(41)  are  the  expected  profit  range  of  each  member  participating  in alliance S . Eq. (40) indicates that the profits of each member in collaboration  cannot  be  less  than  the  profits  in  noncollaboration.  Eq.  (41) limits  the  maximum  profit  of  each  member  in  collaboration,  which equals  their  marginal  profit.  If  the  minimum  profit  of  a  member  is greater than the marginal profit, then set the member s marginal profit ' equal to the minimum profit.

## 5.3.2. SMP selection strategy

The sequence in which members join the collaborative alliance affects the allocation of profits and the stability of that alliance. When a new member participates in the alliance, the existing members are only willing to continue to participating in the collaborative alliance if their profits do not decrease (Cruijssen et al., 2010; Wang et al., 2018b). The SMP selection strategy is a principle used to find the optimal sequence for members to join the alliance. Eq. (42) displays the calculation of the cost  reduction  percentage,  which  provides  evidence  for  the  implementation of the SMP. Variable Ci in Eq. (42) represents the initial cost if member i does not participate in the collaborative alliance.

$$\omega _ { i } = \frac { \Im _ { i } ( S, \varphi ) } { C _ { i } }, i \in S & & ( \text{42} ) & & \text{of about} \cdot \\ & & \text{bound} \, \nu \cdot \dots \..$$

## 6. Implementation and analysis

## 6.1. Small-scale test instances

In  order to assess the performance of the proposed GA-PSO algorithm, this study randomly selects 30 groups of small-scale instances from datasets C101, R101, and RC101 based on Solomon s benchmark ' (Solomon, 1987). Each instance contains three types of customers (i.e., those with delivery demands, pickup demands, and delivery and pickup demands) with time windows, and customers with independent delivery or pickup demands can be directly selected from the above datasets. In addition, we can extract pickup demands from another dataset and add them into customers with only delivery demands to form customers with delivery and pickup demands. The proposed GA-PSO algorithm 1 and CPLEX solver 2 are used to solve the single-objective optimization model of the minimum logistics operating cost in CMVRPTWMDP. There are 20 customers (seven customers with delivery demands, seven customers with pickup demands, and six customer with delivery and pickup demands) in the first 10 instances, and the second 10 instances include 40

1 https://github.com/helloaiyoweily/Work-space-for-ga-pso-and-30-instan ces/tree/master

2 https://github.com/helloaiyoweily/Work-space-for-cplex-and-30-instance s/tree/master

The running time of the CPLEX solver is set to the larger of 7200 s and the time to find a feasible solution for each of these 30 instances. These small-scale instances are implemented using ILOG CPLEX Optimization Studio 12.10 and the proposed algorithm. The proposed algorithm is terminated when no more enhancements can be made over the bestknown solution within 10 consecutive iterations for each run; in addition, the proposed algorithm is used to perform 20 separate runs for each instance, and the average and minimum objective function values can be obtained from these 20 runs. Furthermore, the running time (RT), upper bounds  (UB),  lower  bounds  (LBs),  and  gaps  (i.e., GAP\_AL between average  and  lower  bound, GAP\_UL between  upper  bound  and  lower bound) are also obtained from the CPLEX solver and the proposed algorithm, as shown in Table 5.

In Table 5, the proposed algorithm can provide feasible solutions in a reasonable computing time, while it takes the CPLEX solver much longer to obtain optimal solutions. For example, CPLEX can obtain an optimal solution in about 416 s, while the proposed algorithm takes less than 24 s to obtain the same solution for instance 1. For each of the first and second  10  instances,  CPLEX  can  obtain  the  optimal  solutions  within 7200 s, whereas the proposed algorithm can attain feasible close-tooptimal solutions within a gap of 1.4% and computing time of 50 s. For the third 10 instances, the proposed algorithm can quickly obtain the optimal or close-to-optimal solutions, whereas CPLEX can obtain the optimal solutions in more than 6000 s for instances 21, 22, 24, 25, 26, and 27. In addition, CPLEX can only obtain feasible solutions with gaps of about 4.0% for instances 28 and 30, and CPLEX can only get a lower bound but no feasible solutions can be obtained for instances 23 and 29. These  results  show  the  limitation  of  CPLEX  and  the  stability  and robustness  of  proposed  algorithm  in  solving  the  CMVRPTWMDP. Therefore,  the  proposed  algorithm  outperforms  the  CPLEX  solver  by obtaining high-quality solutions using short computing times in smallscale test instances.

## 6.2. Algorithm comparison

The genetic algorithm and tabu search (GA -TS 4 ) (Liu et al., 2013) and iterated local search algorithm (ILS ) (Li et al., 2015) are leveraged 5 to verify the performance of the proposed GA -PSO. GA TS is usually -adopted in solving VRPSDPTW optimization (Liu et al., 2013; Qiu et al., 2018a). The CMVRPTWMDP is revised to be similar to the VRPSDPTW. The specific revisions include: all customers are regarded as customers with delivery and pickup demands and time windows, that is, customers with only delivery demands can set their pickup demands to zero, and the  time  windows  of  customer  pickup  demands  are  set  equal  to  the corresponding time windows of their delivery demands. Customers with only pickup demands are treated similarly. In the route optimization process, the nearest logistics facility (e.g., DC and PC) can be selected as the return destination according to the types of vehicle loading (e.g.,

3 https://github.com/helloaiyoweily/30-instances/tree/master

4 https://github.com/HelloWorldRS/Codes/tree/master/Codes/GATS

5 https://github.com/HelloWorldRS/Codes/tree/master/Codes/ILS

Y. Wang et al.

Table 5 Performance comparison of the CPLEX and hybrid heuristic algorithm for small-scale instances.

|                         |       | Solution by proposed approach   | Solution by proposed approach   | Solution by proposed approach   | Solution by proposed approach   | Solution by CPLEX   | Solution by CPLEX   | Solution by CPLEX   | Solution by CPLEX   |
|-------------------------|-------|---------------------------------|---------------------------------|---------------------------------|---------------------------------|---------------------|---------------------|---------------------|---------------------|
|                         |       | Average a                       | Minimum b                       | GAP_AL (%) c                    | RT ( s )                        | UB                  | LB                  | GAP_UL (%) d        | RT ( s )            |
| Number of customers: 20 | 1     | 706.2                           | 706.2                           | 0                               | 23.1                            | 706.2               | 706.2               | 0                   | 415.5               |
| Number of customers: 20 | 2     | 686.5                           | 686.5                           | 0                               | 31.4                            | 686.5               | 686.5               | 0                   | 673.7               |
| Number of customers: 20 | 3     | 723.7                           | 723.7                           | 0                               | 22.2                            | 723.7               | 723.7               | 0                   | 604.3               |
| Number of customers: 20 | 4     | 795.4                           | 795.4                           | 0                               | 24.5                            | 795.4               | 795.4               | 0                   | 752.1               |
| Number of customers: 20 | 5     | 715.6                           | 715.6                           | 0                               | 26.3                            | 715.6               | 715.6               | 0                   | 712.9               |
| Number of customers: 20 | 6     | 703.7                           | 703.7                           | 0                               | 26.7                            | 703.7               | 703.7               | 0                   | 621.6               |
| Number of customers: 20 | 7     | 742.3                           | 742.3                           | 0                               | 22.5                            | 742.3               | 742.3               | 0                   | 675.3               |
| Number of customers: 20 | 8     | 687.2                           | 676.4                           | 1.6                             | 30.8                            | 676.4               | 676.4               | 0                   | 732.7               |
| Number of customers: 20 | 9     | 701.8                           | 701.8                           | 0                               | 28.3                            | 701.8               | 701.8               | 0                   | 889.5               |
| Number of customers: 20 | 10    | 792.6                           | 788.5                           | 0.5                             | 18.7                            | 788.5               | 788.5               | 0                   | 903.4               |
| Number of customers: 40 | 11    | 1346.4                          | 1327.5                          | 1.4                             | 42.1                            | 1327.5              | 1327.5              | 0                   | 2533.7              |
| Number of customers: 40 | 12    | 1476.6                          | 1476.6                          | 0                               | 38.5                            | 1476.6              | 1476.6              | 0                   | 3022.5              |
| Number of customers: 40 | 13    | 1398.3                          | 1398.3                          | 0                               | 39.4                            | 1398.3              | 1398.3              | 0                   | 3670.2              |
| Number of customers: 40 | 14    | 1539.5                          | 1527.9                          | 0.8                             | 43.2                            | 1552.6              | 1527.9              | 1.6                 | 7200                |
| Number of customers: 40 | 15    | 1472.4                          | 1472.4                          | 0                               | 45.3                            | 1472.4              | 1472.4              | 0                   | 3528.6              |
| Number of customers: 40 | 16    | 1416.7                          | 1416.7                          | 0                               | 49.2                            | 1416.7              | 1416.7              | 0                   | 3681.5              |
| Number of customers: 40 | 17    | 1423.6                          | 1423.6                          | 0                               | 41.1                            | 1423.6              | 1423.6              | 0                   | 4816.1              |
| Number of customers: 40 | 18    | 1527.3                          | 1510.5                          | 1.1                             | 40.5                            | 1523.3              | 1510.5              | 0.8                 | 7200                |
| Number of customers: 40 | 19    | 1501.8                          | 1501.8                          | 0                               | 48.6                            | 1501.8              | 1501.8              | 0                   | 3927.6              |
| Number of customers: 70 | 21    | 1942.3                          |                                 |                                 | 57.5                            | 1942.3              | 1942.3              | 0                   | 6052.1              |
| Number of customers: 70 | 22    | 1904.7                          | 1942.3 1904.7                   | 0 0                             | 63.7                            | 1904.7              | 1904.7              | 0                   | 6518.3              |
| Number of customers: 70 | 23    | 2193.1                          |                                 | 3.6                             | 68.2                            |                     |                     |                     |                     |
| Number of customers: 70 |       |                                 | 2152.6                          |                                 | 70.1                            | -                   | 2115.6              | -                   | 7200                |
| Number of customers: 70 | 24    | 2023.5                          | 2023.5                          | 0                               |                                 | 2023.5              | 2023.5              | 0                   | 6739.2              |
| Number of customers: 70 | 25    | 2005.2                          | 2005.2                          | 0                               | 69.6                            | 2005.2              | 2005.2              | 0                   | 6875.8              |
| Number of customers: 70 | 26    | 1887.8                          | 1862.5                          | 2.4                             | 65.9 73.7                       | 1843.6              | 1843.6              | 0                   | 6325.5 6637.9       |
| Number of customers: 70 | 27 28 | 1944.1 1957.6                   | 1911.9 1937.3                   | 2.8 2.2                         | 65.1                            | 1891.4 2011.5       | 1891.4 1915.8       | 0 4.7               | 7200                |
| Number of customers: 70 | 29 30 | 2082.5 2021.7                   | 2055.8 1996.3                   | 2.5 2.0                         | 71.2 72.5                       | - 2056.2            | 2031.2 1982.3       | - 3.6               | 7200 7200           |

- a Average objective function value obtained from 20 runs.
- b Minimum objective function value obtained from 20 runs.
- c The gap percentage of the average objective function value from LB.
- d The gap percentage between UB and LB with respect to UB.

Table 6 Characteristics of the adjusted instances.

| No.   | Instances           | Vehicle capacity   | Initial datasets   | Initial datasets   | Revised datasets   | Revised datasets   | Revised datasets   |
|-------|---------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
|       |                     |                    | Number of Depots   | Customers          | Number of DCs      | Number of PCs      | Customers          |
| 1     | 1DC3PC_15D10PD23P   | 50                 | 4                  | 48                 | 1                  | 3                  | 48_(15; 10; 23)    |
| 2     | 2DC2PC_38D27PD31P   | 50                 | 4                  | 96                 | 2                  | 2                  | 96_(38; 27; 31)    |
| 3     | 1DC3PC_48D38PD58P   | 50                 | 4                  | 144                | 1                  | 3                  | 144_(48; 38; 58)   |
| 4     | 2DC2PC_57D58PD77P   | 80                 | 4                  | 192                | 2                  | 2                  | 192_(57; 58; 77)   |
| 5     | 1DC3PC_63D78PD99P   | 80                 | 4                  | 240                | 1                  | 3                  | 240_(63; 78; 99)   |
| 6     | 1DC3PC_95D78PD115P  | 90                 | 4                  | 288                | 1                  | 3                  | 288_(95; 78; 115)  |
| 7     | 4DC2PC_29D32PD11P   | 80                 | 6                  | 72                 | 4                  | 2                  | 72_(29; 32; 11)    |
| 8     | 3DC3PC_58D28PD58P   | 60                 | 6                  | 144                | 3                  | 3                  | 144_(58; 28; 58)   |
| 9     | 2DC4PC_46D84PD86P   | 80                 | 6                  | 216                | 2                  | 4                  | 216_(46; 84; 86)   |
| 10    | 3DC3PC_105D68PD115P | 80                 | 6                  | 288                | 3                  | 3                  | 288_(105; 68; 115) |
| 11    | 2DC2PC_19D10PD19P   | 60                 | 4                  | 48                 | 2                  | 2                  | 48_(19; 10; 19)    |
| 12    | 1DC3PC_27D31PD38P   | 60                 | 4                  | 96                 | 1                  | 3                  | 96_(27; 31; 38)    |
| 13    | 1DC3PC_36D50PD58P   | 70                 | 4                  | 144                | 1                  | 3                  | 144_(36; 50; 58)   |
| 14    | 2DC2PC_79D36PD77P   | 80                 | 4                  | 192                | 2                  | 2                  | 192_(79; 36; 77)   |
| 15    | 2DC2PC_100D44PD96P  | 80                 | 4                  | 240                | 2                  | 2                  | 240_(100; 44; 96)  |
| 16    | 2DC2PC_115D58PD115P | 80                 | 4                  | 288                | 2                  | 2                  | 288_(115; 58; 115) |
| 17    | 2DC4PC_20D23PD29P   | 80                 | 6                  | 72                 | 2                  | 4                  | 72_(20; 23; 29)    |
| 18    | 2DC4PC_55D28PD61P   | 60                 | 6                  | 144                | 2                  | 4                  | 144_(55; 28; 61)   |
| 19    | 3DC3PC_87D43PD86P   | 70                 | 6                  | 216                | 3                  | 3                  | 216_(87; 43; 86)   |
| 20    | 2DC4PC_100D73PD115P | 70                 | 6                  | 288                | 2                  | 4                  | 288_(100; 73; 115) |
| 21    | 2DC2PC_90D54PD96P   | 80                 | 4                  | 240                | 2                  | 2                  | 240_(90; 54; 96)   |
| 22    | 2DC4PC_10D33PD29P   | 80                 | 6                  | 72                 | 2                  | 4                  | 72_(10; 33; 29)    |
| 23    | 1DC3PC_45D38PD61P   | 80                 | 4                  | 144                | 1                  | 3                  | 144_(45; 38; 61)   |
| 24    | 4DC2PC_109D93PD86P  | 100                | 6                  | 288                | 4                  | 2                  | 288_(109; 93; 86)  |
| 25    | 2DC4PC_36D94PD86P   | 100                | 6                  | 216                | 2                  | 4                  | 216_(36; 94; 86)   |

*\_( ;  ; ∘ ≀ ⋇ ): * is the total number of customers in instance.  ,  , and ∘ ≀ ⋇ are the number of customers with delivery demands, with delivery and pickup demands, and with pickup demands in instance, respectively.

Y. Wang et al.

Table 7

Comparison of the different algorithms.

Fig. 7. Spatial distribution of the DCs, PCs and customers.

| Instances           | GA - PSO   | GA - PSO   | GA - PSO   | GA - TS       | GA - TS       | GA - TS       | GA - TS      | GA - TS      | ILS          |
|---------------------|------------|------------|------------|---------------|---------------|---------------|--------------|--------------|--------------|
| Instances           | TC($)      | Nv         | CT(s)      | TC($)         | Nv            | CT(s)         | TC($)        | Nv           | CT(s)        |
| 1DC3PC_15D10PD23P   | 1519.3     | 6          | 41         | 1580.0        | 7             | 66            | 1553.9       | 6            | 42           |
| 2DC2PC_38D27PD31P   | 3185.8     | 21         | 87         | 3766.9        | 23            | 117           | 3519.2       | 21           | 91           |
| 1DC3PC_48D38PD58P   | 4003.2     | 29         | 119        | 4699.3        | 31            | 131           | 4485.4       | 30           | 134          |
| 2DC2PC_57D58PD77P   | 5630.0     | 32         | 154        | 5732.1        | 33            | 162           | 5705.6       | 32           | 158          |
| 1DC3PC_63D78PD99P   | 7465.9     | 37         | 220        | 8002.4        | 39            | 245           | 7663.0       | 38           | 234          |
| 1DC3PC_95D78PD115P  | 8235.2     | 43         | 231        | 8727.0        | 46            | 257           | 8709.6       | 46           | 236          |
| 4DC2PC_29D32PD11P   | 2901.0     | 8          | 61         | 3270.2        | 9             | 88            | 3233.3       | 8            | 72           |
| 3DC3PC_58D28PD58P   | 3997.9     | 28         | 115        | 4352.9        | 30            | 141           | 4163.5       | 29           | 128          |
| 2DC4PC_46D84PD86P   | 6255.5     | 35         | 187        | 7071.8        | 38            | 215           | 7017.7       | 38           | 199          |
| 3DC3PC_105D68PD115P | 8207.3     | 43         | 201        | 8607.2        | 45            | 217           | 8364.0       | 44           | 230          |
| 2DC2PC_19D10PD19P   | 1496.9     | 6          | 30         | 1566.5        | 7             | 60            | 1538.2       | 6            | 48           |
| 1DC3PC_27D31PD38P   | 3159.4     | 20         | 65         | 3782.3        | 23            | 96            | 3400.9       | 21           | 76           |
| 1DC3PC_36D50PD58P   | 4108.2     | 28         | 119        | 4294.8        | 28            | 130           | 4123.1       | 28           | 135          |
| 2DC2PC_79D36PD77P   | 5597.1     | 30         | 154        | 6079.2        | 34            | 179           | 5990.5       | 33           | 166          |
| 2DC2PC_100D44PD96P  | 7645.2     | 38         | 200        | 8011.9        | 41            | 234           | 7691.7       | 39           | 230          |
| 2DC2PC_115D58PD115P | 8136.8     | 42         | 213        | 8745.5        | 45            | 224           | 8562.4       | 43           | 228          |
| 2DC4PC_20D23PD29P   | 2885.9     | 6          | 64         | 3411.0        | 8             | 91            | 3217.0       | 6            | 83           |
| 2DC4PC_55D28PD61P   | 3964.3     | 27         | 119        | 4395.4        | 30            | 147           | 4203.2       | 29           | 122          |
| 3DC3PC_87D43PD86P   | 6371.4     | 36         | 196        | 6569.2        | 36            | 225           | 6441.3       | 36           | 214          |
| 2DC4PC_100D73PD115P | 8341.0     | 44         | 209        | 9068.3        | 48            | 217           | 8570.9       | 46           | 210          |
| 2DC2PC_90D54PD96P   | 7206.8     | 37         | 197        | 7991.9        | 40            | 222           | 7924.5       | 40           | 202          |
| 2DC4PC_10D33PD29P   | 2853.2     | 8          | 52         | 3259.4        | 11            | 79            | 2988.8       | 9            | 68           |
| 1DC3PC_45D38PD61P   | 3492.3     | 22         | 113        | 4073.7        | 25            | 135           | 3634.5       | 23           | 118          |
| 4DC2PC_109D93PD86P  | 8151.9     | 41         | 201        | 8598.0        | 44            | 219           | 8156.0       | 42           | 219          |
| 2DC4PC_36D94PD86P   | 6301.3     | 36         | 179        | 6523.2        | 38            | 201           | 6441.3       | 37           | 191          |
| Average             | 5244.5     | 29         | 141        | 5687.2        | 31            | 164           | 5492.0       | 30           | 153          |
| t -test             |            |            |            | 1.5E-10       | 1.5E-10       | 5.2E-14       | 1.7E-06      | 1.2E-05      | 1.7E-08      |
| p -value            |            |            |            | GLYPH<0> 11.0 | GLYPH<0> 10.4 | GLYPH<0> 14.4 | GLYPH<0> 6.1 | GLYPH<0> 5.3 | GLYPH<0> 8.3 |

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000009_a7b530d06d5c0d1436dd66b8e8c2256ad082b09f22bf2072932ca3458314550a.png)

delivery  demands,  pickup  demands)  after  the  vehicle  has  served  all customers in the route.

In addition, the ILS algorithm is also usually applied in solving the VRPSDPTW  (Li  et  al.,  2015;  Brandao,  2018).  Neighborhood  and ˜

perturbation operations are the main components of the ILS algorithm (Bernardino  and  Paias,  2018;  Maximo  and  Nascimento,  2021).  An ´ adaptive neighborhood selection mechanism is adopted instead of the traditional fixed neighborhood method (Li et al., 2015), which employs

Y. Wang et al.

Table 8 Service relationship of the customers in the 2nd type.

| Service relationship            | Customers                              |   Number of allocated customers |
|---------------------------------|----------------------------------------|---------------------------------|
| Customers served by DC1 and PC1 | C95,C96,C98,C101,C104,C117             |                               6 |
| Customers served by DC2 and PC1 | C94,C100,C103,C105,C120                |                               5 |
| Customers served by DC2 and PC2 | C93,C97,C107,C111,C114,C115            |                               6 |
| Customers served by DC2 and PC3 | C108,C112,C118                         |                               3 |
| Customers served by DC3 and PC3 | C99,C102,C106,C109,C110,C113,C116,C119 |                               8 |

a  scoring  rule  to  evaluate  each  neighborhood  method  and  then  introduces the roulette wheel selection for finding the local feasible solution.  Furthermore,  the  perturbation  method  combined  with  greedy criteria (Avci and Topaloglu, 2015; Brandao, 2020) is applied to escape ˜ from the local optimum and guarantee the solution s feasibility. Spe-' cifically, when the obtained solution is infeasible after perturbation, the extra  nodes  are  removed  by  selecting  the  nodes  with  the  highest reduction in the objective function.

A total  of  25  benchmark  instances  are  constructed  based  on  two datasets (i.e., one for the multidepot VRP  with  time windows (MDVRPTW) 6  and the other for the capacitated VRP with pickups and deliveries and time windows (CVRPPDTW) 7 )  from the NEO Research Group database . Since the CMVRPTWMDP includes two types of cen8 ters and three types of customer demands, the depots in the MDVRPTW dataset can be divided into a certain number of DCs and PCs, and the customer demands can be divided into three types of customer demands (i.e., delivery demands, pickup demands, and delivery and pickup demands). The pickup part of the third type of demand (i.e., delivery and pickup demands) can be extracted from the CVRPPDTW dataset, and the time  windows  of  customer  pickup  demands  are  equal  to  the  corresponding time windows of customer delivery demands. Table 6 lists the characteristics of the modified instances. Here, n1 DC n2 PC\_ n3 D n4 P n5 PD denotes the modified instance, with n1 being the number of DCs, n2 being the number of PCs, n3 being the number of customers with delivery demands, n4 being the number of customers with delivery and pickup demands, and n5 being the number of customers with pickup demands.

In Table 6, there are 25 modified instances with different numbers of centers, customers and types of customer demands. Then the three algorithms, namely, GA-PSO, ILS and GA-TS are used to calculate the TC, Nv and the CT of each instance. The common parameters of the three algorithms are set as follows: selection probability sp = 0.9, crossover probability cp = 0.8, mutation probability mp = 0.1, population size G = 100, number of iterations Maxgen = 150, coefficient of geographic distances ℘ 1 = 0 8, coefficient of time distances . ℘ 2 = 0 2, the number . of iterations without improvement is 20. The parameters of GA-PSO: constant coefficient c1 = 1.5, c2 = 1.5,  number of worst-fit  chromosomes GG = 25, internal reoptimization runs R1 = 25, R2 = 50, personal and global learning coefficients pc = gc = 2, and inertia weight iw = 0.9. The parameters unique to GA-TS: the permitted maximum step size with no improvement is 20, the number of iterations of TS is 100, and the length of the tabu list is 10. The parameters unique to ILS: the number of nearest neighbors is 10, the number of moves allowed in each perturbation is 30. The optimization results of the three algorithms in solving these instances are listed in Table 7, respectively.

In Table 7, the optimization results of those instances verify that the performance of GA -PSO is superior to those of ILS and GA-TS. First, the performance of these three algorithms in optimizing CMVRPTWMDP are significantly different in terms of the TC, Nv, and CT. The p -value and  t test prove the conclusion. Second, the optimization results calculated by

6 http://www.bernabe.dorronsoro.es/vrp/

7 https://neo.lcc.uma.es/vrp/vrp-instances/capacitated-vrp-with-pickup-and-deliveries-and-time-windows-instances/

8 https://neo.lcc.uma.es/vrp/vrp-instances/

GA PSO are better than those by the other two algorithms in these cases. -For example, the averages of TC in these three algorithms are $5244.5, $5687.2, and $5492.0. The average CTs of the three algorithms for those instances are 141 s, 164 s, and 153 s, confirming that the GA -PSO can find a feasible solution faster than the other two algorithms. Therefore, the  proposed  algorithm  GA -PSO  outperforms  the  ILS  and  GA -TS  in solving CMVRPTWMDP with a feasible solution, and the GA PSO al--gorithm  can  be  adjusted  to  solve  the  VRPPDTW,  VRPMDPTW  and MDVRPTW.

## 6.3. Data source

The logistics transportation system in Chongqing, an emerging international logistics transportation hub in China, can be regarded as a suitable  research  object  for  this  study.  A  real  logistics  network  in Chongqing is utilized to test the applicability and effectiveness of the proposed methods in this study. This real logistics network mainly includes three DCs (marked as DC1, DC2, and DC3), three PCs (denoted as PC1, PC2, and PC3), and 210 customers (labeled as C1, C2, C3, … , and C210). The numbers of customers in the 1st, 2nd, and 3rd types are 88, 28, and 94, respectively. Fig. 7 illustrates the location and label of these nodes, including the facilities and customers.

In Fig. 7, the spatial distribution of customers is chaotic. In this logistics network, three DCs and three PCs independently operate and only provide logistics services for their customers. The delivery and pickup demands  of  the  customers  in  the  2nd  type  are  split  and  served  by different facilities, which are listed in Table 8.

In Table 8, the delivery and pickup demands of these customers are served  by  different  logistics  facilities.  For  example,  the  delivery  demands of C95, C96, C98, C101, C104, and C117 are served by DC1, and the pickup demands of these customers are met by PC1. Although DC1 and PC1 jointly provide services for those six customers, DC1 does not collaborate with PC1. Table 9 lists the initial vehicle routes of a logistics network.  Two  superscript  symbols,  namely,  * 1 and  * ,  are  used  to 2 distinguish the served demands of customers in the 2nd type.

In Table 9, the Nv in the initial logistics network is 34, and those vehicle routes are closed. For example, V1 departs from DC1 to serve customers C26, C117* 1 , C7, C9, C20, and C5; then, it directly travels back to DC1 after visiting C5. Each customer in the 2nd type is visited twice by two vehicles. For example, the delivery demands of customer C117 are served by V1, and the pickup demands of C117 are served by V21. The transportation resource and customers ' information are not shared among these logistics facilities, making the vehicle s capacity ' idle. For example, the vehicle departing from DCs only provides delivery service  for  customers.  The  vehicle  departing  from  PCs  only  pickups customer goods.

## 6.4. Parameter settings and optimization results

## 6.4.1. Sensitivity analysis for parameter settings

The  computational  efficiency  and  stability  of  the  algorithm  are related to the parameter values (Goksal et al., 2013; Guo et al., 2018; Wang et al., 2018b). The relevant parameters in the proposed model are set based on the real-world observation and previous literature (Fleszar et al., 2009; Ray et al., 2014) are listed clearly in Table 10. Here, several key parameters, including crossover probability ( cp ), mutation

Y. Wang et al.

Table 9 Vehicle routes in the initial logistic network.

Parameter setting in the proposed model based on the real-world observation

| Facility   | Vehicles   | Routes                                                                                                       |
|------------|------------|--------------------------------------------------------------------------------------------------------------|
| DC1        | V1         | → → 1 → → → → → DC1                                                                                          |
|            | V2         | DC1 C26 C117* C7 C9 C20 C5 DC1 → C22 → C24 → C1 → C17 → C11 → DC1                                            |
|            | V3         | DC1 → C98* 1 → C95* 1 → C104* 1 → C21 → C18 → C14 → C19 → DC1                                                |
|            | V4         | DC1 → C10 → C15 → C23 → C4 → C202 → C204 → C203 → C201 → DC1                                                 |
|            | V5         | DC1 → C25 → C3 → C16 → C6 → C27 → C8 → C96* 1 → C101* 1 → C2 → C13 → C12 → DC1                               |
| DC2        | V6         | DC2 → C167 → C168 → C158 → C170 → C172 → C160 →                                                              |
|            | V7         | C166 → C173 → C171 → DC2 DC2 → C93* 1 → C120* 1 → C157 → C169 → C112* 1 → C161 → C162 → C153 → C111* 1 → DC2 |
|            | V8         | DC2 → C154 → C163 → C151 → C108* → C165 → C174 → C114* 1 → DC2                                               |
|            | V9         | DC2 → C103* 1 → C94* 1 → C105* 1 → C100* 1 → C152 → C155 → C107* → C164 → DC2                                |
|            | V10        | DC2 → C115* 1 → C159 → C97* 1 → C156 → C118* 1 → DC2                                                         |
| DC3        | V11        | DC3 → C66 → C208 → C206 → C74 → C80 → DC3                                                                    |
|            | V12        | DC3 → C84 → C99* 1 → C109* 1 → C67 → C71 → C69 → C91 → DC3                                                   |
|            | V13        | DC3 → C90 → C81 → C79 → C64 → C82 → C65 → C72 → C70 → DC3                                                    |
|            | V14        | DC3 → C76 → C87 → C85 → C78 → C205 → C207 → C83 → DC3                                                        |
|            | V15        | DC3 → C116* 1 → C119* 1 → C68 → C92 → C73 → C77 → C88 → DC3                                                  |
|            | V16        | DC3 → C89 → C110* 1 → C75 → C113* 1 → C102* 1 → C106* 1 → C86 → DC3                                          |
| PC1        | V17        | PC1 → C126 → C128 → C143 → C124 → PC1                                                                        |
|            | V18        | PC1 → C101* 2 → C135 → C140 → C96* 2 → C148 → PC1                                                            |
|            | V19        | PC1 → C122 → C98* 2 → C95* 2 → C127 → C133 → C146 → C134 → PC1                                               |
|            | V20        | PC1 → C130 → C123 → C94* 2 → C120* 2 → C150 → C138 → C100* 2 → C105* 2 → C149 → PC1                          |
|            | V21        | PC1 → C121 → C132 → C104* 2 → C103* 2 → C144 → C117* 2 → PC1                                                 |
|            | V22        | PC1 → C131 → C137 → C147 → C142 → C145 → C125 → C129 → C136 → C139 → C141 → PC1                              |
| PC2        | V23        | PC2 → C32 → C53 → C34 → C30 → C62 → C57 → C54 → PC2                                                          |
|            | V24        | PC2 → C44 → C115* 2 → C93* 2 → C107* 2 → C111* 2 → C63 → C47 → C45 → PC2                                     |
|            | V25        | PC2 → C29 → C58 → C55 → C28 → C39 → C38 → C42 → C46 → C52 → PC2                                              |
|            | V26        | PC2 → C59 → C40 → C49 → C61 → C36 → C114* 2 → C97* 2 → C35 → C37 → PC2                                       |
|            | V27        | PC2 → C48 → C50 → C43 → C60 → C31 → C51 → PC2                                                                |
|            | V28        | PC2 → C56 → C33 → C41 → PC2                                                                                  |
| PC3        | V29        | PC3 → C197 → C191 → C118*2 → C108*2 → C209 → PC3                                                             |
|            | V30        | PC3 → C198 → C193 → C178 → C194 → C176 → C188 → PC3                                                          |
|            | V31        | PC3 → C210 → C113* 2 → C102* 2 → C187 → C195 → C106* 2 → C199 → PC3                                          |
|            | V32        | PC3 → C180 → C192 → C99* 2 → C190 → C175 → C109* 2 → PC3                                                     |
|            | V33        | PC3 → C116* 2 → C119* 2 → C112* 2 → C110* 2 → C177 → C181 → PC3                                              |
|            | V34        | PC3 → C179 → C182 → C196 → C189 → C183 → C185 → C200 → C184 → C186 → PC3                                     |

* : The delivery demands of customer in 2nd type are only served by a delivery 1 vehicle.

* : The pickup demands of customer in 2nd type are only served by a pickup 2 vehicle.

probability  ( mp ),  inertia  weight  ( iw ),  personal  coefficient  ( pc ),  and global coefficient ( gc ), are selected to vary in 20 runs of the proposed algorithm for calculating the minimum logistics operating cost (Cost)

Table 10 and previous literature.

| Parameters related to the model   | Parameters related to the model               |                                                                                     |                                                                                            |                                                                                       |
|-----------------------------------|-----------------------------------------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| Cost                              | DC1: DC2 DC3: PC1: PC2: PC3: Each vehicle v : | G DC1 = 570 G DC2 = 585 G DC3 = 630 G PC1 = 630 G PC2 = 660 G PC3 = 615 M v = 20000 | CC DC1 = 95 CC DC2 = 97 CC DC3 = 105 CC PC1 = 105 CC PC2 = 110 CC PC3 = 102 Each truck k : | I DC1 = 660 I DC2 = 676 I DC3 = 728 I PC1 = 764 I PC2 = 1160 I PC3 = 1080 M k = 25000 |
| Capacity                          | Each facility f : Each vehicle v :            | C f = 1500 C v = 200                                                                | Each truck k :                                                                             | C k = 600                                                                             |
| Other parameters                  | T = 52 f v = 0.06 P v = P k = 6.4             | T v = 10 f k = 0.1                                                                  | T k = 15 ∂ e = 20                                                                          | ∂ l = 30                                                                              |

and  its  standard  deviation  ( SD ).  The  sensitivity  analysis  results  for different parameter settings are shown in Table 11.

In Table 11, the performance of the proposed algorithm is verified by different parameter settings, and the computation results, including Cost and SD are obtained with changing parameters. The optimized logistics operating cost is $22256, and SD is 1.9 when cp is 0.80, which indicates that  the  algorithm s  performance  is  relatively  stable.  The  optimal ' parameter setting should be the one that results in the lowest Cost and SD . Thus, based on the minimal logistics operating cost and the lowest SD values calculated using different parameters, the optimal parameters selected in the proposed algorithm are as follows: cp = 0.80, mp = 0.10, iw = 0.9, pc = 2.0 and gc = 2.0. Table 12 shows the parameter settings in the proposed GA-PSO algorithm.

## 6.4.2. Optimization results

Customer  clustering  is  an  effective  way  to  reduce  computational complexity. This study adopts an improved 3D k -means clustering algorithm based on their coordinates and time windows to simplify the CMVRPTWMDP. When calculating time space distance, the geographic -and time distance coefficients are 0.8 and 0.2, respectively. Fig. 8 illustrates the clustering results of the logistics network.

In Fig. 8, all customers are grouped into five clusters by IDKCA, and each cluster has two designated logistics facilities to provide services to customers.  For  example,  in  the  first  cluster,  the  customer s  delivery ' demands are met by DC1, and the customer s pickup demands are served ' by PC2. In cluster 2, customer delivery and pickup demands are jointly served by DC2 and PC1. Correspondingly, the optimized vehicle routes of each cluster are shown in Table 13. Parts of the optimized vehicle routes are shown in Fig. 9.

In Table 13, the total number of the optimized vehicle routes in the collaborative logistics network is 20, and most of these vehicle routes are open. First, each customer in the 2nd type is served by one vehicle, and their delivery and pickup demands can be met simultaneously. For example, the origin and destination of V1 are DC1 and PC2, respectively, which provide logistics services for C7, C101, C135, C2, C132, C17, C33, and C3, and the delivery and pickup demands of customer C101 are met by V1. Second, most vehicles have performed delivery and pickup activities.  Third,  the  V20  only  provides  delivery  services  on  the  closed route, and its origin and destination are both DC3.

In Fig. 9(a), the open routes of V1 and V17 and the closed route of V20 are shown. The delivery and pickup activities are performed in a mixed manner on the routes of V1 and V17. After V20 completes the delivery service provided to customers C81, C78, C85, C205, C83, and C65, it returns to DC3. In Fig. 9(b), the origin of V8, V9, and V14 is DC2, and their destinations are PC1, PC2, and PC3, respectively. The delivery and pickup demands of C111, C99, C106, and C109 in the 2nd type are satisfied  by  V9.  In  summary,  the  open  vehicle  route  can  only  be

Y. Wang et al.

Table 11 Computation result comparison among different parameter settings.

| Scenario   | cp    | cp      | cp   | mp    | mp      | mp   | iw    | iw      | iw   | pc    | pc      | pc   | gc    | gc      | gc   |
|------------|-------|---------|------|-------|---------|------|-------|---------|------|-------|---------|------|-------|---------|------|
| Scenario   | Value | Cost($) | SD   | Value | Cost($) | SD   | Value | Cost($) | SD   | Value | Cost($) | SD   | Value | Cost($) | SD   |
| 1          | 0.60  | 22,297  | 3.5  | 0.02  | 22,311  | 4.2  | 0.1   | 22,302  | 11.9 | 0.1   | 22,306  | 9.7  | 0.1   | 22,261  | 14.6 |
| 2          | 0.62  | 22,303  | 2.3  | 0.04  | 22,279  | 3.3  | 0.2   | 22,336  | 6.3  | 0.2   | 22,300  | 8.5  | 0.2   | 22,316  | 11.8 |
| 3          | 0.64  | 22,286  | 2.1  | 0.06  | 22,261  | 5.1  | 0.3   | 22,320  | 6.4  | 0.3   | 22,264  | 9.2  | 0.3   | 22,273  | 7.8  |
| 4          | 0.66  | 22,289  | 4.1  | 0.08  | 22,283  | 3.5  | 0.4   | 22,284  | 13.1 | 0.4   | 22,266  | 7.8  | 0.4   | 22,291  | 9.9  |
| 5          | 0.68  | 22,300  | 2.7  | 0.10  | 22,256  | 2.3  | 0.5   | 22,365  | 14.0 | 0.5   | 22,270  | 6.9  | 0.5   | 22,270  | 7.0  |
| 6          | 0.70  | 22,302  | 3.9  | 0.12  | 22,266  | 6.8  | 0.6   | 22,293  | 12.9 | 0.6   | 22,304  | 5.6  | 0.6   | 22,290  | 9.3  |
| 7          | 0.72  | 22,299  | 2.3  | 0.14  | 22,278  | 2.5  | 0.7   | 22,276  | 18.1 | 0.7   | 22,301  | 8.9  | 0.7   | 22,268  | 12.3 |
| 8          | 0.74  | 22,284  | 2.7  | 0.16  | 22,284  | 4.7  | 0.8   | 22,302  | 14.6 | 0.8   | 22,280  | 9.5  | 0.8   | 22,280  | 4.5  |
| 9          | 0.76  | 22,270  | 2.4  | 0.18  | 22,295  | 3.4  | 0.9   | 22,262  | 3.9  | 0.9   | 22,261  | 9.2  | 0.9   | 22,311  | 14.9 |
| 10         | 0.78  | 22,266  | 2.1  | 0.20  | 22,289  | 2.9  | 1.0   | 22,274  | 10.2 | 1.0   | 22,262  | 7.2  | 1.0   | 22,280  | 4.7  |
| 11         | 0.80  | 22,256  | 1.9  | 0.22  | 22,308  | 3.5  | 1.1   | 22,318  | 13.1 | 1.1   | 22,268  | 7.8  | 1.1   | 22,262  | 12.9 |
| 12         | 0.82  | 22,263  | 2.5  | 0.24  | 22,279  | 2.8  | 1.2   | 22,325  | 15.6 | 1.2   | 22,262  | 11.1 | 1.2   | 22,260  | 9.8  |
| 13         | 0.84  | 22,266  | 2.4  | 0.26  | 22,281  | 3.6  | 1.3   | 22,330  | 14.2 | 1.3   | 22,301  | 13.3 | 1.3   | 22,261  | 6.3  |
| 14         | 0.86  | 22,272  | 2.6  | 0.28  | 22,299  | 3.1  | 1.4   | 22,308  | 6.7  | 1.4   | 22,279  | 11.0 | 1.4   | 22,305  | 5.9  |
| 15         | 0.88  | 22,283  | 3.3  | 0.30  | 22,285  | 2.8  | 1.5   | 22,266  | 10.7 | 1.5   | 22,266  | 9.4  | 1.5   | 22,316  | 5.7  |
| 16         | 0.90  | 22,283  | 5.1  | 0.32  | 22,294  | 6.1  | 1.6   | 22,280  | 8.8  | 1.6   | 22,304  | 8.7  | 1.6   | 22,266  | 4.5  |
| 17         | 0.92  | 22,282  | 4.8  | 0.34  | 22,288  | 2.6  | 1.7   | 22,285  | 17.4 | 1.7   | 22,297  | 7.4  | 1.7   | 22,298  | 8.7  |
| 18         | 0.94  | 22,278  | 4.8  | 0.36  | 22,269  | 3.1  | 1.8   | 22,316  | 16.1 | 1.8   | 22,308  | 6.0  | 1.8   | 22,275  | 6.9  |
| 19         | 0.96  | 22,273  | 4.3  | 0.38  | 22,267  | 4.1  | 1.9   | 22,324  | 6.7  | 1.9   | 22,259  | 5.7  | 1.9   | 22,311  | 5.5  |
| 20         | 0.98  | 22,287  | 3.6  | 0.40  | 22,273  | 2.8  | 2.0   | 22,271  | 8.2  | 2.0   | 22,256  | 4.5  | 2.0   | 22,259  | 4.0  |

Table 12

Parameters used in the proposed GA-PSO algorithm.

| Parameters used in the algorithm   |                                                                                                                                                                                         |                                                                                           |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| GA PSO                             | Population size: G = 100 Number of worst-fit chromosomes: GG = 25 Selection probability: sp = 0.9 Personal coefficient: pc = 2 Constant coefficient: c 1 = 1.5 Inertia weight: iw = 0.9 | Mutation probability: mp = 0.1 Crossover probability: cp = 0.8 Global coefficient: gc = 2 |
| Other                              | Coefficient of geographic                                                                                                                                                               | Constant coefficient:: c 2 = 1.5                                                          |
| parameters                         | distances: ℘ 1 = 0 . 8 Maximum number of iteration : Maxgen Internal re-optimization runs: R 1 = 25                                                                                     | Coefficient of time distances ℘ 2 = 0 . 2 = 200 , R 2 = 50                                |

constructed between the DC and the PC due to clustering results. In addition, the coordination of delivery and pickup services requires that vehicles must depart from a DC and eventually return to a PC.

A hypothesis of the optimization results shown in Table 13 is that three DCs and three PCs form a collaborative alliance with six members. However, three DCs and three PCs can form collaborative alliances with different members, and the total number of those alliances is 63. For example, the number of collaborative alliances that can be formed by five different members is six, which are {DC1, DC2, DC3, PC1, and PC2}, {DC1, DC2, DC3, PC1, and PC3}, {DC1, DC2, DC3, PC2, and PC3}, {DC1, DC2, PC1, PC2, and PC3}, {DC1, DC3, PC1, PC2, and PC3}, and {DC2, DC3, PC1, PC2, and PC3}. The differences in the alliance members and scale affect the sharing of resources, customers, and facility capabilities (Blundell et al., 2014; Wang et al., 2018a). Therefore, the optimization results of those 63 collaborative alliances are calculated and shown in Table 14 to analyze and discuss the alliances that are more conducive to optimizing the logistics network.

In Table 14, a significant trend is that the gap between the initial and the optimized logistics networks has gradually become apparent, especially in cost savings. For example, in alliance {DC1, DC2, DC3, PC1, PC2, and PC3}, the costs in the initial and optimized logistics network are $55,650 and $22,256, respectively. This result means that the logistics operating cost of that alliance can be reduced to $33,394 through collaboration  and  transportation resource sharing  strategies.  In  addition, an obvious trend is that the cost gap of the logistics network also increases with the rise in the number of members in the alliance. For example, if PC2 participates in alliance {DC1, DC2, DC3, PC1}, then the cost gap grows from $17,699 to $23,062, signifying that the participation of PC2 stimulates the optimization of alliance profits. The sequence in which those members join the alliance seems to affect on the cost gap of the alliance from the perspective of the members and cost savings in these 63 alliances.

## 6.5. Profit allocation mechanism application and sequential alliance selection

A fair profit allocation mechanism can be regarded as the basis of the CMVRPTWMDP optimization (Wang et al., 2017; Wang et al., 2021), and fair allocation of profits among logistics facilities is beneficial to minimize  the  logistics  operating  cost  and  enhance  the  stability  of collaborative logistics networks (Guajardo and Ronnqvist, 2016; Wang ¨ et al., 2018b). Compared with noncollaboration, if the profits and logistics cost can be optimized by collaboration, then logistics facilities can spontaneously participate  in collaboration  and  actively  maintain collaboration  stability  (Long,  2016a;  Long,  2016b;  Rosenthal,  2017). Different collaborative alliances based on profit allocation mechanisms lead  to  different  optimization  schemes,  including  optimized  logistics operating costs, number of semitrailer trucks, and vehicles. However, the SMP and sequential alliance selection strategy based on a fair profit allocation mechanism  can  determine the collaborative alliance s ' optimal sequences and the stability. Thus, the cost gap of alliance {DC1, DC2, DC3, PC1, PC2, and PC3} is adopted to analyze and determine which profit allocation mechanism is appropriate for this study. MCRS, GQP,  Weak  Least  Core,  Shapley,  Proportional  Least  Core,  and  Core Center (Maschler et al., 1979; Sadegh and Kerachian, 2011; Liu and Sui, 2016), are adopted to calculate the profit allocation scheme of collaborative alliance {DC1, DC2, DC3, PC1, PC2, and PC3}. The comparison results of the profit allocation mechanisms and core center are shown in Table 15 and Fig. 10.

Table 15 and Fig. 10 demonstrate the profit allocation schemes of alliance {DC1, DC2, DC3, PC1, PC2, and PC3} with five different profit allocation  mechanisms.  The  distances  between  the  five  methods  and core center are 572, 1253, 976, 651, and 2814. This result indicates that the profit allocation schemes calculated by MCRS are closer to the core center than the other four methods. The MCRS is selected to allocate the profits based on the snowball principle (Cruijssen et al., 2010; Fernandez ´ et al., 2018; Wang et al., 2021). The profit allocation schemes of the 63 alliances based on MCRS are listed in Table 14.

Two  features  can  be  detected  according  to  the  profit  allocation schemes of 63 alliances shown in Table 14. The first feature is the profit

Y. Wang et al.

Fig. 8. Clustering results of the CMVRPTWMDP logistics network with IDKCA.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000010_c37b9c8dc6845c2e61115815e926630fc5e77244f01a08795950d312e763e411.png)

allocated  to  an  assigned  member  in  each  alliance.  For  example,  the profits  allocated to DC1 in alliances {DC1, DC2, DC3, and PC1} and {DC1, DC2, DC3, and PC2} are $4464 and $3985, respectively, which means that DC1 should join alliance {DC1, DC2, DC3, and PC1}. The second feature is the inability of certain members to join in some alliances successfully. For example, if DC1 participates in alliance {DC2, DC3, PC1, PC2, and PC3}, then the profits allocated to PC3 are reduced from  $5713  to  $5496;  thus,  the  participation  of  DC1  is  limited.  To determine the alliance that is more beneficial for these six facilities and whether the major alliance {DC1, DC2, DC3, PC1, PC2, and PC3} can be formed,  the  SMP  is  adopted  to  analyze  these  63  alliances.  The  cost reduction rate of each member in these 63 alliances is calculated according to Eq. (42). The feasible sequences of members participating in alliance {DC1, DC2, DC3, PC1, PC2, and PC3} are found and shown in Table 16, and the total number of those feasible sequences is 14.

In Table 16, the first members of these sequences mainly include DC1, DC2, PC1, and PC2. The numerical value shown in Table 16 represents the cost saving rate of the new member if this member joins the

Table 13 Optimized vehicle routes after collaboration.

| Cluster   | Original   | Vehicles   | Routes                                                                                           | Destination   |
|-----------|------------|------------|--------------------------------------------------------------------------------------------------|---------------|
| 1         | DC1        | V1         | DC1 → C7 → C101* → C135 → C2 → C132 → C17 → C33 → C3 → PC2                                       | PC2           |
|           |            | V2         | DC1 → C18 → C21 → C104* → C19 → C14 → C59 → C202 → C201 → C57 → C22 → C62 → C20 → C24 → C1 → PC2 |               |
|           |            | V3         | DC1 → C103* → C26 → C50 → C48 → C49 → C117* → C13 → C37 → C12 → C25 → C51 → C56 → C11 → PC2      |               |
|           |            | V4         | DC1 → C43 → C40 → C121 → C23 → C4 → C45 → C15 → C10 → C5 → C60 → C9 → PC2                        |               |
| 2         | DC2        | V5         | DC2 → C93* → C120* → C94* → C155 → C127 → C95* → C123 → C96* → C140 → C63 → C27 → C47 → PC1      | PC1           |
|           |            | V6         | DC2 → C146 → C130 → C98* → C122 → C131 → C128 → C126 → C149 → C139 → C125 → C166 → C148 → PC1    |               |
|           |            | V7         | DC2 → C133 → C137 → C134 → C142 → C147 → C173 → C145 → C136 → C129 → C143 → C124 → C141 → PC1    |               |
|           |            | V8         | DC2 → C107* → C152 → C164 → C138 → C105* → C100* → PC1                                           |               |
| 3         | DC2        | V9         | DC2 → C150 → C8 → C144 → C111* → C35 → C156 → C69 → C99* → C106* → C109* → PC2                   | PC2           |
|           |            | V10        | DC2 → C159 → C58 → C29 → C6 → C16 → C55 → C153 → C44 → C112* → C161 → C110* → C162 → PC2         |               |
| 4         | DC2        | V11        | DC2 → C170 → C189 → C182 → C196 → C172 → C160 → C200 → C158 → C167 → C168 → C171 → PC3           | PC3           |
|           |            | V12        | DC2 → C119* → C115* → C157 → C188 → C184 → C154 → C163 → C194 → C176 → C199 → C181 → PC3         |               |
|           |            | V13        | DC2 → C210 → C113* → C102* → C192 → C75 → C177 → C198 → C118* → C193 → C178 → C209 → PC3         |               |
|           |            | V14        | DC2 → C108 → C116* → C180 → C179 → C186 → C185 → C183 → PC3                                      |               |
| 5         | DC3        | V15        | DC3 → C34 → C53 → C32 → C28 → C39 → C68 → C92 → C36 → C114* → C151 → C97* → C174 → PC2           | PC2           |
|           |            | V16        | DC3 → C76 → C79 → C82 → C64 → C66 → C207 → C206 → C46 → C52 → C74 → C80 → C208 → PC2             |               |
|           |            | V17        | DC3 → C87 → C61 → C38 → C30 → C88 → C204 → C203 → C54 → C31 → PC2                                |               |
|           |            | V18        | DC3 → C70 → C175 → C187 → C195 → C84 → C67 → C42 → C169 → C72 → PC2                              |               |
|           |            | V19        | DC3 → C90 → C190 → C89 → C91 → C71 → C197 → C191 → C77 → C73 → C41 → C86 → PC2                   |               |
|           |            | V20        | DC3 → C81 → C78 → C85 → C205 → C83 → C65 → DC3                                                   | DC3           |

- *: Customer with delivery and pickup demands.

Y. Wang et al.

Table 14 Optimization results comparison of 63 collaborative alliances.

| Alliance S and its members                  | Initial       | Initial   | Optimized     | Optimized   | GAP           | GAP   | Profit allocation Schemes                                     |
|---------------------------------------------|---------------|-----------|---------------|-------------|---------------|-------|---------------------------------------------------------------|
| Alliance S and its members                  | TC            | Nv        | TC            | Nv          | TC            | Nv    | Profit allocation Schemes                                     |
| {DC1}                                       | 7475          | 5         | 6530          | 3           | 945           | 2     | {945; -; -; -; -; -}                                          |
| {DC2}                                       | 7848          | 5         | 7590          | 5           | 258           | 0     | {-; 258; -; -; -; -}                                          |
| {DC3}                                       | 8570          | 6 6       | 7446 7679     | 4 4         | 1124          | 2     | {-; -; 1124; -; -; -}                                         |
| {PC1}                                       | 8788          |           | 11,564        | 6           | 1109          | 2     | {-; -; -; 1109; -; -}                                         |
| {PC2}                                       | 11,752        | 6         |               |             | 188           | 0     | {-; -; -; -; 188; -}                                          |
| {PC3} {DC1,DC2}                             | 10,844        | 6         | 9834          | 4           | 1010          | 2     | {-; -; -; -; -; 1010}                                         |
|                                             | 15,323 16,045 | 10 11     | 8733 9332     | 10 10       | 6590 6713     | 0 1   | {2986; 2299; -; -; -; -}                                      |
| {DC1,DC3}                                   |               |           |               |             |               |       | {2588; -; 2767; -; -; -}                                      |
| {DC1,PC1}                                   | 16,263        | 11        | 8269          | 8           | 7994          | 3     | {3173; -; -; 3337; -; -}                                      |
| {DC1,PC2}                                   | 19,227        | 11        | 10,369        | 8           | 8858          | 3     | {3416; -; -; -; 2659; -}                                      |
| {DC1,PC3}                                   | 18,319        | 11        | 12,148        | 10          | 6171          | 1     | {1737; -; -; -; -; 1802}                                      |
| {DC2,DC3}                                   | 16,418        | 11        | 7838          | 8           | 8580          | 3     | {-; 3173; 4039; -; -; -}                                      |
| {DC2,PC1}                                   | 16,636        | 11        | 7922          | 8           | 8714          | 3     | {-; 3184; -; 4035; -; -}                                      |
| {DC2,PC2}                                   | 19,600        | 11        | 11,295        | 9           | 8305          | 2     | {-; 2791; -; -; 2721; -}                                      |
| {DC2,PC3}                                   | 18,692        | 11        | 10,837        | 9           | 7855          | 2     | {-; 2230; -; -; -; 2982}                                      |
| {DC3,PC1}                                   | 17,358        | 12        | 12,446        | 10          | 4912          | 2     | {-; -; 1690; 1675; -; -}                                      |
| {DC3,PC2}                                   | 20,322        | 12        | 10,878        | 9           | 9444          | 3     | {-; -; 3767; -; 2831; -}                                      |
| {DC3,PC3}                                   | 19,414        | 12        | 10,881        | 9           | 8533          | 3     | {-; -; 2976; -; -; 2862}                                      |
| {PC1,PC2}                                   | 20,540        | 12        | 11,850        | 9           | 8690          | 3     | {-; -; -; 3320; 2399; -}                                      |
| {PC1,PC3}                                   | 19,632        | 12        | 11,445        | 9           | 8187          | 3     | {-; -; -; 2733; -; 2634}                                      |
| {PC2,PC3}                                   | 22,596        | 12        | 13,920        | 9           | 8676          | 3     | {-; -; -; -; 1867; 2689}                                      |
| {DC1,DC2,DC3}                               | 23,893        | 16        | 11,218        | 13          | 12,675        | 3     | {3304; 4531; 4840; -; -; -}                                   |
| {DC1,DC2,PC1}                               | 24,111        | 16        | 10,595        | 10          |               | 6     | {3838; 4207; -; 5472; -; -}                                   |
| {DC1,DC2,PC2}                               | 27,075        | 16        | 13,681        | 11          | 13,516 13,394 | 5     | {4253; 3672; -; -; 5469; -}                                   |
| {DC1,DC2,PC3} {DC1,DC3,PC1}                 | 26,167        | 16        | 14,014        | 12          | 12,153        | 4     | {3390; 4433; -; -; -; 4330}                                   |
|                                             | 24,833        | 17        | 12,938        | 14          | 11,895        | 3     | {2845; -; 4740; 4310; -; -}                                   |
| {DC1,DC3,PC2}                               | 27,797        | 17        | 14,720        | 13          | 13,077        | 4     | {3377; -; 3924; -; 5776; -}                                   |
| {DC1,DC3,PC3}                               | 26,889        | 17        | 14,757        | 13          | 12,132        | 4     | {2964; -; 4804; -; -; 4364}                                   |
| {DC1,PC1,PC2}                               | 28,015        | 17        | 13,796        | 11          | 14,219        | 6     | {4636; -; -; 4533; 5050; -}                                   |
| {DC1,PC1,PC3}                               | 27,107        | 17        | 14,816        | 13          | 12,291        | 4     | {3489; -; -; 5145; -; 3657}                                   |
| {DC1,PC2,PC3}                               | 30,071        | 17        | 16,548        | 12          | 13,523        | 5     | {3962; -; -; -; 5726; 3835}                                   |
| {DC2,DC3,PC1}                               | 25,206        | 16        | 12,565        | 14          | 12,641        | 2     | {-; 5992; 3275; 3374; -; -}                                   |
| {DC2,DC3,PC2}                               | 28,170        | 16        | 14,569        | 13          | 13,601        | 3     | {-; 3893; 5014; -; 4694; -}                                   |
| {DC2,DC3,PC3}                               | 27,262        | 16        | 13,891        | 12          | 13,371        | 4     | {-; 4201; 4905; -; -; 4265}                                   |
| {DC2,PC1,PC2}                               | 28,388        | 17        | 14,196        | 12          | 14,192        | 5     | {-; 4586; -; 5052; 4553; -}                                   |
| {DC2,PC1,PC3}                               | 27,480        | 17        | 13,582        | 11          | 13,898        | 6 5   | {-; 4573; -; 5013; -; 4313}                                   |
| {DC2,PC2,PC3} {DC3,PC1,PC2}                 | 30,444        | 17 17     | 16,632 14,835 | 12 13       | 13,812 14,275 | 4     | {-; 4238 ; -; -; 4894; 4679} {-; -; 4170; 3651; 6454; -}      |
| {DC3,PC1,PC3}                               | 29,110 28,202 | 17        | 14,582        | 12          | 13,620        | 5     | {-; -; 3921; 3691; -; 6007}                                   |
| {DC3,PC2,PC3}                               | 31,166        | 17        | 17,326        | 13          | 13,840        | 4     | {-; -; 4833; -; 4888; 4119}                                   |
| {PC1,PC2,PC3} {DC1,DC2,DC3,PC1}             | 31,384        | 18        | 16,923        | 12          | 14,461        | 6     | {-; -; -; 4770; 4953; 4738} {4464; 5013; 3734; 4458; -; -}    |
| {DC1,DC2,DC3,PC2}                           | 32,681        | 22        | 15,012        | 16          | 17,669        | 6     |                                                               |
|                                             | 35,645        | 22        | 18,052        | 17          | 17,593        | 5     | {3985; 4507; 4192; -; 4908; -} {3464; 4649; 4652; -; -; 4142} |
| {DC1,DC2,DC3,PC3}                           | 34,737        | 22        | 17,829        | 17          | 16,908        | 5     | -}                                                            |
| {DC1,DC2,PC1,PC2}                           | 35,863        | 22        | 15,364        | 11          | 20,499        | 11    | {4938; 4741; -; 5573; 5247;                                   |
| {DC1,DC2,PC1,PC3} {DC1,DC2,PC2,PC3}         | 34,955        | 22        | 17,093        | 15          | 17,862        | 7     | {3643; 5007; -; 5220; -; 3991} {3705; 4045; -; -; 5489; 4141} |
| {DC1,DC3,PC1,PC2}                           | 37,919 36,958 | 22        | 20,539 19,274 | 16 17       | 17,380 17,684 | 6     |                                                               |
|                                             |               | 23        |               |             |               | 6     | {3483; -; 3534; 4711; 5956; -} {4263; -; 5146; 5237; -;       |
| {DC1,DC3,PC1,PC3}                           | 36,050        | 23        | 16,061        | 16          | 19,989        | 7     | 5344} {4170; -; 4459; -; 5387; 4794}                          |
| {DC1,DC3,PC2,PC3} {DC1,PC1,PC2,PC3}         | 39,014 39,232 | 23 23     | 20,204 20,055 | 16 15       | 18,810 19,177 | 7     | {4112; -; -; 4926; 5814; 4325}                                |
|                                             |               |           |               |             |               | 8     |                                                               |
| {DC2,DC3,PC1,PC2} {DC2,DC3,PC1,PC3}         | 36,958 36,050 | 23 23     | 18,614 16,910 | 17 15       | 18,344        | 6 8   | {-; 3992; 4091; 4669; 5591; -}                                |
|                                             |               |           |               |             | 19,140        |       | {-; 4472; 4421; 4841; -; 5405} {-; 4564; 4710; -; 4958;       |
| {DC2,DC3,PC2,PC3} {DC2,PC1,PC2,PC3}         | 39,014 39,232 | 23 23     | 19,907 19,984 | 15 15       | 19,107 19,248 | 8 8   | 4875} {-; 4441; -; 5105; 4955; 4747}                          |
| {DC3,PC1,PC2,PC3}                           | 39,954        | 24        | 20,280        | 15 20       | 19,674        | 9     | {-; -; 4607; 5134; 5185; 4749} {4996; 5325; 3413; 5603; 5320; |
| {DC1,DC2,DC3,PC1,PC2} {DC1,DC2,DC3,PC1,PC3} | 44,806 43,898 | 28 28     | 21,744 22,437 |             | 23,062        | 8 4   | -} {3449; 2569; 4931; 6065; -;5181}                           |
|                                             |               |           |               | 24          | 21,461        |       | {4146; 4166; 5436; -;5511; 5251}                              |
| {DC1,DC2,DC3,PC2,PC3}                       | 46,862        | 28        | 23,929        | 20          | 22,933        | 8 8   | {4689; 4609; -;6218; 5649; 3698}                              |
| {DC1,DC2,PC1,PC2,PC3} {DC1,DC3,PC1,PC2,PC3} | 47,080 47,802 | 28        | 23,801        | 20          | 23,279        |       | {3908; -;4459; 4891; 3668; 6225}                              |
|                                             |               | 29        | 26,227        | 23          | 21,575        | 6     |                                                               |
| {DC2,DC3,PC1,PC2,PC3}                       | 47,802        |           |               | 20          | 23,662        | 9     | {-; 4502; 5019; 5128; 4912; 5713}                             |
| {DC1,DC2,DC3,PC1,PC2,PC3}                   |               | 29        | 24,140        |             |               |       | {5174; 5822; 5451; 5610; 5841; 5496}                          |
|                                             | 55,650        | 34        | 22,256        | 20          | 33,394        | 14    |                                                               |

alliance. For example, DC1 is the first member in sequence π 1 , and the cost reduction rate of PC1 is 37.97%, if PC1 is regarded as the second member joining alliance {DC1, DC2, DC3, PC1, PC2, and PC3}. The cost reduction rate of DC2 is 53.60% if DC2 is regarded as the third member participating in that alliance. In order to find the optimal sequence, the diagonal  principle  is  adopted  (Cruijssen  et  al.,  2010;  Wang  et  al., 2018b), and π 1 and π 8 are regarded as examples. The specific usage of the diagonal principle is as follows. First, the cost reduction rates in sequences π 1 and π 8 are ranked in ascending order. Then, the minimum value of the two sequences is compared, and the sequence with the more significant minimum value is superior. If the minimum values of the two sequences are equal, then the second minimum value of the two sequences needs to be compared and select the better sequence. Therefore, π 1  is  better  than π 8  based  on  the  diagonal  rule.  According  to  this

Y. Wang et al.

Fig. 9. The optimized vehicle routes of V1, V8, V9, V14, V17, and V20.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000011_027970b3654f489a1003bfed099339ef71f01874d40b55b6694e079cdec119e4.png)

Table 15

Comparison of the profit allocation mechanisms and core center.

| Methods                 | Schemes {DC1; DC2; DC3; PC1; PC2; PC3}   | Core center                          |   Distance |
|-------------------------|------------------------------------------|--------------------------------------|------------|
| MCRS                    | {5174; 5822; 5451; 5610; 5841; 5496}     | {5047; 6129; 5246; 5425; 6189; 5358} |        572 |
| GQP                     | {4141; 589; 4907; 5599; 6817; 5341}      |                                      |       1253 |
| Weak Least Core         | {5225; 5912; 5046; 5061; 5982; 6170}     |                                      |        976 |
| Shapley                 | {5078; 5696; 5412; 5817; 5966; 5425}     |                                      |        651 |
| Proportional Least Core | {4569; 4564; 5934; 7412; 6428; 4487}     |                                      |       2814 |

method,  the  optimal  sequence  is π 1  without  considering  the  first member of the alliance, and the change process of each member s cost ' reduction rate is shown in Fig. 11.

Fig. 10. Comparison  of  the  distance  between  different  profit  allocation mechanisms and core center.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000012_f67568ebd356de1f04c7a291b4600943b2f1aac4b773a50a8199d6a5c46627bd.png)

In  Fig.  11,  the  cost  reduction  rate  of  each  member  has  changed during the formation of alliance {DC1, DC2, DC3, PC1, PC2, and PC3}. The cost reduction rate of DC1, as the first member, has been affected many times. The participation of PC1 has directly increased the cost reduction rate of DC1 from 12.64% to 42.45%, which is a substantial effect for DC1. In addition, the cost reduction rate of each member is increasing  even  if  the  growth  is  slow.  The  members  will  not  easily withdraw from this alliance because forming a large alliance with this optimal sequence is more conducive to these six members to optimize their  logistics  operating  costs  compared  with  small  alliances.  Thus, forming alliance {DC1, DC2, DC3, PC1, PC2, and PC3} with the optimal sequence π 1 = {DC1,  PC1,  DC2,  PC2,  DC3,  and  PC3}  is  the  most appropriate and beneficial approach for these six logistics facilities to collaboratively optimize their logistics operating costs.

## 6.6. Analysis and discussion

In designing a multicenter logistics network with mixed deliveries and pickups, how to build a collaborative alliance and optimize the logistics network based on collaboration and resource sharing strategies is a  crucial  issue.  Based  on  customer  demands  and  collaboration  and resource sharing strategies, the collaborative alliance formed by three DCs and three PCs has five possible cases. A significant difference among these five cases is the level to which the mixed delivery and pickup services are coordinated. The definitions and explanations of these cases are shown in Table 17.

In Table 17, the number of collaborative alliances in those five cases varies. In Case 1, these six logistics facilities independently operate. In Case 5, these six logistics facilities form a sizable collaborative alliance: customer information, transportation resources, and facility capabilities in Case 5 are shared by these six facilities. This notion means vehicles can perform mixed delivery and pickup services in Case 5. In addition, the collaborative relationship between facilities allows customers in the 2nd type to be visited only once in Case 5 instead of multiple times in Cases 1, 2, 3, and 4. The optimization results in those five cases are

Y. Wang et al.

GLYPH&lt;0&gt;↔GLYPH&lt;0&gt;𝒮GLYPH&lt;0&gt;𝒮GLYPH&lt;0&gt;ℴGLYPH&lt;0&gt;ℒ

Table 16 Possible sequences for the grand collaborative alliance.

| π 1 = {DC1, PC1, DC2, PC2, Player i   | DC3, PC3} DC1                        | PC1                                  | DC2                                  | PC2                                  | DC3                                  | PC3                                  |
|---------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|
| cost reduction rate(%)                | 12.6                                 | 38.0                                 | 53.6                                 | 44.7                                 | 39.8                                 | 50.7                                 |
| π 2 = {DC1, DC2, PC1, PC2, DC3, PC3}  | π 2 = {DC1, DC2, PC1, PC2, DC3, PC3} | π 2 = {DC1, DC2, PC1, PC2, DC3, PC3} | π 2 = {DC1, DC2, PC1, PC2, DC3, PC3} | π 2 = {DC1, DC2, PC1, PC2, DC3, PC3} | π 2 = {DC1, DC2, PC1, PC2, DC3, PC3} | π 2 = {DC1, DC2, PC1, PC2, DC3, PC3} |
| Player i                              | DC1                                  | DC2                                  | PC1                                  | PC2                                  | DC3                                  | PC3                                  |
| cost reduction rate(%)                | 12.6                                 | 29.3                                 | 53.6                                 | 44.7                                 | 39.8                                 | 50.7                                 |
| π 3 = {DC1, PC1, PC2, DC2, DC3, PC3}  | π 3 = {DC1, PC1, PC2, DC2, DC3, PC3} | π 3 = {DC1, PC1, PC2, DC2, DC3, PC3} | π 3 = {DC1, PC1, PC2, DC2, DC3, PC3} | π 3 = {DC1, PC1, PC2, DC2, DC3, PC3} | π 3 = {DC1, PC1, PC2, DC2, DC3, PC3} | π 3 = {DC1, PC1, PC2, DC2, DC3, PC3} |
| Player i                              | DC1                                  | PC1                                  | PC2                                  | DC2                                  | DC3                                  | PC3                                  |
| cost reduction rate(%)                | 12.6                                 | 38.0                                 | 43.0                                 | 60.4                                 | 40.0                                 | 50.7                                 |
| π 4 = {DC1, PC2, PC1, DC2, DC3, PC3}  | π 4 = {DC1, PC2, PC1, DC2, DC3, PC3} | π 4 = {DC1, PC2, PC1, DC2, DC3, PC3} | π 4 = {DC1, PC2, PC1, DC2, DC3, PC3} | π 4 = {DC1, PC2, PC1, DC2, DC3, PC3} | π 4 = {DC1, PC2, PC1, DC2, DC3, PC3} | π 4 = {DC1, PC2, PC1, DC2, DC3, PC3} |
| Player i                              | DC1                                  | PC2                                  | PC1                                  | DC2                                  | DC3                                  | PC3                                  |
| cost reduction rate(%)                | 12.6                                 | 22.6                                 | 51.6                                 | 60.4                                 | 39.8                                 | 50.7                                 |
| π 5 = {DC2, DC1, PC1, PC2, DC3, PC3}  | π 5 = {DC2, DC1, PC1, PC2, DC3, PC3} | π 5 = {DC2, DC1, PC1, PC2, DC3, PC3} | π 5 = {DC2, DC1, PC1, PC2, DC3, PC3} | π 5 = {DC2, DC1, PC1, PC2, DC3, PC3} | π 5 = {DC2, DC1, PC1, PC2, DC3, PC3} | π 5 = {DC2, DC1, PC1, PC2, DC3, PC3} |
| Player i                              | DC2                                  | DC1                                  | PC1                                  | PC2                                  | DC3                                  | PC3                                  |
| cost reduction rate(%)                | 3.3                                  | 39.9                                 | 62.3                                 | 44.7                                 | 39.8                                 | 50.7                                 |
| π 6 = {DC2, PC1, DC1, PC2, DC3, PC3}  | π 6 = {DC2, PC1, DC1, PC2, DC3, PC3} | π 6 = {DC2, PC1, DC1, PC2, DC3, PC3} | π 6 = {DC2, PC1, DC1, PC2, DC3, PC3} | π 6 = {DC2, PC1, DC1, PC2, DC3, PC3} | π 6 = {DC2, PC1, DC1, PC2, DC3, PC3} | π 6 = {DC2, PC1, DC1, PC2, DC3, PC3} |
| Player i                              | DC2                                  | PC1                                  | DC1                                  | PC2                                  | DC3                                  | PC3                                  |
| cost reduction rate(%)                | 3.3                                  | 46.0                                 | 51.3                                 | 44.7                                 | 39.8                                 | 50.7                                 |
| π 7 = {DC2, PC1, PC2, DC1, DC3, PC3}  | π 7 = {DC2, PC1, PC2, DC1, DC3, PC3} | π 7 = {DC2, PC1, PC2, DC1, DC3, PC3} | π 7 = {DC2, PC1, PC2, DC1, DC3, PC3} | π 7 = {DC2, PC1, PC2, DC1, DC3, PC3} | π 7 = {DC2, PC1, PC2, DC1, DC3, PC3} | π 7 = {DC2, PC1, PC2, DC1, DC3, PC3} |
| Player i                              | DC2                                  | PC1                                  | PC2                                  | DC1                                  | DC3                                  | PC3                                  |
| cost reduction rate(%)                | 3.3                                  | 45.9                                 | 38.8                                 | 66.1                                 | 39.8                                 | 50.7                                 |

Fig. 11. Diagram of the percentage of cost reduction in the formation of π 1.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000013_425a8d4d9324669a082611d5b958f47409202feba1a0bb45786fb0bc1e07e4ed.png)

shown in Table 18 and Fig. 12, including the total transportation cost (TTC), PCO, CC, FC, GI, Nv, the total maintenance cost of vehicles and semitrailer trucks (TMC), and TC.

In Table 18 and Fig. 12, the significant changes of the five critical indicators prove that Case 5 outperforms the other cases. First, the TC values in these five cases are $50,643, $40,295, $38,489, $28,141, and $22,256, which are all lower than the initial TC of the logistics network ($55,650); and the TC in Case 5 is the lowest. Second, the gaps between the cases in terms of TTC are also apparent. For example, the other four cases have better optimized TTC than Case 1, and Case 5 has the minimal TTC. Third, the Nv and TMC can also justify the finding. The number of vehicles spared in these five cases are 8, 7, 10, 9, and 14, contributing to reduced logistics facility maintenance costs. The reduction in vehicles means that more vehicles are used for mixed delivery and pickup services in Case 5. Fourth, the penalty cost is avoided in Case 5. Accordingly, the structure of collaborative alliance in Case 5 leads to the best performance of the logistics. To determine the case that is preferred by

π

8

=

{DC2, PC2, PC1, DC1, DC3, PC3}

| Player i                              | DC2                                   | PC2                                   | PC1                                   | DC1                                   | DC3                                   | PC3                                   |
|---------------------------------------|---------------------------------------|---------------------------------------|---------------------------------------|---------------------------------------|---------------------------------------|---------------------------------------|
| cost reduction rate(%)                | 3.3                                   | 23.2                                  | 57.5                                  | 66.1                                  | 39.9                                  | 50.7                                  |
| π 9 = {PC1, DC1, DC2, PC2, DC3, PC3}  | π 9 = {PC1, DC1, DC2, PC2, DC3, PC3}  | π 9 = {PC1, DC1, DC2, PC2, DC3, PC3}  | π 9 = {PC1, DC1, DC2, PC2, DC3, PC3}  | π 9 = {PC1, DC1, DC2, PC2, DC3, PC3}  | π 9 = {PC1, DC1, DC2, PC2, DC3, PC3}  | π 9 = {PC1, DC1, DC2, PC2, DC3, PC3}  |
| Player i                              | PC1                                   | DC1                                   | DC2                                   | PC2                                   | DC3                                   | PC3                                   |
| cost reduction rate(%)                | 12.6                                  | 42.5                                  | 53.6                                  | 44.7                                  | 39.8                                  | 50.7                                  |
| π 10 = {PC1, DC1, PC2, DC2, DC3, PC3} | π 10 = {PC1, DC1, PC2, DC2, DC3, PC3} | π 10 = {PC1, DC1, PC2, DC2, DC3, PC3} | π 10 = {PC1, DC1, PC2, DC2, DC3, PC3} | π 10 = {PC1, DC1, PC2, DC2, DC3, PC3} | π 10 = {PC1, DC1, PC2, DC2, DC3, PC3} | π 10 = {PC1, DC1, PC2, DC2, DC3, PC3} |
| Player i                              | PC1                                   | DC1                                   | PC2                                   | DC2                                   | DC3                                   | PC3                                   |
| cost reduction rate(%)                | 12.6                                  | 42.5                                  | 43.0                                  | 60.4                                  | 39.8                                  | 50.7                                  |
| π 11 = {PC1, DC2, DC1, PC2, DC3, PC3} | π 11 = {PC1, DC2, DC1, PC2, DC3, PC3} | π 11 = {PC1, DC2, DC1, PC2, DC3, PC3} | π 11 = {PC1, DC2, DC1, PC2, DC3, PC3} | π 11 = {PC1, DC2, DC1, PC2, DC3, PC3} | π 11 = {PC1, DC2, DC1, PC2, DC3, PC3} | π 11 = {PC1, DC2, DC1, PC2, DC3, PC3} |
| Player i                              | PC1                                   | DC2                                   | DC1                                   | PC2                                   | DC3                                   | PC3                                   |
| cost reduction rate(%)                | 12.6                                  | 40.6                                  | 51.3                                  | 44.7                                  | 39.8                                  | 50.7                                  |
| π 12 = {PC1, DC2, PC2, DC1, DC3, PC3} | π 12 = {PC1, DC2, PC2, DC1, DC3, PC3} | π 12 = {PC1, DC2, PC2, DC1, DC3, PC3} | π 12 = {PC1, DC2, PC2, DC1, DC3, PC3} | π 12 = {PC1, DC2, PC2, DC1, DC3, PC3} | π 12 = {PC1, DC2, PC2, DC1, DC3, PC3} | π 12 = {PC1, DC2, PC2, DC1, DC3, PC3} |
| Player i                              | PC1                                   | DC2                                   | PC2                                   | DC1                                   | DC3                                   | PC3                                   |
| cost reduction rate(%)                | 12.6                                  | 40.6                                  | 38.8                                  | 66.1                                  | 39.8                                  | 50.7                                  |
| π 13 = {PC2, DC1, PC1, DC2, DC3, PC3} | π 13 = {PC2, DC1, PC1, DC2, DC3, PC3} | π 13 = {PC2, DC1, PC1, DC2, DC3, PC3} | π 13 = {PC2, DC1, PC1, DC2, DC3, PC3} | π 13 = {PC2, DC1, PC1, DC2, DC3, PC3} | π 13 = {PC2, DC1, PC1, DC2, DC3, PC3} | π 13 = {PC2, DC1, PC1, DC2, DC3, PC3} |
| Player i                              | PC2                                   | DC1                                   | PC1                                   | DC2                                   | DC3                                   | PC3                                   |
| cost reduction rate(%)                | 1.6                                   | 45.7                                  | 51.6                                  | 60.4                                  | 39.8                                  | 50.7                                  |
| π 14 = {PC2, DC2, PC1, DC1, DC3, PC3} | π 14 = {PC2, DC2, PC1, DC1, DC3, PC3} | π 14 = {PC2, DC2, PC1, DC1, DC3, PC3} | π 14 = {PC2, DC2, PC1, DC1, DC3, PC3} | π 14 = {PC2, DC2, PC1, DC1, DC3, PC3} | π 14 = {PC2, DC2, PC1, DC1, DC3, PC3} | π 14 = {PC2, DC2, PC1, DC1, DC3, PC3} |
| Player i                              | PC2                                   | DC2                                   | PC1                                   | DC1                                   | DC3                                   | PC3                                   |
| cost reduction rate(%)                | 1.6                                   | 35.6                                  | 57.5                                  | 66.1                                  | 39.8                                  | 50.7                                  |

Table 17 Description of five cases of collaborative alliances.

| Case     | Structures of collaborative alliances    | Is customer with delivery and pickup demands visited twice?   | Can vehicle perform mixed delivery and pickup activities in one service route?   | Vehicle routes   | Vehicle routes   |
|----------|------------------------------------------|---------------------------------------------------------------|----------------------------------------------------------------------------------|------------------|------------------|
| Case     | Structures of collaborative alliances    | Is customer with delivery and pickup demands visited twice?   | Can vehicle perform mixed delivery and pickup activities in one service route?   | Open?            | Closed?          |
| Case 1   | {DC1}, {DC2}, {DC3}, {PC1}, {PC2}, {PC3} | YES                                                           | NO                                                                               | NO               | YES              |
| Case 2   | {DC1,DC2, DC3}, {PC1}, {PC2}, {PC3}      | YES                                                           | NO                                                                               | NO               | YES              |
| Case 3   | {DC1}, {DC2}, {DC3}, {PC1, PC2,PC3}      | YES                                                           | NO                                                                               | NO               | YES              |
| zzCase 4 | {DC1,DC2, DC3}, {PC1, PC2,PC3}           | YES                                                           | NO                                                                               | NO               | YES              |
| Case 5   | {DC1,DC2, DC3, PC1,PC2, PC3}             | NO                                                            | YES                                                                              | YES              | YES              |

these six logistics facilities, the profit allocation schemes for these five cases, calculated based on MCRS method, are shown in Table 18 and Fig. 13.

In Table 18 and Fig. 13, the cost savings for each facility in Case 5 are $5174,  $5822,  $5451,  $5610,  $5841,  and  $5496,  which  are  significantly higher than those in the other four cases. In Case 1, these six logistics facilities independently operate; thus, their cost savings from the collaboration is the smallest. In Cases 2 and 3, the portions of the logistics facilities collaborated; accordingly, the collaborative logistics facilities  obtained  profits.  It  is  worth  noting  that  although  alliances {DC1, DC2, DC3} and {PC1, PC2, PC3} are formed in Case 4, the delivery and pickup services are not integrated due to the separation of

Y. Wang et al.

Table 18 Comparison of the optimization results in the five cases.

| Cases   | TTC($)   | PCO($)   | CC($)   | FC($)   | GI($)   | Nv   | TMC($)   | TC($)   | Gap   | Gap    | Gap    | Profit allocation                    |
|---------|----------|----------|---------|---------|---------|------|----------|---------|-------|--------|--------|--------------------------------------|
|         |          |          |         |         |         |      |          |         | Nv    | TTC($) | TC($)  | {DC1; DC2; DC3; PC1; PC2; PC3}       |
| Initial | 26,752   | 10,753   | 0       | 5068    | 0       | 34   | 13,077   | 55,650  | -     | -      | -      | -                                    |
| Case 1  | 26,638   | 8937     | 0       | 5068    | 0       | 26   | 10,000   | 50,643  | 8     | 114    | 5007   | {945; 258; 1124; 1109; 188; 1010}    |
| Case 2  | 20,081   | 3987     | 2079    | 5068    | 1785    | 27   | 10,865   | 40,295  | 7     | 6671   | 15,355 | {3304; 4531; 4840; 1109; 188; 1010}  |
| Case 3  | 18,673   | 4722     | 2219    | 5068    | 1905    | 24   | 9712     | 38,489  | 10    | 8079   | 17,161 | {945; 258; 1124; 4770; 4953; 4738}   |
| Case 4  | 9966     | 1922     | 4298    | 5068    | 3690    | 25   | 10,577   | 28,141  | 9     | 16,786 | 27,509 | {3304; 4531; 4840; 4770; 4953; 4738} |
| Case 5  | 8313     | 93       | 4298    | 5068    | 3690    | 20   | 8173     | 22,256  | 14    | 18,919 | 33,394 | {5174; 5822; 5451; 5610; 5841; 5496} |

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000014_0add5fd1eaadabc932e3f683471c194f144658b3ca3ebe292004bcbaf3dbc788.png)

Fig. 12. Comparison of the optimization results in the five cases.

Fig. 13. Comparison of the cost savings of each facility among the five cases.

![Image](Papers_Converted/0_Artifacts/[wang2022]_Collaborative_multicenter_vehicle_routing_problem_with_time_windows_and_mixed_deliveries_and_pickups_artifacts/image_000015_f290b62311eeefa14278ba0a73faebe9fd55870297ab2999e47469eb6c85143f.png)

DCs and PCs. Overall, the six logistics facilities in Case 5 have a higher degree  of  cooperation.  In  Case  5,  these  six  logistics  facilities  form  a single collaborative alliance {DC1, DC2, DC3, PC1, PC2, and PC3}, and customer information, transportation resources, and facility capabilities are fully shared among them. Therefore, Case 5 is a desirable solution for collaboration and resource sharing, which is the most beneficial to the six logistics facilities and the logistics network as a whole.

## 6.7. Management insights

This study promotes collaboration and resource sharing as the main strategies  to  improve  the  efficiency  of  logistics  transportation.  The management  insights  obtained  from  this  study  are  summarized  as

## follows:

- (1)  Collaboration and resource sharing can optimize the allocation of resources and coordinate the mixed delivery and pickup services, thus reducing the logistics operating costs. The proposed model and  methods  contribute  to  optimizing  a  multicenter  logistics network with mixed deliveries and pickups. Customer information and facility capabilities are shared in the optimization of a collaborative multicenter logistics network. This mechanism facilitates the optimization of the allocation of resources and fully uses customer information. The sharing of resources allows the delivery and pickup services to be coordinated to provide customers with high-quality logistics services, and improve transportation efficiency. In comparison with the open -closed mixed vehicle route, collaboration  makes  multiple  facilities more closely  connected, thus  increasing the  stability  of  the  logistics network.
- (2)  Sustainable  and  efficient  development  is  an  urgent  issue  for contemporary logistics companies and local transportation departments.  Emerging  technologies  provide  local  governments and logistics facilities an opportunity to optimize the configuration and utilization of transportation resources continuously. The local governments should design incentive mechanisms to support  the  collaboration  of  logistics  facilities.  Logistics  facilities should actively participate in the collaboration to optimize the logistics  network  and  its  own  benefits.  Emerging  technologies such  as  artificial  intelligence,  blockchain,  and  the  Internet  of Things encourage sharing information and connections among facilities, which lay out the infrastructures for collaboration and resource sharing. Therefore, collaboration and resource sharing allow urban logistics networks to transform into intelligent and efficient logistics systems. This positive feedback greatly accelerates  smart,  efficient  and  sustainable  developments  in  urban logistics and transportation.

## 7. Conclusions

This  work  studies  the  CMVRPTWMDP,  which  accounts  for  three types of customers and optimizes logistics operation through collaboration and resource sharing. Collaboration and resource sharing provide an opportunity to integrate vehicles ' delivery and pickup services and construct  open -closed  mixed  vehicle  routes.  This  work  achieves  the following objectives. First, CMVRPTWMDP is formulated as a mixedinteger  programming  model  that  minimizes  the  total  logistics  operating cost. Second, a two-stage hybrid algorithm composed of IDKCA and GA-PSO is designed to solve the CMVRPTWMDP. Third, the profit allocation mechanism is employed to assess different profit allocation schemes  and  collaborative  alliance  structures,  providing  a  basis  for decision-making  for  the  logistics  facilities that would  potentially collaborate. Finally, The SMP selection strategy is developed to find the optimal sequence of participating in collaborative alliances.

A two-stage hybrid algorithm is designed, which first clusters customers and then optimizes the vehicle routes considering the integration of  vehicle s  delivery  and  pickup  services,  and  the  construction  of '

Y. Wang et al.

open closed mixed vehicle routes. An improved 3D -k -means clustering algorithm is designed to cluster customers based on their geographical coordinates, time windows, and logistics service demands. The intersection of multiple clusters is regarded as a region related to a DC and PC simultaneously. The combination of GA and PSO makes the GA PSO -algorithm robust and improves global search capability. The designed coordination operator between GA and PSO increases the diversity of particles and the probability of finding a feasible solution. In addition, the MCRS profit allocation mechanism has been adopted as the basis for profit allocation.

A real-world logistics network in Chongqing is analyzed to verify the performance  and  applicability  of  the  proposed  methods  in  solving practical problems. The numerical results of this case study show that the  total  logistics  operating  cost  and  the  number  of  vehicles  can  be reduced by $33,394 and 14, respectively, thus indicating the effectiveness  of  the  proposed  methods.  The  designed algorithm GA -PSO outperforms other algorithms in its ability to find a feasible solution. The MCRS mechanism for allocating profits generated by collaboration also outperforms  other  profit  allocation  mechanisms,  such  as  GQP,  Weak Least Core, Shapley value model, and Proportional Least Core. In addition, different  levels  of  collaboration  and  transportation  resource sharing are studied through five cases. The numerical results of those five cases show that the best performance of the logistics network is achieved when all logistics facilities form a single alliance.

Further research can be considered in the following aspects: (1) The dynamic and stochastic demands of customers may affect the applicability and feasibility of the proposed methods in solving the CMVRPTWMDP.  (2)  Dynamic  programming  models  and  exact  algorithms can be considered to solve the CMVRPTWMDP and find a feasible solution. (3) A new clustering method based on the geographical coordinates and time windows of customers can be further developed to coordinate  the  delivery  and  pickup  services  for  solving  a  large-scale CMVRPTWMDP.  (4)  The  profit  allocation  mechanism  adopted  to maintain the stability of the collaborative relationship among logistics facilities can be further studied in solving the CMVRPTWMDP.

## CRediT authorship contribution statement

Yong Wang: Conceptualization, Methodology, Software, Validation, Formal analysis, Investigation, Data curation, Visualization, Writing -original draft, Writing -review &amp; editing, Supervision, Project administration, Funding  acquisition. Lingyu  Ran: Software,  Validation, Formal analysis, Investigation, Data curation, Writing -original draft, Writing -review &amp; editing. Xiangyang  Guan: Conceptualization, Methodology, Formal analysis, Investigation, Data curation, Writing -original draft, Writing -review &amp; editing. Jianxin Fan: Visualization, Supervision, Project administration, Funding acquisition. Yaoyao Sun: Software, Validation, Writing -original draft, Writing -review &amp; editing. Haizhong Wang: Visualization, Supervision.

## Declaration of Competing Interest

The authors declare that they have no known competing financial interests or personal relationships that could have appeared to influence the work reported in this paper.

## Acknowledgments

The authors would like to express our sincere appreciation for the valuable comments made by two anonymous reviewers, which helped us to improve the quality of this paper. This research is supported by National  Natural  Science  Foundation  of  China  (Project  No.  71871035, 41977337), Key Science and Technology Research Project of Chongqing Municipal Education Commission (KJZD-K202000702), Key Project of Human Social Science of Chongqing Municipal Education Commission (No.  20SKGH079),  Chongqing  Liuchuang  Plan  Innovation  Project

(cx2021038), Team Building Project for Graduate Tutors in Chongqing (No.  JDDSTD2019008),  Chongqing  Bayu  Scholar  Youth  Project  (No. YS2021058), and Research and Innovation Program for Graduate Students in Chongqing (No. 2021S0054).

## References

- Agra, A., Christiansen, M., &amp; Wolsey, L. (2021). Improved models for a single vehicle continuous-time inventory routing problem with pickups and deliveries. European Journal of Operational Research, 297 (1), 164 -179.
- Ahkamiraad, A., &amp; Wang, Y. (2018). Capacitated and multiple cross-docked vehicle routing problem with pickup, delivery, and time windows. Computers &amp; Industrial Engineering, 119 , 76 -84.
- Ai, T. J., &amp; Kachitvichyanukul, V. (2009). A particle swarm optimization for the vehicle routing problem with simultaneous pickup and delivery. Computers &amp; Operations Research, 36 (5), 1693 -1702.
- Angelopoulos, A., Gavalas, D., Konstantopoulos, C., Kypriadis, D., &amp; Pantziou, G. (2018). Incentivized vehicle relocation in vehicle sharing systems. Transportation Research Part C: Emerging Technologies, 97 , 175 -193.
- Araghi, M. E. T., Tavakkoli-Moghaddam, R., Jolai, F., &amp; Molana, S. M. H. (2021). A green multi-facilities open location-routing problem with planar facility locations and uncertain customer. Journal of Cleaner Production, 282 , Article 124343.
- Ashouri, M., &amp; Yousefikhoshbakht, M. (2017). A combination of meta-heuristic and heuristic algorithms for the VRP, OVRP and VRP with simultaneous pickup and delivery. Broad Research in Artificial Intelligence and Neuroscience, 8 (2), 81 -95.
- Audy, J. F., D Amours, S., &amp; Rousseau, L. M. (2011). Cost allocation in the establishment ' of a collaborative transportation agreement-an application in the furniture industry. Journal of the Operational Research Society, 62 (6), 960 -970.
- Avci, M., &amp; Topaloglu, S. (2015). An adaptive local search algorithm for vehicle routing problem with simultaneous and mixed pickups and deliveries. Computers &amp; Industrial Engineering, 83 , 15 -29.
- Azadeh, A., &amp; Farrokhi-Asl, H. (2019). The close-open mixed multi depot vehicle routing problem considering internal and external fleet of vehicles. Transportation Letters, 11 (2), 78 -92.
- Aziez, I., Cote, J. F., &amp; Coelho, L. C. (2020). Exact algorithms for the multi-pickup and delivery problem with time windows. European Journal of Operational Research, 284 (3), 906 -919.
- Bernardino, R., &amp; Paias, A. (2018). Solving the family traveling salesman problem. European Journal of Operational Research, 267 (2), 453 -466.
- Blundell, R., Kristensen, D., &amp; Matzkin, R. (2014). Bounding quantile demand functions using revealed preference inequalities. Journal of Econometrics, 179 (2), 112 -127.
- Brandao, J. (2018). Iterated local search algorithm with ejection chains for the open ˜ vehicle routing problem with time windows. Computers &amp; Industrial Engineering, 120 , 146 159. -
- Brandao, J. (2020). A memory-based iterated local search algorithm for the multi-depot ˜ open vehicle routing problem. European Journal of Operational Research, 284 (2), 559 571. -
- Brito, J., Exposito, A., &amp; Moreno, J. A. (2016). Variable neighbourhood search for close-´ open vehicle routing problem with time windows. IMA Journal of Management Mathematics, 27 (1), 25 -38.
- Brito, J., Martínez, F. J., Moreno, J. A., &amp; Verdegay, J. L. (2015). An ACO hybrid metaheuristic for close-open vehicle routing problems with time windows and fuzzy constraints. Applied Soft Computing, 32 , 154 -163.
- Chen, Y. N., &amp; Hao, J. K. (2016b). The bi-objective quadratic multiple knapsack problem: Model and heuristics. Knowledge-Based Systems, 97 , 89 -100.
- Chen, M. C., Hsiao, Y. H., Reddy, R. H., &amp; Tiwari, M. K. (2016). The self-learning particle swarm optimization approach for routing pickup and delivery of multiple products with material handling in multiple cross-docks. Transportation Research Part E: Logistics and Transportation Review, 91 , 208 -226.
- Cruijssen, F., Borm, P., Fleuren, H., &amp; Hamers, H. (2010). Supplier-initiated outsourcing: A methodology to exploit synergy in transportation. European Journal of Operational Research, 207 (2), 763 -774.
- Dutta, P., Mishra, A., Khandelwal, S., &amp; Katthawala, I. (2020). A multiobjective optimization model for sustainable reverse logistics in Indian e-commerce market. Journal of Cleaner Production, 249 , Article 119348.
- Fernandez, E., Roca-Riu, M., &amp; Speranza, M. G. (2018). The shared customer ´ collaboration vehicle routing problem. European Journal of Operational Research, 265 (3), 1078 -1093.
- Fleszar, K., Osman, I. H., &amp; Hindi, K. S. (2009). A variable neighbourhood search algorithm for the open vehicle routing problem. European Journal of Operational Research, 195 (3), 803 -809.
- Gansterer, M., Hartl, R. F., &amp; Savelsbergh, M. (2020). The value of information in auction-based carrier collaborations. International Journal of Production Economics, 221 , Article 107485.
- Goksal, F. P., Karaoglan, I., &amp; Altiparmak, F. (2013). A hybrid discrete particle swarm optimization for vehicle routing problem with simultaneous pickup and delivery. Computers &amp; Industrial Engineering, 65 (1), 39 -53.
- Govindan, K., Paam, P., &amp; Abtahi, A. R. (2016). A fuzzy multi-objective optimization model for sustainable reverse logistics network design. Ecological Indicators, 67 , 753 768. -
- Guajardo, M., &amp; Ronnqvist, M. (2016). A review on cost allocation methods in ¨ collaborative transportation. International Transactions in Operational Research, 23 (3), 371 392. -

Y. Wang et al.

- Guo, R., Guan, W., &amp; Zhang, W. (2018). Route design problem of customized buses: Mixed integer programming model and case study. Journal of Transportation Engineering, Part A: Systems, 144 (11), 04018069.
- Ray, S., Soeanu, A., Berger, J., &amp; Debbabi, M. (2014). The multi-depot split-delivery vehicle routing problem: Model and solution algorithm. Knowledge-Based Systems, 71 , 238 -265.
- Hannan, M. A., Akhtar, M., Begum, R. A., Basri, H., Hussain, A., &amp; Scavino, E. (2018). Capacitated vehicle-routing problem model for scheduled solid waste collection and route optimization using PSO algorithm. Waste Management, 71 , 31 -41.

Hintsch, T., &amp; Irnich, S. (2020). Exact solution of the soft-clustered vehicle-routing problem. European Journal of Operational Research, 280 (1), 164 -178.

- Ho, S. C., &amp; Szeto, W. Y. (2017). A hybrid large neighborhood search for the static multivehicle bike-repositioning problem. Transportation Research Part B: Methodological, 95 , 340 -363.
- Krajewska, M. A., Kopfer, H., Laporte, G., Ropke, S., &amp; Zaccour, G. (2008). Horizontal cooperation among freight carriers: Request allocation and profit sharing. Journal of the Operational Research Society, 59 , 1483 -1491.
- Li, J., Pardalos, P. M., Sun, H., Pei, J., &amp; Zhang, Y. (2015). Iterated local search embedded adaptive neighborhood selection approach for the multi-depot vehicle routing problem with simultaneous deliveries and pickups. Expert Systems with Applications, 42 (7), 3551 -3561.
- Li, L. X., Wang, X., Lin, Y., Zhou, F. L., &amp; Chen, S. (2019). Cooperative game-based profit allocation for joint distribution alliance under online shopping environment. Asia Pacific Journal of Marketing and Logistics, 31 (2), 302 -326.
- Li, H., Zhang, Q. F., Chen, Q., Zhang, L., &amp; Jiao, Y. C. (2016b). Multiobjective differential evolution algorithm based on decomposition for a type of multiobjective bilevel programming problems. Knowledge-Based Systems, 107 , 271 -288.
- Li, H. Q., Zhang, L., Lv, T., &amp; Chang, X. Y. (2016a). The two-echelon time-constrained vehicle routing problem in linehaul-delivery systems. Transportation Research Part B: Methodological, 94 , 169 -188.
- Liu, R., &amp; Jiang, Z. B. (2012). The close-open mixed vehicle routing problem. European Journal of Operational Research, 220 (2), 349 -360.
- Liu, J. C., Sheu, J. B., Li, D. F., &amp; Dai, Y. W. (2021a). Collaborative profit allocation schemes for logistics enterprise coalitions with incomplete information. Omega, 101 , Article 102237.
- Liu, J., &amp; Sui, C. (2016). A comparative study on the profit distribution model of coal supply chain under inventory financing. Economics and Management Research, 16 , 269 274. -
- Liu, S. C., Tang, K., &amp; Yao, X. (2021b). Memetic search for vehicle routing with simultaneous pickup-delivery and time windows. Swarm and Evolutionary Computation, 66 , Article 100927.
- Liu, R., Xie, X. L., Augusto, V., &amp; Rodriguez, C. (2013). Heuristic algorithms for a vehicle routing problem with simultaneous delivery and pickup and time windows in home health care. European Journal of Operational Research, 230 (3), 475 -486.
- Liu, W. H., Zhang, J. H., Wei, S., &amp; Wang, D. (2021c). Factors influencing organisational efficiency in a smart-logistics ecological chain under e-commerce platform leadership. International Journal of Logistics Research and Applications, 24 (4), 364 391. -
- Long, Q. Q. (2016a). A multi-methodological collaborative simulation for interorganizational supply chain networks. Knowledge-Based Systems, 96 , 84 -95.

Long, Q. Q. (2016b). A flow-based three-dimensional collaborative decision-making model for supply-chain networks.

Knowledge-Based Systems, 97

, 101

-

110.

- Lozano, S., Moreno, P., Adenso-Díaz, B., &amp; Algaba, E. (2013). Cooperative game theory approach to allocating benefits of horizontal cooperation. European Journal of Operational Research, 229 (2), 444 -452.
- Marques, A., Soares, R., Santos, M. J., &amp; Amorim, P. (2020). Integrated planning of inbound and outbound logistics with a rich vehicle routing problem with backhauls. Omega, 92 , Article 102172.
- Maschler, M., Peleg, B., &amp; Shapley, L. S. (1979). Geometric properties of the kernel, nucleolus, and related solution concepts. Mathematics of Operations Research, 4 (4), 303 338. -

Maximo, V. R., &amp; Nascimento, M. C. V. (2021). A hybrid adaptive iterated local search ´ with diversification control to the capacitated vehicle routing problem. European Journal of Operational Research, 294 (3), 1108 -1119.

Miranda-Bront, J. J., Curcio, B., Mendez-Díaz, I., Montero, A., Pousa, F., &amp; Zabala, P. ´ (2017). A cluster-first route-second approach for the swap body vehicle routing problem. Annals of Operations Research, 253 , 935 -956.

Nagy, G., &amp; Salhi, S. (2005). Heuristic algorithms for single and multiple depot vehicle routing problems with pickups and deliveries. European Journal of Operational Research, 162 (1), 126 -141.

- Nickabadi, A., Ebadzadeh, M. M., &amp; Safabakhsh, R. (2011). A novel particle swarm optimization algorithm with adaptive inertia weight. Applied soft computing, 11 (4), 3658 3670. -
- Qi, M. Y., Ding, G. X., Zhou, Y., &amp; Miao, L. X. (2011). Vehicle routing problem with time windows based on spatiotemporal distance. Journal of Transportation Systems Engineering and Information Technology, 11 (1), 85 -89.
- Qiu, M., Fu, Z., Eglese, R., &amp; Tang, Q. (2018a). A tabu search algorithm for the vehicle routing problem with discrete split deliveries and pickups. Computers &amp; Operations Research, 100 , 102 -116.
- Qiu, Y. Z., Ni, M., Wang, L., Li, Q. Q., Fang, X. J., &amp; Pardalos, P. M. (2018b). Production routing problems with reverse logistics and remanufacturing. Transportation Research Part E: Logistics and Transportation Review, 111 , 87 -100.
- Quintero-Araujo, C. L., Gruler, A., Juan, A. A., &amp; Faulin, J. (2019). Using horizontal cooperation concepts in integrated routing and facility-location decisions. International Transactions in Operational Research, 26 (2), 551 -576.
- Rabbani, M., Farrokhi-Asl, H., &amp; Asgarian, B. (2017). Solving a bi-objective location routing problem by a NSGA-II combined with clustering approach: Application in waste collection problem. Journal of Industrial Engineering International, 13 , 13 -27.
- Rosenthal, E. C. (2017). A cooperative game approach to cost allocation in a rapid-transit network. Transportation Research Part B: Methodological, 97 , 64 -77.
- Sadati, M. E. H., Çatay, B., &amp; Aksen, D. (2021). An efficient variable neighborhood search with tabu shaking for a class of multi-depot vehicle routing problems. Computers &amp; Operations Research, 133 , Article 105269.
- Sadegh, M., &amp; Kerachian, R. (2011). Water resources allocation using solution concepts of fuzzy cooperative games: Fuzzy least core and fuzzy weak least core. Water Resources Management, 25 , 2543 -2573.
- Sartori, C. S., &amp; Buriol, L. S. (2020). A study on the pickup and delivery problem with time windows: Matheuristics and new instances. Computers &amp; Operations Research, 124 , Article 105065.
- Sellitto, M. A. (2018). Reverse logistics activities in three companies of the process industry. Journal of Cleaner Production, 187 , 923 -931.
- Sever, D., Zhao, L., Dellaert, N., Demir, E., Van Woensel, T., &amp; De Kok, T. (2018). The dynamic shortest path problem with time-dependent stochastic disruptions. Transportation Research Part C: Emerging Technologies, 92 , 42 -57.
- Shapley, L. S. (1971). Cores of convex games. International Journal of Game Theory, 1 (1), 11 26. -
- Shi, Y., Zhou, Y. J., Boudouh, T., &amp; Grunder, O. (2020). A lexicographic-based two-stage algorithm for vehicle routing problem with simultaneous pickup-delivery and time window. Engineering Applications of Artificial Intelligence, 95 , Article 103901.
- Solomon, M. M. (1987). Algorithms for the vehicle routing and scheduling problems with time window constraints. Operations Research, 35 (2), 254 -265.
- Szeto, W. Y., &amp; Shui, C. S. (2018). Exact loading and unloading strategies for the static multi-vehicle bike repositioning problem. Transportation Research Part B: Methodological, 109 , 176 -211.
- Tu, W., Fang, Z. X., Li, Q. Q., Shaw, S. L., &amp; Chen, B. Y. (2014). A bi-level Voronoi diagram-based metaheuristic for a large-scale multi-depot vehicle routing problem. Transportation Research Part E: Logistics and Transportation Review, 61 , 84 -97.
- Van Anholt, R. G., Coelho, L. C., Laporte, G., &amp; Vis, I. F. A. (2016). An inventory-routing problem with pickups and deliveries arising in the replenishment of automated teller machines. Transportation Science, 50 (3), 1077 -1091.
- Verdonck, L., Caris, A. N., Ramaekers, K., &amp; Janssens, G. K. (2013). Collaborative logistics from the perspective of road transportation companies. Transport Reviews, 33 (6), 700 -719.

Wang, Y., Yuan, Y. Y., Assogba, K., G, K., Wang, H. Z., Xu, M. Z., &amp; Wang, Y. H. (2018a). Design and profit allocation in two-echelon heterogeneous cooperative logistics network optimization. Journal of Advanced Transportation , 2018, 4607493.

- Wang, Y., Li, Q., Guan, X. Y., Fan, J. X., Liu, Y., &amp; Wang, H. Z. (2020c). Collaboration and resource sharing in the multidepot multiperiod vehicle routing problem with pickups and deliveries. Sustainability, 12 (15), 5966.
- Wang, Y., Ma, X. L., Li, Z. B., Liu, Y., Xu, M. Z., &amp; Wang, Y. H. (2017). Profit distribution in collaborative multiple centers vehicle routing problem. Journal of Cleaner Production, 144 , 203 -219.
- Wang, Y., Ma, X. L., Xu, M. Z., Liu, Y., &amp; Wang, Y. H. (2015b). Two-echelon logistics distribution region partitioning problem based on a hybrid particle swarm optimization-genetic algorithm. Expert Systems with Applications, 42 (12), 5019 -5031.

Wang, C., Mu, D., Zhao, F.u., &amp; Sutherland, J. W. (2015a). A parallel simulated annealing method for the vehicle routing problem with simultaneous pickup -delivery and time windows. Computers &amp; Industrial Engineering, 83 , 111 -122.

Wang, Y., Peng, S. G., Guan, X. Y., Fan, J. X., Wang, Z., Liu, Y., &amp; Wang, H. Z. (2021). Collaborative logistics pickup and delivery problem with eco-packages based on time -space network. Expert Systems with Applications, 170 , Article 114561.

Wang, Y., Peng, S. G., Zhou, X. S., Mahmoudi, M., &amp; Zhen, L. (2020a). Green logistics location-routing problem with eco-packages. Transportation Research Part E: Logistics and Transportation Review, 143 , Article 102118.

- Wang, J. W., Yu, Y., &amp; Tang, J. F. (2018c). Compensation and profit distribution for cooperative green pickup and delivery problem. Transportation Research Part B: Methodological, 113 , 54 -69.
- Wang, Y., Yuan, Y. Y., Guan, X. Y., Xu, M. Z., Wang, L., Wang, H. Z., &amp; Liu, Y. (2020b). Collaborative two-echelon multicenter vehicle routing optimization based on statespace-time network representation. Journal of Cleaner Production, 258 , Article 120590.

Wang, Y., Zhang, J., Assogba, K., Liu, Y., Xu, M. Z., &amp; Wang, Y. H. (2018b). Collaboration and transportation resource sharing in multiple centers vehicle routing optimization with delivery and pickup. Knowledge-Based Systems, 160 , 296 -310.

Wassan, N. A., &amp; Nagy, G. (2014). Vehicle routing problem with deliveries and pickups: Modelling issues and meta-heuristics solution approaches. International Journal of Transportation, 2 (1), 95 -110.

- Yang, F., Dai, Y., &amp; Ma, Z. J. (2020). A cooperative rich vehicle routing problem in the last-mile logistics industry in rural areas. Transportation Research Part E: Logistics and Transportation Review, 141 , Article 102024.
- Yanik, S., Bozkaya, B., &amp; deKervenoael, R. (2014). A new VRPPD model and a hybrid heuristic solution approach for e-tailing. European Journal of Operational Research, 236 (3), 879 -890.
- Yao, B. Z., Yu, B., Hu, P., Gao, J. J., &amp; Zhang, M. H. (2016). An improved particle swarm optimization for carton heterogeneous vehicle routing problem with a collection depot. Annals of Operations Research, 242 , 303 -320.
- Yu, H., Sun, X., Solvang, W. D., &amp; Zhao, X. (2020). Reverse logistics network design for effective management of medical waste in epidemic outbreaks: Insights from the Coronavirus Disease 2019 (COVID-19) Outbreak in Wuhan (China). International Journal of Environmental Research and Public Health, 17 (5), 1770.