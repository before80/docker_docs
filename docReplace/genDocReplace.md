+++
title = "生成文档前的替换"
date = 2023-06-05T09:37:31+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# 生成文档前的替换

```
{{< ref "">}}

// 在浏览器控制台中执行以下代码
var elements = document.querySelectorAll('p > font > font > font');

elements.forEach(function(element) {        
        var newline = document.createElement('br');
        element.parentNode.insertBefore(newline, element);        
        element.firstChild.nodeValue = '&zeroWidthSpace;' +element.firstChild.nodeValue;
});


```



## 替换掉标题中的链接

```
// 查找匹配如下正则表达式
(#{1,6} )\[([^\n]+)\]\([^\n]+\)

// 替换成
$1$2
```



## 替换掉`&zeroWidthSpace;`

```
// 查找匹配如下字符
&zeroWidthSpace;

// 替换成 tab
​	
```

## 替换掉youtube视频的HTML

```
// 查找匹配如下正则表达式
<iframe src="https:\/\/www\.youtube\.com\/embed\/([\w\-]+)([\w=\?]?)" [^\n]+><\/iframe>

或 

<iframe src="https:\/\/www\.youtube\.com\/embed\/([\w\-]+)\?[\w=]+" [^\n]+><\/iframe>

// 替换成
{{< youtube "$1">}}

```





