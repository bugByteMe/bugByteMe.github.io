# Dynamic Programming

#### Examples

* **Optimal Binary Search Tree (Different from Huffman encoding!!!)**

  We need to maintain the BST property as well as optimal the search cost.

  $c_{i,j} = min_{l\in(i,j]}\{w_{i,j} + c_{i,l-1} + c_{i,l+1}\}$, $w_{i,j}$ is the total weight from i to j, (cause the height of tree increases)

* **All Pairs Shortest Paths**

  $D^k [ i ] [ j ] = min\{D^{k-1}[ i ] [ k ] + D^{k-1}[ k ] [ j ]\}$

* **Product Assembly**

  

  