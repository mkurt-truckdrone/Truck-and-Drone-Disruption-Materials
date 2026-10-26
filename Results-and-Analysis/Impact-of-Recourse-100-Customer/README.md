# 100-Customer Disruption Analysis — Heuristic Method

This Excel workbook contains the computational results and analysis for the **100-customer instances** obtained using the heuristic method under different disruption scenarios.

## Excel Workbook

**File:** `Agatz100custDISR5REP(ALL20) (version1GIT).xlsx`

The workbook contains two worksheets.

### `AGATZROL100-custanalysis`

This worksheet presents the computational results and analysis for the **100-customer instances** under different disruption scenarios.

The disruption scenarios consider combinations of:

- Disruption start times: **50, 125, and 200**
- Disruption durations: **60, 120, and 180**

The sheet reports and compares solution characteristics across the considered scenarios, including objective function values, truck-only objective function values, number of customers served by drones, and computational time.

### `rollingSTART(RECvsNoREC`

This worksheet presents the **Recourse vs. No-Recourse comparison** for the 100-customer instances.

The results are summarized using **boxplots** to compare the objective function values obtained with recourse and without recourse for different disruption start times.

The boxplots provide a visual comparison of the impact of allowing recourse across the considered disruption scenarios.
