---
title: javaScript常用代码块
tags: javascript,级联查询,checkbox,全选,反选
grammar_cjkRuby: true
---

## 级联查询城市

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function change(p) {
		var city = document.getElementById("city");
		var arrcity;
		switch (p.value) {
		case "河南":
			arrcity = [ "郑州", "南阳", "洛阳" ];
			break;
		case "河北":
			arrcity = [ "石家庄", "邢台", "德州" ];
			break;
		case "湖南":
			arrcity = [ "娄底", "衡阳", "长沙" ];
			break;
		case "湖北":
			arrcity = [ "襄阳", "荆门", "武汉" ];
			break;
		}
		city.options.length = 0;
		var para = document.createElement("option");
		para.innerHTML = "请选择城市";
		city.appendChild(para);
		for (var i = 0; i < arrcity.length; i++) {
			var para = document.createElement("option");
			para.innerHTML = arrcity[i];
			city.appendChild(para);
		}
	}

	function showcity(city) {
		alert(city.value);
	}

	var arr = new Array(3);
	arr[0] = new Array("郑州", "开封 ", "南阳", "焦作", "信阳");
	arr[1] = new Array("石家庄", "唐山 ", "邯郸", "雄安", "邢台", "秦皇岛");
	arr[2] = new Array("西安", "咸阳 ", "铜川", "宝鸡");
	
	function selectProvice(a){
		var city = document.getElementById("shi");
		var citys = arr[a];
		if(city == ""){
			return;
		}
		for(var i = 0; i < citys.length; i++){
			var element = document.createElement("option");
			var textNode = document.createTextNode(citys[i]);
			element.appendChild(textNode);
			city.appendChild(element);
		}
	}
	
	function selectCity(a){
		alert(a);
	}
</script>
</head>

<body>
<h2>方式一</h2>
	<select onchange="change(this)" id="provice">
		<option>请选择省份</option>
		<option>河南</option>
		<option>河北</option>
		<option>湖南</option>
		<option>湖北</option>
	</select>

	<select id="city" onchange="showcity(this)">
		<option>请选择城市</option>
	</select>
<br>
<hr>
<h2>方式二</h2>
	<select onchange="selectProvice(this.value)" id="sheng">
		<option value="">请选择省份</option>
		<option value="0">河南省</option>
		<option value="1">河北省</option>
		<option value="2">陕西省</option>
	</select>

	<select onchange="selectCity(this.value)" id="shi">
		<option>请选择城市</option>
	</select>
</body>
</html>
```

## 全选反选

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">

	// 全选
	function selectAll(a) {
		var checkboxs = document.getElementsByName("rowName");
		for (var i = 0; i < checkboxs.length; i++) {
			checkboxs[i].checked = a.checked;
		}
	}
	// 反选
	function backSelect() {
		var checkboxs = document.getElementsByTagName("input");
		for (var i = 0; i < checkboxs.length; i++) {
			checkboxs[i].checked = !checkboxs[i].checked;
		}
	}
	// 设置表格选中当前行变色
	onload = function(){
		var a = document.getElementsByTagName("tr");
		for(var i = 0;i < a.length; i++){
			a[i].onmouseover = function(){
				this.style = "background: red";
			}
			a[i].onmouseout = function(){
				this.style = "background: white";
			}
		}
	}
</script>
</head>
<body>
	<input type="checkbox" onclick="selectAll(this)">全选
	<input type="checkbox" onclick="backSelect()">反选
	<table>
		<tr>
			<td><input type="checkbox" name="rowName"></td>
			<td>1.1</td>
			<td>1.2</td>
			<td>1.3</td>
		</tr>

		<tr>
			<td><input type="checkbox" name="rowName"></td>
			<td>2.1</td>
			<td>2.2</td>
			<td>2.3</td>
		</tr>

		<tr>
			<td><input type="checkbox" name="rowName"></td>
			<td>3.1</td>
			<td>3.2</td>
			<td>3.3</td>
		</tr>
	</table>
</body>
</html>
```
