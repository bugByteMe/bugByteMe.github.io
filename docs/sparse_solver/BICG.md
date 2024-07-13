# BICG

**Initialization**:

- Choose an initial guess $x_0$.
- Compute the initial residual $r_0=b−Ax_0$.
- Set the initial shadow residual $r'_0=r_0$(or another suitable initial residual vector).
- Initialize the search directions $p_0=r_0$and $p'_0=r'_0$.

**Iteration**: For $k = 0, 1, 2, \ldots$ until convergence:

1. **Compute Matrix-Vector Products**:
   - $v = A*p_k$
   - $A^T*p'_k$
2. **Compute the Alpha Coefficient**:
   - $\alpha_k = \frac{r_k^T r_k'}{(A p_k)^T p_k'}$
3. **Update the Solution**:
   - $x_{k+1} = x_k + \alpha_k p_k$
4. **Update the Residuals**:
   - $r_{k+1} = r_k - \alpha_k A p_k$
   - $r_{k+1}' = r_k' - \alpha_k (A^T p_k')$
5. **Check for Convergence** (e.g., if $\|r_{k+1}\|$ is sufficiently small, stop the iteration).
6. **Compute the Beta Coefficient**:
   - $\beta_k = \frac{r_{k+1}^T r_{k+1}'}{r_k^T r_k'}$
7. **Update the Search Directions**:
   - $p_{k+1} = r_{k+1} + \beta_kp_k$
   - $p_{k+1}' = r_{k+1}' + \beta_kp'_k $

**Output**:

- The approximate solution $x_{k+1}$.