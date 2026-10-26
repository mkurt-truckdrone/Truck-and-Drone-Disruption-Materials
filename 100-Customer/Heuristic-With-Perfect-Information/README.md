# 100-Customer Rolling Disruption — Heuristic Method with Perfect Information

This README describes the computational procedure implemented in the Jupyter
Notebooks for the **100-customer rolling-disruption experiments with perfect
information**.

The perfect-information case represents an ideal benchmark in which the
disruption start time, disruption duration, and movement of the disruption
across the five regions are known **before the truck-and-drone solution is
constructed**. The disruption constraints are therefore imposed from the
beginning of the heuristic solution process.

Unlike the corresponding recourse model without perfect information, this
perfect-information procedure does **not** use a disruption-time snapshot or
post-disruption replanning. The complete solution is developed with the
disruption information already available.

## 100-Customer Instance Notebooks

The following 20 Jupyter Notebooks contain the individual 100-customer
perfect-information instances used in the experiments:

1. `agatzheur100custv101v101wconcall3-5perDisrupPerfInf.ipynb`
2. `agatzheur100custv102v102wconcall3-5perDisrupPerfInf.ipynb`
3. `agatzheur100custv103v103wconcall3-5perDisrupPerfInf.ipynb`
4. `agatzheur100custv104v104wconcall3-5perDisrupPerfInf.ipynb`
5. `agatzheur100custv105v105wconcall3-5perDisrupPerfInf.ipynb`
6. `agatzheur100custv106v106wconcall3-5perDisrupPerfInf.ipynb`
7. `agatzheur100custv107v107wconcall3-5perDisrupPerfInf.ipynb`
8. `agatzheur100custv108v108wconcall3-5perDisrupPerfInf.ipynb`
9. `agatzheur100custv109v109wconcall3-5perDisrupPerfInf.ipynb`
10. `agatzheur100custv110v110wconcall3-5perDisrupPerfInf.ipynb`
11. `agatzheur100custv111v111wconcall3-5perDisrupPerfInf.ipynb`
12. `agatzheur100custv112v112wconcall3-5perDisrupPerfInf.ipynb`
13. `agatzheur100custv114v114wconcall3-5perDisrupPerfInf.ipynb`
14. `agatzheur100custv115v115wconcall3-5perDisrupPerfInf.ipynb`
15. `agatzheur100custv116v116wconcall3-5perDisrupPerfInf.ipynb`
16. `agatzheur100custv117v117wconcall3-5perDisrupPerfInf.ipynb`
17. `agatzheur100custv118v118wconcall3-5perDisrupPerfInf.ipynb`
18. `agatzheur100custv119v119wconcall3-5perDisrupPerfInf.ipynb`
19. `agatzheur100custv120v120wconcall3-5perDisrupPerfInf.ipynb`
20. `agatzheur100custv121v121wconcall3-5perDisrupPerfInf.ipynb`

These notebooks contain the instance-specific customer data and coordinates
used by the common perfect-information heuristic procedure.

## Experimental Design

Each 100-customer instance is evaluated using three disruption start times and
three disruption durations:

- **Disruption Start Times (DST):** 50, 125, and 200 minutes
- **Disruption Durations:** 60, 120, and 180 minutes

This gives nine start-time/duration combinations:

1. **DST = 50 minutes, Duration = 60 minutes**
2. **DST = 50 minutes, Duration = 120 minutes**
3. **DST = 50 minutes, Duration = 180 minutes**
4. **DST = 125 minutes, Duration = 60 minutes**
5. **DST = 125 minutes, Duration = 120 minutes**
6. **DST = 125 minutes, Duration = 180 minutes**
7. **DST = 200 minutes, Duration = 60 minutes**
8. **DST = 200 minutes, Duration = 120 minutes**
9. **DST = 200 minutes, Duration = 180 minutes**

For **each start-time/duration combination, five replications** are performed
using the same five random seeds used in the corresponding 100-customer
experiments:

```text
1040, 1389, 330, 7878, 1923
```

The disruption start time, duration, and corresponding regional disruption
constraints are set before each scenario is run.

## Solution Approach

### 1. Initial TSP Solution

Solve the TSP over all customers using **Concorde (via NEOS)** to obtain a
truck-only tour.

### 2. Impose Perfect-Information Disruption Constraints

Before the truck-and-drone heuristic is applied, specify the disruption
constraints according to the selected disruption start time and duration.

Because the complete disruption schedule is assumed to be known in advance,
these constraints are incorporated into the solution process from the
beginning.

### 3. Truck-Drone Assignment

Provide the Concorde tour together with the disruption constraints to the
heuristic. The heuristic evaluates feasible assignments of customers to the
drone while respecting the applicable disruption constraints.

### 4. Improvement

Because the Concorde truck tour does not necessarily guarantee the best
truck-drone solution, local-search improvement procedures are applied,
including:

- 1-point move
- 2-point move
- 2-opt move

The heuristic also applies its savings/improvement procedures while
maintaining disruption feasibility.

The resulting solution is the best **disruption-aware truck-and-drone
solution with perfect information** obtained by the heuristic.

## Rolling-Disruption Representation

The 100-customer experiments use **five disruption rolls/regions**. For
rolling disruption, the disruption moves sequentially across the five rolls.

The total disruption duration is divided equally among the five regions:

- Duration 60 minutes → **12 minutes per roll**
- Duration 120 minutes → **24 minutes per roll**
- Duration 180 minutes → **36 minutes per roll**

`TCon` contains an initial `[0,0]` entry followed by the five regional
disruption intervals.

### DST = 50

For **Duration = 60**:

```python
TCon = [[0,0], [50,62], [62,74], [74,86], [86,98], [98,110]]
```

For **Duration = 120**:

```python
TCon = [[0,0], [50,74], [74,98], [98,122], [122,146], [146,170]]
```

For **Duration = 180**:

```python
TCon = [[0,0], [50,86], [86,122], [122,158], [158,194], [194,230]]
```

### DST = 125

For **Duration = 60**:

```python
TCon = [[0,0], [125,137], [137,149], [149,161], [161,173], [173,185]]
```

For **Duration = 120**:

```python
TCon = [[0,0], [125,149], [149,173], [173,197], [197,221], [221,245]]
```

For **Duration = 180**:

```python
TCon = [[0,0], [125,161], [161,197], [197,233], [233,269], [269,305]]
```

### DST = 200

For **Duration = 60**:

```python
TCon = [[0,0], [200,212], [212,224], [224,236], [236,248], [248,260]]
```

For **Duration = 120**:

```python
TCon = [[0,0], [200,224], [224,248], [248,272], [272,296], [296,320]]
```

For **Duration = 180**:

```python
TCon = [[0,0], [200,236], [236,272], [272,308], [308,344], [344,380]]
```

For example, when **DST = 200** and **Duration = 60**, the 60-minute
disruption is divided across five regions, giving 12 minutes per region.
The disruption affects Roll 1 from 200–212, Roll 2 from 212–224, and
continues sequentially until Roll 5 from 248–260.

## Total-Disruption Scenarios

The same perfect-information heuristic framework is also used for
**total-disruption** scenarios.

For total disruption, all five regions are disrupted simultaneously for the
full disruption duration.

For **Duration = 60**:

```python
TCon = [[0,0], [0,60], [0,60], [0,60], [0,60], [0,60]]
```

For **Duration = 120**:

```python
TCon = [[0,0], [0,120], [0,120], [0,120], [0,120], [0,120]]
```

For **Duration = 180**:

```python
TCon = [[0,0], [0,180], [0,180], [0,180], [0,180], [0,180]]
```

## Perfect Information vs. Recourse without Perfect Information

The underlying truck-and-drone heuristic, customer data, local-search
procedures, and basic feasibility requirements follow the same general
structure as the corresponding 100-customer heuristic model without perfect
information.

The key difference is **when the disruption information becomes available**:

- **Without perfect information:** an initial plan is followed until the
  disruption start time, after which the system state is identified and the
  remaining problem is replanned.
- **With perfect information:** the full disruption schedule is known before
  planning, so the disruption constraints are imposed from the beginning and
  the solution is constructed with that information already available.

Therefore, the perfect-information model does not require the disruption
snapshot, emergency-landing/recovery-state construction, dummy-node
Hamiltonian-path conversion, or post-disruption replanning steps used in the
without-perfect-information recourse procedure.

## Main Computational Components

The perfect-information notebooks combine:

- Concorde/NEOS for the initial TSP solution
- Truck-and-drone heuristic assignment
- 1-point, 2-point, and 2-opt improvement procedures
- Savings-based improvement
- Five-region disruption constraints imposed from the beginning
- Rolling- and total-disruption scenario settings
- Five replications per start-time/duration combination

## Outputs

The notebooks are used to generate the 100-customer perfect-information
truck-and-drone solutions for the specified disruption scenarios.

The complete experimental results across the disruption start times,
durations, instances, and replications are reported in the corresponding
supplementary Excel files.

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
**100-customer heuristic analysis with perfect information**. The
perfect-information solutions provide an ideal benchmark for evaluating the
value of having the complete disruption schedule available before planning.
