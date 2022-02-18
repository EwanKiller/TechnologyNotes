# Model&View&Projection Transformation (MVP)
## Think about how to take a photo ?
- Find a good place and arrange people (model transformation)
- Find a good "angle" to put the camera (view transformation)
- Click shutter ! (projection transformation)
## Modeling Transformations
- 利用基本的变换矩阵(旋转，缩放，平移)，把世界中的模型变换到我们想要放置的地方；
## View/Camera Transformations
- How do you define camera ?
  - Camera at position called $\hat{e}$ , look-at called $\hat{g}$, loop-up called $\hat{t}$;
  - Camera located at origin , look at -Z axis , look up at Y axis ;
  - $M_{view} = R_{view}T_{view}$ 
    - Translate to origin :$T_{view} = \begin{bmatrix} 1&0&0&-X_e\\0&1&0&-Y_e\\ 0&0&1&-Z_e\\0&0&0&1\end{bmatrix}$
    - Rotate $\hat{g}$ to -Z , $\hat{t}$ to Y , $\hat{g} \times \hat{t}$ to X ?
    - Consider its inverse rotation :  rotate X to $\hat{g} \times \hat{t}$, Y to $\hat{t}$ , Z to $-\hat{g}$:$R_{view}^{-1} = \begin{bmatrix}X_{\hat{g} \times \hat{t}}&X_t&X_{-g}&0\\Y_{\hat{g} \times \hat{t}}&Y_t&Y_{-g}&0\\Z_{\hat{g} \times \hat{t}}&Z_t&Z_{-g}&0\\0&0&0&1 \end{bmatrix}$
    - $R_{view}= \begin{bmatrix}X_{\hat{g} \times \hat{t}}&Y_{\hat{g} \times \hat{t}} &Z_{\hat{g} \times \hat{t}} &0\\X_t&Y_t&Z_t&0\\X_{-g}&Y_{-g}&Z_{-g}&0\\0&0&0&1 \end{bmatrix}$

## Projection Transformations
### Orthographic projection
- A simple way to understand
  - Camera located at origin . Look at -Z . Up at  Y ;
  - Drop Z coordinate ;
  - Translate and scale the resulting rectangle to $$[-1,1]^2$$
- In general 
  - A canonical cube $$[l,r] \times [b,t] \times [f,n] \space to \space [-1,1]^3$$;
  - $M_{ortho} = S_{ortho} T_{ortho} = \begin{bmatrix} \frac{2}{r-l}&0&0&0\\0&\frac{2}{b-t}&0&0\\0&0&\frac{2}{n-f}&0\\0&0&0&1\end{bmatrix} \begin{bmatrix}1&0&0&-\frac{r+l}{2}\\0&1&0&-\frac{t+b}{2}\\0&0&1&-\frac{n+f}{2}\\0&0&0&1 \end{bmatrix}$

### Perspective projection
- $M_{persp} = M_{ortho} M_{persp->ortho}$ 即先通过挤压远平面到与近平面一致，然后再做一次正交投影变换 ;
- 推导过程：
  - 从上图根据相似三角形可以得到一个结论：
  $x \prime = \frac{n}{z} x \space and \space y \prime = \frac{n}{z}y$
   ,所以可以到得到以下:
   $$\begin{bmatrix}x\\y\\z\\1 \end{bmatrix} = \begin{bmatrix} \frac{nx}{z}\\\frac{ny}{z}\\unknow\\1 \end{bmatrix} =multi \space by \space z=> \begin{bmatrix}nx\\ny\\unknow\\z \end{bmatrix}$$
1. 如果要使$M_{persp->ortho} \begin{bmatrix} x\\y\\z\\1\end{bmatrix} = \begin{bmatrix}nx\\ny\\unknow\\z \end{bmatrix}$$成立，则$$M_{persp->ortho} = \begin{bmatrix}n&0&0&0\\0&n&0&0\\0&0&\alpha&\beta\\ 0&0&1&0\end{bmatrix}$
2. a-在远平面的中心的点 A ，在挤压过程中z值不会改变；b-在近平面的的点 B，所有的z值均不会改变 ；
    - 根据a-可得$A = M_{persp->ortho} \begin{bmatrix} x\\y\\z\\1\end{bmatrix} = \begin{bmatrix} 0\\0\\f\\1\end{bmatrix} =mult by f=\begin{bmatrix} 0\\0\\f^2\\f\end{bmatrix}==>\begin{bmatrix}0&0&\alpha&\beta \end{bmatrix} \begin{bmatrix} 0\\0\\f\\1\end{bmatrix} = f^2==>\alpha f + \beta = f^2$；

    - 根据b-可得 $B = M_{persp->ortho} \begin{bmatrix} x\\y\\z\\1\end{bmatrix} = \begin{bmatrix} 0\\0\\n\\1\end{bmatrix} =mult by n=\begin{bmatrix} 0\\0\\n^2\\n\end{bmatrix} ==> \begin{bmatrix}0&0&\alpha&\beta \end{bmatrix} \begin{bmatrix} 0\\0\\n\\1\end{bmatrix} = n^2 ==> \alpha n + \beta = n^2$
    - 联合$\begin{cases}\alpha f + \beta = f^2 \\\alpha n + \beta = n^2\end{cases}$$==> $$\begin{cases}  \alpha = f+n\\ \beta = -nf\end{cases}$
    - $M_{persp->ortho} = \begin{bmatrix}n&0&0&0\\0&n&0&0\\0&0&f+n&-nf\\ 0&0&1&0\end{bmatrix}$
- Simplifi formula : $M_{persp} = \begin{bmatrix} \frac{2}{r-l}&0&0&0\\0&\frac{2}{b-t}&0&0\\0&0&\frac{2}{n-f}&0\\0&0&0&1\end{bmatrix} \begin{bmatrix}1&0&0&-\frac{r+l}{2}\\0&1&0&-\frac{t+b}{2}\\0&0&1&-\frac{n+f}{2}\\0&0&0&1 \end{bmatrix}\begin{bmatrix}n&0&0&0\\0&n&0&0\\0&0&f+n&-nf\\ 0&0&1&0\end{bmatrix}$

- 合并后得$$M_{persp} = \begin{bmatrix} \frac{2n}{r-l}&0&-\frac{r+l}{r-l}&0\\0&\frac{2n}{t-b}&-\frac{t+b}{t-b}&0\\0&0&\frac{2(f+n)}{n-f}&-\frac{n+f}{n-f}\\0&0&1&0\end{bmatrix}$$
- 如果已知近平面为zNear，远平面为zFar，视野为eyeFov，宽高比为 aspectRatio ，则可以计算出$$\begin{cases}height=zNear * \tan(\frac{(eyeFov)}{2})\\width = height * aspectRatio\end{cases}$$,并且很容易推得-(r+l),-(t+b)均等于0，r-l=width,t-b=height；
- 于是$$M_{persp} = \begin{bmatrix} \frac{2*zNear}{width}&0&0&0\\0&\frac{2*zNear}{height}&0&0\\0&0&\frac{2(zNear+zFar)}{zNear-zFar}&-\frac{zNear+zFar}{zNear-zFar}\\0&0&1&0\end{bmatrix}$$

