# Analysis of the Vehicle Routing Problem Solved via Hybrid Quantum Algorithms in the Presence of Noisy Channels

## Metadata
- **Link to PDF**: [[[mohanty2023] Analysis of the Vehicle Routing Problem Solved via Hybrid Quantum Algorithms in the Presence of Noisy Channels.pdf]]
- **Tags**:  
  #DeterministicVRP 
  #MultiVehicleRouting 
  #QuantumComputingVRP 
  #ML-Hybrid 
  #AlgorithmSelection 
  #VQE 
  #QuantumNoise 
  #AQC 
  #IsingModel 
  #QUBO
- **Relevant**: true  
- **Fit Score**: 5  
- **State of the Art (SoA) Concepts**:
  - Hybrid quantum-classical optimization algorithms (VQE, QAOA frameworks)
  - Modeling VRP via Ising/QUBO formulations
  - Noise channel analysis in quantum computing
- **Performance Evaluation**: no  
- **Performance Evaluation Framework**: -

## Abstract
"The vehicle routing problem (VRP) is an NP-hard optimization problem that has been an interest of research for decades in science and industry. The objective is to plan routes of vehicles to deliver goods to a fixed number of customers with optimal efficiency. Classical tools and methods provide good approximations to reach the optimal global solution. Quantum computing and quantum machine learning provide a new approach to solving combinatorial optimization of problems faster due to inherent speedups of quantum effects. Many solutions of VRP are offered across different quantum computing platforms using hybrid algorithms, such as quantum approximate optimization algorithm and quadratic unconstrained binary optimization. In this work, we build a basic VRP solver for three and four cities using the variational quantum eigensolver on a fixed ansatz. The work is further extended to evaluate the robustness of the solution in several examples of noisy quantum channels. We find that the performance of the quantum algorithm depends heavily on what noise model is used. In general, noise is detrimental, but not equally so among different noise sources."

## Summary
- **Problem & Motivation**  
  - Addresses the Vehicle Routing Problem (VRP), a well-known NP-hard problem, using a **quantum-classical hybrid approach**.  
  - Investigates whether quantum solvers (VQE-based) can reliably find optimal or near-optimal solutions for small VRP instances and how different quantum noise channels affect solution quality.

- **Methodology**  
  - Formulates VRP as an **Ising Hamiltonian** and, equivalently, a QUBO model suitable for gate-based quantum computation.  
  - Implements a **Variational Quantum Eigensolver (VQE)** approach to solve the VRP, constructing circuits for three- and four-city instances (requiring six and twelve qubits, respectively).  
  - Tests different classical optimizers (COBYLA, SPSA, L-BFGS-B, SLSQP) that update the parameterized quantum circuit to minimize the VRP cost Hamiltonian.  
  - Introduces and analyzes **noise channels** (amplitude damping, bit-flip, phase-flip, bit-phase-flip, depolarizing) applied at varying noise probabilities (0.05–0.5).  
  - Evaluates the deviation of the final quantum energy (objective value) from the exact classical minimum to quantify how noise degrades solution accuracy.

- **Key Results**  
  - In a **noise-free** simulation, VQE can reach or closely approximate the classical optimum, especially for the smaller six-qubit case.  
  - With noise injected, performance worsens, but the magnitude depends heavily on the type of noise channel:
    - **Phase-flip** and **depolarizing noise** cause the most significant degradation, pushing solutions away from optimal values.
    - **Bit-flip** and **bit-phase-flip** noise lead to moderate deviations, with deeper circuit layers sometimes exacerbating the errors.
    - **Amplitude damping** impacts results moderately, though small or single-layer circuits can still yield near-optimal solutions if the noise probability is low.  
  - The COBYLA optimizer shows consistently strong performance relative to other optimizers in the presence of noise.  
  - Contrary to the standard intuition that deeper circuits improve variational solvers, higher-layer circuits here can become more vulnerable to noise accumulation, thereby reducing solution quality.

- **Strengths, Weaknesses, and Limitations**  
  - **Strengths**:
    - Offers a systematic gate-based simulation for small VRPs, clarifying how quantum noise channels affect solution accuracy.
    - Compares multiple classical optimizers within a unified quantum framework (VQE).  
  - **Weaknesses**:
    - Restricted to very small VRP instances (3–4 cities) due to the exponential overhead in simulating gate-based circuits.  
    - Does not leverage large-scale or standard VRP benchmarks, so scalability and real-world viability remain unclear.  
  - **Limitations**:
    - Noise effects are studied in isolation (individual channels), without exploring combined or time-correlated noise.
    - The paper does not incorporate capacity constraints or time windows, limiting direct application to more general VRP variants.

- **Relevance to Deterministic VRP and ML**  
  - Focuses on a **deterministic VRP** setting with a fixed set of cities and no random demands.  
  - Employs **hybrid quantum-classical** optimization, a specialized technique which can be viewed as a form of advanced algorithmic approach overlapping with machine learning concepts (variational ansatz + classical parameter updates).  
  - Illustrates how state-of-the-art quantum methods may eventually complement or replace purely classical ML/heuristic approaches for small VRPs, though they are currently limited by hardware noise and scalability.

- **Implications & Future Work**  
  - Indicates that near-term quantum devices face challenges from noise, undermining the advantage of deeper circuit depths.  
  - Suggests further exploration of error mitigation techniques and specialized ansatz designs to handle noise more effectively.  
  - Calls for extended studies with larger VRP instances, integrated capacity/time constraints, and advanced classical-quantum co-optimization strategies.