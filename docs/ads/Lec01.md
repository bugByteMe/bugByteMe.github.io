# AVL/Splay Trees and Amortized analysis

### BST

The height of BST can be as bad as $O(N)$

#### AVL Trees

* Balance factor $h_L - h_R \in \{-1, 0, 1\}$
* Trouble finder & Trouble maker (important)
* Single rotation (LL/RR rotation)
* Double rotation (LR/RL rotation) (**Parent first & Grandparent next !!!**)
* Height (BF) of the trouble finder **doesn't change** after insert+rotation
* Maximum height of trees? The minimum nodes of height k is $n_k$, then we have $\mathbf{n_k = n_{k-1} + n_{k-2} + 1} = F_{k+2} - 1$. The nodes is exponential to the height.
* Height of empty tree = $-1$

#### Splay Trees

* Zig-Zag (Double Rotation)
* Zig-Zig (Single Rotation on **GrandParent & Single Rotation on Parent**)

| Tree       | LL/RR                             | LR/RL                             |
| ---------- | --------------------------------- | --------------------------------- |
| AVL Tree   | Single Rotation                   | Rotate Parent, Rotate Grandparent |
| Splay Tree | Rotate Grandparent, Rotate parent | Rotate Parent, Rotate Grandparent |

#### Amortized Analysis 

> Average-case bound $\leq$ Amortized bound $\leq$ Worst case bound

* Aggregate analysis

* Accounting method (different operation can have different amortized cost)

* Potential method

  Potential function $\Phi(D)$

  $\Phi_0(D)$ must be the minimum

  $\tilde c_i = c_i + \Phi(D_i) - \Phi(D_{i-1})$

  $\tilde c$ is the amortized cost and $c$ is the real cost of an operation.
  
  If $c_i$ is large, it means we pay in advance in this operation, so the potential should decrease to balance the amortized cost ($\Phi(D_i) \le \Phi(D_{i-1})$). (Potential function design principal)
  
  If $c_i$ is large, we needs to borrow money from $\Phi$, which will be paid later by the low-cost $c_i$
  
* For Splay Trees, $\Phi = \sum_{i \in T} log(S(i))$, where $S(i)$ is the rank of node i. (Size of the subtree $i$)

| Data Structure            | Potential function                                           | Time complexity                      |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------ |
| Splay Trees               | $\Phi = \sum_{i \in T} log(S(i))$, where $S(i)$ is the rank of node i. (Size of the subtree $i$) | $O(logn)$                            |
| Skew Heaps                | $\Phi = $ number of heavy nodes. Heavy: size of $t_r \geq \frac{1}{2} t_{root}$ | $O(logn)$                            |
| Binomial Heap (Insertion) | $\Phi = $ number of heap ordered trees                       | $O(1)$                               |
| Fibonacci Heap            | $\Phi =T(H) + 2m(H)$                                         | $O(1)$ insertion, $O(logn)$ deletion |
