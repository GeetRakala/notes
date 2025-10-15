## Purpose and Intuition
Identify the market’s “mood” (**regime**)--crisis, sector-rotational, or calm--by separating **signal** from **noise** in asset return correlations using **Random Matrix Theory (RMT)**. If correlations were purely random, the correlation matrix’s eigenvalues would follow a known distribution. Deviations from that baseline indicate structure and thus a regime.

## Core Concepts
- **Returns time series**: percentage price changes for each asset over time.
- **Correlation matrix** C: pairwise Pearson correlations of standardized returns across N assets.
- **Eigenvalues** $\lambda_i$ and **eigenvectors** $v^{(i)}$ of $C$**:** quantify the strength and composition of common movements. The largest eigenvalue $\lambda_1$ often captures the **market mode**.
- **Random Matrix Theory (RMT)**: studies spectra of random matrices; in finance, it provides a noise benchmark for sample correlation matrices.
- **Marchenko–Pastur (MP) distribution**: the theoretical eigenvalue distribution of a random correlation/covariance matrix; defines a **bulk** $[\lambda_-,\lambda_+]$ where noise eigenvalues should lie.
- **Eigenvalue separation statistics**: metrics that measure how far observed eigenvalues depart from MP noise.

## **End-to-End Workflow**
### **Step 1 -- Data and Rolling Window**
- Arrange returns as $R\in\mathbb{R}^{T\times N}$ (rows: time, columns: assets).
- Use a rolling window of length W (e.g., 252 trading days). Analyze window $[t-W+1,\,t]$, then roll forward by one period and repeat.

### **Step 2 -- Standardize and Correlate**
For each window:
1. Standardize each asset: $\tilde r_{k,i}=(r_{k,i}-\bar r_i)/s_i$ for $k=1\dots W$.
2. Form $C=\frac{1}{W-1}\tilde R^\top\tilde R$ (an $N\times N$ correlation matrix).

### **Step 3 -- Random Benchmark (MP Bounds)**
Let $Q=W/N$ with $Q\ge 1$. For correlation matrices ($\sigma^2=1$):
$\lambda_{\pm}=\left(1\pm\sqrt{\tfrac{1}{Q}}\right)^2$ .
Eigenvalues inside $[\lambda_-,\lambda_+]$ are consistent with noise; those **outside**, especially $\lambda>\lambda_+,$ indicate structure.

### **Step 4 -- Eigendecomposition**
Compute $C=V\Lambda V^\top$. Sort $\lambda_1\ge\lambda_2\ge\cdots\ge\lambda_N$.

### **Step 5 -- Separation Statistics (Features)**
- **Excess market mode**: $E_1=\lambda_1-\lambda_+$.
- **Spectral gap**: $G_{12}=\dfrac{\lambda_1-\lambda_2}{\lambda_2}$.
- **Outlier count**: $M_{\text{out}}=\#\{\lambda_i>\lambda_+\}$.
- **Outlier variance share**: $S_{\text{out}}=\dfrac{\sum_{\lambda_i>\lambda_+}\lambda_i}{\sum_{j=1}^N \lambda_j}$ (for C, $\sum_j\lambda_j=\mathrm{tr}(C)=N)$.

- **Participation ratio of market mode**:

    $$ \mathrm{PR}_1=\frac{1}{\sum{i=1}^N (v^{(1)}_i)^4}$$ High $\mathrm{PR}_1$ ⇒ broad participation.

### **Step 6 -- Regime Labeling**
- **Crisis / high-correlation**: large $E_1$, large $G_{12}$, high $S_{\text{out}}$, high $\mathrm{PR}_1$ ; often $M{\text{out}}=1$.
- **Sector-rotational**: $M_{\text{out}}\ge 2$; multiple structured modes with lower participation per mode.
- **Calm / idiosyncratic**: $M_{\text{out}}=0$ or $E_1\approx 0$; MP bulk fits well.

**Automation**: Feed feature time series into **k-means** or a **Hidden Markov Model (HMM)** to learn regimes and transitions.

## **Marchenko–Pastur Distribution: Signal vs Noise**
- **Null** (pure noise): asset returns are independent with identical variance. The sample correlation eigenvalues lie in $[\lambda_-,\lambda_+]$.
- **Noise**: eigenvalues inside $[\lambda_-,\lambda_+]$.
- **Signal**: eigenvalues outside, especially $\lambda>\lambda_+$, indicating structured correlations.

**What signals represent**
- **Market mode**: $\lambda_1$ well above $\lambda_+$ ⇒ broad co-movement due to macro forces.
- **Sector/industry modes**: additional outliers linked to synchronized sector moves.
- **Other factors**: style effects (e.g., value–growth).

**Practical use**: **Clean** the correlation matrix by keeping signal modes and shrinking the bulk toward MP to avoid fitting noise, improving portfolio optimization and risk management.

## **Worked Example

**Setup**: $N=100$ assets, $T=W=500$ days ⇒ $Q=T/N=500/100=5$.
$\sqrt{1/Q}=\sqrt{0.2}\approx 0.447$.

**MP bounds**
$\lambda_+=(1+0.447)^2\approx 2.094,\quad \lambda_-=(1-0.447)^2\approx 0.306$.

**Observed eigenvalues**
- $\lambda_1=25.7$ ⇒ very strong market mode (signal).
- $\lambda_2=3.1$ ⇒ second structured mode (e.g., sector).
- $\lambda_3=1.8$ ⇒ inside MP bulk (noise).
    Most remaining 97 eigenvalues lie in $[0.306,2.094]$.

**Implications**
- $M_{\text{out}}=2$.
- $E_1\approx 23.6$; large $G_{12}$.
- $S_{\text{out}}\approx (25.7+3.1)/100\approx 0.288$ of total variance from signal modes.

## **Rolling Window**

**Correct procedure**:
- Do **not** average returns.
- Do **not** compute eigenvalues for a single day.
- For each window (e.g., $500\times 100$ block), compute **one** $100\times100$ correlation matrix and **one** set of 100 eigenvalues. This is a **snapshot**.

**Evolution**:
- **Window 1**: days 1–500 → features at day 500.
- **Window 2**: days 2–501 → new features at day 501.
- Repeat to form time series of $E_1, G_{12}, M_{\text{out}}, S_{\text{out}}, \mathrm{PR}_1$.
    Analyzing these series reveals regime shifts.

## **Correlation Matrix C**

For assets A,B over n days with returns $\{R_{A,i}\},\{R_{B,i}\}$ and means $\mu_A,\mu_B$:

$$\rho(A,B)= \frac{\sum_{i=1}^{n}(R_{A,i}-\mu_A)(R_{B,i}-\mu_B)} {\sqrt{\sum_{i=1}^{n}(R_{A,i}-\mu_A)^2}\;\sqrt{\sum_{i=1}^{n}(R_{B,i}-\mu_B)^2}}$$

**Steps**
1. Compute $\mu_A,\mu_B$.
2. Numerator: sum of cross-deviations.
3. Denominator: product of square roots of summed squared deviations.
4. Divide to get $\rho\in[-1,1]$.
    Repeat for all unique pairs to fill C.

## **Summary**
- MP provides noise bounds for eigenvalues of sample correlation matrices.
- Outlier eigenvalues encode market, sector, and factor structure.
- Rolling-window features quantify regime shifts.
- Use simple rules or ML to classify regimes and track transitions.
