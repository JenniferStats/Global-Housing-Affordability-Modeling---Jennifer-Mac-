# Global Housing Affordability Analysis

A statistical analysis of rental affordability across 121 countries, exploring what economic indicators actually drive rent prices — and why the U.S. is pricing people out.

*Jennifer Mac — Department of Statistics and Data Science, CSU East Bay*

---

## The Motivation

Housing in the U.S. has gotten out of hand. Affording an average two-bedroom rental now requires working nearly **4x the national minimum wage** — and wages aren't keeping up. So the question becomes: is this a uniquely American problem, or is unaffordable housing a global pattern?

This project digs into that question using country-level cost of living data, building a model to predict rent prices from broader economic indicators and mapping out where in the world housing is actually affordable.

---

## The Data

**Source:** [Cost of Living Index by Country (Numbeo, 2024)](https://www.numbeo.com/cost-of-living/rankings.jsp) via Kaggle — 121 countries, all indexed relative to New York City.

> An index of **100** = price parity with NYC. An index of **60** means ~60% of NYC prices.

| Variable | What It Measures |
|---|---|
| **Cost of Living Index** *(excl. rent)* | Everyday expenses — an index of 100 ≈ $4,400/month |
| **Rent Index** | Average cost to rent a home — an index of 100 ≈ $4,400/month |
| **Local Purchasing Power Index** | What residents can actually afford on local wages |
| **Food Index** *(composite)* | Grocery costs relative to restaurant costs |

The Grocery Index and Restaurant Index were combined into a single **Food Index** due to high correlation between the two.

---

## The Model

**Goal:** Predict the rent index from cost of living, purchasing power, and food costs.

Model selection used **AIC (Akaike Information Criterion)**. Because Rent Index and Cost of Living Index were both right-skewed, a **log transformation** was applied to both before modeling.

**Final model:**

```
log(rent_index) = -2.096
                + 1.269 · log(cost_of_living)
                + 0.003 · local_purchasing_power
                - 0.002 · food_index
```

**R² = 0.7722** — the model explains ~77% of the variation in rental costs across countries.

Model assumptions (normality, homoscedasticity) were verified via residual plots. ✓

---

## Key Findings

**Cost of living is by far the strongest driver of rent.**
A 1% increase in the cost of living index is associated with a 1.27% increase in the rent index. This makes intuitive sense — rent doesn't exist in a vacuum; it scales with the broader economic environment.

**Purchasing power has a modest positive effect.**
Higher local purchasing power is weakly associated with higher rent, suggesting that when people can afford more, landlords charge more.

**The food index has a slight negative effect.**
Countries where restaurants are cheaper than groceries tend to have lower rents. One possible explanation: cheaper dining options may reflect a local economy that doesn't cater to high-income residents — and lower-income areas tend to have lower rents.

**The model is better at the country level than the city level.**
Cities are often the priciest pockets within a country, with more volatile housing markets and income distributions. The model's accuracy drops when applied to cities — a known and expected limitation.

---

## Highlights from the Data

| Country | Notable For |
|---|---|
| 🇸🇬 Singapore | Highest rent index in the dataset |
| 🇧🇩 Bangladesh | Lowest rent index in the dataset |
| 🇳🇬 Nigeria | Rent much higher than model predicts (outlier) |
| 🇨🇲 Cameroon | Similar pattern — actual rent exceeds predictions |
| 🇺🇸 United States | High across all variables; rent closely tracks model |
| 🇸🇦 Saudi Arabia / 🇴🇲 Oman | Rent lower than model predicts given cost of living |

---

## Repository Structure

```
├── README.md
├── data/
│   └── cost_of_living_2024.csv      # Source: Numbeo via Kaggle
├── analysis/
│   └── housing_affordability.R      # Full model + plots
└── figures/
    ├── predicted_vs_actual.png      # Country-level & city-level fit
    ├── residuals_qq.png             # Model diagnostics
    └── sparkline_indexes.png        # Country comparisons across all variables
```

---

## References

- National Low Income Housing Coalition. ["About Out of Reach."](https://nlihc.org/oor/about)
- IBISWorld. ["National Average Minimum Wage."](https://www.ibisworld.com/united-states/bed/national-average-minimum-wage/112701/) October 2023.
- Numbeo. ["Motivation and Methodology."](https://www.numbeo.com/common/motivation_and_methodology.jsp)
- Vieira, Matheus. ["Global Cost of Living."](https://www.kaggle.com/datasets/mvieira101/global-cost-of-living/data) Kaggle.
- Myrios. ["Cost of Living Index by Country 2024."](https://www.kaggle.com/datasets/myrios/cost-of-living-index-by-country-by-number-2024) Kaggle.
- Mac, Jennifer. ["NYC Cost of Living Data Table."](https://docs.google.com/spreadsheets/d/14tEH_z6PPvImQGL3bpxUC_uS2xM-jR9IBpxanjbsvO8/edit?usp=sharing) Google Sheets.

---

*CSU East Bay — Department of Statistics and Data Science*
