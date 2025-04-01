## Distribution planning using capacitated clustering and vehicle routing problem

## A case of Indian cooperative dairy

Anubha Rautela, S.K. Sharma and P. Bhardwaj Indian Institute of Technology (BHU) Varanasi,

Department of Mechanical Engineering, Varanasi, India

## Abstract

Purpose -The purpose of this paper is to reduce the distribution cost of an Indian cooperative dairy. The reduction of cost was achieved with the application of the clustering method (k-means clustering) and capacitated vehicle routing problem (cheapest link algorithm (CLA)).

Design/methodology/approach -Capacitated k-means clustering was used to split delivery locations into similar size groups (i.e. clusters) based on proximity without exceeding a specified total cluster capacity. Each cluster would be served by a local stockist. CLA was then used to find delivery routes from dairy (i.e. depot) to stockist in each cluster and from stockist to all other delivery locations within the cluster.

Findings -K-means clustering and CLA suggested optimal delivery routes on which vehicles will run. The complete algorithm was able to provide a solution within 30 s.

Practical implications -Clustering of delivery locations and use of heterogeneous fleet of delivery vehicles can result in considerable savings in daily operational cost.

Originality/value -Most of the research related to the use of demand clustering to improve distribution routes has been theoretical, which do not take into account real-world limitations like vehicle s ' specific limitations. The authors tried to address that gap by taking a real-world case of a cooperative dairy and compared the result with existing distribution routes used by dairy. This work can be used by other dairies or distribution companies according to their scenario.

Keywords Capacitated vehicle routing problem (CVRP), Cheapest link algorithm (CLA),

Clustering problem, Distribution cost, K-means clustering

Paper type Case study

## 1. Introduction

India is the largest producer of mammalian milk and the second largest producer of cow milk (FAOSTAT, 2012). Increasing per capita consumption has made India the largest consumer of milk (Nozaki, 2017). This growth has been partly supported by the more than 76,000 villagelevel and 170 district-level cooperatives (Rajendran and Mohanty, 2004). Not all of these cooperatives have been able to keep up with the demands of end consumers. Instead of going to buy milk, consumers prefer it to have it delivered on time. Therefore, the improved distribution of milk from dairy to customers within the desired time window is important.

Distribution refers to the commercial activities of transporting goods (or services) to customers (Chow et al. , 1994) and is regarded as a branch of supply chain management concerned with the supply and delivery of goods (Cosimato and Troisi, 2015). Companies sometimes prefer third-party logistics to minimise costs and improve customer service (Lieb, 1992; Lambert and Stock, 1993). Companies that produce goods or

Indian cooperative dairy

781

Received 18 December 2018 Revised 10 April 2019 20 April 2019

Accepted 20 April 2019

![Image](image_000000_0df048cfd22b59b28f41cc0fbbf8940ce664917e8b0445f2d8dbe5866a483f98.png)

Journal of Advances in Management Research Vol. 16 No. 5, 2019 pp. 781-795 © Emerald Publishing Limited 0972-7981

## JAMR 16,5

782

services that need to be delivered to customers need a good distributor that can handle the planning of the delivery (Lummus et al. , 2001). For example, a cooperative dairy may be required to deliver milk to several service locations in a city, which requires a well-planned distribution system to deliver the product at minimum cost.

To address distribution and delivery challenges, Dantzig and Ramser (1959) introduced the vehicle routing problem (VRP). The core function of this concept is to provide delivery services to customers, or any other concerned organisation related to logistics (Toth and Vigo, 2014). Therefore, its primary objective is to determine the set of optimal routes that can minimise either the cost of transportation, transportation time, the number of vehicles required for delivery or a combination of the three factors.

The relationship between the buyer (customer) and the seller is vital in any supply chain. Therefore, the role of logistics departments involves improving this relationship by determining the most effective and efficient transportation routes (Bowersox et al. , 2002). The VRP approach addresses this problem by identifying the routes for delivery vehicles to meet customer demands at a minimum distribution cost, hence improving profit margins. Companies employ the VRP to identify the optimal set of routes with a limited fleet capacity to distribute their products, with a known demand and specific service time.

Different alternates to the VRP that have been developed (Li, 2008; Laporte, 1992), including the capacitated vehicle routing problem (CVRP), the VRP with time windows, the VRP with pickup and delivery and the heterogeneous fleet VRP. Among the four variants of the VRP listed above, the CVRP is the most widely used.

The CVRP is a VRP with limits on the number of deliverables vehicles can carry. In the CVRP, demand exists in each location corresponding to the number of items to be picked up or delivered. The total demand at any location, or along any vehicle route, should not be more than the capacity of the delivery vehicle. The CVRP also has other constraints:

- (1) each customer should be visited only once;
- (2) vehicle routes start and end with a depot;
- (3) the total transportation time with service time for a route cannot surpass the upper limit; and
- (4) the overall cost of routes should be minimised.

Companies can significantly reduce delivery costs by grouping customers into an optimum number of clusters, with a consideration of minimising delivery costs for each cluster; this strategy is referred to as clustering. Clustering can be defined as a classification of patterns into groups (Barreto et al. , 2007). In a clustering problem, an algorithm can take one of two forms: the partitioning type or the hierarchical type. A partitioned clustering algorithm defines all clusters together. A hierarchical algorithm, on the other hand, seeks to find successive clusters using pre-determined clusters. The distance used in the clustering, which may be symmetric or asymmetric, is also essential. It is also vital to choose a distance measure that governs similarity between items since this influences the cluster shape. The Euclidean distance is generally used as the measure of distance. Customers are then grouped based on their demand and Euclidean distance. The capacity limit is also considered in the formation of clusters. This problem is defined as the capacitated clustering problem (CPP), which refers to the grouping of items into clusters to minimise costs with the indicated capacity constraints. The CPP also offers a solution to the VRP (Dial, 1979). This case study seeks to develop a new set of routes and capacitated clusters for a cooperative dairy to minimise the milk distribution expenses for delivery to different service locations and, in turn, demonstrate that using the capacitated clustering and VRP methods can minimise the related distribution costs.

## 2. Literature review

Ai and Kachitvichyanukul (2009) found a solution to the CVRP using two solution representation decoded technique with the particle swarm optimisation procedure. Similarly, the artificial bee colony algorithm (Cura, 2013; Szeto et al. , 2011), the ant colony optimisation algorithm (Toklu et al. , 2016), the tabu search algorithm (Seifbarghy and Samadi, 2014; Khambhampati et al. , 2018; Caballero-Morales et al. , 2018), the hybrid particle swarm algorithm (Kechmane et al. , 2018), the hybrid of ant colony and firefly algorithm (Goel and Maini, 2018), the simulated annealing algorithm (Karagul et al. , 2019) and hybrid simulated annealing were also used to solve the CVRP, resulting in improved outcomes (Lin et al. , 2011).

In the past decade, several heuristic and assignment techniques have been developed to solve the CPP. Mulvey and Beck (1984) suggested a heuristic approach and used arbitrarily produced cluster centres as a solution to the CCP. Koskosidis and Powell (1992) expanded on the work of Mulvey and Beck (1984) by suggesting an iterative algorithm that was found to be more operative than other empirical algorithms. This algorithm does not require the speci /uniFB01 cation of seed clients, which other algorithms do. Mulvey and Beck (1984) developed a distinct algorithm for seed selection instead of random seed generation. The iterative heuristic approach uses a self-correcting structure in three stages:

- (1) greedy assignment;
- (2) seed relocation; and
- (3) local exchange.

Thangiah and Gubbi (1993) used an original algorithm to attain a suitable cluster of customers for the HVRP with a ' cluster /uniFB01 rst and route second ' algorithm. Bujel et al. (2018) used density-based spatial clustering of applications with noise clustering to solve the CVRP. Osman and Christofides (1994) proposed using the tabu search and hybrid simulated annealing approaches to solve the CCP. Shieh and May (2001) applied the genetic algorithm to crack the CCP. In this algorithm, binary coded strings are used to symbolise the chromosomes, which eradicates the incidence of infeasible solutions. The chromosome is split into two fragments: one signifies the consumers, and the other denotes the seeds of the clusters. To handle the capacity constraints, an adaptive penalty function is applied, which improves the convergence and quality of the solution. França et al. (1999) created a novel adaptive tabu search method to solve the CCP, using two neighbourhood generation methods of the local search experiment: insertion and pair-wise interchange. Ž alik has presented a k-means algorithm that achieves accurate clustering. This method does not require pre-assigning of the exact number of clusters ( Ž alik, 2008). In this paper, the clusters are created using the k-means algorithm for solving CCP. This method seems competitive and straightforward as compared to others.

The cheapest link algorithm (CLA) is also used for solving the VRP. This algorithm involves a graph with numbers assigned to the edges (Larsson and Patriksson, 1995), in which the numbers represent the weights of the edges. This algorithm leads to a Hamilton circuit. In this research, the VRP was solved using the CLA.

The CLA is defined as an idea to piece together a tour by selecting the separate links of a tour by cost. It does not matter if the links in the intermediate stages are all over

Indian cooperative dairy

783

## JAMR 16,5

784

Figure 1. Graphical representation of cooperative dairy sales from 2010 -2011 to 2016 -2017

the place. If one is careful, the links all come together at the end and form a tour. This algorithm works by choosing the cheapest edge of the graph, whichever edge it may be, after which the next cheapest edge of the graph is selected: the process of selecting the cheapest edge available continues. This step is completed while ensuring that a circuit does not develop at the very end and that no three edges converge at a vertex (Tannenbaum, 2013).

The k-means clustering method, first proposed by Mac Queen in 1967, is the most commonly used method of clustering. However, Hugo Steinhaus had a similar idea in 1957, and Stuart Lloyd first suggested the standard algorithm for pulse-code modulation in the same year. At times, scholars refer to this concept as Lloyd -Forgy because, in 1965, E.W. Forgy also published the same method. Later, Wang and Su (2011) presented an improved k-means algorithm, addressing the imperfections of the conventional k-means clustering algorithm. Geetha et al. (2009) used improved k-means clustering for the CCP for better optimisation results. The algorithm develops density-based detection methods based on the characteristics of noise data by adding the discovery and processing steps of the noise data to the original algorithm. The data are processed to eliminate these noise data before clustering data sets, resulting in a significant reduction of the noise data and a significant improvement of the clustering results. Singh (2015) stated that the runtime of the k-means algorithm is favourable and used several approaches to improve the k-means clustering method.

## 3. Problem definition and algorithm

This case study is based on a cooperative dairy located in Varanasi, India. Presently, the dairy is losing its market base (see Figure 1) and customers to competitors and new entrants. After a thorough study and analysis (house of quality method), the top three concerns identified included market planning, distribution cost and product-line expansion.

The authors focused their research to minimise distribution costs. Presently, the cooperative dairy studied during this research uses seven high-capacity vehicles

![Image](image_000001_5d265cffda45d5f07835ff8a810188dd6252ddf159ed4f609aa5aed919cae111.png)

(i.e. 2,700 litres, with milk packets stored in bins of equal sizes) for their seven existing routes, paying 1.3 Indian rupees (INR) per litre per day. The cooperative dairy has a homogeneous vehicle fleet, but the demands are lower on some routes (e.g. Routes 2 and 4). Despite the low demand in some areas, the dairy uses high-capacity vehicles. Therefore, the way cooperative dairy distributes milk at present does not seem optimal and needs to be remodelled.

Some authors (Rautela et al. , 2017) have used the centre-of-gravity (CG) method by converting one retailer into a stockist at the CG point of a particular route, as the same was repeated for all the routes. The CG method revealed that the approximate per day savings in distribution costs were 17.09 per cent per day. Although there was substantial reduction in distribution costs, the drawback was that the existing routes were used.

In this study, the new routes are identified with the help of k-means clustering and the CLA to see if the costs can be reduced further. Table I displays the data regarding routes, demand and cost in the respective routes.

The constraints used for the k-means clustering and the CLA are as follows:

- (1) capacity constraint;
- (2) distance constraint; and
- (3) demand constraint.

The algorithm used for the problem is presented below:

- · I ¼ Total number of simulation instances
- · G ¼ Number of clusters
- · Dji ¼ Demand of location i in cluster j
- · Pji ¼ Cost of delivery to all demand locations in cluster Cj in simulation instance i :

$$P _ { i } = \sum _ { j = 1 } ^ { G } P _ { j i }.$$

- · Pm ¼ min( S ), S ¼ { P 1 , P 2 , P 3 , … , PI }
- · AD max ¼ Maximum Capacity of large vehicle

| Route   | Requirement (litre)   | Cost incurred (INR)                       |
|---------|-----------------------|-------------------------------------------|
| No. 1   | 2,629                 | 3,417.70                                  |
| No. 2   | 931                   | 1,210.30                                  |
| No. 3   | 1,250                 | 1,625.00                                  |
| No. 4   | 563                   | 731.90                                    |
| No. 5   | 1,438                 | 1,869.40                                  |
| No. 6   | 1,896                 | 2,460.90                                  |
| No. 7   | 1,584                 | 2,059.20 Table I. Details of routes, milk |
| Total   | 10,291                | 13,378.30 distributed and costs           |

Indian cooperative dairy

785

JAMR

16,5

786

```
P_m = \infty, i = 0
while i < I do
  |  createClusters()
  |  solveVRP_factoryToStockist()
  |  solveVRP_stockistToDeliveryPoints()
  |  P_i = \sum _j=1 P_ji
end
```

return Pm =

```
Procedure solveVRP_factoryTo
  |   depot = Dairy Factory
  |   locations = All Stockists
  |   D _ { j } = \sum _ { i = 1 } ^ { N } D _ { j i }
  |   solveVRP()
  |   return
```

Procedure solveVRP\_stockistloDeliveryPoints()

```
Procedure solveVRP_stockistToDeliveryPoints
    |
```

## Procedure createClusters ()

```
Procedure createClusters()
    D _ { \max } = \infty
    while D _ { \max } > A D _ { \max } do
      |   Create G clusters using K-means
      |   D _ { \max } = max(D)
    end
    return
```

The algorithm is explained below in simple language for easier understanding:

- · Step 1: Minimum\_Cost ¼ Infinity; Iterations ¼ 0.
- · Step 2: create clusters:
- -create clusters using simple k-means clustering;
- -calculate the total demand for each cluster;
- -if the total demand for any cluster is more than the capacity of the largest vehicle, then go to Step 2.1; and
- -if the cluster contains a factory, make the factory a cluster centre.
- · Step 3: solve the VRP for factory to stockists (cluster centres):
- -set capacity constraints for each vehicle type;
- -set travel distance constraints for each vehicle type;
- -set the demand of each stockist/cluster centre equal to the total demand of the cluster; and
- -solve the VRP with the milk factory as the depot and the stockists as delivery locations.
- · Step 4: solve the VRP for each cluster separately (i.e. cluster centre to delivery points):
- -set capacity constraints for each vehicle type;
- -set travel distance constraints for each vehicle type; and
- -solve the VRP with the cluster centre as the depot and other points in the cluster as delivery locations.
- · Step 5: calculate the total cost based on travel distance, vehicle labour and stockist storage cost.
- · Step 6: if Total\_Cost o Minimum\_Cost, then assign Minimum\_Cost ¼ Total\_Cost.
- · Step 8: if Iterations o Max\_Iterations, repeat Step 2.
- · Step 7: Iterations ¼ Iterations + 1.

## 4. Research methodology

In this study, the k-means clustering and capacitated vehicle routing methods were implemented to identified new routes. Capacitated k-means clustering was used to split delivery locations into similar size groups (i.e. clusters) based on proximity without exceeding a specified total cluster capacity. Each cluster was to be served by a local stockist. The CLA was then used to find delivery routes from dairy (i.e. depot) to stockist in each cluster and from stockist to all other delivery locations within the cluster.

The problem was solved in two phases. These phases are described as follows:

- (1) Clustering: to divide the problem into smaller parts, the service locations were grouped into clusters (Shieh and May, 2001). Delivery locations were grouped by their closeness to each other. Instead of solving the CVRP for all the delivery locations together, we solved the CVRP for each cluster separately, thus reducing complexity and increasing the optimality of our heuristic solution.
- (2) The CVRP was solved for:
- · the depot to cluster centres (stockists); and
- · each cluster centre (stockist) to each delivery location within the cluster.

## 4.1 Clustering

The first phase in clustering was to partition service locations and the depot into clusters based on their proximity to each other. Numerous clustering methods exist, but not all of them proved suitable for this research. Our criteria for choosing a suitable clustering algorithm were therefore influenced by the following factors:

- (1) Even cluster sizes: if some clusters were small, then others would be big and the larger the cluster, the more complicated it would be to solve the CVRP for it. Evenly distributed cluster sizes were therefore preferred.

Indian cooperative dairy

787

## JAMR 16,5

788

- (2) Not too many clusters: having too many clusters would make the CVRP for the depot to cluster centres complex. With increasing complexity, the optimality of the solution would diminish.

For comparison, both agglomerative and k-means clustering were used on the same data set. The k-means clustering method produced better results for our case. Figures 2 and 3, in which each colour represents a cluster, depict this comparison visually.

Here, lower standard deviation values are better. Keeping all the factors in mind, we chose the k-means clustering algorithm. In Figures 2 and 3, the longitude and latitude appear on the x -axis and y -axis, respectively. The coloured points in the figures represent the delivery locations. Locations grouped into the same cluster were assigned the same colour.

Note: /afii9846 cluster size =3.8

![Image](image_000002_a77a3c279f906207ae214fed1c6e9f073e361606d845b5dd83d43998e252cd19.png)

![Image](image_000003_33260fa8284ebd91249deaa4903ada84e740ea65766de290cedbb5a118eb46c6.png)

## 4.2 Capacitated vehicle routing problem (CVRP)

After successfully grouping a total of 58 demand locations through k-means clustering based on the proximity of all the demand points to each other, a total of six clusters were identified. Each cluster had a centre point designated as a stockist.

Furthermore, to find the best route to deliver milk from the depot to stockists and then to all other service locations, the work was done in two parts:

- (1) we had to find optimal routes for deliveries from the depot (dairy) to each cluster centre/stockist; and
- (2) then, we had to determine the optimal routes for deliveries from each stockist to service locations in that cluster.

Some additional considerations were taken into account:

- (1) Large delivery trucks would only be used for delivery from the depot to stockists. Given the on-the-ground reality that large delivery trucks were hard to find for a reasonable rate, we had to limit their use in the overall delivery plan.
- (2) Small delivery trucks were cheaper and easier to find but had much less capacity than large trucks. The maximum distance they could travel was also less compared to large trucks.
- (3) We faced a time constraint of 2 h from the depot to service locations.

The CLA was used to solve the CVRP. The flowchart for CLA (see Figure 4) illustrates how to create a Hamilton circuit (Berge, 1973), a set of directed edges (paths in the VRP) such that each vertex (delivery point) is visited only once. One can think of vertices as delivery locations and edges as the delivery route. The weights associated with each edge represent the cost of that route. In the context of delivery, the algorithm below is capable of finding an optimal solution to the VRP.

## 5. Results

Figure 5 depicts the clusters and routes made by K-means and the CLA. The algorithm identified a total of six clusters. Different colours represent the different clusters. Our problem consists of 58 total location points and Point 0 represents the depot (dairy).

The high- and low-capacity vehicles cost INR 25 km and INR 20 km, respectively, and the labour costs for high- and low-capacity vehicles was INR 300 and INR 100, respectively. The main source of high-capacity vehicles was the dairy (the depot), and the destinations were the stockists (the centre of each cluster). The source of low-capacity vehicles were the stockists (the centre of each cluster), and the destinations were other customers (the delivery locations). All the points had their own demands, and the cooperative dairy provided a refrigeration system to the points identified as stockists. The equated monthly instalment for refrigerators amounted to INR 25/day for each stockist. Here, the Python programming language was used to solve the problem.

Table II displays the stockist points (1, 5, 40, 23 and 45) that supply the milk using high-capacity vehicles (2,700 litres).

Table III contains information on the supply from the stockist point -in this case, the Point 0 (dairy/depot). Here, a low-capacity vehicle (500 litres) was used for the location point 44, along with another smaller vehicle for the location Points 9, 12 and 43.

Similarly, Tables IV -VIII display the supply of milk from the stockist Points 1, 5, 23, 40 and 45, respectively, to smaller retailer points with low-capacity (500 litres) vehicles.

For example, Table IV lists figures regarding the supply of milk from the stockist Point 1 to other smaller retailer points (7, 57, 54, 58, 2, 56, 55, 51 and 8).

Indian cooperative dairy

789

Table IX contains data on the gross total cost of delivery per programme. The total calculated cost of the delivery from the k-means clustering and CVRP methods was INR 10,387.46 -INR 2,990.84 lower than the present delivery cost of INR 13,378.3 (from Table I), which is a reduction of approximately 22.35 per cent per day in delivery costs. Thus, the new route yielded significant savings in distribution costs, which could represent substantial savings for the cooperative dairy.

![Image](image_000004_ee4ff2bffd2f6816f8ed67ad87dfa217d6dca03f520a371f2874b9c9ec500b0a.png)

Note: Each colour represents a cluster

![Image](image_000005_b60b8024af5931b077e0e383b25b8b460bf98d9b4b3886d7cd65fe3f42243d79.png)

Indian cooperative dairy

791

Figure 5. Delivery routes

| Starting point   | Points visited   | Demand fulfilled   | Cost (Rupees)   |
|------------------|------------------|--------------------|-----------------|
| 0                | 1                | 2,490              | 1,264.50        |
| 0                | 5                | 2,691              | 1,088.52        |
| 0                | 40, 23           | 1,848              | 1,256.45        |
| 0                | 45               | 2,607              | 765.92          |
|                  |                  | 9,636              | 4,375.39        |

Table II.

Route for larger vehicles leaving

from the dairy

| Starting point   | Points visited   |   Demand fulfilled |   Cost (Rupees) | Table III.        |
|------------------|------------------|--------------------|-----------------|-------------------|
| 0 0              | 44               |                252 |          394.4  | Route for smaller |
|                  | 9, 12, 43        |                403 |          480.92 | vehicles leaving  |
| Sum              |                  |                655 |          875.32 | from the dairy    |

| Starting point   | Points visited   | Demand fulfilled   | Cost (Rupees)   |                     |
|------------------|------------------|--------------------|-----------------|---------------------|
| 1                | 7                | 148                | 113.24          |                     |
| 1                | 57, 54           | 498                | 305.74          |                     |
| 1                | 58, 2, 56, 55    | 437                | 426.36          | Table IV.           |
| 1                | 51, 8            | 454                | 309.46          | Route from stockist |
| Sum              |                  | 1,537              | 1,154.80        | Point 1             |

The k-means clustering and the CLA methods suggested optimal routes with the algorithm runtime in seconds. A short algorithm runtime means that new delivery locations can be added or removed, and new delivery routes can be calculated as frequently as needed. Here, the average runtime of the complete programme was approximately 30 s.

## JAMR 16,5

## 792

## Table V.

Route from stockist

Point 5

## Table VI.

Route from stockist Point 23

## 6. Conclusion

As a study on a cooperative dairy located in Varanasi, India, this research employed the clustering model (k-means clustering) and CVRP (CLA) with the aim of minimising distribution costs with constraints of distance, vehicle capacity and demand. Initially, clustering was completed with the k-means clustering method because it generated evenly sized clusters, and then the CVRP was solved for the clusters and for the depot to cluster centres. This approach led to a reduction of 22.35 per cent in distribution costs per day, resulting in significant savings. The research revealed that vehicle routing and clustering can significantly reduce the distribution costs in an organisation (Blumenfeld et al. , 1987). Applying clustering to divide a problem can result in short algorithm runtimes and optimal solutions.

| Starting point   | Points visited   | Demand fulfilled   | Cost (Rupees)   |
|------------------|------------------|--------------------|-----------------|
| 5                | 52, 53           | 472                | 509.40          |
| 5                | 3, 10            | 406                | 176.64          |
| 5                | 6                | 486                | 166.54          |
| 5                | 18, 19, 11, 20   | 368                | 291.64          |
| 5                | 21, 4            | 463                | 166.12          |
| Sum              |                  | 2,195              | 1,310.34        |

| Starting point   | Points visited     |   Demand fulfilled |   Cost (Rupees) |
|------------------|--------------------|--------------------|-----------------|
| 23               | 13, 14, 32         |                261 |          495.46 |
| 23               | 38, 16, 17, 25, 24 |                383 |          393.6  |
| Sum              |                    |                644 |          889.06 |

|                     | Starting point   | Points visited     |   Demand fulfilled |   Cost (Rupees) |
|---------------------|------------------|--------------------|--------------------|-----------------|
| Table VII.          | 40               | 39                 |                203 |          118.03 |
| Route from stockist | 40               | 33, 30, 35, 34, 41 |                476 |          396.12 |
| Point 40            | Sum              |                    |                679 |          514.16 |

|                     | Starting point   | Points visited     | Demand fulfilled   | Cost (Rupees)   |
|---------------------|------------------|--------------------|--------------------|-----------------|
|                     | 45               | 37, 22, 46         | 459                | 332.04          |
|                     | 45               | 49, 36, 50         | 499                | 241.48          |
|                     | 45               | 47                 | 353                | 188.06          |
| Table VIII.         | 45               | 27, 26, 15, 31, 42 | 450                | 289.62          |
| Route from stockist | 45               | 48, 29, 28         | 374                | 217.20          |
| Point 45            | Sum              |                    | 2,135              | 1,268.40        |

| Table IX.           | Points     | 0        | 1        | 5        |     23 |     40 | 45       | Gross total   |
|---------------------|------------|----------|----------|----------|--------|--------|----------|---------------|
| Total delivery cost | Total cost | 5,250.71 | 1,154.80 | 1,310.34 | 889.06 | 514.16 | 1,268.40 | 10,387.46     |

Alimitation of our methodology is the availability of stockists. When a stockist could not be set up at the preferred location in a cluster, we had sub-optimal delivery routes and, in turn, increased distribution costs.

The Indian dairy consumer market is the largest in the world (FAOSTAT, 2012), and the need for efficient and flexible distribution is more important than ever before. With the increase in the number of dairy brands, consumers now have many more options, which can affect demand. Therefore, companies in the industry need to frequently update their delivery networks. This paper attempts to help meet this need by providing a methodology to create optimal delivery routes that can be updated frequently.

Future researchers could expand on this work by including other parts of the region where demand for milk does not exist, with an aim to augment demand. Alternatively, scholars could investigate the distribution of multiple dairy products instead of focussing solely on milk.

## References

Ai, T.J. and Kachitvichyanukul, V. (2009), ' Particle swarm optimization and two solution representations for solving the capacitated vehicle routing problem , ' Computers &amp; Industrial Engineering , Vol. 56 No. 1, pp. 380-387.

Barreto, S., Ferreira, C., Paixao, J. and Santos, B.S. (2007), ' Using clustering analysis in a capacitated location-routing problem , ' European Journal of Operational Research , Vol. 179 No. 3, pp. 968-977.

Berge, C. (1973), Graphs et Hypergraphes , American Elsevier, New York, NY.

Blumenfeld, D.E., Burns, L.D., Daganzo, C.F., Frick, M.C. and Hall, R.W. (1987), ' Reducing logistics costs at General Motors , ' Interfaces , Vol. 17 No. 1, pp. 26-47.

Bowersox, D.J., Closs, D.J. and Cooper, M.B. (2002), Supply Chain Logistics Management , McGraw-Hill Irwin, Boston, MA.

Bujel, K., Lai, F., Szczecinski, M., So, W. and Fernandez, M. (2018), ' Solving high volume capacitated vehicle routing problem with time windows using recursive -DBSCAN clustering algorithm , ' Cornell University, Ithaca, NY.

Caballero-Morales, S.O., Martínez-Flores, J.L. and Sánchez-Partida, D. (2018), ' An evolutive tabu-search metaheuristic approach for the capacitated vehicle routing problem , ' in García-Alcaraz, J., Alor-Hernández, G., Maldonado-Macías, A. and Sánchez-Ramírez, C. (Eds), New Perspectives on Applied Industrial Tools and Techniques , Springer, Cham, pp. 477-495.

Chow, G., Heaver, T.D. and Henriksson, E.L. (1994), ' Logistics performance: definition and measurement , ' International Journal of Physical Distribution &amp; Logistics Management , Vol. 24 No. 1, pp. 17-28.

Cosimato, S. and Troisi, O. (2015), ' Green supply chain management: practices and tools for logistics competitiveness and sustainability: the DHL case study , ' The TQM Journal , Vol. 27 No. 2, pp. 256-276.

Cura, T. (2013), ' An artificial bee colony approach for the undirected capacitated arc routing problem with profits , ' International Journal of Operational Research , Vol. 17 No. 4, pp. 483-508.

Dantzig, G.B. and Ramser, J.H. (1959), ' The truck dispatching problem , ' Management Science , Vol. 6 No. 1, pp. 80-91.

Dial, R.B. (1979), ' Amodel and algorithm for multicriteria route-mode choice , ' Transportation Research Part B: Methodological , Vol. 13 No. 4, pp. 311-316.

FAOSTAT(2012), Food and Agriculture Organization of the United Nations, Satistical Yearbook of the Food And Agricultural Organization for the United Nations, available at: www.fao.org/dairyproduction-products/production/en/ (accessed 31 December 2012).

Indian cooperative dairy

793

## JAMR 16,5

794

França, P.M., Sosa, N.M. and Pureza, V. (1999), ' An adaptive tabu search algorithm for the capacitated clustering problem , ' International Transactions in Operational Research , Vol. 6 No. 6, pp. 665-678.

Geetha, S., Poonthalir, G. and Vanathi, P.T. (2009), ' Improved k-means algorithm for capacitated clustering problem , ' INFOCOMP , Vol. 8 No. 4, pp. 52-59.

Goel, R. and Maini, R. (2018), ' Hybrid of ant colony and firefly algorithms (HAFA) for solving vehicle routing problems , ' Journal of Computational Science , Vol. 25 No. 1, pp. 28-37.

Karagul, K., Sahin, Y., Aydemir, E. and Oral, A. (2019), ' A simulated annealing algorithm based solution method for a green vehicle routing problem with fuel consumption , in Paksoy, T., ' Weber, G.W. and Huber, S. (Eds), Lean and Green Supply Chain Management , Springer, Cham, pp. 161-187.

Kechmane, L., Nsiri, B. and Baalal, A. (2018), ' A hybrid particle swarm optimization algorithm for the capacitated location routing problem , ' International Journal of Intelligent Computing and Cybernetics , Vol. 11 No. 1, pp. 106-120.

Khambhampati, S., Calyam, P. and Zhang, X. (2018), ' A tabu search algorithm for a capacitated clustering problem , ' International Journal of Operational Research , Vol. 33 No. 1, pp. 387-412.

Koskosidis, Y.A. and Powell, W.B. (1992), ' Clustering algorithms for consolidation of customer orders into vehicle shipments , ' Transportation Research Part B: Methodological , Vol. 26 No. 5, pp. 365-379.

Lambert, D.M. and Stock, J.R. (1993), Strategic Logistics Management , 4th ed., Irwin: McGraw-Hill, Homewood, IL.

Laporte, G. (1992), ' The vehicle routing problem: an overview of exact and approximate algorithms , ' European Journal of Operational Research , Vol. 59 No. 3, pp. 345-358.

Larsson, T. and Patriksson, M. (1995), ' An augmented Lagrangean dual algorithm for link capacity side constrained traffic assignment problems , ' Transportation Research Part B: Methodological , Vol. 29 No. 6, pp. 433-455.

Li, S.G. (2008), ' Genetic algorithm for solving dynamic simultaneous route and departure time equilibrium problem , ' Transport , Vol. 23 No. 1, pp. 73-77.

Lieb, R.C. (1992), ' The use of third-party logistics services by large American , ' Journal of Business Logistics , Vol. 13 No. 2, pp. 29-42.

Lin, S.W., Vincent, F.Y. and Lu, C.C. (2011), ' A simulated annealing heuristic for the truck and trailer routing problem with time windows , ' Expert Systems with Applications , Vol. 38 No. 12, pp. 15244-15252.

Lummus, R.R., Krumwiede, D.W. and Vokurka, R.J. (2001), ' The relationship of logistics to supply chain management: developing a common industry definition , ' Industrial Management &amp; Data Systems , Vol. 101 No. 8, pp. 426-432.

Mulvey, J.M. and Beck, M.P. (1984), ' Solving capacitated clustering problems , ' European Journal of Operational Research , Vol. 18 No. 3, pp. 339-348.

Nozaki, Y. (2017), ' Future trends of growing demand for milk and dairy products and milk supply in India , Mitsui &amp; Co. Global Strategic Studies Institute Monthly Report. '

Osman, I.H. and Christofides, N. (1994), ' Capacitated clustering problems by hybrid simulated annealing and tabu search , ' International Transactions in Operational Research , Vol. 1 No. 3, pp. 317-336.

Rajendran, K. and Mohanty, S. (2004), ' Dairy co-operatives and milk marketing in India: constraints and opportunities , ' Journal of Food Distribution Research , Vol. 35 No. 2, pp. 34-41.

Rautela, A., Sharma, S.K. and Bhardwaj, P. (2017), ' Vehicle routing approach for an efficient distribution: a case of a state-owned Indian cooperative dairy , ' International Journal of Procurement Management , Vol. 10 No. 6, pp. 776-789.

Seifbarghy, M. and Samadi, Z. (2014), ' A tabu search-based heuristic for a new capacitated cyclic inventory routing problem , ' International Journal of Mathematics in Operational Research , Vol. 6 No. 4, pp. 491-504.

Shieh, H.M. and May, M.D. (2001), ' Solving the capacitated clustering problem with genetic algorithms , ' Journal of the Chinese Institute of Industrial Engineers , Vol. 18 No. 3, pp. 1-12.

Singh, M. (2015), ' A survey on various k-means algorithms for clustering , ' IJCSNS International Journal of Computer Science and Network Security , Vol. 15 No. 6, pp. 60-65.

Szeto, W.Y., Wu, Y. and Ho, S.C. (2011), ' An artificial bee colony algorithm for the capacitated vehicle routing problem , ' European Journal of Operational Research , Vol. 215 No. 1, pp. 126-135.

Tannenbaum, P. (2013), Excursions in Modern Mathematics , 8th ed., Pearson, Boston, MA.

Thangiah, S.R. and Gubbi, A.V. (1993), Effect of Genetic Sectoring on Vehicle Routing Problems with Time Windows , IEEE, Washington, DC, pp. 146-153.

Toklu, N.E., Papapanagiotou, V., Klumpp, M., Gambardella, L.M. and Montemanni, R. (2016), ' Ant colony optimisation for a 2-stage capacitated vehicle routing problem with probabilistic demand increases , ' International Journal of Business Innovation and Research , Vol. 11 No. 1, pp. 5-17.

Toth, P. and Vigo, D. (2014), Vehicle Routing: Problems, Methods, and Applications , 2nd ed., Society for Industrial and Applied Mathematics, Philadelphia, PA.

Wang, J. and Su, X. (2011), An Improved K-Means Clustering Algorithm , IEEE, Xi an. '

Ž alik, K.R. (2008), ' An efficient k -means clustering algorithm , ′ ' Pattern Recognition Letters , Vol. 29 No. 9, pp. 1385-1391.

## Corresponding author

Anubha Rautela can be contacted at: arautela.rs.mec12@itbhu.ac.in

Indian cooperative dairy

795