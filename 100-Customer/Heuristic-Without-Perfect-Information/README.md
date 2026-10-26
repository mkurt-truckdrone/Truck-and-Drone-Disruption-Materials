# 100-Customer Rolling Disruption — Heuristic Method

This README describes the computational procedure implemented in the Jupyter
Notebooks for the **100-customer disruption experiments**. The method uses
Concorde to obtain truck routes and a truck-and-drone heuristic with
local-search and savings procedures to obtain disruption-aware solutions.

## 100-Customer Instance Notebooks

The following 20 Jupyter Notebooks contain the individual 100-customer
instances used in the experiments:

1. `agatzheur100custv101v101wconcall3-5perDisrupMainv1`
2. `agatzheur100custv102v102wconcall3-5perDisrupMainv1`
3. `agatzheur100custv103v103wconcall3-5perDisrupMainv1`
4. `agatzheur100custv104v104wconcall3-5perDisrupMainv1`
5. `agatzheur100custv105v105wconcall3-5perDisrupMainv1`
6. `agatzheur100custv106v106wconcall3-5perDisrupMainv1`
7. `agatzheur100custv107v107wconcall3-5perDisrupMainv1`
8. `agatzheur100custv108v108wconcall3-5perDisrupMainv1`
9. `agatzheur100custv109v109wconcall3-5perDisrupMainv1`
10. `agatzheur100custv110v110wconcall3-5perDisrupMainv1`
11. `agatzheur100custv111v111wconcall3-5perDisrupMainv1`
12. `agatzheur100custv112v112wconcall3-5perDisrupMainv1`
13. `agatzheur100custv114v114wconcall3-5perDisrupMainv1`
14. `agatzheur100custv115v115wconcall3-5perDisrupMainv1`
15. `agatzheur100custv116v116wconcall3-5perDisrupMainv1`
16. `agatzheur100custv117v117wconcall3-5perDisrupMainv1`
17. `agatzheur100custv118v118wconcall3-5perDisrupMainv1`
18. `agatzheur100custv119v119wconcall3-5perDisrupMainv1`
19. `agatzheur100custv120v120wconcall3-5perDisrupMainv1`
20. `agatzheur100custv121v121wconcall3-5perDisrupMainv1`

These notebooks contain the instance-specific customer data and coordinates
used by the computational procedure.

## Experimental Design

Each 100-customer instance is evaluated under the considered disruption
start-time and duration combinations. For **each start-time/duration
combination, five replications** are performed.

Five different random seeds are used for the five replications:

```text
1040, 1389, 330, 7878, 1923
```

The start time and disruption duration are set to the desired values before
each run. The corresponding disruption constraints are then adjusted for the
five-roll structure.

For the 100-customer experiments, the disruption start times are **50, 125, and 200 minutes**, and each start time is evaluated with disruption durations of **60, 120, and 180 minutes**. This gives nine start-time/duration combinations:

1. **DST = 50 minutes, Duration = 60 minutes**
2. **DST = 50 minutes, Duration = 120 minutes**
3. **DST = 50 minutes, Duration = 180 minutes**
4. **DST = 125 minutes, Duration = 60 minutes**
5. **DST = 125 minutes, Duration = 120 minutes**
6. **DST = 125 minutes, Duration = 180 minutes**
7. **DST = 200 minutes, Duration = 60 minutes**
8. **DST = 200 minutes, Duration = 120 minutes**
9. **DST = 200 minutes, Duration = 180 minutes**

## Solution Approach

### Stage 1: Initial (Pre-Disruption) Truck-Drone Plan

1. **Initial TSP solution.** Solve the TSP over all customers using Concorde
   (via NEOS) to obtain a truck-only tour.

2. **Truck-drone assignment.** Provide the Concorde tour to the heuristic as
   the baseline solution. The heuristic evaluates feasible assignments of
   customers to the drone.

3. **Improvement.** Because the Concorde truck tour does not necessarily
   guarantee the best truck-drone solution, local-search improvement
   procedures are also applied, including:
   - 1-point move
   - 2-point move
   - 2-opt move

4. **Best initial solution.** The best truck-drone solution obtained through
   these procedures is retained as the initial pre-disruption plan.

### Stage 2: Disruption Snapshot

1. Take a snapshot of the system at the specified **Disruption Start Time
   (DST)**.
2. Determine the truck and drone locations at that time.
3. Remove customers already served by the truck or drone. The remaining
   customers form the post-disruption problem.
4. Construct the replanning network using the remaining customers and the
   current truck and drone locations.
5. Label the current truck location as **node 0**.
6. If the truck and drone are separated when the disruption occurs, the
   drone performs an emergency landing and the drone must subsequently be
   rescued to become operational again. The model determines when the
   rescue occurs; the drone cannot be used before it is rescued.

### Stage 3: Post-Disruption Planning

1. **Drone rescue.** If the truck and drone are separated, the truck needs
   to rescue the drone so that it can become operational again.

2. **Hamiltonian path.** The remaining delivery problem is a Hamiltonian
   path from the new truck location (node 0) to the original depot, rather
   than a closed TSP tour. Concorde therefore cannot be applied directly.

3. **Dummy node.** A dummy node is added with zero distance to node 0 and
   the original depot and a sufficiently large distance to all other nodes.

4. **TSP solution.** The dummy node converts the path problem into a
   standard TSP representation that can be solved by Concorde. It only
   serves to connect the original depot to node 0 for this transformation.

5. **Recovering the path.** Removing the dummy node from the Concorde tour
   gives the required Hamiltonian path from node 0 to the original depot.

The resulting path is then provided to the truck-drone heuristic as the
starting solution for the post-disruption problem. The heuristic evaluates
drone assignments for the remaining customers and applies its improvement
and savings procedures while respecting the applicable disruption
constraints.

The final solution is the best **disruption-aware truck-and-drone solution**
obtained by the heuristic for the post-disruption problem.

## Disruption Representation

The 100-customer experiments use **five disruption rolls/regions**. For
rolling disruption, the disruption moves sequentially across the five rolls.
The roll-specific disruption constraints are adjusted according to the
specified disruption start time and duration before each run.

The same instance notebooks are used for the different disruption scenarios;
only the applicable scenario parameters and roll constraints are changed
before the run.

## Total Disruption

The same notebooks are also used for **total-disruption** experiments.

For total disruption, all five roll constraints are set to cover the full
disruption duration, beginning at time 0. Thus, if the disruption duration is
`D` minutes, the roll constraints are represented as:

```python
TConstraint2 = [[0,D], [0,D], [0,D], [0,D], [0,D]]
```

For a **60-minute total disruption**:

```python
TConstraint2 = [[0,60], [0,60], [0,60], [0,60], [0,60]]
```

For a **120-minute total disruption**:

```python
TConstraint2 = [[0,120], [0,120], [0,120], [0,120], [0,120]]
```

For a **180-minute total disruption**:

```python
TConstraint2 = [[0,180], [0,180], [0,180], [0,180], [0,180]]
```

## No-Recourse Case

The same computational framework can also be used to obtain the
**No-Recourse** case. In the post-disruption model, drone sorties are
disabled, so the remaining customers are served by the truck only.

This provides the corresponding no-recourse benchmark for comparison with
the truck-and-drone recourse solution.

## Main Computational Components

The notebooks combine:

- Concorde/NEOS for TSP solutions
- Truck-and-drone heuristic assignment
- 1-point, 2-point, and 2-opt improvement procedures
- Savings-based improvement
- Post-disruption replanning
- Drone rescue when required
- Five-roll disruption constraints
- Truck-and-drone recourse
- Five replications per start-time/duration combination

## Outputs

The notebooks are used to generate the post-disruption computational
results, including the truck-and-drone solution, objective values, drone
assignments, and computational information for the specified disruption
scenario.

The complete experimental results for the 100-customer instances are
reported in the corresponding supplementary Excel files.

## Implementation Note

The computational procedure described above represents the common approach
used for the 100-customer disruption experiments. Individual notebooks may
contain instance-specific settings or manually entered information when
required by the particular solution state.

## Requirements

The notebooks require the software and packages used by the implementation,
including:

- Python
- NumPy
- pandas
- Matplotlib
- Concorde/NEOS access for the TSP solution
- The Python optimization/heuristic libraries used in the notebooks

A working environment supporting the notebooks' required packages and
Concorde/NEOS connection is required.

## Relation to the Supplementary Results

These notebooks provide the computational implementation for the
**100-customer rolling- and total-disruption heuristic analysis**. The
corresponding Excel result files contain the reported results and
comparisons for the experimental scenarios, including the five replications
for each disruption start-time and duration combination.
