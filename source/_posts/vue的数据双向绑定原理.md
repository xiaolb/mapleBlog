---
title: vue的数据双向绑定原理
date: 2018-09-19 10:56:19
tags:
---

细话不多说，直接上代码撸：

```
<!DOCTYPE html>
<html>
<head>
	<title>Vue数据双向绑定原理实现</title>
</head>
<body>
	<input id="entryData" type="text"></input>
	<div id="showData"></div>
	<script type="text/javascript">
		const obj = {
			pwd: 'dhaskj'
		};

		Object.defineProperty(obj, 'name', {
			get() {
				console.log('get data');
			},
			set(val) {
				console.log('set data');
				document.getElementById('entryData').value = val;
				document.getElementById('showData').innerText = val;
			}
		});

		document.getElementById('entryData').addEventListener('keyup', function(ev) {
			obj.name = ev.target.value;
		});
	</script>
</body>
</html>
```
通过Object.defineProperty中的get和set方法实现数据的获取以及更改。