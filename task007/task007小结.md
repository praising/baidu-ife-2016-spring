### 关于原版和改版的说明： ###
1、改版使用了css sprite图，集合了所有的小图标

2、将html标签做了一定的修改，更符合语义化

3、新版select模块做了修改，用div显示了select标签无法修改呈现的样式，并且将select使用opacity：0隐藏，舍弃了使用下面的代码，抹掉select样式的再进行修改的做法，因为以下代码不能消除掉option的样式，为js的编写做了准备

	-webkit-appearance: none;  /*Removes default chrome and safari style*/
	-moz-appearance: none; /* Removes Default Firefox style*/
	background:#fff url(../img/07/sele.png) no-repeat;  /*Adds background-image*/
	background-position: 210px 7px;  /*Position of the background-image*/
	text-indent: 0.01px; /* Removes default arrow from firefox*/

4、尝试了弹性布局，在父元素和子元素宽高不定的情况下，做了元素的水平垂直居中，并尝试了2:1布局和1：1：1的三栏等宽布局

5、使用display：inline-block和vertical-align: middle;	使元素均匀排布，同时解决均匀分列中间的空隙，代替使用浮动布局。

另一些细节上的改进和尝试新思路解决问题的过程，不在一一赘述。