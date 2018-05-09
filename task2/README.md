## css 2D变换

## 1.1 背景知识
`transform`是变换元素的一个css属性，可以实现元素`translate平移`  `rotate旋转` `scale伸缩` `skew倾斜`
* `translate`:  `translateX()` x方向平移，`translateY()` y方向平移，`translate(x, y)` 同时在xy方向平移
* `rotate`: `rotate(angle)` 相对元素中心点旋转，正值顺时针，负值逆时针
* `skew`: `skewX(angle)` 元素x方向倾斜角度，`skewY(angle)` 元素y方向倾斜角度，`skew(xangle, yangle)` 同时在xy方向倾斜角度
* `scale`: `skewX(n)` x轴方向缩放，`scaleY(n)` y轴方向缩放，`scale(n, m)` 同时xy轴缩放
* 可通过 `transform-origin`改变变换的参照中心点

上面都是 2D transform的快捷方法，可以使用matrix()矩阵变换综合实现所有变换效果：  
* `matric(a, b, c, d, e, f)`  
* `matrix(1, 0, 0, 1, x, y)`：平移，x和y分别是x和y方向的平移量，等同于translate(x, y)
* `matrix(n, 0, 0, m, 0, 0)`：缩放，n和m分别是xy方向的缩放倍数，等同于skew(n ,m)
* `matrix(sin(angle), cos(angle), -cos(angle), sin(angle), 0, 0)`：旋转，等同于rotate(angle)
* `matrix(1, tan(anglex), tan(angley), 1, 0, 0)`：倾斜，等同于skew(anglex, angley)

## 1.2 matrix计算的原理：  
假设点的原坐标为 `(x, y)`，经过 `matrix(a, b, c, d, e, f)`变换后坐标为 `(x1, y1)`，如下计算：    
```
a c e     x     ax+cy+e     x1  
b d f  *  y  =  bx+dy +f =  y1  
0 0 1     1       1         1  
```

上面是矩阵相乘的计算，行列对应项相乘后相加，得到的 x1坐标就是 ax+cy+e，y1坐标就是 bx+dy+f，因此有结果：`(x, y)` →`(ax+cy+e,  bx+dy +f)`。<br/>
* 如果平移，那么 dx = ax+cy+e - x，dy = bx+dy +f - y，由于matrix默认的变换中心点是元素中心，假设元素中心坐标为(0, 0)，那么平移后 dx = e, dy = f ,因此坐标为（e, f）

## 1.3 变换叠加
就本例demo的最后一个矩形box ，要综合实现 倾斜 30度，x方向缩小2倍，顺时针旋转 45度，x方向平移10px，y方向平移20px的叠加效果，就可以将变换相乘叠加得到最终变换效果：
``matrix(1, 0, √3/3, 1, 0, 0)*matrix(0.5, 0, 0, 1, 0, 0)*matrix(√2/2,√2/2, -√2/2, √2/2, 0, 0)*matrix(0, 0, 0, 0, 10, 20) = matrix(0.76180, 0.707107, 0.054695, 0.707107, 8.71191, 21.2132)``
