# Aco-evolutionary genetic algorithm with knowledge transfer for multi-objective capacitated vehicle routing problems

## Metadata
- **Link to PDF**: [[[wang2023b]_A_co-evolutionary_genetic_algorithm_with_knowledge_transfer_for_multi-objective_capacitated_vehicle_routing_problems.pdf]]
- **Tags**:  
  #CVRP  
  #MultiObjective  
  #ML-AssistedHeuristics  
  #PopulationBasedEvolution  
  #GeneticAlgorithm  
  #HybridLocalSearch  
  #DeterministicVRP  
  #LargeScaleVRP  
  #TransferLearning
  #Autoencoder
  #CoEvolution  
- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Co-evolutionary approach for MO-CVRP
  - Knowledge transfer from TSP to CVRP
  - Denoising Autoencoder for domain mapping
  - Multi-objective genetic algorithm
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Class ‘‘M’’ and Class ‘‘X’’ CVRP test instances

## Abstract
“The capacitated vehicle routing problem (CVRP) aims to find the routes that minimize transportation costs. However, in real-world applications, the equitable workload is essential for the drivers and logistics companies. For assigning an appropriate load for each vehicle, this paper formulates the CVRP with workload balancing as a multi-objective optimization problem (MO-CVRP). The goal is to simultaneously minimize the total length of routes and the variance of loads assigned to each vehicle. Although several multi-objective evolutionary algorithms (MOEAs) have been proposed for the MO-CVRP, they often suffer from slow convergence speeds due to the complexity of the search space, which hinders the identification of clear search directions. To address this issue, the knowledge transfer method is introduced that can leverage search experiences from related problems to expedite the solution process of the current problem. Specifically, a co-evolutionary genetic algorithm with knowledge transfer is proposed, where a simplified version of the traveling salesman problem (TSP) by disregarding the vehicle capacity constraint is utilized to facilitate the quick solution of the original MOCVRP. To enable information interaction between these two problems, the widely used denoising autoencoder is adopted to construct a transformation relation between solutions from the TSP to the MO-CVRP. Additionally, an adaptive migration strategy is proposed to adaptively select the migration methods and an improved split operator strategy is designed to ensure the feasibility of the solutions. The proposed algorithm is compared with four state-of-the-art MOEAs on MO-CVRP test sets with different scales. Experimental results show that the proposed algorithm exhibits faster convergence speed and achieves solutions with superior convergence.”

## Summary
- **Problem & Objective**  
  - Addresses a **deterministic multi-objective CVRP** variant emphasizing both total route distance and load variance (workload balancing).  
  - Proposes a new approach to improve convergence speed and solution quality when tackling this multi-objective CVRP.

- **Key Methodology**  
  - Introduces a **co-evolutionary genetic algorithm** that runs two populations simultaneously:  
    1. A **simplified TSP** (removing capacity constraints) for fast exploration.  
    2. The original, more complex MO-CVRP.  
  - Uses **knowledge transfer** between the two populations so that promising TSP solutions can guide the CVRP population.  
  - Adopts a **denoising autoencoder (DAE)** to map TSP solutions into feasible CVRP solutions (a domain-transformation approach).  
    - The DAE is trained with pairs of TSP routes (input) and CVRP routes (output).  
    - After training, TSP solutions are mapped (decoded) into CVRP solutions to accelerate search in the primary population.  
  - Proposes an **adaptive migration strategy**, automatically deciding whether to transfer solutions via DAE-based transformation or direct copy from TSP to CVRP, based on the success (survival) rate of newly introduced individuals.  
  - Includes a **modified Split operator** for post-processing the newly mapped solutions, ensuring capacity feasibility and fostering load balancing.

- **Experiments & Results**  
  - Evaluated on two standard CVRP benchmark groups: Class ‘‘M’’ and Class ‘‘X’’.  
  - Compared against four state-of-the-art multi-objective algorithms (MOEA, M-MOEA/D, KBEA, and CCMO).  
  - Achieved superior hypervolume (HV) values on most test cases, demonstrating both **faster convergence** and **better solution diversity**.  
  - The co-evolution framework and DAE-based knowledge transfer produced notable improvements over standard evolutionary approaches for multi-objective routing.

- **Strengths**  
  - Demonstrates how simpler VRP-like tasks (TSP in this case) can transfer partial solutions or structural knowledge to guide a more complex CVRP.  
  - The DAE mapping strategy is a relatively novel method for bridging solution spaces across related problem domains.  
  - Explicitly addresses multi-objective aspects (distance minimization + load balancing), which is directly relevant for fair workload distribution in logistics.

- **Limitations & Future Work**  
  - The training of the DAE model adds computational overhead; for large problem instances, this can increase run times.  
  - Each problem size requires re-training or adjusting the DAE structure. A single universal model might improve scalability.  
  - Authors suggest extending the knowledge-transfer approach to more CVRP variants (e.g., time windows, stochastic demands).  

- **Relevance to Deterministic VRP**  
  - Demonstrates a purely deterministic capacity constraint with balanced load objective.  
  - Fits well within the scope of **ML-assisted heuristic** frameworks for multi-objective VRPs, offering a robust reference for knowledge-transfer methods.  
  - Potentially impactful for large-scale route planning where partial solutions from simpler subproblems can speed up search.