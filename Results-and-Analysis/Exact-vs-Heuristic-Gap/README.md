# Exact vs. Heuristic Comparison — 12-Customer Instances

This Excel workbook compares the **exact** and **heuristic** solution methods
for the 12-customer rolling-disruption instances. The purpose of the analysis
is to evaluate the quality of the heuristic by measuring its percentage gap
relative to the exact solution.

## Excel Workbook

**File:** `HeuristicvsExact(GapCalc).xlsx`

The workbook contains the worksheet:

- `ExactvsHeur(GapCalc)`

## Experimental Comparisons

The exact and heuristic solutions are compared for the four 12-customer
disruption start-time and duration combinations:

1. **DST = 15 minutes, Duration = 30 minutes**
2. **DST = 15 minutes, Duration = 60 minutes**
3. **DST = 30 minutes, Duration = 30 minutes**
4. **DST = 30 minutes, Duration = 60 minutes**

The workbook contains the underlying computational results used to calculate
the average exact-versus-heuristic gap for each case.

## Average Gap Results

The comparison produces the following average gaps:

| Scenario | Average Gap (%) |
| --- | ---: |
| START15 DUR30 — Exact vs. Heuristic | 1.4 |
| START15 DUR60 — Exact vs. Heuristic | 2.6 |
| START30 DUR30 — Exact vs. Heuristic | 2.7 |
| START30 DUR60 — Exact vs. Heuristic | 4.8 |
| **Overall Average Gap** | **2.9** |

Across the four disruption scenarios, the overall average gap between the
exact and heuristic solutions is approximately **2.9%**.

## Relation to the Paper

This workbook provides the computational support for **Table 3: Average gaps
between exact and heuristic methods** in the paper.

The results confirm the finding reported in Table 3 that the heuristic
solutions remain close to the exact solutions across the four 12-customer
disruption scenarios, with an overall average gap of **2.9%**.
