# RB-Tree and B+ Tree

#### Ex.

Suppose: $bh(Tree) < h(Tree) / 2$. 

We choose the longest simple path down to leaf. Then the length of the path is $h(Tree)$.

The number of red nodes is $n(red) = h(Tree) - bh(Tree) \geq bh(Tree) + 1$, which is greater than the number of black nodes.

Plus the root is black, there are only $bh(Tree)$ number of spaces for red nodes. There must be two continuous red nodes. This **violates** the rule (No continuous red nodes along a path).

Therefore, $bh(Tree) \geq h(Tree) / 2$



$bh(Tree) = bh(child) + 1 \ge h(child)/2 + 1 = (h(Tree) - 1)/2 + 1 = h(Tree)/2  + 1/2$

#### Some Corollaries

* In every simple path, red nodes $\leq$ black nodes.

* Minimum RB tree: All blacks

* Maximum RB tree:  Red nodes between all black nodes

* $H \leq 2*bh$ 

* A RB tree with $N$ internal nodes has $H \leq 2ln(N+1)$

  $2N \geq 2^{bh} - 1, H/2 \leq bh \leq log(2N+1), $

#### RB-Tree Insertion

Always **insert a red**

Check **uncle**.

Same color: push up

Else: change(P, GP) **Rotate**

#### RB-Tree Deletion

If we delete a red node, everything is fine. 

If we delete a black node, we temporarily add this black property to node $x$ (which occupies the deleted node's position, if null, we can preserve the deleted node of convenience, and delete it after maintain the r-b property), Then node $x$ is double-black.

There are two ways to eliminate double-black:

* Change the color of a black node to red, on the branch not containing $x$, So $bh( Path(\not x) ) = bh(Path(x)) - 1$. Double black is eliminated. (Case 2)
* Change the color of a red node to black, on the branch not containing $x$, and rotate it the branch containing $x$. So $bh( Path(x) ) = bh(Path(\not x)) + 1$ Double black is eliminated. (Case 4)
* Case 3 -> Case 4, Case1 -> Case 2/3/4

Cheat sheet:

**Make sibling black**. (Case 1) F&S ex color, rotate     (Why ex? Make new root black. In case new root causes new error)

**Nephew** all black. (Case 2) Make sibling red. Check parent. 

Near Nephew red. (Case 3) N&S ex color. rotate

 Far Nephew red. (Case 4) Far nephew black. F&S ex color. rotate

|           | AVL       | RB tree |
| --------- | --------- | ------- |
| Insertion | $\leq2$   | $\leq2$ |
| Deletion  | $O(logn)$ | $\leq3$ |

#### B+ Tree

* The root is **either a leaf** or has between 2 and M children
* All nonleaf nodes **(except the root)** have between $\lceil M/2 \rceil$ and $M$ children.
* Leafs has between $\lceil M/2 \rceil$ and $M$ data!
* <img src="E:\24SprgSmmr\ADS\note-ads\Lec02.assets\image-20240305112407332.png" alt="image-20240305112407332" style="zoom: 50%;" />
* Depth = $log_{\lceil M/2\rceil}(N)$
* Find = $log_{\lceil M/2\rceil}(N) * log_2M = logN$
* Insert = $log_{\lceil M/2\rceil}(N) * M = (M/logM)*logN$

