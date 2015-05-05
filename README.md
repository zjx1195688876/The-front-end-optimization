# The-front-end-optimization/关于前端优化
###关于DOM性能优化：
1.利用文档碎片代替频繁的重复遍历
>当我们向一个节点插入大量的节点（譬如1000个），如果直接使用appendChild就要遍历1000次，影响页面的性能，我们可以通过使用文档碎片的方式优化性能：先创建一个文档碎片，
先把要加的东西放到文档碎片中，再把文档碎片一次性加到DOM-Tree中

```JavaScript
//创建一个文档碎片
var docFrag = document.createDocumentFragment();

//在碎片上加入1000个div节点
for(var i = 0;i<1000;i++){
document.appendChild(document.createElement('div'));
}

//把docFrag加入到DOM-Tree
document.body.appendChild(docFrag);
```
2.DOM0级事件处理程序的注意点：
>DOM0级事件处理程序一定要把对应的JS代码放在DOM的后面，不能放在head里面，如果要放在head里面，我们要在整个函数外再套一个window.onload = function(){//我们的DOM0级程序}

```JavaScript
<input id="btn1" type="button" value="显示/隐藏" />
<div id="demo" class="demo1"></div>

<script type="text/javascript">
	var btn = document.getElementById('btn1');
	//注意这里用的是hideShow，不是hideShow(),因为这是赋值语句！！
	btn.onclick = hideShow; 
	function hideShow(){
		var demo = document.getElementById('demo');
		console.log(demo.className+demo.style+demo);
		if (demo.style.display == "block"){
			demo.style.display = "none";
		}
		else{
			demo.style.display = "block";
		}
	}
</script>
```