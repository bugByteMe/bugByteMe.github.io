# Divide and Conquer

## Simple Example

#### Nearest point pair

Divide and conquer.

Let $\delta = $ $min\{d_l, d_r\}$

For a point lies in the strip, we only search for the near by $\delta * \delta$ square.

At the beginning, the points are preprocessed to sorted by x distance.

During conquer, we merge them with sorted y distance. (Like merge sort).

Each point at most inspect 8 neighbors.

In the conquer part, we only scan the point list linearly. $O(NlogN)$

<img src="E:\24SprgSmmr\ADS\note-ads\Lec07.assets\image-20240408183825460.png" alt="image-20240408183825460" style="zoom: 33%;" />

## Substitution Method

Guess first, proof by induction

#### Pit-fall

<img src="E:\24SprgSmmr\ADS\note-ads\Lec07.assets\image-20240408131001239.png" alt="image-20240408131001239" style="zoom: 33%;" />

## Recursion Tree



## Master Method

$\Large{T(N) = \sum_{j=0}^{log_b(N-1)} a^j \mathbf{f(N/b^j)} + \Theta(N^{log_b a})}$

The last layer has $N^{log_b a}$ nodes

#### Type 1

compare $f(N)$ with $N^{log_b a}$ in $\textbf{Polynomial}$ way.

$f(N) = NlogN > N^{log_b a + \epsilon}$ $\textbf{WRONG}$

* Why comparing with $N^{log_b a}$ ? 

  This determines the result of $(\frac{N}{b^j})^{log_b a + ?} = 1 $ or $b^{\epsilon j}$ or ...

<img src="E:\24SprgSmmr\ADS\note-ads\Lec07.assets\image-20240408125448398.png" alt="image-20240408125448398" style="zoom:33%;" />

* For case 1:

  <img src="E:\24SprgSmmr\ADS\note-ads\Lec07.assets\image-20240408130242407.png" alt="image-20240408130242407" style="zoom: 25%;" />

* For case 2:

if $f(N) = \Theta(N^{log_ba})$

then we have

$\sum_{j=0}^{log_b(N-1)} a^j \mathbf{f(N/b^j)}= \Theta(\sum_{j=0}^{log_b(N-1)} a^j \mathbf{(N/b^j)^{log_b a}}) = $

$\Theta(N^{log_ba}\sum_{j=0}^{log_bN-1} (\frac{a}{b^{log_ba}})^j) = $$\Theta(N^{log_ba} * \sum_{j=0}^{log_bN-1} 1) = \Theta(N^{log_ba}log_bN)$

$\Theta(N^{log_ba}log_bN) + \Theta(N^{log_b a}) = \Theta(N^{log_ba}log_bN))$

#### Type 2 (Feeble)

Compare $af(N/b)$ with $f(N)$

<img src="E:\24SprgSmmr\ADS\note-ads\Lec07.assets\image-20240408130541250.png" alt="image-20240408130541250" style="zoom:25%;" />

#### Type 3 (Robust)

<img src="E:\24SprgSmmr\ADS\note-ads\Lec07.assets\image-20240408130706710.png" alt="image-20240408130706710" style="zoom: 33%;" />





