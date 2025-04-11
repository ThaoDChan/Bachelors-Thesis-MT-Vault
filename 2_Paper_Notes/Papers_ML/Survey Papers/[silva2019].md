# A reinforcement learning-based multi-agent framework applied for solving routing and scheduling problems

## Metadata
- **Link to PDF**: [[[silva2019]_A_reinforcement_learning-based_multi-agent_framework_applied_for_solving_routing_and_scheduling_problems.pdf]]
- **Tags**:
  - #RL-ValueBased
  - #RL-Qlearning
  - #MDP
  - #ReinforcementLearning
  - #MultiAgentSystem
  - #ML-AssistedHeuristics
  - #HybridLocalSearch
  - #LocalSearchHeuristics
  - #DeterministicVRP
  - #VRPTW
  - #HeuristicOrExactBlends
  - #Metaheuristics
- **Relevant**: true
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Multi-agent Q-learning framework for VRPTW
  - Reinforcement learning combined with metaheuristics
  - Parallel and cooperative search among agents
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Solomon’s VRPTW benchmark

## Abstract
“This article presents a multi-agent framework for optimization using metaheuristics, called AMAM. In this proposal, each agent acts independently in the search space of a combinatorial optimization problem. Agents share information and collaborate with each other through the environment. The goal is to enable the agent to modify their actions based on experiences gained in interacting with the other agents and the environment using the concepts of Reinforcement Learning. For better introduction and validation of the AMAM framework, this article uses the instantiation of the Vehicle Routing Problem with Time Windows (VRPTW) and the Unrelated Parallel Machine Scheduling Problem with Sequence-Dependent Setup Times (UPMSP-ST), i.e., two classic combinatorial optimization problems. The main objective of the experiments is to evaluate the performance of the proposed adaptive agents. The experiments confirm that the ability to learn attributed to the agent directly influences the quality of solutions, both from the individual point of view and from the point of view of teamwork. In this way, the framework presented here is a step forward in relation to the other frameworks of the literature regarding to the adaptation to the particular aspects of the problems. Additionally, the cooperation between agents and their ability to influence the quality of the solutions of the agents involved in the search of the solution is confirmed. The results also strengthen the issue of the scalability of the framework, since, with the addition of new agents, there is an improvement of the solutions obtained.”

## Summary
- **Purpose & Scope**  
  - Proposes AMAM, a multi-agent framework that combines metaheuristics with reinforcement learning for complex combinatorial optimization problems.
  - Targets two deterministic problems: Vehicle Routing Problem with Time Windows (VRPTW) and Unrelated Parallel Machine Scheduling with sequence-dependent setup times.
  - Emphasizes the capacity to adapt local search strategies via Q-learning while sharing solution states in a cooperative multi-agent environment.

- **Core Approach**  
  - Each agent independently runs a metaheuristic (Iterated Local Search, VND-based local refinements) on a candidate solution and interacts with a shared solution pool.
  - A Q-learning scheme is embedded to adapt the ordering and selection of local search neighborhoods, treating each neighborhood operator as a “state” and transitions as “actions.”
  - The environment (search space + solution pool) provides the reward signal based on improvements in solution quality.
  - Agents cooperate by continually posting and accessing solutions in a shared pool, seeking collectively to improve solution quality and promote diversity.

- **VRPTW Variant & Deterministic Setting**  
  - Uses the classical Solomon benchmark for VRPTW (deterministic time windows, known customer demands, static data).
  - Distinguishes routes primarily by minimizing the number of vehicles and secondarily by reducing travel distance. 
  - Q-learning is used to automatically prioritize or combine multiple neighborhood operators such as Swap, Shift, and Route-Elimination.

- **Experimental Findings**  
  - Comprehensive experiments on Solomon’s VRPTW instances show that the Q-learning-based adaptive approach outperforms a previous learning-automata local search in both solution quality and speed of convergence.
  - Cooperative scenarios (multiple agents in parallel) yield improved solutions versus a single-agent approach, highlighting the framework’s scalability.
  - For VRPTW, the method is competitive with best-known solutions in many instances, particularly when multiple agents collaborate.

- **Strengths**  
  - Reinforcement learning integration: allows dynamic, data-driven selection of neighborhood operators, avoiding manual tuning of local search sequences.
  - Multi-agent design: fosters parallel exploration, information sharing, and synergy among distinct metaheuristic agents.
  - Maintains solution diversity: a solution pool insertion criterion emphasizes keeping distinct solutions to avoid premature convergence.

- **Weaknesses & Limitations**  
  - Requires careful parameter setting (learning rate, discount factor, etc.) to ensure stable convergence.
  - VRPTW scope limited to a subset of standard instances (100-customer Solomon sets), and deeper large-scale VRP performance is not extensively tested.
  - Method also covers scheduling problems, but the proposed RL scheme might need further adaptation for other VRP variants.

- **Relevance to Deterministic VRP Research**  
  - Demonstrates how multi-agent reinforcement learning can address combinatorial routing under static, deterministic constraints.
  - Provides insights into Q-learning as a local search driver for VRPTW.
  - Offers a scalable, hybrid method suitable for comparing with other RL-based or metaheuristic-based solutions in deterministic VRPs.

- **Conclusion & Outlook**  
  - Highlights that Q-learning-driven local search improves both single-agent and multi-agent VRP solutions.
  - Emphasizes cooperative memory structures and agent autonomy as a way to systematically hybridize metaheuristics.
  - Encourages further exploration of reinforcement signals, reward shaping, and multi-agent communication protocols for large-scale or additional deterministic VRP variants.