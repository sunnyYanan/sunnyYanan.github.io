---
layout: post
title:  jquery小技巧
categories: 前端开发
tag: jquery
---

* content
{:toc}

1、禁止右键点击

```
$(document).ready(function(){
	$(document).bind("contextmenu",function(e){
		return false;
	});
})
```

2、检测浏览器

```
$(document).ready(function(){
	//target firefox 2 and above
	if($.browser.mozilla && $.browser.version>='1.8'){

}
//target safari
if($.browser.safari){
	
}
//target chrome
if($.browser.chrome){
	
}
if($.browser.camino){
	
}
if($.browser.opera){
	
}
if($.browser.msie && $.browser.version <= 6) {
	
}
if($.browser.msie && $.browser.version > 6) {
	
}
});
```

3、预加载图片

```
$(document).ready(function(){
	jQuery.preloadImages = function(){
	for(var i =0; i<ARGUMENT.LENGTH; jQuery(?<img {i++)>").attr("src", arguments[i]);
}
}
//how to use
$.preloadImages("image1.jpg");
})
```

4、返回页面顶部

```
$(document).ready(function() { 
$('a[href*=#]').click(function() { 
	if (location.pathname.replace(/^\//,'') == this.pathname.replace(/^\//,'') 
	&& location.hostname == this.hostname) { 
		var $target = $(this.hash); 
		$target = $target.length && $target || $('[name=' + this.hash.slice(1) +']'); 
		if ($target.length) { 
			var targetOffset = $target.offset().top; 
			$('html,body').animate({scrollTop: targetOffset}, 900); 
			return false; 
		}
 	} 
 }); 
 // how to use 
 // place this where you want to scroll to 
 <A name=top></A> 
 // the link 
 <A href="#top">go to top</A> });

```

5、返回顶部按钮

```
// Back to top 
$('a.top').click(function () { 
$(document.body).animate({scrollTop: 0}, 800); 
return false; 
}); 
<!-- Create an anchor tag -->
 <a href="#">Back to top</a>

```

6、jQuery延时加载功能

```
$(document).ready(function() { 
window.setTimeout(function() {
 // do something 
 }, 1000); });

```

7、与其它js类库冲突解决方案

```
$(document).ready(function() { 
var $jq = jQuery.noConflict();
 $jq('#id').show(); 
 });

```


[原文章地址](http://www.imooc.com/article/8329)