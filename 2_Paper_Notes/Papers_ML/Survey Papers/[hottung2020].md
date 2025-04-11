# Neural Large Neighborhood Search for the Capacitated Vehicle Routing Problem

## Metadata
- **Link to PDF**: [[[hottung2020]_Neural_large_neighborhood_search_for_the_capacitated_vehicle_routing_problem.pdf]]
- **Tags**:  
  #ML-ReinforcementLearning  
  #RL-PolicyGradient  
  #RL-ActorCritic  
  #ML-AssistedHeuristics  
  #ML-AssistedLocalSearch  
  #ML-DestroyRepair  
  #AttentionMechanismAM  
  #CVRP  
  #SDVRP  
  #LargeNeighborhoodSearch  
  #AdaptiveLargeNeighborhoodSearch  
- **Relevant**: true  
  - *Reasoning*: The paper addresses deterministic VRPs (CVRP and SDVRP) using reinforcement learning for the destroy-repair paradigm, aligning well with the thesis scope.  
- **Fit Score**: 10  
- **State of the Art (SoA) Concepts**:
  - Large Neighborhood Search (LNS) with learned repair operators
  - Policy gradient reinforcement learning for VRP
  - Neural attention mechanism for incomplete solution repair
  - Comparison with LKH3 and state-of-the-art CVRP/SDVRP solvers
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**:  
  - Custom CVRP dataset (17 instance groups inspired by [27])  
  - SDVRP instances from [2]  

## Abstract
“Learning how to automatically solve optimization problems has the potential to provide the next big leap in optimization technology. The performance of automatically learned heuristics on routing problems has been steadily improving in recent years, but approaches based purely on machine learning are still outperformed by state-of-the-art optimization methods. To close this performance gap, we propose a novel large neighborhood search (LNS) framework for vehicle routing that integrates learned heuristics for generating new solutions. The learning mechanism is based on a deep neural network with an attention mechanism and has been especially designed to be integrated into an LNS search setting. We evaluate our approach on the capacitated vehicle routing problem (CVRP) and the split delivery vehicle routing problem (SDVRP). On CVRP instances with up to 297 customers, our approach significantly outperforms an LNS that uses only handcrafted heuristics and a well-known heuristic from the literature. Furthermore, we show for the CVRP and the SDVRP that our approach surpasses the performance of existing machine learning approaches and comes close to the performance of state-of-the-art optimization approaches.”

## Summary
- **Context & Motivation**  
  - Addresses the performance gap between purely ML-based approaches and traditional state-of-the-art heuristic/optimization methods for the VRP.  
  - Proposes a way to leverage deep reinforcement learning within a larger metaheuristic framework (Large Neighborhood Search), aiming to combine the strengths of learned heuristics with established LNS practices.  

- **Core Contribution**  
  1. **Neural Large Neighborhood Search (NLNS)**: An LNS metaheuristic that learns complex repair operators (i.e., how to re-insert removed customers).  
  2. **Attention-based Model**: Uses a custom deep neural network with attention to decide how to connect incomplete tours during solution repair.  
  3. **Integration of Reinforcement Learning**: Specifically employs policy gradient with a learned critic as a baseline, allowing the model to improve solution quality by reinforcing better repair actions.  

- **VRP Variant(s) Addressed**  
  - **Capacitated Vehicle Routing Problem (CVRP)**: The standard version with capacity constraints and single service of each customer.  
  - **Split Delivery VRP (SDVRP)**: Allows splitting each customer’s demand across multiple vehicles if needed.  

- **Methodology**  
  - **Destroy & Repair Cycle**:  
    - **Destroy**: Two simple operators (point-based destroy or tour-based destroy) remove a portion of the solution.  
    - **Repair**: A neural network, trained with reinforcement learning, reconnects the destroyed (incomplete) tours.  
  - **Neural Network Architecture**:  
    - Inputs represent partial tours and reference endpoints (depots, nodes).  
    - Uses embedding layers followed by an attention mechanism that combines information from the reference node with context from other partial tours.  
    - Outputs a probability distribution over possible reconnection points.  
    - Trained via policy gradient REINFORCE, using a critic network to estimate the baseline cost of repairing each incomplete solution.  
  - **Search Strategies**:  
    - **Batch Search**: Solve multiple instances in parallel, adaptively choosing the best destroy-repair pair to minimize average solution cost.  
    - **Single Instance Search**: Applies a simulated annealing–style acceptance criterion, splitting a batch of solutions and resetting part of the batch to the best solution in each iteration (maintaining both intensification and diversification).  

- **Experimental Setup & Findings**  
  1. **Comparison with Pure ML Methods** (e.g., attention model (AM) with sampling, beam search approaches):  
     - On CVRP and SDVRP with up to 100 customers, NLNS outperforms existing ML-based methods in solution quality, with comparable or slightly increased runtime.  
     - Highlights the importance of a metaheuristic-level approach rather than just sampling or beam search alone.  
  2. **Comparison with State-of-the-Art Optimizers** (UHGS, LKH3 for CVRP and SplitILS for SDVRP):  
     - For CVRP up to 297 customers, NLNS reaches solution quality near LKH3 and in some settings surpasses it on average.  
     - UHGS still yields the best solutions overall (smallest average cost), but NLNS narrows the gap significantly.  
     - On SDVRP with 100 customers, NLNS outperforms the specialized SplitILS on certain instances, demonstrating strong generalization even on different demand distributions.  
  3. **Ablation**: Replacing the learned operator with a known handcrafted repair operator shows significantly inferior results, illustrating the advantage of learning.  

- **Strengths & Contributions**  
  - **Scalability**: The attention-based model only processes partial tours (due to destroy operators), avoiding the typical complexity explosion on large instances.  
  - **Search Integration**: Embeds the learned repair operator into an effective metaheuristic, improving the exploration of the solution space beyond simple sampling or beam search.  
  - **Generalized Design**: The approach is problem-agnostic (destroy procedures are simple, no domain-specific knowledge), potentially applicable to many VRP variants.  
  - **State-of-the-Art Performance**: Matches or nears top-quality solutions from well-known solvers (LKH3, UHGS, SplitILS) on certain benchmarks, a significant milestone for learning-based VRP heuristics.  

- **Limitations & Challenges**  
  - **Longer Training Times**: Training each operator on a specialized instance distribution can be computationally intensive (several hours to days).  
  - **Instance-Specific Operators**: Model is trained on a certain distribution; performance might degrade if real-world instances deviate markedly from the training scenario.  
  - **No Comprehensive Theoretical Analysis**: Although practical performance is strong, the paper focuses primarily on empirical benchmarking.  

- **Relevance & Impact for Deterministic VRP**  
  - Demonstrates that an RL-based heuristic integrated into a standard large neighborhood search can achieve near state-of-the-art performance.  
  - Supports the feasibility of data-driven heuristics for large-scale deterministic VRPs.  
  - Provides a blueprint for how to combine learned policies with metaheuristic frameworks, informing future hybrid designs in deterministic routing.  
