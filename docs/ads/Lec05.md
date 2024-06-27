# Binomial & Fibonacci Heap

#### Idea: Collection of heaps

Use restraint of distinct heap size (binomial heap) or children num (Fibonacci heap) to guarantee the time bound.

#### Binomial Heap

* A binomial heap is a collection of heap-ordered trees with distinct size. (maximum $logN$)

* For each heap-ordered tree $B_k$, the root has $k$ children, from $B_0$ to $B_{k-1}$. 

* $B_k$ is of size $2^k$. Thus each number can be represented by collection of $B_i$.  

* The number of nodes at depth $d$ is $C_k^d$. (Every path from root to depth d is a case of selection. )

* **FindMin**

  FindMin can be done in $O(1)$ by maintaining a min pointer, o/w $O(logN)$

* **Insertion**

  Amortized analysis.

  $\Phi = $ number of heap-ordered trees.

  If insertion takes c steps, then $\Phi \rightarrow \Phi + (2-c)$

  $\hat c_i = c_i + \Delta \Phi = 2 = O(1)$ 

* **Merge**

  Merge is a sequence of insertion. Root list is $O(logN)$

  Thus $O(logN * 1) = O(logN)$

* **Deletion**

  1. Delete the min node.
  2. Build a binomial heap $H'$ with min's children.
  3. Merge $O(logN)$

* **Implementation**

  Implementation is different from schema.

  sub-trees are attached in **descend** order w.r.t their size.

  Ordered-heap trees **still in ascend order.** **Be careful in deletion !!**
  
  ```c
  BinTree CombineTrees( BinTree T1, BinTree T2 )
  {  /* merge equal-sized T1 and T2 */
  	if ( T1->Element > T2->Element )
  		/* attach the larger one to the smaller one */
  		return CombineTrees( T2, T1 );
  	/* insert T2 to the front of the children list of T1 */
  	T2->NextSibling = T1->LeftChild;
  	T1->LeftChild = T2;
  	return T1;
  }
  ```
  
  ```c
  BinQueue  Merge( BinQueue H1, BinQueue H2 )
  {	BinTree T1, T2, Carry = NULL; 	
  	int i, j;
  	if ( H1->CurrentSize + H2-> CurrentSize > Capacity )  ErrorMessage(); /*Current size = total number of nodes*/
  	H1->CurrentSize += H2-> CurrentSize;
  	for ( i=0, j=1; j<= H1->CurrentSize; i++, j*=2 ) {
  	    T1 = H1->TheTrees[i]; T2 = H2->TheTrees[i]; /*current trees */
  	    switch( 4*!!Carry + 2*!!T2 + !!T1 ) {       /*|Carry|T2|T1|*/
  		case 0: /* 000 */
  	 	case 1: /* 001 */  break;	
  		case 2: /* 010 */  H1->TheTrees[i] = T2; H2->TheTrees[i] = NULL; break;
  		case 4: /* 100 */  H1->TheTrees[i] = Carry; Carry = NULL; break;
  		case 3: /* 011 */  Carry = CombineTrees( T1, T2 );
  			            H1->TheTrees[i] = H2->TheTrees[i] = NULL; break;
  		case 5: /* 101 */  Carry = CombineTrees( T1, Carry );
  			            H1->TheTrees[i] = NULL; break;
  		case 6: /* 110 */  Carry = CombineTrees( T2, Carry );
  			            H2->TheTrees[i] = NULL; break;
  		case 7: /* 111 */  H1->TheTrees[i] = Carry; 
  			            Carry = CombineTrees( T1, T2 ); 
  			            H2->TheTrees[i] = NULL; break;
  	    } /* end switch */
  	} /* end for-loop */
  	return H1;
  }
  ```
  
  Delete be aware of size and order!!
  
  ```c
  ElementType  DeleteMin( BinQueue H )
  {	BinQueue DeletedQueue; 
  	Position DeletedTree, OldRoot;
  	ElementType MinItem = Infinity;  /* the minimum item to be returned */	
  	int i, j, MinTree; /* MinTree is the index of the tree with the minimum item */
  
  	if ( IsEmpty( H ) )  {  PrintErrorMessage();  return –Infinity; }
      
      /* Step 1: find the minimum item */
  	for ( i = 0; i < MaxTrees; i++) {  
  	    if( H->TheTrees[i] && H->TheTrees[i]->Element < MinItem ) { 
  		MinItem = H->TheTrees[i]->Element;  MinTree = i;    } /* end if */
  	} /* end for-i-loop */
      
      /* Step 2: remove the MinTree from H => H’ */ 
  	DeletedTree = H->TheTrees[ MinTree ];  
  	H->TheTrees[ MinTree ] = NULL;  
      
      /* Step 3.1: remove the root */ 
  	OldRoot = DeletedTree;   
  	DeletedTree = DeletedTree->LeftChild;   free(OldRoot);
      /* Step 3.2: create H” */ 
  	DeletedQueue = Initialize();   
  	DeletedQueue->CurrentSize = ( 1<<MinTree ) – 1;  /* 2^MinTree – 1 */
  	for ( j = MinTree – 1; j >= 0; j – – ) {  
  	    DeletedQueue->TheTrees[j] = DeletedTree;
  	    DeletedTree = DeletedTree->NextSibling;
  	    DeletedQueue->TheTrees[j]->NextSibling = NULL;
  	} /* end for-j-loop */
   
      /*Update H's Current size*/
  	H->CurrentSize  – = DeletedQueue->CurrentSize + 1;
      /* Step 4: merge H’ and H” */
  	H = Merge( H, DeletedQueue );  
  	return MinItem;
  }
  ```
  
  

#### Fibonacci Heap

* We restrain the number of children of each heap-ordered tree to be distinct. (More relax requirement)

* Lazy maintenance. (Only maintain tree structure in extract min)

  Potential function for Fibonacci heap is $T(H) + 2m(H)$