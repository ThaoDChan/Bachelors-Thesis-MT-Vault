![Image](image_000000_22550b9d19c8cdc3188a700f0badb5edf8d2efd8e6640eef1f2d18a591ac6b28.png)

## Efficient Vehicle Routing Problem: A Machine Learning and Evolutionary Computation Approach

## Pratyay Mukherjee

Ramanathan A Indian Institute of Technology

## Sawvranu Dey

pratyaymukherjee9@gmail.com Indian Institute of Technology Bombay Mumbai, Maharashtra, India ramosvamos2900@gmail.com Bombay Mumbai, Maharashtra, India

sawvranu212001@gmail.com Indian Institute of Technology Bombay Mumbai, Maharashtra, India

## ABSTRACT

The Vehicle Routing Problem with Time Windows (VRPTW) is an extension of VRP that introduces time window constraints to the routing optimization process. Scaling Evolutionary Computation algorithms for VRPTW to handle large-scale problems poses significant challenges. Machine Learning assisted Evolutionary Computation strategy have been proposed to enhance optimization algorithms' efficiency and effectiveness. This study proposes a machine-learning model that exploits the graphical nature of VRP to design and improve evolutionary computational methods. The aim is to improve the resilience and efficiency of VRPTW optimization and provide better-quality solutions for practical applications.

can improve the performance of logistics and transportation systems, resulting in economic and social benefits.

## CCS CONCEPTS

The Vehicle Routing Problem with Time Windows (VRPTW) extends the classical VRP by introducing time window constraints to the routing optimization process. In addition to the pre-existing capacity constraint, this variant requires vehicles to adhere to specified time frames when serving customers. Arriving before the time window opens is permitted, but service can only commence once the window opens. Conversely, it is not permissible for a vehicle to arrive after the time window has closed, making the problem even more challenging. The VRP is a combinatorial optimization problem belonging to the NP-hard class. Finding an optimal solution to VRP is computationally intractable for large instances and requires significant computational resources.

· Theory of computation → Evolutionary algorithms ; · Computing methodologies → Machine learning approaches .

## KEYWORDS

Vehicle Routing Problems with Time Window(VRPTW), Evolutionary Computation(EC), Graph Neural Network(GNN)

## ACMReference Format:

Pratyay Mukherjee, Ramanathan A, and Sawvranu Dey. 2023. Efficient Vehicle Routing Problem: A Machine Learning and Evolutionary Computation Approach. In Genetic and Evolutionary Computation Conference Companion (GECCO '23 Companion), July 15-19, 2023, Lisbon, Portugal. ACM, New York, NY, USA, 2 pages. https://doi.org/10.1145/3583133.3596425

## 1 INTRODUCTION

The Vehicle Routing Problem (VRP) addresses the challenge of efficiently allocating a fleet of vehicles to serve customers with distinct service requirements. VRP aims to determine a set of routes that comprehensively covers all customer demands while minimizing the associated costs. Optimizing vehicle routing can lead to substantial cost savings, reduced travel time, and enhanced customer service. By studying VRP, researchers and practitioners can enhance optimization algorithms for more effective and efficient problem-solving. Real-world implementation of these algorithms

Permission to make digital or hard copies of part or all of this work for personal or classroom use is granted without fee provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the full citation on the first page. Copyrights for third-party components of this work must be honored. For all other uses, contact the owner/author(s).

GECCO '23 Companion, July 15-19, 2023, Lisbon, Portugal

ACM ISBN 979-8-4007-0120-7/23/07.

© 2023 Copyright held by the owner/author(s).

https://doi.org/10.1145/3583133.3596425

## 1.1 Solving VRPTW with Evolutionary Computation

Various methods can be employed to solve the VRPTW. For smallerscale problems with relatively straightforward constraints, exact methods such as Mixed Integer Programming can be effectively employed. However, for larger-scale problems with complex constraints, heuristic methods such as local search or Meta-heuristics are often more effective. While these methods may not guarantee an optimal solution, they provide solutions close to optimal within a reasonable time frame.

Applying Evolutionary Computing (EC) to the Vehicle Routing Problem with Time Windows (VRPTW) presents several challenges. VRPTW is a complex combinatorial optimization problem, and choosing an appropriate representation suitable for EC is difficult. The search space of VRPTW can be vast, and EC algorithms need

Evolutionary algorithms are metaheuristic optimization algorithms inspired by natural selection. In the case of the vehicle routing problem with time windows (VRPTW), each individual in the population corresponds to a unique set of routes taken by the vehicles and the sequence in which customers are visited. An individual's fitness is determined by how well it satisfies the VRPTW's constraints, including capacity and time window constraints, and howefficiently it minimizes the total distance or time traveled by vehicles. The evolutionary algorithm progresses through generations, with new individuals created through mutation by remove-insert operations applied to the fittest individuals from the previous generation. Generating new individuals continues until a termination criterion is met, such as a predetermined maximum number of generations or an acceptable level of solution quality. Finally, the best individual from the final generation is selected as the solution to the VRPTW.

help to search such a large space efficiently. Furthermore, vehicle capacity and time window constraints make it challenging to ensure that candidate solutions satisfy these conditions when using EC algorithms that rely on random mutations.

## 1.2 Machine Learning Assisted Evolutionary Computation

To address these issues, researchers have investigated Machine Learning assisted Evolutionary Computation (MLEC) strategies for solving VRPTW to increase optimization algorithms' efficiency and effectiveness. Machine learning approaches can aid the evolutionary process by forecasting the fitness of candidate solutions and finding promising individuals for future evolution, resulting in higherquality solutions and shorter convergence times.

Graphical neural networks (GNNs) are a class of deep learning models that are designed to operate on graph-structured data. GNNs can be used to learn node embeddings that capture the topological and structural properties of the graph. Using these embeddings, GNNs can perform various tasks, such as node classification and link prediction.

This study aims to create a machine-learning model to exploit the graphical nature of VRP that can design and improve evolutionary computational methods. In this regard, the problem can be represented as a graph, where nodes represent customers and edges represent the routes between them. The strengths of graphical neural networks and their variants can be used to analyze and optimize the solution.

## REFERENCES

- [1] Ruibin Bai, Xinan Chen, Zhi-Long Chen, Tianxiang Cui, Shuhui Gong, Wentao He, Xiaoping Jiang, Huan Jin, Jiahuan Jin, Graham Kendall, Jiawei Li, Zheng Lu, Jianfeng Ren, Paul Weng, Ning Xue, and Huayan Zhang. 2021. Analytics and Machine Learning in Vehicle Routing Research. arXiv:2102.10012 [cs.LG]
- [3] Uroš Klanšek and Mirko Pšunder. 2010. Solving the nonlinear transportation problem by global optimization. Transport 25, 3 (2010), 314-324. https://doi.org/ 10.3846/transport.2010.39 arXiv:https://doi.org/10.3846/transport.2010.39
- [2] Nasser A. El-Sherbeny. 2010. Vehicle routing with time windows: An overview of exact, heuristic and metaheuristic methods. Journal of King Saud University Science 22, 3 (2010), 123-131. https://doi.org/10.1016/j.jksus.2010.03.002
- [4] Wouter Kool, Herke Van Hoof, and Max Welling. 2018. Attention, Learn to Solve Routing Problems! 7th International Conference on Learning Representations, ICLR 2019 (3 2018). https://arxiv.org/abs/1803.08475v3
- [5] Petar Veličković, Arantxa Casanova, Pietro Liò, Guillem Cucurull, Adriana Romero, and Yoshua Bengio. 2017. Graph Attention Networks. 6th International Conference on Learning Representations, ICLR 2018 - Conference Track Proceedings (10 2017). https://doi.org/10.1007/978-3-031-01587-8\_7