<h1 style="text-align:center"> 三栏式布局小结 </h1>

对于三栏式布局，我能想到的布局技术是：

1. 使用float进行浮动布局
2. 使用display : inline-block进行布局
3. 使用position定位进行布局
4. 使用flex弹性盒子布局

以上4种技术都能够很好的完成三栏式布局，但是每种布局都有一些小的问题，需要寻找一些解决办法。

跟布局有关的html代码我放在这里：

     <body>
     	<div class="container clearfix">
     		<nav> </nav>
     		<section> </section>
     		<aside> </aside>
     </body>

### 浮动布局 ###
css code如下：

    /*布局样式*/

    .clearfix{
    	overflow:auto;
    	_height:1%;
    }
    .container{
		border:1px solid #999999;
		background: #eee;
		border-radius: 10px;
		padding: 20px;
		min-width: 688px;
    }
    nav, section, aside{
		margin-right: 20px;
		border:1px solid #999;
		background: white;
		border-radius: 10px;
		padding:20px;
		float: left;
    }
    nav{
		width: 300px;
    }
    section{
		width: calc(100% - 560px);
    }
    aside{
		width: 220px;
		margin-right: 0;
    }

会出现的一些问题：

1、在书写过程中，如果不对最外层的容器添加清浮动，父元素会出现高度塌陷的现象，并不会随着子元素的高度变化而变化。

原因：设置浮动的子元素脱离了普通流，需要闭合浮动元素，使其父元素表现出正常的高度

解决：闭合浮动（清浮动），清浮动的写法很多，在这里我借鉴了其中的一种，了解更多的清浮动写法请看[http://www.daqianduan.com/3606.html](http://www.daqianduan.com/3606.html)

2、中间栏自适应宽度的问题，适用于浮动定位的解决方法有：

1. 使用css3的计算属性calc：具体实现用calc（100%-左边栏长度-右边栏长度-中间的两段间距）
2. 使用margin负值，将中间栏设置成  margin-left：左边栏长度； margin-right：右边栏长度；然后对左边栏设置margin：-100%；右侧栏设置margin：负的右边栏长度。即可实现自适应布局。code如下：

        nav {
        	width: 300px;
    		margin-left: -100%; 
    	}
   		section {
      		 margin-left: 320px;   /*加上了中间的间距*/
      		 margin-right: 240px 
    	}
   		 aside {
   	 		width: 220px;
   			margin-right: -220px;
    	}
    

3、浏览器在极限状况下，左中右三栏会变成上中下三栏。

解决：给容器加一个最小的min-width，极限状态下从左滑动栏，代替布局的打乱。


### inline-block布局 ###
css code如下：

    .container{
		border:1px solid #999999;
		background: #eee;
		border-radius: 10px;
		padding: 20px;
		font-size: 0;
		letter-spacing: -4px;     /* 用于消除inline-block排列时中间的4px缝隙*/
		min-width: 800px;
	}
	.container *{
		font-size: 1rem;
		letter-spacing: normal;
	}
	nav, section, aside{
		display: inline-block;
		vertical-align: top;    /* 前两行代码是为了使其成为inline-block元素*/
		border:1px solid #999;
		background: white;
		border-radius: 10px;
		margin-right: 20px; 
		padding:20px;
	}
	nav{
		width: 300px;
		height: 310px;
	}
	section{
		height: 600px;
		width: calc(100% - 560px);
	}
	aside{
		width: 220px;
		margin-right: 0;
	}


出现的问题：

1、inline-block默认的是基线对齐，而它的基线跟文本基线一致，内容不同的时候不能水平对齐

解决：用需要用vertical-align显式声明一下top/bottom/middle对齐

2、inline-block元素中间会有4px左右的空隙，这个需要去除

解决：张大神给了全面的总结：[http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/](http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/)

3、中间栏自适应问题，这里只能用css3的calc计算属性解决，使用margin负值得方式会产生问题，因为所有的元素都在普通流中。


### position定位布局 ###
css code 如下：

    /*布局样式*/
	.container{
		border:1px solid #999999;
		background: #eee;
		border-radius: 10px;
		padding: 20px;
		position: relative;
		min-height: 800px;
	}
	nav, section, aside{
		margin-right: 20px;
		border:1px solid #999;
		background: white;
		border-radius: 10px;
		padding:20px;
	}
	nav{
		width: 300px;
		padding:20px;
		position: absolute; /*这里因为有margin值可以不设置left和top，能做到自适应*/
	}
	section{
		margin-left: 320px;
		margin-right: 240px
	}
	aside{
		width: 220px;
		margin-right: 0;
		position: absolute;
		top: 20px;
		right: 20px;
	}

出现的问题：
1、子元素会溢出，而父元素无法随之改变

原理：子元素由于是绝对定位，已经脱离了文档流，且在层级上也高于父元素，在文档流中相当于宽高都是0

解决：唯一想出的解决方法是，在确定知道最高子元素的高度情况下，给父元素设置min-height，使其看起来像包裹了子元素。但是此种情况并不适合最高子元素高度不确定的情况，可以借助js解决问题。
网上的解决方式有：1、将溢出部分隐藏（不符合题意） 2、让子元素相对定位，父元素绝对定位。这种情况只适用于只有一个子元素的情况。 3、网上有人提出[http://wenda.tianya.cn/question/657292a2950d75da ](http://wenda.tianya.cn/question/657292a2950d75da )该网站的信息可以解决，但是我是新手，尚不清楚自己测试是否正确，没有测试成功。

2、中间栏自适应

解决：这里可使用的方法有：calc方法，还有代码里使用的双外边距法。原因是定位的元素都脱离的普通流且位置确定。

3、浏览器在极限状态下，中间栏会消失，左右栏会相交一部分，解决办法同样是对容器设置min-width。

### Flexbox布局 ###
css code如下：

    .container{
		border:1px solid #999999;
		background: #eee;
		border-radius: 10px;
		padding: 20px;
		display: -webkit-flex;
		display: flex;
	}
	nav, section, aside{
		margin-right: 20px;
		border:1px solid #999;
		background: white;
		border-radius: 10px;
		padding:20px;
	}
	nav{
		width: 300px;
		height: 310px;
		-webkit-flex:none;
		flex: none;
	}
	section{
		-webkit-flex:1;
		flex: 1;
		height: 600px;
	}
	aside{
		width: 220px;
		margin-right: 0;
		-webkit-flex:none;
		flex: none;
	}

会遇到的问题：

如果不对左中右栏设置高度，三栏的高度会相同都等着最高元素的宽度。

用4种技术做完了三栏布局后，感觉对一些常用属性有了更深入的了解，position和float的工作原理，margin取负值的使用以及取值是负的百分比和像素的影响都有了一些很深入的理解。目前最喜欢的还是弹性盒子flex布局，简单明了，但是一直以来变化太多，这里有一篇文章做了总结：[https://bocoup.com/weblog/dive-into-flexbox](https://bocoup.com/weblog/dive-into-flexbox)可以看到最新的变动情况。


参考的链接：

[http://www.cnblogs.com/meiwenzx/p/5660329.html](http://www.cnblogs.com/meiwenzx/p/5660329.html) 

[http://blog.csdn.net/weixin_36185028/article/details/52741803](http://blog.csdn.net/weixin_36185028/article/details/52741803)

[http://www.zhangxinxu.com/wordpress/2010/01/absolute%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E7%9A%84%E9%9D%9E%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E7%94%A8%E6%B3%95/](http://www.zhangxinxu.com/wordpress/2010/01/absolute%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E7%9A%84%E9%9D%9E%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E7%94%A8%E6%B3%95/)

[http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/](http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/)