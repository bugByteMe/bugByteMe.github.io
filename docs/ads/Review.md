注意名词解释和算法细节（full binary tree, complete binary tree？B+树）

考前训练

PPT代码熟悉（也可以背下来）



PPT，历年卷，平时作业

AVL可能考代码，dp或贪心



时间分配（提前分配，做历年卷的时候可以试试）

做题策略 重量级题目可以先跳过



| **Problem**              | **Method**                                                   | **Approximation Ratio**                                      | **Time Complexity**                          |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | :------------------------------------------- |
| Bin Packing              | Next Fit                                                     | 2-approximation                                              | Polynomial                                   |
|                          | First Fit                                                    | 1.7-approximation                                            | Polynomial                                   |
|                          | First Fit decreasing                                         | 11/9-approximation                                           | Polynomial                                   |
| Knapsack Problem         | Greedy (max profit or density)                               | 2-approximation                                              | Polynomial                                   |
| K-center                 | Binary search for r, and draw cycle with 2r                  | 2-approximation No 1+$\epsilon$ unless P=NP                  |                                              |
| Hopfield Neural Networks | Flipping until all nodes are stable                          |                                                              | May NOT Polynomial $\sum w_i$ iterations     |
| Maximum Cut              | Flipping                                                     | 2-approximation                                              | May NOT Polynomial                           |
|                          | Flipping only when big improvement                           | (2+$\epsilon$)-approximation                                 | Polynomial O($\frac{n}{\epsilon}$logW) flips |
|                          | Flipping only when big improvement (start with max $\{v, V-v\}$ configuration) | (2+$\epsilon$)-approximation                                 | Polynomial O($\frac{n}{\epsilon}$logn) flips |
|                          |                                                              | $17/16$ approximation, best unless P=NP                      |                                              |
| Job Scheduling           | Greedy Best Fit, Random order                                | 2-approximation (more precise $2(1-\frac{1}{m})$) (associate OPT and greedy answer with last job) |                                              |
|                          | Greedy Best Fit, Length desc order                           | 1.5-approximation (4/3 is tight)                             |                                              |
| TSP                      | MST with preorder                                            | 2-approximation                                              |                                              |
