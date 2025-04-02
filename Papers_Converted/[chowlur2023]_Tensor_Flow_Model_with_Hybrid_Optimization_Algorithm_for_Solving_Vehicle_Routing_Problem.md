## Tensor Flow Model with Hybrid Optimization Algorithm for Solving Vehicle Routing Problem

![Image](image_000000_64d758547db935da7e7245f33bee8eb5ac9142d2b26479bf2838f9f8d4cfdff8.png)

Jai Keerthy Chowlur Revanna and Nushwan Yousif B. Al-Nakash

Abstract Vehicle routing and path management system improves the best key point of selecting the path of the vehicle to move. The applications that are used for delivering the products utilize Google data to organize the vehicle movement and its coordinate positions. The traffic level indicator and the speed of vehicle movement validate the Vehicle Routing Problem (VRP)- route. Since there is another important parameter that needs to consider for the delivery process. In that, the application needs to validate the amount of traveling time and the length through which the vehicle can travel to deliver the products. This requires a better prediction model to estimate the multiple parameters of vehicle routing problems. In the proposed study, a Tensor Flow-based routing path prediction approach was chosen to train and predict the best route using the attribute weight matrix. From the parameters of the distance between the coordinates with the amount of traffic range and other related attributes, the Tensor Flow model forms the rule to train the machine for predicting the route for vehicle movement. This model updates the learning model based on the change in parameter value and its range. The experimental result compares the suggested work to the current model of optimum VRP prediction approaches.

Keywords Vehicle routing problem (VRP) · Ant colony optimization (ACO) · Tensor flow model (TF) · Optimal routing path and multi-objective learning

## 1 Introduction

The vehicle path identification and prediction process in area of coordinate position and grouping represents a dynamic update of data from the database to analyze the state of routing path prediction in coordinates and mining system for VRP dataset.

J. K. Chowlur Revanna ( B ) · N. Y. B. Al-Nakash

Information Systems Engineering and Management, Harrisburg University of Science and

Technology,

326 Market Street, Harrisburg, PA 17101, USA

e-mail: JChowlur@my.harrisburgu.edu

N. Y. B. Al-Nakash

e-mail: nal-nakash@harrisburgu.edu

'The Author(s), under exclusive license to Springer Nature Singapore Pte Ltd. 2023

V. Suma et al. (eds.),

Inventive Systems and Control

, Lecture Notes in Networks

This will be updated for a period interval that is processed in frequent order. There are several methods in classification and analyze the database contents like neural techniques and other machine learning technics. In that, sequence pattern method with the several neural network were most used to classify and match the relevant features from database [1]. Since, for the huge amount of node characteristics in data, it struggles in predicting class with proper attributes of feature vector. This searching process predicts the relevant feature that compares the input query with the database in high-speed analysis of large path data. In the recent research work, the matching prediction is complicated due to more irrelevant features present in the database [2]. To improve the performance of feature identification in the searching process, machine learning technique helps to predict the best matching between the query data and feature sets in database. There are several type of machine learning techniques like support vector machine (SVM), relevant vector machine (SVM), and other methods. Since, in the recent days, the neural network and deep learning technique takes place a major role of data analysis and match prediction among the bulk amount of raw data.

For this condition, it directs into the routing-based data monitoring and controlling system by remote locations [3]. This is to provide data management and optimized travel to database of hospital management. In the data travel process, it needs to travel through the worldwide connectivity [4]. In that, there are lot of data patterns that can capture and identify the data which can change the database information about the user path parameters and about medicinal value that are ordered by people. This needs to be prevented by highly optimized data travel and storage systems in the cloud environment [5]. For data management and predicting there are lot of prediction and re-routing techniques available to optimize the data that can protect data who doesn't have the encryption technique. Still, there are some limitations that are in the data storage and even to retrieve the database.

To solve this management and predicting problem in routing environment, this work proposes a novel routing management and predicting system integrated with Tensor Flow (TF) model. In this model, the TF manage the path that vehicle can travel by identifying the properties/features of data from routing path traffic level to predict the normal/range of traffic pattern by using the Ant Colony Optimization with Genetic Algorithm (ACO-GA). If this was identified as normal, then the normal flow of data travel will take care. If this was identified as pattern type of the traffic range, then this was reported to controlling system or to the routing systems to predict the data pattern at the stage of initial flow. Then this was also forward to the training model of TF to update the characteristics of the data pattern and arrange the features of it. This will enhance the feature learning of TF and the characteristics and parameters of data pattern up to time instant. This can be achieved by the Spatial Pattern Super Learning (SPSL) method. This analyses the parameters by probabilistic distributional features to update the learning model. This can identify the multiple combination of parameters to group it and form as the cluster for better prediction process.

The primary objectives of this paper are.

- 1. By carefully choosing the best route for data forecasting, to improve the prediction performance of routing.
- 2. To increase the speed of vehicle movement and the reliability data processing on vehicle, optimization method using ACO-GA was designed.
- 3. To estimate the location of users based on the routing properties for improved data analysis and forecasting.
- 4. Tensor Flow is introduced into the process of generating parameters to facilitate the analysis of the multi-objective parameters of the optimization model.

The rest of the other sub-sections are organized and elaborated as mentioned below. Section 2 gave a study and review of the current VRP model, along with its benefits and drawbacks, based on the proposed enhancements. Section 3 explains about the proposed optimal routing path selection system based on the ACO-GA with Tensor Flow model. Section 4 evaluates the effectiveness of suggested VRP model with the prediction rate, and it contrasts the estimated findings with the conventional methods to demonstrate the superiority of this new implementation. In Sect. 5, which concludes the paper and summarizes the future recommendations on this work.

## 2 Related Works

Here a critical review is performed on different optimization models and the routing algorithms that are to optimize the vehicle routing problems. In that, the merits and the demerits of each model were explained and validated.

The benefit of this plan was that it guaranteed the vehicle's reliability and routing. An adaptive technique for effectively boosting the accuracy of safety message forecasting on the vehicle was introduced in [6]. Here, the error recovery probability rate of data forecasting was calculated using the Adaptive Byte Hybrid Automatic Repeat Request (AB-HARQ) approach [7]. A dynamic virtual bat algorithm [7]. In the development of a method for supplying routing to VRP with a reduced latency frequency. The objective here is to combine the benefits of Simulated Annealing (SA) and Particle Swarm Optimization (PSO) to improve the performance of optimization during path selection. A new data forwarding strategy was suggested in [8] for raising the VRP's overall performance rate. Here, traffic information was used to guarantee accurate vehicle forecasts. A sensor clustering strategy was developed in [9] to address the vehicle's concealed terminal problem, reliability, and resource scarcity problems. The applications of privacy preservation, target tracking, and misbehavior detection may be appropriate for this clustering technique.

Asummaryofdata dissemination techniques and the significance of QoS for VRP are given in [10]. The difficulties with connection stability and energy consumption have been examined in this paper, along with the best methods for enhancing VRP performance. A unique algorithm for decreasing the broad-cast storms on VRP was devised in [11]. This work's strength was that it established the emergency message

forecasting in the simplest possible way. A new V2VR approach for ensuring VRP routing was introduced in [12]. In this case, a routing choice system based on the Manhattan mobility model was used to shorten the longer predicted distance. In this paper [13], a data distribution strategy for enhancing the efficiency of data delivery via VRP is suggested. To distribute the data among the users, a probabilistic forwarding mechanism was used [14]. This work's benefits were less message latency.

In [15] the author a Batch Verification Certificateless Ring Signature (BV-CLES) technique for making sure reliable navigation and dependable information forecasting on VRP. The primary objective of this research was to effectively reduce the computational overhead and delay of the vehicle for VRP connection. In addition, the transportation of the vehicle was accelerated by implementing a signature verification method. The most important contribution of this research was that it offered a faster routing system for automobiles and lowered computing expenses. A decentralized parameters control system was suggested in [15] to enhance VRP's information forecasting. This work used a lightweight mutually relevant prediction approach to raise the vehicle's routing level. The following is a list of the main factors that this work took into account: licensed parameter storage, licensed parameter management, mutual relevant prediction, updated parameters, and revocation. Additionally, distinct categories of missing data, such as those caused by collusion, DoS, and resisting internal missing data, were identified in this work based on the routing analysis. The need to lower computational overhead, computational cost, and storage overhead still exists.

For improving the routing of VRP [16] used a hybrid conditional privacy preservation approach. This work's main goal was to use a privacy-relevant prediction algorithm to address the identity revocation problem and lessen computational overhead. In this instance, this anonymous identity was considered the local short-term identifier accountable for signing the safety-related communication. In addition, a bilinear pairing based on the cyclic groups of the bilinear map was performed [17]. This technique's design objectives included efficiency, pertinent prediction, confidentiality, revocation, and privacy. This mechanism's benefits included improved speed and robustness with effective message-relevant prediction. Reliable communication over VRP can be established by using an improved routing method that uses the Multipoint Relay (MPR) scheme, as described in [17]. The goal of utilizing the OLSR algorithm in this case was to enhance the MPR selection strategy to prevent data reforecasting. Additionally, several metrics including delivery ratio, throughput, and vehicle delay were estimated.

A self-checking procedure was used in [18] to increase the routing VRP data transfer. This system was divided into four stages: registration, relevant prediction, missing data detection, and self-checking. By assuring randomization, this approach primarily aims to safeguard the vehicle from missing data users. For the purpose of ensuring the routing and confidentiality of VRP, [19] adopted a Comprehensive Identity Relevant Prediction Scheme (CIAS). In this study, asymmetric processing and pertinent prediction procedures were used to build trust amongst the entities [19]. Finding a damaging lacking data against the automobile and offering appropriate, pertinent prediction solutions were the main focuses of this work. Blockchain and

traffic flow computation procedures were used to build an effective routing architecture for VRP in [20]. This architecture includes the service the road traffic computing layer, service layer and the perception layer [21]. In which the perception layer was used to enhance the routing of the vehicle during data transfer. The drawback of this architecture was that it necessitated increasing the vehicle's overall routing and data forecasting pace.

Here prediction models are used based on the Confusion matrix. The primary classifiers are F1 score, precision, accuracy, and recall which are used as prediction model in the paper [22]. The confusion matrix is necessary to determine the classification accuracy of the machine learning algorithm while categorizing input into their respective labels. Below is the definition of each terms:

Accuracy : It is the ratio of occurrences of correctly categorized data to the total number of instances of data.

Precision: A strong classifier must preferably have a precision value of 1 (high). When the numerator and denominator are identical does precision equal 1.

Recall : Recall must preferably equal 1 (high) for a classifier to be effective. When the numerator and denominator are equal does recall equal 1.

F1 score : The F1 score is only good if both accuracy and recall are indeed high. F1 score is the arithmetic average of recall and precision and is a more accurate measurement than precision.

## 3 Methodology

This section discussed optimal vehicle routing models with the optimization functions and the other related parameter-based categorizations. In this, the overall process was segmented as the following stages:

- · Preprocessing
- · Tensor Flow Prediction
- · Optimization

Figure 1 shows the architecture of ACO used with Tensor Flow which is the proposed work in this paper. In this, the preprocessing covers the initial cluster of data to organize the coordinate position of users and the initial weight estimation [23]. Then from that, the optimization functions retrieve the best routing path and the minimum distance-based path selection. Since, the optimization models are tuned to select the best routing path for vehicles based on their objective function design and the weight calculation of each path attributes. The capacity and demand are decided based on the customer needs in the area. The capacity is estimated based on Solomon benchmark data set for each case [24]. For example, in Case C101 the capacity is estimated based on the number of customers. In this paper the capacity is estimated for 100 customers and the demand is served according to the customer's

needs. Similarly, for R101, RC101, R103, and all the cases shown below in the figures are defined. The demand is the number of locations requested to be delivered. The demand is the number of locations requested to be delivered. Once the capacity estimation is performed, demand is served.

The following sub-sections explain the detail structure of routing path selection. The parameters are defined by time interval, no. of iterations, no. of customers. All the above parameters are based by counting the number of vehicles and locations. The parameters are initialized as shown by algorithm 1 below from p to 1, that starts the loop from 1 and by repeating the iterations it selects the best path. As shown in Table 1 different parameters were also compared with ACO-GA and it is shown

Fig. 1 Proposed work

![Image](image_000001_a787f6ff71a5161d74d08bd14cb36bbb66173db42f1454c713028660d1937e5a.png)

Table 1 Results of different parameters for proposed work

| Parameters          | Value           |
|---------------------|-----------------|
| ACC macro           | 0.99326         |
| Conditional entropy | 0.20469         |
| Kappa               | 0.96153         |
| Hamming loss        | 0.02359         |
| Overall ACC         | 0.97641         |
| Overall MCC         | 0.96171         |
| Overall RACC        | 0.38682         |
| PPV (macro-micro)   | 0.93204-0.97641 |
| Reference entropy   | 1.69515         |
| TNR (macro-micro)   | 0.99584-0.99607 |
| Zero-one loss       | 4449            |
| Response entropy    | 1.73724         |
| Standard error      | 0.00035         |
| TPR (macro-micro)   | 0.9769-0.97641  |

by the Tensor Flow output in Table 2 that ACO-GA has increased the performance compared to other methods. Regarding crossover and mutation for the GA algorithm, crossover is the primary operation, mutation is the secondary. As mentioned above crossover is primary which is 90% and mutation is 10%. Mutation alters the value of parameters here just like the genes and crossover is a special process that is employed to alter the sequencing of chromosomes through one generation to the next.

Once the parameters are fed, it combines with the ACO parameters, and two different genes are crossed. Here genes are nothing but destination routes. In this way the best parameter is mutated and fed into the channel till it yields the best route after multiple iterations. Lastly, the ACO-GA is integrated with google maps API. Once the ACO-GA results are provided, the results are calculated by Tensor Flow

Table 2 Detection rate comparison of proposed vs. state of art methods

| Methods        |   F1 score (%) |   Accuracy (%) |   Recall (%) |   Precision (%) |
|----------------|----------------|----------------|--------------|-----------------|
| PSO-SVM        |           19.3 |           48.8 |         14.8 |            27.9 |
| ACO-SVM        |           23.1 |           72.9 |         15.7 |            43.3 |
| GA-SVM         |           28.6 |           51.9 |         19.9 |            50.5 |
| ACO-GA         |           73.6 |           67.7 |         75.2 |            71.3 |
| Neural Network |           76.8 |           74.7 |         76.3 |            77.3 |
| Proposed       |           83.6 |           81.4 |         84.2 |            84.3 |
| ANN            |           74.2 |           71.9 |         75.4 |            73   |
| PNN            |           76.8 |           74.7 |         76.3 |            77.3 |
| Proposed       |           83.6 |           81.4 |         84.2 |            84.3 |

model and the best route is decided, it verifies the current route with google maps and overwrites the best path on google maps which then updates the correct path.

## 3.1 Preprocessing

Data preprocessing or filtering is one of the most essential stages in data processing application systems, because the performance of classification system is highly dependent on the quality enhanced data. Filtering techniques are mainly used to remove noise/artifacts, improve contrast, improve quality, and smooth data. To this end, attribute collaboration and filtering techniques were used to remove noisy content present in the original data. This is one of the filtering approaches widely used in many data processing systems. The main reason for using this technique is to efficiently deblur the data with a better smoothing effect. Computes a filter function based on averaging neighboring pixels to remove noisy pixels. The primary reason for preprocessing the dataset is to filter the non-essential data. Here missing values which are the vehicle stops were inserted into the dataset values. Without merging values, it could not be merged with google maps and performance cannot be measured without this. By merging the data with google maps, the data integration worked here. Data preprocessing also helped in selecting the best attributes from the Solomon benchmark dataset to improve performance.

## 3.2 Optimization

The optimization model of this paper is to focus on the hybrid model of ACO and GA optimization algorithm that are fused to perform the routing path selection for the vehicle database. In this the optimization helps in analyzing the cost value with the hybrid model of objective function to estimate the convergence of cost value for each iteration count. In this, the ACO selects the possible paths for the vehicle to travel in the way. The ACO-GA, when hybridized, provides the best path by performing the max number of iterations [25]. When different paths are analyzed repeatedly, the algorithm chooses the shortest path with the least constraints and is chosen as the best path. Then, the GA estimates the acceptable level of the similarity and based on the cost value, the best path among the listed path was selected and justified as the best selection of routing path. Algorithm 1 explains the steps involved in hybrid ACO-GA optimization algorithm.

## 3.3 Tensor Flow Model

Following the extraction of the route patterns, the TF classification technique-which involves the processes of data training and testing-is used to precisely identify the occluded item. The TF is developed based on the machine learning model of trained feature set, where the classification has been done by splitting the features into blocks. The main intention of this technique is processing the paths based on the separate blocks of featured paths, which helps to increase both the prediction accuracy and efficiency of classification. In this architecture, it has three layers mainly input, output, and hidden layers where the input patterns are gained by using the input layer [26]. After this hidden layer starts processing the patterns of path. Algorithm 1 explains the steps involved in Tensor Flow prediction algorithm. In the tensor flow model, the matrix used is F1 score, precision, accuracy, and recall.

## 3.4 Algorithm [3, 7, 23]

Input: Data { Ni } , , flow link cost (FLC) ci j

Output: Efficient routing selection process and security technique

For iteration = 1 to p

//Looping for 'p' number of data links in a database

Invoke γ be the data link's load value, which may be expressed

$$\gamma = \{ N, L \}$$

// where 'N' defines the number of database records and 'L' defines the distance between each record.

For i = 1 to n

//'n' is data count

$$\text{For $j$ } = \, 1 \, \text{ to } l$$

// ' l ' is no. of connections. Create network traffic { f k i , j } for 'k' no. of attempts

$$\text{Update } \gamma _ { ( i, j ) } ^ { k } = \gamma _ { ( i, j ) } ( f _ { ( i, j ) } ^ { k } ), \forall ( i, j ) \in L$$

Calculate route selection probability { y k i , j } in the database design as

$$\left \{ y _ { i, j } ^ { k } \right \} = \sum _ { s, d } \sum _ { t } h _ { s, d } \times P ( r | C _ { n } ) \left ( v _ { s, d } \left ( f _ { i, j } ^ { k } \right ) \right ) \times a _ { i, j } ^ { r }$$

where (s, d) represent the pair between source and destination.

s d v ,

-Vulnerability weight of Docker path a r i , j -is the no. of transmissions to data in (i, j) as shown in the architecture for the territory of 'r'.

Update flow pattern,

$$f _ { i, j } ^ { k + 1 } = f _ { i, j } ^ { k } + \alpha _ { n } \times \left ( y _ { i, j } ^ { k } - f _ { i, j } ^ { k } \right )$$

Verify closure for every k+1 value

$$\text{Calculate } G ( y ) = \max ( y _ { ( i, j ) } ^ { k } ).$$

// Find maximum potential point for determining selected duration

$$\text{Calculate } L ( j ) = \frac { 1 } { n } \sum _ { ( x = 1 ) } ^ { n } \left \| N ( f _ { ( i, j ) } ^ { k } ) - G ( y ) \right \|$$

//Find the distance vector between each data.

$$\text{If} \, \lambda < L, \, \text{then}$$

// λ defines the security strength of data Data transmission.

$$\text{Calculate } \Delta _ { ( j ) } = \Delta _ { j - 1 } + \mu \times \partial L / \partial W _ { i } ^ { l }.$$

// From the data index, find the best data with the nearest distance property.

$$\text{Calculate } c _ { i, j } = W _ { j } ^ { l } + \Delta _ { ( j ) }.$$

// Adjust the vulnerability cost value

Continue.

Else

Affirm the path

Use data parameters to evaluate risk parameters

Continue loop.

End If

End For ' ' j

End For ' ' i

From the updated table, pick the best option. ' y k i , j '

Update database weight and design architecture.

End For.

## 4 Results and Discussion

Here in this section, simulation results discusses and the comparative analysis of ACO-GA with Tensor Flow model and other optimization algorithm of the vehicle routing system. The overall test analysis was experimented with the standard dataset of Solomon's benchmark dataset which is publicly available and referred from the existing work of the routing system [26], there are 100 records taken for Solomon benchmark. The overall work was implemented in the PYTHON 3.8 tool in terms of '.py' scripts and related libraries. In this test analysis, the results can be compared with the other existing optimization methods that are referred to in [27].

In that, the dataset contains the several position and coordinate information about the user that are in the coverage area. In that, the dataset is divided into three different categories of 'R', 'C', and the combination of both as 'RC'.

Figure 2 shows the simulation result for the sample of data combination of C101, R105, for all the 100 number of users available in the dataset. In these graph result, the X and Y -axis represent the ' X ' and ' Y ' coordinate position of users, respectively. Similarly, the Fig. 3 shows the simulation result for RC105.

This simulation step concentrates on the output graphical result of ACO-GA optimization for VRP. In that graph, the title displays the number of vehicles that are optimized and the amount of area that covered by those vehicles are mentioned for the 10th iteration of simulation result.

The maximum number of iteration was initialized as 100 to search for the 100 number of users that are to be linked. In that, the vehicle was started from the center of that are coverage and serves the link combination between users and the vehicle. Table 1 it discusses about results of different parameters for proposed work and its values.

Wecan see the execution time taken for each simulation result is 5.491 s. We have multi-objective, along with cost, we have distance reduction, etc. Cost function-It is

Fig. 2 Simulation results for instance C101-100 Users and R105-100 Users

![Image](image_000002_0ecc9c1fdb4a106938dda4e9d661f3a88cee09e094837bd26e97ff6e2705714b.png)

![Image](image_000003_7ff09477df7f5db9a945c9951d0373f51e0a83e2f42da52481ae482608fad15d.png)

## OvcrallDistancc Travcllcd: 1828.89 , Vchiclc Count: 15

Fig. 3 Simulation results for instance RC105-100 Users

![Image](image_000004_c3eee580001fa2fd7cd03540ad43dfcf2aeafa15ffe8e3d6d0254fae567116cf.png)

typically viewed as a single-purpose optimal solution that reduces the overall shipping costs of vehicles, which are the combination of their fixed costs plus expenses proportional to the distance traveled.

Figure 4 above shows for C103, C104, R103, and R104 cases. The above experiment uses Tensor Flow (TF) model prediction for the results prediction of ACO-GA. The TF is created based on a machine learning model of a trained functionality, in which the categorization is accomplished by slicing the features into blocks. The primary objective of this approach is to analyze pathways based on distinct blocks of featured paths, which improves both prediction accuracy and classification efficiency. The results show here have improved the accuracy, precision, recall, and F1 score [27] as shown in Table 2. Tensor Flow-This is because the hybridization model performance is calculated from Tensor Flow model as the algorithm learnt from previous cases C101, R101, and measured the future responses.

Table 2 it discusses detection rate, this is used to predict the best routing path mentioned in Fig. 1 where it chooses the best routing path.

In the above table we have used C, R, RC which represent cases. Here the data sets have been used from Solomon benchmark and is used for 100 customers for all cases.

![Image](image_000005_a16631947985e748acbda19a83ba5b50021f81887e0217059cc4ba79e03fe591.png)

Fig. 4 Simulation plot for 100 users of instance a C103, b C104, c R103, and d R104

![Image](image_000006_046ea720da0dda18f1305618a8494c693df8200b51f31a50e32148168df5c054.png)

![Image](image_000007_decd5b057b9571694d4def214b97edbdae5bb7fb091a3fc0cf34a9c3490040a0.png)

![Image](image_000008_d214ff5beb7a9f90143d6fc6fa9f9b9178bcb5263911bda9f4f0eea67c8ad92b.png)

## 5 Conclusion

The paper work proposed the Tensor Flow-based vehicle routing prediction along with the optimal routing path selection of ACO-GA. By referring to the parameters of path distance and the number of vehicles that need to traverse the region, the routing path is generated from the data of vehicle coordinates and the delivery point. This also reduces the fuel cost and the time consumption of overall system. The Tensor Flow predicts the path that are best along with ACO-GA by estimating the cluster of the relevancy that are trained with the feature patterns of data. The experimental result and the graph shows the traveling distance and the amount of area that the vehicle can travel with minimum number of vehicle count. These type of TF-based VRP system improves the prediction performance and the process of optimal path selection compare to other existing model of VRP.

For future papers we can implement optimized VRP paper for different structure of data combination and can integrate with the AI model to update the training model based on the feature update from time instant.

## References

- 1. Qiu M et al (2018) A tabu search algorithm for the vehicle routing problem with discrete split deliveries and pickups. Comput Operat Res 100(2018):102-116
- 2. Min J, Jin C, Lu L (2018) A three-stage approach for split delivery vehicle routing problem solving. In: 2018 8th international conference on logistics, ınformatics and service sciences (LISS). IEEE
- 3. Koç Ç, Laporte G (2018) Vehicle routing with backhauls: review and research perspectives. Comput Oper Res 91:79-91
- 4. Neves-Moreira F et al (2018) The time window assignment vehicle routing problem with product dependent deliveries. Transp Res Part E: Log Transp Rev 116(2018):163-183
- 5. Madankumar S, Rajendran C (2018) Mathematical models for green vehicle routing problems with pickup and delivery: a case of semiconductor supply chain. Comput Oper Res 89:183-192
- 6. Gschwind T, Bianchessi N, Irnich S (2019) Stabilized branch-price-and-cut for the commodityconstrained split delivery vehicle routing problem. Eur J Oper Res 278(1):91-104
- 7. Marques A et al (2020) Integrated planning of inbound and outbound logistics with a rich vehicle routing problem with backhauls. Omega 92:102172
- 8. Sitek P, Wikarek J (2019) Capacitated vehicle routing problem with pick-up and alternative delivery (CVRPPAD): model and implementation using hybrid approach. Ann Oper Res 273(1):257-277
- 9. Liu D et al (2020) Two-echelon vehicle-routing problem: optimization of autonomous delivery vehicle-assisted E-grocery distribution. IEEE Access 8:108705-108719
- 10. Hornstra RP et al (2020) The vehicle routing problem with simultaneous pickup and delivery and handling costs. Comput Operat Res 115:104858
- 11. Li J et al (2020) Branch-and-price-and-cut for the synchronized vehicle routing problem with split delivery, proportional service time and multiple time windows. Transp Res Part E: Log Transp Rev 140:101955
- 12. Wang Y et al (2020) Collaboration and resource sharing in the multidepot multiperiod vehicle routing problem with pickups and deliveries. Sustainability 12(15):5966
- 13. Gayialis SP, Konstantakopoulos GD, Tatsiopoulos IP (2019) Vehicle routing problem for urban freight transportation: a review of the recent literature. Operat Res Dig Era-ICT Challenges 89-104
- 14. Dumez D, Lehuédé F, Péton O (2021) A large neighborhood search approach to the vehicle routing problem with delivery options. Transp Res Part B: Methodol 144:103-132
- 15. Samra B, Semchedine F (2020) A certificateless ring signature scheme with batch verification for applications in V ANET. J Inf Secur Appl 55. https://doi.org/10.1016/j.jisa.2020.102669
- 16. Euchi J, Sadok A (2021) Hybrid genetic-sweep algorithm to solve the vehicle routing problem with drones. Phys Commun 44:101236
- 17. Liu W et al (2021) A hybrid ACS-VTM algorithm for the vehicle routing problem with simultaneous delivery and pickup and real-time traffic condition. Comput Ind Eng 162:107747
- 18. Balasubramaniam V (2021) Design an adaptive hybrid approach for genetic algorithm to detect effective malware detection in android division. J Ubiquitous Comput Commun Technol 3:135149
- 19. Konstantakopoulos GD, Gayialis SP, Kechagias EP (2022) Vehicle routing problem and related algorithms for logistics distribution: a literature review and classification. Oper Res Int J 22(3):2033-2062
- 20. Huang S-H et al (2022) Solving the vehicle routing problem with drone for delivery services using an ant colony optimization algorithm. Adv Eng Inf 51:101536
- 21. International Conference on Automation and Computing Yu H. Chinese Automation and Computing Society in the UK University of Lancaster and Institute of Electrical and Electronics Engineers. Improving productivity through automation and computing: 2019 25th international conference on automation &amp; computing: lancaster University UK IEEE (2019)

- 22. Sokolova M, Japkowicz N, Szpakowicz S (2006) Beyond accuracy, F-score and ROC: a family of discriminant measures for performance evaluation. In: Australasian joint conference on artificial intelligence. Springer, Berlin, Heidelberg, pp 1015-1021
- 23. Min J, Lu L, Jin C (2022) A two-stage heuristic approach for split vehicle routing problem with deliveries and pickups. LISS 2021. Springer, Singapore, pp 478-490
- 24. Solomon MM (1987) Algorithms for the vehicle routing and scheduling problems with time window constraints. Oper Res 35(2):254-265
- 25. Revanna JKC, Al-Nakash NYB (2022) Analysis of optimal design model in vehicle routing problem based on hybrid optimization algorithm. In: 2022 4th International conference on advances in computing, communication control and networking (ICAC3N-22). IEEE
- 26. ZhaoF,YaoZ,LuanJ,SongX(2016)Anovelfusedoptimizationalgorithmofgeneticalgorithm and ant colony optimization. Math Prob Eng
- 27. Revanna JKC, Al-Nakash NYB (2022) Vehicle routing problem with time window constrain using KMeansclustering to obtain the closest customer. Global J Comp Sci Technol 22(D1):2537