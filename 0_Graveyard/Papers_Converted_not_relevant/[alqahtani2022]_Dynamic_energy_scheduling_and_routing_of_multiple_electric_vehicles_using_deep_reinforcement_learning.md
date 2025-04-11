See discussions, stats, and author profiles for this publication at: https://www.researchgate.net/publication/356408340

## Dynamic energy scheduling and routing of multiple electric vehicles using deep reinforcement learning

![Image](Papers_Converted/0_Artifacts/[alqahtani2022]_Dynamic_energy_scheduling_and_routing_of_multiple_electric_vehicles_using_deep_reinforcement_learning_artifacts/image_000000_5a1f5897d8ab7b7bf56578e193b1063d9a782503dcc57524a6232846f6d99ba1.png)

Article in Energy · November 2021

DOI: 10.1016/j.energy.2021.122626

CITATIONS

43

READS

429

2 authors , including:

Mohammed Alqahtani

King Khalid University

13

PUBLICATIONS 131 CITATIONS

SEE PROFILE

## Dynamic Energy Scheduling and Routing of Multiple Electric Vehicles using Deep Reinforcement Learning

Mohammed Alqahtani a,b, GLYPH&lt;3&gt; , Mengqi Hu b a Department of Industrial Engineering, King Khalid University, King Fahad St., Guraiger, Abha,

- 62529, Saudi Arabia
- b Department of Mechanical and Industrial Engineering, University of Illinois at Chicago, 842 W Taylor St., Chicago, IL, 60607, U.S.A.

## Abstract

The demand on energy is uncertain and subject to change with time due to several factors including the emergence of new technology, entertainment, divergence of people's consumption habits, changing weather conditions, etc. Moreover, increases in energy demand are growing every day due to increases in world's population and growth of global economy, which substantially increase the chances of disruptions in power supply. This makes the security of power supply a more challenging task especially during seasons (e.g. summer and winter). This paper proposes a reinforcement learning model to address the uncertainties in power supply and demand by dispatching a set of electric vehicles to supply energy to di GLYPH&lt;11&gt; erent consumers at di GLYPH&lt;11&gt; erent locations. An electric vehicle is mounted with various energy resources (e.g., PV panel, energy storage) that share power generation units and storages among di GLYPH&lt;11&gt; erent consumers to power their premises to reduce energy costs. The performance of the reinforcement learning model is assessed under di GLYPH&lt;11&gt; erent configurations of consumers and electric vehicles, and compared to the results from CPLEX and three heuristic algorithms. The simulation results demonstrate that the reinforcement learning algorithm can reduce energy costs up to 22.05%, 22.57%, and 19.33% compared to the genetic algorithm, particle swarm optimization, and artificial fish swarm algorithm results, respectively.

Keywords:

Mobile energy network, Electric vehicle, vehicle routing, Energy scheduling, Deep reinforcement learning

GLYPH&lt;3&gt; Corresponding author:

Email address:

m.alqahtani@kku.edu.sa,

Phone number:

+ 966567585111

October 8, 2021

## 1. Introduction

Power outage is one of the major power issues of the twenty first century due to changes in the world's population, weather conditions, people's life styles, and technology advancement. The U.S.-Canadian blackout of August 14, 2003 is one real world example of power issues caused by the overload on the main power grid and lack of supporting units, which a GLYPH&lt;11&gt; ected nearly 50 million people in both U.S. and Canada [1]. These power issues a GLYPH&lt;11&gt; ect people's security, quality of life, and health [2]. Hence, the need of an immediate response to these power disruptions is imperative to ensure the sustainability of power supply via providing temporary backup energy solutions until the power is restored from the power grid.

To address or mitigate the impact of power supply problems, many studies investigate the causes of energy supply problems and develop plans to increase the resilience of the power grid to ensure sustainability of power supply. Di GLYPH&lt;11&gt; erent solution approaches and new technologies, such as electric vehicle (EV), smart grid, microgrid, and wide area monitoring applications, are developed to provide temporal solutions and enable faster restoration of power services [3]. EVs can charge its battery of energy received from solar and supply it to power grids later [4]. Due to its mobility, EVs can be dispatched to the disrupted areas to supply energy.

With this as our motivation, we will study how to optimally dispatch multiple EVs which equipped with energy storage and photovoltaic (PV) panel to provide energy services for spatially and temporally distributed consumers. To ensure e GLYPH&lt;14&gt; -cient energy dispatching and supply, a simultaneous vehicle routing (VR) and Energy scheduling (ES) problem should be studied. In addition, the energy demands of the consumers may be uncertain and dynamically changed, and the energy productions from renewable energy are highly uncertain. Stochastic Programming (SP) is one of the commonly used algorithms for handling the problems with uncertainties. For example, a SP model for emergency response has been developed in [5] to dispatch first-aid commodities to a GLYPH&lt;11&gt; ected locations during disasters is developed . A multi-objective, robust, SP model is developed in [6] to optimize the preparedness and response of the relief operations. The study in [7] employs SP model to handle the uncertainties in solar energy, electricity prices, and energy load in a cluster of energy sharing providers. SP is used in [8] to find the optimal energy pricing for EV to increase profitability of power aggregation business and customer satisfaction. To improve the e GLYPH&lt;14&gt; ciency of wind farms, a dynamic economic dispatching model using SP and wind speed forecasting is developed in [9].

The literatures above indicate that SP is capable of dealing with uncertainties in optimization problems and yield solutions for energy problems that have multiple

sources of uncertainties. However, it may not be an e GLYPH&lt;14&gt; cient approach to handle the dynamics in ES and VR problem due to its high computational cost [10, 11]. To enable fast decision-making for the dynamic ES and VR problems, we will first reformulate this problem using Markov decision processes (MDP). A large volumes of historical data including energy demand and solar irradiance are collected to train a deep reinforcement learning agent to map the system states (e.g., energy load, solar irradiance, battery energy level, etc.) to optimal actions (e.g., VR, ES). Once completing the training process, the agent can generate near-optimal solutions very fast to handle system dynamics and uncertainties in energy load and radiant energy without solving the problems from scratch. We will compare the performance of the developed reinforcement learning algorithm with exact optimization and heuristic algorithms to demonstrate its e GLYPH&lt;11&gt; ectiveness for fast decisionmaking.

This work is arranged as follows: section 2 discuss relevant research studies. Section 3 shows problem statement and mathematical model formulated as mixed integer programming (MIP) optimization problem. Section 4 shows the reformulation of the MIP model using MDP. The experiments to assess the performance of MDP are illustrated in section 5 and section 6 presents the conclusion.

## 2. Related Works

EV is one of the most commonly used alternatives to provide temporal solutions for power disruptions and work as a secondary power supplying units as they have the feature of vehicle-to-grid (V2G) where EVs can exchange energy with power grids [12], which enable EVs to act as mobile energy storages to give more balance of energy requested from power grids via shifting energy from peak periods to o GLYPH&lt;11&gt; -peak periods. EVs can supply energy to compensate energy shortages from main power grid and maintain energy supply to users [13]. Ref. [14] discusses the integration of the EV-grid including power interaction mode and mainstream dispatching method. Ref. [15] presents a control scheme with renewable energy to reduce the operational expenses for EVs in the distribution network. Ref. [16] discuss the capabilities of V2G to secure power and to provide ancillary services using renewable energy resources.

A two-stage SP model is developed in [17] to reduce energy costs in a community of buildings, EVs, and power networks. A novel management system is developed in [18] to manage and coordinate microgrid which uses plug-in EVs as distributed energy resource (DER). EVs can be e GLYPH&lt;14&gt; ciently incorporated with energy distribution network via providing instantaneous frequency arrangements [19]. A two-stage model is presented in [20] to dispatch energy in o GLYPH&lt;14&gt; ce buildings by implementing EVs as flexible sources of energy. Ref. [21] presented a novel EV

squadron aggregator model to discuss problems in the implementation and scheduling of DER under stochastic conditions and evaluate the contribution of EVs on optimal solutions. Ref. [22] investigates the scheduling operations of RES and develop a model for short time decisions operated in two phases. Renewable energy resources considered a reliable source of clean energy that can be used to supply energy to communities while preserving the environment [23]. Ref. [24] presented operation models that operate under multiple scales to evaluate the performance of power storages at di GLYPH&lt;11&gt; erent time periods. Ref. [25] propose a MIP model to develop an operational strategy for DERs to reduce energy costs and sustain the environment. A MIP model presented in [26] to reduce the annual energy expenses in a district-scale DER by optimizing the operation of power grid. Ref. [27] present a smart energy management system to improve the performance of energy storages in a microgrid. Ref. [28] presented a multi-objective MILP model to reduce total energy costs and environmental pollution in a community of residential and o GLYPH&lt;14&gt; ce buildings. [29, 30] proposed a MILP based mathematical formulation for electric vehicle routing problem (EVRP), which integrates a thorough model for EV energy usage and taking into account time-of-use energy (TOU) prices, and used CPLEX as a solver. There are literatures discussing the uncertainties that stem from unpredictability of energy demand and supply. For example, a risk-constrained SP model is developed in [31] to address the energy procurement issue for large number of users. Although SP and heuristic algorithms can handle the uncertainties in power supply and demand. However, they are computationally expensive [10, 32] and can not provide real-time decisions in power supply network [11]. The dynamic energy system requires instant decision-making process to maintain its operations under various dynamics and uncertainties.

The traditional optimization tools (e.g. SP) can be used to handle the uncertainties in energy systems and yield optimal solutions for ES. However, these tools may not be e GLYPH&lt;14&gt; cient to handle system dynamics for real-time decision-making. Since these tools may take longer time to generate solutions and so may not response to the systems dynamics faster enough compared to reinforcement learning approach, especially in the case of routing problems [33]. Hence, reinforcement learning may be a better alternative to traditional optimization approaches, since it can be trained using real world data to interact more e GLYPH&lt;14&gt; ciently with system dynamics and uncertainties in energy supply and demand to yield faster real-time decisions for the routing and energy dispatching.

Reinforcement learning is widely used to address dynamic decision-making problems under uncertainties. For example, a stochastic optimization framework based on reinforcement learning is developed in [34] to address the uncertainty in energy prices and available wind energy. Uncertainties in tra GLYPH&lt;14&gt; c demand and supply in a stochastic tra GLYPH&lt;14&gt; c network are studied using a MDP formulation, and the

formulated dynamic speed limit problem is solved by a real-time control mechanism [35]. A reinforcement learning algorithm is developed in [36] to solve the VR problem which can provide fast routing decisions without solving the new problem from scratch. Ref [37] propose a model and learning procedure to solve the problem of routing multiple vehicles with heterogeneous capacities. A realtime delivery of products is modeled in [38] in the framework of average-reward reinforcement learning with the context of stochastic demands and multiple vehicles. Ref [39] demonstrate that a reinforcement learning algorithm is able to yield optimal routing actions for autonomous taxis in the city of Singapore. A deep reinforcement learning-based neural combinatorial optimization strategy is presented in [40] to solve VR problems with minimal computation time.

Ref. [41] presented a deep reinforcement learning-based method to address the stochastic and dynamic nature of RES and power electronic devices in modern power systems without system modeling. Ref. [42] proposes a partially observable MDP model to address the stochastic nature in future electricity loads and renewable power generation. A reinforcement learning algorithm for hierarchical automatic generation control is proposed in [43] to achieve multi-objective dynamic optimal allocation of a microgrid in islanded mode. Distributed multi-energy systems optimization problem is addressed in [44] using an alternative approach based on multi-agent reinforcement learning. A reinforcement learning model is proposed in [45] for flexible and cost-e GLYPH&lt;11&gt; ective dispatch of the electricity used by buildings in a smart grid. Ref. [46] proposes a novel energy dispatching approach for real-time scheduling of a microgrid considering the stochastic nature of energy demand, renewable energy, and energy price. A fuzzy Q-learning model is proposed in [47] with the use of renewable resources for modeling hour-ahead electricity market. The dimensionality issues in the dynamic optimization of generation command dispatch for automatic generation control is addressed in [48] using an improved hierarchical reinforcement learning approach. Ref. [49] presents a deep reinforcement learning model using the deep deterministic policy gradient algorithm to control energy transactions of shared energy storage assets within building clusters. An economic dispatch problem is formulated as a multi-stage decision making problem in [50] and is solved using reinforcement learning.

## 3. Mathematical Optimization Model

Section 3.1 discuss the system architecture and components to model the EV network, and Section 3.2 presents the mathematical formulation of the operational decision problem of EV network.

## 3.1. System Configuration for Electric Vehicle Network

In this work, it is assumed that a geographic area is splitted into mutually exclusive and collectively exhaustive squared areas (see Fig. 1) with equal distances (one mile) between each pair of connected regions. It is assumed that each area may contain one consumer or may not contain any consumer, hence the distance between each pair of consumers ranges from one to multiple miles. Fig. 1 demonstrates a system of eight users and four EVs. These users have di GLYPH&lt;11&gt; erent energy load and radiant energy and receive energy from a network of EVs. Each EV is mounted with a photovoltaic panel and power storage. Each EV makes decisions at each time step (e.g., one hour), summing up to a total decision time horizon (e.g., one day).

Figure 1: Demonstration of VR decisions at time t and t + 1

![Image](Papers_Converted/0_Artifacts/[alqahtani2022]_Dynamic_energy_scheduling_and_routing_of_multiple_electric_vehicles_using_deep_reinforcement_learning_artifacts/image_000001_9419b9b9404a4b8623d0a2e8f1166cc10928dfd0d2c2b99cfb6f8086ea666537.png)

In this research, each EV moves around and supply energy to the users at its current area only. In other words, each customer's load can be satisfied using the EV only if it exists at the user's location. Otherwise, customer's load will be satisfied from power grid with an incurred cost. In addition, each EV can charge its energy storage by requesting energy from the power grid at its current location.

## 3.2. Operational Decision Model for Electric Vehicle Network

Tables 1 and 2 describe all the variables and parameters introduced in the model, respectively.

Table 1: Description of decision variables in the presented model

## Objective function

f i

Electricity cost for user in region i ($)

f Net electricity costs for all users ($)

## Variables for power grids

eGp i t Energy requested from power grid at region i at time t (kWh)

## Variables for energy transactions between EV and user

ed t n i ; Electricity discharged from EV n to user i at time t (kWh)

ec n i ; t Electricity charged to EV's battery n from power grid at user i 's location at time t (kWh)

xed t n i ; Indicate whether EV n discharge electricity to user i at time t

xec n i ; t Indicate whether EV n charge electricity from power grid at user i 's region at time t

## Variables for PV panel

ePV n t Electricity output by PV on EV n at time t (kWh)

## Variables for power storage

eB n t State of charge for storage on EV n at time t (kWh)

eBd n t Discharging rate of storage on EV n at time t (kWh)

eBc n t Charging rate of storage on EV n at time t (kWh)

xBd n t Indicates whether storage on EV n is discharging at time t

xBc n t Indicates whether storage on EV n is charging at time t

## Variables for VR

x

n i

;

t

Illustrate if EV n is at at user i 's area at time t

xm n

t

Illustrate if EV n moves to another user's location at time t

## 3.2.1. Objective function

In this paper, the main objective is to reduce energy costs for all consumers via reducing the dependence on the power purchased from the main power grid. Other system costs including design, investment, and configuration costs are disregarded, as this paper focus on managing system operations (i.e., routing and dispatching) for a given system setting. The objective function is calculated as

$$\min f = \sum _ { i } f _ { i }$$

where the energy cost for each user i is computed as

$$\min f _ { i } = \sum _ { t \in T } e G p _ { t } ^ { i } \times P G p _ { t } \ \forall i \in I$$

Eq. (2) shows the sum of the costs for each consumer. The cost for each consumer depends on the amount of energy that cannot be satisfied by the electric vehicle network, and so the consumer will have to purchase it from the power grid at a certain pricing rate.

## Sets

N Total count of EVs referenced by n

I Total count of users referenced by i

T Total count of time steps referenced by t (hr)

## Parameters for power grid

PGpt Unit cost of energy requested from power grid at time t ($ kWh) /

## Parameters for consumers

L i t Energy demand of user i at time t (kWh)

Sol i t Solar irradiance at user i 's location at time t (kWh m 2 ) /

## Parameters for PV panel

S PV n Area of PV for EV n (m 2 )

GLYPH&lt;17&gt; PV Energy production e GLYPH&lt;14&gt;

ciency of PV

## Parameters for power storage

GLYPH&lt;17&gt; Bd E GLYPH&lt;14&gt; ciency of discharging energy from power storage

GLYPH&lt;17&gt; Bc

E GLYPH&lt;14&gt; ciency of charging energy to power storage

S BS n Capacity of power storage on EV n (kWh)

GLYPH&lt;11&gt; Bc Coe GLYPH&lt;14&gt; cient for maximum charging bound of power storage

GLYPH&lt;11&gt; Bd Coe GLYPH&lt;14&gt; cient for maximum discharging bound of power storage

GLYPH&lt;11&gt; Coe GLYPH&lt;14&gt; cient for minimum discharging bound of power storage

Bd

GLYPH&lt;11&gt; Bc Coe GLYPH&lt;14&gt; cient for minimum charging bound of power storage

EOB n Initial state of charge in power storage on EV n (kWh)

## Parameters for EV

X n i ; 0 Illustrate if EV n 's initial area is i

## 3.2.2. Constraints

## Power balance constraint for users

$$\ e G p _ { t } ^ { i } + \sum _ { n \in N } e d _ { t } ^ { n, i } - \sum _ { n \in N } e c _ { t } ^ { n, i } \geq L _ { t } ^ { i } \ \forall i \in I, \forall t$$

The total power supply from all EVs and power grids is shown in the left hand side of Eq. (3), whereas the right hand side corresponds to the energy load of each consumer.

## Energy balance constraint for EVs

$$\ e P V _ { t } ^ { n } + \ e B d _ { t } ^ { n } \times \eta _ { B d } + \sum _ { i \in I } e c _ { t } ^ { n, i } = \frac { \ e B c _ { t } ^ { n } } { \eta _ { B c } } + \sum _ { i \in I } e d _ { t } ^ { n, i } \ \forall n \in N, \forall t \quad \quad ( 4 )$$

Eq. (4) represents the balance constraint for energy supply and demand. Where the amount of power supplied from PV, power storage, and grid at user's location is illustrated in the left hand side. Whereas the amount of electricity received by power storage and users are shown in the right hand side.

## Constraints for photovoltaic panel

$$\ e P V _ { t } ^ { n } \leq S P V ^ { n } \times S o l _ { t } ^ { i } \times x _ { t } ^ { n, i } \times \eta _ { P V } \quad \forall n \in N, \forall t$$

The PV output constraint is illustrated in eq. (5), which is a function of the area of the PV ( S PV n ) and radient energy at user i 's location ( Sol i t ).

## Constraints for energy storage

$$\ x B c _ { t } ^ { n } + \ x B d _ { t } ^ { n } \leq 1 \quad \forall n \in N, \forall t$$

$$S B S ^ { n } \times \alpha _ { B m i n } \leq e B _ { t } ^ { n } \leq S B S ^ { n } \ \forall n \in N, \forall t$$

$$\ e B _ { 1 } ^ { n } = E O B ^ { n } + ( e B c _ { 1 } ^ { n } - e B d _ { 1 } ^ { n } ) \times \Delta t \ \forall n \in N$$

$$\ e B _ { t } ^ { n } = e B _ { t - 1 } ^ { n } + ( e B c _ { t } ^ { n } - e B d _ { t } ^ { n } ) \times \Delta t \ \forall n \in N, \forall t \geq 2$$

$$S B S ^ { n } \times \underline { \alpha } _ { B c } \times x B c _ { t } ^ { n } \leq e B c _ { t } ^ { n } \leq S B S ^ { n } \times \overline { \alpha } _ { B c } \times x B c _ { t } ^ { n } \ \forall n \in N, \forall t \quad ( 1 0 )$$

$$S B S ^ { n } \times \underline { \alpha } _ { B d } \times x B d _ { t } ^ { n } \leq e B d _ { t } ^ { n } \leq S B S ^ { n } \times \overline { \alpha } _ { B d } \times x B d _ { t } ^ { n } \ \forall n \in N, \forall t \quad ( 1 1 )$$

At each time step, energy storage can either supply or receive energy (see Eq. (6)). The state of charge in power storage at any time step must be kept within certain boundaries (see Eq. (7)), and depends on the amount of energy supplied received / by power storage (see Eqs. (8)-(9)), where GLYPH&lt;1&gt; t is the value of time step. The rates of receiving supplying energy from storage cannot fall below the permitted lowest / level and cannot exceed the highest level (see Eqs. (10)-(11)).

## Constraints for electricity transaction among EVs and users

$$\ x e c _ { t } ^ { n, i } + \ x e d _ { t } ^ { n, i } \leq x _ { t } ^ { n, i } \ \forall n \in N, \forall i \in I, \forall t$$

$$\ e c _ { t } ^ { n, i } \leq M \times x e c _ { t } ^ { n, i } \ \forall n \in N, \forall i \in I, \forall t$$

$$\ e d _ { t } ^ { n, i } \leq M \times x e d _ { t } ^ { n, i } \ \forall n \in N, \forall i \in I, \forall t$$

Constraint in eq. (12) ensures that each EV can either supply or receive electricity from consumers' regions at each time step. At any time, if the state variable xec n i ; t = 1 then EV n is able to receive energy from power grid at user i 's location, whereas if the variable xed t n i ; = 1 then EV can supply electricity to consumer i .

## Mobility constraints for EVs

The movements of EV's are modeled using two sets of decision variables: 1) x n i ; t to show whether EV n is at user i 's location at time t , and 2) xm n t to show

whether EV will move at time t . It is assumed that EV at consumer i 's location is allowed to only move one step ahead to its neighboring regions j 2 Ni . Let P j 2 Ni x n j ; t indicate if the EV n will go to its neighboring regions when it is at user i 's location, then the relations between xm n t , x n i ; t and P j 2 Ni x n j ; t are modeled in Eqs. (17)-(20).

$$\sum _ { i \in I } x _ { t } ^ { n, i } = 1 \ \forall n \in N, \forall t$$

$$\sum _ { \substack { n \in N } { x _ { t } ^ { n, i } } } \leq 1 \ \forall i \in I, \forall t$$

$$x _ { t } ^ { n, i } - x _ { t + 1 } ^ { n, i } \leq x m _ { t } ^ { n } \ \forall n \in N, \forall i \in I, \forall t$$

$$x _ { t } ^ { n, i } - \sum _ { j \in N _ { i } } x _ { t + 1 } ^ { n, j } \leq 1 - x m _ { t } ^ { n } \ \forall n \in N, \forall i \in I, \forall t$$

$$X _ { 0 } ^ { n, i } - x _ { 1 } ^ { n, i } \leq x m _ { 1 } ^ { n } \ \forall n, i$$

$$X _ { 0 } ^ { n, i } - \sum _ { j \in N _ { i } } x _ { 1 } ^ { n, j } \leq 1 - x m _ { 1 } ^ { n } \ \forall n, i$$

Constrain in eq. (15) ensure that each EV cannot be located at more than one location at any time step, also no more than one EV can be at the same region at the same time (see Eq. (16)).

## 4. Proposed Reinforcement Learning based Decision Model

In this section, the mathematical model proposed in section 3 will be reformulated using Markov decision processes. A reinforcement learning algorithm is adopted to solve the reformulated problem. The agent is modeled in Section 4.1. The environment model is presented in Section 4.2. The architecture of the deep Q-network to solve the MDP formulation is discussed in Section 4.3.

## 4.1. Electric Vehicle Agent

The EV agent is assumed to be equipped with a PV panel and energy storage unit that move around and supply energy to consumers at various locations to satisfy their energy demand at lower energy costs. The system state s 2 S is a tuple of four variables st = ( pt ; eBt ; Solt ; Lt ) corresponding to vehicle location pt , battery state of charge eBt , solar irradiance Solt , and energy load Lt respectively. The action space a 2 A is defined as the mobility actions (moving up, down, left, right, and no move) and energy transaction actions (charging, discharging, and idle). The movement of each vehicle is restricted to the neighboring regions in an orthogonal movement in a 5 GLYPH&lt;2&gt; 4 grid of regions.

## 4.2. Environment Agent

Given the system state st at time step t , the system state will transit to st + 1 after executing action at following the state transition rules: Once the vehicle chooses which mobility action to take, the location of the vehicle pt is updated to pt + 1. Given the vehicle's location at the next state, the energy load Lt and solar irradiance Solt are updated to next state load Lt + 1 and solar irradiance Solt + 1. After that, based on the ES action taken (charge, discharge, idle) at current state, the battery state of charge variable eBt is updated to the next state battery level eBt + 1, and output from PV will be calculated based on the solar irradiance at next state Solt + 1. The state of charge at next state ( eBt + 1) can be computed in Eqs. (8)-(9), given the state of charge at current state ( eBt ). Whereas the location of the vehicle at next state ( pt + 1) can be obtained from routing constraints in Eqs. (15)-(20). The solar irradiance and load at next state ( Solt + 1 and Lt + 1) are obtained from the sequence of the solar irradiance and load data.

The reward function is defined as following:

$$R ( s _ { t }, a _ { t } ) = \sum _ { i \in I } e G p _ { t } ^ { i } \times P G p _ { t }$$

where PGpt is the price of energy from power grid. The energy costs can be obtained from Eq. (21), as it represents the amount of unsatisfied demand by EVs, but can be satisfied by the main power grid at that region. eGp i t represents the energy requested from the power grid and can be computed as following:

$$\ e G p _ { t } ^ { i } = \max \left ( L _ { t } ^ { i } - \sum _ { n \in N } e d _ { t } ^ { n, i } + \sum _ { n \in N } e c _ { t } ^ { n, i }, 0 \right ) \ \forall i \in I, \forall t$$

The EV energy balance constraints in Eq. (4) is automatically satisfied based on the definition of system states and actions. The first energy storage constraint (Eq. (6)) is satisfied based on the definition of the battery actions, so that at each time t , the battery can only take one action (charge, discharge, or idle). The battery capacity constraint in Eq. (7) is included in the definition of the storage parameters, so that at each time step, the level in each battery is maintained between certain boundaries. The constraint in Eq. (8) is satisfied during model initialization. For example, at time t = 1, a certain amount of energy is stored in the battery. The constraint in Eq. (9) is satisfied based on the state transition rule. The amount of energy transactions between power grid and battery shown in Eqs. (10)-(11) are maintained between certain boundaries (rates of charging and discharging). These constraints are satisfied by calculating the amount of energy received or supplied by the battery at each time step, such that they do not fall below lower limit or exceed upper limit of charging discharging rates, respectively. /

The energy exchange between EVs and consumers is modeled according to Eqs. (12-14). Eq. (12) is satisfied based on the definition of action space. At each time step, each EV can either receive energy from power grid or supply energy to consumer. Constraints in Eqs. (13-14) is handled by setting an upper and lower limits for the amount of energy exchange between EVs and consumers to a fixed amount.

Finally, the mobility constraint in Eq. (15) is automatically satisfied. Constraint in Eq. (16) is satisfied by adding a condition for each region to have no more than one EV at each time t . The mobility of vehicles at each time step shown in Eq. (17) and Eq. (19) are considered in the definition of the action space, such as each EV will make decisions about whether to move to surrounding regions or stay at the current region. The locations of EV at next time step shown in Eq. (18) and Eq. (20) are satisfied based on the definition of action space, which restricts the mobility to only the neighboring regions of each vehicle.

Figure 2: Proposed deep Q-network architecture

![Image](Papers_Converted/0_Artifacts/[alqahtani2022]_Dynamic_energy_scheduling_and_routing_of_multiple_electric_vehicles_using_deep_reinforcement_learning_artifacts/image_000002_5c9181240d196ca134f267065e5b1fa398e0a09cb815c43ac8b38544b2a54811.png)

## 4.3. Deep Q-Network Architecture

To solve the reformulated decision problem, we adopt deep Q-network (DQN) to estimate the Q-value for each action by the EV agent. It takes the state variables including vehicle's location, battery level, consumer load, and solar irradiance at

consumer's locations ( st = ( pt ; eBt ; Solt ; Lt )) and the routing and scheduling actions ( at ) as input, and pass them thought hidden layer and output layer of the network to approximate Q-values corresponding to each combination (routing and scheduling) of agent's actions. The architecture of the proposed DQN is illustrated in Fig. 2.

## 5. Simulation Results Analysis

In this research, we assume there are twenty energy users located at di GLYPH&lt;11&gt; erent locations in the Chicago area. 24hr data for energy demand profiles for these users at the weather zone of Chicago are collected [51]. The solar irradiance profile used in this paper is taken from the city of Chicago in year 2010 [52]. Prices of electricity purchased from the power grid is taken from [53]. The energy consumption due to mobility for EV (2019 Tesla Model X) is taken from [54]. All the other parameters used in the experiments in this paper are taken from [51, 55, 56]. The mathematical programming model proposed in Section 3 is solved using CPLEX with a relative gap of 0.005. The CPLEX algorithm is considered as an exact optimizer to provide optimal solution for benchmarking.

To compare the performance of the presented DQN model, three heuristic solutions including genetic algorithm, particle swarm, and artificial fish swarm algorithms are studied. Each EV is assumed to go to the four neighboring locations (users) at each time step. The DQN is trained using one year energy load and solar irradiance data for 1,000 episodes. We evaluate each algorithm on 100 problem instances.

## 5.1. Scalability Evaluation

## 5.1.1. Energy network with four EVs and twenty consumers

Table 3: Simulation results for energy network with four EVs and twenty consumers

| Algorithm   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Decision   |
|-------------|------------------------|------------------------|------------------------|------------------------|------------|
| Algorithm   | Mean                   | Std. dev.              | Min                    | Max                    | time (sec) |
| PG          | 573.38                 | 12.78                  | 354.31                 | 613.54                 | -          |
| CPLEX       | 435.44                 | 20.85                  | 231.13                 | 488.37                 | 41         |
| DQN         | 467.13                 | 10.35                  | 291.46                 | 495.84                 | 2          |
| GA          | 535.14                 | 19.06                  | 336.86                 | 585.80                 | 68         |
| PSO         | 537.54                 | 19.06                  | 339.26                 | 588.20                 | 17         |
| AFSA        | 523.46                 | 17.00                  | 334.69                 | 568.79                 | 215        |

In this section, the performance of DQN algorithm is evaluated using a case of four EVs and twenty consumers. The operational costs and decision time per problem instance for five algorithms (CPLEX, DQN, and three heuristic algorithms) are shown in Table 3. These algorithms are compared with a power grid only algorithm (PG), where there are no prosumers in the network, which acts as a baseline of solution quality for this case. It is observed that the DQN algorithm can obtain better solutions compared to baseline and heuristic algorithms, with an average cost reduction of 21.31%. In addition, the DQN algorithm can obtain near-optimal solution very fast compared to other algorithms. The convergence curve for DQN during training is demonstrated in Fig. 3.

Figure 3: Convergence curve for the DQN during training

![Image](Papers_Converted/0_Artifacts/[alqahtani2022]_Dynamic_energy_scheduling_and_routing_of_multiple_electric_vehicles_using_deep_reinforcement_learning_artifacts/image_000003_b2c736fec19da4970963f0ccd982c7d61ed779cda1fd180786f39c2f8ec74868.png)

## 5.1.2. Energy network with eight EVs and twenty consumers

In this section, the performance of DQN algorithm is evaluated using a case of eight EVs and twenty consumers. The operational costs and decision time per problem instance for four algorithms (DQN and three heuristic algorithms) are shown in Table 4. CPLEX is computationally expensive for this case, so we do not report CPLEX result here.

It is observed that the DQN algorithm can generate better solutions compared to heuristic algorithms, with average cost reduction of 19.53%. It further demonstrates the computational e GLYPH&lt;14&gt; ciency of DQN algorithm for fast decision-making.

## 5.1.3. Energy network with twelve EVs and forty consumers

In this section, the performance of DQN algorithm is evaluated using a case of twelve EVs and forty consumers. The operational costs and decision time per

Table 4: Simulation results for energy network with eight EVs and twenty consumers

| Algorithm   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Decision   |
|-------------|------------------------|------------------------|------------------------|------------------------|------------|
| Algorithm   | Mean                   | Std. dev.              | Min                    | Max                    | time (sec) |
| DQN         | 418.55                 | 7.99                   | 285.85                 | 442.99                 | 6          |
| GA          | 498.75                 | 25.38                  | 321.81                 | 560.45                 | 81         |
| PSO         | 501.15                 | 25.38                  | 324.21                 | 562.85                 | 30         |
| AFSA        | 503.55                 | 25.38                  | 326.61                 | 565.25                 | 230        |

problem instance for four algorithms (DQN and three heuristic algorithms) are shown in Table 5.

Table 5: Simulation results for energy network with twelve EVs and forty consumers

| Algorithm   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Decision   |
|-------------|------------------------|------------------------|------------------------|------------------------|------------|
| Algorithm   | Mean                   | Std. dev.              | Min                    | Max                    | time (sec) |
| DQN         | 821.03                 | 18.64                  | 514.47                 | 877.41                 | 13         |
| GA          | 962.14                 | 44.27                  | 586.29                 | 1074.14                | 125        |
| PSO         | 966.93                 | 44.28                  | 591.08                 | 1078.93                | 44         |
| AFSA        | 969.33                 | 44.28                  | 593.48                 | 1081.33                | 246        |

It is observed that the DQN algorithm can yield better solutions compared to heuristic algorithms, with average cost reduction of 17.51%. It is observed that the DQN algorithm can output solutions that are better in terms of energy cost compared to the heuristic algorithms. Moreover, DQN algorithm outperforms the heuristic algorithms in terms of the simulation time.

## 5.1.4. Energy network with sixteen EVs and forty consumers

In this section, the performance of DQN algorithm is evaluated using a case of sixteen EVs and forty consumers. The operational costs and decision time per problem instance for four algorithms (DQN and three heuristic algorithms) are shown in Table 6.

Table 6: Simulation results for energy network with sixteen EVs and forty consumers

| Algorithm   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Decision   |
|-------------|------------------------|------------------------|------------------------|------------------------|------------|
| Algorithm   | Mean                   | Std. dev.              | Min                    | Max                    | time (sec) |
| DQN         | 606.03                 | 14.98                  | 345.50                 | 657.37                 | 23         |
| GA          | 923.48                 | 49.62                  | 571.23                 | 1048.75                | 149        |
| PSO         | 928.25                 | 49.62                  | 576.02                 | 1053.55                | 55         |
| AFSA        | 930.65                 | 49.62                  | 578.42                 | 1055.95                | 279        |

The DQN algorithm can generate better solutions, with average cost reduction of 52.78%. DQN also outperforms other three heuristic algorithms in terms of computational time.

## 5.1.5. Summary of scalability evaluation

Fig. 4 represents the simulation time for these four energy networks. It is observed that GA and AFSA algorithms show quadratic or parabolic patterns, whereas PSO shows linear pattern as the problem scale increases. The DQN's simulation time increases in a linear pattern with a smaller slope compared to PSO, which makes it be more appropriate for fast decision-making to quickly respond to system dynamics.

Figure 4: Simulation time for four di GLYPH&lt;11&gt; erent energy networks

![Image](Papers_Converted/0_Artifacts/[alqahtani2022]_Dynamic_energy_scheduling_and_routing_of_multiple_electric_vehicles_using_deep_reinforcement_learning_artifacts/image_000004_42d41fcb66c855b11fd1e0ad44d2a91c1cc0c413348ad7fefc68d849b18c98a7.png)

In summary, we use t-test to compare the five algorithms in terms of solution quality and computational time. Table 7 summarizes the results of t-test for each case. The first symbol in ' +/+ ' indicates DQN algorithm significantly outperforms the compared algorithm in terms of solution quality, and the second symbol indicates DQN algorithm significantly outperforms the compared algorithm in terms of computational time. From Table 7, we can conclude that DQN algorithms significantly outperforms the three heuristic algorithms in terms of both solution quality and computational time.

Fig. 5 illustrate the routing decisions of EVs that supply energy to consumers in di GLYPH&lt;11&gt; erent regions using DQN algorithm along with other algorithms at critical time steps (hour 11, 12, 13, and 15), respectively. For demonstration purpose, one EV for each algorithm is used to show the routing decisions of an EV at critical time steps for twenty consumers. The total energy costs of each algorithm at each time step are shown to demonstrate how the routing decisions a GLYPH&lt;11&gt; ect the energy costs of multiple consumers in the network. It is observed that the CPLEX and DQN algorithms outperform the heuristic algorithms in terms of total energy costs

Table 7: Summary of t-test results to compare DQN with other algorithms

| Algorithm   | t-test   | t-test   | t-test   | t-test   |
|-------------|----------|----------|----------|----------|
| Algorithm   | Case 1   | Case 2   | Case 3   | Case 4   |
| CPLEX       | - /+     |          |          |          |
| GA          | +/+      | +/+      | +/+      | +/+      |
| PSO         | +/+      | +/+      | +/+      | +/+      |
| AFSA        | +/+      | +/+      | +/+      | +/+      |

at critical time steps (hour 11, 12, 13, and 15). Both CPLEX and DQN approaches show better capability to exploit solar energy to satisfy energy demands (better ES decisions) compared to the heuristic algorithms. Further, the CPLEX and DQN algorithms show better routing decisions compared to heuristic algorithms, since they choose locations where energy loads at these regions and their neighboring regions are higher for consecutive time steps to incur higher net energy savings.

## 5.2. Robustness Evaluation

In this section, the DQN algorithm is examined under the case of variations in load and solar irradiance data from one region to another due to di GLYPH&lt;11&gt; erent consumption patterns among consumers and di GLYPH&lt;11&gt; erent weather conditions among di GLYPH&lt;11&gt; erent locations. The DQN algorithm is compared to the CPLEX and heuristic algorithms to determine the impact of variability in input data on solution quality and simulation time.

## 5.2.1. Variability in energy load

Table 8: Simulation results for energy network with di GLYPH&lt;11&gt; erent load patterns

| Algorithm   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   |
|-------------|------------------------|------------------------|------------------------|------------------------|
| Algorithm   | Mean                   | Std. dev.              | Min                    | Max                    |
| CPLEX       | 661.41                 | 45.77                  | 327.63                 | 714.80                 |
| DQN         | 687.53                 | 32.44                  | 416.85                 | 721.61                 |
| GA          | 729.41                 | 44.78                  | 401.37                 | 782.18                 |
| PSO         | 741.41                 | 44.78                  | 413.37                 | 794.18                 |
| AFSA        | 711.65                 | 43.17                  | 413.42                 | 781.32                 |

In this section, the performance of di GLYPH&lt;11&gt; erent algorithms are compared under di GLYPH&lt;11&gt; erent patters of energy load. The operational costs for five algorithms (CPLEX, DQN, and three heuristic algorithms) are shown in Table 8.

Figure 5: Routing decisions of EVs using di GLYPH&lt;11&gt; erent algorithms at critical time steps

![Image](Papers_Converted/0_Artifacts/[alqahtani2022]_Dynamic_energy_scheduling_and_routing_of_multiple_electric_vehicles_using_deep_reinforcement_learning_artifacts/image_000005_78cc9c5f4621d7187d995b8c93b8b74c812b491685efbc81aa70231efa2a3729.png)

The results from Table 8 show that the DQN algorithm is more robust considering variability in energy load compared to the CPLEX and heuristic algorithms (GA, PSO, and AFSA) since it has the lowest standard deviation.

## 5.2.2. Variability in solar irradiance

In this section, the performance of five algorithms are compared under di GLYPH&lt;11&gt; erent patters of solar irradiance. The operational costs for five algorithms (CPLEX, DQN, and three heuristic algorithms) are shown in Table 9.

Table 9: Simulation results for energy network with di GLYPH&lt;11&gt; erent solar irradiance patterns

| Algorithm   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   | Operational cost ($)   |
|-------------|------------------------|------------------------|------------------------|------------------------|
| Algorithm   | Mean                   | Std. dev.              | Min                    | Max                    |
| CPLEX       | 415.26                 | 11.04                  | 400.09                 | 451.14                 |
| DQN         | 441.90                 | 9.58                   | 429.82                 | 465.11                 |
| GA          | 444.48                 | 10.80                  | 430.08                 | 476.33                 |
| PSO         | 450.48                 | 10.80                  | 436.08                 | 482.33                 |
| AFSA        | 508.39                 | 34.47                  | 435.20                 | 556.16                 |

The results from Table 9 show that the DQN algorithm is more robust considering variability in solar irradiance compared to the CPLEX and heuristic algorithms (GA, PSO, and AFSA) since it has the lowest standard deviation.

## 6. Conclusion and future work

In this research, a network of EVs is studied which can be dispatched to supply energy to a set of consumers in di GLYPH&lt;11&gt; erent locations. A reinforcement learning model is proposed to allow multiple EVs to operate under uncertainties of energy loads and solar irradiance at di GLYPH&lt;11&gt; erent locations. Two case studies are designed to evaluate scalability and robustness of the proposed reinforcement learning model. The results show that the reinforcement learning model can better handle changes and problem scales compared to heuristic algorithms since it can reduce the simulation time by 93.87%, 96.30%, 84.77%, and 98.83% compare to the CPLEX, GA, PSO, and AFSA, respectively. Further, reinforcement learning algorithm outperforms the three heuristic algorithms (GA, PSO, and AFSA) and can achieve a reduction in energy costs up to 22.05%, 22.57%, and 19.33% compared to GA, PSO, and AFSA, respectively. Moreover, the reinforcement learning algorithm shows better robustness to variability in load and solar irradiance compared to the CPLEX and heuristic algorithms. In the future, a multi-agent reinforcement learning algorithm will be proposed where decisions can be distributed among EVs so that they can take actions of mobility and energy transaction in a way that is independent to other EVs.

## Acknowledgement

We would like to express our gratitude to King Khalid University for providing the financial support and scholarship to the main author of this research. And we would like to thank the National Science Foundation for supporting this research under grant number CMMI 1634738.

## References

- [1] G¨ oran Andersson, Peter Donalek, Richard Farmer, Nikos Hatziargyriou, Innocent Kamwa, Prabhashankar Kundur, Nelson Martins, John Paserba, Pouyan Pourbeik, Juan Sanchez-Gasca, et al. Causes of the 2003 major grid blackouts in north america and europe, and recommended means to improve system dynamic performance. IEEE transactions on Power Systems , 20(4):1922-1928, 2005.
- [2] Chaamala Klinger and Virginia Murray Owen Landeg. Power outages, extreme events and health: a systematic review of the literature from 2011-2012. PLoS currents , 6, 2014.
- [3] Yezhou Wang, Chen Chen, Jianhui Wang, and Ross Baldick. Research on resilience of power systems under natural disasters-a review. IEEE Transactions on Power Systems , 31(2):1604-1613, 2015.
- [4] Henrik Lund and Willett Kempton. Integration of renewable energy into the transport and electricity sectors through v2g. Energy policy , 36(9):35783587, 2008.
- [5] Gulay Barbarosoˇ glu and Yasemin Arda. A two-stage stochastic programming framework for transportation planning in disaster response. Journal of the operational research society , 55(1):43-53, 2004.
- [6] Ali Bozorgi-Amiri, Mohamad Saeed Jabalameli, and SMJ Mirzapour Al-e Hashem. A multi-objective robust stochastic programming model for disaster relief logistics under uncertainty. OR spectrum , 35(4):905-933, 2013.
- [7] Nian Liu, Minyang Cheng, Xinghuo Yu, Jiangxia Zhong, and Jinyong Lei. Energy-sharing provider for pv prosumer clusters: A hybrid approach using stochastic programming and stackelberg game. IEEE Transactions on Industrial Electronics , 65(8):6740-6750, 2018.

| [8]   | Jo˜o a Soares, Mohammad Ali Fotouhi Ghazvini, Nuno Borges, and Zita Vale. Dynamic electricity pricing for electric vehicles using stochastic program- ming. Energy , 122:111-127, 2017.                                                                                                              |
|-------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [9]   | YZ Sun, Jun Wu, GJ Li, and Jian He. Dynamic economic dispatch consider- ing wind power penetration based on wind speed forecasting and stochastic programming. Proceedings of the CSEE , 29(4):41-47, 2009.                                                                                          |
| [10]  | David Abramson and J Abela. A parallel genetic algorithm for solving the school timetabling problem . Citeseer, 1991.                                                                                                                                                                                |
| [11]  | Suvrajeet Sen. Stochastic programming: computational issues and chal- lenges. Encyclopedia of ORMS / , pages 1-11, 2001.                                                                                                                                                                             |
| [12]  | Christophe Guille and George Gross. A conceptual framework for the vehicle-to-grid (v2g) implementation. Energy policy , 37(11):4379-4390, 2009.                                                                                                                                                     |
| [13]  | Willett Kempton and Jasna Tomi´. c Vehicle-to-grid power implementation: From stabilizing the grid to supporting large-scale renewable energy. Journal of power sources , 144(1):280-294, 2005.                                                                                                      |
| [14]  | Yanchong Zheng, Songyan Niu, Yitong Shang, Ziyun Shao, and Linni Jian. Integrating plug-in electric vehicles into power grids: A comprehensive re- view on power interaction mode, scheduling methodology and mathematical foundation. Renewable and Sustainable Energy Reviews , 112:424-439, 2019. |
| [15]  | Shuang Gao, KT Chau, Chunhua Liu, Diyun Wu, and Ching Chuen Chan. Integrated energy management of plug-in electric vehicles in power grid with renewables. IEEE Transactions on Vehicular Technology , 63(7):3019-3027, 2014.                                                                        |
| [16]  | Sayed Saeed Hosseini, Ali Badri, and Masood Parvania. A survey on mo- bile energy storage systems (mess): Applications, challenges and solutions. Renewable and Sustainable Energy Reviews , 40:161-170, 2014.                                                                                       |
| [17]  | Md Abdul Quddus, Omid Shahvari, Mohammad Marufuzzaman, John M Usher, and Raed Jaradat. A collaborative energy sharing optimization model among electric vehicle charging stations, commercial buildings, and power grid. Applied Energy , 229:841-857, 2018.                                         |
| [18]  | Taha Selim Ustun, Umit Cali, and Mithat C Kisacikoglu. Energizing micro- grids with electric vehicles during emergencies-natural disasters, sabotage                                                                                                                                                 |

|      | and warfare. In 2015 IEEE International Telecommunications Energy Con- ference (INTELEC) , pages 1-6. IEEE, 2015.                                                                                                                                                                    |
|------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [19] | Willett Kempton, Victor Udo, Ken Huber, Kevin Komara, Steve Letendre, Scott Baker, Doug Brunner, and Nat Pearre. A test of vehicle-to-grid (v2g) for energy storage and frequency regulation in the pjm system. Results from an Industry-University Research Partnership , 32, 2008. |
| [20] | Xiaolong Jin, Jianzhong Wu, Yunfei Mu, Mingshen Wang, Xiandong Xu, and Hongjie Jia. Hierarchical microgrid energy management in an o GLYPH<14> ce building. Applied Energy , 208:480-494, 2017.                                                                                      |
| [21] | Goncalo Cardoso, Michael Stadler, Mohammed C Bozchalui, Ratnesh Sharma, Chris Marnay, Ana Barbosa-P´voa, o and Paulo Ferr˜o. a Optimal in- vestment and scheduling of distributed energy resources with uncertainty in electric vehicle driving schedules. Energy , 64:17-30, 2014.  |
| [22] | G Guti´rrez-Alcaraz, e E Galv´n, a N Gonz´lez-Cabrera, a and MS Javadi. Re- newable energy resources short-term scheduling and dynamic network recon- figuration for sustainable energy consumption. Renewable and Sustainable Energy Reviews , 52:256-264, 2015.                    |
| [23] | AG Tsikalakis and ND Hatziargyriou. Environmental benefits of distributed generation with and without emissions trading. Energy Policy , 35(6):3395- 3409, 2007.                                                                                                                     |
| [24] | Wai Shin Ho, Sandro Macchietto, Jeng Shiun Lim, Haslenda Hashim, Za- rina Ab Muis, and Wen Hui Liu. Optimal scheduling of energy storage for renewable energy distributed energy generation system. Renewable and Sus- tainable Energy Reviews , 58:1100-1107, 2016.                 |
| [25] | Jeremy Jie Ming Kwok, Nan Yu, Iftekhar A Karimi, and Dong-Yup Lee. Microgrid scheduling for reliable, cost-e GLYPH<11> ective, and environmentally friendly energy management. Industrial & Engineering Chemistry Research , 52(1):142-151, 2013.                                    |
| [26] | Yun Yang, Shijie Zhang, and Yunhan Xiao. An milp (mixed integer linear programming) model for optimal design of district-scale distributed energy resource systems. Energy , 90:1901-1915, 2015.                                                                                     |
| [27] | Changsong Chen, Shanxu Duan, Tong Cai, Bangyin Liu, and Gangwei Hu. Smart energy management system for optimal microgrid economic operation. IET renewable power generation , 5(3):258-267, 2011.                                                                                    |

| [28]   | Longxi Li, Hailin Mu, Nan Li, and Miao Li. Economic and environmen- tal optimization for distributed energy resource systems coupled with district energy networks. Energy , 109:947-960, 2016.                                                                            |
|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [29]   | Giulio Ferro, Massimo Paolucci, and Michela Robba. Optimal charging and routing of electric vehicles with power constraints and time-of-use energy prices. IEEE Transactions on Vehicular Technology , 69(12):14436-14447, 2020.                                           |
| [30]   | G Ferro, M Paolucci, and M Robba. An optimization model for electrical vehicles routing with time of use energy pricing and partial recharging. IFAC- PapersOnLine , 51(9):212-217, 2018.                                                                                  |
| [31]   | Miguel Carri´n, o Andy B Philpott, Antonio J Conejo, and Jos M Arroyo. A stochastic programming approach to electric energy procurement for large consumers. IEEE Transactions on Power Systems , 22(2):744-754, 2007.                                                     |
| [32]   | Dian Palupi Rini, Siti Mariyam Shamsuddin, and Siti Sophiyati Yuhaniz. Par- ticle swarm optimization: technique, system and challenges. International journal of computer applications , 14(1):19-26, 2011.                                                                |
| [33]   | Penghao Sun, Yuxiang Hu, Julong Lan, Le Tian, and Min Chen. Tide: Time- relevant deep reinforcement learning for routing optimization. Future Gener- ation Computer Systems , 99:401-409, 2019.                                                                            |
| [34]   | Jose L Crespo-Vazquez, C Carrillo, E Diaz-Dorado, Jose A Martinez- Lorenzo, and Md Noor-E-Alam. A machine learning based stochastic op- timization framework for a wind and storage power plant participating in en- ergy pool market. Applied energy , 232:341-357, 2018. |
| [35]   | Feng Zhu and Satish V Ukkusuri. Accounting for dynamic speed limit con- trol in a stochastic tra GLYPH<14> c environment: A reinforcement learning approach. Transportation research part C: emerging technologies , 41:30-47, 2014.                                       |
| [36]   | Mohammadreza Nazari, Afshin Oroojlooy, Lawrence Snyder, and Martin Tak´c. a Reinforcement learning for solving the vehicle routing problem. In Advances in Neural Information Processing Systems , pages 9839-9849, 2018.                                                  |
| [37]   | Jos´ e Manuel Vera and Andres GAbad. Deep reinforcement learning for rout- ing a heterogeneous fleet of vehicles. In 2019 IEEE Latin American Confer- ence on Computational Intelligence (LA-CCI) , pages 1-6. IEEE, 2019.                                                 |

| [38]   | Scott Proper, Prasad Tadepalli, Hong Tang, and Rasaratnam Logendran. A reinforcement learning approach for product delivery by multiple vehicles. In IIE Annual Conference. Proceedings , page 1. Institute of Industrial and Systems Engineers (IISE), 2003.                          |
|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [39]   | Miyoung Han, Pierre Senellart, St´phane e Bressan, and Huayu Wu. Routing an autonomous taxi with reinforcement learning. In Proceedings of the 25th ACM International on Conference on Information and Knowledge Manage- ment , pages 2421-2424, 2016.                                 |
| [40]   | JQ James, Wen Yu, and Jiatao Gu. Online vehicle routing with neural com- binatorial optimization and deep reinforcement learning. IEEE Transactions on Intelligent Transportation Systems , 20(10):3806-3817, 2019.                                                                    |
| [41]   | Jiajun Duan, Haifeng Li, Xiaohu Zhang, Ruisheng Diao, Bei Zhang, Di Shi, Xiao Lu, Zhiwei Wang, and Siqi Wang. Adeep reinforcement learning based approach for optimal active power dispatch. In 2019 IEEE Sustainable Power and Energy Conference (iSPEC) , pages 263-267. IEEE, 2019. |
| [42]   | Lei Lei, Yue Tan, Glenn Dahlenburg, Wei Xiang, and Kan Zheng. Dynamic energy dispatch in isolated microgrids based on deep reinforcement learning. arXiv preprint arXiv:2002.02581 , 2020.                                                                                             |
| [43]   | Lingxiao Gan, Tao Yu, Jing Li, and Jie Tang. Smart scheduling strategy for islanded microgrid based on reinforcement learning algorithm. Int. J. Smart Grid and Clean Energy , 1(1):122-128, 2012.                                                                                     |
| [44]   | Lynn A Bollinger and Ralph Evins. Multi-agent reinforcement learning for optimizing technology deployment in distributed multi-energy systems. In 23rd International Workshop of the European Group for Intelligent Comput- ing in Engineering. Krakow, Poland , 2016.                 |
| [45]   | Sunyong Kim and Hyuk Lim. Reinforcement learning based energy manage- ment algorithm for smart energy buildings. Energies , 11(8):2010, 2018.                                                                                                                                          |
| [46]   | Ying Ji, Jianhui Wang, Jiacan Xu, Xiaoke Fang, and Huaguang Zhang. Real- time energy management of a microgrid using deep reinforcement learning. Energies , 12(12):2291, 2019.                                                                                                        |
| [47]   | Mohammad Reza Salehizadeh and Salman Soltaniyan. Application of fuzzy q-learning for electricity market modeling by considering renewable power penetration. Renewable and Sustainable Energy Reviews , 56:1172-1181,                                                                  |

View publication stats

| [48]   | T Yu, YMWang, WJ Ye, B Zhou, and KWChan. Stochastic optimal genera- tion command dispatch based on improved hierarchical reinforcement learn- ing approach. IET generation, transmission & distribution , 5(8):789-797, 2011.                                                                                                         |
|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [49]   | Philip Odonkor and Kemper Lewis. Control of shared energy storage as- sets within building clusters using reinforcement learning. In International Design Engineering Technical Conferences and Computers and Information in Engineering Conference , volume 51753, page V02AT03A028. American Society of Mechanical Engineers, 2018. |
| [50]   | EAJasmin, TP Imthias Ahamed, and VPJagathiraj. Areinforcement learning algorithm to economic dispatch considering transmission losses. In TENCON 2008-2008 IEEE Region 10 Conference , pages 1-6. IEEE, 2008.                                                                                                                         |
| [51]   | Yang Chen and Mengqi Hu. Balancing collective and individual interests in transactive energy management of interconnected micro-grid clusters. En- ergy , 109:1075-1085, 2016.                                                                                                                                                        |
| [52]   | National Renewable Energy Laboratory.. National solar radiation data base. http: // rredc : nrel : gov / solar / old data / nsrdb / . Accessed: 2018-09-06.                                                                                                                                                                           |
| [53]   | Arizona.. Salt river project utility company. https: // www : srpnet : com / prices / home / tou : aspx. Accessed: 2018-09-06.                                                                                                                                                                                                        |
| [54]   | U.S. Department of Energy. The o GLYPH<14> cial U.S. government source for fuel economy information. https: // www : fueleconomy : gov / feg / PowerSearch : do?action = noform&path = 1&year1 = 1984&year2 = 2019&vtype = Electric&pageno = 1&sortBy = Comb&tabView = 0&rowLimit = 10. Accessed: 2018-09-06.                         |
| [55]   | Rui Dai, Mengqi Hu, Dong Yang, and Yang Chen. A collaborative operation decision model for distributed building clusters. Energy , 84:759-773, 2015.                                                                                                                                                                                  |
| [56]   | Mohammed Alqahtani and Mengqi Hu. Integrated energy scheduling and routing for a network of mobile prosumers. Energy , 200:117451, 2020.                                                                                                                                                                                              |