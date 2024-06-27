# Back Tracking

* **Game Tree**

  Each path from the root to a leaf defines an element of the solution space.

* **Template**

  ```c
  bool Backtracking ( int i )
  {   Found = false;
      if ( i > N )
          return true; /* solved with (x1, …, xN) */
      for ( each xi in Si ) { 
          /* check if satisfies the restriction R */
          OK = Check((x1, …, xi) , R ); /* pruning */
          if ( OK ) {
              Count xi in;
              Found = Backtracking( i+1 );
              if ( !Found )
                  Undo( i ); /* recover to (x1, …, xi-1) */
          }
          if ( Found ) break; 
      }
      return Found;
  }
  ```

* **Efficiency**

  回溯的效率跟S的规模、约束函数的复杂性、满足约束条件的结点数相关。

  约束函数决定了剪枝的效率，但是如果函数本身太复杂也未必合算。

  满足约束条件的结点数最难估计，使得复杂度分析很难完成。

* **$\alpha-\beta$ pruning**

  Limits search to $O(\sqrt N)$ nodes, where $N$ is the total nodes in a game tree.

#### Examples

* **The Turnpike Reconstruction Problem**

  Given $N ( N – 1 ) / 2$ distances.  Reconstruct a point set from the distances.

  **Solution:** Reconstruct from the largest distance. Two options ($X_{l}, X_r$)

  Answers are symetric.

* **Tic-tac-toe**

  $f(P) = W_{Computer} - W_{human}$

  $W$ is the number of potential wins at P.

  
