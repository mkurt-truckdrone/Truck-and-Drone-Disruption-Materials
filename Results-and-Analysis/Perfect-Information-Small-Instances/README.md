# Small Instances — Perfect Information Analysis

This Excel workbook contains the computational results supporting the **Small Instances** analysis in Section **4.3.1** of the paper, focusing on the impact of perfect information on recourse decisions when minimizing makespan.

## Excel Workbook

**File:** `12cus20INS(PerftvsNoPerfGIT).xlsx`

The workbook contains seven worksheets.

### `12custPerfectResults`

Contains the detailed exact-method results for the **12-customer instances with perfect information** under the considered disruption start times and durations.

The reported measures include makespan, truck-only objective function value, number of drone customers, truck waiting time, and computational time.

### `NOPERRECvsPERREC(makespan)`

Compares the **makespan** obtained by:

- Recourse without perfect information
- Recourse with perfect information

The results show that **Recourse with perfect information improves makespan by 7.6% on average** compared with Recourse without perfect information.

### `NOPERRECvsPERREC(drone)`

Compares the **number of drone sorties/customers** for Recourse with and without perfect information.

Recourse with perfect information uses **4.5% more drone sorties** on average than Recourse without perfect information.

### `NOPERNORECvsPERREC(makespan)`

Compares the **makespan** obtained by:

- No Recourse
- Recourse with perfect information

Recourse with perfect information improves makespan by **19.9% on average** compared with No Recourse.

### `NOPERNORECvsPERREC(drone)`

Compares the **number of drone sorties/customers** between No Recourse and Recourse with perfect information.

Recourse with perfect information uses **143% more drone sorties** than No Recourse.

## Paired t-Test Analysis

The workbook also includes the paired t-test calculations supporting the statistical-significance statements reported in Section **4.3.1**.

### `NOPERRECvsPERREC(paired)`

Contains the paired makespan observations and Excel **paired two-sample t-test** results comparing:

- Recourse without perfect information
- Recourse with perfect information

The analysis uses **80 paired observations**. The test gives **t(79) = 12.85** with a two-tailed **p < 0.001**, confirming a statistically significant difference between the two recourse solutions.

### `NOPERNORECvsPERREC(paired)`

Contains the paired makespan observations and Excel **paired two-sample t-test** results comparing:

- No Recourse
- Recourse with perfect information

The analysis uses **80 paired observations**. The test gives **t(79) = 17.60** with a two-tailed **p < 0.001**, confirming a statistically significant difference between No Recourse and Recourse with perfect information.

## Summary

For the small instances, Recourse with perfect information improves makespan by **7.6%** compared with Recourse without perfect information and by **19.9%** compared with No Recourse. The paired t-tests confirm that **both makespan differences are statistically significant (p < 0.001)**.

Recourse with perfect information also uses **4.5% more drone sorties** than Recourse without perfect information and **143% more drone sorties** than No Recourse.

