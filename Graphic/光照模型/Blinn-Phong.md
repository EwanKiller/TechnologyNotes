- 最终光照 = 环境光 + 漫反射 + 高光
即$L_(out) = L_(ambient) + L_(diffuse) + L_(specular)$  

- 通常会写成：


![[公式]](https://www.zhihu.com/equation?tex=L_%7Bout%7D+%3D+C_%7Bambi%7Dk_a+%2Bk_dC_%7Bdiff%7D%28n.l%29+%2B+k_sC_%7Bspec%7D%28n.h%29%5Em)

- ![[公式]](https://www.zhihu.com/equation?tex=C_%7Bambi%7D) 表示环境光颜色，Ka是修正系数；

- ![[公式]](https://www.zhihu.com/equation?tex=C_%7Bdiff%7D) 表示漫反射颜色，Kd是修正系数，**n.l**是表面法线n和光线方向l的点乘，也就是两向量的余弦，可记为 ![[公式]](https://www.zhihu.com/equation?tex=cos%28%5Ctheta_l%29)
- ![[公式]](https://www.zhihu.com/equation?tex=C_%7Bspec%7D) 表示高光颜色，Ks是修正系数，**n.h**是表面法线n和半角向量h的点乘，也就是两向量的余弦，可记为 ![[公式]](https://www.zhihu.com/equation?tex=cos%28%5Ctheta_h%29) ,其中h半角向量是光照方向l和观察方向v的相加并归一化，m是高光因子，越大高光效果越强，亮点越聚集。

- 为了效果与阴影的加入，实际游戏渲染通常对Blinn-Phong模型公式计算进行了一定变形,比如。

![[公式]](https://www.zhihu.com/equation?tex=L_%7Bout%7D+%3D+C_%7Bambi%7Dk_a+%2Bk_dk_%7Bshadow%7DC_%7Bdiff%7D%280.5%2Amax%280%2C%28n.l%29%29%2B0.5%29+%2B+k_sC_%7Bspec%7Dmax%280%2C%28n.h%29%29%5Em)

