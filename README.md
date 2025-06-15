# Analysis of Variance (ANOVA)

## Table of Contents
1. [Introduction](#introduction)  
2. [Why Use ANOVA?](#why-use-anova)  
3. [Types of ANOVA](#types-of-anova)  
4. [Key Concepts & Terminology](#key-concepts--terminology)  
5. [Statistical Model & Hypotheses](#statistical-model--hypotheses)  
6. [Assumptions](#assumptions)  
7. [ANOVA Table & Formulas](#anova-table--formulas)  
8. [Step-by-Step Example](#step-by-step-example)  
9. [Implementations](#implementations)  
   - [Python (SciPy)](#python-scipy)  
   - [Excel](#excel)  
10. [Interpreting Results](#interpreting-results)  
11. [Limitations & Alternatives](#limitations--alternatives)  
12. [References](#references)  

---

## Introduction  
Analysis of Variance (ANOVA) is a collection of statistical models and their associated estimation procedures used to analyze the differences among group means. Invented by Ronald A. Fisher in the 1920s, ANOVA extends the two-sample _t_-test to more than two groups by partitioning variability in the data into components.

---

## Why Use ANOVA?  
- **Multiple Groups**: When comparing means across three or more independent samples.  
- **Control Type I Error**: Conducting multiple _t_-tests inflates the probability of a false positive; ANOVA controls the overall significance level.  
- **Decompose Variance**: Separates total variability into “between‐group” and “within‐group” components, helping to pinpoint sources of variation.

---

## Types of ANOVA  
| Type             | Design                           | Notes                                                  |
|------------------|----------------------------------|--------------------------------------------------------|
| **One-Way ANOVA**| One categorical factor           | Tests whether 𝜇₁ = 𝜇₂ = … = 𝜇ₖ for _k_ groups.         |
| **Two-Way ANOVA**| Two categorical factors          | Can test main effects and interaction effect.          |
| **Repeated Measures ANOVA** | Same subjects at multiple levels | Accounts for correlation between repeated observations. |
| **Mixed-Effects ANOVA** | Fixed and random effects         | Models both population‐level and subject‐level effects. |

---

## Key Concepts & Terminology  
- **Group Mean (𝑥̄ᵢ)**: Average of observations in group _i_.  
- **Grand Mean (𝑥̄)**: Average across all observations (all groups).  
- **Between‐Group Sum of Squares (SS<sub>B</sub>)**:  
  \[
    SS_B = \sum_{i=1}^{k} n_i (x̄_i - x̄)^2
  \]  
- **Within‐Group Sum of Squares (SS<sub>W</sub>)**:  
  \[
    SS_W = \sum_{i=1}^{k} \sum_{j=1}^{n_i} (x_{ij} - x̄_i)^2
  \]  
- **Total Sum of Squares (SS<sub>T</sub>)**:  
  \[
    SS_T = SS_B + SS_W = \sum_{i=1}^{k} \sum_{j=1}^{n_i} ( x_{ij} - x̄ )^2
  \]  
- **Degrees of Freedom (df)**:  
  - Between: _k–1_  
  - Within: _N–k_  
  - Total: _N–1_  
- **Mean Squares (MS)**: SS divided by corresponding df.  
  - MS<sub>B</sub> = SS<sub>B</sub> / (k–1)  
  - MS<sub>W</sub> = SS<sub>W</sub> / (N–k)  
- **F-Statistic**:  
  \[
    F = \frac{MS_B}{MS_W}
  \]

---

## Statistical Model & Hypotheses  
For a one‐way ANOVA with _k_ groups and observations _x<sub>ij</sub>_:

\[
  x_{ij} = \mu + \tau_i + \varepsilon_{ij}, \quad \varepsilon_{ij}\sim N(0,\sigma^2)
\]

- **Null Hypothesis (H₀)**: All group means are equal:  
  \[
    H_0:\; \mu_1 = \mu_2 = \dots = \mu_k
  \]
- **Alternative Hypothesis (H₁)**: At least one group mean differs.

---

## Assumptions  
1. **Independence**: Observations are independent.  
2. **Normality**: Residuals _ε<sub>ij</sub>_ are normally distributed.  
3. **Homoscedasticity**: Equal variances across groups (σ² common).  

> _Note_: If homoscedasticity fails, consider Welch’s ANOVA or non‐parametric Kruskal–Wallis test.

---

## ANOVA Table & Formulas  

| Source        | SS         | df     | MS              | F               |
|---------------|------------|--------|-----------------|-----------------|
| Between (B)   | SS<sub>B</sub> | k–1    | MS<sub>B</sub>=SS<sub>B</sub>/(k–1) | F=MS<sub>B</sub>/MS<sub>W</sub> |
| Within (W)    | SS<sub>W</sub> | N–k    | MS<sub>W</sub>=SS<sub>W</sub>/(N–k) |                 |
| **Total (T)** | SS<sub>T</sub> | N–1    |                 |                 |

- **Critical F-value**: _F<sub>crit</sub> = F<sub>α; df1=k–1; df2=N–k</sub>_  
- **P-value**: _P = P(F ≥ observed F)_

---

## Step-by-Step Example  
Suppose three fertilizers (A, B, C) tested on crop yield (kg) over _n = 5_ plots each:

| Plot | A   | B   | C   |
|------|-----|-----|-----|
| 1    | 30  | 28  | 35  |
| 2    | 32  | 26  | 33  |
| 3    | 31  | 27  | 36  |
| 4    | 29  | 25  | 34  |
| 5    | 33  | 29  | 37  |

1. **Compute group means**:  
   - 𝑥̄<sub>A</sub> = 31, 𝑥̄<sub>B</sub> = 27, 𝑥̄<sub>C</sub> = 35  
   - Grand mean 𝑥̄ = (31+27+35)/3 = 31  

2. **Compute SS<sub>B</sub>**:  
   \[
     SS_B = 5[(31-31)^2 + (27-31)^2 + (35-31)^2]
           = 5[0 + 16 + 16] = 160
   \]

3. **Compute SS<sub>W</sub>**:  
   \[
     SS_W = \sum_i \sum_j (x_{ij} - x̄_i)^2
           = \ldots = 40  \quad(\text{details omitted for brevity})
   \]

4. **Degrees of freedom**:  
   - df<sub>B</sub> = 3–1 = 2  
   - df<sub>W</sub> = 15–3 = 12  

5. **Mean squares**:  
   - MS<sub>B</sub> = 160 / 2 = 80  
   - MS<sub>W</sub> = 40 / 12 ≈ 3.333  

6. **F-statistic**:  
   \[
     F = 80 / 3.333 ≈ 24
   \]

7. **Decision**:  
   - For α = 0.05, df1 = 2, df2 = 12, _F<sub>crit</sub>_ ≈ 3.89.  
   - Observed F (24) > 3.89 ⇒ reject H₀ ⇒ at least one mean differs.

---

## Implementations

### Python (SciPy)
```python
import numpy as np
from scipy import stats

# Data
A = np.array([30, 32, 31, 29, 33])
B = np.array([28, 26, 27, 25, 29])
C = np.array([35, 33, 36, 34, 37])

F_stat, p_value = stats.f_oneway(A, B, C)
print(f"F = {F_stat:.3f}, p = {p_value:.3f}")
