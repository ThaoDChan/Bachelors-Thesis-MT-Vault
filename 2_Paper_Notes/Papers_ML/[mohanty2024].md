# Solving the vehicle routing problem via quantum support vector machines

## Metadata
- **Link to PDF**: [[[mohanty2024] Solving the vehicle routing problem via quantum support vector machines.pdf]]
- **Tags**:  
  #ML-Supervised  
  #ML-Hybrid  
  #DeterministicVRP  
  #QuantumComputingVRP  
  #IsingModel  
  #QUBO  
  #QAOA  
  #VQE  
- **Relevant**: true  
- **Fit Score**: 8
- **State of the Art (SoA) Concepts**:
  - Quantum support vector machines (QSVM)
  - Encoding strategies (amplitude, angle, higher-order, IQP)
  - Ising model mapping for VRP
  - Hybrid quantum-classical optimization (VQE, QAOA)
- **Performance Evaluation**: no  
- **Performance Evaluation Framework**: -

## Abstract
"The vehicle routing problem (VRP) is an example of a combinatorial optimization problem that has attracted academic attention due to its potential use in various contexts. VRP aims to arrange vehicle deliveries to several sites in the most efficient and economical manner possible. Quantum machine learning offers a new way to obtain solutions by harnessing the natural speedups of quantum effects, although many solutions and methodologies are modified using classical tools to provide excellent approximations of the VRP. In this paper, we employ 6 and 12 qubit circuits, respectively, to build and evaluate a hybrid quantum machine learning approach for solving VRP of 3- and 4-city scenarios. The approach employs quantum support vector machines (QSVMs) trained using a variational quantum eigensolver on a static or dynamic ansatz. Different encoding strategies are used in the experiment to transform the VRP formulation into a QSVM and solve it. Multiple optimizers from the IBM Qiskit framework are also evaluated and compared."

## Summary
- **Overall Goal and Contribution**  
  - Proposes a *hybrid quantum-classical* approach to solve the deterministic Vehicle Routing Problem (VRP).  
  - Uses a *Quantum Support Vector Machine (QSVM)*, trained via a *Variational Quantum Eigensolver (VQE)*, to optimize routes.  
  - Demonstrates how encoding schemes (amplitude, angle, higher-order, IQP) can embed VRP data into quantum states.  
  - Compares the results across different circuit depths, encoding strategies, and classical optimizers.

- **VRP Formulation and Mapping**  
  - The authors first map the VRP onto an Ising model/QUBO framework, a common step for quantum optimization methods.  
  - They define binary decision variables \(x_{ij}\) (1 if the route takes edge \(i \to j\), 0 otherwise), and then convert them into spin variables \(\pm 1\).  
  - The VRP cost function is incorporated into a Hamiltonian whose ground state corresponds to an optimal or near-optimal route.  
  - They rely on standard VRP constraints (each location visited exactly once, return to depot, sub-tour elimination) but do not introduce stochastic or time-window features, ensuring the problem remains deterministic.

- **Quantum SVM (QSVM) and Variational Optimization**  
  - Unlike a purely classical SVM that performs kernel-based classification, they adapt the SVM idea to a *quantum feature map* (QSVM).  
  - The quantum kernel is derived from fidelity measures between quantum states.  
  - A hybrid loop is used: a *quantum computer* encodes the data (the VRP cost structure) and measures expectation values, while a *classical optimizer* (COBYLA, SLSQP, or L-BFGS-B) iteratively updates circuit parameters (the VQE part).  
  - The system attempts to learn a separation boundary—analogous to the VRP solution margin—through iterative energy minimization.

- **Encoding Strategies**  
  - **Amplitude Encoding (AE)**: Embeds classical data into amplitudes of a quantum state. Offers high accuracy but requires large gate counts, limiting scale to 6 qubits in practice.  
  - **Angle Encoding (AgE)**: Uses rotation gates (R\_y, R\_z) parametrized by VRP data. Less gate-intensive, feasible up to 12 qubits in their experiments, showing stable accuracy.  
  - **Higher-Order Encoding (HO)**: Extends angle encoding by adding entangling operations with multi-layered rotation. Moderate accuracy but can become complex for deeper circuits.  
  - **IQP Encoding (IqpE)**: Inserts multiple layers of Hadamard and ZZ rotations, creating intricate entanglement patterns. Found to have lower accuracy in VRP tasks, possibly due to more complex superposition states.

- **Experiment Design and Results**  
  - VRP instances:  
    - 3-city and 4-city routing tasks, implemented as 6-qubit and 12-qubit circuits, respectively.  
    - Evaluated each encoding scheme with both a *fixed Hamiltonian* (single set of cost/constraints) and a *variable Hamiltonian* (slight changes in cost parameters).  
  - Repeated each scenario for up to 50 simulation runs, measuring how often the method reached or approached the “classical minimum.”  
  - Findings:  
    - **Amplitude Encoding**: Very high accuracy (often near 100%) for 6 qubits, but not extended to 12 qubits due to large gate depth and hardware limitations.  
    - **Angle Encoding**: High accuracy (near 90–100%) on both 6- and 12-qubit setups; more scalable than amplitude encoding.  
    - **Higher-Order Encoding**: Good performance but less stable than angle or amplitude encoding when circuit depth or qubits increased.  
    - **IQP Encoding**: Poorer accuracy and higher error rates.  
    - **Optimizers**: COBYLA generally outperformed SLSQP and L-BFGS-B in consistency, although all can find feasible solutions.

- **Complexity and Cost**  
  - QSVM-based approach *reduces* the need for multiple QAOA layers or complicated mixer Hamiltonians: the encoding does the heavy lifting of creating superposition.  
  - However, circuit complexity can increase substantially (especially in amplitude encoding), leading to practical limits on the number of qubits.  
  - The authors note a trade-off: deeper circuits or more complex encodings can degrade solution reliability, potentially from quantum noise or hardware resource constraints.

- **Conclusions and Relevance**  
  - The paper validates that a *machine-learning-based quantum approach* can solve small-scale deterministic VRPs and approach the known classical optimal solutions.  
  - Demonstrates the synergy of QSVM + VQE as an alternative to typical quantum optimization algorithms (like standard QAOA).  
  - Performance indicates angle encoding combined with COBYLA is a promising approach for near-term quantum hardware in route optimization tasks.  
  - While the paper does not benchmark standard VRP sets, it highlights directions for quantum ML, especially for future larger-scale VRP cases.

- **Strengths, Weaknesses, and Positioning**  
  - *Strengths*: Presents a novel QSVM-based approach; compares multiple encodings and optimizers; systematically reports success rates under fixed and variable Hamiltonians.  
  - *Weaknesses*: Current experiments limited to at most 4 cities (12 qubits) due to hardware and simulation constraints; does not adopt standard VRP benchmark datasets.  
  - *Positioning*: Provides evidence of how deterministic VRP can be tackled by quantum ML solutions, relevant for research bridging classical combinatorial optimization and quantum machine learning.  
  - *Implications for Thesis*: Adds a distinctive perspective on “hybrid supervised approaches” for VRP. Though not a mainstream method due to quantum hardware limitations, it expands the literature on deterministic VRP tackled with advanced ML paradigms.