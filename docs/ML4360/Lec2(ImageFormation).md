#### Primitives and Transformations

* inhomogeneous coordinate

  $\tilde x  = (\tilde x, \tilde y, \tilde z, \tilde w)^T$

* homogeneous coordinate

  $$x = (x, y, z)^T$$

* Transformations

![image-20240215182631979](img\image-20240215182631979.png)

* Transformation relation of lines and pixels

  $\tilde{x'} = \tilde H \tilde x$

  $\tilde {l'}\tilde x =  \tilde{l'^T}\tilde H \tilde x = (\tilde {H^T} \tilde l' )^T \tilde x = \tilde l ^T \tilde x = 0$

  $\tilde l' = \tilde{H^{-T}} \tilde l$

* Direct Linear Transform for Homography Estimation

  ```
  u, d, v_trans = np.linalg.svd(A)
  # H is the homography function
  H = v_trans[-1].reshape(3,3)
  ```

  

#### Geometric Image Formation

* Orthographic projection

  ![image-20240215194133790](img\image-20240215182631989.png)

* Perspective projection

  ![image-20240215194228983](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20240215194228983.png)

  

![image-20240215194302476](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20240215194302476.png)

K is the camera intrinsic, intrinsic matrix

s = 0 in practice

* Chaining transformation

  ![image-20240215194438425](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20240215194438425.png)

  ![image-20240215194451856](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20240215194451856.png)

  #### Photometric Image formation

  * Rendering equation
  
    ![image-20240216100502459](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20240216100502459.png)
  
    