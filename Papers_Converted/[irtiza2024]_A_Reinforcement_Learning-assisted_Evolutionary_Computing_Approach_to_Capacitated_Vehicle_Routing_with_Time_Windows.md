![Image](image_000000_22550b9d19c8cdc3188a700f0badb5edf8d2efd8e6640eef1f2d18a591ac6b28.png)

## A Reinforcement Learning-Assisted Evolutionary Computing Approach to Capacitated Vehicle Routing with Time Windows

Muhammad Irtiza † School of Computing, Engineering &amp; Mathematical Sciences La Trobe University Melbourne, VIC, Australia m.irtiza@latrobe.edu.au

Rudri Kalaria School of Computing, Engineering &amp; Mathematical Sciences La Trobe University Melbourne, VIC, Australia r.kalaria@latrobe.edu.au

A.S.M. Kayes School of Computing, Engineering &amp; Mathematical Sciences La Trobe University Melbourne, VIC, Australia a.kayes@latrobe.edu.au

## ABSTRACT

This study  proposes  an innovative approach  to address the Capacitated Vehicle Routing Problem with Time Windows (CVRPTW)  by  integrating  Reinforcement  Learning  (RL)  into Evolutionary Algorithms (EAs), forming the Reinforcement Learning-assisted  EA  (RL-EA).  While  traditional  EAs  struggle with  scalability  and  convergence  speed,  RL  offers  promise  in dynamic  decision-making.  By  combining  the  strengths  of  both paradigms, RL-EAs aim to enhance the state-of-the-art solutions for  CVRPTW.  The  proposed  approach  seeks  to  optimise  fleet routes while adhering to capacity and time constraints, crucial for logistics  and  transportation  sectors.  This  study  contributes to  the advancement  of  optimisation  techniques  in  complex  transport scenarios, offering potential improvements in efficiency and costeffectiveness.

## CCS CONCEPTS

- · Computing methodologies → Bio-inspired approaches;
- • Applied computing → Transportation.

## KEYWORDS

Vehicle Routing, Capacitated Vehicle Routing Problem with Time Windows, Evolutionary Computing, Reinforcement Learning, Reinforcement Learning-assisted Evolutionary Computing

## ACM Reference format:

Muhammad Irtiza, Rudri Kalaria, and A.S.M. Kayes. A Reinforcement Learning-assisted Evolutionary Computing Approach to Capacitated Vehicle Routing with Time Windows. In Proceedings of the Genetic and Evolutionary Computation Conference 2024 (GECCO '24), July 14 - 18, 2024, Melbourne, Australia. ACM, New York, NY, USA, 2 pages. https://doi.org/10.1145/3638530.3664049

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Permission to make digital or hard copies of all or part of this work for personal or classroom use is granted without fee provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the full citation on the first page. Copyrights for third-party components of this work must be honored. For all other uses, contact the Owner/Author.

GECCO '24 Companion, July 14-18, 2024, Melbourne, VIC, Australia © 2024 Copyright is held by the owner/author(s). ACM ISBN 979-8-4007-0495-6/24/07. https://doi.org/10.1145/3638530.3664049

## 1 INTRODUCTION

The  vehicle  routing  problem  (VRP)  was  first  introduced  in management science as the 't ruck dispatching p roblem' in 1959 by Dantzig and Ramsar with the objective to find a set of least-cost routes for a fleet of vehicles, satisfying the demands of geographically dispersed customers [1].

The problem has seen a resurgent interest recently due to the recent technological  advancements  such  as  global  positioning  system (GPS), emerging innovative business models such as ridesharing, electronic food delivery and ecommerce, and increasing customer demands such as same-day delivery [2].

Several variants of the problem have been formulated, including the  CVRPTW. The following constraints, in addition to the cost minimisation objective function of classical VRP, are to be satisfied when converging to a solution in this variant:

- 1. The total demand of the customers assigned to a vehicle must not exceed the carrying capacity of the vehicle.
- 2. Each  customer  should  be  served  within  their time window within each vehicle's operational time limits.

The  additional  constraints  in  the  CVRPTW  variant  of  the  VRP result in increased computational complexity.

## 1.1 Solving CVRPTW with EAs

EAs  are  bio-inspired, stochastic, multi-objective optimisation heuristic  algorithms  that  have  shown  promising  results  when applied to vehicle routing problems.

EAs  start  with  a  population  of  potential  solutions,  which  are evaluated against a fitness/objective function. The 'fittest' solutions against the objective function are chosen for replication for the next generation  of  solutions  through  mutation  and/or  recombination until the termination conditions are met.

In the context of VRP, the population consists of a set of unique routes satisfying a series of customers being visited sequentially. The fitness function corresponds to the constraints, which in the case  of  CVRPTW  are  the  capacity  constraints  of  vehicles,  the service time windows for customers, and the total distance and time

travelled by the vehicles. The termination conditions could be an acceptable level of solution quality or a predetermined number of generations.

In the case of complex combinatorial optimisation problems such as CVRPTW, EAs struggle to converge to an optimal solution in practical time since they rely on random mutations and trial-anderror. They also fail to generalize over small changes in inputs and have to be solved from scratch [3].

## 1.2  Machine Learning-Assisted EAs

To address the shortcomings of EAs and improve their performance, machine learning-assisted EAs are an emerging trend in research. Authors in [4] solve the multi-objective split delivery vehicle routing problem with three-dimensional loading constraints (3L-MSDVRP)  using  a  multi-objective  evolutionary  algorithm assisted by offline machine learning (MOEA/D-OM). The results were  comparable  to  existing  algorithms,  but  with  90%  lower computational costs.

Additionally,  RL-EAs  are  showing  promising  performance  in complex combinatorial optimization problems which are sequential in  nature,  utilizing  the  Markov  decision  process  (MDP).  For example,  reinforcement  learning  can  make  adaptive  selection during the iterative search phase by evaluating the performance of heuristic rules [5].

In this study, the authors aim to design an RL-EA model to exploit the sequential nature of the CVRPTW and improve computational methods in this emerging research trend.

## REFERENCES

- [1] G. B. Dantzig, and J. H. Ramser. 1959. The Truck Dispatching Problem. Management Science 6, 1, https://doi.org/10.1287/mnsc.6.1.80
- [2] Brenner Humberto Ojeda Rios, Eduardo C Xavier, Flávio K Miyazawa, Pedro Amorim, Eduardo Curcio, and Maria João Santos. 2021. Recent dynamic vehicle routing problems: A survey. Computers &amp; Industrial Engineering 160 https://doi.org/10.1016/j.cie.2021.107604
- [3] Aigerim Bogyrbayeva, Sungwook Jang, Ankit Shah, Young Jae Jang, and Changhyun Kwon. 2022. A Reinforcement Learning Approach for Rebalancing Electric Vehicle Sharing Systems. IEEE Transactions on Intelligent Transportation Systems 23, 7, https://doi.org/10.1109/TITS.2021.3085217
- [4] Fei Liu, Qingfu Zhang, Qingling Zhu, Xialiang Tong, and Mingxuan Yuan. 2024. Machine Learning Assisted Multiobjective Evolutionary Algorithm for Routing and Packing. IEEE Transactions on Evolutionary Computation https://doi.org/10.1109/TEVC.2024.3357819
- [5] Yanjie Song, Yutong Wu, Yangyang Guo, Ran Yan, Ponnuthurai Nagaratnam Suganthan, Yue Zhang, Witold Pedrycz, Swagatam Das, Rammohan Mallipeddi, and Oladayo Solomon Ajani. 2024. Reinforcement learning-assisted evolutionary algorithm: A survey and research opportunities. Swarm and Evolutionary Computation 86 https://doi.org/10.1016/j.swevo.2024.101517