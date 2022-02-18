# 空间变化过程

- Vertex（Model Space）
- =>  Model Matrix 
- => Vertex（World Space）
- => View Matrix 
- => Vertex (View Space) 
- => Projection Matrix 
- => Vertex(Clip Space) 
- => Projection Division
-  => Vertex(NDC space) 
- => ViewPort Matrix
-  =>Vertex(Screen Space)

# Shader中的输入输出空间

- Vertex Shader的输入是Model Space，输出是Clip Space；
- Frament Shader的输入是Screen Space；