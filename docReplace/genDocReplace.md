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



```sh
# 替换步骤 (使用GoLand这款IDE进行替换)
# 1 替换文档中图片链接的.svg+xml后缀为.svg
// 查找匹配如下字符
.svg+xml

// 替换成 tab
.svg

# 2 去掉文档中的标题出现的链接
// 查找匹配如下正则表达式
(#{1,6} )\[([^\n]+)\]\([^\n]+\)
// 替换成
$1$2

# 3 使用 https://github.com/before80/local_tools_with_go/tree/main/replace_all_content_md_file_link_to_hugo_link 上的程序来替换文档中的链接，做到尽量都本地化处理链接。



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





