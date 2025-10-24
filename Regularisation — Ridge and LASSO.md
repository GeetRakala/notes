## What is regression?

- Predict a numeric target y from features $x_1,\dots,x_p$.
- Linear model: $\hat y=\beta_0+\sum_{j=1}^p \beta_j x_j$.
- Fit by minimizing mean squared error (MSE): $\frac{1}{n}\sum_i (y_i-\hat y_i)^2$.
- Goal: low error on new data and a readable link between x and y.

## Why regularize?

- Plain least squares overfits with many or correlated features.
- Add a penalty to discourage complex models:
$$\text{Loss}=\text{MSE}+\lambda\cdot\text{Penalty}(\beta)$$
- $\lambda$ controls the strength. Tune it with cross-validation.

## Ridge vs LASSO

- **Ridge (L2):** penalty $\sum_j \beta_j^2$. Shrinks coefficients toward 0, rarely to exactly 0. Good with many small effects and collinearity.
- **LASSO (L1):** penalty $\sum_j |\beta_j|$. Forces many coefficients to **exactly 0** (sparse model). Good when only a subset matters or you want a short list.

## Why L2 ≈ “near zero” but L1 = “exact zero”

- 1D solutions: Let $z$ be the unregularised estimate. 
  - Ridge (L2):
    - Solve : $- \min_\beta \tfrac12(\beta-z)^2+\tfrac{\lambda}{2}\beta^2$ (take first order derivative)
    - $\beta=\frac{z}{1+\lambda}$ (continuous shrink; zero only if z=0).
  - LASSO (L1):
    - Solve : $- \min_\beta \tfrac12(\beta-z)^2+\lambda|\beta|$ (take first order derivative)
    - $\beta=\operatorname{sign}(z)\,(|z|-\lambda)_+$ (soft-thresholding; if $|z|\le\lambda$ then $\beta=0$).
- Geometry:
  - L2 constraint is smooth (circle); optimum rarely lands on axes.
  - L1 constraint has sharp corners on axes; optimum often hits a corner → zeros.

  
## When to use what

- **Use Ridge:** many weak signals, strong collinearity, you care about stable predictions more than feature selection.
- **Use LASSO:** you want automatic variable selection, simpler models, lower operational cost.
- **Use Elastic Net:** mix of L1 and L2 for correlated features when you still want sparsity.


## Workflow

1. Standardise features. Keep a proper test set; for time series, use walk-forward splits.
2. Choose Ridge, LASSO, or Elastic Net.
3. Tune \lambda (and \alpha for Elastic Net) with cross-validation.
4. Refit on training data with chosen hyperparameters.
5. Validate on held-out data; check error, sparsity, stability, and turnover if you rebalance over time.

  
## Pitfalls

- No standardisation → uneven penalties.
- LASSO over-shrinks large true effects; consider **relaxed LASSO** or refit OLS on selected features.
- Highly correlated predictors → prefer **Elastic Net** or **Group LASSO** for stable selection.
- Avoid data leakage, especially in temporal data.

## Names

- **LASSO:** “Least Absolute Shrinkage and Selection Operator.”
- **Ridge:** from “ridge analysis” and the “ridge trace” diagnostic in earlier response-surface work.
- **L1/L2:** refer to the $\ell_1$ (sum of absolutes) and $\ell_2$ (sum of squares) norms used in the penalties.


## Other named penalties
- **Elastic Net**: $\lambda(\alpha\|\beta\|_1+(1-\alpha)\|\beta\|_2^2)$. Sparse and stable for correlated features.
- **Group LASSO**: sum of group norms. Drops or keeps whole groups.
- **Sparse Group LASSO**: group selection plus within-group sparsity.
- **Fused LASSO** and trend filtering: add $\sum |\beta_{j}-\beta_{j-1}|$ to enforce smoothness in ordered coefficients.
- **SCAD** and **MCP**: nonconvex. Sparse with less bias on large coefficients.
- $L_\infty$: often used as a constraint to cap coefficients. No sparsity.

## Other regression types (brief map)

- **Generalized linear models (GLMs):**
  - Logistic (binary outcomes), Poisson (counts), Gamma (positives), etc.
- **Quantile regression:** models conditional quantiles; robust to outliers.
- **Robust regression:** Huber, Tukey, LAD (\ell_1 loss) to blunt outliers.
- **Subset/stepwise selection:** explicit feature picking (\ell_0-style); unstable and computationally heavy.
- **Group/structured penalties:** Group LASSO, Sparse Group, Fused LASSO, Trend filtering.
- **Nonlinear and flexible:** Polynomial features, splines, **GAMs**.
- **Kernel methods:** Kernel ridge, Support Vector Regression.
- **Bayesian:** Gaussian priors (≈ Ridge), Laplace (≈ LASSO), Horseshoe, Spike-and-Slab.
- **Tree/ensemble regressors:** Random Forests, Gradient Boosting, XGBoost, though not “linear” regression.

**Summary:** Regularization trades a little bias for big variance reduction. Ridge shrinks many, keeps all. LASSO shrinks and zeroes many. Elastic Net bridges the two, especially with correlated predictors.


