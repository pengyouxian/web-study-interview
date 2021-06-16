- [div水平垂直居中](#1-div水平垂直居中)

## 1. div水平垂直居中

方法一：利用定位（常用方法,推荐）
```css
.parent{
  position:relative;
}
.child{
  position:absolute;
  top:50%;
  left:50%;
  margin-top:-50px;
  margin-left:-50px;
}
```
方法一的原理就是定位中心点是盒子的左上顶点，所以定位之后我们需要回退盒子一半的距离。

方法二：利用`margin:auto`;
```css
.parent{
  position:relative;
}
.child{
  position:absolute;
  margin:auto;
  top:0;
  left:0;
  right:0;
  bottom:0;
}
```
方法三：利用`display:table-cell`;
```css
.parent{
  display:table-cell;
  vertical-align:middle;
  text-align:center;
}
.child{
  display:inline-block;
}
```
方法四：利用`display：flex`设置垂直水平都居中；
```css
.parent{
  display:flex;
  justify-content:center;
  align-items:center;
}
```
方法五：计算父盒子与子盒子的空间距离(这跟方法一是一个道理)；

计算方法：父盒子高度或者宽度的一半减去子盒子高度或者宽的的一半。
```css
.child{
  margin-top:200px;
  margin-left:200px;
}
```
方法六：利用`transform`
```css
.parent{
  position:relative;
}
.child{
  position:absolute;
  top:50%;
  left:50%;
  transform:translate(-50%,-50%);
}
```
方法七：利用`calc`计算
```css
.parent{
position:relative;
}
.child{
  position:absolute;
  top:calc(200px);//（父元素高-子元素高）÷ 2=200px
  let:calc(200px);//（父元素宽-子元素宽）÷ 2=200px
}
```