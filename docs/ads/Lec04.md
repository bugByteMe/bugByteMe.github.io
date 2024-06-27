# Leftist Heap & Skew Heap

#### Idea: faster merge

### Leftist Heap

$O(logN)$

* **Keep the length right path low.(logN)**

  Balanced tree can do this, but too more constraint.

  A more relaxed constraint: $NPL$ (NULL path length)

  Shortest path to a node without 2 child.

  $NPL(single node) = 0$

  $NPL(NULL) = -1$

  $NPL(x) = min_{c \in child(x)}\{NPL(c) + 1\}$

* **Maintain NPL**

  Swap childs.

* **Merge Operation**

  Heap A, B

  1. Sort right path of A B, let it to be the new right path.

     <img src="E:\24SprgSmmr\ADS\note-ads\Lec04.assets\image-20240318220214707.png" alt="image-20240318220214707" style="zoom: 50%;" />

  2. Attach remained left sub tree.
  3. Maintain NPL.

* **Time complexity**

  $O(logN)$
  
* **Codes**

  Don't forget to maintain NPL.

  ```c
  PriorityQueue  Merge ( PriorityQueue H1, PriorityQueue H2 )
  { 
  	if ( H1 == NULL )   return H2;	
  	if ( H2 == NULL )   return H1;	
  	if ( H1->Element < H2->Element )  return Merge1( H1, H2 );
  	else return Merge1( H2, H1 );
  }
  static PriorityQueue
  Merge1( PriorityQueue H1, PriorityQueue H2 )
  { 
  	if ( H1->Left == NULL ) 	/* single node */
  		H1->Left = H2;	        /* H1->Right is already NULL and H1->Npl is already 0 */
  	else {
  		H1->Right = Merge( H1->Right, H2 );     /* Recursive Merge */
  		if ( H1->Left->Npl < H1->Right->Npl )   /* Maintain subtree */
  			SwapChildren( H1 );	
  		H1->Npl = H1->Right->Npl + 1;           /* Update NPL value*/
  	} /* end else */
  	return H1;
  }
  ```

### Skew Heap

* **No maintaining of NPL anymore.**

  Length of right path is **ONLY** guaranteed under **amortized analysis**.

* **Merge Operation**

  1. Sort right path of A B, let it to be the new **left path**.
  2. Attach left-sub tree to the corresponding right child. (except the largest on new left path)

* **Amortized analysis**

  $\Phi = $ number of heavy nodes.

  After swap, heavy nodes **MUST** becomes light nodes.

  Light nodes **MAY** become heavy nodes.

  New heavy nodes $\leq $ Old light nodes.

  <img src="E:\24SprgSmmr\ADS\note-ads\Lec04.assets\image-20240318220754013.png" alt="image-20240318220754013" style="zoom:67%;" />