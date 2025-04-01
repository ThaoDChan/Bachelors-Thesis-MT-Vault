## Development of Machine Learning Based State-of-Charge Prediction Model for Plug-in Electric Vehicle's Relocation in Sharing System

Subhasish Deb , Arup Kumar Goswami , Member, IEEE , Bikram Ghosh Hajra, Rahul Lamichane Chetri , Mitadru Roy, and Rajesh Roy

Abstract -This paper presents a Plug-in electric vehicle (PEV) relocation strategy in a sharing system considering machine learning based state-of-charge (SOC) prediction model. The proposed work involves the machine learning approaches like the Adaboost algorithm and XGBoost algorithm for PEVs SOC prediction in relocation problems for the first time. Also, the low-capacity batteries of PEVs are considered as it is affordable by the common people with less charging time. It is also believed that the large number of low capacities PEVs based taxi owners in metropolitan cities will be attracted towards relocation-based sharing system considering machine learning based SOC prediction model. The customer request for sharing PEVs will be reflected in the management system at the beginning. Assuming all the PEVs with a certain level of SOC during pre-relocation are connected to the management system. The customer request for PEVs service will beassigned to the nearest PEVs depending on the predicted value of SOCbymachinelearningapproaches.Themachinelearning-based predicted value of PEVs SOC will help the management system to identify the nearest PEVs for the customer sharing system. The present work steps are listed here. Firstly, the PEVs with higher predicted SOC value by machine learning have been shared with the customer from the nearest location considering descending order of their SOC. Further, the rest of the PEVs will be assigned to the nearest charging station by the PEV sharing management system. Secondly, the customer request is assigned from the station to relocate PEVs with higher SOC values in descending order. Different case studies are assumed including one practical scenario of Mumbai, India. The optimization problem of multi-depot vehicle routing problem (MDVRP)withtheminimizationofrelocationcost has been solved by IBM CPLEX optimizer in this work. Finally, the proposed work efficacy has been demonstrated with some validated results including less relocation costs for all the case studies under consideration with lower capacity based PEVs.

Manuscript received 1 March 2022; revised 8 June 2022, 3 November 2022, and12February2023;accepted28March2023.Dateofpublication1May2023; date of current version 19 July 2023. Paper 2021-TSC-1604.R3, presented at the 2020 IEEE International Conference on Power Electronics, Drives and Energy Systems (PEDES), Jaipur, India, Dec. 16-19, and approved for publication in the IEEE TRANSACTIONS ON INDUSTRY APPLICATIONS by the Transportation Systems Committee of the IEEE Industry Applications Society [DOI: 10.1109/PEDES49360.2020.9379906]. (Corresponding author: Arup Kumar Goswami.)

Subhasish Deb is with the Mizoram University, Aizawl 796004, India (e-mail: subhasishdeb30@yahoo.co.in).

Arup Kumar Goswami, Bikram Ghosh Hajra, Rahul Lamichane Chetri, Mitadru Roy, and Rajesh Roy are with the National Institute of Technology Silchar, Silchar 788010, India (e-mail: gosarup@gmail.com; bikramghosh.bgh@gmail. com; rahullamichane@gmail.com; mitadru2014@gmail.com; gdroy.1999@ gmail.com).

Color versions of one or more figures in this article are available at https://doi.org/10.1109/TIA.2023.3271967.

Digital Object Identifier 10.1109/TIA.2023.3271967

Index Terms -Adaboost algorithm, and XGBoost algorithm, machine learning approaches, plug-in electric vehicle (PEV), relocation in sharing system, state-of-charge (SOC).

## I. INTRODUCTION

T HE rapid worldwide expansion of electric mobility indicates the increase in non-dependency on fossil fuel-based vehicles [1]. At the same time, the greenhouse gas (GHG) also reduces as the PEVs penetration increases. But with the PEVs penetration, the emission, pollution, congestion, etc., also reduces [2]. Therefore, sustainable transport reduces all these effects. The main aspect of the smart city concept is mobility, as shown in [3]. Similarly, mobility refers to service to the users; relatively, a new concept has been addressed [4].

A new concept of mobility like electric vehicle (EV) sharing is spreading faster in smart cities. The vehicles can be rented for a short duration as per the requirements [5]. The stationbased approach in the vehicle sharing concept is categorized by two strategies: one-way and round-trip rental strategy. The only possibilities in a one-way rental strategy are pick-up and drop-off in different stations. Whereas, in a round-trip rental strategy, the possibility is to a drop-off in the same station where from picked-up. Also, the free-floating approach allows the vehicle to pick up and drop off at any location in the city. But in both station-based and free-floating approaches, the battery SOC is a major factor for a trip by users [6].

In the EV sharing system, the relocation is handled by different decisional points of view like strategy, tactics, and operation basis. The relocation strategy aims to manage the imbalance in EV distribution in the sharing station in cities [7] and [8]. However, EV relocation is challenging compared to conventional vehicle relocation since the EV should have sufficient SOC while picking up the items [9].

Theproblemofrelocation can also be categorized as operatorbased and user-based methods. In the operator-based relocation method, the company staff operator performs the relocation process when it is urgent. On the other hand, the user-based relocation method is performed by users with incentives from the company during the change in destination [10].

The EV relocation can also be performed on both static and dynamic scenarios. In a static scenario, the vehicle relocation is performed during parking the vehicle at the station. In a

0093-9994 ' 2023 IEEE. Personal use is permitted, but republication/redistribution requires IEEE permission. See https://www.ieee.org/publications/rights/index.html for more information.

dynamic scenario, the relocation is performed in a real-time scenario during the running condition of vehicles [11]. The station-based and free-floating strategies considering static and dynamic relocation are addressed in each of the operator-based and user-based relocation problems. In the operator-based approaches, the company staff drives the vehicle from source to destination. In [12], the revenue generated related to each performed relocation request and the cost of each operator has been shown. In [13], an optimization strategy for relocation of vehicles and staff with three different models has been shown.

The EV relocation in the optimization framework has been constructed to maximize customer satisfaction level and minimize relocation cost [14]. The car-sharing system with dynamic relocation and a Markov chain model for predicting the future states of the station has been shown in [15] and [16]. In a one-way EV sharing system, the profit has been shown by investigating the effect of temporal and spatial flexibility with different options for trip requests [17].

In a free-floating system, the relocation of the vehicle with an operator-based approach has been adopted in the bike-sharing system. The global positioning system data-based bike-sharing system has been shown in [18]. Also, the validation method demonstrated the relocation process's effectiveness. In [19], demand direction-based bike-sharing in the free-floating approach considering dynamic relocation has been shown.

In [20], the competition of car-sharing operators has been investigated on two levels. The cost-effective relocation for reducing relocation driver's walking time has been shown in [21]. The EV relocation in car sharing system with three objectives such as maximizing customer satisfaction and minimizing number of workers involved and longest route duration has been shown [22].

In the case of the user-based method, the majority ask users to update their trips dynamically. In [23], floating shared cars as an alternative transportation mode has been formulated using a dynamic user balance model in the different pricing schemes. In [24], the EV relocation has been developed in two steps. The first one is the EV relocation in the Interzone by moving EVs in the microscopic zone. The second is intrazonal relocation by driving EV within the same zone. In [25], the use of interest points for pricing strategy to balance vehicles in different areas by the free-floating approach has been shown.

The area-pricing model-based decision support system has been developed to balance the supply and demand of vehicles, and additional cost identification has been shown in [26]. In [27], a mathematical model for relocation to provide incentives to the users to change the vehicle position of drop by discount rate has been presented. In [28], a dynamic relocation model is designed and applied in real-time in an unbalanced fleet of vehicles concerning the demand. The user's travel behavior in the EV car-sharing system has been shown in [29], [30], [31]. Some other approaches in EV relocation in sharing systems have been shown in [32] and [33]. In [34] and [35], the machine learning approaches like gradient boosting method (GBM) and random forest method (RFM) are utilized for PEVs SOC prediction. In [36] and [37], the Adaboost algorithm and XGBoost algorithm details are given. Also, the VRP problem in home health care system has been addressed in [39], [40], [41], [42].

In the smart city, the faster growth of electric mobility due to the user flexibility and environmental concern makes the management platform to consider only PEVs of different categories in this work. From the service provider perspective, the management platform requires a strategy to satisfy customer requests by sharing information about nearest PEVs SOC or station parked PEVs SOC. The contribution in terms of novelties of the work is summarized below.

- a) In this work, the PEVs SOC prediction has been made by machine learning approaches such as the Adaboost algorithmandXGBoostalgorithmtohelpthemanagement system decide the PEVs for the customers. The predicted value of PEVs SOC by machine learning approaches has been utilized for PEVs relocation in a sharing system for the first time.
- b) The multi-depot vehicle routing problem (MDVRP) with the minimization of relocation cost has been solved by CIPLEX optimizer for several case studies including a real-time practical scenario. The machine learning based predicted amount of PEVs SOC are exploited to recognize the PEVs status prior to relocation. The effective minimization of relocation cost for all the MDVRP case studies including practical scenario indicates the efficacy of the proposed work.

The organization of the paper is as follows. Section I presents the introduction, literature review and contribution of the work. Section II provides the problem overview. Section III gives brief discussion about Methodology i.e., A. Adaboost algorithm, B. XGBoost algorithm, C. PEVs SOC Calculation using Travel Distance and D. flowchart of the routing of PEV. Section IV includes detail about routing problem formulation. Section V provides the results and discussions. Lastly, Section VI gives the conclusion.

## II. THE PROBLEM OVERVIEW

The article focuses on PEV's relocation strategy from station to the users. In this work, multi-depots are considered where PEVs are parked. In this study, PEVs with large number are allocated for relocation depending on the status of SOC.

The status of PEVs SOC during pre-relocation to the users at different destinations has been forecasted using machine learning approaches like Adaboost and XGBoost. The customer demand for shared PEVs has been reflected at the management platform at the beginning. The management platform analyzes the ride details and identifies the available PEVs near the customer with sufficient SOC for the particular ride. The management platform works on the smart technology like apps through which customer approach for a PEV and operator assign a PEV to the customer. The Adaboost and XGBoost algorithms are utilized to predict PEVs SOC prior to assigning to the customer. It is assumed that PEVs can charge their vehicles at designated stations. The PEVs with lesser value SOC below a specific prescribed limit will be charged at the nearest station. Charging priority will be given to the PEVs having less SOC in ascending

Fig. 1. The MDVRP based PEVs relocation in sharing system.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000000_5cf581f4898eea940efd5baec67b787b92439c8c3bf7007052254bf9bb3941ee.png)

order. Once the charge of the PEVs rises above the prescribed limit, the PEVs will be ready for relocation based on their SOC.

The installation of the station has been decided randomly for all the assumed case studies in this work. In the MDVRP for Mumbai, the longitude and latitude of different densely populatedareasaretakenasthelocationofcustomers.Moreover, the longitude and latitude of depots of Mumbai are provided in the paper. The IBM CPLEX optimizer has been utilized to reduce the relocation cost in a sharing system. The Fig. 1 describes the relocation strategy in a sharing method.

## III. METHODOLOGY

Machine learning approaches are gaining popularity due to their application in different domains. This work considers Adaboost algorithm and XGBoost algorithm for forecasting PEVs SOC, which are explained in below.

## A. Adaboost Algorithm

TheAdaboostalgorithmisaboostingalgorithmthatgenerates a strong predictor from multiple integrated weak predictors. The predicted output is improved based on training of samples corresponding to the distribution of new weight. The following are Adaboost calculation steps [36].

- a) Assuming a sample set

U = { ( x , y i i ) | i = 1 2 , , . . . .., N } and D t ( ) i , t = 1 2 , , . . . , T denotes the distribution of weight for the samples at t th iteration. Also, weight distribution initialization at first iteration: D t ( ) i = 1 /N at t = 1 .

- b) Forecasted output f t ( x i ) is calculated considering distribution of weight. Next, calculation of error forecasting:

$$e _ { t } ( i ) = | f _ { t } ( x _ { i } ) - y _ { i } | \,, \ e _ { t } ( i ) \in [ 0, 1 ] \quad \ \ ( 1 )$$

ω

- c) The proportional error calculation: ε t = ∑ N i = 1 D t ( ) i e t ( ) i . d) Connection weight calculation:

t

=

t

- 1 2 / log( 1 /β t ) where, β t = -e) Weight distribution updation as per the following:

ε /

(

1

ε

t

)

.

$$D _ { t + 1, ( i ) } = ( D _ { t, ( i ) } \times \beta _ { t } ^ { - \varepsilon t } ) / V _ { t + 1 } \, \text{and} \, V _ { t + 1 } = \sum _ { i = 1 } ^ { N } D _ { t + 1, ( i ) } \\ \text{for} \, \text{$A$ $\mathfrak{ }D$ and $V$ or a distinct amount normal can be at $d$}$$

Here, β , D and V are adjustment parameter, sample set, and training sample weight.

- f) The final predictors after the final iteration T can be expressed as:

$$F ( x ) = \sum _ { t = 1 } ^ { T } \omega _ { t } f _ { t } ( x ). \text{ \quad \ \ } ( 3 )$$

## B. XGBoost Algorithm

The XGBoost algorithm, an extreme gradient boosting decision tree is a popular machine learning approach. In XGBoost, the continuous addition of regression trees generates new tree that is fitted with previous models residuals. The XGBoost has the accurate loss function that utilizes the Taylor series expansion function [37]. The XGBoost algorithm is explained in below [37].

In an n examples dataset, D = { ( x , y i i ) | i = 1 2 , , . . . .., n } , where, x i is the input data and y i is the target data. After training a model with K trees, the model result:

$$\hat { y } _ { i } = \sum _ { k = 1 } ^ { K } f _ { k } ( x _ { i } ), \ f _ { k } \in W$$

Where, W is called classification and regression trees and f(x) is regression tree.

$$W = \{ f ( x ) = \mu _ { l ( x ) } \}$$

Where, l(x) and µ are the leaf node and leaf score respectively. The forecasted results of the t th iteration:

$$\hat { y } _ { i } ^ { t } = \hat { y } _ { i } ^ { t - 1 } + f _ { t } ( x _ { i } )$$

So, the objective function of XGBoost algorithm is:

$$J ( f _ { t } ) = \sum _ { i = 1 } ^ { n } L ( y _ { i }, \hat { y } _ { i } ^ { t - 1 } + f _ { t } ( x _ { i } ) + \psi ( f _ { t } ) ) \quad ( 7 )$$

Where, L and ψ (f t ) are the loss function and model complexity. Also,

$$\psi ( f _ { t } ) = \gamma. T _ { t } + \lambda \frac { 1 } { 2 } \sum _ { j = 1 } ^ { T } \mu _ { j } ^ { 2 }$$

Where, γ and T are the coefficient of penalty and leaves number respectively. Using Taylor series expansion of second

order, (7) can be written as:

$$J ( f _ { t } ) = \sum _ { i = 1 } ^ { n } [ L ( y _ { i }, \hat { y } _ { i } ^ { t - 1 } ) + g _ { i } f _ { t } ( x _ { i } ) + \frac { 1 } { 2 } h _ { i } f _ { t } ^ { 2 } ( x _ { i } ) ] + \psi ( f _ { t } ) \quad & \prod _ { \substack { \text{$\Delta$} \\ \text{$\Delta$} } } ^ { \infty }$$

Where,

$$g _ { i } = \frac { \partial L ( y _ { i }, \hat { y } _ { i } ^ { t - 1 } ) } { \partial \hat { y } _ { i } ^ { t - 1 } } \quad \quad ( 1 0 )$$

And

$$h _ { i } = \frac { \partial ^ { 2 } L ( y _ { i }, \hat { y } _ { i } ^ { t - 1 } ) } { \partial \hat { y } _ { i } ^ { t - 1 } } \quad \quad ( 1 1 )$$

Thus, the final objective function of XGBoost algorithm is:

$$J ( f _ { t } ) = \sum _ { i = 1 } ^ { n } \left [ g _ { i } \mu _ { l } ( x _ { i } ) + \frac { 1 } { 2 } h _ { i } \mu _ { l } ^ { 2 } ( x _ { i } ) \right ] + \gamma T + \lambda \frac { 1 } { 2 } \sum _ { j = 1 } ^ { T } \mu _ { j } ^ { 2 } \\ ( 1 2 ) \left ( 1 2 ) \right ) \left ( 7 \right ) \left ( \text{Pl} \right )$$

## C. PEVs SOC Calculation Using Travel Distance

The information of total driving time, driving distance helps to recognize the status of SOC at PEVs tour halt. Therefore, SOC of PEVs during journey halt [35]:

$$S O C \left ( t _ { f } \right ) = S O C \left ( t _ { i } \right ) - \frac { u. d } { b } \times 1 0 0 \quad \ \left ( 1 3 \right ) \quad \text{The} \quad \\ \quad \ \ \text{the mi}$$

Where, u is the specific energy consumption, d is the total distance driven and b is the battery capacity. Similarly, PEVs SOC at the start of next tour:

$$S O C \left ( t _ { f } + t _ { p } \right ) = & \min \left \{ S O C \left ( t _ { f } \right ) + \frac { \eta \times s \times t _ { p } \times 1 0 0 } { b }, \ 1 0 0 \% \right \} \\ \intertext { w n s........................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................$$

Where, η is the charging efficiency and s is the charging rate in kW. The battery duration of charging t c :

$$t _ { c } = \frac { ( S O C \left ( t _ { f } + t _ { p } \right ) - S O C \left ( t _ { f } \right ) ) \times b } { \eta \times s } \quad ( 1 5 ) \quad \begin{array} { c } \dot { \bar { K } } \end{array} \dot { \bar { S } } \text{is} \\ \text{The PREV} \text{ start time } t. \text{ have been desioned by the normal} \quad \begin{array} { c } \dot { \bar { \ } s } \end{array} \dot { \bar { K } } \text{ is}$$

The PEVs start time t i have been designed by the normal distribution function.

## D. Flowchart of the Routing of PEV

Routing of the PEV is described in the flowchart of PEVs MDVRPforthe relocation in sharing system in Fig. 2. The steps of the proposed methods are as below:

- 1) The management platform receives the request for PEVs sharing.
- 2) The predicted value of PEVs SOC by machine learning approaches are determined as vehicle arrives at station.
- 3) The station will assign the nearest PEVs for MDVRP with the highest predicted SOC value by the machine learning approach.
- 4) Further, the less SOC of PEV will be charged in the nearest station until SOC rises to the prescribed limit.
- 5) The input parameters of PEVs, depots/stations and customer demands are considered for the MDVRP optimization problem by IBM CIPLEX optimizer.
- 6) The management system will preferentially relocate the PEV with the highest SOC in descending order.
- 7) OnceaPEVisrelocated, its actions will be continued until it arrives at a station.
- 8) The overall best solution of the MDVRP has been generated and the algorithm stops.

Fig. 2. Flowchart of PEVs MDVRP for relocation in sharing system.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000001_ab2249f4f654d2481f5f5e0e122b2b121b885745c3bf30149003193e739ba9a6.png)

## IV. ROUTING PROBLEM FORMULATION

The objective function of the proposed work of MDVRP is the minimization of the PEVs relocation cost in sharing system (16) [33].

## Minimize

$$C o s t _ { \text{Re} l } = C _ { k }. \sum _ { i = 1 } ^ { J } \sum _ { j = 1, i \neq j } ^ { J } \sum _ { k = 1 } ^ { K } D _ { i, j } \times x _ { i, j, k } \quad ( 1 6 )$$

/negationslash

Where, C k is kilo-meter cost for relocation process. J is set of stations.

D i,j is distance between station i and station j.

K is set of EV's to be relocated. x i,j,k is binary decision variable. And S j is the number of PEVs.

## Constraints:

1)

$$s _ { j } = \hat { s } _ { j } + \sum _ { i = 1 } ^ { j } \sum _ { K = \stackrel { K } { s } _ { i } } ^ { K } x _ { i, j, k } - \sum _ { i = 1 } ^ { j } \sum _ { K = \stackrel { K } { s } _ { j } } ^ { K } x _ { j, i, k } \quad ( 1 7 )$$

- 2) The number of PEVs in each station is less than the maximum admissible number.

$$S _ { j } \leq N _ { \max, j } \quad \quad ( 1 8 ) \\ \dots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots$$

- 3) The number of PEVs in each station is greater than the minimum admissible number of PEVs.

$$S _ { j } \geq N _ { \min, j } \quad \quad ( 1 9 ) \\ \cdot \cdot \cdot \cdot \cdot \quad \, - \cdot \cdot \cdot \cdot \cdot \cdot \cdot$$

- 4) This ensures that only vehicles k /2208 K in station i with enough autonomy can relocate to station j .

$$x _ { i, j, k } \leq v _ { i, j, k } \text{ \quad \ \ } ( 2 0 )$$

Fig. 3. Predicted versus actual SOC by Adaboost at the stop of EVs trip for (a) Case A, (b) Case B, and (c) Case C.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000002_29ed42c545d29f3a0ec2b49bf6d0ac4971dbe2910607a35cd5a1545c9a495443.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000003_3c948c54781ebe81620b955ffb1f8888c0f3163095836553fbd016eb730f0d7b.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000004_77f6c11adff66775a42790b8291472250ec0ab8328d6c70b6a24bbfde5d56f3b.png)

Fig. 4. Predicted versus actual SOC by XGBoost at the stop of EVs trip for (a) Case A, (b) Case B, and (c) Case C.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000005_db2a8bbe7a6de5a0b0fe5b19f5fc02d305a625c4405e827a182c9b3dc3d7bd13.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000006_db70937bcf7845798d6ef3741253f71da7e89ee7f3803df678d5b5d2bcde9209.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000007_44b397711c16c9ddb6534f851dbc19c4cc5e68bbb93b4d3a4c60c117b24287f0.png)

- 5) Each vehicle k can perform only one trip from the departure station i to a destination station j .

TABLE I ERRORS CALCULATION USING ADABOOST AND XGBOOST

$$\sum _ { i = 1 } ^ { J } \sum _ { i = 1 } ^ { J } x _ { i, j, k } \leq 1 \quad \quad ( 2 1 )$$

- 6) The SOC of PEVs battery must be within the prescribed limits of maximum (SOC max) and minimum (SOC min) values.

$$S O C _ { \min } \leq S O C \leq S O C _ { \max } \quad \ \ ( 2 2 ) \quad \overline { \ B R }$$

- 7) Discharge protection during travel.

$$\S O C \left ( t _ { 2 } + p \right ) \geq S O C \left ( t _ { d } \right ) \ast \zeta \quad \ \left ( 2 3 \right ) \\ \tilde { \lambda } \quad \ \begin{array} { c } S O C \left ( t _ { d } \right ) \ast \zeta \quad \\ \quad \ \end{array} \quad \ \left ( 2 3 \right )$$

Where, S j no of PEV's in station j after relocation process. N max ,j max number of PEV's is given by the number of parking lots of a station where PEV's can be recharged.

N ,j the minimum number of PEVs in each station j .

v i,j,k where v i,j,k = 1 if the PEV k K autonomy to travel from station i to station j .

min /2208 has enough battery t 2 is time of ending the trip by customer.

t 1 is time of starting the trip by customer. p is parking time.

t d is the travelling time, equals to ( t 2 -t 1 )

ζ is the safety factor, determined by efficiency of customer.

## V. RESULTS AND DISCUSSIONS

Therelocation-based sharing system considers different types of PEVswithbattery capacities ranging from 14 kWh to 18 kWh. It is believed that lower capacities batteries are more flexible and available in affordable prices for the common users. Three case studies are considered in this work, namely:

- /a114 Case A: 800 PEVs distributed in 2 stations
- /a114 Case B: 500 PEVs distributed in 2 stations
- /a114 Case C: 300 PEVs distributed in 2 stations

In this work, the specific energy consumptions are considered depending upon the capacity of batteries. Generally, specific energy consumption is kept at 0.1716 kWh/km, 0.1904 kWh/km, 0.2181 kWh/km and 0.2073 kWh/km. Probability distribution function (PDF) based initial SOC and travel distance has been generated. The standard deviation and mean value are 10 km and 50 km for travel distance of PEVs from their source of starting towards stations. The mean and standard deviation for starting time PEVs SOC are 85% and 5% respectively from their source of starting. This initial SOC is generated by PDF only to calculate SOC of PEVs once they reach to the nearest customer. The prediction of PEVs SOC has been done by using Adaboost and XGBoost algorithms.

Fig. 5. Relocation of PEVs among customers with 2 depots/stations for (a) Case A, (b) Case B, and (c) Case C.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000008_ae65bab08378266d346edfb6af97f98fac547b77bfdf8f0f3f5410b55e5e89c6.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000009_c46434f32e91d16e011a60fdefaa0d749b5ece6a1057fdb5adce6386ed3a345d.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000010_8879d925833f7da2e905929fd6ebdf32a225a6ec3152654638a94e324c474f0b.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000011_c9b4c4dcb41d1b0e40e01e58dfaa8889ad1b8ed40cd6818b9d04ac8c3f074d88.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000012_203040eb1babde906f97b2f85460f48260c0e61efd3f0988e0b0e7b7abef833a.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000013_68004a5f3809a92dadea400826fbe182250432d5a98525b3672ac1f21f374d63.png)

Fig. 3 shows the predicted values vs. actual values of SOC at the end of the PEVs trip using Adaboost for all the case studies. Similarly, Fig. 4 demonstrates the predicted SOC vs. initial SOC of PEVs using XGBoost for all the case studies. It is observed that the predicted SOC in Figs. 3 and 4 is near about their calculated actual values. The comparative analysis between Adaboost and XGBoost has been shown in Table I. Table I shows the results of mean absolute percentage error (MAPE), relative squared error (RSE), root mean squared error (RMSE), coefficient of correlation (R) and mean squared error (MSE). The evaluation indexes are:

$$M S E = \frac { 1 } { n } \sum _ { i = 1 } ^ { n } \left ( y _ { i } - \hat { y } _ { i } \right ) ^ { 2 }, \text{MAPE} = \frac { 1 } { n } \sum _ { i = 1 } ^ { n } \frac { \left ( y _ { i } - \hat { y } _ { i } \right ) } { y _ { i } } \times 1 0 0 \, \% \quad \text{for further}$$

$$\mathbb { C } _ { \text{se} } \ R M S E = \sqrt { \frac { 1 } { n } \sum _ { i = 1 } ^ { n } \left ( y _ { i } - \hat { y } _ { i } \right ) ^ { 2 } }, \\ \tilde { \nu } _ { \text{se} } \ \tilde { n } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se} } \ \tilde { \nu } _ { \text{se}) }$$

$$\text{vs.} \text{$es$}. \text{$R^{2}=1-RSE=1-\frac{\sum_{i=1}^{n}\left(y_{i}-\hat{y}_{i})^{2}}{\sum_{i=1}^{n}\left(y_{i}-\overline{y}_{i})^{2}}$} \\ \text{$is$} \\ \colon I. \quad \text{$In this work, different case studies are conside}$$

In this work, different case studies are considered by choosing different number of PEVs. PEVs with different SOC values at the end of trip are considered for PEVs relocation in sharing system. In the Adaboost and XGBoost, the training and testing data are 75% and 25%. The result for MSE, RMSE and MAPE using Adaboost and XGBoost for case numbers A, B, and C are shown in Table I. Hence, it is visible that XGBoost performs better than Adaboost in terms of MSE, RMSE, MAPE etc. So, for further analysis of PEVs SOC prediction-based relocation in sharing, result of XGBoost algorithm is considered.

Fig. 6. Total relocation cost vs. number of depots for (a) Case A, (b) Case B, and (c) Case C.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000014_d42fd00c8feb56697bef52b22994082d1fb7d12dbeccfa33d84027279f9ad498.png)

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000015_4b40ed860f6379409dceebae21c58d5afd0fbe743ef886cf625c65b8807c1648.png)

TABLE II RELOCATION COST ($/HR) BY IBM CIPLEX OPTIMIZER

Simulation of the MDVRP Optimization: Total number of PEV'saredistributed among stations. The PEV's are categorised for serving the customer according to their SOC. The optimal relocation of MDVRP, characterized by relocated PEVs, connected between station and customers, has been solved by IBM CIPLEX optimizer for the different case studies. The relocation cost of MDVRP found by using IBM CIPLEX optimizer considering 800 PEVs (case A), 500 PEVs (Case B) and 300 PEVs (Case C), as shown in Table II. The data of the simulation is scaled to the constraints limited by the CPLEX optimization student version limits.

In Table II, the relocation cost in ($/Hr) for MDVRP by IBM CIPLEX optimizer used to reduce as we increase the number of stations/depots. By increasing the number of depots, additional route alternatives between depots and customers become available. Due to this gradual increment in number of depots, the location of some of the depots stay near to the users. Since the relocation cost considers the travel distance, the relocation costs reduce further with the increment of depots. Since the case A consists highest number of PEVs, thus the chances of having sufficient SOC level for a large number of PEVs can be assumed. The relocation cost decreases as the number of depots/stations increases. In these case studies, we have assumed approx. 350 sq. km between depots and customer positioning in a small blankspace. The relocation cost involves energy cost only, which relates to the distance travelled by the vehicle. However, both the capital cost and daily human labour cost are not considered in the work and may be considered in future. It is true that the overall relocation cost reduces as the number of stations

TABLE III THE TRAVEL DISTANCE (KM) FOR ALL THE CASES

increases, which has been demonstrated in the paper through different case studies. It is also true that both the capital cost and the daily human labour cost will further increase with the increment in number of stations and vehicles. Since our main objective is to minimize the relocation cost only, hence, both the capital cost and the daily human labour cost may be considered in future for multi-objective based MDVRP in accordance to the optimum number of stations and vehicles.

Assuming per hour relocation cost is $1. This assumption is done only to simplify our study as well as calculation since no real data of relocation cost per hour is available for the MDVRP case studies. For case study A, the total relocation cost has been found as 88.93 $/Hr in one iteration for 2 stations, 96.98 $/Hr in one iteration for 4 stations, 90.05 $/Hr in one iteration for 6 stations, 72.78 $/Hr in one iteration for 8 stations, and 65.98 $/Hr in one iteration for 10 stations. Similarly, in case B, the total relocation cost has been found as 238.65 $/Hr in one iteration for 2stations, 216.06 $/Hr in one iteration for 4 stations, 261.98 $/Hr in one iteration for 6 stations, 207.52 $/Hr in one iteration for 8 stations, and 122.79 $/Hr in one iteration for 10 stations. Also, in case C, the relocation cost has been found as 1816.31 $/Hr in one iteration for 2 stations, 1332.26 $/Hr in one iteration for 4 stations, 805.95 $/Hr in one iteration for 6 stations, 753.66 $/Hr in one iteration for 8 stations, and CIPLEX Limit Exceeded for 10 stations. Even for both case B and C, the relocation cost reduces as the number of stations further increases in MDVRP study.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000016_a06b50f558ecd6fb0c69c40be824b7fd2ca1375012072facbcc5d7b7f862adfa.png)

Fig. 7. The non-relocated Depots (in red Dot) and Customers (in blue Dot) Locations across Mumbai, India.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000017_762a5c61a5d89dfafa4b76b39355b83098fa33249c89d11a3d5a93262fa3d280.png)

Fig. 5 depicts the relocation of PEVs between station and customers. The MDVRP is solved for three cases namely Case A, Case B, and Case C that studies the routing of 10, 40, and 90 customers. The distribution of customers around the stations hasbeenfoundbyamathematicaldistributionprocess( a 2 × 10), where a /2208 0,1,2 …. serving with two depots. This indicates the exponential rise in customers.

Moreover, the depots and customer are positioned in a approx. 350 sq. km. blank space distributed with Poisson distribution function f ( x ) = λ x x ! e -λ , where λ is the expected rate of occurrences, the number of customers in these cases. Fig. 5 demonstrates the non-relocated and relocated PEVs among the customers from two number of depots for the different case studies A, B and C. Thus, the Fig. 5 depicts the effectiveness of the relocation-based sharing system for MDVRP using IBM CPLEX optimizer for all the case studies. The cost versus number of depots for case studies A, B, and C has been shown in Fig. 6. The MDVRP is further examined by changing the number of depots along with different sets of customers. The minimumandmaximumdistancesforallthecases are addressed in Table III. The distances are found based on rectilinear distance between depot and customer. Both the minimum and maximum distance indicate the nearest customer and the farthest customer from depot.

For all these cases A, B, and C the number of customers 10, 40, and 90 are considered with the variation of number of depots. As shown in Fig. 6, the relocation cost of MDVRP reduces as the number of depots increases. Moreover, the iteration times for case A are 0.08 sec. for 2 depots, 0.31 sec. for 4 depots, 0.27 sec. for 6 depots, 0.27 sec. for 8 depots, and 0.30 sec. for 10 depots. Similarly, iteration times for case B are 0.11 sec. for 2 depots, 0.34 sec. for 4 depots, 0.38 sec. for 6 depots, 0.35 sec.

for 8 depots, and 0.33 sec. for 10 depots. And iteration times for case C are 0.05 sec. for 2 depots, 0.31 sec. for 4 depots, 0.34 sec. for 6 depots, 0.34 sec. for 8 depots, and CIPLEX limit exceeded for 10 depots respectively.

The MDVRPis a critical issue in Mumbai, a city with a population of over 21 million and rapidly growing demand for electric vehicles. The efficient distribution of electric vehicles is crucial to meet the increasing demand for sustainable transportation solutions. The MDVRP aims to find the most efficient route for electric vehicles, considering the limited charging infrastructure, increasing population density, and high demand for electric vehicles.

In the practical case study of the MDVRP in Mumbai, we have taken into account multiple factors to optimize the distribution of electric vehicles in the city. One of the key factors considered is the real-time population density data of Mumbai, which is further used to identify multiple locations of high customer demand. In this work, real-time data from the charging depots of electric vehicles currently present in Mumbai has been used in order to allocate vehicles to areas where customer demand is high in real-time. The real-time charging depot data was gathered from various sources, including both government agencies and private companies operating within the city. By considering these multiple factors and utilizing a data-driven approach, the proposed case study provides a practical solution to the challenges faced by Mumbai, such as traffic congestion, overcrowding etc.

The limitation on the number of customer locations was imposed due to the optimization constraints of the student version of the optimization software, CIPLEX. The software was used to analyze the data and find the most efficient distribution of electric vehicles, but its student version has limitations on the size and complexity of the problems. By taking these limitations into account, a practical and meaningful study of the MDVRP in Mumbai has been conducted. The electric vehicles were stationed at these depots, either from the beginning of their routes or after completing a delivery. The depots calculate the SOC of each vehicle, based on the SOC calculation described in Section III. The MDVRP optimization algorithm has been applied to determine which vehicle would serve which customer.

The optimization algorithm considered several factors, including the customer's location, the current location of the PEVs, and the vehicle's SOC, and to determine the most efficient route. Theoptimizationalgorithm was able to provide valuable insights into the most efficient routes for electric vehicles in Mumbai, taking into account the complex interplay between the supply of charging resources, demand for electric vehicles, and the location of charging depots. The proposed case study of MDVRP in Mumbai demonstrates the importance of using real-time data and optimization techniques to optimize the distribution of electric vehicles.

The longitude and latitude of depots and customers are given in Table IV. The study area of Mumbai is 386.56 sq. km. The population density of Mumbai is 25,357 people per sq. km. And per capita income is Rs. 1,91,736 in 2019-2020. The locations of customers are selected from densely populated areas of Mumbai city whose longitude and latitudes are mentioned in Table IV.

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000018_db26e2a13cb447e51cce416317441fe02589b26c13d1199a5c221c725339a1ff.png)

Relocated MDVRP for Mumbai-Part A

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000019_459ff884c4ead2c79447c6c4d252c7078d304d4f2761b88b508fa2d84e6ef076.png)

Relocated MDVRP for Mumbai-Part B

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000020_c90d06317f458ba6ce3aa51b9e655e5fd6d281363c15bf8ea56d1fc525aab6e9.png)

Relocated MDVRP for Mumbai-Part €

![Image](Papers_Converted/0_Artifacts/[deb2023]_Development_of_machine_learning_based_state-of-charge_prediction_model_for_plug-in_electric_vehicles_relocation_in_sharing_system_artifacts/image_000021_527f55e1712c19bda2da1256a8be4d4747f3d9ef3500b459a74f6a66cd97ad30.png)

Relocated MDVRP for Mumbai-Part D

Fig. 8. The practical case study of relocated MDVRP for Mumbai, India.

Population of Mumbai city on any given day is 10 mill (assumption) [38]. Out of this, 60% belongs to middle class which are our target population in this study. The remaining upper and lower-class people hardly use cabs. Also, assuming 0-15 years of age (almost 50%) among the middle-class people do not use PEVbased cabs frequently. So, our target population in Mumbai city who uses the cabs lies approximate 3 mill (50% of 60% of 10 mill). Assuming that the 20% of middle-class people of Mumbai city is using the PEV based cabs on daily basis. It is to be mentioned that only the densely populated locations are considered as customers pick up points in the work. But due to the limitations of CIPLEX student version, limited customer locations are considered in the work. In Fig. 7, the non-relocated depots positioning of Mumbai city has been shown. The depots location in red dots and customers locations in blue dots are presented in Fig. 7.

probability density of potential customers is estimated to be highest. These locations have the potential to serve multiple customers. The total number of PEVs in the 31 depots are considered as 500 whereas, total number of customers in 29 customer pick-up locations are considered as 5000. The information regarding the customers at each of these locations is presented in Table IV. In the MDVRP, most of the customers are served by the available vehicles through relocation. Fig. 8 shows the routing of PEVs from depots (A) to the different destination of customers (B). In order to improve visual clarity, Fig. 8(a) has been dissected into Fig. 8(b), (c) and (d). The MDVRP for Mumbai has been solved by IBM CIPLEX optimizer with the reduced relocation cost of 259.54 $/Hr, 0.30 seconds iteration time.

In Fig. 8, the depots are mentioned by A and customer locations are mentioned by B. The total number of depots is 31. In the city under consideration, there are 29 locations where the

## VI. CONCLUSION

This paper proposes a novel approach for MDVRP based PEVs relocation in a sharing system. For the first time,

Authorized licensed use limited to: Technische Universitaet Muenchen. Downloaded on March 30,2025 at 13:01:36 UTC from IEEE Xplore.  Restrictions apply.

TABLE IV LOCATION OF DEPOTS &amp; CUSTOMERS IN MUMBAI, INDIA

machine learning-based PEVs SOC prediction is utilized in PEVsrelocation strategy in a sharing method. The Adaboost and XGBoost are used to predict PEVs SOC once they reach near the customers. The PEVs are assumed to be traveled through a particular distance before reaching the customers. The prediction of PEVs SOC considering travel time and travel distance using Adaboost and XGBoost algorithms has been found in this work. The predicted value of PEVs SOC has been stored in the management platform, which helps the system relocate the PEVs to the customers. For the first time, the concept of PEVs relocation in a sharing system has utilized the forecasted value of PEVs SOC using machine learning approaches. The results of PEVs SOC prediction are compared and analyzed. It is found that the XGBoost algorithm works better than the Adaboost algorithm. The relocation strategy considers the predicted SOC value that has been generated by the XGBoost algorithm. The relocation cost of MDVRP has been found by using IBM CIPLEX optimizer, which shows the least cost value for all the case studies. This method has been shown to be cost-effective, as it enables prior planning of charging scheduling and routing. The proposed scheme can serve as a valuable tool for transportation companies to optimize their PEV management practices in the sustainability of their transportation operations. Moreover, the study also further applied on Mumbai city, India where the PEVs routing problem has also been demonstrated with practical dataset. The effectiveness of the relocation-based sharing system of MDVRP considering machine learning-based SOC prediction of PEVs has been successfully validated and demonstrated in this work.

## REFERENCES

- [1] J. G. J. Olivier and J. A. H. W. Peters, 'Trends in global CO2 and total greenhouse gas emissions: 2019 report,' PBL Netherlands Environ. Assessment Agency, The Hague, The Netherlands, Tech. Rep. PBL 4068, 2020.
- [2] H. Van Essen et al., Handbook On the External Costs of Transport, Version 2019 . Brussels, Belgium: European Commission, Jan. 2019.
- [3] J. Lingli, 'Smart city, smart transportation: Recommendations of the logistics platform construction,' in Proc. IEEE Int. Conf. Intell. Transp., Big Data Smart City , 2015, pp. 729-732.
- [4] P. Jittrapirom, V . Caiati, A.-M. Feneri, S. Ebrahimigharehbaghi, M. J. A. GonzÆlez, and J. Narayan, 'Mobility as a service: A critical review of definitions, assessments of schemes, and key challenges,' Urban Plan. , vol. 2, no. 2, pp. 13-25, Jun. 2017.
- [5] M. Clemente, M. P. Fanti, G. Iacobellis, M. Nolich, and W. Ukovich, 'A decision support system for user-based vehicle relocation in car sharing systems,' IEEE Trans. Syst., Man, Cybern.: Syst. , vol. 48, no. 8, pp. 1283-1296, Aug. 2018.
- [6] M. P. Fanti, A. M. Mangini, G. Pedroncelli, and W. Ukovich, 'Fleet sizing for electric car sharing systems in discrete event system frameworks,' IEEE Trans. Syst., Man, Cybern.: Syst. , vol. 50, no. 3, pp. 1161-1177, Mar. 2020.

- [7] X. Huo, X. Wu, M. Li, N. Zheng, and G. Yu, 'The allocation problem of electric car-sharing system: A data-driven approach,' Transp. Res. D, Transp. Environ. , vol. 78, pp. 1-21, Jan. 2020.
- [8] B. Boyaci, K. G. Zografos, and N. Geroliminis, 'An optimization framework for the development of efficient one-way car-sharing systems,' Eur. J. Oper. Res. , vol. 240, no. 3, pp. 718-733, Feb. 2015.
- [9] M. Bruglieri, A. Colorni, and A. LuŁ, 'The vehicle relocation problem for the one-way electric vehicle sharing: An application to the Milan case,' Procedia-Social Behav. Sci. , vol. 111, pp. 18-27, Feb. 2014.
- [10] S. Weikl and K. Bogenberger, 'Relocation strategies and algorithms for free-floating car sharing systems,' IEEE Intell. Transp. Syst. Mag. , vol. 5, no. 4, pp. 100-111, Winter 2013.
- [11] T. Raviv, M. Tzur, and I. A. Forma, 'Static repositioning in a bike-sharing system: Models and solution approaches,' EURO J. Transp. Logistics , vol. 2, no. 3, pp. 187-229, Aug. 2013.
- [12] M. Bruglieri, F. Pezzella, and O. Pisacane, 'Heuristic algorithms for the operator-based relocation problem in one-way electric carsharing systems,' Discrete Optim. , vol. 23, pp. 56-80, Feb. 2017.
- [13] B. Boyacı, K. G. Zografos, and N. Geroliminis, 'An integrated optimization-simulation framework for vehicle and personnel relocations of electric carsharing systems with reservations,' Transp. Res. B, Methodol. , vol. 95, pp. 214-237, Jan. 2017.
- [14] A. Ait-Ouahmed, D. Josselin, and F. Zhou, 'Relocation optimization of electric cars in one-way car-sharing systems: Modeling, exact solving and heuristics algorithms,' Int. J. Geographical Inf. Sci. , vol. 32, no. 2, pp. 367-398, Sep. 2017.
- [15] M. Bruglieri, F. Pezzella, and O. Pisacane, 'An adaptive large neighborhood search for relocating vehicles in electric carsharing services,' Discrete Appl. Math. , vol. 253, pp. 185-200, Jan. 2019.
- [16] M. Repoux, M. Kaspi, B. Boyacı, and N. Geroliminis, 'Dynamic prediction-based relocation policies in one-way station-based carsharing systems with complete journey reservations,' Transp. Res. B, Methodol. , vol. 130, pp. 82-104, Dec. 2019.
- [17] B. Boyacı and K. G. Zografos, 'Investigating the effect of temporal and spatial flexibility on the performance of one-way electric carsharing systems,' Transp. Res. B, Methodol. , vol. 129, pp. 244-272, Nov. 2019.
- [18] S. Reiss and K. Bogenberger, 'GPS-data analysis of Munich's free-floating bike sharing system and application of an operator-based relocation strategy,' in Proc. IEEE 18th Int. Conf. Intell. Transp. Syst. , 2015, pp. 584-589.
- [19] L. Caggiani, R. Camporeale, M. Ottomanelli, and W. Y. Szeto, 'A modeling framework for the dynamic management of free-floating bike-sharing systems,' Transp. Res. Part C: Emerg. Technol. , vol. 87, pp. 159-182, Feb. 2018.
- [20] M. Balac, H. Becker, F. Ciari, and K. W. Axhausen, 'Modeling competing free-floating carsharing operators-A case study for Zurich, Switzerland,' Transp. Res. Part C: Emerg. Technol. , vol. 98, pp. 101-117, Jan. 2019.
- [21] D. Kypriadis, G. Pantziou, C. Konstantopoulos, and D. Gavalas, 'Optimizing relocation cost in free-floating car-sharing systems,' IEEE Trans. Intell. Transp. Syst. , vol. 21, no. 9, pp. 4017-4030, Sep. 2020.
- [22] M. Bruglieri, F. Pezzella, and O. Pisacane, 'A two-phase optimization method for a multiobjective vehicle relocation problem in electric carsharing systems,' J. Combinatorial Optim. , vol. 36, no. 1, pp. 162-193, Apr. 2018.
- [23] Q. Li, F. Liao, H. J. P. Timmermans, H. Huang, and J. Zhou, 'Incorporating free-floating car-sharing into an activity-based dynamic user equilibrium model: A demand-side model,' Transp. Res. B, Methodol. , vol. 107, pp. 102-123, Jan. 2018.
- [24] S. Weikl and K. Bogenberger, 'A practice-ready relocation model for freefloating carsharing systems with electric vehicles-Mesoscopic approach and field trial results,' Transp. Res. Part C: Emerg. Technol. , vol. 57, pp. 206-223, Aug. 2015.
- [25] C. Willing, K. Klemmer, T. Brandt, and D. Neumann, 'Moving in time and space-Location intelligence for carsharing decision support,' Decis. Support Syst. , vol. 99, pp. 75-85, Jul. 2017.
- [26] A. B. Brendel, J. T. Brennecke, P. Zapadka, and L. M. Kolbe, 'A decision support system for computation of carsharing pricing areas and its influence on vehicle distribution,' in Proc. 38th Int. Conf. Inf. Syst. , 2017, pp. 1-21.
- [27] A. Di Febbraro, N. Sacco, and M. Saeednia, 'One-way car-sharing profit maximization by means of user-based vehicle relocation,' IEEE Trans. Intell. Transp. Syst. , vol. 20, no. 2, pp. 628-641, Feb. 2019.
- [28] X. Zhu, J. Li, Z. Liu, and F. Yang, 'Location deployment of depots and resource relocation for connected car-sharing systems through mobile edge computing,' Int. J. Distrib. Sensor Netw. , vol. 13, Jun. 2017, Art. no. 1550147717711621.
- [29] J. Kopp, R. Gerike, and K. W Axhausen, 'Do sharing people behave differently? An empirical evaluation of the distinctive mobility patterns of free-floating car-sharing members,' Transportation , vol. 42, no. 3, pp. 449-469, 2015.
- [30] H. Becker, F. Ciari, and K. W. Axhausen, 'Comparing car-sharing schemes in Switzerland: User groups and usage patterns,' Transp. Res. Part A: Policy Pract. , vol. 97, pp. 17-29, 2017.
- [31] S. Schmöller et al., 'Empirical analysis of free-floating carsharing usage: The Munich and Berlin case,' Transport. Res. C: Emerg. Technol. , vol. 56, pp. 34-51, 2015.
- [32] X. Lu, Q. Zhang, Z. Peng, Z. Shao, H. Song, and W. Wang, 'Charging and relocating optimization for electric vehicle car-sharing: An event-based strategy improvement approach,' Energy , vol. 207, 2020, Art. no. 118285.
- [33] M. P. Fanti, A. M. Mangini, M. Roccotelli, and B. Silvestri, 'Innovative approaches for electric vehicles relocation in sharing systems,' IEEE Trans. Automat. Sci. Eng. , vol. 19, no. 1, pp. 21-36, Jan. 2022, doi: 10.1109/TASE.2021.3103808.
- [34] S. Deb et al., 'Charging coordination of plug-in-electric vehicle for congestion management in distribution system integrated with renewable energy sources,' IEEE Trans. Ind. Appl. , vol. 56, no. 5, pp. 5452-5462, Sep./Oct. 2020.
- [35] S. Deb, A. K. Goswami, R. L. Chetri, and R. Roy, 'Prediction of plugin electric vehicle's state-of-charge using gradient boosting method and random forest method,' in Proc. IEEE Int. Conf. Power Electron., Drives Energy Syst. , 2020, pp. 1-6.
- [36] L. Wang, Y. Guo, M. Fan, and X. Li, 'Wind speed prediction using measurements from neighbouring locations and combining the extreme learning machine and the AdaBoost algorithm,' Energy Rep. , vol. 8, pp. 1508-1518, 2022.
- [37] R. Shi, X. Xu, J. Li, and Y. Li, 'Prediction and analysis of train arrival delay based on XGBoost and Bayesian optimization,' Appl. Soft Comput. , vol. 109, 2021, Art. no. 107538.
- [38] Dhritiman, 'Guesstimate /estimate - Number of taxis / OLA / UBE operating in Mumbai in a day: The obvious strategies, guesstimates, estimation, &amp; strategies' 2020, Accessed: Nov. 1, 2022.[Online]. Available: https://theobviousstrategies.wordpress.com/2016/02/07/ estimate-number-of-taxis-in-mumbai/
- [39] H. Ettazi, N. Rafalia, and J. Abouchabaka, 'Metaheuristics methods for the VRP in Home Health Care by minimizing fuel consumption for environmental gain,' in Proc. E3S Web Conferences , 2020, Art. no. 00094.
- [40] E. Haitam, R. Najat, and J. Abouchabaka, 'GRASP combined with ILS for the vehicle routing problem with time windows, precedence, synchronization and lunch break constraints,' Int. J. Adv. Comput. Sci. Appl. , vol. 12, no. 5, pp. 169-177, 2021.
- [41] E. Haitam, R. Najat, and J. Abouchabaka, 'A vehicle routing problem for the collection of medical samples at home: Case study of Morocco,' Int. J. Adv. Comput. Sci. Appl. , vol. 12, no. 4, pp. 345-351, 2021.
- [42] E. Haitam, R. Najat, and J. Abouchabaka, 'A survey of the vehicle routing problem In-home health care services,' Proc. Eng. Sci. , vol. 3, no. 4, pp. 391-404, 2021.