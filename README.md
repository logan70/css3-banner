# css3-banner

标题及三个导航按钮`Demo1``Demo2``Demo3`的样式设置很简单，这里不再做过多赘述。

## 轮播图的切换按钮

**核心思想**是通过

1. `<label>`标签和`<input>`标签的关联性
2. `<input>`标签有一个`checked`属性，可以通过`:checked`伪类选择器来进行控制

上面这两点来控制轮播图的特效

**轮播图按钮HTML结构**

```html
<input type="radio" name="slide" id="slide-1" class="slide" checked="checked" />
<label for="slide-1">1</label>
<input type="radio" name="slide" id="slide-2" class="slide" />
<label for="slide-2">2</label>
<input type="radio" name="slide" id="slide-3" class="slide" />
<label for="slide-3">3</label>
<input type="radio" name="slide" id="slide-4" class="slide" />
<label for="slide-4" class="no-border">4</label>
```
**`<input>`标签不显示**

```css
input { display: none;}
```

**`<label>`标签样式设置**

```css
label {
    font-size: 24px;
    font-style: italic;
    line-height: 32px;
    position: relative;
    z-index: 999;
    float: left;
    width: 140px;
    height: 30px;
    margin-top: 250px;
    cursor: pointer;
    text-align: center;
    color: #fff;
}
```

**轮播图按钮圆圈通过`:before`伪类来设置**

```css
label:before {
    position: absolute;
    z-index: -1;
    left: 50%;
    width: 34px;
    height: 34px;
    content: '';
    transform: translate(-50%);
    border-radius: 50%;
    background-color: rgba(130, 195, 217, .9);
    box-shadow: 0 0 0 4px rgba(255, 255, 255, .3);
}
```

> 需要注意的是，按钮边框用`box-shadow`来设置，这样不会影响按钮的尺寸和布局

**轮播图区域的渐变分割线通过`:after`伪类来设置**

```css
label:not(:last-of-type):after {
    position: absolute;
    right: 0;
    bottom: -20px;
    width: 1px;
    height: 300px;
    content: '';
    background: -webkit-linear-gradient(transparent, rgba(255, 255, 255, .8));
    background: -moz-linear-gradient(transparent, rgba(255, 255, 255, .8));
    background: -o-linear-gradient(transparent, rgba(255, 255, 255, .8));
    background: linear-gradient(transparent, rgba(255, 255, 255, .8));
    filter:progid:DXImageTransform.Microsoft.gradient(startColorstr="00FFFF",endColorstr="#fffff",GradientType=0);
}
```

> 需要注意的是，只需要三个分割线，所以用`:not()`伪类选择器来排除最后一个

## 轮播图图片区域

**轮播图图片区域HTML结构**

```html
<div class="banner-slide clearfix">
	<div class="banner-slide-slice">
		<span class="img-slice"></span>
		<span class="img-slice"></span>
		<span class="img-slice"></span>
		<span class="img-slice"></span>
	</div>
	<div class="banner-slide-slice">
		<span class="img-slice"></span>
		<span class="img-slice"></span>
		<span class="img-slice"></span>
		<span class="img-slice"></span>
	</div>
	<div class="banner-slide-slice">
		<span class="img-slice"></span>
		<span class="img-slice"></span>
		<span class="img-slice"></span>
		<span class="img-slice"></span>
	</div>
	<div class="banner-slide-slice">
		<span class="img-slice"></span>
		<span class="img-slice"></span>
		<span class="img-slice"></span>
		<span class="img-slice"></span>
	</div>
	<div class="title">
		<h3><span>Serendipity</span><span>What you've been dreaming of</span></h3>
		<h3><span>Adventure</span><span>Where the fun begins</span></h3>
		<h3><span>Nature</span><span>Unforgettable experiences</span></h3>
		<h3><span>Serenity</span><span>When slience touches nature</span></h3>
	</div>
</div>
```

**class为`banner-slide`的div为包裹层**

```css
.banner-slide{
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    overflow: hidden;
}
```

**class为`banner-slide-slice`的div为图片显示层**

每个占宽1/4，左浮动水平显示

```css
.banner-slide-slice{
    width: 25%;
    height: 100%;
    float: left;
    position: relative;
    overflow: hidden;
}
```

**class为`img-slice`的span为图片具体显示**

作用是显示不同图片的不同位置

**通过`:nth-of-type()`来确定显示哪个图片**

```css
.img-slice:first-of-type,
#slide-1:checked ~ .banner-slide{
background: url('https://raw.githubusercontent.com/logan70/css3-banner/master/images/1a.jpg') no-repeat;
}
.img-slice:nth-of-type(2),
#slide-2:checked ~ .banner-slide{
background: url('https://raw.githubusercontent.com/logan70/css3-banner/master/images/2a.jpg') no-repeat;
}
.img-slice:nth-of-type(3),
#slide-3:checked ~ .banner-slide{
background: url('https://raw.githubusercontent.com/logan70/css3-banner/master/images/3a.jpg') no-repeat;
}
.img-slice:last-of-type,
#slide-4:checked ~ .banner-slide{
background: url('https://raw.githubusercontent.com/logan70/css3-banner/master/images/4a.jpg') no-repeat;
}
```

> 这个ID选择器`#slide-*:checked ~ .banner-slide`的作用是给父元素添加背景，防止子元素渲染出错而导致显示不正常

**通过父元素`:nth-of-type()`来确定显示图片哪个位置**

```css
.banner-slide-slice:first-of-type .img-slice{
    background-position: 0 0;
}
.banner-slide-slice:nth-of-type(2) .img-slice{
    background-position: -140px 0;
}
.banner-slide-slice:nth-of-type(3) .img-slice{
    background-position: -280px 0;
}
.banner-slide-slice:last-of-type .img-slice{
    background-position: -420px 0;
}
```

## 轮播图切换效果

**`.img-slice`样式设置**

```css
.img-slice{
    width: 140px;
    height: 300px;
    overflow: hidden;
    position: absolute;
    left: -140px;
    top: 0;
    transition: left 1s;
}
```

**定义其他部分的移出效果动画**

```css
@keyframes slideOut{
	from {
		left: 0;
	}
	to {
		left: 140px;
	}
}
```

**调用动画**

```css
.slide:checked ~ .banner-slide .img-slice{
	-webkit-animation: slideOut 1s;
	animation: slideOut 1s;
}
```

**点击切换按钮时相应图片的移入效果**

注意要清除上面调用的动画

```css
#slide-1:checked ~ .banner-slide .img-slice:first-of-type,
#slide-2:checked ~ .banner-slide .img-slice:nth-of-type(2),
#slide-3:checked ~ .banner-slide .img-slice:nth-of-type(3),
#slide-4:checked ~ .banner-slide .img-slice:last-of-type
{
	z-index: 2;
	left: 0;
	-webkit-animation: none;
	-o-animation: none;
	animation: none;
}
```

## 图片相应介绍文字的效果

设置方法与上述的轮播图切换效果设置方法相似

```css
.title h3{
    position: absolute;
    width: 100%;
    text-align: center;
    color: #fff;
    z-index: 999;
    top: 110px;
    opacity: 0;
    transition: opacity 1s;
}
.title span:first-child{
    display: block;
    font-size: 70px;
    letter-spacing: 3px;
    padding-bottom: 5px;
}
.title span:last-child{
    display: block;
    font-size: 14px;
    padding: 10px;
    background-color: rgba(104, 171, 194, .5);
    font-style: italic;
}
#slide-1:checked ~ .banner-slide .title h3:first-child,
#slide-2:checked ~ .banner-slide .title h3:nth-child(2),
#slide-3:checked ~ .banner-slide .title h3:nth-child(3),
#slide-4:checked ~ .banner-slide .title h3:last-child
{
    opacity: 1;
    animation: none;
}
```