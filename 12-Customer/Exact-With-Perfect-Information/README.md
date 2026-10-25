# 12-Customer Exact Method --- Rolling Disruption with Perfect Information

This README documents the **20 Jupyter Notebooks** used for the
12-customer exact-method experiments under **perfect information**.

The perfect-information case represents an ideal setting in which all
disruption information is assumed to be known **at the beginning of the
planning horizon**. Therefore, unlike without perfect information
notebooks, these experiments do **not** use a two-stage replanning
procedure. The disruption information is incorporated directly into the
optimization model as constraints, and the complete truck-and-drone
solution is obtained in a **single-stage optimization**.

## 12-Customer Instance Notebooks

Each notebook contains the coordinates and instance-specific data for
one of the 20 12-customer instances.

1.  `drone(scen_12_cust_v51DST15GIT)perinfo.ipynb`
2.  `drone(scen_12_cust_v52DST15GIT)perinfo.ipynb`
3.  `drone(scen_12_cust_v53DST15GIT)perinfo.ipynb`
4.  `drone(scen_12_cust_v54DST15GIT)perinfo.ipynb`
5.  `drone(scen_12_cust_v55DST15GIT)perinfo.ipynb`
6.  `drone(scen_12_cust_v56DST15GIT)perinfo.ipynb`
7.  `drone(scen_12_cust_v57DST15GIT)perinfo.ipynb`
8.  `drone(scen_12_cust_v58DST15GIT)perinfo.ipynb`
9.  `drone(scen_12_cust_v59DST15GIT)perinfo.ipynb`
10. `drone(scen_12_cust_v60DST15GIT)perinfo.ipynb`
11. `drone(scen_12_cust_v70DST15GIT)perinfo.ipynb`
12. `drone(scen_12_cust_v71DST15GIT)perinfo.ipynb`
13. `drone(scen_12_cust_v72DST15GIT)perinfo.ipynb`
14. `drone(scen_12_cust_v73DST15GIT)perinfo.ipynb`
15. `drone(scen_12_cust_v74DST15GIT)perinfo.ipynb`
16. `drone(scen_12_cust_v75DST15GIT)perinfo.ipynb`
17. `drone(scen_12_cust_v76DST15GIT)perinfo.ipynb`
18. `drone(scen_12_cust_v77DST15GIT)perinfo.ipynb`
19. `drone(scen_12_cust_v78DST15GIT)perinfo.ipynb`
20. `drone(scen_12_cust_v79DST15GIT)perinfo.ipynb`

## Perfect-Information Assumption

In these experiments, the disruption start time, disruption duration,
and the sequential movement of the disruption across the three affected
regions are assumed to be known before the delivery plan is generated.

Consequently:

-   there is no initial solution followed by post-disruption replanning;
-   there is no need to determine truck/drone locations at the
    disruption start time and reconstruct the remaining network;
-   the disruption restrictions are included in the model from the
    beginning; and
-   the model is solved once to obtain a solution that accounts for the
    known future disruption.

This represents the **ideal perfect-information benchmark** used in the
computational analysis.

## Experimental Cases

Each of the 20 instances is evaluated under four disruption scenarios:

1.  **DST = 15 minutes, Duration = 30 minutes**
2.  **DST = 15 minutes, Duration = 60 minutes**
3.  **DST = 30 minutes, Duration = 30 minutes**
4.  **DST = 30 minutes, Duration = 60 minutes**

For each scenario, `TConstraint2` is adjusted to represent the known
disruption interval for each of the three rolls.

The notebook uploaded for each instance represents its **last-saved/run
configuration**. The visible parameter values and saved notebook output
therefore should not be interpreted as containing the executed results
for all four cases. The complete computational results are reported in
the corresponding supplementary Excel files.

## Rolling-Disruption Representation

The disruption moves sequentially across three rolls/regions. The total
disruption duration is divided equally among the three rolls.

### DST = 15, Duration = 30

Each roll is disrupted for 10 minutes:

``` python
TConstraint2 = [[15,25], [25,35], [35,45]]
```

This means:

-   Roll 1: disruption is effective from time 15 to 25
-   Roll 2: disruption is effective from time 25 to 35
-   Roll 3: disruption is effective from time 35 to 45

### DST = 15, Duration = 60

Each roll is disrupted for 20 minutes:

``` python
TConstraint2 = [[15,35], [35,55], [55,75]]
```

### DST = 30, Duration = 30

Each roll is disrupted for 10 minutes:

``` python
TConstraint2 = [[30,40], [40,50], [50,60]]
```

### DST = 30, Duration = 60

Each roll is disrupted for 20 minutes:

``` python
TConstraint2 = [[30,50], [50,70], [70,90]]
```

The appropriate `TConstraint2` setting is selected before solving the
corresponding perfect-information scenario.

## Main Model Variables and Parameters

The core truck-and-drone variables and basic model structure are the
same as in the 12-customer exact formulation.

### Decision Variables

-   `x[i,j]` --- binary truck-routing variable; equals 1 when the truck
    travels directly from node `i` to node `j`.
-   `g[i,j]` --- binary drone launch/service variable; identifies a
    drone operation launched from node `i` to serve customer `j`.
-   `h[j,k]` --- binary drone rendezvous variable; identifies the drone
    movement from drone-served customer `j` to rendezvous node `k`.

Together, `g[i,j]` and `h[j,k]` represent a drone sortie:

``` text
launch node i -> drone-served customer j -> rendezvous node k
```

-   `t[i]` --- continuous timing variable used for truck/drone timing
    and synchronization.
-   `w[i]` --- continuous truck waiting-time variable associated with
    truck-drone synchronization.
-   `d[i]` --- continuous drone timing variable used to track drone
    arrival/service timing.
-   `d1[i]` --- continuous drone departure/launch-time variable.
-   `m[i]` --- binary indicator associated with a drone rendezvous at
    node `i`.
-   `m2[i]` --- binary indicator associated with a drone launch from
    node `i`.
-   `v[i]` --- binary coordination/status variable used in truck-drone
    feasibility and synchronization constraints.

### Disruption Auxiliary Variables

-   `binv1[i]`--`binv9[i]` --- auxiliary binary variables used to
    enforce the disruption timing restrictions for the three affected
    rolls. They implement before-or-after timing conditions so that
    relevant drone activities do not occur during the known disruption
    interval of the corresponding roll.

Other auxiliary variables may be declared in the implementation but are
not central to the common perfect-information disruption logic described
here.

### Main Parameters and Sets

-   `TConstraint2` --- known disruption intervals for the three rolls.
-   `e` --- drone endurance parameter.
-   `z1[i,j]` --- truck travel time between nodes `i` and `j`.
-   `z2[i,j]` --- drone travel time between nodes `i` and `j`.
-   `C` --- customer-node set.
-   `N0` --- node set used for possible departures.
-   `N1` --- node set used for possible arrivals/visits.
-   `A` --- feasible directed arc set `(i,j)`.
-   `G1` --- nodes/customers excluded from drone service in the
    corresponding model.
-   `G2` --- nodes/customers eligible for drone-related decisions.
-   `MyReg2` --- groups the nodes according to the three affected
    disruption regions.

## Disruption Constraints

Because the disruption schedule is known before optimization, the
corresponding disruption intervals are imposed directly when the model
is solved.

For each roll, the model restricts relevant drone launch, service, and
rendezvous activities so that they occur either **before the disruption
begins** or **after the disruption interval ends** for that region.

The `binv1`--`binv9` variables are used to implement these timing
alternatives within the mixed-integer formulation.

## Requirements

The notebooks use:

-   Python
-   NumPy
-   pandas
-   Matplotlib
-   IBM DOcplex
-   IBM ILOG CPLEX Optimizer

A working CPLEX installation/license is required to solve the
optimization models through DOcplex.

## How to Run

1.  Select the desired 12-customer perfect-information notebook.
2.  Select the desired disruption start time and duration.
3.  Set `TConstraint2` to the corresponding three rolling-disruption
    intervals.
4.  Run the exact DOcplex model with the disruption constraints included
    from the beginning.
5.  Record the resulting perfect-information solution for the
    corresponding experimental case.

## Relation to the Computational Results

These notebooks provide the **12-customer exact-method implementation
and instance-specific data for the rolling-disruption experiments with
perfect information**.

The perfect-information solutions serve as an ideal benchmark because
the complete disruption schedule is assumed to be known before the
delivery plan is optimized.

The uploaded notebooks represent the last-saved/run configuration for
each instance. The complete results across all four disruption
start-time and duration combinations are reported in the corresponding
supplementary Excel result files.
