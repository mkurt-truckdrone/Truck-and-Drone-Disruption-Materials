# Truck-and-Drone Disruption Materials

This repository contains supplementary computational materials for a study of **truck-and-drone delivery systems under disruption**. It includes exact and heuristic implementations for 12-customer and 100-customer instances, experiments with and without perfect disruption information, and Excel workbooks supporting the computational analysis reported in the paper.

## Repository Structure

### 12-Customer Instances

- **[Exact Method — Without Perfect Information](12-Customer/Exact-Without-Perfect-Information/)**  
  DOcplex/CPLEX exact-method notebooks for post-disruption replanning with recourse and no-recourse cases.

- **[Exact Method — With Perfect Information](12-Customer/Exact-With-Perfect-Information/)**  
  Exact-method notebooks in which disruption information is known before optimization and incorporated directly into the model.

- **[Heuristic — Without Perfect Information](12-Customer/Heuristic-Without-Perfect-Information/)**  
  Concorde/NEOS and truck-and-drone heuristic notebooks for rolling- and total-disruption experiments with post-disruption replanning.

### 100-Customer Instances

- **[Heuristic — Without Perfect Information](100-Customer/Heuristic-Without-Perfect-Information/)**  
  Concorde/NEOS and truck-and-drone heuristic notebooks for the 100-customer rolling- and total-disruption experiments.

- **[Heuristic — With Perfect Information](100-Customer/Heuristic-With-Perfect-Information/)**  
  Heuristic notebooks in which the disruption schedule is known before planning and disruption constraints are imposed from the beginning.

### Results and Analysis

- **[Impact of Recourse — 12-Customer](Results-and-Analysis/Impact-of-Recourse-12-Customer/)**  
  Excel results comparing recourse, no recourse, and no-disruption solutions for the 12-customer instances.

- **[Impact of Recourse — 100-Customer](Results-and-Analysis/Impact-of-Recourse-100-Customer/)**  
  Excel results and analysis for the 100-customer heuristic experiments, including recourse vs. no-recourse comparisons.

- **[Perfect Information — Small Instances](Results-and-Analysis/Perfect-Information-Small-Instances/)**  
  Analysis of the effect of perfect information on makespan and drone utilization for the 12-customer instances.

- **[Perfect Information — Large Instances](Results-and-Analysis/Perfect-Information-Large-Instances/)**  
  Analysis of perfect information for the 100-customer instances, including paired t-tests, computational times, and rolling-vs.-total disruption comparisons.

- **[Exact vs. Heuristic Gap](Results-and-Analysis/Exact-vs-Heuristic-Gap/)**  
  Comparison of exact and heuristic solutions for the 12-customer instances and the average gap analysis.

## Software

The computational materials use **Python**, **IBM DOcplex/CPLEX** for the exact method, and **Concorde/NEOS** together with the truck-and-drone heuristic for the heuristic experiments. Additional requirements are documented in the corresponding folder READMEs.

## Documentation

Each folder contains its own `README.md` describing the associated files, experimental settings, computational procedure, and relationship to the reported results.

## License

This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

