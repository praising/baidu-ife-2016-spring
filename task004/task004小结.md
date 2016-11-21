读完题目，了解到题目中有两个重点：

1. div水平垂直居中的解决方法
1. position定位的详细原理理解

关于position的理解：[http://www.cnblogs.com/meiwenzx/p/5660329.html](http://www.cnblogs.com/meiwenzx/p/5660329.html)
这篇文章可以做补充理解。


自行做完了任务，查看了排名前10的任务代码，对相关问题的解决方式做了以下整理

### 1、半圆的解决方案 ###


1. div直接作出半圆样式的： 
  
        border-bottom-right-radius:50px;
    	border-radius：100% 0 0 0;

2. div设置为全圆，然后定位，再隐藏多余部分
3. 不使用div，使用：after和：before伪类，在元素内容中插入一个半圆的行内元素

        .box::before, .box::after {
	   		content: ''
			display: block;
			position: absolute;
			width: 50px;
			height: 50px;
			background-color: #FC0;
			left:0;
			top:0;
			border-radius: 0 0 100% 0;
    		}
    


### 2、div水平垂直居中的方法 ###
  
绝对定位加transform（原理是translate样式的百分比是相对于自身高度的，而top的百分比是相对于父级元素的高度的）

	position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

绝对定位加margin

    position: absolute;
    top: 50%;
    margin-top: -100px（高度的一半）;
    left: 50%;
    margin-left: -200px（宽度的一半）; 

相对定位加margin
    
    position: relative;
    margin: 0 auto;
    top: 50%;
    margin-top: -100px（高度的一半）;

定位+calc计算属性：

    position: absolute;
    top: calc(50% - 100px);
    left: calc(50% - 200px);

flex居中：

    display: flex;
    justify-content: center;
    align-items: center;

table-cell居中：

要在居中的元素外层添加一个div跟下面的代码，实现垂直居中，然后需要居中的元素上使用margin：0 auto；实现水平居中

    display: table-cell;
    vertical-align: middle;


本来想总结一些所有的水平垂直居中的问题，看了参考资料发现都已经总结的很完善了，为了节省时间，贴上链接：

[https://css-tricks.com/centering-css-complete-guide/](https://css-tricks.com/centering-css-complete-guide/)

[http://howtocenterincss.com/#contentType=text&container.width=400px&container.height=400px&horizontal=center&vertical=middle&browser.IE=6](http://howtocenterincss.com/#contentType=text&container.width=400px&container.height=400px&horizontal=center&vertical=middle&browser.IE=6)
