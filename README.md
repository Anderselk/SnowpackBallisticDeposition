# SnowpackBallisticDeposition
Simulation of snowpack formation using a ballistic deposition model with spatially correlated snowfall to study weak-layer formation and avalanche risk.
# Snowpack Ballistic Deposition Model

## Overview
Avalanches are one of the most dangerous natural hazards in mountainous regions, often triggered by weak layers within the snowpack. These weak layers arise when successive snowfall events deposit structurally distinct layers with poor cohesion.

This project models snowpack formation using a **1D ballistic deposition framework**, extended to include:
- Spatially correlated snowfall (storm structure)
- Realistic storm size distributions (log-normal)
- Layer-by-layer statistical analysis

The goal is to understand how **storm spatial structure influences snowpack heterogeneity** and whether this can help identify avalanche-prone conditions.

---

## Key Research Question
How does **spatial correlation in snowfall deposition** affect the structure of snowpack layers, and can this be used to identify weak-layer formation associated with avalanche risk?

---

## Methodology

### Ballistic Deposition Model
- 1D lattice of width `L`
- Particles fall vertically and stick according to:

\[
h_i = \max(h_{i-1}, h_i, h_{i+1}) + 1
\]

- Produces rough, correlated surfaces consistent with the **KPZ universality class**

---

### Spatial Correlation
- Uncorrelated case (`σ = 0`): uniform random deposition
- Correlated case (`σ > 0`):
  - Gaussian-smoothed noise determines deposition probabilities
  - Mimics storm systems depositing snow unevenly across space

---

### Storm Modeling
- Storm sizes drawn from a **log-normal distribution**
- Captures real-world snowfall behavior:
  - Many small storms
  - Few extreme events

---

### Layer Definition
Each storm creates a layer:

\[
l^{(k)}(x) = h^{(k)}(x) - h^{(k-1)}(x)
\]

---

### Layer Statistics

#### 1. Intra-layer Variance
- Measures spatial unevenness within a layer

#### 2. Inter-layer Contrast
- Measures difference between consecutive layers

#### 3. Weak Layer Index (WLI)
\[
WLI = \frac{\Delta h}{\sigma_{\text{layer}}}
\]

- High WLI → structurally distinct layers → potential instability

---

## Code Structure

### Core Functions
- `ballistic_deposition(...)`
- `ballistic_deposition_variable_storms(...)`

### Analysis Utilities
- `extract_beta(...)` – growth exponent (KPZ validation)
- `extract_alpha(...)` – roughness exponent
- `compute_layer_statistics(...)` – layer metrics

### Visualization
- Surface growth scaling plots
- Layer-by-layer statistics
- Snowpack stratigraphy heatmaps

---

## Validation

The model is validated against **KPZ theory**:

| Exponent | Theory | Observed |
|----------|------|----------|
| β (growth) | 1/3 | ~0.33 |
| α (roughness) | 1/2 | ~0.5 |

This confirms correct implementation before introducing correlations.

---

## Results

### 1. KPZ Scaling Confirmed
- Surface growth follows expected power laws
- Validates baseline (σ = 0)

### 2. Increased Correlation → More Structure
- Higher σ leads to:
  - Increased intra-layer variance
  - Increased inter-layer contrast
  - More spatially coherent deposition

### 3. Weak Layer Index (WLI)
- High variance obscures clear trends
- Suggests WLI is better as a **relative metric** rather than absolute predictor

### 4. Visual Insights
- Uncorrelated snowfall → irregular, inconsistent layers
- Correlated snowfall → smoother, more uniform stratigraphy

---

## Key Insights

- Spatially correlated storms produce **systematically different snowpack structures**
- Weak layers emerge from **structural mismatch between consecutive deposits**
- Realistic storm distributions (log-normal) significantly improve model realism
- Visual stratigraphy reveals patterns not captured by scalar metrics alone

---

## Limitations

This model simplifies real snowpack physics:
- 1D geometry (no lateral 2D effects)
- No temperature-driven metamorphism
- No wind redistribution
- No compaction or sintering processes

---

## Lessons Learned

### 1. Model Design
Separating validation from feature development is critical for debugging and correctness.

### 2. Computational Constraints
- Naive implementation: **O(L³)** complexity
- Optimization via sparse measurement drastically improved runtime

### 3. Trade-offs
- Larger systems improve accuracy but increase runtime
- Model realism vs computational feasibility is a constant balance

---

## Future Work
- Extend to **2D deposition models**
- Incorporate **temperature and wind effects**
- Use real snowfall data for calibration
- Develop improved weak-layer detection metrics

---

## Installation

```bash
pip install numpy matplotlib scipy
