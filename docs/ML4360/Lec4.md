#### Two-View Stereo Matching

* Task: Construct a dense 3D model from 2 images of a static scene 

Pipeline: 

1. Calibrate cameras intrinsically and extrinsically (Lecture 3.1)
2. Rectify images given the calibration
3. Compute disparity map for reference image
4. Remove outliers using consistency/occlusion test
5. Obtain depth from disparity using camera calibration
6. Construct 3D model, e.g., via volumetric fusing and meshing (Lecture 8.4)

* Image rectification

  **Goal:**Correspondences search along horizontal scanlines (simplifies implementation)

  ![image-20240218144003587](Lec4.assets\image-20240218144003587.png)

  ![image-20240218144013565](Lec4.assets\image-20240218144013565.png)

  #### Block Matching

  #### Siamese Networks

  ![image-20240218144142889](Lec4.assets\image-20240218144142889.png)

  Loss function:

  $L = max(0, m + s_- - s_+)$

  #### Spatial regularization

  more smooth between pixels

  ![image-20240218144357978](Lec4.assets\image-20240218144357978.png)

  

  