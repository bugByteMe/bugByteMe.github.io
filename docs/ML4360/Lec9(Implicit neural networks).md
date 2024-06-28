#### Occupancy network

<img src="E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225102149808.png" alt="image-20240225102149808" style="zoom:33%;" />

In essence a classifier!

#### Representing Materials and Lighting

$\Large{\mathbf L_{cSLF}(\mathbf{p,v,l}) : \mathbb R^3 \times \mathbb R^3 \times \mathbb R^M \rightarrow \mathbb R^3}$

#### Representing Motion

#### Representing Scenes

Needs to exploit local features with CNN

![image-20240225102729114](E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225102729114.png)

#### Differentiable Volumetric Rendering

Learning from Images

**No 3D supervisions**

![image-20240225102834653](E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225102834653.png)

* Forward

  ![image-20240225102918368](E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225102918368.png)

* Backward

  ![image-20240225102943720](E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225102943720.png)

#### Neural Radiance Fields

Render image from novel viewpoints, instead of fine 3D reconstruction

<img src="E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225103126139.png" alt="image-20240225103126139" style="zoom: 50%;" />

* Fourier Features

  Learn high dimensional function in low dimensional space

  Avoid over-smoothing, more sharp details

#### Generative Radiance Fields

Can train from unstructured and unposed image collections. (With no camera pose)

![image-20240225103459433](E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225103459433.png)

![image-20240225103516244](E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225103516244.png)

View dependent appearance and view independent density! Disentangle appearance and occupancy.

*  GIRAFFE 

  ![image-20240225103631222](E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225103631222.png)

  Different parts of an image

#### Wrap up

![image-20240225103657686](E:\3DV\notes\Lec9(Implicit neural networks).assets\image-20240225103657686.png)

