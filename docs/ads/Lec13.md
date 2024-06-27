# Randomized Algorithms

#### The Hiring Problem

Hiring cost: $C_h$

* **Permute candidates**

  $E[X] = E[\sum X_i] = \sum E[X_i] = \sum 1/i = ln(N)$

  $O(ln(N)C_h)$

  * **How to permute?**

    Assign each element a random priority and sort.

* **Hire Only Once**

  For first k candidates, we determine the threshold, no hire.

  For remaining candidates, we hire the first that is above the threshold.

  $P($no one at k+1 - i-1 are hired$) = k/(i-1)$ (The maximum of first i is at first j)
  
  Best $k = N/e$

#### Quick Sort

The pivot always divides the set so that each side is at least $n/4$.

If the pivot doesn't satisfies, we choose again.

The expected number of choose is $E = 1/2*1 + 1/2*(E+1)$ ,$E = 2$

* Type j problem

  The problem divided by $3/4$ for $j$ times.

  $N(3/4)^{j+1} \leq |S| \leq N(3/4)^j$. $|S|$ is the problem size.

  There are at most $(4/3)^{j+1}$ problems at type $j$.

  Number of different types: $log_{4/3}N = O(logN)$

**Some proofs:**

* $\int_k^N 1/x dx \leq \sum_{i=k}^{N-1} 1/i \leq \int_{k-1}^{N-1} 1/x dx$.

  It is obvious that : $\int_k^{k+1} 1/x dx \leq \int _k^{k+1} 1/k dx = 1/k$.

  So we have: $\int_k^N 1/x dx = \int_k^{k+1} 1/x dx + \int_{k+1}^{k+2} 1/x dx +\dots + \int_{N-1}^{N} 1/x dx\leq \sum_{i=k}^{N-1} 1/i $.

  We also have: $\int_k^{k+1} 1/x dx \geq \int _k^{k+1} 1/(k+1) dx = 1/(k+1)$.

  With similar strategy, we can get: $\sum_{i=k}^{N-1} 1/i \leq \int_{k-1}^{N-1} 1/x dx$.

  Warp it up, we have $\int_k^N 1/x dx \leq \sum_{i=k}^{N-1} 1/i \leq \int_{k-1}^{N-1} 1/x dx$.

* Optimum k?

  $f'(k) = 1/Nln(N/k) + -1/N = 1/N*(ln(N/k) - 1)$

  $N/k = e$

  $k = N/e$ 