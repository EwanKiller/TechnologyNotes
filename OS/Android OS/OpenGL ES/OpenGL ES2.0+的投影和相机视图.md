# OpenGL ES 2.0+版本中的投影和相机视图

1. 为顶点着色器程序添加矩阵 - 为视图投影矩阵创建一个变量，并将其作为着色器程序位置的调节系数；下面代码中包含一个uMVPMatrix成员，它可以将投影和相机视图矩阵应用到使用此着色器程序的对象的坐标：
```java
private final String vertexShaderCode =
    "uniform mat4 uMVPMatrix;   \n" +
    "attribute vec4 vPosition;  \n" +
    "void main(){               \n" +
    " gl_Position = uMVPMatrix * vPosition; \n" +
    "}  \n";
```
2. 访问着色器程序矩阵 - 在顶点着色程序中创建钩子机制以分别应用投影和相机视图后，可以访问该变量，以应用投影和相机查看矩阵：
```java
public void onSurfaceCreated(GL10 unused, EGLConfig config) {
    ...
    muMVPMatrixHandle = GLES20.glGetUniformLocation(program, "uMVPMatrix");
    ...
}
```
3. 创建投影和相机视图矩阵 - 生成要应用于图形对象的投影和视图矩阵：
```java
public void onSurfaceCreated(GL10 unused, EGLConfig config) {
    ...
    // Create a camera view matrix
    Matrix.setLookAtM(vMatrix, 0, 0, 0, -3, 0f, 0f, 0f, 0f, 1.0f, 0.0f);
}

public void onSurfaceChanged(GL10 unused, int width, int height) {
    GLES20.glViewport(0, 0, width, height);
    float ratio = (float) width / height;
    // create a projection matrix from device screen geometry
    Matrix.frustumM(projMatrix, 0, -ratio, ratio, -1, 1, 3, 7);
}
```
4. 应用投影和相机视图矩阵 - 要应用投影和相机视图转换，将这些矩阵相乘，再设置到顶点着色器程序中：
```java
public void onDrawFrame(GL10 unused) {
    ...
    // Combine the projection and camera view matrices
    Matrix.multiplyMM(vPMatrix, 0, projMatrix, 0, vMatrix, 0);
    // Apply the combied projection and camera view transformations
    GLES20.glUniformMatrix4fv(muMVPMatrixHandle, 1, false, vPMatrix, 0);
    // Draw objects
    ...
}
```