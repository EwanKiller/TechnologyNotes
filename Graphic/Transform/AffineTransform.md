# Affine Transform 

## Transform matrix in 2D

### Scale matrix

$$\begin{bmatrix}x \prime\\y \prime\\\end{bmatrix} = \begin{bmatrix}S_x&0\\0&S_y\\\end{bmatrix}\begin{bmatrix}x\\y\\\end{bmatrix}$$

### Rotate matrix

$$\begin{bmatrix}x \prime\\y \prime\\\end{bmatrix}=\begin{bmatrix}cos\theta&-sin\theta\\sin\theta&cos\theta\\\end{bmatrix}\begin{bmatrix}x\\y\\\end{bmatrix}$$

### Reflection matrix

$$\begin{bmatrix}x \prime\\y \prime\\\end{bmatrix}=\begin{bmatrix}-1&0\\0&1\\\end{bmatrix}\begin{bmatrix}x\\y\\\end{bmatrix}$$

### Shear matrix

$$\begin{bmatrix}x \prime \\y \prime \\\end{bmatrix}=\begin{bmatrix}1&t_x\\t_y&1\\\end{bmatrix}\begin{bmatrix}x\\y\\\end{bmatrix}$$

### Translation

- By using homogeneous coordinates (齐次坐标系），add a third coordinate[w-coordinate] , so $point(x,y) = (x,y,w)^T= (x/w,y/w,1)^T$ , w!=0 in here ;

- In homogenerous coordinates:
  
  $2D \space point = (x,y,1)^T\\ 2D \space vector = (x,y,0)^T\\$

- Translation matrix：

$$\begin{bmatrix}x \prime \\y \prime \\1\\\end{bmatrix}=\begin{bmatrix}1&0&t_x\\0&1&t_z\\0&0&1\\\end{bmatrix}\begin{bmatrix}x\\y\\1\\\end{bmatrix}$$

### Composit transform

- Composite transform  (缩放旋转平移)：
$$T \cdot R_{\theta} \cdot S \cdot A=\begin{bmatrix}x \prime\\y \prime\\1\\\end{bmatrix}=\begin{bmatrix}1&0&t_x\\0&1&t_y\\0&0&1\\\end{bmatrix}\begin{bmatrix}cos\theta&-sin\theta&0\\sin\theta&cos\theta&0\\0&0&1\\\end{bmatrix}\begin{bmatrix}s_x&0&0\\0&s_y&0\\0&0&1\\\end{bmatrix}\begin{bmatrix}x\\y\\1\\\end{bmatrix}$$

- Simplified formula：
$$T \cdot R_{\theta} \cdot S \cdot A = \begin{bmatrix}x \prime\\y \prime\\1\\\end{bmatrix} = \begin{bmatrix}s_x \cos\theta&-s_y \sin\theta&t_x\\s_x \sin\theta&s_y \cos\theta&t_y\\0&0&1\\\end{bmatrix} \begin{bmatrix}x\\y\\1\\\end{bmatrix}$$

### Affine Transform 仿射变换

- linear transform + translation =  affine transformations (线性变换 + 平移 = 仿射变换)

## Transform matrix in 3D

- $ 3D \space point = (x,y,z,1)^T \\ 3D \space vector = (x,y,z,0)^T $
### Scale

- Scale matrix

$$\begin{bmatrix} x \prime\\  y \prime \\ z \prime \\\end{bmatrix} = \begin{bmatrix} S_x&0&0\\0&S_y&0\\0&0&S_z\end{bmatrix}\begin{bmatrix} x \\  y \\ z  \\\end{bmatrix}$$

### Rotation

- Rotation around x-axis:
$$R_x(\alpha) = \begin{bmatrix} x \prime\\  y \prime \\ z \prime \\ 1 \\\end{bmatrix} = \begin{bmatrix} 1&0&0&0\\0&cos\alpha&-sin\alpha&0\\0&sin\alpha&cos\alpha&0 \\ 0&0&0&1\end{bmatrix}\begin{bmatrix} x \\  y \\  z \\ 1  \\\end{bmatrix}$$

- Rotation around y-axis :
$$R_y(\alpha) = \begin{bmatrix} x \prime\\  y \prime \\ z \prime \\ 1 \\\end{bmatrix} = \begin{bmatrix} cos\alpha&0&sin\alpha&0\\0&1&0&0\\-sin\alpha&0&cos\alpha&0 \\ 0&0&0&1\end{bmatrix}\begin{bmatrix} x \\  y \\  z \\ 1  \\\end{bmatrix}$$

  在绕y轴旋转中，有一些特殊，因为在right-hand coorinate下, $\vec{X} \times \vec{Z}$的结果是指向Y的负方向，根据$\vec{A} \vec{A}^T = I_n$，绕Y轴旋转，y坐标不变，所以剩下的矩阵要按逆矩阵的方式写 ;

- Rotation around z-axis: 
$$R_z(\alpha) = \begin{bmatrix} x \prime\\  y \prime \\ z \prime \\ 1 \\\end{bmatrix} = \begin{bmatrix} cos\alpha&-sin\alpha&0&0\\sin\alpha&cos\alpha&0&0\\0&0&1&0 \\ 0&0&0&1\end{bmatrix}\begin{bmatrix} x \\  y \\  z \\ 1  \\\end{bmatrix}$$

### Translation

- $$T(t) = \begin{bmatrix} x \prime\\  y \prime \\ z \prime \\ 1 \\\end{bmatrix} = \begin{bmatrix} 1&0&0&T_x\\0&1&0&T_y\\0&0&1&T_z \\ 0&0&0&1\end{bmatrix}\begin{bmatrix} x \\  y \\  z \\ 1  \\\end{bmatrix}$$

