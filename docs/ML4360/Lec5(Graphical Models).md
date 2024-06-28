#### Markov Random Field(MRF)

![image-20240219113033071](E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113033071.png)

![image-20240219113115238](E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113115238.png)

Where $\mathbf{ a,b,c}$ are random variables.

#### Factor Graphs

![image-20240219113227744](E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113227744.png)

![image-20240219113238601](E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113238601.png)

#### Belief Propagation

Inference in factor graphs

<img src="E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113351784.png" alt="image-20240219113351784" style="zoom:50%;" />

* Factor to variable message

  ![image-20240219113431083](E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113431083.png)

* Variable to factor message

  ![image-20240219113458374](E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113458374.png)

#### Sum-Product Algorithm

![image-20240219113544890](E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113544890.png)

With initialization:

<img src="E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113601311.png" alt="image-20240219113601311" style="zoom:33%;" />

Calculate marginals:

$p(\mathbf x) \propto \prod_{f \in ne(x)} \mu_{f\rightarrow x}(x)$

#### Max product Algorithm

Convert $\sum to \ max()$

![image-20240219113952819](E:\3DV\notes\Lec5(Graphical Models).assets\image-20240219113952819.png)

