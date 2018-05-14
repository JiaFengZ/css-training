## 使用 `transition` 和 `transform` 实现笑脸动画
这是一个可爱的小猫，我们的目标是鼠标移上去是让它笑起来，动作可划分为4部分：耳朵竖起、眼睛咪起、嘴巴翘起、脸红。
* 耳朵竖起
```css
.ear {
    transition: transform 1s ease-out;
}
.face:hover + .ear-wrap > .ear.left {
    transform: rotate(-30deg);
}
.face:hover + .ear-wrap > .ear.right {
    transform: rotate(30deg);
}
```

* 眼睛
```css
/*下眼皮向上闭合*/
.eye-bottom {
    transition: margin 1s linear;
}
.face:hover .eye-bottom {
    margin-top: 30px;
}

/*眼球变扁*/
.eye-core {
    transition: transform 1s linear;
}
.face:hover .eye-core {
    transform: scaleX(0.8);
}
```

* 嘴巴
```css
/*嘴角抬起*/
.mouth {
    transition: border-radius 1s ease;
}
.face:hover .mouth.left {
    border-bottom-left-radius: 40%;
}
.face:hover .mouth.right {
    border-bottom-right-radius: 40%;
}
```

* 脸红
```css
.face-red {
    transition: opacity 1s linear 0.2s; /*延迟0.2s触发过渡，配合下眼皮上合显得更加自然*/
}
.face:hover .face-red {
    opacity: 0.6;
}
```