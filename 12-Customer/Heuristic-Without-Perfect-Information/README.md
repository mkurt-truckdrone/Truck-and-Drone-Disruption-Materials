# 12-Customer Disruption Analysis — Heuristic Method

This README describes the computational procedure used for the **12-customer rolling- and total-disruption heuristic experiments**. The overall heuristic follows the same approach as the 100-customer heuristic experiments: Concorde provides the truck route, the truck-and-drone heuristic evaluates drone assignments, local-search and savings procedures improve the solution, and post-disruption replanning is performed under the applicable disruption constraints.

The main 12-customer-specific differences are the customer data, drone endurance, number of disruption regions, and disruption start-time/duration settings.

## 12-Customer Instance Notebooks

The following 20 Jupyter Notebooks contain the individual 12-customer instances used in the heuristic experiments:

1. `agatzheur12custv51v51BASEv1(BASEDST15)v1`
2. `agatzheur12custv52v52BASEv1(BASEDST15)v1`
3. `agatzheur12custv53v53BASEv1(BASEDST15)v1`
4. `agatzheur12custv54v54BASEv1(BASEDST15)v1`
5. `agatzheur12custv55v55BASEv1(BASEDST15)v1`
6. `agatzheur12custv56v56BASEv1(BASEDST15)v1`
7. `agatzheur12custv57v57BASEv1(BASEDST15)v1`
8. `agatzheur12custv58v58BASEv1(BASEDST15)v1`
9. `agatzheur12custv59v59BASEv1(BASEDST15)v1`
10. `agatzheur12custv60v60BASEv1(BASEDST15)v1`
11. `agatzheur12custv70v70BASEv1(BASEDST15)v1`
12. `agatzheur12custv71v71BASEv1(BASEDST15)v1`
13. `agatzheur12custv72v72BASEv1(BASEDST15)v1`
14. `agatzheur12custv73v73BASEv1(BASEDST15)v1`
15. `agatzheur12custv74v74BASEv1(BASEDST15)v1`
16. `agatzheur12custv75v75BASEv1(BASEDST15)v1`
17. `agatzheur12custv76v76BASEv1(BASEDST15)v1`
18. `agatzheur12custv77v77BASEv1(BASEDST15)v1`
19. `agatzheur12custv78v78BASEv1(BASEDST15)v1`
20. `agatzheur12custv79v79BASEv1(BASEDST15)v1`

The notebooks follow the same general heuristic and post-disruption replanning procedure. The customer nodes/coordinates and corresponding instance-specific Concorde solutions change across instances.

## Experimental Design

For the 12-customer experiments:

- **Disruption Start Times (DST):** 15 and 30 minutes
- **Disruption Durations:** 30 and 60 minutes
- **Drone endurance:** 20 minutes
- **Number of disruption rolls/regions:** 3

This gives four start-time/duration combinations:

1. **DST = 15 minutes, Duration = 30 minutes**
2. **DST = 15 minutes, Duration = 60 minutes**
3. **DST = 30 minutes, Duration = 30 minutes**
4. **DST = 30 minutes, Duration = 60 minutes**

The applicable start time, duration, and disruption constraints are adjusted before each scenario is run.

## Solution Approach

### Stage 1: Initial (Pre-Disruption) Truck-Drone Plan

1. **Initial TSP solution.** Solve the TSP over all customers using Concorde (via NEOS) to obtain a truck-only tour.
2. **Truck-drone assignment.** Provide the Concorde tour to the heuristic as the baseline solution. The heuristic evaluates feasible assignments of customers to the drone.
3. **Improvement.** Because the Concorde truck tour does not necessarily guarantee the best truck-drone solution, local-search improvement procedures are applied, including:
   - 1-point move
   - 2-point move
   - 2-opt move
4. **Best initial solution.** The best truck-drone solution obtained through these procedures is retained as the initial pre-disruption plan.

### Stage 2: Disruption Snapshot

At the specified disruption start time, the current truck and drone states are determined. Customers already served by the truck or drone are removed, and the remaining network is constructed using the unserved customers and the current vehicle locations.

The current truck location becomes the new **node 0**. If the truck and drone are separated when the disruption occurs, the drone performs an emergency landing and must be rescued before it can become operational again. The model determines when the rescue occurs.

### Stage 3: Post-Disruption Planning

The remaining delivery problem is treated as a Hamiltonian path from the new truck location to the original depot. A dummy node is used to convert this path problem into a TSP representation that can be solved using Concorde. After the dummy node is removed, the recovered path is provided to the truck-and-drone heuristic.

The heuristic then evaluates drone assignments for the remaining customers and applies its improvement and savings procedures while respecting the applicable disruption constraints. If drone rescue is required, the drone cannot be used until the rescue has occurred.

The final solution is the best **disruption-aware truck-and-drone solution** obtained by the heuristic for the post-disruption problem.

## Rolling-Disruption Representation

The 12-customer experiments use **three disruption rolls/regions**. For a rolling disruption, the disruption moves sequentially across the three regions.

For a **30-minute duration**, the disruption is effective for 10 minutes in each region:

```python
TConstraint2 = [[0,10], [10,20], [20,30]]
```

For a **60-minute duration**, the disruption is effective for 20 minutes in each region:

```python
TConstraint2 = [[0,20], [20,40], [40,60]]
```

The disruption start time is handled separately through the scenario settings used for the post-disruption problem.

## Total-Disruption Scenarios

The same 12-customer notebooks are also used for **total-disruption** experiments.

For total disruption, all three regions are disrupted simultaneously for the full disruption duration.

For a **30-minute duration**:

```python
TConstraint2 = [[0,30], [0,30], [0,30]]
```

For a **60-minute duration**:

```python
TConstraint2 = [[0,60], [0,60], [0,60]]
```

Thus, the same heuristic framework is used for rolling and total disruption; the disruption constraints are changed to represent the corresponding scenario.

## No-Recourse Case

The same computational framework can also be used for the **No-Recourse** case. In the post-disruption model, drone sorties are disabled, so the remaining customers are served by the truck only.

## Main Differences from the 100-Customer Heuristic Experiments

The overall computational approach is the same. The principal 12-customer-specific settings are:

- 12 customer nodes rather than 100
- **20-minute drone endurance** rather than 40 minutes
- **3 disruption rolls/regions** rather than 5
- DST values of **15 and 30 minutes**
- Disruption durations of **30 and 60 minutes**
- Instance-specific customer coordinates and corresponding Concorde solutions

## Outputs and Supplementary Results

The notebooks provide the computational implementation for the **12-customer rolling- and total-disruption heuristic analysis**.

Each uploaded notebook represents an instance-specific implementation. The complete experimental results across the disruption scenarios are reported in the corresponding supplementary Excel result files.

## Requirements

The notebooks require the software and packages used by the implementation, including:

- Python
- NumPy
- pandas
- Matplotlib
- Concorde/NEOS access for the TSP solution
- The Python optimization/heuristic libraries used in the notebooks

A working environment supporting the notebooks' required packages and Concorde/NEOS connection is required.
