# Large Instances --- Perfect Information Analysis

This Excel workbook contains the computational results supporting the
**Large Instances** analysis in the paper, focusing on the impact of
perfect information on recourse decisions when minimizing makespan. The
workbook also reports computational times, a comparison of rolling and
total disruption makespan, and the paired t-test analysis used to
evaluate the start-time comparisons.

## Excel Workbook

**File:** `Agatz100PerfvsNoPerfGIT4 (fv).xlsx`

The workbook contains **11 worksheets** with computational results,
comparison data, and statistical-analysis results for the 100-customer
instances.

### `NoPerf100(AGATZdisTOTCOMB)`

Contains the results for the **100-customer total-disruption instances
without perfect information**.

Results are reported for the initial solutions and for combinations of
disruption start times (**50, 125, and 200 minutes**) and disruption
durations (**60, 120, and 180 minutes**).

The reported measures include objective function values, truck-only
objective function values, number of drone customers, and computational
time.

### `NoPerf100(AGATZdisROLCOMB)`

Contains the corresponding results for the **100-customer
rolling-disruption instances without perfect information**.

The sheet reports objective function values, truck-only objective
function values, number of drone customers, and computational time
across the considered disruption scenarios.

### `RollingvsTotal`

Contains the **makespan comparison between rolling and total
disruption** for the three disruption start times:

-   Start50: Rolling vs. Total
-   Start125: Rolling vs. Total
-   Start200: Rolling vs. Total

This worksheet provides the data used to compare the effect of the two
disruption patterns on makespan. Across the reported large-instance
comparisons, **rolling disruption produces a 2.4% lower makespan on average
than total disruption**.

### `PerfInf100-1(AGATZdisROLCOM)`

Contains the **100-customer rolling-disruption results with perfect
information** under the considered disruption start times and durations.

The reported measures include objective function values, number of drone
customers, and computational time.

### `rolNoPerfStartvsPerfStartfv`

Contains the makespan data used to compare **Recourse with Perfect
Information** and **Recourse without Perfect Information**, organized by
disruption start time:

-   Start50
-   Start125
-   Start200

These data support the paired t-test analysis.

### `rolNoPerfRecvsPerfRec`

Contains the makespan comparison between **Recourse with Perfect
Information** and **No Recourse**.

### `drone(RollPerfvsNoPer)`

Contains the comparison of the **number of drone sorties/customers**
between Recourse with perfect information and Recourse without perfect
information.

Recourse with perfect information uses **3.5% more drone sorties** on
average than Recourse without perfect information.

## Paired t-test Analysis

The paired t-tests compare Recourse with Perfect Information and
Recourse without Perfect Information separately for the three disruption
start times.

### `Paired_t_test_Summary`

Summarizes the paired t-test results for **Start50, Start125, and
Start200**.

Each comparison contains **300 paired observations**. Statistically
significant differences are found for all three start-time comparisons
(**p \< 0.001**).

### `Start50_Pairs`

Contains the 300 paired observations and paired differences used for the
Start50 t-test.

### `Start125_Pairs`

Contains the 300 paired observations and paired differences used for the
Start125 t-test.

### `Start200_Pairs`

Contains the 300 paired observations and paired differences used for the
Start200 t-test.

## Summary

For the large instances, Recourse with perfect information improves
makespan by **2.3%** compared with Recourse without perfect information
and by **11.9%** compared with No Recourse.

Paired t-tests confirm statistically significant differences between
Recourse with and without perfect information for **Start50, Start125,
and Start200** (**p \< 0.001 for all three comparisons**).

The workbook also includes a direct **rolling-disruption
vs. total-disruption makespan comparison** for Start50, Start125, and
Start200.

The corresponding makespan boxplots are reported in **Figures 8 and 9**
of the paper. Recourse with perfect information also uses **3.5% more
drone sorties** than Recourse without perfect information.

The workbook additionally provides computational times for the
100-customer total- and rolling-disruption experiments.
