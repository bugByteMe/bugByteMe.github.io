# Wrap Up

#### Geometric Image Formation

* Camera model, Projection model
* Calibration matrix (From **real world position** to **image position**)

#### Photometric Image Formation

* Rendering equation

#### Structure From Motion

* Recover 3D structure of an object (3D position, in **Sparse Feature Points**), 2 frame

  Depth: through triangulization.

* Estimate Camera pose
* Bundle Adjustment, **multiple views**

#### Stereo reconstruction

* Recover 3D structure of an object (3D position, in **Dense**)

  Rectification, Block matching along the epipolar line

  Depth: from disparity

* Siamese Network

  Learn similarity score between blocks, used for **block matching**