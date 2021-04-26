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



openGL:开放图形库,es(嵌入式系统)
openGLES:一个子集.

3D游戏:
图形处理:

openGL语言:
函数:

c:
java:

动画:
ps:
透视:

矩阵:

SurfaceView:表层视图

GLSurfaceView:
renderer:渲染器

黑色  +  白色
000  255 

**viewPort:视口,输出画面的区域**.

openGL:matrix,矩阵.

投影:
	透视投影:有深度,越远越小.
	正投影:没有深度,相同大小.

GLSurfaceView:GL表层视图,输出openGL画面的控件.
GLSurfaceView.Renderer:openGL渲染器,绘制openGL的类.

gl.glMatrixMode(int n);
	矩阵模式,openGL基于状态的,操纵很多矩阵,通过该函数指定使用哪个矩阵.
	常用的矩阵有:
		**GL10.GL_PROJECTION:投影矩阵.**
		**GL10.GL_MODELVIEW:模型视图矩阵**.
	指定使用哪个矩阵之后**,需要先加载单位矩阵(使用gl.loadIdentity()方法,类似于矩阵归零.**

frustum:平截头体,拍摄画面的一个区域,是一个棱台,对投影矩阵进行操纵.
		参数:
		left:左侧距离
		right:右侧距离
		bottom:下方距离
		top:上方距离
		zNear:近平面距离
		zFar:远平面距离
		gl.glFrustum(,,,,,,);

设置眼睛的位置.
	//涉及到眼球的坐标,观察的点,向上的向量
	//操纵的是模型视图矩阵,因此需要先设置gl.glMatrixMode(Gl_modelview);
	GLU.gluLookat(gl,eyex,eyey,eyez,centerx,centery,centerz,upx,upy,upz);

使用openGL步骤:
1.创建GLSurfaceView对象
2.创建GLSurfaceView.renderer实现类.
3.设置activity的contentView,以及设置view的render对象.
4.实现render类的过程.
	a.onSurfaceCreate()方法
		1.**设置清屏的颜色和启用顶点缓冲区**
		//设置清屏色
		gl.glClearColor(0, 0, 0, 1);
		//启用顶点缓冲区.
		gl.glEnableClientState(GL10.GL_VERTEX_ARRAY);
	b.onSurfaceChanged()方法
		1.设置viewport(视口)
			gl.glViewport(0, 0, width, height);
		2.操纵投影矩阵,设置平截头体(比例通常和视口比例相同,否则输出画面会走样)
			//矩阵模式,投影矩阵,openGL基于状态机
			gl.glMatrixMode(GL10.GL_PROJECTION);
			//加载单位矩阵
			gl.glLoadIdentity();
			//平截头体
			gl.glFrustumf(-1f, 1f, -ratio, ratio, 3, 7);
	c.onDrawFrame()方法
		1.清除颜色缓冲区
			gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
		2.操纵模型视图矩阵,设置眼球的参数
			gl.glMatrixMode(GL10.GL_MODELVIEW);
			gl.glLoadIdentity();//加载单位矩阵
			GLU.gluLookAt(gl, 0, 0, 5, 0, 0, 0, 0, 1, 0);
		3.定义图形顶点坐标值数组
			float[] coords = {
				0f,0.5f,0f,
				-0.5f,-0.5f,0f,
				0.5f,-0.5f,0f
			};
		4.将顶点坐标转换成缓冲区数据
			//分配字节缓存区空间,存放顶点坐标数据
			ByteBuffer ibb = ByteBuffer.allocateDirect(coords.length * 4);
			//设置的顺序(本地顺序)
			ibb.order(ByteOrder.nativeOrder());
			//放置顶点坐标数组
			FloatBuffer fbb = ibb.asFloatBuffer();
			fbb.put(coords);
			//定位指针的位置,从该位置开始读取顶点数据
			ibb.position(0);
		5.设置绘图颜色
			gl.glColor4f(1f, 0f, 0f, 1f);
		6.指定顶点缓冲区指针
			//3:3维点,使用三个坐标值表示一个点
			//type:每个点的数据类型 
			//stride:0,跨度.
			//ibb:指定顶点缓冲区
			gl.glVertexPointer(3, GL10.GL_FLOAT, 0, ibb);
		7.绘图
			//0:起始点:
			//3:绘制点的数量
			gl.glDrawArrays(GL10.GL_TRIANGLES, 0, 3);

纹理:
	openGLES1.0 ---> openGL1.3
	openGLES1.1 ---> openGL1.5
	
**pipeline:管线:**

管线 是一个过程，可能涉及两个或多个独特的阶段或步骤，应用程序进行OpenGL函数调用时，这些命令被放置在一个命令缓冲区，该缓冲区最终填满了命令，定点数据，纹理数据等东西，缓冲区被刷新时，命令和数据就传递给下一个阶段。





opengl application: geometry(几何图形) + texture(纹理贴图)
vertex data(顶点数据):lighting(光照) transform(变换) scale:缩放
geometry:rasterization(光栅) clipping(剪裁) **将几何信息转换成一个个的栅格组成的图像的过程**

fragment(段):fog(雾) + texture.
framebufer(帧缓冲区):stecil(蒙版) z-test:深度测试 alpha: 透明 blending(混合)
eyeball(眼球):

屏幕坐标:原点在左上
		 |-------->  		
		 |          
		 |          
		 |          
		\|/         
		 .          
数学坐标系:原点左下角
	   /|\
		|
		|
		o-------->

openGL3D坐标系(笛卡尔坐标系)
		|
		|
		|
		|-------->
	   /
	  /
	 /

clip:

renderer:渲染
渲染模式:
	持续渲染.

**图元:**
1.点
	pionts:
2.线 
	lines:线段集合
	line_strip,线带,没有成圆环
	line_loop,线环

![](img/02.bmp)

3.三角形
	triangles:三角形集
	triangle_strip:三角形带
	triangle_fan:扇面.

顶点着色模式:
	1.smooth,平滑模式.默认
	2.flat,单调.

深度轴:z轴
深度测试:启用z值,被遮挡的问题看不见

剔除:如果看不见的部分,告诉openGL不要绘图.



### 点的绘制

**3D中绘制点**
像素是计算机监视器上最小的元素.在彩色系统中,像素可以是许多可用颜色的一种.屏幕上的一个位置对应一个点,并为
之指定颜色.然后,在此基础上,用最擅长的计算机语言生成直线,多边形,圆形以及其他图形,甚至整个GUI.
**设置3D画布**
下图把可视区域看成是三维画布,可使用OpenGL命令和函数进行绘图

![](img\01.png)

