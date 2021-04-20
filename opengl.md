### opengl 笔记

### 

**近平面**：近平面距离越远，显示区域越小。

**坐标原点：**三维坐标中，绘制物体需要指定坐标，设置的平截头体是一些标量值。跟坐标没有任何关系

三维坐标系来说，如果把相机的位置设置为坐标原点，那么应该是往里看。（z轴的负方向）

如果近平面到远平面的距离是4，摄像机到近平面是3。

平时画物体，**最好以整个平截头体的中心，在三维平面的中心上**。 相机的坐标是(0,0,5)

**相机的摆正，镜头的朝向**当拍照的时候，发现大头朝下。只是告诉了相机放在什么位置，并没有把相机摆正。

所以相机还得有一个指向上方得一个向量。**相机不仅仅是有上下，还有左右的位置。相机的镜头要对准z轴去拍。** 还要指定**镜头的朝向**。



openGL:matrx 矩阵。

opengl 是一个状态机。



**投影**：

​     透视投影： 有深度，越远越小。

​     正投影：没有深度概念，相同大小。

​    这两种投影都是坐标变换。



 **单位矩阵：**

 从左上角到右下角的对角线（称为主对角线）上的元素均为1。除此以外全都为0

 和任何矩阵相乘都等于任何矩阵，相当于自然数1，任何矩阵在操作之前，都得恢复单位矩阵。矩阵的清0.



模型矩阵和投影矩阵

从GL的角度看场景时，您有一个照相机和一个镜头。

ModelView是代表相机的矩阵（位置，指向和上矢量）。

ProjectionView是代表相机镜头（光圈，远场，近场等）的矩阵。

```java
		public void onDrawFrame(GL10 gl) {
		//清除颜色缓冲区
		gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
		//设置绘图颜色
		gl.glColor4f(1f, 0f, 0f, 1f);
		
		//操作模型视图矩阵
		gl.glMatrixMode(GL10.GL_MODELVIEW);
		gl.glLoadIdentity();
		//设置眼球的参数
		GLU.gluLookAt(gl,0f,0f,5f, 0f, 0f, 0f, 0f,1f,0f);
		
		//旋转角度
		gl.glRotatef(xrotate, 1, 0, 0);
		gl.glRotatef(yrotate, 0, 1, 0);
		
		//计算点坐标
		float r = 0.5f ;//半径
		float x = 0f,y = 0f,z = 1f ;
		float zstep = 0.01f ;
		float psize = 1.0f ;
		float pstep = 0.5f ;
		
		//循环绘制点
		for(float alpha = 0f ; alpha < Math.PI * 6 ; alpha = (float) (alpha + Math.PI / 16)){
			x = (float) (r * Math.cos(alpha));
			y = (float) (r * Math.sin(alpha));
			z = z - zstep ;
			gl.glPointSize(psize = psize + pstep);
			//转换点成为缓冲区
			gl.glVertexPointer(3, GL10.GL_FLOAT, 0, BufferUtil.arr2ByteBuffer(new float[]{x,y,z}));
			gl.glDrawArrays(GL10.GL_POINTS, 0, 1);
		}
		
		
	}
```

```java
	public void onSurfaceChanged(GL10 gl, int width, int height) {
		//设置视口
		gl.glViewport(0, 0, width, height);
		ratio = (float)width / (float)height;
		//投影矩阵
		gl.glMatrixMode(GL10.GL_PROJECTION);
		//加载单位矩阵
		gl.glLoadIdentity();
		//设置平截头体
		gl.glFrustumf(-ratio, ratio, -1, 1, 3f, 7f);
	}
```



参考：

https://cn.faithcov.org/768422-difference-between-glmatrixmodegl-projection-and-LEHAHQ







