# 12-Customer Exact Method --- Disruption Replanning with Recourse

This README documents the **20 Jupyter Notebooks** used for the
12-customer exact-method experiments. The notebooks implement the
truck-and-drone model in **DOcplex/CPLEX**, first compute the initial
no-disruption solution, and then perform **post-disruption replanning**.
The same notebooks are also used for the **No-Recourse** scenario, in
which drone sorties are disabled after the disruption and the remaining
customers are served by the truck.

The same instance notebooks are used for both **rolling-disruption** and
**total-disruption** experiments by changing the disruption-time
parameter `TConstraint2`.

## 12-Customer Instance Notebooks

Each notebook contains the coordinates and instance-specific data for
one of the 20 12-customer instances.

1.  `drone(scen_12_cust_v51DST15GIT).ipynb`
2.  `drone(scen_12_cust_v52DST15GIT).ipynb`
3.  `drone(scen_12_cust_v53DST15GIT).ipynb`
4.  `drone(scen_12_cust_v54DST15GIT).ipynb`
5.  `drone(scen_12_cust_v55DST15GIT).ipynb`
6.  `drone(scen_12_cust_v56DST15GIT).ipynb`
7.  `drone(scen_12_cust_v57DST15GIT).ipynb`
8.  `drone(scen_12_cust_v58DST15GIT).ipynb`
9.  `drone(scen_12_cust_v59DST15GIT).ipynb`
10. `drone(scen_12_cust_v60DST15GIT).ipynb`
11. `drone(scen_12_cust_v70DST15GIT).ipynb`
12. `drone(scen_12_cust_v71DST15GIT).ipynb`
13. `drone(scen_12_cust_v72DST15GIT).ipynb`
14. `drone(scen_12_cust_v73DST15GIT).ipynb`
15. `drone(scen_12_cust_v74DST15GIT).ipynb`
16. `drone(scen_12_cust_v75DST15GIT).ipynb`
17. `drone(scen_12_cust_v76DST15GIT).ipynb`
18. `drone(scen_12_cust_v77DST15GIT).ipynb`
19. `drone(scen_12_cust_v78DST15GIT).ipynb`
20. `drone(scen_12_cust_v79DST15GIT).ipynb`

The filenames above are used for the GitHub supplementary files.

## Experimental Cases

Each of the 20 instances was evaluated under four disruption start-time
and duration combinations:

1.  **DST = 15 minutes, Duration = 30 minutes**
2.  **DST = 15 minutes, Duration = 60 minutes**
3.  **DST = 30 minutes, Duration = 30 minutes**
4.  **DST = 30 minutes, Duration = 60 minutes**

The notebook uploaded for each instance represents its **last-saved/run
configuration**. Therefore, the visible parameter values and notebook
output should not be interpreted as containing the executed results for
all four experimental cases.

The complete computational results for the experimental cases are
reported in the corresponding **Excel result files** included in the
supplementary materials.

## Computational Procedure

The common procedure consists of two stages.

### 1. Initial No-Disruption Solution

The exact truck-and-drone model is first solved without disruption using
DOcplex/CPLEX.

### 2. Post-Disruption Replanning with Recourse

At the specified **Disruption Start Time (`DST`)**, the notebook
determines the current truck and drone locations and identifies the
customers already served.

The served customers are removed from the remaining problem. New nodes
representing the truck and drone locations at the disruption time are
incorporated into the remaining network. The original depot node `0` is
removed, and the current truck location is assigned as the new **node
0**.

The exact model is then solved for the remaining customers under the
specified disruption scenario with **recourse**.

### 3. No-Recourse Scenario

The same notebooks are also used to compute the **No-Recourse** case.
The initial no-disruption solution and the procedure used to determine
the system state at the disruption start time remain the same.

After the disruption starts, drone sorties are disabled in the
second-stage model by imposing:

``` python
mdl.add_constraints(
    (g[i,j] == 0 for i in N0 for j in G2 if i != j),
    names='myD2dr2IFN1'
)
```

Because `g[i,j]` represents a drone launch/service decision, setting
these eligible `g[i,j]` variables to zero prevents new drone sorties in
the post-disruption solution. The remaining delivery service therefore
continues with the **truck only**.

## Rolling and Total Disruption Scenarios

The same notebooks are used for both rolling and total disruption
experiments. The model and post-disruption replanning procedure remain
the same; the disruption pattern is specified through `TConstraint2`.

### Rolling Disruption

For a rolling disruption, the three affected regions are disrupted
consecutively.

For a **30-minute duration**:

``` python
TConstraint2 = [[0,10], [10,20], [20,30]]
```

For a **60-minute duration**:

``` python
TConstraint2 = [[0,20], [20,40], [40,60]]
```

### Total Disruption

For a total disruption, all three regions are disrupted simultaneously.

For a **30-minute duration**:

``` python
TConstraint2 = [[0,30], [0,30], [0,30]]
```

For a **60-minute duration**:

``` python
TConstraint2 = [[0,60], [0,60], [0,60]]
```

Thus, the same instance notebook can be used for rolling or total
disruption by changing `TConstraint2` to represent the corresponding
disruption pattern.

## Main Model Variables and Parameters

### Decision Variables

The principal DOcplex variables used in the formulation include:

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
-   `r[i]` --- binary rescue/rendezvous indicator used in the
    post-disruption model for the designated drone rescue node.

### Rolling-Disruption Auxiliary Variables

-   `binv1[i]`--`binv9[i]` --- auxiliary binary variables used to
    enforce the rolling-disruption timing restrictions for the three
    affected regions. They implement before-or-after timing conditions
    so that relevant drone activities do not occur during the disruption
    interval of the corresponding region.

Other auxiliary variables may be declared in individual implementations,
but they are not central to the common disruption logic described here.

### Main Parameters and Sets

-   `DST` --- Disruption Start Time.
-   `TConstraint2` --- disruption intervals for the three affected
    regions.
-   `e` --- drone endurance parameter.
-   `z1[i,j]` --- truck travel time between nodes `i` and `j`.
-   `z2[i,j]` --- drone travel time between nodes `i` and `j`.
-   `C` --- customer-node set.
-   `N0` --- node set used for possible departures.
-   `N1` --- node set used for possible arrivals/visits.
-   `A` --- feasible directed arc set `(i,j)`.
-   `G1` --- nodes/customers excluded from drone service in the
    corresponding model stage.
-   `G2` --- nodes/customers eligible for drone-related decisions.
-   `MyReg2` --- groups the remaining nodes according to the three
    affected disruption regions.
-   `RN` --- designated drone rescue node used in post-disruption
    replanning.

## Implementation Note

The 20 notebooks follow the same general exact-model and post-disruption
replanning procedure for the Recourse and No-Recourse cases, but some
instance-specific information may be **manually entered or adjusted when
required by the state of the solution**, including information such as
the rescue node (`RN`).

The notebooks should therefore be viewed as the computational
implementation and source of the instance-specific coordinates/data,
while the corresponding Excel files provide the complete reported
experimental results.

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

1.  Select the desired 12-customer instance notebook.
2.  Run the initial exact model to obtain the no-disruption solution.
3.  Set the desired disruption start time (`DST`).
4.  Set `TConstraint2` according to the disruption type and duration.
5.  Determine the truck and drone states at the disruption start time.
6.  Identify and remove customers already served before the disruption.
7.  Construct the remaining network using the current truck and drone
    locations.
8.  Assign the current truck location as the new node `0`.
9.  Apply the corresponding disruption constraints.
10. For the **Recourse** case, solve the remaining problem using the
    post-disruption exact model with drone sorties allowed.
11. For the **No-Recourse** case, add the constraint `g[i,j] == 0` for
    the eligible post-disruption drone-launch variables and solve the
    remaining problem with truck-only service.

## Relation to the Computational Results

These notebooks provide the **12-customer exact-method implementation
and instance-specific data** used for the rolling- and total-disruption
experiments with **Recourse and No Recourse**.

The uploaded notebooks represent the last-saved/run configuration for
each instance. The complete results across the four DST-duration cases
and the corresponding disruption scenarios are reported in the
supplementary Excel result files.
