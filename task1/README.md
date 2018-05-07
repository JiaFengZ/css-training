## 实现一个文字颜色展开的css效果

## 1.1 transition背景知识
transition 是 css3 过渡属性，将元素从一种效果过渡到另一种效果，具有动画的视觉效果。
* `transition` 属性值： <transition-property> <transition-duration> <transition-timing-function> <transition-delay>
* `transition-property`：产生过度效果的属性，例如 `width` `color` 
* `transition-duration`：过渡的持续时间，例如 `0.5s` `1s`
* `transition-timing-function`：过渡的动画类型，可选值 `ease` `linear` `ease-in` `ease-out` `ease-in-out`  
`ease`:平滑过渡  
`linear`:线性过渡  
`ease-in`:由慢到快  
`ease-out`:由快到慢  
`ease-in-out`:由慢到快再到慢  
`cubic-bezier(<number>, <number>, <number>, <number>)`:特定的贝塞尔曲线类型，4个数值需在[0, 1]区间内
* `transition-delay`：延迟过渡的时间，默认是 0s

## 1.2 实现思路
具体要实现两个效果：
* 1 蓝色的下划线从中间向两边逐渐展开
* 2 蓝色文字从中间向两边逐渐展开展开

解决方案：
* 一个标签容器<div>包裹标题文字，设置容器的 after 伪类元素充当蓝色覆盖展开层
```html
<div id="title" class="title" title="前端学院"><span>前端学院</span></div>
```
* after 伪类元素利用 `content: attr(title)` 获取父容器的文字内容
* after 伪类元素设置 width 和 text-indent 过渡效果，点击按钮时添加 active 类，width:0 → 100% text-indent:-50% → 0
```css
.title::after {
  content: attr(title);
  display: inline-block;
  background: #fff;
  width: 0;
  height:100%;
  position: absolute;
  top: 0;
  left: 50%;
  text-align: left;
  text-overflow: clip;
  white-space: nowrap;
  text-indent: -2em; /*非firefox可设置为 -50%*/
  overflow: hidden;
  color: #2a73e2;
  border-bottom: solid 2px #2a73e2;
  transform: translateX(-50%);
  transition: width 0.5s ease-in-out,  
        text-indent 0.5s ease-in-out; /* after伪类元素充当覆盖层，覆盖层宽度和文字缩进组合过渡 实现蓝色文字展开效果 */
}
```
* 监听 transitionend，过度完毕移除 active 类。
```javascript
var title = document.getElementById('title');
var button = document.getElementById('button');
function transition() {
  title.classList.add('active');
  title.addEventListener('transitionend', removeTransition)
}
function removeTransition() {
  title.classList.remove('active');
  title.removeEventListener('transitionend', removeTransition)
}
button.addEventListener('click', transition);   
```
## 1.3 `text-indent`在 firefox浏览器下百分比计算的特异
经过测试，在ie、chrome、opera浏览器下 text-indent 百分比是相对父元素的宽度计算的，如果 `text-indent` 设置初始值 `-50%` 相对 `<div>`父容器宽度的一半，由 `-50%` 过渡到 `0` 并且配合 `width` 的由 `0` 过渡到 `100%` ，达到了文字展开的效果。不幸的是 firefox 浏览器计算 after 伪类元素的 text-indent 值是相对本元素也就是 after 元素计算的，因此文字展开时 after 覆盖层的文字不能够与父容器的文字重合，为了兼容 firefox，text-indent 只能退而求其次，使用 em ，初始负缩进 2个字符宽度，实现文字覆盖的效果，但是灵活性就降低了，文字增减时也要同时修改 text-indent 的初始缩进值，而且如果文字含有英文字母，计算初始缩进值就有点麻烦了，只能使用 px 绝对数值了。