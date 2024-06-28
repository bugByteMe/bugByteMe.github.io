#### Shape from shading

#### Gradient Space Representation

A 3-dim free vector represented by gradients.($\frac{\partial z}{\partial z} = 1$ is omitted)

<img src="E:\3DV\notes\Lec8(Shape-from-X).assets\image-20240226095011036.png" alt="image-20240226095011036" style="zoom:33%;" />

It is actually the intersection with plane $z = 1$

<img src="E:\3DV\notes\Lec8(Shape-from-X).assets\image-20240226095130299.png" alt="image-20240226095130299" style="zoom: 33%;" />





#### Photometric stereo

Render equation $I = \rho \ \mathbf s^T \mathbf n$

For different light sources:

<img src="E:\3DV\notes\Lec8(Shape-from-X).assets\image-20240225101347258.png" alt="image-20240225101347258" style="zoom:33%;" />

$\mathbf{\tilde n = (S^T S)^{-1} S^T I}$

#### Shape from X

#### Volumetric Fusion

* 3 Steps:

  Depth-to-SDF Conversion 

  Volumetric Fusion

  MeshExtraction

* Implicit representation (Signed distance field SDF)

  signed distance to the closest surface at each voxel

* Volumetric Fusion

  Weighted average of SDF from different view point

* Mesh extraction

  marching cubes.

  Classify the topology in each cube and do interpolation for the vertex

  <img src="E:\3DV\notes\Lec8(Shape-from-X).assets\image-20240225102008877.png" alt="image-20240225102008877" style="zoom:50%;" />

  