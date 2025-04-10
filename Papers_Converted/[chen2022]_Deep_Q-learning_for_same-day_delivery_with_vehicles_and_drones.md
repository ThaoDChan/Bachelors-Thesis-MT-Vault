## DEEP Q-LEARNING FOR SAME-DAY DELIVERY WITH VEHICLES AND DRONES

## A PREPRINT

## Xinwei Chen

Applied Mathematical and Computational Sciences University of Iowa Iowa City, United States xinwei-chen-1@uiowa.edu

## Marlin W. Ulmer

Technische Universität Braunschweig Carl-Friedrich-Gauß-Fakultät Braunschweig, Germany m.ulmer@tu-braunschweig.de

## Barrett W. Thomas

Department of Business Analytics University of Iowa Iowa City, United States barrett-thomas@uiowa.edu

March 9, 2021

## ABSTRACT

In this paper, we consider same-day delivery with vehicles and drones. Customers make delivery requests over the course of the day, and the dispatcher dynamically dispatches vehicles and drones to deliver the goods to customers before their delivery deadline. Vehicles can deliver multiple packages in one route but travel relatively slowly due to the urban traffic. Drones travel faster, but they have limited capacity and require charging or battery swaps. To exploit the different strengths of the fleets, we propose a deep Q-learning approach. Our method learns the value of assigning a new customer to either drones or vehicles as well as the option to not offer service at all. In a systematic computational analysis, we show the superiority of our policy compared to benchmark policies and the effectiveness of our deep Q-learning approach. We also show that our policy can maintain effectiveness when the fleet size changes moderately. Experiments on data drawn from varied spatial/temporal distributions demonstrate that our trained policies can cope with changes in the input data.

K eywords Dynamic Vehicle Routing · Same-day Delivery · Reinforcement Learning · Drones · Q-learning

## 1 Introduction

Same-day delivery (SDD) changes the way people shop as it combines immediate product availability and the convenience of ordering from electronic devices (Hausmann et al. 2014). Because of its attractive nature, SDD were already expected to reach 15% of last-mile delivery volumes in 2020 (Joerss et al. 2016), and retailers such as Amazon, Target, and Walmart were racing to expand their SDD options (Thomas 2019). With people seeking to limit social contact, the COVID-19 pandemic has accelerated demand (Peltz and Morgan 2020) causing even Amazon to struggle to fulfill orders (Bertoni et al. 2020).

Yet, even before the surge of demand brought on by the COVID-19 pandemic, providing SDD was a challenge. The timing of requests and the delivery locations are not known until a customer places an order. Further, because of the need to meet tight delivery deadlines, consolidation opportunities are scarce, rendering the use of conventional delivery vehicles (hereafter referred to as 'vehicles') inefficient, especially for delivery in less dense areas of a city. In addition, vehicles are slowed by congestion on urban streets.

As an alternative, companies have begun to complement vehicles with Unmanned Aerial Vehicles (hereafter referred to as 'drones'). In 2016, Amazon Prime Air made its first drone delivery in Cambridgeshire, England (Kim 2016). In May 2019, Alphabet Inc., Google's parent company, announced its subsidiary Wing would launch drone deliveries in Finland starting in June 2019 (Pero 2019). Wing has also been approved for commercial drone deliveries in the United States and Australia. In September 2019, Wing announced it would begin to test drone deliveries in Christiansburg, Virginia, teaming up with FedEx and Walgreens to provide deliveries of customer packages and over-the-counter medicines as well as food and beverages (Bhagat 2019). Because they provide an alternative way of delivery that supports social distancing for both customers and essential workers amid the COVID-19 pandemic, industry sees an emerging safety need for drones in last-mile delivery (Wolf 2020). In April 2020, UPS announced that it would dramatically expand their drone delivery program to deliver prescription medicines to the largest retirement community in the US (Shapiro 2020).

While drones offer some advantages over vehicles, they also have limitations. Existing drones can usually carry only one item at a time and require regular charging or battery swaps. As a result, drones may not entirely replace vehicles in last-mile delivery, especially when the volume of customer requests is high (Wang 2016). Further, recent studies show that there are benefits to combining fleets of vehicles and drones for SDD (Ulmer and Thomas 2018). However, the challenge arises as to how to effectively exploit the strengths of the individual vehicle types (e.g. capacity, speed) to offer service to as many customers as possible per day.

In this paper, we address this challenge for the same-day delivery problem with vehicles and drones (SDDPVD). We study a problem where, over the course of a day, vehicles and drones deliver goods from a depot to customers and then return to the depot for future dispatches. The vehicles and drones differ in their travel speeds, capacities, and the need for charging or battery swaps. Customers are unknown until they make requests, and each request is associated with a delivery deadline. For every request, the dispatcher must determine whether the request is accepted and, if so, whether a vehicle or drone will make the delivery. All accepted requests must be delivered within a certain period of time. The objective is to maximize the expected number of customers served.

This problem is complex because each decision impacts the availability of drones and vehicles to serve future requests, and because customers are waiting for a response, decisions need to be made instantaneously. To overcome these challenges, we propose a deep Q-learning approach. Q-learning is a form of reinforcement learning that seeks to learn the value of state-action pairs. Deep Q-learning uses deep neural networks as approximation architecture. Because the neural networks can be trained offline, the method can be employed for real-time decision making. We compare the proposed approach to high-quality benchmark policies from the literature. We also develop new methods for this paper that seek to improve the quality of the benchmarks from the literature by adding new features to them.

This research makes three important contributions to the literature:

- 1. We show that the proposed approach is an effective and fast approach to an important and emerging delivery problem. Our computational results demonstrate the proposed Q-learning approach provides higher-quality solutions than all of the benchmarks.
- 2. Our computational study demonstrates that the proposed solution method is robust to changes in the distributions on the timing and locations of customer requests as well as changes in the delivery fleet.
- 3. The paper is among the first papers to implement deep Q-learning techniques for SDD and for dynamic routing problems in general. Our work highlights the potential and challenges of reinforcement learning techniques in dynamic vehicle routing. Particularly, this paper shows the value of including features that reflect both resource utilization and the impact of action selection in the current state. The identification of these categories of features offers general guidance for feature selection in other dynamic routing problems.

The paper is organized as follows. In Section 2, we present the literature related to SDD and reinforcement learning for routing-related problems. Section 3 describes the problem and presents a sequential decision process model of the problem. In Section 4, we introduce the deep Q-learning solution approach for the SDDPVD and the selected features. Section 5 describes the details of instances, implementation, and the benchmark policies as well as presents the results of our computational study. Section 6 closes the paper with conclusions and discussion on future work.

## 2 Literature Review

In this section, we present the literature related to the SDDPVD. We first review the literature related to the SDD and then present the existing applications of reinforcement learning in vehicle routing problems (VRPs). For a general review of drone routing problems, we refer the reader to Otto et al. (2018). It is worth noting that, most of the papers cited by Otto et al. (2018) do not consider the dynamism of the SDDPVD. In contrast to the work in this paper, most of

the literature involving drone delivery assumes that the customers to be served are known a priori. Boysen et al. (2020) provide a review of the last-mile-delivery literature, including drone delivery, and the emerging methods for solving the challenge.

## 2.1 Same-day Delivery

The existing related literature on the SSD is limited, but is increasing as the service becomes more popular. The most closely related work is found in Ulmer and Thomas (2018). Similar to this paper, Ulmer and Thomas (2018) consider the SDD with a fleet of vehicles and drones. Their work is the first to consider a heterogeneous fleet in the context of SDD. It is also the first to investigate the impact of adding drones to a fleet of conventional vehicles for a dynamic routing problem. The authors introduce a parametric policy function approximation (PFA) approach to heuristically solve the assignment portion of the problem. They use a fixed travel time threshold to determine whether a customer should be served by a drone or vehicle.

The PFA policy presented in Ulmer and Thomas (2018) uses only the location of customers to determine whether to use a vehicle or drone to serve a given customer. Yet, there is considerably more available information that might help produce better decisions. The approach in this paper uses not only the location, but also other available information reflecting resources and demand. We demonstrate that this additional information greatly improves upon the performance of the PFA and other benchmarks. To facilitate the incorporation of this additional information, we turn to a different type of approximation, Q-learning-an approximation of the value of state-action pairs, which we implement using neural networks (NNs).

Also related is the work of Liu (2019) in which the author considers on-demand meal delivery using drones. In the paper, the dispatcher dynamically assigns drones of different capacities to pick up food from restaurants and deliver to customers. The author first introduces a mixed-integer-programming model to minimize the total lateness for the static version of the problem and then uses heuristics to solve the dynamic problem. The problem is similar to the SDDPVD in that they both consider the dynamism of customer orders and involve the routing with drones. However, in the dynamic case, Liu (2019) present a rolling-horizon approach, an approach that ignores the future when making current decisions. For the SDD, Voccia et al. (2019) shows that accounting for the future leads to considerably better performance than a rolling-horizon approach. Thus, our approach uses deep Q-learning to incorporate each decision's current value as well as its value on the future. Further, in addition to an assignment (routing) decision, the dispatcher in the SDDPVD must also make an acceptance decision for each customer request, which makes the action space in the problem even larger than that in the problem studied by Liu (2019). Finally, the SDDPVD requires the routing of vehicles, further complicating assignment decisions.

Grippa et al. (2016) also study a version of the SDD in which deliveries are made by a fleet of drones. The authors consider a system of drones that deliver goods from the depot to customers and model it as a queuing problem. Performance of the policies with different heuristics are evaluated computationally. Because of the drones' interaction with the vehicles and the possibility of consolidating multiple customers packages onto vehicles, the queueing approach proposed in Grippa et al. (2016) does not apply to the problem in this work.

Other literature in SDD presents anticipatory methods for single-vehicle problems. Klapp et al. (2016) consider a dynamic routing problem of a vehicle traveling on a line, where the probabilistic information is used to anticipate future requests. Klapp et al. (2018) use an a-priori-based rollout policy to determine the customers to serve and whether the vehicle should leave the depot for deliveries at the current time or wait for more customer orders coming in. Klapp et al. (2020) then consider making immediate acceptance decisions in SDD and use an a-priori-based rollout policy to minimize the sum of costs and penalties. Ulmer et al. (2019) consider a SDD problem in which the vehicle is allowed for preemptive returns to the depot, e.g., returning to the depot before completing the deliveries in the current route. The authors introduce an approximate dynamic programming (ADP) approach, where at each customer (or at the depot) a decision of whom the vehicle visits next is made using the information in the state. In contrast to Klapp et al. (2016, 2018, 2020) and Ulmer et al. (2019), this paper focuses on not only multiple vehicles but a fleet of both vehicles and drones. The methods proposed in Klapp et al. (2016, 2018, 2020) and Ulmer et al. (2019) do not scale to the problem discussed in this paper.

Azi et al. (2012) and Voccia et al. (2019) consider using multiple, but homogeneous vehicles in dynamic routing problems. These papers solve the problems using multiple-scenario approaches (MSAs). While effective, MSA requires real-time computation that can be challenging in the context of the large fleet and the many incoming requests that we consider in this paper. Our proposed method uses offline computation to learn the approximation and can provide instantaneous solutions in real-time. Dayarian and Savelsbergh (2020) consider crowdshipping in SDD. In addition to the store's own fleet, in-store customers can also make deliveries for online orders on their way home. The problem

is similar to ours in that it considers whether an order should be served by in-store customers or the store's own fleet. However, the authors propose two rolling horizon approaches while we introduce a deep Q-learning approach.

Dayarian et al. (2020) consider the SDD with drone resupply, where a vehicle performs the actual deliveries of goods to customers, and a drone resupplies goods from the depot to the vehicle en route. The use of the drone resupply enables the vehicle to serve a new set of customers without the need of returning to the depot to pick up the goods, and thus more customers can be served before their delivery deadlines. Ulmer and Streng (2019) consider the SDD with a fleet of autonomous vehicles, where the autonomous vehicles deliver the ordered goods from the depot to a set of pick-up stations. The authors introduce a PFA approach to minimize the expected sum of delivery times. The emerging business models studied in Dayarian et al. (2020) and Ulmer and Streng (2019) complement the work in this paper.

## 2.2 Reinforcement Learning for VRPs

In this section, we focus only on the literature that uses reinforcement learning for VRPs. We do not consider supervised learning approaches such as are discussed in Potvin et al. (1992), Vinyals et al. (2015), and Fagerlund (2018). Reinforcement learning (RL) is an area of machine learning that is often applied to sequential decision processes for problems in robotics, artificial intelligence, and signal control. Sutton and Barto (2018) provide a general overview. There are two common types of RL algorithms: policy-based methods and value-based methods. Policy-based learning algorithms seek to optimize a policy directly. Value-based learning algorithms learn the value of being in particular states or of particular state-action pairs. We use a deep Q-learning approach, an approach that seeks to learn the value of state-action pairs. Deep Q-learning network was introduced by Mnih et al. (2013) who demonstrate its ability to play Atari games with super-human performance.

There are a few papers presenting RL-work in dynamic vehicle routing, for example, Toriello et al. (2014), Rivera and Mes (2017), Ulmer et al. (2018), and Joe and Lau (2020). For a recent overview, we refer the reader to Ulmer and Thomas (2020). The work in the literature usually draws on the concept of value-function approximation (VFA). VFAs approximate the value of post-decision states by means of simulations. The values are stored in approximation architectures, usually either functions or lookup tables. The main difference between VFAs and Q-learning is that the latter considers the value of state-decision pairs versus just the value of a post-decision state. Even if post-decision states are a summary of the result of taking a particular action in a particular state, we show that, for the SDDPVD at least, Q-learning's ability to take both state- and action-space information into account has an advantage over relying on just information available in the post-decision state.

Work on using NNs in RL for VRPs is particularly scarce. Ma´ ndziuk (2018) provides an overview of recent advances in the VRP literature over the last few years, where the author points out that 'it came as a surprise to the author that NNs have practically not been utilized in the VRP domain . . . ."

Among the papers using RL with NNs applied to dynamic routing problems, the most closely related to this paper is Chen et al. (2019). Chen et al. (2019) introduce an actor-critic framework, a policy-based RL method, for the problem of making pick-ups at customers who make dynamic requests for service. The problem is similar to the SDDPVD in that they both consider dynamic requests and customer locations are unknown in advance. The papers differ in that Chen et al. (2019) learn a policy for a single vehicle and then apply this policy to all vehicles. As a result, the policy that is learned does not account for the interaction among the vehicles. Our results show that accounting for this interaction leads to superior solution quality. In addition, Chen et al. (2019) do not make decisions on whether or not to offer service to customers, but rather they allow unserved customer requests to expire.

In their appendix, Nazari et al. (2018) discuss a problem in which a single vehicle serves dynamic requests. Requests not served in a specified time period are lost. The vehicle has a limited amount of inventory and must return to the depot to replenish once the inventory is depleted. Like Chen et al. (2019), Nazari et al. (2018) propose a policy-gradient approach. The problem studied by Nazari et al. (2018) is different and has a much smaller state and action space than the problem studied in this paper. Likewise, Kool et al. (2018) use a policy-gradient approach to solve a stochastic prize collecting traveling salesman problem and also presents an application to a variant in which customer locations and demands can change. Again, given the different problems, the commonality between Kool et al. (2018) and this paper is the use of RL. Yet, the use of policy-gradient methods in Chen et al. (2019), Nazari et al. (2018), and Kool et al. (2018) suggests a future opportunity to explore the value of policy-gradient versus Q-learning approaches for dynamic vehicle routing.

## 3 Problem Definition

In this section, we present a formal definition of the SDDPVD, initially presented in Ulmer and Thomas (2018). We will first give a problem narrative and then model the problem as a sequential decision process.

## 3.1 Problem Narrative

In this problem, a fleet of vehicles and a fleet of drones deliver goods from a depot to customers requesting service over the course of the day. Vehicles and drones each have operating periods during the day, starting and ending their day at the depot. The operating periods are limited, for example, because of working-hour regulations or because the warehouse closes in the evening. Vehicles and drones differ in their characteristics, namely their capacity and their speed. Drones are faster but they can only carry one order at a time. Furthermore, drones require recharging after each trip. Vehicles are slower, but spatial capacity can be neglected (Guglielmo 2013). Both fleets require time for loading at the depot (independent of the number of parcels to load) and for drop off of parcels at each customer. For the vehicles, we assume preemptive depot returns are prohibited. Thus, the vehicles must finish one delivery tour and then start the next one.

Over the course of the day, customers request SDD service. The requests are unknown before they are revealed. Whenever a new customer requests delivery, the provider decides if the customer can be offered service, and if so, whether the delivery will be conducted by a vehicle or a drone. If no service is offered, the customer request is declined and leaves the system. If service is offered, it must be conducted before a deadline, a predefined time span after the request is placed. If the order is assigned to a vehicle, decisions are also made about how the planned routes of the vehicle fleet is adapted to integrate the new order. If the order is assigned to the drone fleet, the decision also indicates which drone will conduct the delivery. Due to different handling of parcels for drone and vehicle deliveries, we assume that the assignment to a fleet type is permanent. A decision is feasible if the planned routes ensure deliveries are made before their corresponding deadline, and vehicles and drones return to the depot before the end of their operating periods.

The objective of the SDDPVD is to maximize the expected number of deliveries per day.

## 3.2 Preparation of the Model

The problem presented in this paper is a sequential decision problem, and in modelling the problem, we follow the framework presented in Powell (2019). We also use route plans to model the updates and evolution of routes. Route plans are described in detail in Ulmer et al. (2020). Before we define the components of the sequential decision problem, we will provide some general notation as well as the concepts related to route plans. We will then give an example for a state and a decision.

## General Notation

We denote the fleet of m vehicles V = { v , v 1 2 , . . . , v m } and the fleet of n drones D = { d , d 1 2 , . . . , d n } . Delivery takes place in a service area A , with the depot N being part of the area. The set of customers in the system are denoted by C and individual customers by C , C , . . . 1 2 . The time of request for each customer C i is denoted by t ( C i ) .

The travel times between two points in the area are determined by function τ V ( · , · ) for vehicles and for a drone by τ D ( · , · ) . It takes t D N units of time to load a package onto a drone and t D C units of time for a drone to drop off a package at a customer. The charging time after each delivery trip for a drone is t D B . For vehicles, the loading time is denoted t V N and the drop off time at a customer is t V C . Vehicles must return to the depot before t V max and drones before t D max . Thus, the overall operation period is [0 , max { t D max , t V max } ] . We denote the delivery deadline of customer C i as δ C ( i ) . The delivery deadline per customer C i is ¯ δ units of time after the request is placed, and thus, δ C ( i ) = t ( C i ) + ¯ δ .

## Route Plans

Our model operates on route plans. A route plan represents the tentative routes vehicles and drones follow until the next decision update takes place (at the time of a new request). A planned route of a vehicle or drone contains information about the set of planned stops and the corresponding arrival times. For depot stops, it also contains the planned departure times of subsequent routes. Over the operation period, each decision leads to an update of the planned routes for vehicles and drones. We denote the set of planned routes for the vehicles by Θ V and the set of planned routes for the drones by Θ D . Then, the set of all planned routes Θ is denoted as Θ = (Θ V , Θ ) D .

The set of vehicle routes Θ V = ( θ v ( 1 ) , . . . , θ ( v m )) contains a route for each vehicle. A route is a sequence of stops with locations and information about the arrival (and departure) times. Because preemptive returns for vehicles are not allowed, the planned route for each vehicle contains information about only the end of its current delivery tour and the planned subsequent tour. We also note that the information per stop differs depending on whether it is a customer or a depot stop. Because waiting at the depot is possible, a planned route not only contains the planned time of arrival but

also the time of departure from the depot. Mathematically, a route for a vehicle v is represented by

<!-- formula-not-decoded -->

The first entry of a planned route θ v ( ) represents vehicle v 's next depot visit N θ 1 , the arrival time at the depot a N ( θ 1 ) , and the time s N ( θ 1 ) at which the vehicle starts to load packages for the assigned customers in the next tour. Therefore, the difference between a N ( θ 1 ) and s N ( θ 1 ) is the time the vehicle spends waiting at the depot before it starts its next delivery tour. Following the first depot visit is a sequence of customers C θ i , i = 1 2 , , . . . , h , that are assigned to vehicle v but not yet loaded. The customer stops are accompanied with arrival time information a C ( θ i ) for each customer C θ i . The last entry in a planned route represents the vehicle's return to the depot at time a N ( θ 2 ) , implying the vehicle will wait until t V max .

A planned route for a drone is slightly different from that for a vehicle in that a customer entry must be between two depot entries due to the capacity of drones. For a drone d , its planned route θ d ( ) is represented by

<!-- formula-not-decoded -->

Because there can be more than two depot entries in a drone's planned route, we use index -1 to represent the last entry, -2 the second to last and so on. The difference between a N ( θ 1 ) and s N ( θ 1 ) represents drone d 's charging time after the previous delivery and the time the drone spends on waiting at the depot before it starts to load the items for its next delivery tour. Finally, the waiting time after the last planned delivery is set to t D max .

## Illustrative Example

In Figure 1, we illustrate the SDDPVD for time t = 60 (in minutes), an hour after the shift begins. At this time, we assume that the dispatcher receives a new customer request C 6 . The two panels in the figure describe the corresponding pre -and post -decision states, which are formally described in Section 3.3. In this example, the depot is located in

Figure 1: Decision and decision state.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000000_31994c859059d37296216557a366923a1381bd98f2a41c7d3a954e0ffd802940.png)

the center of the area. The fleet consists of a vehicle and a drone. We assume the vehicle travels on a Manhattan-style grid, and the drone travels in the Euclidean plane. The vehicle needs 20 minutes to travel a segment. The drone travels in the Euclidean plane with twice the speed of the vehicle. All travel times are rounded up to the nearest integral minute. Customer orders must be completed within 240 minutes after acceptance. For both the vehicle and the drone, a loading time at the depot and a delivery time at a customer are 10 minutes. The charging time for the drone is 20 minutes.

The panel on the left shows the status of the vehicle and drone before the dispatcher makes the acceptance and assignment decisions regarding C 6 . The vehicle is currently en route serving C 1 and then C 2 . Its planned route is θ v ( ) = (( N, 220 220) ( , , C , 5 290) ( , N, 360 , t V max )) . It will arrive at C 1 at t = 60 + 40 = 100 , and then arrive at C 2 at t = 100 + 10 + 40 = 150 . The vehicle then will leave C 2 at t = 150 + 10 = 160 and return to the depot at t = 160 + 60 = 220 . Thus, the arrival time of the first depot entry in the planned route is 220. The vehicle is planned to serve C 5 in its next route. Customer C 5 is accepted but not yet loaded because the vehicle has not returned to the depot to load the package for it. The vehicle plans to arrive at C 5 at t = 220 + 10 + 60 = 290 and then return to the depot at t = 290 + 10 + 60 = 360 . The drone is currently en route serving C 3 , with the planned route θ d ( ) = (( N, 124 144) ( , , C , 4 191) ( , N, 238 , t D max )) . The drone will arrive at C 3 at t = 40 + 10 √ 13 ≈ 77 , and return to the depot at t = 77 + 10 + 10 √ 13 ≈ 124 . Due to the charging time, the drone will not load the new package until t = 124 + 20 = 144 . It is then planned to leave the depot for C 4 and arrive at C 4 at t = 144 + 10 + 10 √ 13 ≈ 191 . The drone will return to the depot at t = 191 + 10 + 10 √ 13 ≈ 238 .

As the new customer C 6 makes a request, the dispatcher determines the feasibility of assigning the new customer to the vehicle and the drone. The delivery deadline of C 6 is 60 + 240 = 300 . If the vehicle serves C 6 right after C 5 , then it will arrive at C 6 at t = 290 + 10 + 40 = 340 , which is not feasible because it passes the deadline of C 6 . Similarly, it is not feasible either to serve C 6 first and then C 5 . Overall, it is not feasible to serve C 6 by the vehicle. Applying the same analysis yields that it is feasible for the drone to serve C 4 , return to the depot, and then serve C 6 . Thus, it is feasible to serve C 6 with the drone because there exists a feasible route.

The dispatcher next makes the decision. Let us assume the dispatcher accepts the request C 6 and assigns it to the drone. Then, the update is shown in the panel on the right in Figure 1. The vehicle's planned route remains the same, and the drone's planned route becomes

<!-- formula-not-decoded -->

## 3.3 Sequential Decision Process Model

In this section, we model the SDDPVD as a sequential decision process, a sequence of states connected by actions and transitions. Due to similarities in the problems, this model is similar to that found in Ulmer and Thomas (2018).

## Decision Point

A decision point is a time at which a decision is made. In the SDDPVD, a decision point occurs when a customer requests service. We denote the k th requesting customer as C k and the time of the k th decision point as t k = ( t C k ) .

## State

The state at a decision point summarizes the information needed to make the decision. In the SDDPVD, the state S k at decision point t k includes the information about the customers, the vehicles and drones as well as the planned routes. Because the planned routes include the relevant information for the fleets, the fleet information can be omitted from the state information. Mathematically, a state has four components:

- · t k : Time of the decision point, t k = ( t C k ) .
- · C k : New request.
- · C k = ( C θ V ,k , C θ D ,k ) : Set of customers still to be loaded and served as well as their deadlines. The set is split between customers currently planned for service with vehicles C θ V ,k and with drones C θ D ,k .
- · Θ k = (Θ V ,k , Θ D ,k ) : Set of planned routes for vehicles Θ V ,k and drones Θ D ,k when the request C k is received.

In summary, we represent the state S k as a tuple S k = ( t k , C k , C k , Θ ) k . Note, in the initial state t = 0 , no customer requests are pre-assigned, so all planned routes contain only the depot stop. In this case, N θ 1 represents a vehicle's (or drone's) initial position at the depot, and a N ( θ 1 ) is 0 because every vehicle (drone) is available once the shift begins.

## Action and Reward

At every decision point, we determine an action x k . In the following, we present the full action space of the SDDPVD, noting that our solution methods will use heuristic measures to reduce the action space.

In the SDDPVD, an action incorporates whether the request is accepted and, if so, which vehicle or drone will provide the service. We represent the action at a decision point t k as a tuple x k = ( α , k Θ x V ,k , Θ x D ,k ) , where α k is the acceptance and assignment decision, and Θ x V ,k ( Θ x D ,k ) is the updated set of vehicle (drone) planned routes. In addition, C x k = ( C θ,x V ,k , C θ,x D ,k ) represents the updated set of customers planned to be serviced by vehicles C θ,x V ,k and drones C θ,x D ,k but not yet loaded.

The first part of the action is the acceptance and assignment decision α k , which is defined as:

<!-- formula-not-decoded -->

The second and third parts of an action x k are the corresponding updates of the route plans. An update Θ x V ,k for the vehicle fleet is feasible if it satisfies the following six conditions:

- 1. The route plan in Θ x V ,k contains all the customers in C θ V ,k . It also contains C k , in the case α k = 1 .
- 2. For every customer C ∈ C θ,x V ,k , the planned arrival time is not later than the deadline, a C ( ) ≤ δ C ( ) .
- 3. In each planned route θ v ( ) ∈ Θ x V ,k , the start of loading at the depot s N ( θ 1 ) is not earlier than the arrival time a N ( θ 1 ) .
- 4. In each planned route, the difference between the beginning of loading for the next tour s N ( θ 1 ) at the depot and the arrival time at the next customer is the sum of travel time and loading time.
- 5. In each planned route, the difference between the arrival times of two consecutive customers is equal to the sum of travel time and service time.
- 6. The vehicles must return to the depot not later than the end of the shift, a N ( θ 2 ) ≤ t V max .

Similarly, a drone update Θ x D ,k is feasible if it satisfies the following five conditions:

- 1. The route plan in Θ x D ,k contains all the customers in C θ D ,k . It also contains C k , in the case α k = 2 .
- 2. For every customer C ∈ C θ,x D ,k , the planned arrival time is not later than the deadline, a C ( ) ≤ δ C ( ) .
- 3. In each planned route θ d ( ) ∈ Θ x D ,k , the start of loading at the depot s N ( θ 1 ) is not earlier than the arrival time a N ( θ 1 ) plus the charging time t D B .
- 4. In each planned route, every customer entry must be in between of two depot visits.
- 5. In each planned route, the arrival time of the last depot visit is not later than the end of the shift, a N ( θ -1 ) ≤ t D max

We note that the action x k = (0 Θ , V ,k , Θ D ,k ) is always feasible. The reward of an action x k is

<!-- formula-not-decoded -->

## Post-Decision State

The combination of a state S k and an action x k leads to a post-decision state S x k . The post-decision state contains the following information:

- · t k : Point of time.
- · C x k = ( C θ,x V ,k , C θ,x D ,k ) : Set of customers still to be loaded and served as well as their deadlines. The sets C θ,x V ,k and C θ,x D ,k are updated dependent on α k .
- · Θ x k = (Θ x V ,k , Θ x D ,k ) : Updated set of planned routes for vehicles and drones.

## Stochastic Information and Transition

Transitions occur as a result of the realization of random customer requests. These requests are exogenous to the decision-making process. The realized information ω k +1 either reveals a new request and the time of the request ω k +1 = { C k +1 , t ( C k +1 ) } or leads to termination of the process because the operation periods ended, ω k +1 = {} . In the latter case, the process terminates in the final state, S k +1 = S K .

Otherwise, the new state S k +1 is a combination of post-decision state S x k and stochastic information ω k +1 with the following updates:

- · The time of the decision point t k +1 is t ( C k +1 ) , the time of the new request C k +1 .
- · The vehicles and drones may have started new delivery tours in the time span between t k and t k +1 . That is, there exists some s N ( θ 1 ) &lt; t k +1 , indicating a set of customers were loaded before t k +1 . We denote the set of such customers by C x k,k +1 . These customers are no longer part of the state, and therefore, C k +1 = C x k \C x k,k +1 . The C θ,x V ,k and C θ,x D ,k are also updated in the same way.
- · Similarly, the routes Θ k +1 are updated by removing all delivery trips that already started before t k +1 .

## Objective Function

A solution to the SDDPVD is a policy π ∈ Π that assigns an action to each state. The optimal solution is a policy π ∗ that maximizes the total expected reward and can be expressed by

<!-- formula-not-decoded -->

where X π k ( S k ) is the action selected by policy π at state S k .

## 4 Solution Approach

In this section, we present our solution approach. It is well known that the solution to an instance of a sequential decision process model can theoretically be found using backward induction applied to the Bellman equation:

<!-- formula-not-decoded -->

where X S ( k ) is the set of actions available at state S k .

As with many other dynamic routing problems, however, the problem studied in this paper incurs the 'curses of dimensionality." For one, the state space of the SDDPVD is too large to even enumerate for realistically-sized problem instances. In addition, we also encounter a very large action space. Notably, the dispatcher must not only determine whether to offer service, but also to which fleet to assign accepted customers and how to route any customers assigned to vehicles. Thus, we propose a heuristic. In the following, we give a conceptual overview and an outline of our heuristic. We then describe the components in detail.

## 4.1 Motivation

For the SDDPVD, decisions not only need to be effective but fast. They should be effective by accounting for immediate and expected future revenues. Meanwhile, they should be made fast as well because customers expect immediate feedback about their service requests. To satisfy both requirements, we draw on methods of reinforcement learning (Sutton and Barto 2018). The idea is to learn the value of a state and action by means of simulation. This simulation is done 'offline' in a learning phase. The learned values can then be accessed within the 'online' execution without any additional calculation time required. Because the size of the action space would prohibit even offline learning, we also draw on a runtime-efficient routing heuristic, reducing the action space to ˆ X . In ˆ X , actions are concerned with whether offering service to a customer and, if so, what fleet providing the service. The routing and assignment heuristic determines which individual vehicle or drone to make the delivery.

Learning values is challenging because of the enormous sizes of state and action spaces. For the SDDPVD, multiple vehicles and drones are routed to serve many customers. Storing the value for every potential state and action combination not only leads to substantial memory consumption, it also makes frequent value observations and therefore learning impossible. Thus, in addition to the action space, we also reduce the state space to a set of selected features and approximate the value for each feature vector by means of deep Q-learning.

Figure 2: Conceptual Process of Decision Making.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000001_2322c12398576fed2f15de8372ff9757afd84f1aa96d56650402a6a59f253299.png)

Figure 2 presents a flow chart of the process. In a pre-decision state, we first check feasibility for vehicles and drones by means of the routing heuristic. If serving the customer is infeasible, no service is offered. Otherwise, dependent on the drone and vehicle feasibility, we extract state features and evaluate the state and corresponding routing decision provided by the heuristic. Based on the evaluation, we assign the customer to a drone or vehicle, or we do not offer service to the customer at all.

Two main challenges arise from the process: what features to extract and how to evaluate the value of a specific state-decision pair. For the first, we will present a selection of features in Section 4.4. For the latter, we draw on Q-learning as discussed in the following. We denote our policy π Q .

## 4.2 Deep Q-learning

In this section, we introduce the deep Q-learning (DQL) solution approach and structure of the NNs used for the SDDPVD. Q-learning is a reinforcement learning approach that learns the value of taking an action in a given state. Thus, Q-learning learns a value Q S ,x ( k ) for each state S k and action x ∈ X S ( k ) , and this value is an approximation of the expected future reward when making action x in state S k . Given that we operate on the restricted action space ˆ X , we learn a value for each state S k and action x ∈ ˆ ( X S k ) . With these Q-values and the reduced action space, we can solve an approximation of Equation 1 written as

<!-- formula-not-decoded -->

Depending on the existence of feasible route plans for vehicles and drones, we have a potential action set for each of the three cases when the decision is not trivially No Service (see Figure 2). When both vehicles and drones can feasibly provide the service, the set contains three actions Drone Vehicle , and No Service . When only drones or only vehicles can feasibly do so, the set contains two actions Drone/Vehicle and No Service . For each of the potential sets, we create a NN to approximate the value of the corresponding decisions. This set of NNs are denoted as Φ = { φ , j j = 1 2 3 , , } , where each φ j represents the set of weights or parameters in the corresponding NN. They are indexed in the same order as the cases are presented in Figure 2 from top to bottom. In the following, we present the structure of the NNs and the procedure to train them.

## Setup

A NN is characterized by its input layer, hidden layers, and output layer. In our approach, each of the three NNs has the same structure:

- · Input Layer. The input layer receives the extracted features and passes them to the hidden layers. Each NN uses the same features, and we present the features in Section 4.4.
- · Hidden Layers. Each NN has n hidden hidden layers and n nodes nodes in each hidden layer. We use the rectified linear unit function (ReLU) as the activation function for each hidden layer.
- · Output Layer. The output layer of the NNs in the SDDPVD outputs the Q-values for each possible action ˆ ( X S k ) for a state S k . Specifically, the possible actions are ( Drone Vehicle , , No Service ) for φ 1 , ( Drone No , Service ) for φ 2 , and ( Vehicle , No Service ) for φ 3 . Because the NNs approximate real-valued future rewards, there is no activation function on the output layer.

To determine the best model for the NNs, we test different numbers of hidden layers for each NN, n hidden ∈ { 2 5 10 , , } , and different numbers of hidden nodes, n nodes ∈ { 20 50 10 , , · |V|} with |V| the number of vehicles for each layer. The computational results show the combination of n hidden = 2 and n nodes = 10 · |V| outperforms the others.

## Training

We learn the parameters of the NNs in a training phase. The training set consists of 500 instances. An instance is a set of customer requests made during a day, including their request times and locations. Training is performed by sampling an instance with replacement from the training set and then simulating on the sampled instance. We refer to each simulated day as a sample path. During training, we usually select the action that maximizes the total expected reward associated with a state. This is known as exploitation. However, it is well known that occasionally randomly selecting an action improves the quality of the approximation (Powell 2011, Ch. 12). These random selections are known as exploration. We set the probability of exploring glyph[epsilon1] and exploiting 1 -glyph[epsilon1] , where glyph[epsilon1] decays from 1 to 0 01 . over the training steps. Each sample path is a training step of the NNs. Thus, we update the NNs after each sample path with a batch of the new observations and previous observations, a practice known experience replay (Lin 1993). The goal of experience replay

is to overcome the correlations between successive states in a sample path and the similarities between different paths. We use the mean-squared error as the loss function and minimize it using the well known Adam optimizer (Kingma and Ba 2014), a stochastic gradient-based algorithm. As a result of experimentation, for weight updates, we use a learning rate that exponentially decays from 0 01 . with the base 0 96 . and the decay rate 1 / 6000 .

## 4.3 Routing and Assignments

The action space is very large because an action determines an update of the routing plan. To overcome the large action space, we apply the assignment and routing heuristic from Ulmer and Thomas (2018) (where the algorithmic details can be found).

The heuristic maintains the plans for drones and vehicles in the case the customer is not assigned to the corresponding fleet type. Otherwise, it changes the plans for drones and vehicles as follows.

For drones, the heuristic assigns customers in a first-in-first-out (FIFO) manner. This procedure prioritizes drones idling at the depot and assigns a new customer to an idling drone prior to one en route. If all the drones are en route, we assign a new customer to the drone with the earliest availability. We arbitrarily choose a drone if there is a tie. It is not feasible to service a customer by drone if no drones can deliver the goods by the customer's delivery deadline.

For vehicles, the heuristic first checks if there is a vehicle currently idling at the depot. In that case, it assigns the new customer to this vehicle. If no vehicles are idling at the depot, the heuristic iterates through all vehicles to find the route with the cheapest feasible insertion position. An insertion (update) is feasible if it satisfies the conditions described in Section 3.3. For every feasible insertion, we then calculate the increase in the tour time of the vehicle to which the new customer is inserted, called insertion cost. We assign the new customer to the vehicle with the insertion that minimizes the cost over all feasible insertions. We denote this insertion cost by ∆ Vehicle . If there is no feasible insertions for any vehicle, then it is not feasible to serve the new customer by the fleet of vehicles. Based on the observations in Ulmer and Thomas (2018), we omit waiting at the depot and start the next tour right after arriving back from the previous one.

We note that the heuristic provides only one possible vehicle and one possible drone assignment to the DQL-approach. This procedure has the advantage that the action space is limited and the learning process is likely to be fast. However, such a limited action space may ignore more effective assignments. We explore this question in Appendix A.5.

## 4.4 Features

In this section, we present the features in the DQL for the SDDPVD. In Appendix A.1, we present the analytical results that motivate the choice of features by examining the functional form of the Bellman equation as well as several situations in which we can characterize optimal action selection. Using the analysis in Appendix A.1, we can identify information that needs to be extracted from the state that is to be input into the NNs. The features comprise information about the time, the customers, and the fleet. Our DQL approach uses all the features presented below. Extracted features are normalized using min-max normalization before being input to the NNs.

Time. Given a state S k , the first feature is the time of the decision point t k . Proposition A.1 shows that the value function is monotonically decreasing in time, and the point of time of a decision point should help determine the Q-value. Computational results presented in papers such as Ulmer (2017) have also demonstrated the value of the point in time for SDD problems.

Fleet. To reflect the availability of delivery resources in the state S k at a decision point, we include as features the time at which the vehicles and drones return to the depot from their ongoing routes. As shown in Propositions A.2 and A.3, we know that the value function is monotonically decreasing in these values. The available time for vehicle v is a N ( θ 2 ) indicated in its planned route as described in Section 3.2. Similarly, for drone d , its available time is a N ( θ -1 ) .

Actions. We also include two features that capture the resource consumption that would occur if the customer request is accepted. These features also incorporate information about the requesting customer. The first feature is the point-topoint distance d C ( k ) between C k and the depot N . Proposition A.5 shows that this distance plays an important role in determining whether or not to assign a customer to a drone. However, the feature is not suitable for making decisions concerning the assignments to vehicles as they do not travel in a point-to-point fashion. To this end, we also consider a second feature ∆ Vehicle , the insertion time required when assigning the customer to a vehicle.

## 5 Computational Study

In this section, we present the computational study for the SDDPVD. We first present the settings for our computations and describe the benchmark policies. We then analyze solution quality, the impact of feature selection, and performance

with distributional data change. We refer the reader to Appendix A for detailed illustrations of decision-making process, and comparisons of our proposed approach to its extension or some other method in the literature. The results presented in these sections are computed on a test set of 1000 instances that are different from the training set, but follow the same distributions.

## 5.1 Computational Settings

The settings assume delivery requests are made from 8 am to 3 pm and that vehicle drivers work eight hours from 8 am to 4 pm thus t V max = 8 hours. The drones are available from 8 am to 8 pm thus t D max = 12 hours. We assume that accepted requests must be serviced within ¯ = 4 δ hours. The loading and service times for both vehicle and drones are each 3 minutes ( t V N = t D N = t V C = t D C = 3 minutes). The battery charging time required for drones is set to t D B = 20 minutes, which is a conservative estimate based on the discussion in Grippa et al. (2016).

The vehicles travel at a speed of 30 km/h. We assume vehicles travel on a road network. To reflect the effect of road distances and traffic, we transform Euclidean distances using the method introduced by Boscoe et al. (2012). The method transforms Euclidean into an approximate street-network distance by multiplying the Euclidean distance between two points by 1 5 . . We compute the travel time based on these transformed distances.

We assume drones travel in a point-to-point fashion at a speed of 40 km/h. This speed is a conservative estimate of drone capability accounting for security measures within the city (Pero 2019). As described in Section 3.1, we assume drones are capable of carrying any package in the SDDPVD.

In our experiments, we consider three different sets of customer requests that differ in spatial and temporal distributions.

- · Homogeneous Customer Demand. Both the temporal and spatial distributions of customer requests are homogeneous for this set. For time of requests, we assume customers make delivery requests according to a homogeneous Poisson process with 500 requests expected in each day. For customer locations, the x and y coordinates of customer locations are generated from independent and identical normal distributions with the depot at the center. This set of instances is the one used in Ulmer and Thomas (2018). This geography reflects the structure of many cities in Europe where most customers are in the central area and the rest are sparsely located in the suburb. We set the standard deviation to 3 0 . km for each coordinate resulting in 50 % of the customers being in a core that is within 10-minute vehicle travel time from the depot (approximately 3 3 . km) and about 99 9 . %of the customers being within 30 minutes (approximately 10 km). This distance is within the travel capability of existing drones, which can carry payloads weighing up to 5 pounds and travel up to 12 5 . miles ( 20 km) (King 2019).
- · Spatially Heterogeneous Customer Demand. In this set, the temporal distribution of requests is the same as that for homogeneous customer demand. The spatial distribution is heterogeneous over time. This geography is motivated by the idea that, in the beginning of the day, customers often order to their homes, in the middle of the day, more customers order to work, and later, more customers order to their homes again. Thus, we vary the standard deviation of the customer coordinates. In the first and last two hours, it is 3 0 . km as before. For the three hours in between, it is reduced to 1 0 . km.
- · Temporally Heterogeneous Customer Demand. The spatial distribution of this set is the same as that for the homogeneous customer demand. For the temporal distribution, we consider a heterogeneous rate that mimics peak and off-peak hours of requests in real world. The arrivals still follow a Poisson process. However, we vary the arrival rate of requests over time in a way that they are normally distributed in two peaks around times t = 90 and t = 300 with standard deviations of 30 minutes.

For each of the three sets of requests, we test nine different combinations of fleet sizes. We consider combinations of 2 , 3 , and 4 vehicles with 5 , 10 , and 15 drones.

## 5.2 Benchmarks

In this section, we introduce the policies that we use to benchmark the performance of our DQL approach. In the main body of the paper, we present comparisons to five benchmarks, three existing and two new policies. Additional benchmarks are introduced and analyzed in Appendix. We first briefly review the PFA approach introduced in Ulmer and Thomas (2018) and its two special cases. We then introduce two additional PFA variants.

As discussed in Section 2.1, Ulmer and Thomas (2018) introduce a PFA approach to solve the SDDPVD. The authors consider a policy that incorporates the intuition that drones are suitable to serve the customers that are farther from the depot and vehicles serve those that are closer. The PFA policy π PFA is parameterized by a vehicle-travel-time threshold τ . That is, the only feature that π PFA uses is d C ( k ) . The policy works as follows. When customer C k makes a delivery

request at t k , the dispatcher assigns a request either to a drone if d C ( k ) greater than the threshold τ or to a vehicle, if d C ( k ) ≤ τ . When the corresponding fleet type is not available, the order is assigned to the alternative fleet, or it is rejected in the case such an assignment is also not feasible.

We then consider two special cases of the PFA policy. The first one is π PFA\_dro , where we preferably serve customers by drones ( τ = 0 ). We address it as drones first as it always assigns requests to drones whenever it is feasible, and considers assignments to vehicles only when no drones are available. The second one is π PFA\_veh in which customers are served preferably by vehicles ( τ = ∞ ). It is addressed as vehicles first as it oppositely considers assignments to vehicles first and then drones.

We also consider a similar policy to π PFA that allows the rejection of feasible customers. We call this policy π PFA\_rej . In this policy, when a customer is infeasible with regard to the fleet designated by the threshold, the customer is not offered service. For example, if a customer falls within the threshold but cannot be feasibly served by any vehicle, then it is offered no service rather than being considered for service by a drone. For both policies π PFA and π PFA\_rej , we learn the threshold τ in the manner described in Ulmer and Thomas (2018).

To take advantage of alternative state information, we also consider a PFA-based policy π Delta that is parametrized by an insertion-cost threshold κ for vehicles. As described in Section 4.3, every new customer is associated with a potential insertion cost ∆ Vehicle if it is feasible to serve the customer by vehicles. One of the goals of using drones in SDD is to discourage vehicles from traveling to remote areas because such dispatches can be costly. The π Delta is designed to avoid these less preferred decisions by controlling the insertion cost to be within a threshold κ . Given a new customer request, the policy checks feasibility for vehicles and the corresponding insertion cost ∆ Vehicle . If service by a vehicle is feasible and ∆ Vehicle &lt; κ , the customer is assigned to the vehicle. Otherwise, the policy selects delivery by drones if feasible, or no service is offered. Parameter κ is determined by enumeration.

## 5.3 Solution Quality

We use the solution quality Q , the average number of served customers, as the measure of performance of different policies. For each policy π , we define its solution quality as the average percentage of served orders:

<!-- formula-not-decoded -->

To compare the performance of different policies to the main benchmark π PFA , we define the improvement of π a over π PFA as:

<!-- formula-not-decoded -->

The results are summarized in Table 1. We denote our DQL approach π Q . For π PFA , we present the average number of customers served. We then use π PFA as a benchmark and report the other policies' performance relative to π PFA . We also perform a paired-sample t -test with π PFA . The mark * indicates that the p-value of a paired-sample t -test is greater than 0 01 . .

Overall, our DQL approach π Q outperforms the benchmarks and improves upon π PFA by up to 22% . The value of π Q is greatest in the cases in which resources are more constrained. For example, with 2 vehicles and 5 drones, π PFA can only serve about 45 5% . ( 51 0% 43 3% . / . ) of homogeneously (spatial-/temporal- heterogeneously) distributed customers. The policy π Q improves the solution quality by more than 20% in all three cases. As resources become less constrained, the relative performance of π Q diminishes. Simply, with abundant resources, there is sufficient slack to overcome the poorer decisions of the benchmark policies.

In only a very few cases, π PFA achieves better results than π Q . Yet, in those settings, the difference is small, and both π Q and π PFA serve a large percentage of customers. Given sufficient resources, the PFA is an effective policy. In these cases, the option of not offering service is therefore not beneficial. Because decision making of π Q is based on approximated values, in a few cases no service is offered nonetheless. This is likely the reason for the slight inferiority of π Q for these instances.

With the same size of the fleet, all policies serve more customers in the spatially heterogeneous instances than those in the homogeneously distributed instances. This difference results from the fact that, on average, customers for the spatially heterogeneous distribution are closer to the depot and therefore easier to serve. For the temporally heterogeneous instances, fewer customers are served, likely because, during off-peak times, vehicles are not fully utilized while during peak-times, the number of customers is too large for the given resources.

Table 1: Solution Quality: Improvements (%) over π PFA for different fleet setups and customer distributions.

| Fleet Size (Veh, Drone)                        | π PFA                                          | P ( π PFA_dro )                                | P ( π PFA_veh )                                | P ( π Delta )                                  | P ( π PFA_rej )                                | P ( π Q )                                      |
|------------------------------------------------|------------------------------------------------|------------------------------------------------|------------------------------------------------|------------------------------------------------|------------------------------------------------|------------------------------------------------|
| homogeneously distributed customers            | homogeneously distributed customers            | homogeneously distributed customers            | homogeneously distributed customers            | homogeneously distributed customers            | homogeneously distributed customers            | homogeneously distributed customers            |
| 2, 5                                           | 227.6                                          | -8.0                                           | -6.9                                           | 5.8                                            | 18.7                                           | 22.0                                           |
| 2, 10                                          | 312.8                                          | -10.1                                          | -9.2                                           | 0.5                                            | 8.8                                            | 10.7                                           |
| 2, 15                                          | 391.1                                          | -11.6                                          | -9.6                                           | -1.4                                           | 4.4                                            | 6.8                                            |
| 3, 5                                           | 293.4                                          | -8.1                                           | -7.4                                           | 3.5                                            | 13.5                                           | 16.7                                           |
| 3, 10                                          | 376.2                                          | -11.1                                          | -9.7                                           | -0.3*                                          | 6.5                                            | 9.2                                            |
| 3, 15                                          | 460.3                                          | -14.6                                          | -11.9                                          | -3.7                                           | 0.3                                            | 3.0                                            |
| 4, 5                                           | 354.7                                          | -8.1                                           | -7.4                                           | 1.9                                            | 9.0                                            | 11.0                                           |
| 4, 10                                          | 439.9                                          | -13.0                                          | -10.9                                          | -2.3                                           | 2.7                                            | 3.6                                            |
| 4, 15                                          | 499.6                                          | -12.2                                          | -9.4                                           | -2.2                                           | -0.9                                           | 0.0*                                           |
| heterogeneously distributed customers in space | heterogeneously distributed customers in space | heterogeneously distributed customers in space | heterogeneously distributed customers in space | heterogeneously distributed customers in space | heterogeneously distributed customers in space | heterogeneously distributed customers in space |
| 2, 5                                           | 255.2                                          | -10.1                                          | -6.0                                           | 9.5                                            | 14.4                                           | 21.9                                           |
| 2, 10                                          | 349.9                                          | -12.7                                          | -10.0                                          | 2.1                                            | 4.9                                            | 10.2                                           |
| 2, 15                                          | 449.4                                          | -9.6                                           | -12.6                                          | -3.8                                           | -2.5                                           | 3.2                                            |
| 3, 5                                           | 336.2                                          | -11.3                                          | -8.8                                           | 9.0                                            | 10.9                                           | 16.6                                           |
| 3, 10                                          | 441.6                                          | -11.7                                          | -13.7                                          | -0.4                                           | 0.7                                            | 5.2                                            |
| 3, 15                                          | 498.0                                          | -7.3                                           | -9.1                                           | -1.0                                           | -1.4                                           | 0.0*                                           |
| 4, 5                                           | 425.2                                          | -10.6                                          | -12.1                                          | 2.6                                            | 4.5                                            | 6.5                                            |
| 4, 10                                          | 497.7                                          | -7.8                                           | -11.0                                          | -1.2                                           | -1.2                                           | -0.1                                           |
| 4, 15                                          | 499.2                                          | -0.9                                           | -1.3                                           | 0.0*                                           | 0.0*                                           | 0.0                                            |
| heterogeneously distributed customers in time  | heterogeneously distributed customers in time  | heterogeneously distributed customers in time  | heterogeneously distributed customers in time  | heterogeneously distributed customers in time  | heterogeneously distributed customers in time  | heterogeneously distributed customers in time  |
| 2, 5                                           | 216.7                                          | -8.7                                           | -10.3                                          | 5.9                                            | 20.4                                           | 20.9                                           |
| 2, 10                                          | 291.7                                          | -10.9                                          | -11.4                                          | 0.8                                            | 10.6                                           | 12.2                                           |
| 2, 15                                          | 363.2                                          | -11.6                                          | -11.6                                          | -1.7                                           | 5.3                                            | 4.7                                            |
| 3, 5                                           | 280.8                                          | -8.5                                           | -9.7                                           | 4.4                                            | 16.6                                           | 17.1                                           |
| 3, 10                                          | 358.0                                          | -12.0                                          | -11.8                                          | -0.6                                           | 8.0                                            | 6.7                                            |
| 3, 15                                          | 433.8                                          | -13.6                                          | -13.4                                          | -4.0                                           | 1.8                                            | -1.4                                           |
| 4, 5                                           | 342.2                                          | -8.6                                           | -9.0                                           | 2.7                                            | 11.3                                           | 10.7                                           |
| 4, 10                                          | 422.0                                          | -12.2                                          | -12.2                                          | -2.3                                           | 3.3                                            | 1.6                                            |
| 4, 15                                          | 485.9                                          | -12.8                                          | -12.3                                          | -3.8                                           | 0.0                                            | -3.4                                           |

Another interesting observation is the relative performance of policies π PFA and π Delta . For instances with only a few drones, π Delta outperforms π PFA . This changes when the number of drones increases. Policy π PFA utilizes a drone-centric feature of direct travel time to the customer while π Delta draws on the corresponding, but vehicle-centric feature of the increase in route duration. Thus, by shifting the fleet composition from vehicles to drones, the performance advantage shifts from π Delta to π PFA as well. We further observe that π PFA\_rej performs better than π PFA . Thus, just having the option of not offering service already improves solution quality. Finally, both policies π PFA\_dro and π PFA\_veh perform very poorly regardless of the instance setting.

In the next sections, we analyze the impact of feature selection and robustness of our policies. For an illustration of decision-making process, we refer the reader to Appendix A.2.

## 5.4 Impact of Feature Selection

To demonstrate the value of our feature selection, we present how our policy performs for different subsets of features. For the computations in this section, we use the customer demand that follows a homogeneous distribution in both time and location with 3 vehicles and 10 drones.

Figure 3 presents the results of the training for four different feature sets. Additional sets are presented in Appendix A.3. All sets contain the point of time of the current decision point ('Time'). The first set, whose results are shown in the top left, contains the features proposed in this paper, the DQL-features reflecting the state and action spaces. The action space is represented by 'Dist', which is a proxy for the travel time when sending a drone, and 'Delta\_veh', which is

Figure 3: Learning process for different sets of features (The proposed DQL-approach is shown on the top left), for homogeneous customer demand with 3 vehicles and 10 drones.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000002_267e9169a7f72d18ad5c14bfec681985986b1036296e0eb97be29eeaa319cdba.png)

the additional travel time when assigning a vehicle. The state space is reflected by the availability times for all drones ('Dros') and vehicles ('Vehs'). The second subset, whose results are shown are shown on the top right, contains similar features, but focuses on the directly affected single vehicle ('Veh') and single drone ('Dro'), ignoring the state of the rest of the fleets. This set provides a comparison to the approach of Chen et al. (2019) who do not consider fleet interactions in their approach. Q-learning considers the value of a state-action pair. In contrast, the third and fourth sets consider only one part, either the action or state information. The third set, whose results are shown on the bottom left, contains features that reflect only the action space of the problem ('Dist' and 'Delta\_veh'). This set therefore ignores the state information about the utilization of the fleets.

As a fourth set of features, we consider a different approach. We use features representing the post-decision state. The results of using post-decision features are shown on the bottom right of Figure 3.

The post-decision state represents the state immediately following an action selection, but before the realization of new exogenous information. In this case, the post-decision state includes the point of time as well as the availability times of vehicles ('Post\_vehs') and drones ('Post\_dros') that result from an assignment or a rejection of a request. These availability times are extracted after the heuristics are applied, in the case the request is offered service. However, the post-decision features do not include features of the action space such as the distance to the request or the deviation in the vehicle routes needed to include the request, the value we refer to as ∆ Vehicle .

The use of the post-decision state is similar to what is proposed in Joe and Lau (2020). We also evaluate the ability to learn the value of post-decisions states because post-decision state VFA is common in the literature (see Ulmer (2017)) and particularly because post-decision state VFA is used in Ulmer and Thomas (2018), a paper that also looks at SDD but does not consider a heterogeneous fleet.

Each sub-figure of Figure 3 plots the solution quality curve for the first 400,000 training steps. The horizontal axis represents the number of training steps, and the vertical axis represents the solution quality. The horizontal line represents the best found solution value.

We observe that the feature selection proposed in this paper outperforms all other selections. The second and third sets show substantially worse solution quality and limited learning. The fourth set shows a constant, but slow learning process. Even after 400,000 training runs, the solution quality still increases. We note that the VFA can implicitly access the action-space features ('Dist' and 'Delta\_veh') when comparing the different assignment decisions. However, using the features explicitly as input in the DQL such as we do in the first set leads to more effective approximation and better solution quality, at least for this number of training steps. The action-space features are likely to guide decision making, especially in early training runs when the approximation is still weak. This indicates that enriching a value function approximation with action-space features may be beneficial for large-scale problems such as those often observed in dynamic vehicle routing.

## 5.5 Robustness of DQL Policies

In this section, we explore how DQL policies perform when the instance data is varied. We first present the performance of the polices when the fleet size changes. We then show more results when the spatial/temporal distribution of demand is varied.

## 5.5.1 Varying Fleet Size

Our presented approach learns the values for predefined numbers of vehicles and drones. We now analyze how our policy performs when the size of fleet changes. To this end, we take the policy trained for 3 vehicles and 10 drones and test its performance when the number of vehicles changes. Specifically, we change the number of vehicles to 2 and 4 . To apply our method, we then adjust the features for each case because the trained policy expects 3 vehicle availability times. For experiments with 2 vehicles in the fleet, we set the availability time of the third vehicle to t V max throughout the day. For 4 vehicles, we use the earliest and latest vehicle availability times along with the average value of the middle two as the three adjusted features.

Table 2: Solution quality vs. varied fleet sizes over homogeneous customer demand.

|                            |   2 vehs. &10 drones |   3 vehs. &10 drones |   4 vehs. &10 drones |
|----------------------------|----------------------|----------------------|----------------------|
| upper bound                |                346.2 |                410.7 |                455.9 |
| π Q (3 vehs. &10 drones)   |                333.6 |                410.7 |                454.5 |
| π PFA (3 vehs. &10 drones) |                308.8 |                376.2 |                430.8 |

Table 2 presents the solution quality for both π Q and π PFA evaluated with different numbers of vehicles. The first row upper bound presents the policies trained for each fleet size using the original approach where the NNs are trained on the actual fleet size. As shown in the table, π Q still outperforms π PFA when the number of vehicles changes. When the delivery resources become constrained, both policies show a significant gap compared to the upper bound, with 3 6% . for π Q and 10 8% . for π PFA . When there are additional vehicle resources, π Q can serve almost as many customers as the upper bound does, with the gap of only 0 3% . , while π PFA suffers a loss of 5 5% . customers compared to the upper bound. This analysis suggests that, when the fleet size changes moderately, our proposed approach still enables effective decision making.

## 5.5.2 Varying Demand Distribution

In the following, we analyze how our policies handle changes in the demand distribution. To this end, we consider two pairs of customer distributions, the homogeneous and temporally heterogeneous distributions, and the homogeneous and spatially heterogeneous distributions. For each pair, we then stepwise vary the evaluation instances between the two distributions. In each step, we take a percentage of requests from the heterogeneous distribution while the rest of requests is taken from the homogeneous one. The percentage ranges from 0% (fully homogeneous) to 100% (fully heterogeneous), with a step size of 10% . Note, 0% and 100% indicate the data sets on which our presented DQL policies are trained. For all the following tests, we consider a fleet of 3 vehicles and 10 drones. As a benchmark, we apply the same procedure for π PFA .

We first analyze changes in the spatial distribution. Figure 4 presents the solution quality for the policies as we vary the spatial distribution of requests. Policies π Q (0%) and π PFA (0%) ) are trained on the fully homogeneous data, while π Q (100%) and π PFA (100%) are trained on the fully heterogeneous data. All policies show a monotonic increase as percentage of heterogeneity increases because customers become more clustered and geographically closer to the depot.

We observe that π Q outperforms π PFA for every instance setting. For both policy types π Q and π PFA , we see that the policies trained on the original data outperform the respective counterpart. However, π Q (100%) can serve almost as

Figure 4: Solution quality for DQL policies π Q (0%) ( π PFA (0%) ) trained on homogeneous data and policy π Q (100%) ( π PFA (100%) ) trained on spatially heterogeneous data for varied spatial distribution of requests.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000003_d6be2a4f3c39870a26b1abbaa2a81d5fcf9d4edece01a9d86bcb92516bea2fd5.png)

many customers as π Q (0%) for the fully homogeneous case, while π Q (0%) serves about 17 customers fewer on average than π Q (100%) in the fully heterogeneous case. Thus, the DQL-policy trained on heterogeneous data seems more 'robust' to changes, likely because the range of observations during training might be larger.

We next analyze how the policies perform when the arrival rates change. Figure 5 presents the solution quality for the four stated policies evaluated on the data with different heterogeneity in the temporal distribution.

Figure 5: Solution quality for DQL policies π Q (0%) ( π PFA (0%) ) trained on homogeneous data and policy π Q (100%) ( π PFA (100%) ) trained on temporally heterogeneous data for varied temporal distribution of requests.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000004_814cf4d0a90c2781120d2bef95a9dad4118068ee7d71b8b484b88122bada4ead.png)

Again, the DQL policies outperform the PFA in all the temporal settings. Further, we observe a decrease in solution quality with increasing heterogeneity in request times. This indicates that demand peaks may lead to a congestion of the fleet resources and therefore fewer services.

As with the spatial experiments, the respective policies perform best when applied to instances similar to their training data. However, we observe that the policy trained on instances with high temporal heterogeneity π Q (100%) performs significantly worse compared to π Q (0%) . Policy π Q (100%) reserves resources in off-peak times in expectation of a large amount of requests (see Figure 20). For instances without such peaks, such reservations are unnecessary and may lead to an ineffective use of the fleet.

## 6 Conclusions and Future Work

In this paper, we present a SDD problem with vehicles and drones and approximately solve it using reinforcement learning. For this purpose, we identify particular features of the state to include as inputs to the NNs and estimate values of state-decision pairs. Computational results demonstrate that the method is capable of making service decisions

and assignments that appropriately balance the use of vehicles and drones throughout the day resulting in increased expected number of customers served relative to benchmarks. We also show that our policies can still maintain the effectiveness when the fleet size or input data changes moderately.

There are various directions for future research. The analytical results show that the value function of the SDDPVD is monotonic in a number of the state elements. One direction for future research is to explore the enforcement of monotonicity to the learning process for the problem. To the best of the authors' knowledge, the enforcement of monotonicity in NN approximations is relatively unexplored. Our results show that the proposed solution method can cope with changing data. However, this ability depends on the training data. Future work could further explore this issue to more systematically identify how to structure training data for this and other dynamic vehicle routing problems.

We can consider additional instance parameters, for example, different vehicle travel speeds during peak and off-peak hours, and assignment restrictions due to package weight, aerial restrictions and etc. In addition, the problem in this paper considers accepting or not customer requests for service. An alternative would be to consider the pricing of the deadline as a way of serving more customers.

Finally, our policy learns assignments of customers to fleet types based on fleet and customer features. This strategy may be valuable for a variety of dynamic routing problems with heterogeneous fleets and/or heterogeneous customers.

## References

- N. Azi, M. Gendreau, and J.-Y. Potvin. A dynamic vehicle routing problem with multiple delivery routes. Annals of Operations Research , 199(1):103-112, 2012.
- S. Bertoni, M. McGrath, B. Garrett, and K. Jennings. The great retail reinvention. Forbes , pages 61-74, Novemeber 2020.
- A. Bhagat. FedEx teams up with Google Wing to test drone delivery, Sep 2019. URL https://www yahoo com/news/ . . m/f793ddf0-b664-3cda-ba14-eec00760db4e/fedex-teams-up-with-google html . . . Accessed November 20, 2020.
- F. P. Boscoe, K. A. Henry, and M. S. Zdeb. A nationwide comparison of driving distance versus straight-line distance to hospitals. The Professional Geographer , 64(2):188-196, 2012.
- N. Boysen, S. Fedtke, and S. Schwerdfeger. Last-mile delivery concepts: a survey from an operational research perspective. OR Spectrum , pages 1-58, 2020.
- Y. Chen, Y. Qian, Y. Yao, Z. Wu, R. Li, Y. Zhou, H. Hu, and Y. Xu. Can sophisticated dispatching strategy acquired by reinforcement learning? In Proceedings of the 18th International Conference on Autonomous Agents and MultiAgent Systems , pages 1395-1403, 2019.
- I. Dayarian and M. Savelsbergh. Crowdshipping and same-day delivery: Employing in-store customers to deliver online orders. Production and Operations Management , 29(9):2153-2174, 2020.
- I. Dayarian, M. Savelsbergh, and J.-P. Clarke. Same-day delivery with drone resupply. Transportation Science , 54(1): 229-249, 2020.
- D. Fagerlund. Comparison of machine learning algorithms for real-time vehicle selection in transport management, 2018.
- P. Grippa, D. A. Behrens, C. Bettstetter, and F. Wall. Job selection in a network of autonomous UA Vs for delivery of goods. arXiv e-prints , art. arXiv:1604.04180, Apr. 2016. Accessed November 20, 2020.
- C. Guglielmo. Turns out Amazon, touting drone delivery, does sell lots of products that weigh less than 5 pounds, 2013. URL https://www forbes com/sites/connieguglielmo/2013/12/02/turns-out-amazon-. . touting-drone-delivery-does-sell-lots-of-products-that-weigh-less-than-5-pounds/?sh= 8145ff1455ed . . Accessed November 20, 2020.
- L. Hausmann, N. Herrmann, J. Krause, and T. Netzer. Same-day delivery: The next evolutionary step in parcel logistics, 2014. URL https://www mckinsey com/industries/travel-logistics-and-transport-. . infrastructure/our-insights/same-day-delivery-the-next-evolutionary-step-in-parcellogistics . . Accessed November 20, 2020.
- W. Joe and H. C. Lau. Deep reinforcement learning approach to solve dynamic vehicle routing problem with stochastic customers. In Proceedings of the International Conference on Automated Planning and Scheduling , volume 30, pages 394-402, 2020.

M. Joerss, J. Schröder, F. Neuhaus, C. Klink, and F. Mann. Parcel delivery: The future of last mile, 2016.

- E. Kim. The most staggering part about Amazon's upcoming drone delivery service, Jun 2016. URL https:// www businessinsider com/cost-savings-from-amazon-drone-deliveries-2016-6 . . . . Accessed November 20, 2020.
- H. King. UPS partners with Matternet to transport medical samples via drone across hospital campus, Apr 2019. URL https://www parcelandpostaltechnologyinternational com/news/automation/ups-partners-. . with-matternet-to-transport-medical-samples-via-drone-across-hospital-campus-2 html . . . Accessed November 20, 2020.
- D. P. Kingma and J. Ba. Adam: A method for stochastic optimization. arXiv e-prints , art. arXiv:1412.6980, Dec. 2014. Accessed November 20, 2020.
- M. A. Klapp, A. L. Erera, and A. Toriello. The one-dimensional dynamic dispatch waves problem. Transportation Science , 52(2):402-415, 2016.
- M. A. Klapp, A. L. Erera, and A. Toriello. The dynamic dispatch waves problem for same-day delivery. European Journal of Operational Research , 271(2):519-534, 2018.
- M. A. Klapp, A. L. Erera, and A. Toriello. Request acceptance in same-day delivery, 2020.
- W. Kool, H. van Hoof, and M. Welling. Attention, learn to solve routing problems! arXiv e-prints , art. arXiv:1803.08475, Mar. 2018. Accessed November 20, 2020.
- L.-J. Lin. Reinforcement learning for robots using neural networks. Technical report, Carnegie-Mellon Univ Pittsburgh PA School of Computer Science, 1993.
- Y. Liu. An optimization-driven dynamic vehicle routing algorithm for on-demand meal delivery using drones. Computers &amp;Operations Research , 111:1-20, 2019.
- J. Ma´ ndziuk. New shades of the vehicle routing problem: Emerging problem formulations and computational intelligence solution methods. IEEE Transactions on Emerging Topics in Computational Intelligence , 2018.
- V. Mnih, K. Kavukcuoglu, D. Silver, A. Graves, I. Antonoglou, D. Wierstra, and M. Riedmiller. Playing Atari with deep reinforcement learning. arXiv e-prints , art. arXiv:1312.5602, Dec. 2013. Accessed November 20, 2020.
- M. Nazari, A. Oroojlooy, L. Snyder, and M. Takác. Reinforcement learning for solving the vehicle routing problem. In Advances in Neural Information Processing Systems , pages 9839-9849, 2018.
- A. Otto, N. Agatz, J. Campbell, B. Golden, and E. Pesch. Optimization approaches for civil applications of unmanned aerial vehicles (UA Vs) or aerial drones: A survey. Networks , 72(4):411-458, 2018.
- J. F. Peltz and E. Morgan. Amazon taught us to rely on fast delivery. In coronavirus era, it can't keep up, Mar 2020. URL https://www latimes com/business/story/2020-03-18/amazon-delivery-coronavirus . . . . Accessed November 20, 2020.
- J. Pero. Alphabet-owned Wing will begin making drone deliveries in Finland next month, May 2019. URL https://www dailymail co uk/sciencetech/article-7041995/Alphabet-owned-Wing-. . . begin-making-drone-deliveries-Finland-month html . . . Accessed November 20, 2020.
- J.-Y. Potvin, Y . Shen, and J.-M. Rousseau. Neural networks for automated vehicle dispatching. Computers &amp; Operations Research , 19(3-4):267-276, 1992.
- W. B. Powell. Approximate Dynamic Programming: Solving the Curses of Dimensionality . Wiley Series in Probability and Statistics. John Wiley &amp; Sons, Inc., second edition, 2011.
- W. B. Powell. A unified framework for stochastic optimization. European Journal of Operational Research , 275(3): 795-821, 2019.
- A. E. P. Rivera and M. R. Mes. Anticipatory freight selection in intermodal long-haul round-trips. Transportation Research Part E: Logistics and Transportation Review , 105:176 - 194, 2017.
- A. Shapiro. UPS expands drone delivery to fight COVID-19, Apr 2020. URL https://finance yahoo com/news/ . . ups-expands-drone-delivery-to-fight-covid-19-143015430 html . . . Accessed November 20, 2020.
- R. S. Sutton and A. G. Barto. Reinforcement learning: An introduction . MIT press, Cambridge, MA, second edition, 2018.
- L. Thomas. Target expands same-day shipping option in the latest move in the delivery wars with Walmart and Amazon, June 13 2019. URL https://www cnbc com/2019/06/12/target-expands-same-day-delivery-option-. . in-battle-with-walmart-amazon html . . . Accessed November 20, 2020.
- A. Toriello, W. B. Haskell, and M. Poremba. A dynamic traveling salesman problem with stochastic arc costs. Operations Research , 62(5):1107-1125, 2014.

- M. W. Ulmer. Approximate Dynamic Programming for Dynamic Vehicle Routing , volume 61 of Operations Research/Computer Science Interfaces Series . Springer, Berlin, 2017.
- M. W. Ulmer and S. Streng. Same-day delivery with pickup stations and autonomous vehicles. Computers &amp; Operations Research , 108:1-19, 2019.
- M. W. Ulmer and B. W. Thomas. Same-day delivery with heterogeneous fleets of drones and vehicles. Networks , 72(4): 475-505, 2018.
- M. W. Ulmer and B. W. Thomas. Meso-parametric value function approximation for dynamic customer acceptances in delivery routing. European Journal of Operational Research , 285(1):183-195, 2020.
- M. W. Ulmer, D. C. Mattfeld, and F. Köster. Budgeting time for dynamic vehicle routing with stochastic customer requests. Transportation Science , 52(1):20-37, 2018.
- M. W. Ulmer, B. W. Thomas, and D. C. Mattfeld. Preemptive depot returns for same-day delivery. EURO Journal of Transportation and Logistics , 19(4):327-361, 2019.
- M. W. Ulmer, J. C. Goodson, D. C. Mattfeld, and B. W. Thomas. On modeling stochastic dynamic vehicle routing problems. EURO Journal on Transportation and Logistics , page 100008, 2020.
- O. Vinyals, M. Fortunato, and N. Jaitly. Pointer networks. In Advances in Neural Information Processing Systems , pages 2692-2700, 2015.
- S. A. Voccia, A. Melissa Campbell, and B. W. Thomas. The same-day delivery problem for online purchases. Transportation Science , 53(1):167-184, 2019.
- D. Wang. The economics of drone delivery. IEEE Spectrum , 5, 2016.
- H. Wolf. We're about to see the golden age of drone delivery - here's why, Jun 2020. URL https://www weforum org/ . . agenda/2020/07/golden-age-drone-delivery-covid-19-coronavirus-pandemic-technology/ . . Accessed November 20, 2020.

## A Appendix

In Appendix, we present the analytical results along with proofs, illustrations of the decision making, additional feature combinations, an analysis of the value of not offering service, and the results for expanding the assignment decisions.

## A.1 Analytical Results

In this section, we present a series of analytical results with proofs for the SDDPVD. We begin by characterizing Equation (1). Our goal is to identify how the value function reacts to changes in the state. First, we show that Equation (1) is monotonically decreasing in time, the return time of the vehicles, and the return time of the drones. The results are formalized in Propositions A.1, A.2, and A.3.

Proposition A.1. In the SDDPVD, let t represent time of the decision point in the state. Then, the expected reward is monotonically decreasing in t .

Proof. Note, in the SDDPVD and the simplified version, we always assume t v and t d are no earlier than t . For example, if t = 10 in the current state, and the vehicle becomes available at t = 9 , then we set t v = 10 . Consider two states that only differ in time of the decision point. Say s = ( t, . . . . . . ) and s ′ = ( t , . . . ′ . . . ) such that 0 ≤ t &lt; t ′ . If the dispatcher is in the state s , the worst case is that, the dispatcher does not accept any requests that arrive during [ t, t ′ ) , and then, starting t ′ , follows the same path as it will do from the state s ′ . In this worst case, the expected rewards are equal, V ( s ) = V ( s ′ ) . On the other hand, the dispatcher expects to receive n = µ t ( ′ -t ) requests during [ t, t ′ ) . If the dispatcher can accept feasible requests, then, compared to being in the state s ′ , the dispatcher has more requests from which to choose. Therefore, considering both cases, we have V ( s ) ≥ V ( s ′ ) .

Proposition A.2. Suppose, in the SDDPVD, the fleet has one vehicle. Let t v represent the time at which the vehicle returns to the depot from the current route. Then, the expected reward is monotonically decreasing in t v .

Proposition A.3. Suppose, in the SDDPVD, the fleet has one drone. Let t d represent the time at which the drone returns to the depot from the current route. Then, the expected reward is monotonically decreasing in t d .

Proof. The proofs for t v and t d are similar, so we only present that for t v . Consider two states that differ only in the time at which the vehicle becomes available. Say s = ( t v , . . . . . . ) and s ′ = ( t ′ v , . . . . . . ) such that t ≤ t v &lt; t ′ v . The worst case is that the dispatcher can simply postpone t v to t ′ v and then follows the same path as it will do starting from the state s ′ . Thus, we have V ( s ) = V ( s ′ ) for the worst case. Similarly, on the other hand, the earlier available time of the vehicle results in t ′ v -t v extra units of time for the vehicle to make deliveries. Hence, we have V ( s ) ≥ V ( s ′ ) .

We next seek to characterize circumstances in which we can identify optimal actions. Our goal is to identify information that is needed to determine these optimal actions and to make sure that this information is available as a feature. Importantly, we analytically demonstrate the value that not offering service to a particular customer can have on the objective. Appendix A.4 offers a computational demonstration.

Because the structure of the SDDPVD is complex, we simplify the problem for this analysis. We assume that customers dynamically make delivery requests over [0 , max { t D max , t V max } ] . Customer requests are revealed following a Poisson process with rate µ . Each requesting customer's distance from the depot D follows a uniform distribution with the support [0 , D max ] , where D max is the maximum possible distance. The distance D between a customer and the depot is converted to the corresponding vehicle's travel time. In the remainder of this section, we use the term 'distance' synonymously with travel time of the corresponding fleet type. The arrival times of requests are independent, and the random variables, time t , and distance D are also independent. The fleet consists of a vehicle and a drone. We omit the loading and charging times for the fleet. The capacity of the vehicle is unlimited, and the capacity of the drone is 1. We assume that the drone travels c ( &gt; 1 ) times as fast as the vehicle.

We first consider the situation at the end of the day. Proposition A.4 identifies the circumstances in which a drone idling at the depot can serve at least one more customer before the end of the day.

Proposition A.4. Suppose a drone is available when a new customer that is b ′ (vehicle travel time) units from the depot requests service at time t ′ &lt; t D max . Then, we can serve this customer if t ′ ≤ t D max -b ′ c .

Proof. If the drone is dispatched to serve the request b ′ , it will return to the depot at t ′ + b ′ c . For the drone returning to the depot no later than the end of the horizon T , simply solving the inequality t ′ + b ′ c ≤ t D max gives t ′ ≤ t D max -b ′ c .

This proposition can also be seen as the condition for the idling drone to serve at least 1 customer during [ t , t ′ D max ] . Simply dispatching the drone to serve b ′ will increase the number of customers served by 1. It is also possible when the

drone returns to the depot from b ′ , it is dispatched for other customers. This results in that, at least 1 customer can be served by the drone during [ t , t ′ D max ] .

We now determine whether we should serve an end-of-the-day customer request with an idling drone. Assume the vehicle is in its last route and will return to the depot at t V max . The drone can feasibly serve the customer b ′ at time t ′ as given in Proposition A.4. To determine whether to serve this request at t ′ , we want to know how many requests the drone is expected to serve during [ t , t ′ D max ] . Importantly, it is possible that, instead of serving the current request, the drone could serve several customers whose request arrive after t ′ but that are close to the depot.

To address this question, we calculate the probability that the drone can serve at least one more customer after serving b ′ . We present the probability in Lemma A.1.

Lemma A.1. Suppose that the drone is available when a customer request that is located b ′ (vehicle travel time) units from the depot that is revealed at time t ′ . If the drone is dispatched to serve the customer, then the probability that the drone can serve at least one more customer after returning to the depot is

<!-- formula-not-decoded -->

Proof. At t ′ , the drone is dispatched distance 1 b ′ so it will return to the depot at t ′ + b ′ c , which divides [ t , t ′ D max ] into two disjoint sub-intervals [ t , t ′ ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] ] and ( t ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] , t D max ] . We use the ceiling because the drone will not be dispatched until the end of a unit period.

We consider [ t , t ′ ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] ] as a single period because the drone cannot start a second delivery tour until it is back to depot. Note, µ is the average number of arriving customer requests per unit period, and [ t , t ′ ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] ] consists of glyph[ceilingleft] b ′ c glyph[ceilingright] unit periods. The new rate µ [ t ,t ′ ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] ] for the single interval [ t , t ′ ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] ] is

<!-- formula-not-decoded -->

When the drone returns to the depot, the time left till the end of service period is t D max -t ′ -glyph[ceilingleft] b ′ c glyph[ceilingright] , so for a customer revealed during [ t , t ′ ′ + b ′ c ] to be able to be serviced by the drone, according to Corollary A.4, its distance d must satisfy

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

Since the distance of customers is a uniform random variable with the support [0 , D max ] , the probability of each arriving request during [ t , t ′ ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] ] that can be served by the drone is

<!-- formula-not-decoded -->

Because customer request arrivals and service times are independent, it follows that such feasible customer requests arrive following a Poisson process with rate

<!-- formula-not-decoded -->

Hence, the probability that no such feasible requests arrive during [ t , t ′ ′ + b ′ c ] is

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

1 Throughout the proofs, we use 'distance' synonymously with the travel time of the corresponding fleet type.

<!-- formula-not-decoded -->

Next, we consider ( t ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] , t D max ] as t D max -t ′ -glyph[ceilingleft] b ′ c glyph[ceilingright] individual periods. For the k th minute after t ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] , we can repeat the similar process performed for the interval [ t , t ′ ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] ] and get the probability of no feasible requests arriving during the k th minute is

<!-- formula-not-decoded -->

Because we assume arrival times of requests are independent, the probability that no feasible requests are revealed during the interval ( t ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] , t D max ] is

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

Therefore, the probability that, no feasible requests for the drone will arrive if we dispatch the drone to serve b ′ is

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

Then, the desired probability is

<!-- formula-not-decoded -->

Instead of serving the current customer request arriving at time t ′ , the dispatcher can choose not to offer the service to customer. We would make such a decision if by doing so we would expect to serve more customers thereafter. The probability that the drone can serve at least one customer is given in Lemma A.2.

Lemma A.2. The drone is available when a customer request b ′ units from the depot is revealed at t ′ . If the dispatcher does not offer service to the new customer, then the probability that the drone can instead serve more than 1 customer thereafter is

<!-- formula-not-decoded -->

Proof. We tackle the desired probability P [ t ,t ′ D max ] ,n ≥ 2 ∣ ∣ rej by computing

<!-- formula-not-decoded -->

- · P [ t ,t ′ D max ] ,n =0 ∣ ∣ rej

The proof is similar to that for Lemma A.1. Simply replace the interval ( t ′ + glyph[ceilingleft] b ′ c glyph[ceilingright] , t D max ] by [ t , t ′ D max ] , and we get the probability

<!-- formula-not-decoded -->

- · P [ t ,t ′ D max ] ,n =1 ∣ ∣ rej

Assume the drone is dispatched to the only customer it serves during [ t , t ′ D max ] at the k th minute after t ′ . It divides [ t , t ′ D max ] into three intervals [ t , t ′ ′ + k -1] , ( t ′ + k -1 , t ′ + ] k and ( t ′ + k, t D max ] .

During [ t , t ′ ′ + k -1] , the probability that there are no requests the drone can feasibly serve arriving is

<!-- formula-not-decoded -->

We assume the only request the drone serves is made during ( t ′ + k -1 , t ′ + ] k , and its distance is between integers m -1 and m . By Proposition A.4, the maximum distance of this feasible request is c t ( D max -t ′ -k ) , which results in m ∈ { 1 2 3 , , , . . . , glyph[floorleft] c t ( D max -t ′ -k ) glyph[floorright]} . Given the distance of the request, by Proposition A.1, the probability the drone serves this customer and cannot serve any more customer thereafter is

<!-- formula-not-decoded -->

Iterating k from 1 to t D max -t ′ , we get the probability

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

Hence, we get the simplified desired probability

<!-- formula-not-decoded -->

Using Lemmas A.1 and A.2, Proposition A.5 identifies when the dispatcher should not accept a feasible request.

Proposition A.5. Assume that the drone is available when a customer request that is b ′ units from the depot is revealed at t ′ . Let b ∗ be the travel time that equates the probabilities in Lemmas A.1 and A.2. The dispatcher should always accept and assign the feasible request to the drone if b ′ ≤ b ∗ , and not accept it if b ′ &gt; b ∗ .

Proof. Consider the probabilities in Lemmas A.1 and A.2 as functions in b ′ , denoted y 1 ( b ′ ) and y 2 ( b ′ ) . Because both functions involve ceiling and/or floor functions, it is impossible to solve for b ∗ explicitly. Instead, we prove the proposition using the first derivative of y 1 ( b ′ ) . Note that y 2 ( b ′ ) is constant in b ′ because its expression does not contain b ′ . When b ′ = 0 ,

<!-- formula-not-decoded -->

If we drop the ceiling function, the derivative of y 1 ( b ′ ) is

<!-- formula-not-decoded -->

The critical number is b ′ = c 2 &gt; 0 . It means y 1 ( b ′ ) is increasing on (0 , c 2 ) and decreasing on ( c 2 , ∞ ) . Given y 1 (0) &gt; y 2 (0) , we can always find the b ∗ at which y 1 ( b ′ ) and y 2 ( b ′ ) intersect.

Furthermore, because of the increasing and decreasing intervals, we have y 1 ( b ′ ) ≥ y 2 ( b ′ ) for 0 &lt; b ′ ≤ b ∗ , and y 1 ( b ′ ) &lt; y 2 ( b ′ ) for b ∗ &lt; b ′ &lt; ∞ . If y 1 ( b ′ ) is greater, it means the probability of achieving 1 immediate reward and at least 1 future reward is higher than that of achieving at least 2 future rewards. In such case, the dispatcher should always accept the request and assign it to the drone. Otherwise, the dispatcher should not provide service for the customer because the probability of having at least 2 future rewards is higher.

For three values of t ′ , Figure 6 illustrates Proposition A.5 for parameter settings c = 1 5 . , µ = 1 , D max = 40 , and t D max = 420 . The horizontal axis represents the possible distance of the new feasible customer, and the vertical axis is the probability. The probability in Lemma A.1 (black dots in Figure 6) can be seen as that of achieving 1 immediate reward and at least 1 future reward. The probability in Lemma A.2 (blue dots) can be seen as that of achieving at least 2 future rewards.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000005_6b918557f8f91a0d3b909a1c88d159dd00a67598e95b16e5126a3304f2cfd78b.png)

Figure 6: The probabilities vs. the distance of the customer request

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000006_d9a12edeb4aa8a4827aaaf5fe0c2923fd69118d813917e25e679ff4c0d03ccd0.png)

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000007_d36b9b21f8b3396710e0c24a2d86242f1ffdedf363fa974cf141c4714c1b0610.png)

As shown in the left-hand figure, given these parameter settings, there is a time before the horizon at which we always accept the customer. When it is long enough before the end of the horizon, b ∗ is much greater than D max so the two probabilities do not intersect in the plot. The probability that the drone can serve at least one more customer after serving the current one is always higher than the other one. In this case, the dispatcher should always take the immediate reward. As it is closer to the end of horizon, t ′ = 410 , the probability of achieving a future reward of 2 goes down. The two probabilities intersect at about b ∗ = 7 5 . . In this situation, the dispatcher should accept the feasible request if b ′ is between 0 and approximately 7 5 . . Otherwise, the dispatcher should consider not accepting the request, because as larger b ′ becomes larger, the chance that we cannot serve any more customer thereafter grows. At t ′ = 416 when it is even closer to the end of the horizon, both probabilities are relatively low. In this situation, we observe the value of b ∗ is smaller than that for t ′ = 410 . As shown in this illustrative example, the threshold concerning denial decisions should incorporate the time in a shift.

In the case the drone is unavailable for the rest of the day, and the vehicle is idling at the depot, we can derive results analogous to Proposition A.5. In the case of the vehicle though, we need to consider the cost of inserting a customer in the vehicle's route rather than just the travel time from the depot. We denote this cost as ∆ Vehicle , the increase in the vehicle's tour time that results from inserting the customer to the vehicle's planned route. Due to their different capacities, the times at which the drone and the vehicle are planned to return to the depot are determined by different quantities. The drone has a capacity one so it must return to the depot after every delivery. The vehicle can serve multiple customers in a route. If the dispatcher assigns a new request to the vehicle, the time at which it is planned to return is postponed by ∆ Vehicle , the insertion cost of this new request. We wanted to develop the proposition for the vehicle similar to Proposition A.5. However, due to the unlimited capacity of the vehicle, we expect it is far more complicated and thus not practical to do so. Because of the inter-dependencies of decisions, we were not able to provide a straightforward proof.

Although the SDDPVD is way more complicated than the simplified version, we can still use the similar logic for the analysis of the SDDPVD. For example, when the delivery resources become limited, the dispatcher should consider offering no service to some feasible requests in exchange for a higher expected reward in the future. We take the logic in this analysis as motivation to select the features for the SDDPVD.

## A.2 Illustration of the Decision Making

In this section, to gain insight into why π Q outperforms π PFA , we graphically illustrate the differences in the policies π PFA and π Q . We first present the results for the homogeneous customer demand and then for the temporally heterogeneous demand because it allows a very clear distinction between the two policies, followed by that for the spatially heterogeneous demand.

## On Homogeneous Customer Demand

To demonstrate these differences, we first select a single instance realization (a day) on which different policies are evaluated. Then, for each policy, we plot the acceptance and assignment decisions of each customer throughout the day. We consider instances with a fleet of 3 vehicles and 10 drones. We first assume homogeneous demand. For this selected instance, policies serve slightly more customers than on average with policy π PFA serving 394 and π Q serving 444 customers.

Figures 7 and 8 illustrate the served customers and how they are served. The horizontal axis is the time in minutes ranging from 0 to the latest possible order time 420 , and the vertical axis represents the vehicle travel time needed by a vehicle to service a given request where the travel time is based on the customer's distance from the depot. Each dot in the figures represents a customer order whose coordinates on the plot are determined by when and where they make the request and whose color is depends on the decision made by the corresponding policy, service by vehicle, drone, or no service.

Figure 7: Illustration of decision making under π PFA . Assignment decisions over time and customer distance for the selected homogeneous instance realization.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000008_617337211a51e8fb58ddd5ed781d3e38281db53f42e36c020cd99a1708b16485.png)

Figure 7 presents the decisions for the policy π PFA . This policy uses the vehicle travel time threshold τ = 14 to choose between vehicles and drones. The figure shows that, in the interval [0 , 200] , both vehicles and drones can feasibly serve customers. However, starting around t = 200 , vehicle capacity becomes limited because of existing assignments, and vehicles are unable to meet the delivery deadlines of new requests. Thus, the policy π PFA starts to assign customers close to the depot to drones. This result follows from the fact that π PFA will assign customers that cannot be served by vehicles to the drone fleet if such an assignment is feasible. As a result, the vehicle travel time threshold vanishes over the second half of the day, and both drone and vehicle capacity becomes limited. Eventually, a relatively large number of customers are left unserved.

Intuitively, a good policy serves as many closer customers as possible because the cost of traveling to them is relatively low. Yet, consider customer (221 4) , that is close to the depot and orders in the middle of the day. This customer does not get served because both drone and vehicle capacity is consumed. This occurs because customers, like that customer (278 3) , who is close to the depot, are and assigned to a drone. Such an assignment does not make an efficient use of the drone because the relatively long setup and charging times outweighs the travel speed advantage for customers close to the depot. This example illustrates the shortcomings of the policy π PFA serving customers whenever it is feasible for any fleet type.

Figure 8 illustrates the decision making of policy π Q . In the selected instance, π Q demonstrates significant improvements over all the benchmarks, ranging from 12 1% . (over π PFA\_rej ) to 12 7% . (over π PFA ). Although π Q does not operate on any kind of threshold, Figure 8 indicates an emergent time-dependent threshold that results from the learned Q-values. In the

Figure 8: Illustration of decision making under π Q . Assignment decisions over time and customer distance for the selected homogeneous instance realization.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000009_65cdd44faae917042fad94445c087e628569e549e8163f46fe8891b1e014a0c6.png)

beginning of the day when the delivery resources are sufficient, π Q assigns most customers within the threshold (about 14 minutes) to vehicles and distant customers to drones. During about [100 200] , , when vehicle capacity is mostly consumed, π Q shows a slightly diminishing threshold, maintaining the availability of vehicles. Unlike the benchmarks, even when drones can feasibly serve closer customers during [100 200] , , π Q does not assign them to drones. In fact, it is true for the whole day except customer (418 6) , at the very end of the day. This exception can be explained by Lemma A.2. Because the time of the decision point is one of the features, π Q recognizes that this request is made nearly at the end of the shift. Rather than offering no service to the customer, π Q takes the immediate reward and assigns it to a drone because the probability of serving at least 2 customers in the future if not serving the current one is relatively low. The policy π Q self-learns the policy without any explicit knowledge of the lemmas and propositions.

Figure 9: Time vs. travel time vs. decision under π PFA\_rej on the selected homogeneous instance.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000010_a45515edfb2a3ad68c5eb68add41ed1e07ae838a062d6480a75b01a46b586093.png)

Figure 9 presents an illustration of the decision making of policy π PFA\_rej . In this case, the policy has a hard threshold τ = 13 . Because the policy strictly obeys the threshold, we observe an explicit horizontal line throughout the day. In rigidly maintaining the threshold, we also see that the policy does not provide service to a number of customers over the second half of the day. Yet, in determining which customers do not receive service in a more controlled way than the policy π PFA , the policy π PFA\_rej serves 2 more customers than π PFA in this selected instance and even about 6 5% . on average for the instance setting.

Figure 10 presents an illustration of the decision making of π Delta . Because π Delta decides whether to offer service to a customer on the insertion cost, no threshold is visible. While the policy performs well relative to the other benchmarks, we can see times when the rule-based decision making leads to less desirable decisions. Consider the assignments near time 100. At this point, the vehicles end up serving customers that are relatively far from the depot. The threshold-based policies would have controlled the farther away customers from being added to the routes and thus less efficient use of the vehicles. The result is that a series of relatively closer customers are assigned to drones in the time interval

Figure 10: Time vs. travel time vs. decision under π Delta on the selected homogeneous instance.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000011_7264304ec42ee1d72c849fac84a9b9ffaeb09c17633029536bd612f7844a5c87.png)

[100 200] , . Then, just after time 200, a number of requests are denied service because both vehicle and drone capacity have been consumed.

We then extend this anecdotal analysis to the full set of instance realizations. For each policy and assignment decision, we calculate the average distance of the corresponding customer for time intervals of 15 minutes. An average value of zero indicates that no customers received the corresponding decision. The results for π PFA and π Q are depicted in Figures 11 and 12. We note that the figures do not depict customer quantities. For both policies, we observe similar

Figure 11: Illustration of decision making under π PFA . Average assignment decisions over time and customer distance over all homogeneous instances.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000012_d13b0e86fba09ad0c162dabf9368ef0baf8d25de165a69801e58d7710685b14a.png)

Figure 12: Illustration of decision making under π Q . Average assignment decisions over time and customer distance over all homogeneous instances.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000013_6e2ff0479acff9f29b8062eaba84e37c7b5cbae253adf1fa93e7a6cd7020d039.png)

patterns in the beginning. Customers assigned to drones are further away from the depot than customers assigned to vehicles. Further, all customers are offered service. Later, the two policies differ. For the PFA, the first customers that cannot be served occur around time 150. Furthermore, after that time, the distances for customers assigned to drones and to vehicles even out. This confirms the observation in Figure 7 that for the second half of the day, no clear assignment pattern can be observed.

For policy π Q , the first customers where no service is offered occur earlier compared to π PFA . Further, they are more distant. This indicates that while the customers might be feasible to serve, policy π Q starts budgeting resources to keep the fleet's flexibility. As a result, the clear and effective structure of drone assignment to distant customers and vehicle assignment to closer customers can be maintained throughout the day.

Figures 13 and 14 present the average distance of customer requests with respect to different assignment decisions under π PFA\_rej and π Delta . Because it does not allow vehicles and drones to cross the threshold, π PFA\_rej shows a

Figure 13: Average distance of requests vs. assignment decisions under π PFA\_rej for homogeneous demand.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000014_30932a2edb1b01b743dc7643d8f7513f6fa692c5de8784b845469cfda01ec506.png)

Figure 14: Average distance of requests vs. assignment decisions under π Delta for homogeneous demand.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000015_01a237e080ba0244d968d8006724c40d2bb09e1135cd5c74c866c31c78792e20.png)

relatively consistent average distance for decisions vehicle and drone throughout the day. It starts to offer no service to some customers shortly after t = 100 . The average distance for no service decreases as the resources become more constrained thereafter. For π Delta , the average distance for drone decreases throughout the day, while that for vehicle is relatively consistent. Among all the policies considered, π Delta is the earliest to offer no service to some customers. One explanation is that the policy carefully assigns requests to vehicles but does not consider the efficiency of drones, and thus uses up the overall resources relatively faster.

## On Temporally Heterogeneous Customer Demand

For the selected single instance with temporally heterogeneous demand, π PFA serves 324 customers, while π PFA\_rej serves 371 , π Delta serves 358 , and π Q serves 361 customers.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000016_c93622946013f58295059a22644efec2d081d408c66dfc1acf0aaff7cdb9b5e6.png)

Figure 15: Time vs. travel time vs. decision under π PFA on the selected temporally heterogeneous instance.

Figure 16: Time vs. travel time vs. decision under π PFA\_rej on the selected temporally heterogeneous instance.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000017_558c39b636785b814ce56914d487603ab978289b483044f70f9ff24ea96fd45b.png)

Figure 17: Time vs. travel time vs. decision under π Delta on the selected temporally heterogeneous instance.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000018_e0fbf654300a3f65fc34faa69191a3e3bb77578f7fa8519ed8b44d943f61d0e0.png)

On the selected single instance, all the policies start to offer no service to some customers at the end of the first peak, and many are not offered service during the second peak. A notable difference between π Q and the others is that, in the early stage, π Q assigns most of the requests to drones, while the others uses both fleets.

We now analyze the results for the entire set of instances with temporally heterogeneous demand, again with 3 vehicles and 10 drones. Similar to Figures 11-14, we depict the average distances over time in Figures 19-22. For the instances, two demand-peaks occur between times 50 and 150 as well as 250 and 350 . We observe that the structure of π PFA only lasts up to the first peak, recovers between the peaks and is then disturbed again in the second peak. Shortly after beginning of the first peak and right when the second peak begins, customers are not offered service. The first peak consumes resources fast and at time the second peak starts, the resources have not fully recovered, especially, the vehicle fleet.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000019_6ca4fbb8691822151eca6d8ba4fe615c256b974d8113e9b3e1666ff64ec4105d.png)

Figure 18: Time vs. travel time vs. decision under π Q on the selected temporally heterogeneous instance.

Figure 19: Illustration of decision making under π PFA . Average assignment decisions over time and customer distance over all temporally heterogeneous instances.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000020_c683ba632a15402be307369a761948ebf43aa9143c9dd61474c87ab4369c8730.png)

Figure 20: Illustration of decision making under π Q . Average assignment decisions over time and customer distance over all temporally heterogeneous instances.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000021_50febff279aeab30738f3b7ce487063df3b0f9d19bf28a8fe42845e4c47e0d1d.png)

There are several differences for policy π Q . As for the homogeneous instances, offering no service starts earlier than for π PFA and is also applied in the off-peak times, likely to free resources for the peak-times. However, we also observe that the vehicles are used differently. During off-peak times, at some times, no orders at all are assigned to vehicles, and if orders are assigned, they are close to the depot. Instead, off-peak orders are served by drones. This behavior indicates that π Q reserves vehicle resources for the peak-times where the consolidation potential of the vehicles become particularly valuable. That is, the advantage that the vehicles have for serving many customers on a single route becomes available as the customer density increases.

Figure 21: Average distance of requests vs. assignment decisions under π PFA\_rej for temporally heterogeneous demand.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000022_eaea4ba2a2e4102e739fe46cc131c2d45ed92b5b19aec4c4f91f6f892949989b.png)

Figure 22: Average distance of requests vs. assignment decisions under π Delta for temporally heterogeneous demand.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000023_326b07e4bf3dc52d0c221c19506075ef9f99b5a4ed18099e8e521d1f77b32ad6.png)

## On Spatially Heterogeneous Customer Demand

For the selected instance with spatially heterogeneous demand, policies serve slightly more customers than those with homogeneous demand on average with policy π PFA serving 447, π PFA\_rej serving 437, π Delta serving 447, and π Q serving 476 customers.

As for the decision making, we observe similar solution structures to those for homogeneous customer demand. Figures 23 through 25 present the decision making on the single instance for π PFA , π PFA\_rej and π Delta . Figure 26 shows the results for the heterogeneous distribution and policy π Q .

Figure 23: Time vs. distance vs. decision under π PFA on the selected spatially heterogeneous instance.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000024_c97f3de4e381328f26c630027ba6576a2b70bfb3f3c66fc6ced08a5e4489b417.png)

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000025_5a90b8aa25b3dda674ae5465e25e50fb3b8b64abd89a1fceb386184dacfbbe11.png)

Figure 24: Time vs. distance vs. decision under π PFA\_rej on the selected spatially heterogeneous instance.

Figure 25: Time vs. distance vs. decision under π Delta on the selected spatially heterogeneous instance.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000026_40cf1dfb13caeed3123daeff66bdccc26619ff382ff4ce3f93572cb52cc090ce.png)

Figure 26: Time vs. travel time vs. decision under π Q on the selected spatially heterogeneous instance.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000027_87574d56c7b203c86751949c794dc641751a69618d5331075417b3add8c6334c.png)

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000028_d691eeb227d7610df31fae82a9bfbccf3e3c5c29c3b283cc534f01b50b058a72.png)

Figure 27: Average distance of requests vs. assignment decisions under π PFA for spatially heterogeneous demand.

Figure 28: Average distance of requests vs. assignment decisions under π PFA\_rej for spatially heterogeneous demand.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000029_6877161581e9e265da160507da5e938b09c26a0aff425276a8fa1d2a62e5c4eb.png)

Figure 29: Average distance of requests vs. assignment decisions under π Delta for spatially heterogeneous demand.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000030_75a2cdfcc42435ad2aef6d1dc08960a851c20d4af65ba05079f2015efd967dae.png)

Figure 30: Average distance of requests vs. assignment decisions under π Q for spatially heterogeneous demand.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000031_6abaecf9d85056bcedbdc46d08590b5acf092734cddf2a1681c8dd820ef4faa1.png)

## A.3 Impact of Feature Selection

In Figure 31, we present two additional sets of features for our problem. On the left, the features time, distance to the

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000032_c185816c434a16a52cb2f757bee034bda51c2eb976019190691c4d61f6b54561.png)

Figure 31: Solution quality curves with different features (3 vehicles 10 drones)

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000033_043063b1e49ac816c17c03a6f40cfaaef7a480a140e5a16235abd031c5fd4ccc.png)

customer, and availability times of the corresponding vehicle and drone are shown. On the right side, the set contains the time and the feature suggested in Ulmer and Thomas (2018), the distance to the customer. The results show that, the policies trained with these two sets of features still underperform our proposed policy after the same number of training epochs.

## A.4 The Value of Not Offering Service

This section presents a computational comparison of a variant of π Q that does not offer the opportunity to not offer service. Rather, the policy, denoted π Q\_no\_rej uses logic of offering service similar to π PFA . Table 3 summarizes the logic.

Table 3: Logic of π Q\_no\_rej

| Vehicle Feasibility   | Drone Feasibility   | Accpt. and Assignment                              |
|-----------------------|---------------------|----------------------------------------------------|
| × √ × √               | × × √ √             | No service Vehicle Drone φ ( s ) = { Vehicle Drone |

The policy π Q\_no\_rej requires only one NN. We use this NN to determine whether to serve a customer with vehicles or drones when both are feasible. We use the same features as those for π Q .

We take the homogeneous customer demand to illustrate the performance of π Q\_no\_rej . Tables 4 presents results analogous to those in Table 1 found in Section 5.3. The results show that the policy π Q\_no\_rej is marginally better than the policy π PFA . However, π Q\_no\_rej generally performs significantly worse than π Q . The reason for this is illustrated in Figures 32 and 33. These figures are analogous to Figures 8 and Figure 12, respectively.

## A.5 Expanding the Assignment Decisions

For our main experiments, we use a cheapest inserting heuristic to determine the assignment, i.e., the vehicle the order is assigned to. This leads to a significant restriction of the action space. Such a restriction allows faster decision-making and learning, however, we may potentially ignore more valuable assignments. In the following, we analyze what happens if we learn the assignment decisions explicitly.

## Extending Our Method

We extend our method to allow explicit vehicle assignment decisions. Instead of learning the value of assigning to only the vehicle with the insertion that has the smallest ∆ Vehicle , the NNs learn the values for assignments to all the possible

Table 4: Improvements (%) over π PFA (500 expected homogeneously distributed customers)

| Fleet Size (Veh, Drone)   |   π PFA |   P ( π Q_no_rej ) | P ( π Q )   |
|---------------------------|---------|--------------------|-------------|
| 2, 5                      |   227.6 |                0.3 | 22.0        |
| 2, 10                     |   312.8 |                0.1 | 10.7        |
| 2, 15                     |   391.1 |                0.2 | 6.8         |
| 3, 5                      |   293.4 |                0.4 | 16.7        |
| 3, 10                     |   376.2 |                0.4 | 9.2         |
| 3, 15                     |   460.3 |                0.6 | 3.0         |
| 4, 5                      |   354.7 |                0.2 | 11.0        |
| 4, 10                     |   439.9 |                0.3 | 3.6         |
| 4, 15                     |   499.6 |                0.1 | 0.0*        |

Figure 32: Time vs. travel time vs. decision under π Q\_no\_rej on the selected homogeneous instance,

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000034_0927c9794efd025b7e25ad79f40976854373e66f4506a23e67665778f04c2ade.png)

.

vehicles. Learning such assignments requires us to augment the set of features by incorporating ∆ Vehicle 's for all the vehicles. When the fleet of vehicles can feasibly serve the customer, we also augment the set of actions by having an output node for each of the m vehicles. That is, we learn a state-action value for each vehicle. Note, different from learning individual values, the reward in this experiment is still calculated for the whole fleet.

We train this extended method similar to π Q . The learning process is shown in Figure 34. We observe a slow and constant increase, but a large variation in the learning curve. One explanation is that, as the set of actions is augmented, the dispatcher has more choices when exploring. This leads to more variation and slower learning. As for the number of customers served on the test set, the extended method can eventually serve 409 8 . customers while π Q serves 410 7 . customers. However, the learning curve for the extended method indicates that the learning process has not yet finished after 400,000 training epochs. When running another 100,000 training epochs, we indeed observe the solution quality of the new policy further improves by 2 8% . . This indicates that for our setup, the insertion heuristic performs well, but in cases where more training can be applied, our method should be extended to allow explicit assignment decisions.

## Extending Method from Literature

For a different dynamic routing problem, Chen et al. (2019) implement a method whose features are not dependent on fleet information and can thus be applied to varying fleet sizes. Their work studies a courier dispatching problem by learning the probability of choosing an action on the individual level. In that work, the authors use one NN for all the individual couriers. The request is then assigned to the courier with smallest opportunity cost, the difference in expected number of customers the vehicle will serve in the future without and with the new customer. Thus, this policy is able to consider assignment decisions explicitly.

We adapt the work by Chen et al. (2019) as follows. We use two NNs for the two fleets. Each NN represents the expected number of additional services a vehicle or drone can serve in a state. However, the NNs ignore the remaining setup of the fleet and only focus on the state information of individual vehicles and drones. Thus, the NN for the vehicles only uses the three features time t k , the increase in route duration ∆ Vehicle , and the time the vehicle becomes available again a N ( θ 2 ) . For the NN for the drones, the three features are time t k , distance to the customer d C ( k ) , and drone available time a N ( θ -1 ) .

Figure 33: Average distance of requests vs. assignment decisions under π Q\_no\_rej for homogeneous demand.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000035_e06a642702fc6ecaff56abb7e4e89fa01d8d630a5c419dc4773e95f5f547851f.png)

Figure 34: Extending our method: Learning assignments explicitly.

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000036_ec0619ba7d87cb9d731ff186cbf097201675b7a142e6cd6a2f3adbadc3bf96e7.png)

Every time a customer makes a delivery request, the dispatcher checks the feasibility of serving it for all the vehicles and drones. Then, for each vehicle and drone that can feasibly serve the customer, the dispatcher use the corresponding NN to approximate the difference in the value of offering or not offering the service for the respective vehicle or drone, the opportunity cost. The customer is assigned to the vehicle or drone with the smallest opportunity cost or is not offered service if all opportunity cost are larger than one.

Figure 35: Learning values individually as suggested in Chen et al. (2019)

![Image](0_Artifacts/[chen2022]_Deep_Q-learning_for_same-day_delivery_with_vehicles_and_drones_artifacts/image_000037_0db0543883b67679f33229ddb2b8659f0e1f57278a27a663977ba4157040b28e.png)

In Figure 35, we present the training curve for individual vehicles and drones. Although the curve shows small variance, the number of customers served barely improves in 400,000 steps. The best policy found can serve 372 8 . customers, while our policy π Q learning on fleet level serves 410 7 . customers. The result suggests that, for our problem, it is advantageous to learn the values on the fleet level than on the individual level. The reasons are twofold. First, the

policy based on Chen et al. (2019) ignores the fleet information and focuses only on individual vehicles and drones. Second, the problem at hand is substantially more complex compared to the problem addressed in Chen et al. (2019). Our problem is a pickup and delivery problem with many customers in the system and longer planned routes, while in Chen et al. (2019), no routes are considered and only the movements within a grid are planned.