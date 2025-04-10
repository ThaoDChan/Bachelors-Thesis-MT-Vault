Received 29 March 2023; revised 6 July 2023; accepted 2 August 2023; date of publication 10 August 2023; date of current version 7 September 2023.

Digital Object Identifier 10.1109/TQE.2023.3303989

## Analysis of the Vehicle Routing Problem Solved via Hybrid Quantum Algorithms in the Presence of Noisy Channels

## NISHIKANTA MOHANTY 1 , BIKASH K. BEHERA 2 , AND CHRISTOPHER FERRIE 3

1 Centre for Quantum Software and Information, University of Technology Sydney, Sydney, NSW 2007, Australia 2 Bikash's Quantum (OPC) Pvt. Ltd., Mohanpur 741246, India

3 Centre for Quantum Software and Information, University of Technology Sydney, Sydney, NSW 2007, Australia

Corresponding author: Nishikanta Mohanty (e-mail: nishikanta.m.mohanty@student.uts.edu.au).

This work was supported by the Centre for Quantum Software and Information, University of Technology Sydney and Sydney Quantum Academy.

ABSTRACT The vehicle routing problem (VRP) is an NP-hard optimization problem that has been an interest of research for decades in science and industry. The objective is to plan routes of vehicles to deliver goods to a fixed number of customers with optimal efficiency. Classical tools and methods provide good approximations to reach the optimal global solution. Quantum computing and quantum machine learning provide a new approach to solving combinatorial optimization of problems faster due to inherent speedups of quantum effects. Many solutions of VRP are offered across different quantum computing platforms using hybrid algorithms, such as quantum approximate optimization algorithm and quadratic unconstrained binary optimization. In this work, we build a basic VRP solver for three and four cities using the variational quantum eigensolver on a fixed ansatz. The work is further extended to evaluate the robustness of the solution in several examples of noisy quantum channels. We find that the performance of the quantum algorithm depends heavily on what noise model is used. In general, noise is detrimental, but not equally so among different noise sources.

INDEX TERMS Combinatorial optimization (CO), Ising model, quantum noise channels, variational quantum eigensolver (VQE).

## I. INTRODUCTION

Quantum computers, the next generation of computing technology, are expected to solve complex optimization problems much faster than their traditional counterparts. As parallelism is quantum computing's most notable benefit [1], [2], it is only natural to turn to quantum computing to speed up calculations in complicated optimization problems (such as those described by an quantum approximate optimization algorithm (QAOA) [3], adiabatic computation (AC) [4], Grover's algorithm [5], and others). When applied to a multidimensional problem, classical optimization techniques in machine learning (ML) can take a long time to calculate global optimum and consume a lot of CPU and GPU power [6]. In higher dimensional problem spaces, classical algorithms have been shown to be less effective in general [7]. This is due to the fact that NP-hard optimization tasks are often assigned to ML algorithms [6].

VRPcomesunderthecategory of routing problems that try to address multiple issues related to fleet management [8]. The objective is always to optimize vehicle movement to minimize the cost or maximize the profit. Notwithstanding the difficulty in delivering quick and dependable solutions to the computationally hard VRP problem, several precise and heuristic techniques have been developed for solving it [8], [9]. Describing the VRP in its simplest form, a single vehicle is tasked to deliver goods at multiple customer locations; also, the vehicle needs to return to pick up additional items when it runs out of goods [10]. The goal is to minimize the cost of service by finding the best feasible combination of routes that begin and terminate at a central location (the depot) while maximizing the reward (often the inverse of the total distance traveled or the mean service time). This problem is computationally difficult to solve, even with only a few hundred customer nodes, and is classified as an NP-hard problem [6], [11].

VOLUME 4, 2023

![Image](image_000000_514b90654a44040311b794954f001bd318481424dae017045b716666b97b45db.png)

![Image](image_000001_57656f605441bc85056633c29a82b3290c270830d811866ba718e9a49e92f670.png)

Any VRP ( n k , ) involves ( n -1) locations with k vehicles and a depot D [8], [12]. Its solution is a set of routes in which all k vehicles begin and terminate at the given depot D , ensuring that each place is visited only once. The route with the shortest sum total of distance traveled by k vehicles is the ideal one. For a long time, VRP is studied as an extension of the classical traveling salesman problem [9], [13], where now a group of k salesmen has to service collectively ( n -1) locations, such that each location is serviced exactly once [8]. Constraints, such as vehicle capacity or restricted covering time, often complicate the VRP issue in practical settings. As a result, a plethora of conventional and quantum approaches have been presented in an effort to effectively solve the problem. Current quantum approaches for solving optimization problems include QAOA [3], quadratic unconstrained binary optimization (QUBO) [14], [15], quantum annealing [16], [17], [18], and variational quantum eigensolver (VQE) [12], which we will define in detail later.

In this work, we study the VRP in a different light. Here, we explore adding controlled noise to an adapted quantum solution to determine if it improves or degrades the overall results. Recent works in QAOA [19], [20], [21], [22] and VQE algorithms [23] studied the generic effects of noise in these hybrid algorithms. Our work complements these results by analyzing the effects of noise in a detailed gate-based simulation of an algorithm to solve VRP. We analyze the effect of various noise channels on an existing, yet variable, ansatz developed as a solution to VRP. We apply amplitude damping, bit-flip, phase-flip, bit-phase-flip, and depolarizing noise channels to VRP circuits, analyze the effects, and consolidate our findings.

The rest of this article is organized as follows. Section II discusses fundamental mathematical concepts, such as combinatorial optimization (CO), AC, QAOA, and the Ising model. Section III discusses the formulation of VRP using the concepts discussed in the previous section. Section IV covers the basic building blocks of circuits to solve VRP. Section V covers building an ansatz for VRP. Section VI covers the effects of applying noise models on the VRP circuit. Then, Section VII presents the observations from the simulation results. Finally, in Section VIII, we summarize the effects of various noise models on the VRP circuit and future directions of research.

## II. MATHEMATICAL BACKGROUND

The fundamental concepts used to solve routing problems involve techniques and procedures from the field of CO. This is followed by converting the mathematical models to a quantum equivalent mathematical model for formulating the objective function. The solution of the objective function is often achieved by maximization or minimization of the function. In this section, we outline the key concepts.

## A. COMBINATORIAL OPTIMIZATION

A classical CO problem is finding an optimal object from a finite set of objects. Exhaustive search is impractical in finding the optimal object due to the potentially high number of objects. Mathematically defining, if s is a string in some set S and m number of clauses, where s ≥ m , we have a maximization or minimization problem, known as a CO problem. Each clause expects a string parameter and returns a corresponding value [24]. It is the sum over the m clauses that constitute the string's total cost function. If we refer to the input string as z and clauses as C α , we can write the total cost function as

<!-- formula-not-decoded -->

The objective is to identify z ∈ S such that C z ( ) ≥ C z ( ) for all z ∈ S (or, in the case of minimization, C z ( ) ≤ C z ( ) for all z ∈ S ). Here, z is not required to be unique. C α ( z ) can take two values 0 or 1 and z can be written as z = z 0 z 1 z 2 . . . zn -1 for zi ∈{ 0 1 . Also, considering maximization problems (as-, } suming C α ( z ) is a clause), the minimization problems can be studied as C ′ α ( z ) = -1 C α ( z )

<!-- formula-not-decoded -->

## B. ADIABATIC QUANTUM COMPUTATION (AQC)

AQC is a theoretical framework of a quantum computer [4], [25]. The adiabatic theorem asserts that if the change to the Hamiltonian is sufficiently gradual, the system remains in the ground state of the given Hamiltonian [26]. The Hamiltonian is an energy operator of a system. In AQC, there are two Hamiltonians: the driver Hamiltonian ( Hd ) and the problem Hamiltonian ( Hp ). The driver Hamiltonian ( Hd ) is the energy operator whose ground state is easy to prepare, whereas the problem Hamiltonian ( Hp ) is the energy operator whose ground state is obtained after evolution [4]. Interpolation times are proportional to the energy gap between the two lowest states of the Hamiltonian being used.

The procedure begins with an easy-to-prepare ground state (i.e., the ground state of ( Hd )) and ends (ideally) with the ground state of ( Hp ), which is, in general, not directly characterizable. Mathematically, constitute function s t ( ) on [0 , T ] where s (0) = 0 and s T ( ) = 1. T is the value of time set high enough for the adiabatic theorem to hold. We define the Hamiltonian, H t ( ) = (1 -s t ( )) HD + s t ( ) HP . According to the adiabatic theorem, a system maintains its ground state of H t ( ) across the whole interval [0 , T ], provided a suitable s t ( ); hence, the system is in the initial ground state ( Hd ) at time t = 0, and it will evolve into the intended ground state ( Hp ) at time t = T . In general, it is challenging to assess the integral describing the temporal evolution under this time-dependent Hamiltonian [27]

<!-- formula-not-decoded -->

It is possible to assess this Hamiltonian using Trotterization methods [28]. We divide U T ( ) into intervals of δ t small enough such that the Hamiltonian is almost constant over them. This permits us to use the much more streamlined formula for the Hamiltonian that is independent of time. Assuming U b a ( , ) is the time evolution from instant a to instant b

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

where the approximation gets better as p gets larger (or, in other words, as δ t gets smaller), and where δ t is measured in /planckover2pi1 . Now using the approximation e i ( A + B x ) = e iAx e iBx + O ( x 2 ) and adding Hamiltonian H j t ( δ ) = (1 -s ( j δ t )) HD + s ( j δ t ) HP , the integral U t ( ) becomes

<!-- formula-not-decoded -->

AQC may be approximated by allowing the system to develop under HP for a small s ( j δ t ) δ t and then HD for a small (1 -s ( j δ t )) δ t , and unitaries can be derived for these operations using U = e -i α H t δ . Here, α is an integer in the range [0, 1], and this includes the scaling resulting from s ( j δ t ). AQC forms the theoretical basis of the variational quantum algorithm QAOA, which is discussed briefly in the next section.

## C. QUANTUM APPROXIMATE OPTIMIZATION ALGORITHM

QAOA is a variational algorithm proposed by Farhi et al. in 2014 [3], [4]. This algorithm relies on the framework of AQC. Because of its use of both conventional and quantum methods, this algorithm is considered as hybrid algorithm. In the previous section, quantum AC drove the system from the eigenstate of driver Hamiltonian to that of the eigenstate of problem Hamiltonian.

In the context of optimization problems, the problem Hamiltonian can be written as

<!-- formula-not-decoded -->

The maximum energy eigenstate of C solves the CO problem. Similarly, for driver Hamiltonian, we use

<!-- formula-not-decoded -->

where σ x j represents the Pauli operator σ x on the bit z j . B is also known as the mixing operator. Let us also define Uc ( γ ) = e -i γ C and UB ( β ) = e -i β B , which lets the system evolve under C for some γ amount of time and under B for

![Image](image_000002_b254dff7bece0ea114f84985e55be39d5bf99a644980162f236f4259f20fcf7a.png)

some β amount of time, respectively. QAOA then constructs a state

<!-- formula-not-decoded -->

where | s 〉 is a superposition state of all input qubits. The expectation value of the cost function ∑ m α = 1 〈 β,γ | C α | β,γ 〉 gives the solution or the approximate solution of the problem after simplex or gradient-based optimization starting with some initial set of parameters [29].

## D. ISING MODEL

The Ising model of ferromagnetism is a well-established mathematical model used extensively in the field of statistical mechanics [30], [31]. There are two possible states for the magnetic dipole moments of atomic 'spins' ( + 1 and 1), each of which is represented by a discrete variable in the model. Each spin is able to communicate with its neighbors because of how they are organized in a graph, usually a lattice (where the local structure regularly repeats in all directions). The system tends toward the lowest energy state when neighboring spins agree, but heat interrupts this tendency, allowing for the emergence of alternate structural phases. The model serves as a simplification of reality that may be used to spot phase transitions [32]. Using the following Hamiltonian, we can describe the sum of the spin energies

<!-- formula-not-decoded -->

where Ji j represents the interaction between i and j , which are adjacent spins, and h represents an external magnetic field. If J is positive, the ground state at h = 0 is a ferromagnet. If J is negative, the ground state at h = 0 is an antiferromagnet for a bipartite lattice. Hence, for simplification and in the context of this document, we can write the Hamiltonian as

<!-- formula-not-decoded -->

Here, σ z and σ x represent Pauli z and x operator. For simplification, we can consider the following conditions to be ferromagnetic ( Ji j &gt; 0), h = 0 assuming no external influence on the spin. Thus, we can rewrite the Hamiltonian as follows:

<!-- formula-not-decoded -->

## E. VARIATIONAL QUANTUM EIGENSOLVER

VQE is a hybrid quantum-classical technique used to determine the eigenvalue of a large matrix or Hamiltonian H [33]. The basic objective of this method is to search for a trial qubit state of a wave function | ψ θ ( /vector ) 〉 , which is dependent on a parameter set /vector θ = θ , θ 1 2 , . . . also called as the variational parameters. By quantum theory, the expectation of an observable or Hamiltonian H in a state | ψ θ ( /vector ) 〉 can be expressed as

<!-- formula-not-decoded -->

![Image](image_000003_57656f605441bc85056633c29a82b3290c270830d811866ba718e9a49e92f670.png)

By spectral decomposition, H can be written as

<!-- formula-not-decoded -->

/negationslash where λ i and | ψ 〉 i are the eigenvalues and eigenstates, respectively, of matrix H . Also, the eigenstates of H are orthogonal so 〈 ψ i | ψ j 〉 = 0 if i = j . The wave function | ψ θ ( /vector ) 〉 is represented as the superposition of eigenstates

<!-- formula-not-decoded -->

Thus, the expectation becomes

<!-- formula-not-decoded -->

Clearly, E ( /vector θ ) ≥ λ min. So in VQE algorithm, we vary the parameters /vector θ = θ , θ 1 2 , . . . until E ( /vector θ ) is minimized. This characteristic of VQE is important for addressing CO problems in which a parameterized circuit is used to construct the trial state of the algorithm, and E ( /vector θ ) is the cost function, which is the expected value of the Hamiltonian in the trial state. On the assumption that the ansatz or parameterized quantum circuit may describe the ground state of the system, the ground state of the target Hamiltonian may be obtained by iterative minimization of the cost function. At each optimization stage, the optimization procedure employs a classical optimizer that employs a quantum computer to analyze the cost function and calculate its gradient.

## III. MODELING VRP IN QUANTUM

To find a solution to the vehicle routing problem, we can map the cost function to an Ising Hamiltonian Hc [32]. The minimization of Ising Hamiltonian Hc gives the solution to the problem. To begin, let us consider an arbitrary connected graph of n vertices and a binary decision variable xi j who has a value 1 if there exists an edge between i and j for edge weight w i j &gt; 0 else; the value is 0. To represent the VRP problem, we need n × ( n -1) decision variables. For every edge from i → j , we define two sets of nodes source [ ] and i target [ j ]. The set source [ ] contains the nodes i j to which i sends an edge j /epsilon1 source [ ]. The set target [ i j ] contains the nodes i to which i sends an edge i /epsilon1 target [ j ]. We define VRP as follows [12], [34]:

<!-- formula-not-decoded -->

where k is the number of vehicles and n is the total number of locations. Considering the starting location as 0th location or Depot D , we have n -1 locations for vehicles to travel. This is subject to the following constraints:

<!-- formula-not-decoded -->

/negationslash

/negationslash

<!-- formula-not-decoded -->

The first two constraints impose the restriction that the delivering vehicle must visit each node only once. The middle two constraints enforce the restriction that the vehicle must return to the depot after delivering the goods. The last two constraints impose the subtour elimination conditions and are bound on ui , with Q &gt; qj &gt; 0, and ui , Q qi , ∈ R .

For the VRP equation, we can form the Hamiltonian of VRP as follows [12]:

H

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

where A &gt; 0 is a constant.

The set of all binary decision variables xi j can be represented in vector form as

<!-- formula-not-decoded -->

Using the above-mentioned vector, we can define two additional vectors for each node

/negationslash

/negationslash

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

The aforementioned vectors will assist in the formulation of the QUBO model of VRP. For a linked graph G = ( N V , ), the QUBO model [14], [15], [35], [36] is defined as

<!-- formula-not-decoded -->

where Q is a quadratic coefficient of the edge weights, g is a linear coefficient of the node weights, and c is a constant. In order to find these coefficients in the QUBO formations of H VRP given in (18), we first put in (21) in terms HB and Hc , respectively, then expand and regroup the expression of H VRP according to (22)

<!-- formula-not-decoded -->

Hence, for QUBO formulation of (18), we get the coefficients Q n n ( ( -1) × n n ( -1)) , g n n ( ( -1) × 1) and c

<!-- formula-not-decoded -->

Here, J is the matrix of all ones, I is the identity matrix, and e 0 = [1 , 0 , . . . , 0] T .

From the above-mentioned equations, we can expand (22) to form the Ising Hamiltonian of VRP [14]

The binary decision variable xi j is transformed to spin variable si j ∈ {-1 1 , } as xi j = ( si j + 1) / 2.

<!-- formula-not-decoded -->

Here, the terms Ji j , hi , and d are defined as follows:

<!-- formula-not-decoded -->

## IV. ANALYSIS AND CIRCUIT BUILDING

In this section, we create a gate-based circuit to realize the above-mentioned formulation using the IBM gate

![Image](image_000004_b254dff7bece0ea114f84985e55be39d5bf99a644980162f236f4259f20fcf7a.png)

model, which we have implemented using the Qiskit framework [37]. For any arbitrary VRP problem using qubits, we begin with the state of |+〉 ⊗ n n ( -1) the ground state of H mixer by applying the Hadamard to all qubits initialized as zero states, and we prepare the following state:

<!-- formula-not-decoded -->

The energy E of the state | β, γ 〉 is calculated by the expectation of H cost from (12). Once again, the H cost term may be expressed in terms of Pauli operators using the Ising model, as

<!-- formula-not-decoded -->

Thus, for a single term of state in | β, γ 〉 as β 0 and γ 0, the expression reads e -iH mixer β 0 e -iH cost γ 0 . The first term H cost can be expanded to following:

iJ i j

e

γ σ σ

0

i

j

cos

Ji j

γ

0

I

i

sin

Ji j

γ σ σ

0

i

j

<!-- formula-not-decoded -->

=

+

Applying a cnot gate on, before, and after the aforementioned matrix ' M ,' we can swap the diagonal elements

<!-- formula-not-decoded -->

Observing the upper and lower blocks of the matrix, we can rewrite

<!-- formula-not-decoded -->

[ 1 0 0 e -2 iJ i j γ 0 ] is a phase gate. Looking at the second term of H cost , we get

<!-- formula-not-decoded -->

Fig. 1(a) depicts the basic circuit with two qubits along with gate selections for H cost .

FIGURE 1. (a) Sample circuit showing gate selections for H cost . (b) Sample circuit showing gate selections with additional U gate after barrier for H mixer . Note: The sample circuits displayed in the figures represent the building blocks of the actual circuit and do not represent actual angles that are obtained as a solution of VRP using VQE.

![Image](image_000005_57656f605441bc85056633c29a82b3290c270830d811866ba718e9a49e92f670.png)

![Image](image_000006_13f49b44aff26d8f7597486e3545f480a82af40144d717f9b824ffcfe90b818a.png)

Similarly, H mixer can be derived as follows:

<!-- formula-not-decoded -->

Considering a single term of H mixer and taking the unitary

<!-- formula-not-decoded -->

The IBMQ U gate is defined as follows:

<!-- formula-not-decoded -->

Comparing (31)-(35), we can establish relation of circuit parameters with γ and β to U gate, which will form building blocks of circuit. From Fig. 1(a), the circuit represents H cost term, where the first U gate takes the parameters, θ = 0 , φ = -2 Ji j γ 0, and λ = 0, and the second U gate takes the parameters, θ = 0 , φ = -2 hi γ 0, and λ = 0. Similarly, in Fig. 1(b), the U gate after the barrier represents the H mixer term having the parameters θ = 2 β , φ 0 = π/ 2, and λ = -π/ 2.

## V. VQE SIMULATION OF VRP

We construct the VRP circuit using the above-mentioned equations and create the Hamiltonians for three-city and four-city scenarios. Since we need n n ( -1) qubits, we end up with only Hamiltonians and circuits with 6 and 12 qubits. Beyond four cities, it is impossible to simulate in a classical desktop computer due to memory limitations. We create the ansatz using a quantum circuit defined in the previous section and run it across various VQE optimizers available in the IBM Qiskit framework: COBYLA, L\_BFGS\_B, SPSA, and SLSQP. We run the circuit up to four layers across all the optimizers and obtain the results depicted in Figs. 2 and 3. The primary difference between Figs. 2 and 3 is the while the

FIGURE 2. Plot illustrating the circuit simulation of VRP with five layers using various optimizers (COBYLA, L\_BFGS\_B, SLSQP, and SPSA). The plot consists of two separate graphs depicting the simulation output of 6 and 12 qubit circuits, respectively. Each plot, in turn, consists of four lines indicating energy values for different optimizers. The average value at each layer is represented in pairs with (layer, average energy) format.

![Image](image_000007_d633438559ba49ab62fe524e0d19ac69ceeb29bb56679b427105bdb038977090.png)

FIGURE 3. Plot illustrating the circuit simulation of VRP with five layers using various optimizers (COBYLA, L\_BFGS\_B, SLSQP, and SPSA). The plot consists of two graphs depicting the simulation output of 6 and 12 qubit circuits, respectively. Each plot consists of four lines indicating energy values for different optimizers. The minimum value at each layer is represented in pairs with (layer, minimum energy) format.

![Image](image_000008_330ab43d8289c84282bdd8be2dc36464d755defe8e82b077c5b1b8488096808a.png)

former represents average energy value the later represents minimum energy value of 15 consecutive runs at each layer.

The tables for these figures are presented in the Appendix with Tables 4 and 5. The figures are derived from 15 consecutive runs of the VRP circuit, each with all the optimizers and four layers. While the average energy values of VRP

simulations decrease as optimizations increase across layers for most of the six qubit circuits, the same trend is not observed for 12 qubit circuits. Also, the energy curves vary significantly across all optimizers. Similarly, for minimum energy graphs in Fig. 3, energy values have no clear decreasing trend as optimization layers increase. Yet from the minimumenergy graphs, we can reliably say that the protocol has achieved the minimum energy value. However they do not always follow the downward trend or stay at the same level as optimization layers increase. This, of course, is heavily dependent on the optimizer. Thus, when selecting an optimizer for simulation, we chose the optimizer that achieves the lowest minimum, the fewest number of optimization layers. In summary, we have found that COBYLA is the bestperforming optimizer, followed by SPSA, L\_BFGS\_B, and SLSQP. However, in the following, when we pass the circuit through various noise models, we will use only the COBYLA optimizer.

## VI. NOISE MODEL SIMULATION OF VQE

In a noisy quantum environment, a pure input state will be transformed into a mixed state represented as a density matrix [21], [38]. In the case of a 6-qubit state pure state | ψ 〉 qoq q q q q 1 2 3 4 5 , the density matrix can be defined as ρ = | ψ 〉 qoq q q q q 1 2 3 4 5 〈 ψ | qoq q q q q 1 2 3 4 5 . After the implementation of the noise model, the density matrix takes the following form:

<!-- formula-not-decoded -->

where r ∈ { A B W F D , , , , } . The elements of the noise channels are described as follows, A is amplitude damping noise, B is bit-flip noise, W is phase-flip noise, F is bit-phase-flip noise, and D is depolarizing noise. We apply these noise channels to our VRP circuit and ansatz, which is variable based on the number of qubits (6 or 12) and layers (1 to 5). For simulation purposes, we choose the optimizer COBYLA as it has the best performance characteristics in the simulation of VQE. We restrict the noise probability to 0.5 as noisy environments beyond this noise level are unlikely and irrelevant in practice. The following sections discuss the noise channels and operators we experimented on in the VRP circuit.

## A. AMPLITUDE DAMPING

The energy dissipation is a consequence of the interaction of the quantum system with an amplitude-damping channel. A quantum system gaining or losing energy from or to its environment is described as a change in amplitude rather than phase [38], [39]. If κ A is the probability of gain or loss of amplitude or decoherence rate, the Kraus operators of amplitude damping channel can be described as follows:

<!-- formula-not-decoded -->

![Image](image_000009_77de4933f947cabddf9164544a8549092616c15b7a5763a7613abd814d61a034.png)

<!-- formula-not-decoded -->

## B. BIT-FLIP NOISE

Random bit-flip errors characterize bit-flip noise [39] with probability κ B . Thus, the Kraus operators of the bit-flip noise channel can be described as

<!-- formula-not-decoded -->

## C. PHASE-FLIP NOISE

Phase-flip noise alters the phase parameter of the quantum system without exchange of energy [38], [39]. The decoherence rate or the phase-flip noise parameter also follows the simple Bernoulli distribution with probability parameter κ W . Thus, the Kraus operators of phase-flip noise channel can be defined as follows:

<!-- formula-not-decoded -->

## D. BIT-PHASE FLIP NOISE

Bit-phase flip noise channel is characterized by a combination of random bit-flip errors and a change in the quantum system's phase information without energy loss [39]. Like other noise channels, the decoherence rate or the combined probability of bit-phase flip error follows the distribution κ F . The Kraus operator of the bit-phase flip channel could be given by

<!-- formula-not-decoded -->

## E. DEPOLARIZING NOISE

Adepolarizing noise channel leaves the system untouched or replaces it with a maximally mixed state of I / d for a d -level quantum system. The decoherence rate or the depolarization noise probability follows the distribution with parameter κ D . The Kraus operators are as follows:

<!-- formula-not-decoded -->

It is to be noted that, in all cases, the noise channel is applied locally to each qubit in the circuit.

![Image](image_000010_57656f605441bc85056633c29a82b3290c270830d811866ba718e9a49e92f670.png)

TABLE 1 VQE Simulation of Amplitude Damping, Bit-Flip, Phase-Flip, Bit-Phase-Flip, and Depolarizing Channel for 6 and 12 Qubits With Four Layers Involving Optimizer COBYLA, Where Energy Costs are Averaged Over Ten Simulations

![Image](image_000011_96c28b20a2a23ed2bccfc73abe264227e6b5c7194f16455db5134c572a1a8e5e.png)

## VII. INFERENCES FROM SIMULATION

In the experiment of simulating VRP across various noise channels, we vary the noise probability from 0.05 to 0.5 and observe the energy values of VQE. We execute the VRP circuit with 1-4 layers on our chosen optimizer COBYLA for both 6 and 12 qubit configurations. The experiment is repeated ten times for each noise model with different noise realizations. In the same experiment, we calculate the minimum eigenvalue of classical Hamiltonian and record the difference in energy cost after noise induction.

The state's energy is recorded for each layer from 1 to 4 of the QAOA circuit. These values are averaged over the ten iterations to arrive at the average energy cost for each value of the noise parameter and each layer number. The results are given in Table 1. The deviation from the optimal value is shown in Table 2. Finally, Table 3 summarizes the inferences

TABLE 2 For 6 and 12 Qubits With Four Layers and Using COBYLA as an Optimizer, the Table Above Shows the Average Deviation From the Classical Minimum (Over Ten Simulations) for VQE Simulations Utilizing Amplitude Damping, Bit-Flip, Phase-Flip, Bit-Phase-Flip, and Depolarizing Channels

![Image](image_000012_ed8a99770eb73dcd646837950dc37eccd48d92962988d1121cd7fc5599c19b28.png)

on deviation from the classical minimum for various noise models.

We observe that VQE results are impacted due to the induction of noise. In the following sections, we will describe our observations briefly for each noise model.

## A. AMPLITUDE DAMPING NOISE

Amplitude damping noise shows values range between 50% and 75% of classical minimum for both 6 qubit ( -17 68) . and 12 qubit ( -65 684) circuits. . There are a few outliers where the algorithm can reach very close to the classical minimum for 6 qubit circuits. For 12 qubit circuits, the values are mostly above 50% of classical minimum but never reach classical minimum as close as in 6 qubit circuits. This trend is seen across multiple layers for amplitude damping channels. It is noticed that the global minimum across layers is observed at the second layer at -48 947, but it is very . close to the minimum of the first layer at -47 219. Hence, we .

TABLE 3 Summary of the Inferences on Deviation From the Classical Minimum for VQE Simulation of VRP Using Amplitude Damping, Bit-Flip, Phase-Flip, Bit-Phase-Flip, and Depolarizing Channels for 6 and 12 Qubits

![Image](image_000013_b254dff7bece0ea114f84985e55be39d5bf99a644980162f236f4259f20fcf7a.png)

FIGURE 4. Plot illustrating the average deviation of the energy cost of VRP with four layers using various amplitude damping and bit-flip noise models. The plot consists of two charts depicting the simulation output of 6 and 12 qubit circuits, respectively. The average deviation at each layer is represented in pairs with (noise parameter, average deviation of energy cost) format.

![Image](image_000014_3b159616deaed86226969865b33b721c75c395a43d46c836f1668ae60bee9b07.png)

can infer that increasing layers does not necessarily improve the results for the amplitude-damping channel. Table 6 in the Appendix summarizes the amplitude damping average energy values. Fig. 4 refers to the average deviation of energy cost from the classical minimum at each noise parameter across layers. This again confirms that deviation from classical minimum energy cost due to amplitude damping noise remains within 25%-50%, which is recorded in the Table 3.

## B. BIT-FLIP NOISE

For the bit-flip noise channel, we note that the VQE values are 50% or above the classical minimum for the first layer, but it degrades to around 25% for the second layer, falling further on third layer and finally close to zero on fourth layer. Since this trend is seen in both 6 and 12 qubit circuits, we can infer that increasing the number of layers degrades the VQE values for the bit-flip noise channel. We have summarized the bit-flip noise channel average energy values in Table 8 in the Appendix. Fig. 4 refers to the average deviation of energy cost from the classical minimum at each noise parameter across layers. This again confirms that deviation from classical minimum energy cost due to bit-flip noise remains within the range of 50%-75% for the first two layers before deteriorating further, which is recorded in Table 3.

FIGURE 5. Plot illustrating the average deviation of the energy cost of VRP with four layers using bit-phase-flip and depolarizing noise. The plot consists of two charts depicting the simulation output of 6 and 12 qubit circuits, respectively. The average deviation at each layer is represented in pairs with (noise parameter, average deviation of energy cost) format.

![Image](image_000015_57656f605441bc85056633c29a82b3290c270830d811866ba718e9a49e92f670.png)

![Image](image_000016_fc47bd93023051fa5055de1c5e6558462026d937d68645dcec3797e87d1f1a9a.png)

## C. BIT-PHASE-FLIP NOISE

## E. DATA GATHERING AND STATISTICS COLLECTION

There is a similar observation for the bit-phase-flip channel. The VQE values are 25% (or above) of the classical minimum for the first layer, but they degrade as the layers increase for 6 qubit. For 12 qubit circuits, the VQE values are consistently poor. We have summarized bit-phase-flip noise channel average energy values in Table 8 in the Appendix. Fig. 5 refers to the average deviation of energy cost from the classical minimum at each noise parameter across layers. This again confirms that deviation from classical minimumenergy cost due to bit-phase-flip noise remains close to 100%; this is recorded in Table 3.

## D. PHASE-FLIP AND DEPOLARIZING NOISE CHANNEL

Finally, for both depolarizing and phase-flip channels, the VQE values remain close to zero for both 6 and 12 qubit circuits. It appears that phase-flip and depolarizing noise channels are the most detrimental in VQE circuits (see Tables 10 and 14 in the Appendix). Figs. 5 and 6 refer to the average deviation of energy cost from the classical minimum at each noise parameter across layers. This again confirms that deviation from classical minimum energy cost due to depolarizing and phase-flip noise remains close to 100%, recorded in Table 3.

In all the simulations, we have used a quantum instance object and a fixed random seed in the Qiskit framework to avoid VQE terminating early and mitigate statistical fluctuations. Hence, all the noise models used here are applied to the quantum instance object, which in turn applies noise to the circuit whose parameters are varied by VQE to arrive at a result. We have executed ten iterations of VQE using various noise channels described previously. From the results of the ten simulations, we have taken the average energy value of each noise parameter at each layer. Our figure of merit is the difference between the layer's classical minimum and average energy cost. We remind the reader that gate-based simulations are extremely expensive. The results reported here amounted to 219 h of CPU time on a standard laptop computer using Qiskit's built-in simulators [37]. While more iterations would improve the variability of the average energy calculations, some clear trends have already been observed.

## VIII. CONCLUSION

Detailed simulation results, first for a noiseless case (see Figs. 2 and 3) and then for each noise channel, are provided in the Appendix. Each noise channel's simulation results are divided into two tables: an average and a minimum. These tables make it easy to see that the deviation from the classical

FIGURE 6. Plot illustrating the average energy cost of VRP with four layers using the phase-flip noise model. The plot consists of two charts depicting the simulation output of 6 and 12 qubit circuits, respectively. The average deviation at each layer is represented in pairs with (noise parameter, average deviation of energy cost) format.

![Image](image_000017_b254dff7bece0ea114f84985e55be39d5bf99a644980162f236f4259f20fcf7a.png)

![Image](image_000018_22494016e487c0e158b867510dc664ca63cf3441a0968a8402d5e1be1597bb8e.png)

minimum does not change linearly when the noise parameter is changed from 0.05 to 0.5 for each given noise channel. It is also clear that the deviation from the classical minimum does not grow linearly with the number of circuit layers used for optimization. Consider the instance of a bit-flip noise channel with a noise parameter of 0.25; for layers 1-3, the minimum energy cost is almost the classical minimum at -16 8, . but at fourth layer, it abruptly lowers to -1 9. . In the same scenario, the average energy continues a downward trend, going from 10.9 to 0.5. As we go over the tables for all of the noise channels, we observe several similar patterns. As a corollary, this validates making all noise channels use the same 0.05 to 0.5 broad noise parameter range. When we choose a lesser range or a range of noise parameters that do not ruin the results (different for each noise channel), we will see similar tendencies as we have found here, despite the fact that separate noise channels impact the circuit and the findings differently. We can see similar behaviors and range of noise parameters (though the case and experiment is different) for the application of noise channels in quantum teleportation [39].

The work we have presented in this article provides an interesting avenue for evaluating the effect of noise on detailed gate-based simulations of hybrid quantum algorithms for real-world applications. Noise is considered the most problematic aspect of today's intermediate-scale devices, and hence, understanding the details of the effects of noise is critical in understanding how to make the most effective use of them.

In most cases, the effects of noise are minimal at the first layer of the VRP circuits. While additional layers improve upon the results in the noiseless case, the opposite is valid with the induction of noise. Since some noise will always be present in quantum circuits, an empirical finding of our results is that the COBYLA optimizer performs better for VQE circuits compared with the other available optimizers. Yet there is room to study other optimizers, such as SPSA. As we had come across prior work on noise simulations in quantum circuits, one clear trend is that the results are heavily

TABLE 4 Table Summarizing the Average Energy Cost (Out of 15 Runs) of VQE Simulation Using 5 Optimizers

TABLE 5 Table Containing the Minimum Energy Value (Out of 15 Runs) Across Each Layer of VQE Simulation Using 5 Optimizers

influenced by the optimizer used for the simulations, while some optimizers perform well at first for lower values of noise, as the noise probabilities increase the performance degrades; yet, among them, COBYLA performs well as seen by multiple experiments [22].

Wealso aim to test and compare these results on more significant VRP instances in physical devices, which is beyond the ability to simulate classically. Future work is also needed to analyze more detailed noise models guided by the measured characteristics from the real NISQ devices proposed to solve problems, such as VRP.

## APPENDIX

Tables 4-15 show noiseless VQE simulation statistics.

![Image](image_000019_57656f605441bc85056633c29a82b3290c270830d811866ba718e9a49e92f670.png)

TABLE 6 Table Containing the Average Energy Values (Out of 10 Runs) for Amplitude Damping Noise Channel Across Each Layer of VQE Simulation Using COBYLA

TABLE 7 Table Containing the Minimum Energy Values (Out of 10 Runs) for Amplitude Damping Noise Channel Across Each Layer of VQE Simulation Using COBYLA

## TABLE 8 Table Containing the Average Energy Values (Out of 10 Runs) for BitFlip Noise Channel Across Each Layer of VQE Simulation Using COBYLA

## TABLE 9 Table Containing the Minimum Energy Values (Out of 10 Runs) for BitFlip Noise Channel Across Each Layer of VQE Simulation Using COBYLA

TABLE 10 Table Containing the Average Energy Values (Out of 10 Runs) for PhaseFlip Noise Channel Across Each Layer of VQE Simulation Using COBYLA

![Image](image_000020_97bdde0143663fdf098f5a82a9b456a398f307a074a1789ab0124cb80ab7f214.png)

TABLE 11 Table Containing the Minimum Energy Values (Out of 10 Runs) for PhaseFlip Noise Channel Across Each Layer of VQE Simulation Using COBYLA

TABLE 12 Table Containing the Average Energy Values (Out of 10 Runs) for Bit-Phase-Flip Noise Channel Across Each Layer of VQE Simulation Using COBYLA

TABLE 13 Table Containing the Minimum Energy Values (Out of 10 Runs) for Bit-PhaseFlip Noise Channel Across Each Layer of VQE Simulation Using COBYLA

TABLE 14 Table Containing the Average Energy Values (Out of 10 Runs) for Depolarising Noise Channel Across Each Layer of VQE Simulation Using COBYLA

TABLE 15 Table Containing the Minimum Energy Values (Out of 10 Runs) for Depolarising Noise Channel Across Each Layer of VQE Simulation Using COBYLA

## ACKNOWLEDGMENT

The authors would like to thank the IBM Quantum Experience platform and their team for developing the Qiskit platform and providing open access to their simulators for running quantum circuits and performing the experiments reported here [37].

## REFERENCES

- [1] A. Montanaro, 'Quantum algorithms: An overview,' npj Quantum Inf. , vol. 2, no. 1, Jan. 2016, Art. no. 15023, doi: 10.1038/npjqi.2015.23.
- [2] S. Jordan, 'Optimization, numerics, and machine learning.' Accessed: Aug. 24, 2023. [Online]. Available: https://quantumalgorithmzoo. org/#ONML

![Image](image_000021_57656f605441bc85056633c29a82b3290c270830d811866ba718e9a49e92f670.png)

- [3] E. Farhi, J. Goldstone, and S. Gutmann, 'A quantum approximate optimization algorithm,' 2014, arXiv:1411.4028 , doi: 10.48550/arXiv.1411.4028.
- [4] E. Farhi, J. Goldstone, S. Gutmann, and M. Sipser, 'Quantum computation by adiabatic evolution,' 2000, arXiv:quant-ph/0001106 , doi: 10.48550/arXiv.quant-ph/0001106.
- [5] L. K. Grover, 'A fast quantum mechanical algorithm for database search,' in Proc. 28th annual ACM Symp. Theory Comput. , 1996, pp. 212-219, doi: 10.1145/237814.237866.
- [6] V. Dasari, M. S. Im, and L. Beshaj, 'Solving machine learning optimization problems using quantum computers,' Proc. SPIE , vol. 11419, Apr. 2020, Art. no. 114190F, doi: 10.1117/12.2565038.
- [7] National Academies of Sciences, Engineering, and Medicine, 'Quantum algorithms and applications,' in Quantum Computing: Progress and Prospects . Washington, DC, USA: Nat. Acad. Press, 2019, ch. 3, pp. 57-94, doi: 10.17226/25196.
- [8] S. Harwood, C. Gambella, D. Trenev, A. Simonetto, D. Bernal, and D. Greenberg, 'Formulating and solving routing problems on quantum computers,' IEEE Trans. Quantum Eng. , vol. 2, 2021, Art. no. 3100118, doi: 10.1109/TQE.2021.3049230.
- [9] K. Srinivasan, S. Satyajit, B. K. Behera, and P. K. Panigrahi, 'Efficient quantum algorithm for solving travelling salesman problem: An IBM quantum experience,' May 2018, arXiv:1805.10928v1 , doi: 10.48550/arXiv.1805.10928.
- [10] S. Feld et al., 'A hybrid solution method for the capacitated vehicle routing problem using a quantum annealer,' Front. ICT , vol. 6, 2019, Art. no. 13, doi: 10.3389/fict.2019.00013.
- [11] M. Nazari, A. Oroojlooy, L. Snyder, and M. Takac, 'Reinforcement learning for solving the vehicle routing problem,' in Proc. 32nd Int. Conf. Neural Inf. Process. Syst. , 2018, pp. 9861-9871, doi: 10.5555/3327546.3327651.
- [12] U. Azad, B. K. Behera, E. A. Ahmed, P. K. Panigrahi, and A. Farouk, 'Solving vehicle routing problem using quantum approximate optimization algorithm,' IEEE Trans. Intell. Transp. Syst. , vol. 24, no. 7, pp. 7564-7573, Jul. 2023, doi: 10.1109/TITS.2022.3172241.
- [13] C. Papalitsas, T. Andronikos, K. Giannakis, G. Theocharopoulou, and S. Fanarioti, 'A QUBO model for the traveling salesman problem with time windows,' Algorithms , vol. 12, no. 11, 2019, Art. no. 224, doi: 10.3390/a12110224.
- [14] F. Glover, G. Kochenberger, M. Ma, and Y. Du, 'Quantum bridge analytics II: QUBO-Plus, network optimization and combinatorial chaining for asset exchange,' 4OR , vol. 18, no. 4, pp. 387-417, 2020, doi: 10.1007/s10288-020-00464-9.
- [15] G. Kochenberger et al., 'The unconstrained binary quadratic programming problem: A survey,' J. Combinatorial Optim. , vol. 28, pp. 58-81, Jul. 2014, doi: 10.1007/s10878-014-9734-0.
- [16] H. Irie, G. Wongpaisarnsin, M. Terabe, A. Miki, and S. Taguchi, 'Quantum annealing of vehicle routing problem with time, state and capacity,' in Quantum Technology and Optimization Problems (Lecture Notes in Computer Science) , S. Feld and C. Linnhoff-Popien, Eds., Cham, Switzerland: Springer Int. Publishing, 2019, pp. 145-156, doi: 10.1007/978-3-030-14082-3\_13.
- [17] A. Crispin and A. Syrichas, 'Quantum annealing algorithm for vehicle scheduling,' in Proc. IEEE Int. Conf. Syst., Man, Cybern. , Oct. 2013, pp. 3523-3528, doi: 10.1109/SMC.2013.601.
- [18] M. Sao, H. Watanabe, Y. Musha, and A. Utsunomiya, 'Application of digital annealer for faster combinatorial optimization,' FUJITSU Sci. Tech. J. , vol. 55, no. 2, pp. 45-51, 2019. [Online]. Available: https://www.fujitsu.com/global/documents/about/resources/ publications/fstj/archives/vol55-2/paper12.pdf
- [19] E. Campos, D. Rabinovich, V. Akshay, and J. Biamonte, 'Training saturation in layerwise quantum approximate optimization,' Phys. Rev. A , vol. 104, no. 3, Sep. 2021, Art. no. L030401, doi: 10.1103/PhysRevA.104.L030401.
- [20] S. Wang et al., 'Noise-induced barren plateaus in variational quantum algorithms,' Nature Commun. , vol. 12, no. 1, Nov. 2021, Art. no. 6961, doi: 10.1038/s41467-021-27045-6.
- [21] J. Marshall, F. Wudarski, S. Hadfield, and T. Hogg, 'Characterizing local noise in QAOA circuits,' IOP SciNotes , vol. 1, no. 2, Aug. 2020, Art. no. 025208, doi: 10.1088/2633-1357/abb0d7.
- [22] W. Lavrijsen, A. Tudor, J. Müller, C. Iancu, and W. de Jong, 'Classical optimizers for noisy intermediate-scale quantum devices,' in Proc. IEEE Int. Conf. Quantum Comput. Eng. , Oct. 2020, pp. 267-277, doi: 10.1109/QCE49297.2020.00041.
- [23] M. Cerezo et al., 'Variational quantum algorithms,' Nature Rev. Phys. , vol. 3, no. 9, pp. 625-644, Sep. 2021, doi: 10.1038/s42254-021-00348-9.
- [24] N. Guerrero, 'Solving combinatorial optimization problems using the quantum approximation optimization algorithm,' Mater's ThesisDept. Eng. Phys., Wright-Patterson AFB, OH, USA, Mar. 2020. [Online]. Available: https://scholar.afit.edu/etd/3263
- [25] T. Albash and D. A. Lidar, 'Adiabatic quantum computation,' Rev. Mod. Phys. , vol. 90, no. 1, Jan. 2018, Art. no. 015002, doi: 10.1103/RevModPhys.90.015002.
- [26] E. K. Grant and T. S. Humble, 'Adiabatic quantum computing and quantum annealing,' in Oxford Research Encyclopedia of Physics . New York, NY, USA: Oxford Univ. Press, 2020, doi: 10.1093/acrefore/9780190871994.013.32.
- [27] R. Hoque, 'The quantum approximate optimization algorithm,' 2019. Accessed: Aug. 24, 2023. [Online]. Available: https://ryanhoque.github.io/data/QAOA.pdf
- [28] Y. Sun, J.-Y. Zhang, M. S. Byrd, and L.-A. Wu, 'Adiabatic quantum simulation using trotterization,' New J. Phys. , vol. 22, no. 5, May 2020, Art. no. 053012, doi: 10.1088/1367-2630/ab7a31.
- [29] L. Zhou, S.-T. Wang, S. Choi, H. Pichler, and M. D. Lukin, 'Quantum approximate optimization algorithm: Performance, mechanism, and implementation on near-term devices,' Phys. Rev. X , vol. 10, no. 2, Jun. 2020, Art. no. 021067, doi: 10.1103/PhysRevX.10.021067.
- [30] S. P. Singh, 'The ising model: Brief introduction and its application,' in Metastable, Spintronics Materials and Mechanics of Deformable Bodies . London, U.K.: IntechOpen, Feb. 2020, ch. 8, Art. no. 155201, doi: 10.5772/intechopen.90875. [Online]. Available: https://www. intechopen.com/chapters/71210
- [31] S. G. Brush, 'History of the Lenz-Ising model,' Rev. Mod. Phys. , vol. 39, pp. 883-893, Oct. 1967, doi: 10.1103/RevModPhys.39.883.
- [32] A. Lucas, 'Ising formulations of many NP problems,' Front. Phys. , vol. 2, 2014, Art. no. 5, doi: 10.3389/fphy.2014.00005.
- [33] A. Peruzzo et al., 'A variational eigenvalue solver on a photonic quantum processor,' Nature Commun. , vol. 5, no. 1, Jul. 2014, Art. no. 4213, doi: 10.1038/ncomms5213.
- [34] 'Qiskit: Vehicle routing.' Accessed: Aug. 24, 2023. [Online]. Available: https://qiskit.org/ecosystem/optimization/tutorials/07\_examples\_ vehicle\_routing.html
- [35] P. Date, R. Patton, C. Schuman, and T. Potok, 'Efficiently embedding QUBO problems on adiabatic quantum computers,' Quantum Inf. Process. , vol. 18, no. 4, Mar. 2019, Art. no. 117, doi: 10.1007/s11128-019-2236-3.
- [36] G. G. Guerreschi, 'Solving quadratic unconstrained binary optimization with divide-and-conquer and quantum algorithms,' Jan. 2021, arXiv:2101.07813 , doi: 10.48550/arXiv.2101.07813.
- [37] M. S. A. et al., 'Qiskit: An open-source framework for quantum computing,' 2021. Accessed: Aug. 24, 2023. [Online]. Available: https://github.com/Qiskit/qiskit/tree/0.25.0
- [38] S. Ahadpour, F. Mirmasoudi, S. Ahadpour, and F. Mirmasoudi, 'The role of noisy channels in quantum teleportation,' Revista Mexicana de Física , vol. 66, no. 3, pp. 378-387, Jun. 2020, doi: 10.31349/revmexfis.66.378. [Online]. Available: http://www.scielo.org.mx/scielo.php?script=sci\_ abstract&amp;pid=S0035-001X2020000300378&amp;lng=es&amp;nrm=iso&amp;tlng=en
- [39] R.-G. Zhou, Y.-N. Zhang, R. Xu, C. Qian, and I. Hou, 'Asymmetric bidirectional controlled teleportation by using nine-qubit entangled state in noisy environment,' IEEE Access , vol. 7, pp. 75247-75264, 2019, doi: 10.1109/ACCESS.2019.2920094. [Online]. Available: https://ieeexplore.ieee.org/document/8726306