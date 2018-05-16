# css 3D 变换

## transform 3D 转换属性
* ``transform-origin: x-axis y-axis z-axis``:变换的原点坐标
* ``transform-style:flat|preserve-3d``:3D空间中子元素的视觉效果，`flat`为平面，`preserve-3d`为3d透视，一般使用 `preserve-3d`
* ``perspective``:设置元素距视图的距离，元素定义 perspective 属性时子元素具有3d透视效果
* ``perspective-origin: x-axis y-axis``:定义3d元素透视中心
* ``backface-visibility:visible|hidden``:定义3d元素背面是否可见

## 3d 变换类型
* ``translate3d(x,y,z)`` 平移
* ``translateX(x)`` x轴平移
* ``translateY(y)`` y轴平移
* ``translateZ(z)`` z轴平移
* ``scale3d(x,y,z)`` 缩放
* ``scaleX(x)`` x轴长度缩放
* ``scaleY(y)`` y轴长度缩放
* ``scaleZ(z)`` z轴长度缩放 
* ``rotate3d(x,y,z,angle)`` 旋转
* ``rotateX(angle)`` 绕x轴旋转
* ``rotateY(angle)`` 绕y轴旋转
* ``rotateZ(angle)`` 绕z轴旋转

## 终极矩阵变换
``matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)`` 使用16个值的矩阵变换