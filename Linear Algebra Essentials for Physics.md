# Linear Algebra Essentials for Physics

Notation: matrices uppercase, vectors lowercase. All norms are 2-norm unless noted.

1. **Rank–nullity and the four subspaces**  
    What: $(\dim(\ker A)+\operatorname{rank} A = n)$. Column space = reachable outputs, null space = hidden directions.  
    Example: Fitting $(Ax\approx b)$ for an underdetermined inverse problem (current reconstruction on a grid). Null-space components correspond to non-unique currents that do not change the measured fields.
    

2. **Orthogonal projections**  
    What: Project onto a subspace $(\mathcal S)$ with $(P=P^\top=P^2)$. Gives least-squares solutions.  
    Example: Remove background trend from a spectrum by projecting data onto the orthogonal complement of polynomial baselines.
    

3. **Normal equations and QR for least squares**  
    What: Solve $(\min_x |Ax-b|)$. Avoid $((A^TA)^{-1}A^Tb)$; use $(A=QR)$, then solve $(Rx=Q^Tb)$.  
    Example: Linear fit of energy vs field strength from noisy measurements.
    

4. **SVD and Eckart–Young–Mirsky**  
    What: $(A=U\Sigma V^\top)$. Best rank-$(r)$ approximation $(A_r)$ minimizes Frobenius and operator norm error.  
    Example: Compress a time series of images from a CCD; top singular vectors give dominant spatial modes and temporal coefficients.
    

5. **Spectral theorem (normal matrices)**  
    What: Real symmetric or Hermitian $(A)$ diagonalizes as $(A=Q\Lambda Q^\top)$. Eigenvectors form an orthonormal basis.  
    Example: Normal modes of a coupled mass–spring chain; eigenvalues are squared frequencies.
    

6. **Schur decomposition**  
    What: Any $(A)$ satisfies $(A=QTQ^*)$ with upper triangular $(T)$. Basis of QR eigenvalue algorithms.  
    Example: Stability of a discrete-time update $(x_{k+1}=Ax_k)$: eigenvalues of $(T)$ show growth/decay even if $(A)$ is not normal.
    

7. **Polar decomposition**  
    What: $(A=QH)$, with unitary/orthogonal $(Q)$ and PSD $(H)$. Separates rotation from stretch.  
    Example: Deformation gradient in continuum mechanics: extract pure rotation of a material element from strain.
    

8. **LU / Cholesky / LDLᵀ**  
    What: Direct factorizations for fast solves. SPD matrices use Cholesky $(A=LL^\top)$.  
    Example: Solve the discrete Poisson equation (electrostatics or diffusion) with Cholesky on the sparse SPD matrix.
    

9. **Perron–Frobenius**  
    What: Nonnegative irreducible $(A)$ has a simple largest eigenvalue with positive eigenvectors.  
    Example: Classical 1D Ising transfer matrix; largest eigenvalue sets free energy density and correlation length.
    

10. **Courant–Fischer (min–max)**  
    What: Variational characterization of eigenvalues. $(\lambda_1=\min_{|x|=1} x^\top A x)$ for symmetric $(A)$.  
    Example: Ground-state energy of a tight-binding Hamiltonian via Rayleigh–Ritz with a small trial subspace.
    

11. **Interlacing (Cauchy, Ky Fan)**  
    What: Eigenvalues of principal submatrices interlace those of $(A)$.  
    Example: Changing boundary conditions on a vibrating membrane shifts frequencies in an interlaced way when you restrict the domain.
    

12. **Weyl and Hoffman–Wielandt (perturbation of values)**  
    What: Bounds on shifts of eigen/singular values under perturbations.  
    Example: Small changes in spring constants bound the shift in normal-mode frequencies by the operator norm of the stiffness change.
    

13. **Davis–Kahan (perturbation of subspaces)**  
    What: Angle between invariant subspaces is controlled by perturbation size divided by spectral gap.  
    Example: How much an experimental covariance’s leading principal component deviates from the true one given finite-sample noise.
    

14. **Bauer–Fike (non-normal sensitivity)**  
    What: For diagonalizable $(A=V\Lambda V^{-1})$, eigenvalue shifts are bounded by $(\kappa(V)|E|)$.  
    Example: Amplified eigenvalue drift in a nearly defective amplification matrix in laser cavity modeling.
    

15. **Gershgorin disks**  
    What: Each eigenvalue lies in $(\bigcup_i D(a_{ii}, \sum_{j\ne i}|a_{ij}|))$.  
    Example: Quick stability check that all eigenvalues of a discretized diffusion operator are nonpositive.
    

16. **Condition number $(\kappa)$**  
    What: $(\kappa_2(A)=|A||A^{-1}|)$ measures error amplification.  
    Example: Fitting high-degree polynomials (Vandermonde) to data is ill-conditioned; small noise ruins coefficients.
    

17. **Moore–Penrose pseudoinverse**  
    What: $(A^+)$ yields minimum-norm least-squares and least-norm solutions.  
    Example: Compute coil currents with minimum RMS power that reproduce a target magnetic field on a surface.
    

18. **Sherman–Morrison–Woodbury and determinant lemma**  
    What: Fast updates for $((A+U C V)^{-1})$ and $(\det(A+uv^\top))$.  
    Example: Add a point impurity to a Green’s function $((H-\omega I)^{-1})$ as a rank-$1$ update instead of refactoring the full matrix.
    

19. **Sylvester’s criterion**  
    What: Symmetric $(A)$ is SPD iff all leading principal minors are positive.  
    Example: Verify a Hessian from a continuum energy is positive definite to ensure stable small oscillations.
    

20. **Kronecker products and vec identities**  
    What: $(\mathrm{vec}(AXB)=(B^\top!\otimes!A),\mathrm{vec}(X))$.  
    Example: Build Liouvillians for coupled spins or harmonic oscillators as Kronecker sums to act on vectorized density matrices.
    

21. **Matrix exponential**  
    What: $(e^{At})$ solves $(\dot x=Ax)$. Compute via diagonalization/Schur with scaling–squaring and Padé.  
    Example: Diffusion on a lattice with $(x(t)=e^{Lt}x(0))$ where $(L)$ is the discrete Laplacian.
    

22. **Projection geometry of subspaces**  
    What: Principal angles quantify similarity of subspaces.  
    Example: Compare POD modes from two flow regimes by computing principal angles between their leading subspaces.
    

23. **Jordan form (conceptual use)**  
    What: Non-diagonalizable $(A)$ yields polynomial-in-time transients $(t^k e^{\lambda t})$.  
    Example: Exceptional points in non-Hermitian coupled-mode systems produce non-exponential transient growth predicted by Jordan blocks.