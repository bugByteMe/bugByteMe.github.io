# Self-Supervised Learning

#### Task Specific Models

![image-20240229093558453](E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240229093558453.png)

<img src="E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240229093616918.png" alt="image-20240229093616918" style="zoom:33%;" />

<img src="E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240229093642113.png" alt="image-20240229093642113" style="zoom:33%;" />

#### Pretext Task

**Goal: **Define an auxiliary task (e.g., rotation) for pre-training with lots of unlabeled data.

Then, remove last layers, train smaller network on fewer data of target task.

Pre-train feature function $f_{\theta}$, and fine tune the readout layer. (e.g. MLP, classifiers)

e.g.:

<img src="E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240229093903788.png" alt="image-20240229093903788" style="zoom: 33%;" />

* Short cuts

  Useful for solving pre-text but not target task.

  The Neural Network often finds the most simple way to complete a task.(edge continuity...)

#### Contrastive Learning

**Goal: **the pre-training task and the transfer task are “aligned”

<img src="E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240229094157492.png" alt="image-20240229094157492" style="zoom:33%;" />



* Score functions are often chosen as cosine similarity: $s(\mathbf{f_1, f_2}) = \mathbf{\frac{f_1^T f_2}{||f_1||\ ||f_2||}}$, similar to the siame network!

* Loss function: multi-class cross entropy loss

  <img src="E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240301090442102.png" alt="image-20240301090442102" style="zoom:33%;" />

  This is also known as the InfoNCE loss. For mutual information $MI$, $MI[f(x), f(x^+)] \geq log(N) - \mathcal L$

  More sample $N$, tighter bound, larger mutual information.

#### Momentum Contrast

<img src="E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240301090748264.png" alt="image-20240301090748264" style="zoom:33%;" />

#### Barlow Twins

<img src="E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240301090824846.png" alt="image-20240301090824846" style="zoom: 50%;" />

**Goal: ** reduce redundancy between neurons, as well as increase similarity between $Z^A, Z^B$.

In other words, Neurons should be invariant to data augmentations but independent of others.



<img src="E:\3DV\notes\Lec11(Self-Supervised Learning).assets\image-20240301091001749.png" alt="image-20240301091001749" style="zoom:33%;" />

$\mathcal C$ is just $Cov(Z^A, Z^B)$ ! $\mathcal C_{i,j}$ = 0, means no corrolation.

**No negative samples needed** any more!
