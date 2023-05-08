
# chrome搜索结果没有工具按钮

根据使用工具比如按时间段查询的链接的特点，写个油猴脚本将就用。

例子：

搜索java链接：https://www.google.com/search?q=java&oq=java&aqs=chrome.0.69i59j0i271l3j69i61l3j69i65.200j0j9&sourceid=chrome&ie=UTF-8

按一年时间段搜索：https://www.google.com/search?q=java&oq=java&aqs=chrome.0.69i59j0i271l3j69i61l3j69i65.200j0j9&sourceid=chrome&ie=UTF-8&tbs=qdr:y

多了 &tbs=qdr:y 参数，页面也有些错误，调整下 css 就行了。

```javascript
// ==UserScript==
// @name         google search tools display
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://www.google.com/*
// @icon         data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
// @grant        none
// ==/UserScript==

function addTools(obj, new_url) {
    let a_html = '<a href="'+new_url+'" style="padding: 0px 30px;" >Tools</a>';
    let div = window.document.createElement("div");
    div.className = "Nk2ERd HU5Dkd";
    div.style.cssText = 'vertical-align: bottom;height: 26px;';
    div.innerHTML = a_html;
    obj.appendChild(div);
}

function updateQueryStringParameter(uri, key, value) {
	if(!value) {
		return uri;
	}
	var re = new RegExp("([?&])" + key + "=.*?(&|$)", "i");
	var separator = uri.indexOf('?') !== -1 ? "&" : "?";
	if (uri.match(re)) {
		return uri.replace(re, '$1' + key + "=" + value + '$2');
	}
	else {
		return uri + separator + key + "=" + value;
	}
}

(function() {
    'use strict';
    // Your code here...
    var url = window.location.href;
    let search_url = 'https://www.google.com/search';
    let div_class_name = 'a6ZQye';
    var arr = document.getElementsByClassName(div_class_name);
    // 在列表后加一个a标签，点击后就会显示搜索工具
    if (url.indexOf(search_url) >= 0 && arr.length > 0) {
        let new_url = updateQueryStringParameter(url, 'tbs', 'qdr:y');
        addTools(arr[0], new_url);  
    } else console.log("didn't work...")

    // 显示的工具样式有问题的话就调整下
    let tool_id_name = 'hdtbMenus';
    var tool_obj = document.getElementById(tool_id_name);
    if (tool_obj) tool_obj.style.cssText = 'top: 15px;';

})();
```
