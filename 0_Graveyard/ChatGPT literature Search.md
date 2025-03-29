## Analytics and Machine Learning in Vehicle Routing Research
Im Folgenden finden Sie eine komprimierte Durchsicht des oben zitierten Übersichtsartikels, bei der alle Arbeiten herausgefiltert wurden, die sich ausschließlich (oder zumindest ganz überwiegend) mit dem deterministischen VRP (bzw. deterministischen Varianten wie CVRP, VRPTW etc.) beschäftigen. Sämtliche Referenzen, die schwerpunktmäßig stochastische, robuste oder dynamische VRP-Probleme (d. h. mit unsicheren Parametern, Zufallsanforderungen oder in Echtzeit entstehenden Aufträgen) behandeln, bleiben ausgeschlossen, da Sie diese laut Absprache mit Ihrer Betreuerin nicht berücksichtigen möchten.

1. ML-gestützte (deterministische) Modellierung und Parametrierung
1.1. Lernen zur Algorithmen- bzw. Parameter-Konfiguration
Calvet et al. (2016b)
→ Beschreiben eine statistisch basierte „Parameter Selection“-Methode, die auf Multi-Depot Vehicle Routing angewandt wird. Im Artikel wird keine Unsicherheit erwähnt; das Problem ist daher als deterministisch anzusehen.

Kadaba, Nygard, and Juell (1991)
→ Frühere Arbeit, die ein wissensbasiertes System und neuronale Netze für Routing- und Scheduling-Probleme einsetzt, überwiegend ohne stochastische Annahmen.

Cooray and Rupasinghe (2017)
→ Nutzen Clustering-Methoden, um die Parameter einer Genetischen Algorithmen-Suche für ein deterministisches VRP besser zu wählen.

Žunić, Ðonko, and Buza (2020)
→ Entwickeln ein Vorhersagemodell (z. B. Support Vector Machines), das die Hyperparameter ihres Heuristikverfahrens für das heterogene VRP mit Zeitfenstern (HVRPTW) auf realen Datensätzen dynamisch anpasst – aber ohne stochastische Demands, also deterministisch.

1.2. Clustering-Ansätze in deterministischen Settings
Mehrere zitierte Arbeiten schlagen vor, Kunden (oder Aufträge) zunächst in Cluster aufzuteilen und dann pro Cluster eine deterministische Routing-Optimierung durchzuführen. Diese Cluster-First-Routing-Second-Ansätze (CFRS) sind in der Regel für deterministische VRP-Varianten formuliert:

Erdoğan and Miller-Hooks (2012)
→ „Green VRP“ (G-VRP) mit deterministischem Charakter: Nutzung eines DBSCAN-Clustering für die Kunden, anschließende (modifizierte) Clarke-Wright-Heuristik.

Comert et al. (2017/2018)
→ Zweistufige Lösung für das VRP mit Zeitfenstern (VRPTW). Erst Clustering (K-Means, K-Medoids, DBSCAN), dann exakte bzw. heuristische Tourenplanung. Keine stochastischen Elemente erwähnt.

Göçmen and Erol (2019)
→ Kombination von VRP (Pick-up-Routing) mit 3D-Bin-Packing-Aspekten; nutzt ein K-Means-Clustering. Es handelt sich um deterministische Problemstellungen.

Kubra, Muhammet, and Ozcan (2019)
→ „Route First – Cluster Second“-Ansatz (RFCS) für Mitarbeiterrouten; erst wird eine TSP-Tour erstellt und anschließend mit K-Means oder K-Medoids partitioniert. Keine Unsicherheiten genannt.

(Auch in dem Überblick genannten) He et al. (2009), Nallusamy et al. (2010), Reed, Yiannakou, and Evering (2014), Xu, Pu, and Duan (2018), Geetha, Poonthalir, and Vanathi (2013), Rautela, Sharma, and Bhardwaj (2019), Yücenur and Demirel (2011), Qi et al. (2011), Luo and Chen (2014), Gao et al. (2016), Miranda-Bront et al. (2017)
→ Werden zusammen vom Übersichtsartikel als Beispiele dafür genannt, dass verschiedenste Clustering-Methoden (etwa K-Means, K-Medoids) mit deterministischen Routing-Heuristiken kombiniert werden. Im Review wird kein stochastischer Aspekt erwähnt.

2. ML-gestützte (deterministische) Lösungsverfahren
2.1. Lernunterstützte Zerlegung (Decomposition) und Column Generation
Desaulniers, Lodi, and Morabit (2020)
→ Setzen Graph Neural Networks ein, um in einem Column-Generation-Verfahren für VRPs effizient die besten Spalten (Routen) zu identifizieren. Primär für klassische (deterministische) Varianten geeignet.

Kruber, Lübbecke, and Parmentier (2017)
→ K-Nearest-Neighbor-Klassifikator zur Wahl einer Dantzig-Wolfe-Zerlegung; anwendbar u. a. bei Vehicle Routing. Nicht spezifisch stochastisch.

Yao et al. (2019)
→ Nutzung von ADMM (Alternating Direction Method of Multipliers) für die Dekomposition einer deterministischen VRP-Formulierung (mehrdimensionales Commodity-Flow-Modell). Keine stochastischen Parameter.

2.2. Lernunterstützte Nachbarschaftssuche / Lokale Suche (perturbative Methoden)
Viele der erwähnten klassischen Metaheuristiken (Tabu Search, ALNS, etc.) sind in der Literatur überwiegend auf deterministische VRP-Varianten ausgelegt, sofern nicht explizit Unsicherheiten oder Dynamik angegeben sind. Beispiele aus dem Review:

Bai et al. (2007), Sabar et al. (2015), Soria-Alcaraz et al. (2017)
→ Hyper-Heuristics mit Reinforcement-Learning-Elementen oder adaptiven Auswahlregeln, getestet u. a. am deterministischen VRP mit Zeitfenstern (VRPTW).

Hottung and Tierney (2019)
→ „Neural Large Neighborhood Search“ (NLNS) für das CVRP und das Split-Delivery-VRP (beide normalerweise deterministisch). Keine Erwähnung stochastischer Demands.

Lu, Zhang, and Yang (2020)
→ „Learn to Improve (L2I)“-Methode mit Verstärkungslernen zur Operatorwahl (Verbesserungs- und Zerstör-Operatoren). Sie demonstrieren die Methode am deterministischen CVRP.

Gao et al. (2020), Chen et al. (2020b)
→ Lernen ebenfalls automatisiert „Destroy/Repair“-Heuristiken (angelehnt an ALNS) bzw. 2-Opt-artige Züge. Im Text nur CVRP/VRP-Beispielszenarien erwähnt, ohne stochastische Komponenten.

2.3. ML-gestützte Konstruktionsverfahren (Constructive Solvers)
Hierunter fallen die neueren Arbeiten mit neuronalen Netzen, die schrittweise Touren konstruieren („Learn to Construct“):

TSP-spezifische Arbeiten (z. B. Bello et al. (2017), Vinyals, Fortunato, and Jaitly (2015), Deudon et al. (2018), Kool, van Hoof, and Welling (2019), Joshi, Laurent, and Bresson (2019a), Ma et al. (2020) u. a.) gelten als deterministische Grundprobleme ohne Unsicherheit. Da der TSP eine (wenn auch vereinfachte) Sonderform des VRP darstellt, können diese Ansätze durchaus für eine deterministische VRP-Bachelorarbeit relevant sein.

Kool, van Hoof, and Welling (2019) und Peng, Wang, and Zhang (2020)
→ Transformer- bzw. Attention-Mechanismen zur Lösung von CVRP/SDVRP (ohne stochastischen Fokus).

Sheng, Ma, and Xia (2020)
→ Ein Reinforcement-Learning-Modell (Pointer Network-Variante) für VRP mit Prioritäten und Kapazitätsrestriktionen. Im Artikel wird keine Unsicherheit erwähnt.

Duan et al. (2020)
→ Kombinieren Graph Convolutional Networks und RNN-Policies zur sequenziellen Konstruktion von VRP-Lösungen. Anwendung auf „real-world data“ – jedoch ohne stochastische Modellierung.

Kaempfer and Wolf (2019), Hu, Yao, and Lee (2020)
→ Mehrfach-TSP (mTSP), ebenfalls deterministisch, da man mehrere „Fahrer“ wie unabhängige TSP-Touren betrachtet.

3. Nicht-relevante bzw. explizit stochastische/dynamische Arbeiten
Alle Referenzen, die stochastische Nachfrage, Zufallsvariablen für Fahrzeiten oder Dynamik (z. B. neue Aufträge im Zeitverlauf) thematisieren, sind für Ihre Bachelorarbeit laut Vorgabe ausgeschlossen. Insbesondere in den Bereichen

Stochastic VRP / Markov Decision Processes

Robust VRP

Chance-Constrained VRP

Rolling Horizon, Demand Forecasting

Dynamische VRP (inkl. Online- oder Echtzeit-Szenarien)

sind zahlreiche Quellen im Artikel erwähnt (z. B. Bent and Van Hentenryck (2005), Li, Tian, and Leung (2010), Ghosal and Wiesemann (2020), Balaji et al. (2019), Sabar et al. (2013) [für dynamisches VRP], usw.). Diese sollten Sie nach eigener Aussage nicht aufnehmen.


## Machine Learning to Solve Vehicle Routing Problems: A Survey
**33 (Peng et al.)**
- Problem: **CVRP**
- Methode: REINFORCE (baseline) + Graph Attention Network

**40 (Zhang et al.)**
- Problem: **mVRPSTW** (Multiple Vehicles, Soft Time Windows)
- Methode: REINFORCE + Multi-Head Attention

**47 (Lin et al.)**
- Problem: **EVRPTW** (Electrical VRP mit Time Windows, deterministisches Modell)
- Methode: REINFORCE (greedy rollout) + S2V

 **51 (Falkner und Schmidt-Thieme)**
- Problem: **mCVRPTW** (Multiple Vehicles, Time Windows)
- Methode: REINFORCE (Self-Attention)

**60 (Li et al.)**
- Problem: **PDP** (Pickup-and-Delivery) – **deterministisch**
- Methode: RL (Multi-Head Attention)

**61 (Li et al.)**
- Problem: **HCVRP** (Heterogeneous Capacitated VRP)
- Methode: REINFORCE + Multi-Head Attention

**62 (Duan et al.)**
- Problem: **CVRP**
- Methode: RL (REINFORCE), GCN-Embeddings
	
69 (Qi et al.)**
- Problem: **TDGVRPTW** (Time Dependent Green VRP mit Time Windows) – **deterministisch**
- Methode: Q-Learning + modifizierte NSGA-II

**71 (Hottung und Tierney)**
- Problem: **CVRP, SDVRP**
- Methode: Policy-basierte RL (Destroy-&-Repair-Operator + Neighbourhood Search)
        
**72 (Syed et al.)**
- Problem: **VRPTW** (Ride-Hailing-Anwendung)
- Methode: Supervised Learning (Insertion-Heuristik) + Large Neighborhood Search
        
**73 (Fernandes et al.)**
- Problem: **VRPTW**
- Methode: Q-Learning zur adaptiven Local Search
        
**81 (Delarue et al.)**
- Problem: **CVRP**
- Methode: Value-based RL + MIP (Mixed Integer Programming)
        
*83 (Wang et al.)** / **84 (Wang et al.)** / **86, 87**
- Probleme: VRP-Varianten (bspw. CMVRPTWMDP o. Ä.) mit deterministischen Annahmen
- Methoden: Clustering-, Q-Learning- oder GA-basierte Ansätze (je nach Paper)
        
**88 (Ma et al.)**
- Problem: **CVRP**
- Methode: Policy-Gradient (Actor-Critic) + 2-opt, Swap, Insert
        
**89 (Wu et al.)**
- Problem: **VRPTW** 
- Methode: Policy Gradient + Pairwise Local Operators

**91 (Gao et al.)**
- Problem: **CVRP, CVRPTW** 
- Methode: Actor-Critic („Destroy & Repair“) mittels GNN

**92 (Li et al.)**
- Problem: **Large-scale CVRP**
- Methode: Supervised Learning + MIP

**94 (Zhao et al.)**
- Problem: **CVRP, VRPTW**
- Methode: Policy Gradient + OR-Tools und LNS

104 (Hottung et al.)**
- Problem: **TSP, CVRP** (der CVRP-Teil ist deterministisch relevant)
- Methode: SL zur latenten Repräsentation; Unconstrained Optimization

**121 (Kim et al.)**
- Problem: **TSP, CVRP, PCTSP** – der **CVRP**-Aspekt ist deterministisch
- Methode: REINFORCE + Multi-Head Attention

113 (Zong et al.)**
- Problem: **CVRP**
- Methode: PCA, LSTM + Generator + Hybrid Genetic Search




## Integrating Machine Learning Into Vehicle Routing Problem: Methods and Applications
