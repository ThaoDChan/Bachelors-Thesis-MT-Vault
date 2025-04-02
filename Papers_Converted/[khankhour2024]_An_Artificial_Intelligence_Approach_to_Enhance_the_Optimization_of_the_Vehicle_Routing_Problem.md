![Image](image_000000_b0cea7a9a82f327de3755b7c88dbb52add715809081cc31ea8adc4bf2ee55c3b.png)

## An Artificial Intelligence Approach to Enhance the Optimization of the Vehicle Routing Problem

Hala Khankhour ( B ) , Jaafar Abouchabaka , and Najat Rafalia

Research Laboratory in Computer Science, Faculty of Sciences, Ibn Tofail University, Kenitra, Morocco

Hala.khankhour@uit.ac.ma

Abstract. Sustainable development involves an economic plan that prioritizes meeting basic human needs while also taking care of our environment through the use of technology. A key step we can take towards this goal is optimizing our supply chain processes to reduce air pollution and traffic congestion on our planet. This approach benefits all citizens by reducing daily traffic jams. To achieve this, we are focused on solving the vehicle routing problem (VRP) with time windows and synchronization constraints. Our multi-agent system utilizes genetic and metaheuristic algorithms, such as simulated annealing and the nearest neighbour method, to generate efficient routes for three vehicles in response to customer requests. Our objective is to calculate the total distance for each route and assess the probability of stopping for each vehicle. Through parallel processing, our agents collect and analyze data related to VRP problems for customer locations and classify vehicle data based on their model and type. By utilizing these methods, we can achieve sustainable development while also improving the lives of citizens.

Keywords: VRP · Artificial intelligence · Multi-agent system · Genetic algorithms · Machine learning · Parallel processing · Traffic congestion

## 1 Introduction

Thetransportation industry has seen a significant transformation with the advent of information technology, resulting in the development of Intelligent Transport Systems (ITS) [1]. This progress has now given rise to a new era of Cooperative Transport Systems (CITS), which includes advanced features like V2X communication, autonomous driving, and coordination, along with advanced traffic environment perception. However, planning routes for multiple vehicles with diverse service characteristics, such as emergency vehicles, public transit, and private vehicles, continues to be a significant challenge. The Vehicle Routing Problem (VRP) plays a crucial role in efficiently managing traffic [2]. However, traditional solutions based on the Commercial Traveler Problem (TSP) have limitations in terms of computational complexity and local optimization. To tackle these challenges, heuristic approaches, optimization mechanisms inspired by nature, and artificial intelligence techniques have been explored. This study aims to present an

innovative methodology to overcome the challenges of traffic and resource management in these constantly evolving transport systems. By examining the current state of knowledge, designing a multi-agent system incorporating relevant algorithms, and exploring specific tools, this approach hopes to offer increased flexibility and adaptability through an elaborate multi-agent architecture. The research opens up promising prospects for the effective resolution of complex problems within modern transport systems [3].

## 2 State of the Art

TheVehicle Routing Problem (VRP) is not a new concept as it dates back to the middle of the 20th century. Today, logistics providers can benefit from route optimization software, which provides considerable support in this area. Thanks to many high-speed algorithms, the VRP can now be optimized in just a few minutes.

Exact Methods: This session will discuss the most effective methods for solving VRPtype problems. In combinatorial optimization, the first step is to explore the possibility of solving the problem by modelling it in linear form and then using the simplex method (Nash, 2000) [4]. Fisher and Jaikumar (1981) [5] formulated a widely recognized linear model to solve VRP. Therefore, it is necessary to combine this algorithm with another resolution method in order to obtain a workable solution for the given problem [6].

Methods by Reinforcement: In reinforcement methods, the objective is to improve the behaviour (called policy) of an agent within an environment. Define S as the set of possible agent states and A as the set of possible actions [7].

The machine learning techniques we cited play an important role in solving the Vehicle Routing Problem (VRP) because of their ability to provide more efficient and adaptive solutions [8].

By combining traditional optimization techniques with machine learning methods, solutions to the VRP problem become more flexible, adaptable, and able to adjust to the various constraints and dynamics encountered in the real world [9].

## 3 Methodology

This study examines the increasing use of multi-agent systems and machine learning to solve the vehicle routing problem (VRP) [10]. The application of machine learning aims to improve routing strategies by leveraging historical data to optimize routes, predict future customer demand, and adapt to dynamic changes. Starting with the use case diagrams and sequence diagrams, this study carefully selects algorithms such as simulated annealing to explore efficient solutions and linear regression to accurately predict customer demands. The use of the multi-agent system models a dynamic environment for effective collaboration between actors, offering a holistic approach to solving the complex challenges of VRP. This integrated approach opens up promising prospects for optimizing transport routing systems [11] (Fig. 1).

Fig. 1. Sequence diagram of multi-agent system

![Image](image_000001_e57433ce477c2447e4b195a1c20377e10872a48f96aa97d4e93f20879247ad19.png)

## 4 Proposed Approach

LinearRegression: Thechoiceoflinear regression to predict demand levels in different geographic areas is based on several advantages inherent in this ML method. Moreover, linear regression is often used as a starting point to model more complex relationships. It can be extended to include additional variables, data transformations, or advanced techniques to improve prediction accuracy [12].

Multi-agent System (MAS): The choice of a MAG to address VRP stems from its ability to model complex environments, facilitate collaboration between autonomous entities, and provide flexible and adaptive solutions to logistical problems [13].

UsedData(VehiclesData): Wewillstart with the collection of vehicle data, the purpose of this is to help people who want to take a vehicle for a tourist trip for example. Therefore, we take this data from the Kaggle website (data source: https://www.kaggle.com/ datasets/adityamahimkar/car-driving-distance-range-dataset). Therefore, we integrate vehicle data into our system for these points (Fig. 2).

Fig. 2. Vehicles datasets

![Image](image_000002_c23c449ef20f1fd8b74776096d86494b6ffd4451393a1dd29954d2e3b1de1326.png)

After cleaning up vehicle data and performing necessary type conversions, we displayed the following graphs:

1A pie chart (pie chart) showing the distribution of vehicle types in the data set, highlighting the share of each vehicle type with the whole.

- 2Abar graph representing the distribution of vehicle models, illustrating the frequency of each model in the data set.

Each graph aims to provide a clear and informative visualization of the important features and trends present in the cleaned vehicle data. These graphical representations can help to better understand the different characteristics and relationships within the vehicle data set after the types of cleaning and conversion steps (Fig. 3).

Fig. 3. Histogram car distribution

![Image](image_000003_7d9eafc2349590be861a352dfa709924f783e707699bcc49b20080b53dab4e56.png)

After converting our DataFrame to a Python dictionary, we transformed it into a JSON format before sending it to agentOne using sockets to perform the required processing. This conversion to a Python dictionary was undertaken to facilitate data manipulation in a format suitable for communication with the agentOne. Then, by transforming this dictionary into a JSON format, we used this standard data structure to transmit information in an efficient and organized way to the agentOne. This transmission of data as a JSON file allows the agentOne to process, analyse or perform any specific operation necessary on this data, in the context of our application or system (Fig. 4).

Fig. 4. VRP datasets in dataframe

![Image](image_000004_b056864cc2ffcb2e8ffcf8c61d9258e333405ed054759540c318fbe3c1c7d535.png)

The DataFrame provided presents various data on several cities in Morocco, including information such as geographical coordinates (latitude and longitude), population, demand levels, time, country, administrative region and other details. After analysing the data and performing a data cleansing process to resolve issues such as incorrect or missing values, we turned this data into JSON format. Then, we sent this JSON file to the responsible agent for route generation for each vehicle. This approach was intended to provide clean and structured data, ready to be used by the responsible officer to create

routes for vehicles, using the JSON format for better readability and easier integration into systems or applications required for route management.

Simulated Annealed Algorithm: The choice of the simulated annealing algorithm for solving the Vehicle Routing Problem (VRP) is based on the intrinsic properties that make it suitable for this type of complex problem. The VRP, being a challenging combinatorial optimization, requires effective solutions to.

Fig. 5. The flow diagram of the SA algorithm

![Image](image_000005_e58dbf269b64a329ea84b01185044efda63cbaa2f4f558f4dffde4f338aff72e.png)

The simulated annealing process begins with an initial solution, and then iteratively improves the current solution by randomly disrupting it and accepting the disturbance with some probability. The probability of accepting a worse solution is initially high and gradually decreases as the number of iterations increases. The SA algorithm is quite simple, and it can be implemented directly as described in Fig. 5.

## 5 Implementation and Results

Using the SA algorithm (Simulated Annealing), we were able to determine separate optimal routes for each vehicle, each one being unique compared to the others. The time required for this resolution was 32 ms. To assess the accuracy of these routes, we calculated the difference between the distance obtained on the optimal route and the reference route (benchmark), which allowed us to establish the accuracy of the results (Fig. 6).

```
Route for Vehicle 1: (55, 46, 1, 56, 90, 71, 84, 52, 10, 54, 16, 79, 27, 96, 20, 25, 100, 66, 64, 60, 69, 77, 41, 82, 95, 32, 44, 34, 6, 31,
99, 30, 59, 11, 65, 84, 28, 65, 12, 17, 101, 35, 96, 81, 42, 62, 69, 73, 21, 30, 40, 5, 70, 74, 33, 37, 81, 97, 15, 0, 50, 4, 75, 34, 9, 63,
7, 3, 83, 26, 46, 34, 22, 43, 0, 10, 19, 23, 97, 51, 45, 82, 2, 94, 24, 61, 37, 39, 09, 67, 99, 14, 60, 8, 72, 46, 74, 47, 13, 29, 53, 70)
Total Distance for Vehicle 1: 10724.037470709013
Accuracy for &A : 95.33792469123597
Route for Vehicle 2: (14, 57, 61, 69, 26, 24, 17, 32, 27, 31, 6, 34, 61, 90, 20, 90, 91, 37, 33, 3, 7, 34, 63, 64, 8, 60, 72, 53, 70, 28,
93, 20, 44, 35, 101, 99, 67, 94, 19, 85, 83, 12, 80, 64, 77, 25, 100, 41, 22, 66, 5, 76, 10, 32, 84, 71, 90, 75, 4, 50, 87, 92, 40, 23, 2,
82, 42, 42, 10, 46, 45, 78, 63, 9, 56, 97, 51, 1, 15, 0, 11, 59, 39, 69, 65, 54, 16, 79, 96, 45, 73, 21, 38, 95, 13, 47, 50, 74, 48, 55, 66,
29)
Total Distance for Vehicle 2: 10622.6445010038762
Accuracy for &A : 95.230071497458
Route for Vehicle 3: (78, 36, 1, 51, 92, 45, 97, 0, 5, 33, 83, 91, 89, 9, 28, 35, 101, 25, 100, 13, 29, 40, 61, 56, 9, 63, 84, 52, 10, 75,
16, 31, 81, 27, 93, 80, 65, 54, 4, 94, 82, 36, 21, 73, 69, 95, 66, 60, 99, 14, 57, 11, 59, 37, 15, 24, 90, 34, 6, 79, 36, 70, 53, 17, 23,
40, 69, 64, 39, 67, 44, 20, 96, 42, 62, 32, 72, 55, 64, 74, 47, 41, 54, 77, 43, 85, 19, 22, 0, 12, 87, 50, 76, 71, 7, 3, 90, 60, 26, 2, 46,
18)
Total Distance for Vehicle 3: 11566.370021470592
Accuracy for &A : 95.6660400446105
/                                                                                                                                                                                                        
                                                                                                                                                                                                        This. 6.  SA results
```

## Fig. 6. SA results

Using the Directions API of Google Maps is a service provided by Google that allows developers to access and integrate routing, distance matrix calculation and navigation information into their applications. It provides detailed instructions for moving between different places using different modes of transport such as driving, walking, cycling or public transport. We were able to obtain information on the levels of traffic congestion in each city by specifying the mode of transport driven.

We wanted to find the cars in a list of vehicles that requires the fewest refuelling stops to cover a distance calculated by our SA algorithm by dividing the final distance by the maximum distance of each vehicle.

The prediction list is a series of predicted or estimated values for our linear regression model (or other prediction model). On the other hand, the "mean squared error" (MSE) metric we obtained, which is about 3.1571730924173083, is a measure of the mean quadratic error between the predictions of our model and the corresponding real (or target) values. Interpreting these results involves considering both the predictions obtained and the MSE:

- GLYPH&lt;129&gt; Predictions: The values provided in the list represent what your model predicted for a specific data set. Each value in the list is the model prediction for a particular example of data. For example, if you make demand predictions for customers or localities, these values would represent the demand levels estimated by your model.
- GLYPH&lt;129&gt; Mean Squared Error (MSE): The MSE is a measure that evaluates the quality of the model's predictions by calculating the average of the squares of the differences between the predicted values and the actual values. The lower the MSE, the better the performance of the model. A MSE of 3.1571730924173083 means that, on average, the squares of the differences between the predictions and the actual values are about 3.16.

In this context, a smaller MSE indicates that the model tends to have better performance, that is, it predicts values closer to real values. However, the exact interpretation depends on the specific field of application and the meaning of the predicted values (in your case, demand levels) for your particular problem (Table 1).

Table 1. Results table.

| Approche           | Execution time   | AVG Distance   | Refuel stops                      | car         |   Accuracy |
|--------------------|------------------|----------------|-----------------------------------|-------------|------------|
| Plus proche voisin | 4 ms             | 25.9km         | 3                                 | Lexus       |      94.83 |
| Recuit simule      | 31 ms            | 11216.33 km    | 5.87925815326671 4.34056112948754 | Lexus Dacia |      95.5  |

These results are promising, especially considering the work constraints and sometimes inaccurate estimates we have used. They reinforce the quality of our SA algorithm and validate our methods and our system designs.

## 5.1 Comparison and Discussion

By comparing our results with other recent paper (Sultana 2022) [14] and projects by researchers using different approaches, we observe the effectiveness of our algorithm. It stands out for its ability to generate more optimal routes, thus reducing travel time to a minimum (Fig. 7).

Fig. 7. Comparison of tour length and solving time of different methods

| Mcthod          | Len Tour   | Gap    | Time   | Len Tour   | Opl. Gap   | Time   | Tour Len   |         | Time    |
|-----------------|------------|--------|--------|------------|------------|--------|------------|---------|---------|
|                 |            |        |        | 5.69       |            |        |            |         |         |
|                 |            |        |        |            |            |        |            | 2199    | 230     |
| POMO            |            |        |        | 70         | 0.2190     |        |            | 0.469   |         |
|                 |            |        |        | 5.69       |            |        |            |         |         |
|                 | 3,84       | 0.2790 |        | 78         |            |        | 08         |         |         |
|                 |            |        |        |            |            |        | 92         | 939     |         |
| MDAM (rcedy)    | 84         |        |        |            |            |        | 93         |         |         |
| MDAM            |            |        | mn     |            | 0.049      |        |            |         |         |
| NLNS            |            |        |        |            |            |        |            |         |         |
|                 |            |        |        | 5,70       |            | Sh     |            |         | 2h      |
| LCP             | 383        |        |        | 5.70       | 0.109      | 25h    |            |         | 25h     |
|                 | VRP2O      | VRP2O  | VRP2O  | CVRPSO     | CVRPSO     | CVRPSO | CVRPTOO    | CVRPTOO | CVRPTOO |
| Method          | oui Len    | Gap    |        |            |            |        | ou Len     | Gap-    |         |
| LKH3            |            | 00%6   |        |            |            |        | 15.68      |         |         |
| AM (greedy)     | 640        |        |        | 10.98      |            |        | 1676       | 6899    |         |
|                 | 27         |        |        | 10.62      | 2.389      |        | 16.24      |         |         |
|                 |            |        |        | 10.49      |            |        | 15.83      |         |         |
|                 |            | 02190  |        | 10.2       |            | 26     | 15.73      |         |         |
| MRAM (greedy)   | 6.39       |        |        | 10.95      |            |        |            | 6.4390  |         |
| MRAM (sumpling) | 25         |        |        |            |            |        | 16.20      |         | 53m     |
|                 |            |        |        | 10.74      |            |        | 16,40      |         |         |
| MDAM            |            |        |        |            | 1890       |        | 1603       |         |         |
| NcuRewriler     |            | 0.7490 | 18m    | 10.51      |            | 22m    |            | 2719    |         |
| NLNS            | 6.19       | 1.3190 | 7m     | 10.54      |            | 24m    | 15.99      | 1.9870  |         |
|                 | 612        | 0-3990 | 2h     | 10.45      |            |        | 16,03      | 2,4790  |         |
| LCP             | 6,16       |        |        | 10.54      |            |        |            | 2,4390  |         |

## 6 Conclusion

This study combines technologies such as JADE for agent management, Python with machine learning models, and the simulated Recuit algorithm to optimize logistics planning. Its results demonstrate the system's ability to generate more efficient routes by

minimizing vehicle refuelling stops, providing a more efficient solution for route planning. Future prospects include refining the simulated Recuit algorithm, integrating realtime traffic data, adapting the solution to other areas such as fleet management or urban logistics, and consideration of fuel consumption for a more sustainable approach. This research marks a significant advance in intelligent systems for efficient route planning, offering promising potential for continuous improvements and extensive application in various logistics and transportation sectors.

## References

- 1. Dantzig, G.B., Ramser, J.H.: The truck dispatching problem. Manage. Sci. 6 (1), 80-91 (1959)
- 2. Andersen, S.M., Lydersen, C., Grahl-Nielsen, O., Kovacs, K.M.: Autumn diet of harbour seals (Phoca vitulina) at Prins Karls Forland, Svalbard, assessed via scat and fatty-acid analyses. Can. J. Zool. 82 (8), 1230-1245 (2004)
- 3. Khankhour, H., Abdoun, O., Abouchabaka, J.: Parallel genetic approach for routing optimization in large ad hoc networks. Int. J. Electr. Comput. Eng. (IJECE) 12 (1), 748-755 (2022)
- 4. Nash, C.: Performativity in practice: some recent work in cultural geography. Prog. Hum. Geogr. 24 (4), 653-664 (2000)
- 5. Fisher, M.L., Jaikumar, R.: A generalized assignment heuristic for vehicle routing. Networks 11 (2), 109-124 (1981)
- 6. Laporte, G., Nobert, Y. : A branch and bound algorithm for the capacitated vehicle routing problem. Oper. Res. Spektrum, 5 , 77-85 (1983)
- 7. Fisher, M.L.: Optimal solution of vehicle routing problems using minimum k-trees. Oper. Res. 42 (4), 626-642 (1994)
- 8. Rummery, G.A., Niranjan, M.: On-line Q-learning using connectionist systems, vol. 37, p. 14. University of Cambridge, Department of Engineering, Cambridge (1994)
- 9. Gosavi, A.: Reinforcement learning: a tutorial survey and recent advances. INFORMS J. Comput. 21 (2), 178-192 (2009)
- 10. Uchoa, E.: New benchmark instances for the capacitated vehicle routing problem. Eur. J. Oper. Res. 257 (3), 845-858 (2017)
- 11. Pessoa-Amorim, G., et al.: Admission of patients with STEMI since the outbreak of the COVID-19 pandemic: a survey by the European Society of Cardiology. Eur. Heart J. Qual. Care Clin. Outcomes 6 (3), 210-216 (2020)
- 12. Clarke, G., Wright, J.W.: Scheduling of vehicles from a central depot to a number of delivery points. Oper. Res. 12 (4), 568-581 (1964)
- 13. Yellow, P.C.: A computational modification to the savings method of vehicle scheduling. Oper. Res. Q. (1970-1977) 21 (2), 281-283 (1970)
- 14. P Sultana, N., Chan, J., Sarwar, T., Qin, A.K. :Learning to optimise general TSP instances. Int. J. Mach. Learn. Cybern. 13 (8), 2213-2228 (2022). Paessens, H.: The savings algorithm for the vehicle routing problem. Eur. J. Oper. Res. 34 (3), 336-344 (1988)