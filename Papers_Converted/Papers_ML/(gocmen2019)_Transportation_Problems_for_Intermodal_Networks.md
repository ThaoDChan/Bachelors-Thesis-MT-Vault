![Image](Papers_Converted/0_Artifacts/[gocmen2019]_Transportation_Problems_for_Intermodal_Networks_artifacts/image_000000_2f2a34f5189c5c8f5634beb2790671d1d297025ce8d0380e1cb2d7f98b65b98e.png)

Contents lists available at ScienceDirect

## Expert Systems With Applications

journal homepage: www.elsevier.com/locate/eswa

## Transportation problems for intermodal networks: Mathematical models, exact and heuristic algorithms, and machine learning

## Elifcan Göçmen , Rızvan Erol

∗

Department of Industrial Engineering, Çukurova University, Adana, Turkey

## a r t i c l e i n f o

## a b s t r a c t

Article history: Received 17 November 2018 Revised 28 March 2019 Accepted 11 June 2019 Available online 13 June 2019

Keywords:

Intermodal transportation

Pick-up routing with three-dimensional loading Train loading Heuristic approach

Mathematical model, Machine learning

This paper presents a combinatorial problem called a pick-up routing problem with a three-dimensional (3D-PRP) loading constraint, clustered backhauls at the operational level, and train loading at the tactical level for an intermodal transportation network. A two-phase approach, called clustering first, packingrouting second, is proposed for use during the first stage. The clustering of backhauls is carried out using the k-means algorithm. A hybrid approach is provided, which combines the packing of orders by first solving a 3D loading problem for each cluster using machine learning with a best-fit-first strategy, with routing using a genetic algorithm. During the second stage, the train-loading problem is solved using a mixed integer programming approach to minimise the total costs by incorporating various cost types, in which detention and demurrage costs are taken into account. All solution approaches are computationally evaluated on real-world data provided by an international logistics firm and new randomly generated instances. Comparisons are carried out using both exact solution methods and heuristic approaches, and the proposed approach was shown to be more effective for real-world problems.

© 2019 Elsevier Ltd. All rights reserved.

## 1. Introduction

In recent years, intermodal problems related to decisions such as transport mode selection, vehicle routing (VRP), load planning, and consolidation have gained substantial attention in the transportation sector. An intermodal network design that ensures good solutions to multiple decisions is an important challenge. In this context, most researchers have focused on vehicle-routing problems and its variants, which are practical issues in the area of intermodal transportation. 3D-PRP is a variant of one of the most discussed vehicle-routing problems concerning practical and theoretical importance. Regarding the practical aspect, 3D-PRP has many real-world applications that are particularly relevant for logistics companies dealing with distribution and loading issues. 3DPRP is of significant value from a theoretical aspect because it includes two NP-hard problems: the pick-up routing problem and three-dimensional loading.

Train transportation plays a key role in intermodal networks, providing the efficient movement of items. There has recently been growing interest in shifting the transportation modes from road to rail. Preand post-haulage in the road transportation have a larger cost per tonne-km (Bergqvist &amp; Behrends, 2011). Rail transport ensures a reduction in external costs (Janic &amp; Vleugel, 2012).

Although both 3D-PRP and train loading problems have been separately discussed, there is a need to coordinate these problems in the present paper. The combination of different levels is necessary because 3D-PRP is a precondition of a train loading problem. Backhaul orders are packed during the 3D-PRP stage and the packed orders are then assigned to trains.

The present paper addresses the interactions between 3D-PRP and a train loading problem for an intermodal transportation network including road and rail transportation modes. During the first stage, road transportation is provided for the 3D-PRP, which handles the clustering of backhauls by incorporating the latitude and longitude of the loading cities using the k-means algorithm, the picking up of the orders, a feasible loading of the orders, and vehicle routing. During the second stage, rail transportation is examined for the train-loading problem, in which consolidated orders are assigned to trains with a focus on reducing the penalty and demurrage costs.

Our motivation for this paper is derived from the packing first, routing second approaches proposed by Bortfeldt and Homberger (2013) and Reil, Bortfeldt and Mönch (2018). We modified this approach by first using the clustering of backhauls for a fast computational time, then packing using machine learning for a feasible loading pattern with four loading dimensions, and finally applying vehicle routing using a genetic algorithm. The packing phase also handles four dimensions (length, width, height, and weight). Therefore, this phase can be called a four-dimensional

![Image](image_000001_eb29456ed05158fcbc41a1d72db71f007ed70ad03c8c22cb4755e6ffc11c3c46.png)

![Image](Papers_Converted/0_Artifacts/[gocmen2019]_Transportation_Problems_for_Intermodal_Networks_artifacts/image_000002_db68c0943ad676508bf66e281f3c053c575d4b3efef4a35572b0d000f913f362.png)

loading problem (4D loading). As the main contribution of this paper, we combine operational and tactical levels of decision making with a two-stage approach for considering the picking up of orders from customers in the hinterland, and then loading the orders into the vehicles using a 3D bin packing formulation for the operational and tactical levels to assign these order groups to trains. The combining of two different levels of decision (operationaltactical) is called a 'vertical integration' (Pishvaee, Farahani &amp; Dullaert, 2010). Several researchers have provided the benefits of combining different decision levels (Lerhlaly, Lebbar, Allaoui, Ouazar &amp; Afifi, 2016). The combination of levels ensures a bridging of the gap between academic and real-world problems. In addition, to the best of our knowledge, this is the first study integrating a genetic algorithm and a machine learning approach to solve 3D-PRP. We provide a new principle, namely, clustering first, packing-routing second for 3D-PRP. We consider various cost variants by incorporating penalty and demurrage costs for the train loading problem and the four loading dimensions, making it more challenging than the three loading dimensions for a 3D loading problem. A holistic view is presented regarding the cost minimization and utility maximization simultaneously. Finally, we compare exact solution approaches, open-source coding approaches, and heuristic approaches on real-world data using an international logistics firm and new randomly generated instances.

The remainder of this paper is organised as follows. The next section presents a literature review, and Section 3 describes the materials and methods applied. Section 4 presents the proposed solution approaches. Section 5 presents the computational results. Finally, Section 6 provides some concluding remarks regarding this study.

## 2. Literature review

Many published papers related to intermodal transportation are based on determining the mode choices (Bierwirth, Kirschstein &amp; Meisel, 2012; Min, 1991; Serper &amp; Alumur, 2016) and a comparison of transportation modes (Barnhart &amp; Ratliff, 1993) and load planning (Bauer, Bekta ¸ s &amp; Crainic, 2010; Bruns &amp; Knust, 2012). The combination of operational and tactical levels of decision-making related to intermodal transportation is presented for the first time in this paper. 3D-PRP and train loading problems have been studied intensively but independently. Thus, a literature review can be divided into two groups:

## 2.1. Studies related with pick-up routing problems with 3D loading constraints

VRP with real-world constraints have attracted the attention of both researchers and practitioners during the last few years. However, VRP with clustered backhauls and 3D loading was addressed by only Bortfeldt, Hahn, Männel and Mönch (2015) and Reil et al. (2018). In the present article, PRP with clustered backhauls and 3D loading including orientation, stacking, weight and clustering constraints are considered, suggesting that these constraints are more realistic. Papers considering some of these constraints are summarised in Table 1. Bortfeld and Homberger (2013) merely considered the orientation, stacking, and weight constraints for the problem. Each box has its own weight, and 90 ° rotations are permitted as well as in this work. Zachariadis, Tarantilis and Kiranoudis (2013) designed a vehicle routing problem under loading constraints, in which 90 ° rotations are allowed for all boxes. Zu, Qin, Lim and Wang (2012) introduced a vehicle routing problem with 3D loading constraints imposed based on the item fragility, weight, and orientation. Clustered backhauls, which were considered in the work of Reil et al. (2018) are also contribution of this work. Reil et al. (2018) dealt with this issue regarding the precedence relationship between clustered linehauls and backhauls while clustering of backhauls was carried out using the k-means algorithm for a fast computational time. Additional constraints such as LIFO, support by Tao and Wang (2015), load bearing by Ceschia and Schaerf (2013), fragility by Lacomme, Toussaint and Duhamel (2013) were presented. We also provide a two-stage mathematical model for 3D-PRP (see Section 3). 3D-PRP can be formulated into two stages it includes two NP-hard problems as mentioned in Ruan, Zhang, Miao and Shen (2013). Gendreau, Iori, Laporte and Martello (2008) presented

Table 1 Overview of papers dealing with vehicle routing problems with 3D loading constraints.

| Paper                                               | Problem characteristics                    | Objective                                         | Orientation   | Stacking   | Weight   | Clustering   | Solution approach                                 |
|-----------------------------------------------------|--------------------------------------------|---------------------------------------------------|---------------|------------|----------|--------------|---------------------------------------------------|
| Bortfeld (2012)                                     | Vehicle routing with 3D loading            | Total travel distance and total computation times | X             | -          | X        | -            | Tabu search                                       |
| Bortfeld and Homberger (2013)                       | Packing first, routing second              | Total travel distance and vehicle numbers         | X             | X          | X        | -            | Two stage heuristic                               |
| Ruan et al. (2013)                                  | Routing first, packing second              | Total transportation cost                         | X             | -          | X        | -            | Proposed Heuristic                                |
| Gendreau et al. (2008)                              | Routing to check the loading               | Total travel distance                             | -             | X          | X        | -            | Tabu search                                       |
| Junqueira, Oliveria, Carravilla and Morabito (2013) | Vehicle routing with 3D loading            | Minimum cost delivery routes                      | X             | -          | -        | -            | Mathematical modeling                             |
| Ceschia and Schaerf (2013)                          | 3D Packing                                 | Empty linear space                                | X             | -          | X        | -            | Local search heuristic                            |
| Lacomme et al. (2013)                               | Routing with 3D Packing                    | Total transportation cost                         | X             | -          | X        | -            | GRASP × ELS algorithm                             |
| Tao and Wang (2015)                                 | Routing that calls the loading             | Utilisation rate of container                     | X             | -          | X        | -            | Tabu search                                       |
| Tarantilis et al. (2009)                            | Combined routing and 3D-packing            | Minimum cost set of routes                        | X             | -          | X        | -            | Tabu search                                       |
| Zachariadis et al. (2013)                           | Vehicle routing with loading arrangement   | Utilisation rate of pallet volume                 | X             | -          | -        | -            | Local search heuristics                           |
| Zu et al. (2012)                                    | Vehicle routing with loading sub problem   | Total transportation cost                         | X             | -          | X        | -            | Tabu search                                       |
| Reil et al. (2018)                                  | Packing first, routing second              | Total travel distance                             | X             | X          | X        | -            | Packing first, routing second heuristic           |
| Presented study                                     | Clustering first, then packing and routing | Total travel distance and number of vehicles      | X             | X          | X        | X            | Clustering first, then packing-routing heuristics |

two variants of the problem: one for routing and the other for feasible loading.

Most commonly, many of the published papers provide heuristic approaches. The algorithm proposed by Gendreau, Iori, Laporte and Martello (2006) determines the routes using an outer Tabu search and solves the bin packing with an inner Tabu search. Tarantilis, Zachariadis and Kiranoudis (2009) presented Tabu search and a guided local search for routing, six packing heuristics for loading. A Tabu search for routing and two packing heuristics for loading were presented by Zu et al. (2012). The two-stage heuristic, which is the motivation of our study, was provided by Bortfeldt and Homberger (2013) and Reil et al. (2018) using packing first, routing second (P1R2) approaches while we also used the bin packing solver in order to obtain more optimum and fast packing plans.

## 2.2. Studies related with train freight assignments

Train freight assignments are a sub-problem of a train planning problem. A few papers that have solved the freight-to-train assignment problem are provided herein. In the present paper, a mixed integer programming approach is used to solve the freightto-train assignment problem. In contrast to Xiao, Pachl, Lin and Wang (2018), who uses heuristic approach for a block-train assignment to determine the train service and travel plans, most of the literature developed linear integer models. Feo and GonzalesVelarde (1995) provided an integerlinear programming to assign two trailers to a single wagon, which is a limitation of their study. Corry and Kozan (2008) also developed a linear integer problem for a load-planning problem regarding the train length, load pattern restriction, and handling time. Most commonly, the overall aim is considered as minimising the loading time, maximising the train usage, as mentioned in Bruns and Knust (2012). Anghinolfi and Paolucci (2014) evaluated the train loading by taking into account the distances between containers. However, container handling time, weight distribution are also regarded in the work of Corry and Kozan (2006). But in reality, it is necessary to regard the penalty and demurrage costs for real-world conditions. In this work, the total costs by incorporating various cost types, in which detention and demurrage costs are taken into account (see Section 3.2).

## 3. Materials &amp; methods

## 3.1. Problem description

ing orders are allocated to the trains when considering real-world constraints.

The 3D-PRP is defined as follows:

Carrying units are considered with identical containers or trailers with length L , depth D , height H , and weight W . Each clustered backhaul numClusters requires 3D orders with length L , depth D , height H , and weight W . Loading cities are denoted by V = (0,1,…, n ), and the depot is represented by node 0. The orders are divided into two groups: export and import containers. Export containers, which can be defined as the containers transported from the customers in the hinterland to the depot (Sterzik &amp; Kopfer, 2013), are considered. Each order is loaded within the loading space. The loading space and weight of the orders cannot exceed the vehicle space and weight. Each order lies in the vehicle orthogonally. Fragile orders can be stacked on other fragile orders. Orders that are non-fragile cannot be loaded on top of fragile orders (Ruan et al., 2013). This limitation is valid for our problem. Each order can be rotated by 90 ° horizontally. Each vehicle starts and completes its tour at the depot. Each clustered backhaul is visited once to pick up the orders.

In the present paper, in addition to decisions in terms of 3DPRP, the assignment of packaging orders to the block trains is considered during the second stage. The considered network, including both road and rail transportation, is presented in Fig. 2. A real-life application is presented to demonstrate the practical value of this study. The aim of this study is to develop effective solutions regarding this complex intermodal network concerning its practical importance.

The first stage is dominated by road transportation, in which trucks are used to pick up orders from the loading cities. The main aim of this stage is to increase the efficiency by reducing the usage costs of the vehicles and achieve a high capacity utilisation of the load units, such as trailers and containers. Thus, a holistic view is put forward considering both minimum cost and maximum utilization of carrying units. The second stage is related to rail transportation, in which block trains are used to transfer the carrying units. The purpose of train-load planning is to ensure the efficient delivery and timely availability of the carrying units with a main focus on a reduction of the total cost. In the objective function, the aim is a minimisation of the demurrage, usage and detention costs. The main constraints considered in this problem are capacity and assignment constraints.

The presented models reflecting practical requirements and the proposed solution approaches can be used by freight forwarders and logistics operators dealing with container delivery and loading. The proposed models are highly relevant to actual practice when considering both inland and international transportation networks.

In this section, we describe the 3D-PRP and train loading problems in detail. The first stage is based on the idea that orders of each clustered backhauls are packed using a 3D bin packing formulation and the vehicle routing problem is solved. A simple representation is given in Fig. 1. During the second stage, the packag-

## 3.2. Mathematical model approach

In this section, we start by discussing all problems using mathematical modelling approaches to find the exact

Fig. 1. Clustering of loading cities and 3D bin loading.

![Image](image_000003_51fef666a49e0e8c982d286569fd5ceb4e4ddfabc86a8c477c767dfc7b6a381c.png)

/negationslash

Fig. 2. Intermodal transportation network in presented study.

![Image](image_000004_88d902e5ea082dd55665377087a2f22f82ab2024758e8f4ba031bee43f4b31df.png)

solutions. This mathematical modelling is based on an approach by Ruan et al. (2013), in which 3D-PRP can be divided into two groups, one for pick-up routing and the other for 3D packing. In addition, clustering the loading points by incorporating the latitudes and longitudes is our contribution to this particular approach. We suppose the ( x, y ) coordinates of the loading points, and each order i is assigned to k clusters. Before employing the developed k-means algorithm, a random assignment is employed using random clusters along the coordinates mkxy , the coordinates of the points p ixy , the cluster number of point c i , the total number of clusters num , the order numbers assigned to cluster n, the centroid cntr kxy , the squared distance d ik , and the closest distance to the cluster, close i The decision variable Xik is an assignment of points to clusters. The mathematical formulation is shown below:

Clustering function:

$$\text{objective} & = \frac { 1 } { n } \sum _ { i = 1 } ^ { n } \sum _ { k = 1 } ^ { n u m } \left \| p _ { i x y } - c _ { k x y } \right \| ^ { 2 } & ( 1 ) \quad \min z \\ & \text{In the objective function, the aim is a minimisation of the dis-} _ { l }$$

In the objective function, the aim is a minimisation of the distance of the loading points. Thus, each point is assigned to the closest cluster.

## ∗ Stage 1

A random assignment of the points is as follows:

uniformint(1, num)

$$\cdots \quad \cdots \quad \cdots \quad \cdots \\ c _ { i } = \text{unifo} \\ x _ { i k } = c _ { i } \\ * \text{Stage 2} \\ \text{The countries}$$

The centroids are calculated as follows:

nk sum(

Xik

,

1);

cntr kxy

=

∗

=

p ixy

/

n

Stage 3

∗ The points are reassigned as follows:

$$\begin{smallmatrix} \text{$m$ pow a c a s p s u a n o v w s. } \\ d _ { i k } = \sum _ { i = 1 } ^ { n } \sum _ { k = 1 } ^ { n u m } \left \| p _ { i x y } - c _ { k x y } \right \| ^ { 2 } \\ \text{close} _ { i } = d _ { i k } \end{smallmatrix}$$

/negationslash

After formulating the clustering of loading points, a pick-up routing problem (PRP) is formulated. PRP calls for the routes of vehicle k with capacity Qk , which visits a set of customers N = (1, 2,…, n ). All locations are shown as ( i, j, l ), where i, j, l ε N and i = j , i = l. Here, c ij is the distance measured by the generalized cost. Generalized cost is composed by price between location i and j (distance multiplied by cost per km) and time (generalized time between I and j multiplied by the price parameter of the average value of time). p i is the load amount picked up from point i . Given k identical vehicles, each has a fixed cost F .

Decision Variables

## Uik : Upperlimitofloadamountpickedupfrompoint i with vehicle k ( unit )

$$X _ { i j k } = \begin{cases} 1, & \\ 0, & \end{cases}$$

, i f v ehicle k goesto point j f rom point i , otherwise

$$z _ { i k } = \begin{cases} 1, & \\ 0, & \end{cases}$$

, i f vehicle k picks up from point i , otherwise

$$y _ { k } = \begin{cases} 1, & i f \text{vehicle} \, k \, is \, used \\ 0, & o t h e r w i s e \end{cases}$$

$$\sum _ { i = 1 } ^ { k } \sum _ { k = 1 } ^ { l } \sum _ { i = 1 } ^ { j } \sum _ { j = 1 } ^ { j } c _ { i j * } X _ { i j k } \ + \sum _ { k = 1 } ^ { k } F * y _ { k }$$

/negationslash

$$\text{the dis} ^ { - } \text{ to the} \quad \sum _ { i = 0 } ^ { l } X _ { i k } = \sum _ { j = 0 } ^ { J } X _ { l j k } \ l = 1, \dots, n ; \, k = 1, \dots, K ; l \neq i ; i \neq j \quad \text{ (1)}$$

/negationslash

/negationslash

$$& \sum _ { k i = 1 } ^ { K } \sum _ { j = 0 } ^ { J } X _ { i j k } = 1 \quad i = 0, \dots., n ; \ i \neq j \\ & \quad. \. \. \. \. \. \. \. \. \. \. \. \. \.$$

$$0 \leq U _ { i k } \leq Q _ { k } \ i = 1, \dots, n ; \ k j = 1, \dots, K$$

$$U _ { j k } & \geq U _ { i k } + \, p _ { j } \ast z _ { j k } - \left ( 1 - X _ { i j k } \right ) \ast Q _ { k } \ \ Q _ { k } \, \mathfrak { i } = 1, \dots, n ; \\ \mathfrak { j } & = 1, \dots, \, n ; \, k = 1, \dots,$$

$$j = 1, \dots, \, n ; \, k = 1,$$

$$\underset { \underset { \underset { \underset { \underset { \underset { \underset { \underset { \text{pink}} { k } } { \epsilon } } { \epsilon } } } } { \sum } } \sum _ { j = 1 } ^ { J } X _ { 0 j k } \ \leq y _ { k } \ k = 1, \dots, K \quad \text{(5)}$$

$$\stackrel { r \cdots \, \_ r } { \text{utes of} } \, \quad \, \stackrel { J } { N = ( 1, \quad \sum _ { j = 1 } ^ { J } X _ { j 0 k } \ \leq y _ { k } \ k = 1, \dots, K } \, & & ( 6 ) \\ \text{arialized} \quad \, \quad \, \, \,$$

/negationslash

$$\text{ralized} \int i \, \text{and} \quad \sum _ { j = 0 } ^ { J } X _ { i j k } \ = \ z _ { j k } \, i = 1, \dots, n ; \, k = 1, \dots, K ; \ i \neq j \quad \quad ( 7 )$$

$$\sum _ { i = 1 } ^ { l } z _ { i k ^ { * } } \ p _ { i } \leq Q _ { k } \ k = 1, \dots K$$

$$z _ { r } + h _ { r } \leq z _ { p } + ( 1 - f _ { p r } ). M \, \forall p, \ r \ p < r$$

$$\sum _ { i, j \in U } ^ { J } X _ { i j k } \ = \ W - 1 \, k = 1, \dots, K & & ( 9 ) & & x _ { p } + l _ { p } \\ \quad \ \. \quad \. \. \. \. \. \. & & \dots$$

$$a _ { p r } + b _ { p r } + c _ { p r } + d _ { p r } + e _ { p r } + f _ { p r } \geq o _ { k p } + o _ { k r } - 1 \ \forall p, \ r \ p < r \quad ( 7 )$$

$$X _ { i j k }, \, z _ { i k }, \, y _ { k } \, \epsilon \, ( 0, 1 ), \ U _ { i k } \, \geq \, 0 & & ( 1 0 ) & & y _ { p } + w _ { l }$$

The objective function minimises the total travel cost and number of vehicles. Constraint (1) is a flow conservation; when a vehicle arrives at a node, it must travel from that node to another one. Constraint (2) ensures that each node is visited once. Constraint (3) ensures that the upper limit of the pick-up amount cannot exceed the vehicle capacity. Constraint (4) balances the pick-up amount transferred from one node to another. Constraints (5) and (6) ensure that vehicles start and end at the depot. Constraint (7) ensures that the vehicle picks up from a point if the vehicle goes to that point. Constraint (8) ensures that the picked-up amounts cannot exceed the vehicle capacity. Constraint (9) is a sub-tour constraint. Constraint (10) is a non-negativity constraint in which the decision variables should be greater than or equal to zero. A binary variable can only take on a value of zero or one.

Before formulating the 3D loading problem, we discuss the indices and parameters. The notation is based on that by Chen, Lee and Shen (1995). Let P order (1,…, P ), where K is a set of available carrying units (1,…, K ). Each order has a loading space l p, wp, hp , and each carrying unit has a loading space L k , Wk, H k . The utilisation cost and capacity of the carrying units k are Ck and Qk .

Decision variables:

## okp : Assignmentoforderstothecarryingunits

apr = { 1 , i f order pis le f t of order r 0 , otherwise bpr = { 1 , i f order pis right of order r 0 , otherwise

cpr = { 1 , i f order pis f ront of order r 0 , otherwise dpr = { 1 , i f order pis behind of order r 0 , otherwise

epr = { 1 , i f order pis on of order r 0 , otherwise f pr = { 1 , i f order pis under of order r 0 , otherwise

xp : Coordinate x of order p ( p = 1 , . . . , P ), zp : Coordinate z of order p ( p = 1 , . . . , P ),

yp : Coordinate y of order p ( p = 1 , . . . , P ),

$$\min z = \sum _ { k = 1 } ^ { \cdot } \sum _ { p = 1 } ^ { P } o _ { k p } * C _ { k } \\ \nu _ { - } \pm I _ { - } < \nu _ { - } \pm T 1 -$$

$$x _ { p } + l _ { p } \leq x _ { r } + ( 1 - a _ { p r } ). M \, \forall p, \ r \ p < r \quad \quad ( 1 ) \quad \ \ v \quad.$$

$$x _ { r } + l _ { r } \leq x _ { p } + ( 1 - b _ { p r } ). M \, \forall p, \ r \ p < r & & ( 2 ) & & \frac { \lambda _ { i j }, \ u _ { i } } { \Theta }$$

$$( 1 0 ) & \quad y _ { p } + w _ { p } \leq W _ { k } + \left ( 1 - o _ { k p } \right ). M \, \forall p, \ \beta & \quad ( 9 ) \\ d \, \text{num} _ { \cdot } & \quad \sim \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \cdot \colon$$

$$& x _ { p } + l _ { p } \leq L _ { k } + \left ( 1 - o _ { k p } \right ). M \, \forall p, \ \beta & & ( 8 ) \\ ( 1 0 ) & \quad. \ \dots \quad \psi \psi \, \psi \, \lambda \, \psi \, \lambda \, \psi \, \alpha & & \quad \, \nu \infty$$

$$\ a \, \text{num} ^ { - } & \ a \, \text{veh} ^ { - } & \quad z _ { p } + h _ { p } \leq H _ { k } + \left ( 1 - o _ { k p } \right ). M \, \forall p, \ \beta & & & ( 1 0 ) \\ \text{er one.} & \ a \, \text{int} \, ( 3 ) \quad. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \.. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \. \.$$

$$\int _ { \text{xceed} } ( 3 ) \quad x _ { p }, \, y _ { p }, \, z _ { p } \geq 0, \, a _ { p r }, \, b _ { p r }, c _ { p r }, d _ { p r }, e _ { p r }, f _ { p r }, o _ { k p }, \in ( 0, 1 ) \quad ( 1 1 )$$

The objective function minimises the utilisation cost of the carrying units. Constraints (1)-(6) provide the order locations considering the x, y, z coordinates. Constraint (7) ensures that two orders in a carrying unit must be only one of the directions described above. Constraints (8)-(10) ensure that the loading spaces of the orders cannot exceed the loading spaces of the carrying units. Constraint (11) is the non-negativity constraint in which decision variables should be greater than or equal to zero. A binary variable can only take on a value of zero or 1.

In the train-loading problem, let us consider a set of trains ( i = 1,…, I ) and a set of carrying units ( j = 1,…, J ). Before their assignments to the trains, the carrying units are assumed to be transferred from a ship. Thus, the shipping day ship with tolerance num is incorporated into the model. Train capacity b i must not be exceeded. Each train has g i working days and tr i departure days that must be respected. The train usage cost is f ij when the carrying units are assigned to the trains. An extra cost is defined by the detention cost p j and demurrage cost cst when the due date d j is not ensured. The other parameter is the utilisation a ij .

Decision Variables wi : Demurrage ( day )

xij = { 1 , if i trains is assigned to j carrying units 0 , otherwise sj : Detention day ( )

$$\min z = \sum _ { i = 1 } ^ { l } f _ { i j } * X _ { i j } + \sum _ { j = 1 } ^ { J } s _ { j } * p _ { j } + \sum _ { i = 1 } ^ { l } w _ { i } * \text{cst}$$

$$\sum _ { i = 1 } ^ { I } X _ { i j } = 1 \quad ( j = 1, \dots J )$$

$$\sum _ { j = 1 } ^ { \mathbb { J } } x _ { i j } * a i j \leq b _ { i } \ i = 1, \dots \dots, I$$

$$w _ { i } = \sum _ { j = 1 } ^ { J } x _ { i j } * ( s h i p + n u m - t r _ { i } \ ) \ i = 1, \dots, I$$

$$g _ { i } \leq d _ { j } + s _ { j } + \left ( 1 - x _ { i j } \right ) * M \, i = 1, \dots,., l ; \ j = 1, \dots, J \quad \quad ( 5 )$$

$$( 2 ) ^ { \quad X _ { i j }, \ u _ { i } \in \ ( 0, 1 ), \ s _ { j } \ w _ { i } \ \geq \ 0 }$$

$$y _ { p } + w _ { p } \leq y _ { r } + ( 1 - c _ { p r } ). M \ \forall p, \ r \ p < r$$

$$y _ { r } + w _ { r } \leq y _ { p } + ( 1 - d _ { p r } ). M \, \forall p, r \ p < r$$

$$z _ { p } + h _ { p } \leq z _ { r } + ( 1 - e _ { p r } ). M \, \forall p, \ r \ p < r$$

(3) (4) (5) The objective function (1) minimises the usage cost of the carrying units, the detention cost, and the demurrage cost. Constraint (2) is an assignment constraint that ensures that each carrying unit must be assigned to one train. Constraint (3) ensures that train capacity cannot be exceed. Constraint (4) corresponds to the demurrage cost. Constraint (5) ensures that the number of train transportation days cannot exceed the total travel days of the carrying

units assigned to the train. Constraint (6) is a non-negativity constraint in which the decision variables should be greater than or equal to zero. A binary variable can only take on a value of zero or 1.

Lastly, we produce a DataforR list of cluster solutions including oid (order codes), sku (clusters), and loading spaces (l,d,h,w ) for a 3D packing phase.

## 4. Proposed solution approaches

As previously mentioned, 3D-PRP is one of the most wellknown NP-hard problems. Computational complexity of the problem is examined. The complexity of the total decision variables is calculated 2 n k 2 + 3 nk + 2 k and the complexity of the total constraints is calculated 2 n k 2 + 3 nk + 2 k for PRP. The complexity of the total decision variables is calculated 12 n 2 + 2 kp + 3 p and the complexity of the total constraints is calculated 6 n 2 + n k 2 + 3 kp for 3D. Real-world data including 103 nodes ( n ), 20 vehicles ( k ) and 4444 orders ( p ) cannot be solved by exact algorithmic approaches within a reasonable amount of time. Therefore, we present heuristics, meta-heuristics, and machine learning approaches to solve these problems. In the present paper, the clustering of the loading points is solved using the k-means algorithm; the 3D bin packing problem is dealt with using machine learning with a best-fit-first strategy, which is considered more effective than other packing solutions (Yang &amp; Mu, 2018); and routing section is addressed using a genetic algorithm. A heuristic approach developed to integrate these three methods is presented to solve the combinatorial problem.

## 4.1. K-means clustering algorithm

K-means clustering ensures the assignments of n objects into k clusters, where the initial cluster number is assigned before the algorithm starts. Fig. 3 shows a representation of k-means clustering. The clustering starts with an initial cluster number, numClusters , or the use of the k-means algorithm to choose the initial cluster number. The next step is the computation of the distances between the objects p i and the cluster center c k . The assignment of each object to the clusters with the closest centroid then proceeds. The step is repeated until the maximum number of iterations is reached.

K-means uses the squared Euclidean distances given by:

$$\text{Minimise sum} & = \sum _ { i = 1 } ^ { n } d i s t a n c e ( \boldsymbol p _ { i } - \boldsymbol c _ { k } ) \\ \text{$r$_{i}$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \cdots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$ - \dots $r$.}$$

The pseudo-code of k-means is depicted in Fig. 3. Dataset N, the initial k clusters, cluster centroids, and the cluster label are represented as A, B i , B , and C , respectively.

## 4.2. Machine learning for 3D bin packing

In this subsection, the orders of each cluster are packed in a corresponding carrying unit. A 3D problem for each carrying unit bin is defined based on the length l , weight w , depth d , and height h , and the set of orders it is defined by the length l , weight w , depth d , and height h. Initially, orders it are loaded into the same carrying unit bin. A list of loading positions is generated in descending order of l, d, h , and w , respectively. The aim of this order is to load larger orders first. At the beginning of the heuristics, the loading place of the carrying unit (0, 0, 0) is available. Orders are loaded without any constraint violations until all orders are packed into carrying units. Then, orders are loaded into a carrying unit using best-fit-first heuristics. Given the orders and carrying units, the solver searches for the optimum packing plans to minimise the number of vehicles and maximise the volume utilisation. The loading process is as follows (Algorithm 1):

Inputs: Clustered orders from k-means clustering algorithm

Outputs: Order positions into the vehicles

- #- bpp\_solver: output: packing solution
- #-sn
- # sn bpp\_solution packing solution &lt; list &gt;
- # it item &lt; data.table &gt;
- # oid: order id &lt; integer &gt;
- # sku: stock keeping unit as it id &lt; character &gt;
- # tid: ticket id - a unique id within oid &lt; integer &gt;
- # otid: order id x ticket id - a unique identifier indicates that it with the same tid can be packed into one bin &lt; character &gt;
- # - bid: bn id &lt; integer &gt;
- # - x, y, z it position in bid bin &lt; numeric &gt;
- # - l, d, h it scale along x, y, z &lt; numeric &gt;
- # - w it weight &lt; numeric &gt;
- # - bn bins &lt; data.table &gt;
- # - id bn id &lt; character &gt;
- # - l bn length limit along x-coordinate &lt; numeric &gt;
- # - d bn depth limit along y-coordinate &lt; numeric &gt;
- # - h bn height limit along z-coordinate &lt; numeric &gt;
- # - w bn weight limit along w - a separate single dimension &lt; numeric &gt;
- sn &lt; - gbp::bpp\_solver(it = it, bn = bn)

Fig. 4. Swap operator representation.

## 4.3. Vehicle routing using genetic algorithm

The genetic algorithm (GA) used for the vehicle routing problem provides specific procedures. Firstly, we produce an initial population, which includes chromosomes in a random manner. Secondly, each population member is evaluated, and the first generation is found. Thirdly, we initialise some genetic algorithm operators to obtain better generations. Various mutation operators such as flip, swap, and slide are selected to mix the routes. Using a swap operator, I and J locations are chosen, and their positions are swapped (Bestroute (k, [I J]) = Bestroute (k, [J I])). Fig. 4 shows the swap representation, where City\_2 and City\_8 positions are swapped.

With a flip operator, I and J locations are chosen, and their positions are flipped (Bestroute (k, [I J]) = Bestroute (k, J: -1: I). With a slide operator, I and J locations are chosen, and their positions are slid (Bestroute (k, [I J]) = Bestroute (k, [I + 1: J I]). This process is repeated to optimise the objective function. The pseudocode for the GA is as follows (Algorithm 2):

## Algorithm 2 Pseudocode for genetic algorithm.

Input: popSize, numIter, Pmutation

Output: optRoute, minDist

//InıtializePopulation of routes randomly with a function random()

## for k : = 2 to popSize do

if ( k = 1) then

- An initial population pop randomly

- Current best solution is pbest from pop

else

Initialise the population popSize with pbest

## end if

//GA

While (max iteration has not been reached) do

For I: = 1 to popSize do

- Selection of best route p2 from popSize

-Reduce the total distance of p2

-Best solution pbest = p2

- Perform mutation on the best route to get three new routes

## End for

Evaluate solution pbest with evaluate function

If minDist &lt; globalMin Then Updata the best solution pbest end if OptRoute- BestSolution

End while

## 4.4. General layout of the proposed approach

Our proposed approach integrates a k-means algorithm, machine learning, and a GA. K-means applies clustering heuristics to produce the initial clusters for the 3D loading phase. We obtain a dataset including clusters for the loading points. The 3D loading problem using machine-learning best-first-fit heuristics is then discussed. Finally, the routing phase is addressed using a GA. A general layout of the three different types of algorithms applied for 3D-PRP is as follows:

## // First stage: Clustering

//cluster locations with k-means clustering algorithm:

for each city (pick-up location) do

if (customer is a backhaul) then

-find the k-nearest neighbours by latitude and longitude

- store the result clusters dataforR

## end if

end for

//Second stage: Packing

// Use clusters dataforR

for i: = 1 to numclusters do

solve 3D bin packing instance given by length l, depth d, height h, and vehicle ids;

store 3D packing plan 'Result.txt'

## endfor

//Third stage: Routing

// Use the data processed by R

Initialise the population

While iteration &lt; = itermax do

Iteration = + Evaluate each population member

iteration

1

Find the best individuals in the population

Perform mutation operators to find new individuals

Evaluate the fitness of new individuals and place them

## 5. Computational results

To solve the real-world data for 3D-PRP, the hybrid algorithmic approach was executed on a computer with an Intel 3.6GHz CPU with 16 GB of RAM. As previously mentioned, 3D-PRP is one of the most well-known NP-hard problems. Exact algorithmic approaches cannot solve real-world problems that include a large dataset within a reasonable amount of time. Thus, small-sized test instances including customer sets, vehicles with corresponding orders, and medium-sized test instances are generated. We divide 3D-PRP into two groups: 3D and PRP. Based on mathematical models, namely, the commercial optimisation solver GAMS and Open Door Logistics software, both the 3D and PRP problems are solved. Meta-heuristics and machine learning approaches solve a real-world instance containing 4444 orders and newly generated medium test instances containing 994 orders. We compare the performances of the exact method (two-stage approach), open-source coding, and the proposed approach (first clustering, then packingrouting).

5.1. Results of the exact algorithmic and open source software for small-sized instances

We start by presenting the results for small-sized instances related to 3D-PRP. The results were obtained using both open-source coding and a mixed integer programming (mip) approach. We focus on the total travel distance, number of vehicles for routing section, and the loading positions for the loading section. Firstly, by means of the proposed k-means algorithm, 20 loading points of the orders are clustered, and eight clusters (orders) are formed. Next, the orders are loaded onto the vehicles with 3D loading constraints. Finally, the optimal routes for vehicles are obtained. The computing time for an instance is 0 s for clustering, approximately 34s for routing, and 60s for loading. Fig. 5 shows the positions

Fig. 5. Positions of the orders into the vehicles.

![Image](image_000005_0e42ffd7f33de583d2f8b0174608f522530d850561a45d4791f3b4ff6d0d04b7.png)

Fig. 6. Routes created using open-source coding approach.

![Image](image_000006_a3904e7cff95c3eb3e35494cb4982cbd28555b7f8e77bbf4630df1ce5a1219bb.png)

|         | stop-Id   | Job-Id vehlcle name   | stop-number   | stop-name   | stop      | address stop latltude   | stop-longitude   |
|---------|-----------|-----------------------|---------------|-------------|-----------|-------------------------|------------------|
|         |           |                       |               | stop        | adana     |                         | 35,331           |
| 2-1     | P3        | truckl1               |               | stop        | aksaray   | 38,369                  |                  |
| 2-1     | P7        |                       |               | stop        | bursa     | 40,269                  | 29,074           |
| 2-1     | End_2     |                       | End_2         |             |           | 40,975                  | 29,257           |
| 2-2     |           | truckl-2              |               | stop        | antalya   |                         | 30.784           |
| 2-2     | P8        | truckl-2              |               |             | denizll   | 37,783                  |                  |
| 2-2     | P6        |                       |               | stop        | balikesir |                         | 27,889           |
| 2-2     | End_2     | truckl-2              | End_2         | depot       |           | 40,975                  | 29,257           |
| 2-3     | Pz        | truckl-3              |               | stop        | afyon     | 38,757                  | 30,535           |
| 10 2-3  | Pa        | truckl-3              |               | stop        | ankara    | 40,123                  | 32,63            |
| 11  2-3 | End_2     |                       | End_2         | depot       |           | 40,975                  | 29,257           |

Routes and pick-up quantities obtained from GAMS commercial solver.

| Routes                                                                                                           | Routes                                                                                                           | Routes                                                                                                           | Routes                                                                                                           |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| Truck Depot-Afyon-Ankara-depot Truck Depot-Adana-Aksaray-Bursa-depot Truck Depot-Balıkesir-Denizli-Antalya-depot | Truck Depot-Afyon-Ankara-depot Truck Depot-Adana-Aksaray-Bursa-depot Truck Depot-Balıkesir-Denizli-Antalya-depot | Truck Depot-Afyon-Ankara-depot Truck Depot-Adana-Aksaray-Bursa-depot Truck Depot-Balıkesir-Denizli-Antalya-depot | Truck Depot-Afyon-Ankara-depot Truck Depot-Adana-Aksaray-Bursa-depot Truck Depot-Balıkesir-Denizli-Antalya-depot |
| Cities/vehicles                                                                                                  | Truck 1                                                                                                          | Truck 2                                                                                                          | Truck 3                                                                                                          |
| Adana                                                                                                            |                                                                                                                  | 2                                                                                                                |                                                                                                                  |
| Afyon                                                                                                            | 10                                                                                                               |                                                                                                                  |                                                                                                                  |
| Aksaray                                                                                                          |                                                                                                                  | 7                                                                                                                |                                                                                                                  |
| Ankara                                                                                                           | 4                                                                                                                |                                                                                                                  |                                                                                                                  |
| Antalya                                                                                                          |                                                                                                                  |                                                                                                                  | 10                                                                                                               |
| Balıkesir                                                                                                        |                                                                                                                  |                                                                                                                  | 1                                                                                                                |
| Bursa                                                                                                            |                                                                                                                  | 10                                                                                                               |                                                                                                                  |
| Denizli                                                                                                          |                                                                                                                  |                                                                                                                  | 6                                                                                                                |

visually. The orders are located based on their x, y, z coordinates. Thus, the optimum location planning is obtained.

We can see from Table 2 that the starting point for each vehicle is a depot. Note that we make sure that each truck is formed by incorporating the capacity and number of orders. In addition, the order locations are also important when determining the truck routes.

The results from open-source coding are similar to the results by the mip solver. The pick-up routing problem is solved using the open-source coded program Open Door Logistics (ODL). Given the vehicle numbers, the addresses and pick-up quantities are input for the system, and the routes are then generated. We can see from

Comparison of exact solution approach and ODL for routing sub-problem.

| Instances   | Exact method   | Exact method   | ODL   | ODL   |
|-------------|----------------|----------------|-------|-------|
|             | nv             | ttd            | nv    | ttd   |
| 1           | 1              | 2908           | 1     | 0.2   |
| 2           | 2              | 4325           | 2     | 3     |
| 3           | 3              | 4407           | 3     | 22    |
| 4           | 4              | 5065           | 4     | 33    |

Fig. 6 that the open-source software applies 3D-PRP using similar nv and ttd with the mip solver.

The computing time is approximately 34s for the mip solver, whereas the computing time is approximately 22 s for ODL. Fig. 7 shows a map including three routes of orders. We can conclude that the optimum routes under the vehicle capacity distances are generated using ODL.

Table 3 shows that a comparison of both methods when considering computing times (ct) is fair because the nv and ttd values and routes are similar. Fig. 8 shows comparisons of GAMS and ODL, in which the computing times increase when the number of orders is increased. We can also observe that the ODL heuristics becomes faster for routing the vehicles.

## 5.2. Results of proposed solution approach for medium data

There are no other real-world data or test instances for our problem, and a randomly generated test instance is implemented in addition to the given case study. The parameters are created

Fig. 7. Map result of routes created using open-source coding approach.

![Image](image_000007_8d622d32945d3636bd3406ed791e6aac1af52faf0c7f86ba72b10386f06b8763.png)

Fig. 8. Comparison of computing times for GAMS and ODL.

![Image](image_000008_fa482141a187348592b2ed2d37ee3281a95065a9e6768144c29e9ba6287ac432.png)

in- located onto Vehicle 1. There are no violations regarding the loading spaces.

| Parameters       | Values                |
|------------------|-----------------------|
| Order latitude   | UNIF(10) + 35         |
| Order longitude  | UNIF(10) + 27         |
| Package quantity | Rand(10)              |
| Weight           | UNIF(9000) + 1000     |
| Volume           | UNIF(29) + 1          |
| Width            | Rand([60 120])        |
| Height           | Rand([50 250])        |
| Depth            | Rand([50 200])        |
| Vehicle weight   | Rand([20,000 30,000]) |
| Width            | Rand([2 3])           |
| Height           | Rand([2 3])           |
| Length           | Rand([10 13])         |

through a uniform distribution in MATLAB, as shown in Table 4. The order latitude, longitude, weight, and volume values are created using a uniform distribution. The other parameters are randomly created. All parameters are created when considering realworld data.

The results were obtained from employing the proposed approach (clustering first, then packing-routing) on a medium test instance. The results demonstrate that 12 carrying units ( nv ) are used, and the total travel distance ( ttd ) is 25.3225km. In addition, the orders in the same cluster are placed into the same vehicle. These are compared with the values obtained using the exact method and the GA heuristics. However, it is fair to compare the running times because the data size is different. We also demonstrate in Fig. 9 the order positions when loaded into Vehicle 1 for Cluster 1. Considering the loading coordinates, 29 orders have been

Fig. 10 shows the routes of the carrying units loaded onto the trucks. Vehicle 1 for Cluster 1 travels to the order locations according to the latitudes and longitudes.

## 5.3. Results of the proposed solution approach for larger data

In this subsection, we present the computational results obtained for 3D-PRP. The vehicle numbers, order numbers, and positions of the orders for all clusters are described in this section. Our results show that four clusters are formed, in which all orders are located by considering the x, y, z coordinates. For instance, in Cluster 1, all orders are loaded onto four types of carrying units. We can see from Table 8 that the number of vehicles ( nv ) and total travel distance ( ttd ) are larger when compared to the medium test instance. We also demonstrate the order positions loaded onto Vehicle 1 for Cluster 1 in Fig. 10. Considering the loading coordinates, 31 orders were located on Vehicle 1. We report the clustering performance over two instances. The packing results were strongly improved by the clustering heuristics. Improvements of up to 60% and 45% are possible for nv and ttd . We can also see that the GA heuristics become faster for the input processed through machine learning.

Fig. 12 shows the routes of the carrying units loaded onto the trucks. Vehicle 1 for Cluster 1 travels to the order locations according to the latitudes and longitudes.

## 5.4. Experiment design

The design of the experiments was applied to justify the model behaviours under various conditions for the 3D-PRP results. A factorial design is provided, and the results are presented in Table 5.

![Image](image_000009_ab71e3c4fdf25b3c98b43a72ea06904aca6c1c861a9b76a686e5fda7e1e63523.png)

Fig. 10. Optimal route of Vehicle 1 for Cluster 1 for medium data.

![Image](image_000010_cbbac03de61bc6b771533c19eb5bb075c3b573c8bf1185fa8ed6d6833891a8e9.png)

Fig. 11. Order positions loaded onto Vehicle 1 for Cluster 1.

![Image](image_000011_20791675a7847208682d617e6a37be297cb642cffb798d565194ed764af0700c.png)

Fig. 12. Optimal route of Vehicle 1 for Cluster 1.

![Image](image_000012_fbc91bb8940048af02328f75987355844187c4fa36884451ffb8a3f57b2cfe2d.png)

Fig. 13. Effects of number of nodes and periods on the objective value and solution time for 3D-PRP.

![Image](image_000013_2df332047e3bec475a8d98127b10e9fadb733f710f502f34ee88904a9338fc7a.png)

![Image](image_000014_4c964932085a642be0b2e4d28e7f6b12270e373275b68fb250432ef34745354a.png)

Two-stage 3D-PRP results.

|   N |   K | PRP objective value   | 3D loading objective value   | Total objective value   | Solution time   |
|-----|-----|-----------------------|------------------------------|-------------------------|-----------------|
|  16 |   3 | 259,686               | 4900                         | 264,586                 | 35              |
|  12 |   6 | 3415,200              | 7520                         | 3422,720                | 18,567          |
|   8 |   3 | 3322,100              | 4500                         | 3326,600                | 15,345          |
|  16 |   3 | 568,935               | 45,670                       | 614,605                 | 67              |
|  16 |   6 | 2485,700              | 3400                         | 2489,100                | 45,467          |
|   4 |   6 | 1657,643              | 5400                         | 1663,043                | 654             |
|   8 |   6 | 456,345               | 23,300                       | 479,645                 | 125             |
|  12 |   3 | 2215,200              | 4670                         | 2219,870                | 12,430          |
|   8 |   3 | 836,595               | 4556                         | 841,151                 | 3456            |
|  12 |   3 | 2067,895              | 12,200                       | 2080,095                | 7890            |
|  16 |   6 | 3659,874              | 48,900                       | 3708,774                | 4356            |
|   4 |   3 | 562,569               | 3400                         | 565,969                 | 8               |
|   4 |   3 | 1564,569              | 3500                         | 1568,069                | 15              |
|   8 |   6 | 3574,569              | 7800                         | 3582,369                | 45,453          |
|  12 |   6 | 3658,945              | 12,300                       | 3671,245                | 45,400          |
|   4 |   6 | 1688,945              | 5600                         | 1694,545                | 250             |

Fig. 14. Interactions between number of nodes and periods on the objective value and solution time for 3D-PRP.

![Image](image_000015_82526b5fce3de6c3c2bddb0dfc390f0f1f2b266180936f1fc0486e9873872880.png)

![Image](image_000016_433053f96376e3a1bebe36b64af83ea797b022b7cc0d0f175167c671ad6602f8.png)

Fig. 15. Sensitivity analysis regarding the effects of parameters on the objective function.

![Image](image_000017_c11c60eac992b55f5115ff03bf650c2bf4bf29a63e5bae2fa2bff5c4c80759c5.png)

The number of nodes, including four levels (4, 8, 12, and 16), and periods, including two vehicles (3, 6), were chosen as factors. Thus, a 4 × 2 type factorial design was obtained. Two replications were applied for each experiment. The response variables were chosen as the total objective value involving the PRP and 3D loading values and the solution time.

Interactions of the numbers of nodes and periods on the problem are demonstrated in Fig. 14. The objective function is not significantly affected with eight and four nodes. However, for 16 and 12 nodes, the number of periods affects the objective value. However, the solution time is not significantly affected when the number of nodes is less than 16.

The main effects plot in Fig. 13 shows the effects of the numbers of nodes and vehicles in the problem. The objective value increases with an increasing number of nodes and periods until the number of nodes is 16. The solution time is affected based on the increasing number of periods. The results of the solution time are similar to those of the objective value.

## 5.5. Comparison of the methods with different instances

The methods used in these instances were compared based on the number of vehicles ( nv ) and the total travel distance ( ttd ), as shown in Table 6. There are no other real-world data or test

Table 6 Comparison of mathematical modelling, the proposed approach, and open-source coding approach.

|             | Two-stage approach   | Proposed approach (First cluster, then packing-routing)   | Open-source coding   |
|-------------|----------------------|-----------------------------------------------------------|----------------------|
| Instances   | nv ttd               | nv ttd                                                    | nv ttd               |
| Small data  | 3 4407               |                                                           | 3 4407               |
| Medium data |                      | 12 25.3225                                                |                      |
| Larger data |                      | 14 47.2043                                                |                      |

Train assignments.

| Trains               |   Detention | Assignment of Vehicle 1   | Assignment of Vehicle 2   | Assignment of Vehicle 3   | Assignment of Vehicle 8   |
|----------------------|-------------|---------------------------|---------------------------|---------------------------|---------------------------|
| Trieste-Koln         |       0.515 | X                         | X                         | X                         | X                         |
| Trieste-Ludwigshafen |       0.322 |                           |                           |                           |                           |

instances for our realistically sized instance. Thus, it is fair to compare the results of real-world and generated medium-sized instances. We can conclude that nv and ttd increase based on the order size, and clustering the backhauls ensures less travel distance. The results are as follows: ttd is 4407km for small data, approximately 25km for medium data, and approximately 47km for larger data, and Nv is 3 for small data, 12 for medium data, and 14 for larger data.

machine learning approach for 3D loading, and a GA for PRP. We proposed a new mathematical formulation for the train loading, which considers demurrage and detention costs.

## 5.6. Results of the train assignment problem

As mentioned above, the intermodal transport network proposed in the present study consists of road and rail transportation modes. The first stage of the intermodal transport network focuses on road transportation. The results of this stage are discussed Sections 5.1-5.3, and the output of the 3D-loading is the input of the train assignment problem. The carrying units (trailers or containers) of orders packed with 3D loading constraints are optionally assigned to the block trains for two different train stations. The carrying units are transferred by Roros in Turkey to Trieste, and then from Trieste to Koln or Ludwigshafen. Vehicle 1 is assigned to Trieste-Ludwigshafen, and the other vehicles are assigned to Trieste-Koln. The results are shown in Table 7.

Our results indicate that the proposed exact approach can solve realistic instances within a reasonable amount of time. We can conclude that the demurrage and detention affect the delays for Vehicles 2, 3, and 8. Orders are delivered to the customers with delays of 1, 2, and 3 days. We analysed the performance of the algorithm based on a sensitivity analysis, as shown in Fig. 15 (a, b, c). We can see from these figures that the objective function is mostly sensitive to the usage costs of the trains, and the demurrage and detention costs are negligible. Thus, it is not surprising to obtain an increase in the total cost when the usage costs of the trains are added.

## 6. Conclusion

In this study, we considered intermodal network problems combining pick-up routing problems with three-dimensional (3DPRP) loading constraints, clustered backhauls at the operational level, and train loading at a tactical level. To the best of our knowledge, the first cluster, then packing-routing described herein is the first of this type of approach. A main challenge is the proposed solution method in which four expert systems are employed to solve the combinatorial problems. Four small-sized instances are solved to optimality using GAMS and ODL. An open-source coding approach was compared with the exact solution method. The results show that ODL heuristics can solve the routing problem within seconds. To solve realistic-sized instances, we proposed hybrid meta-heuristics including k-means heuristics for clustering, a

This paper also presented both academic and practical implications. Regarding the academic aspects, the paper provided a new approach, called first cluster then packing-routing for 3DPRP, which is of significant value from a theoretical aspect because it includes two NP-hard problems: the pick-up routing problem and 3D loading. In addition, this is first time a machine learning approach for a 3D loading problem has been provided. Regarding the practical aspects, freight forwarders and decision makers should develop effective solutions for more complex intermodal networks. 3D-PRP plays a key role in real-world distribution logistics in which a container delivery of large items and loading is important. 3D-PRP has many transportation applications with intermodal means of transportation, including road, rail, and marine networks.

This paper has a limitation considering only road and rail transportation modes used between Turkey and European customers. To generalise the results, other regions can be added to the network to compare all alternative solutions. In addition, backhaul items were considered for this paper. This paper can be extended by considering both linehaul and backhaul items. Separate compartments can be assigned for these items using the variable neighbourhood search from Bortfeldt et al. (2015). The application of clustering first, packing-routing second is based on the assumption that the k-means clustering is used to cluster the backhaul customers using the Euclidean distance. However, Bortfeldt and Homberger (2013) consider customer combinations with a high-volume utilisation. Customer combinations that avoid packing losses, long travel distances can be used for future studies. For future research, pick-up routing with 3D loading can be extended under various constraints such as the time-window concept and delivery options to obtain more realistic solutions. The proposed load-planning models can be extended by incorporating the fleet size, empty container repositioning, and fleet expansion decisions. Furthermore, exact fuzzy-based exact approaches from von Westarp and Schinas (2016) that can handle the uncertainty of transport demands, transit times, freight capacities, and cost parameters may also be incorporated as a future study. During the train loading stage, consolidated orders are shipped using ro-ro. In this paper, the marine environment of ro-ro (waves, wind, and storm) and the ro-ro speed are negligible. However, considering these conditions is highly relevant to actual practice.

## Conflict of interest

None.

## Supplementary material

Supplementary material associated with this article can be found, in the online version, at doi:10.1016/j.eswa.2019.06.023.

## Credit authorship contribution statement

Elifcan Göçmen: Conceptualization, Methodology, Software, Resources, Data curation, Writing -original draft, Investigation. Rızvan Erol: Conceptualization, Methodology, Writing -original draft, Supervision, Data curation, Validation, Visualization.

## References

Anghinolfi, D., &amp; Paolucci, M. (2014). General purpose Lagrangian heuristic applied to the train loading problem. Procedia Social and Behavioral Sciences, 108 , 37-46. Barnhart, C., &amp; Ratliff, H. (1993). Modelling intermodal routing. Journal of Business Logistics, 14 , 205-223.

- Bauer, J., Bekta¸ s, T., &amp; Crainic, T. G. (2010). Minimizing greenhouse gas emissions in intermodal freight Transport: An application to rail service design. Journal of Operational Research Society, 61 (3), 530-542.

Bierwirth, C., Kirschstein, T., &amp; Meisel, F. (2012). On transport service selection in intermodal rail/road distribution networks. Business Research, 5 (2), 198-219.

- Bortfeldt, A. (2012). A hybrid algorithm for the capacitated vehicle routing problem with three-dimensional loading constraints. Computers &amp; Operations Research, 39 (9), 2248-2257.
- Bortfeldt, A., &amp; Homberger, J. (2013). Packing First, routing SecondA Heuristic for the vehicle routing and loading problem. Computers &amp; Operations Research, 40 (3), 873-885.
- Bortfeldt, A., Hahn, T., Männel, D., &amp; Mönch, L. (2015). Hybrid algorithms for the vehicle routing problem with clustered backhauls and 3D loading constraints. European Journal of Operational Research, 243 (1), 82-96.
- Bergqvist, R., &amp; Behrends, S. (2011). Assessing the effects of longer vehicles: The case of pre- and post-haulage in intermodal transport chains. Transport Reviews, 31 (5), 591-602.
- Bruns, F., &amp; Knust, S. (2012). Optimized load planning of trains in intermodal transportation. OR Spectrum, 34 (3), 511-533.
- Ceshia, S., &amp; Schaerf, A. (2013). Local search for a multi-drop multi-container loading problem. Journal of Heuristics, 19 (2), 275-294.
- Chen, C. S., Lee, S. M., &amp; Shen, Q. S. (1995). An analytical model for the container loading problem. European Journal of Operational Research, 80 (1), 68-76.
- Corry, P., &amp; Kozan, E. (2006). An assignment model for dynamic load planning of intermodal trains. Computer &amp; Operation Research, 33 (1), 1-17.
- Corry, P., &amp; Kozan, E. (2008). Optimised loading patterns for intermodal trains. OR Spectrum, 30 , 721-750.
- Feo, T. A., &amp; Gonzales-Velarde, J. L. (1995). The intermodal trailer assignment problem. Transportation Science, 29 , 330-341.
- Gendreau, M., Iori, M., Laporte, G., &amp; Martello, S. (2006). A Tabu search algorithm for a routing and container loading problem. Transportation Science, 40 (3), 342-350.
- Gendreau, M., Iori, M., Laporte, G., &amp; Martello, S. (2008). A Tabu search heuristic for the vehicle routing problem with two dimensional loading constraints. Networks, 51 (1), 4-18.
- Janic, M., &amp; Vleugel, J. (2012). Estimating potential reductions in externalities from rail-road substitution in trans-european freight transport corridors. Transportation Research Part D: Transport and Environment, 17 (2), 154-160.
- Junqueira, L., Oliveria, J. F., Carravilla, M. A., &amp; Morabito, R. (2013). An optimization model for the vehicle routing problem with practical three-dimensional loading constraints. International Transactions in Operational Research, 20 (5), 645-666.
- Lacomme, P., Toussaint, H., &amp; Duhamel, C. (2013). A grasp x ELS for the vehicle routing problem with basic three-dimensional loading constraints. Engineering Applications of Artificial Intelligence, 26 , 1796-1810.
- Lerhlaly, S., Lebbar, M., Allaoui, H., Ouazar, D., &amp; Afifi, S. (2016). An integrated inventory location routing problem considering CO2 emissions. Contemporary Engineering Sciences, 9 , 303-314.
- Min, H. (1991). International intermodal choices via chance constrained goal programming. Transportation Research Part A:GEneral, 25 (6), 351-362.
- Pishvaee, M. S., Farahani, R. Z., &amp; Dullaert, W. (2010). A memetic algorithm for bi-objective integrated forward/reverse logistics network design. Computers &amp; Operations Research, 37 (6), 1100-1112.
- Reil, S., Bortfeldt, A., &amp; Mönch, L. (2018). Heuristics for vehicle routing problems with Backhauls, time Windows, and 3D loading constraints. European Journal of Operational Research, 266 (3), 877-894.
- Ruan, Q., Zhang, Z., Miao, L., &amp; Shen, H. (2013). A hybrid approach for the vehicle routing problem with three-dimensional loading constraints. Computers &amp;Operations Research, 40 (6), 1579-1589.
- Serper, E. Z., &amp; Alumur, S. A. (2016). The design of capacitated intermodal hub networks with different vehicle types. Transportation Research Part B: Methodological, 86 , 51-65.
- Sterzik, S., &amp; Kopfer, H. (2013). A Tabu search heuristic for the inland container transportation problem. Computers &amp; Operations Research, 40 (4), 953-962.
- Tarantilis, C. D., Zachariadis, E. E., &amp; Kiranoudis, C. T. (2009). A hybrid metaheuristic algorithm for the integrated vehicle routing and three-dimensional container-loading problem. IEEE Transactions on Intelligent Transportation Systems, 10 (2), 255-271.
- Tao, Y., &amp; Wang, F. (2015). An efficient Tabu search approach with improved loading algorithms for the 3L-CVRP. Computers &amp; Operations Research, 55 , 127-140.
- Von Westarp, A. G., &amp; Schinas, O. (2016). A fuzzy approach for container positioning considering sustainable profit optimization. Transportation Research Part E: Logistics and Transportation Review, 92 , 56-66.
- Xiao, J., Pachl, J., Lin, B., &amp; Wang, J. (2018). Solving the Block-to-Train assignment problem using the heuristic approach based on the genetic algorithm and Tabu search. Transportation Research Part B, 108 , 148-171.
- Yang, G., &amp; Mu, C. (2018). Machine learning approach to shipping box design. In Proceedings of the 13th INFORMS workshop on data mining and decision analytics .
- Zachariadis, E. E., Tarantilis, C. D., &amp; Kiranoudis, C. T. (2013). Designing vehicle routes under time windows and loading con-
- for a mix of different request Types, straints. European Journal of Operational Research, 229 (2), 303-317.
- Zu, W., Qin, H., Lim, A., &amp; Wang, L. (2012). A two stage tabu search algorithm with enhanced packing heuristic for the 3L-CVRP and M3L-CVRP. Computers &amp; Operations Research, 39 , 2178-2195.