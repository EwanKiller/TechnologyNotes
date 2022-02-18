# Math in graphic

## Linear Algebra 

### Vector

- Usually written as $$\vec{a}$$ , called vector a ; 
- Using end point subtract start point : $$ \vec{AB}=B-A$$ ;
- Represent direction and length ;
- But no absolute starting position ;

### Vector Normalization

- Magnitude(length) of a vector written as $$||\vec{a}||$$; 
- Unit Vector
  - A vector with magnitude of 1;
  - Usually written as $$\hat{a}$$ , called a hat ;
  - Used to represent direaction;

### Matrix

- 矩阵是为了更方便的表达向量与空间的线性关系；
- 两种表示形式：colum matrix  ：$$\begin{bmatrix}x\\y \end{bmatrix}$$，row matrix：$$\begin{bmatrix}x&y \end{bmatrix}$$；

- Default use **column matrix** to represent vector ： $\vec{a} = (x,y) => A = \begin{bmatrix}x\\y\\ \end{bmatrix}$, 目的是为了可以方便左乘；
- matrix - matrix muliplication 的记忆规则 **一招解决不用背** ！
  - ***结果在 第n行  第m列 ，就找matrix X的第n行 与 matrix Y的第m列，对应位置相乘然后加起来*** ;
  $$X \times Y=\begin{bmatrix}a&b\\c&d\\\end{bmatrix}\begin{bmatrix}A&B\\C&D\\\end{bmatrix}=\begin{bmatrix}aA+bC&aB+bD\\cA+dC&cB+dD\\\end{bmatrix}$$

###  Base of Coordinate System

- base ;
  - 坐标基$${\begin{bmatrix}A\\B\\C \end{bmatrix}}$$概念引入是为了表达一个坐标系内所有的点（x,y,z) 都有Ax + By + Cz = 0 这种线性关系；
  - 即任意一点P(x,y,z)用向量表示$\vec{OP}$，在以${\begin{bmatrix}A\\B\\C \end{bmatrix}}$为坐标基的坐标系下，均满足Ax + By + Cz = 0 这种线性关系；O代表坐标原点；

- unit base ;
  - 单位基：比如常见的笛卡尔坐标系，就是选取 unit  base = ${\begin{bmatrix}1\\1\\1 \end{bmatrix}}$当作单位基，单位基的各项均为1；

### Matrix with Coordinate Base

- For **orthogonal unit base =** $$\begin{bmatrix}1\\1\\1 \end{bmatrix}$$in Cartesian Coordinates : Point A written as matrix$$ A = \begin{bmatrix} x\\y\\z \end{bmatrix} \space A^{T} = \begin{bmatrix}x&y&z \end{bmatrix} $$ ;

  -  $$A^{T} \times UnitBase= \begin{bmatrix}x&y
    &z \end{bmatrix}\times \begin{bmatrix}1\\1\\1 \end{bmatrix} = 0 $$ ；
- 在笛卡尔坐标系下定义一个正交单位基$$\begin{bmatrix}1\\1\\1 \end{bmatrix}$$，该坐标系中任意一点P(x,y,z)与坐标原点O构成的向量$$\vec{OP}$$可以与正交单位基相乘得到一个线性关系：$$\vec{OP}^{T} \times UnitBase= \begin{bmatrix}x&y
&z \end{bmatrix}\times \begin{bmatrix}1\\1\\1 \end{bmatrix} = 0 => x * 1+ y * 1 + z * 1 = 0  $$  ；这里是将向量$$\vec{OP}$$写成column matrix形式；

### 坐标系的基

- 当讨论空间中的一个向量时，往往会先确定该向量所在的坐标系，原因如下：
  - 对于一个向量A（2，3），在以$$\hat{i},\hat{j}$$为基的坐标系中，向量A为（2i，3j）；当该向量在以$$\hat{m},\hat{n}$$为基的坐标系中，向量A为（2m，3n）;也就是说向量A写法虽然不变，但在不同的基的作用下将会表示不同的方向，所以坐标系的变换的实质就是坐标基的变换，而坐标基可以写成矩阵的形式$$\begin{bmatrix} i_x & j_x\\i_y & j_y \end{bmatrix}$$，于是对于任意向量$$\vec{a} = \begin{bmatrix} x_a\\y_a\end{bmatrix}$$，在笛卡尔坐标系下，我们认为它的坐标基为$$\hat{i} = (1,0) , \hat{j} = (0,1)$$，当我们想把该该向量变换到其他坐标系下时，我们只需要找到该坐标系的基，然后通过矩阵变换即可得到该坐标系下，向量$$\vec{a}$$的对应向量$$\vec{a^{\prime}}$$:$$\vec{a^{\prime}} = \begin{bmatrix} \hat{m} & \hat{n}\end{bmatrix} \begin{bmatrix} x_a \\ y_a \end{bmatrix} = \begin{bmatrix} m_x & n_x \\ m_y & n_y \end{bmatrix} \begin{bmatrix} x_a \\ y_a \end{bmatrix} $$;

### Dot Product 

$$\vec{a} \cdot \vec{b} = ||\vec{a}|| *||\vec{b}||* \cos\theta$$

In 2D :  $$\vec{a} \cdot \vec{b} = x_{a}x_{b} + y_{a}y_{b} $$

In 3D :  $$\vec{a} \cdot \vec{b} = x_{a}x_{b} + y_{a}y_{b} + z_{a}z_{b} $$

#### dot product in graphic

- determine how close between $\vec{a}$ and $\vec{b}$
- determine $\vec{b}$ is forward or backward to $\vec{a}$

### Cross Product 

$$||\vec{a} \times \vec{b}|| = ||\vec{a}|| ||\vec{b}|| \sin\theta$$

$$\vec{a} \times \vec{b} = A*b = \begin{bmatrix} 0 & -z_a & y_a \\ z_a & 0 & -x_a \\ -y_a & x_a & 0 \end{bmatrix} \begin{bmatrix}x_b \\ y_b \\ z_b \end{bmatrix} = \begin{bmatrix} -z_{a}y_b+y_{a}z_{b}\\ z_{a}x_{b}-x_{a}z_{b}\\ -y_{a}x_{b}+x_{a}y_{b} \end{bmatrix}$$

#### cross product in graphic

- determine $\vec{b}$ is on right or left to $\vec{a}$ ;
  - In right-hand coordinates , $$(\vec{b} \times \vec{a}).z$$ > 0 , indicate $$\vec{b}$$ is on right to $$\vec{a}$$;
- determine point a is inside or outside to a traingle ;
  - Ensure triangle points order  ;
    - for example by anticlockwise  , we have a  $$\triangle_{ABC}$$ and point P ,  $$\vec{AB} \times \vec{AP} = \vec{a},\vec{BC} \times \vec{BP} = \vec{b}, \vec{CA} \times \vec{CP} = \vec{c}$$ ;
    - If a.z b.z c.z are both positive or negative , that means point P is inside of $$\triangle{ABC}$$;


### 更多阅读
[GAMES101: 现代计算机图形学入门 - 第一周第二周课程](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html)