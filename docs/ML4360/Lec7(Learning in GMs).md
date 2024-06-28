#### Conditional Random Fields(CRF)

![image-20240220165058868](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20240220165058868.png)

#### Parameter Estimation

* $p (\mathcal{Y|X}, \mathbf w)$ denotes the probability of output $\mathcal Y$ with the input $\mathcal X$ and parameter $\mathbf w$

* $\mathcal Y$ can be an arbitrary picture $\in \mathbb Y$

* $\psi$ is the feature function (prior knowledge or learned). e.g. pair-wise feature: $\psi_{i,j}(xi,yi)$

* vector/matrix $\mathbf w$ is of $w_i$ for each feature function $\psi_i$. The inner product is the simplicity form of $\sum w_i\psi_i$

We needs to maximize the probability (distribution) on the given dataset:

<img src="E:\3DV\notes\Lec7(Learning in GMs).assets\image-20240220165438595.png" alt="image-20240220165438595" style="zoom:50%;" />



 #### Deep Structured Models

Feature function $\psi$ can be non-linear with respect to parameter $\mathbf w$, by alter the inner product $<.>$ to $\large{\psi(\mathcal {X,Y}, \mathbf w)}$, which can be learned by deep neural network

* The **RayNet**

**CNN extracts local features, and CRF controls global constrains!**

