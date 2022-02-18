# 响应触摸事件
[响应触摸事件官方文档](https://developer.android.com/training/graphics/opengl/touch?hl=zh-cn)

让OpenGL ES应用响应触摸互动关键是拓展GLSurfaceView实现以替换onTouchEvent(),从而监听触摸事件；

## 设置触摸监听器

- 监听MotionEvent.ACTION_MOVE事件：

```java

private float previousX = NULL;
private float previousY = NULL;

@Override
public boolean onTouchEvent(MotionEvent e) {
    float x = e.getX();
    float y = e.getY();

    switch(e.getAction()){
        case MotionEvent.ACTION_MOVE:
            float dx = x - previousX;
            float dy = y - previousY;

            ......
            // 调用requestRender()告知渲染程序该渲染帧里；
            requestRender();
    }
    previousX = x;
    previousY = y;
    return true;
}
```
当设置RenderMode后，可以要求渲染程序仅在数据发生更改时重新绘制；
```java
public MyGLSurfaceView(Context context) {
    ...
    // Render the view only when there is a change in the drawing data
    SetRenderMode(GLSurfaceView.RENDERMODE_WHEN_DIRTY);
}
```