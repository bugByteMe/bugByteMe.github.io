# :ghost:DeVRF: Fast Deformable Voxel Radiance Fields for Dynamic Scenes

#### Abstract

* **Task**

  Free-viewpoint photorealistic view synthesis

* **Technical Challenges For Previous Methods**

  Slow convergence (NeRF)

* **Key Insight / Motivation**

  static → dynamic learning paradigm

   Efficient learning of deformable radiance fields

* **Technical Contributions**

  Model both the 3D canonical space and 4D-deformation field of a dynamic, non-rigid scene with explicit and discrete voxel based representations.

  static → dynamic learning paradigm

* **Experiment**

#### Introduction

* **Task and Application**

  Free-viewpoint photorealistic view synthesis techniques from a set of captured images unleash new opportunities for immersive applications such as virtual reality, telepresence, and 3D animation production.

* **Technical Challenges For Previous Methods**

  Multi-plane images & NeRF:

  ​	mainly focus on static scenes

  Volume-Deform (unified volumetric representation to encode both the scene’s geometry and its motion), 

  Neural Volumes (represents dynamic objects with a 3D voxel grid plus an implicit warp field), 

  D-NeRF (learns a deformation field that maps coordinates in a dynamic field to a NeRF-based canonical space), 

  HyperNeRF ( model the motion in a higher dimension space, representing the time-dependent radiance field by slicing through the hyperspace):

  ​	 Require days of GPU training time

  DVGO (explicit and discretized volume representations),

  Plenoxels (employs sparse voxel grids as the scene representation and uses spherical harmonics to model view-dependent appearance),

  Instant-ngp (multiresolution hash encoding):

  ​	Mainly focus on static scenes

* **Our Pipeline**

  In the first stage, DeVRF learns a 3D volumetric canonical prior (b) from multi-view static images.

  In the second stage, a 4D deformation field (d) is jointly optimized from taking few-view dynamic sequences (c) and the 3D canonical prior

* **Demos & Application**

#### Method

* **Overview**

  Specific task. Input, output. First stage, second stage

* **3D Volumetric Canonical Space.**

  **Motivation**

  We take inspiration from the volumetric representation of DVGO.

  **Method**

   $Tri-Interp([x,y,z], Vp) : \R^3, \R^{C× N_x× N_y× N_z} → \R^C ,∀p ∈ {density, color}$

  where C is the dimension of scene property. Property learned are density and color. 

  We employ softplus and post-activation in $V_{density}$

  We also apply a shallow MLP in $V_{color}$ to enable view-dependent color effects

  **Advantage**

  Efficiently query the scene property of any 3D point

* **4D Voxel Deformation Field**

  **Motivation**

  **Method**

  The 3D motion $∆X_{t→0} = {∆X^{t→0}_i}$  (i means neighbours)can be efficiently queried through quadruple interpolation of their neighboring voxels at neighboring time steps in the 4D **backward** deformation field. 

  $Quad-Interp([x,y,z,t], V_{motion}) : \R^4, \R^{N_t× C× N_x× N_y× N_z} → \R^C$

  $C$ is the degrees of freedom (DoFs)

  $N_t$ is the number of key time steps

  Backward here because we needs to find the original position in the static scene.

  **Advantage**

  Scene properties of $X_t$ can then be obtained by querying the scene properties of their corresponding canonical points $X_0$ through trilinear interpolation.

  Efficient.

* **Coarse-to-Fine Optimization**

* **4D Deformation Cycle Consistency**

* **Optical Flow Supervision**

  we first compute the corresponding 3D points of $X_0$ at $t−1$ time step via forward motion got $\tilde X_{t−1}$. After that, we project $\tilde X_{t−1}$onto the reference camera and get  their pixel locations $\tilde{P_{t−1}}$, and compute the induced optical flow with respect to the pixel location $P_t$ from which the rays of $X_t$ are cast. 

  Then we compare induced optical flow with gt.

#### Experiments

* **Comparison Experiments**
* **Ablation Study**

#### Limitations

 	The model size is large due to its large number of parameters.

​	DeVRF currently does not synchronously optimize the 3D canonical space prior during the second stage, and thus may not be able to model drastic deformations.