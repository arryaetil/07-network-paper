# Network Centrality in Global Value Chains and Worker Wages

**Maastricht University · EBC2109 Network Economics · May 2026**

Hoang Viet Nguyen · Anton Viakkerev · Arrya Willems  
*Supervised by Prof. Robin Cowan and Mario Alberto Macchioni*

---

## Research Question

> **Do country-industries in more central positions in global value chains pay higher wages?**

We construct a production network from the World Input-Output Database (WIOD) and test whether structural brokerage — measured by betweenness centrality — predicts log wages at the country-industry level, after controlling for trade volumes and GVC integration depth.

| Hypothesis | Statement |
|---|---|
| **H₀** | Network centrality does not predict log wages |
| **Hₐ** | Country-industries with higher betweenness centrality pay higher log wages |

---

## Data

| File | Source | Variables | Role |
|---|---|---|---|
| `WIOT2014_October16_ROW` | WIOD 2016 Release | Bilateral intermediate flows, 44×56 country-industries | Build network; compute centrality & GVC indices |
| `Socio_Economic_Accounts.xlsx` | WIOD SEA | COMP (compensation), EMPE (employees) | Compute log wage |
| `Exchange_Rates.xlsx` | WIOD SEA | EXR: USD per unit of local currency | Convert wages to USD |

**Sample:** 2,171 country-industry observations (2014 cross-section). ROW excluded (no wage data); China excluded (EMPE missing from WIOD 2016 release).

### Descriptive Statistics

| Variable | N | Mean | SD | Min | Max |
|---|---|---|---|---|---|
| Log Wage (USD) | 2,171 | 3.51 | 0.96 | −1.15 | 6.48 |
| In-strength ($M) | 2,171 | 22,471 | 61,218 | 0 | 1,158,375 |
| Out-strength ($M) | 2,171 | 22,613 | 59,882 | 0 | 975,794 |
| Betweenness (norm.) | 2,171 | 0.0003 | 0.001 | 0 | 0.011 |
| Backward GVC | 2,171 | 0.161 | 0.142 | 0 | 1.00 |
| Forward GVC | 2,171 | 0.181 | 0.204 | 0 | 1.00 |

---

## The Production Network

The network is **directed** and **weighted**: nodes are country-industry pairs; a directed edge from *i* to *j* carries the dollar value of intermediate inputs that *i* supplies to *j*. Edges below $1M are filtered (89% of all edges — noise at the 90th percentile).

### Network-Level Statistics

| Statistic | Value | Interpretation |
|---|---|---|
| Nodes | 2,464 | 44 countries × 56 industries |
| Edges (filtered) | 553,929 | Significant trade relationships |
| Density | 9.1% | Sparse — meaningful positional variation |
| Global clustering | 0.468 | Fairly clustered supply chains |
| Mean path length | 3.28 hops | Small-world structure |
| Diameter | 5 hops | Every node reachable in ≤5 steps |
| Degree assortativity | −0.153 | Hub-and-spoke: hubs link to small nodes |

### Path Length Distribution

<img src="plots/01_path_length_distribution.png" width="700"/>

Most country-industry pairs are 3 hops apart — a compact, small-world topology consistent with densely interconnected global supply chains.

### Top 30 Broker Nodes

<img src="plots/08_network_top30_brokers.png" width="820"/>

Node size scales with betweenness centrality; edge width scales with log trade value. Broker nodes are concentrated in manufacturing-heavy economies (DEU, USA, CHN, JPN), sitting at the intersection of multiple otherwise-disconnected supply chain segments.

---

## Variable Relationships

### Wage Distribution by Income Group

<img src="plots/03_wage_distribution.png" width="720"/>

Log wages are right-skewed within each income group and show clear stratification: high-income country-industries cluster above log wage = 3.5, while lower-middle-income nodes cluster below 2.5.

### Correlation Matrix

<img src="plots/02_correlation_matrix.png" width="620"/>

Betweenness has the **highest bivariate correlation with log wages** (r = 0.143) among the network variables — stronger than either in-strength (0.103) or out-strength (0.124). GVC participation indices show moderate positive correlations with wages.

### Betweenness Centrality vs. Log Wage

<img src="plots/04_betweenness_vs_wage.png" width="720"/>

A clear positive gradient: industries occupying broker positions — bridging otherwise unconnected supply chain segments — consistently pay higher wages. This relationship holds across all income groups.

### GVC Participation vs. Log Wage

<img src="plots/05_gvc_vs_wage.png" width="820"/>

Both backward integration (reliance on imported inputs) and forward integration (feeding into others' production) are positively associated with wages, consistent with GVC participation raising productivity and compensation.

---

## Regression Results

Three specifications isolate the betweenness effect step by step, plus a fourth that adds country fixed effects:

| | M1: Baseline | M2: + Betweenness | M3: Full | M4: Country FE |
|---|---|---|---|---|
| In-strength (bn USD) | 0.0003 | −0.0001 | 0.0002 | −0.0003 |
| Out-strength (bn USD) | **0.002\*\*\*** | **0.001\*\*** | **0.002\*\*\*** | **0.001\*\*** |
| **Betweenness centrality** | — | **163.7\*\*\*** | **119.4\*\*\*** | **43.5\*\*\*** |
| Backward GVC | — | — | **0.632\*\*\*** | **0.264\*\*** |
| Forward GVC | — | — | **0.288\*\*** | 0.104 |
| Country FE | No | No | No | Yes (43 dummies) |
| R² | 0.015 | 0.026 | 0.044 | 0.746 |
| N | 2,171 | 2,171 | 2,171 | 2,171 |

*Heteroskedasticity-robust standard errors (HC1). \*p<0.10, \*\*p<0.05, \*\*\*p<0.01. Country dummy coefficients omitted from M4 for brevity.*

### Standardised Coefficients — Full Model

<img src="plots/06_regression_coefficients.png" width="760"/>

Standardised betas confirm that **betweenness centrality has the largest and most precisely estimated effect** on log wages among all predictors, followed by backward GVC participation.

### Explained Variance Across Specifications

<img src="plots/07_r2_progression.png" width="580"/>

Each specification adds explanatory power. The low overall R² (4.4%) is expected: 2,171 observations spanning Indonesia and Luxembourg introduce vast unobserved heterogeneity. Industry fixed effects and a panel setting are the natural extension; country fixed effects are added in M4 (see Robustness).

---

## Robustness

**The broker premium survives within-country comparisons and alternative network thresholds.**

**Country fixed effects (M4).** Adding 43 country dummies to the full specification absorbs all cross-country variation (R² rises from 0.044 to 0.746) — wages are now compared *within* a country across industries. Betweenness centrality remains positive and highly significant: **coefficient 43.455 (SE 13.695, p ≈ 0.002)**. This means industries that broker between supply-chain segments pay more *relative to other industries in the same country*, not just relative to industries in other (richer) countries.

**Threshold robustness.** The main results filter edges below $1M. Rebuilding the network at alternative cutoffs — $500K and $5M — and recomputing betweenness, in-strength and out-strength, the Model 2 specification (`log_wage_usd ~ in-strength + out-strength + betweenness`) gives:

| | $500K cutoff | $5M cutoff |
|---|---|---|
| Betweenness centrality | **217.07\*\*\*** (32.69) | **90.49\*\*\*** (17.16) |
| N | 2,171 | 2,171 |

*Robust SEs (HC1) in parentheses; \*\*\*p<0.001. Full table in `robustness_table.txt`.*

Betweenness remains highly significant (p < 0.001) at both alternative thresholds — the result is not an artefact of the $1M filtering choice.

**Bottom line:** betweenness remains significant within countries (p ≈ 0.002), so the broker premium is not a rich-country artifact.

---

## Conclusions

**H₀ is rejected. Betweenness centrality significantly predicts higher log wages (p < 0.001), robust across all three specifications.**

1. **Brokers pay more.** A one-standard-deviation increase in betweenness is associated with 0.08–0.11 SD higher log wages (depending on specification) — the strongest network effect in the model.

2. **The broker premium is not explained by trade volume.** Betweenness remains significant and large after controlling for in- and out-strength, showing the wage premium stems from structural position, not raw trading size.

3. **GVC integration controls do not eliminate the effect.** Adding backward and forward GVC participation (M3) reduces the betweenness coefficient from 163.7 to 119.4 (−27%), but it remains highly significant. Some of the broker premium overlaps with integration depth, but most is independently attributable to network position.

4. **Out-strength, not in-strength, matters.** Industries supplying inputs to others (forward-linked) earn more than industries that merely buy a lot. Selling into supply chains confers more bargaining power than buying from them.

5. **GVC integration raises wages.** Both backward and forward participation are positive and significant, consistent with the productivity-enhancing role of global integration documented in the literature (Timmer et al., 2015).

---

## Limitations

- **Cross-section (2014 only):** causal inference is limited; network position and wages are jointly determined.
- **Single cross-section, no industry fixed effects:** country fixed effects are now included (M4), but industry wage premia and time variation remain unmodelled; a panel with node and year fixed effects is the natural extension.
- **China excluded:** missing EMPE in WIOD 2016 removes 56 observations from the world's largest manufacturing node.

---

## How to Run

```r
# 1. Install dependencies (first run only)
install.packages(c("igraph", "readxl", "dplyr", "corrplot",
                   "sandwich", "lmtest", "stargazer", "car",
                   "ggplot2", "scales"))

# 2. Set the working directory in line 15 of 'working file.R'
#    setwd("/your/path/to/07-network-paper")

# 3. Run the script
source("working file.R")
```

All figures are saved to `plots/`. Regression tables are saved to `descriptives.txt` and `regression_table.txt`.

---

## References

- Burt, R. S. (2001). Structural holes versus network closure as social capital. *Social Capital: Theory and Research*, 31–56.
- Mahutga, M. C. (2014). Global models of networked organisation, the positional power of nations and economic development. *Review of International Political Economy*, 21(1), 157–194.
- Timmer, M. P., Dietzenbacher, E., Los, B., Stehrer, R., & de Vries, G. J. (2015). An illustrated user guide to the World Input-Output Database. *Review of International Economics*, 23(3), 575–605.
