# 覆寫原生功能

### jq_GET ,Get Parameter form URL by javascript
如果你的網頁不想要用 PHP 取得 $_GET，你可以嘗試使用 javascript 去取得參數。

使用 jq_GET 可以將你的URL裡面的變數，全部整成一個 Object 讓你存取。
```javascript
//範例網址
"http://www.google.com/?id=268&cat=apple&form=window"
//在此網頁得到的 jq_GET則會是
{
    "id":268,
    "cat":"apple",
    "form":"window"
}
//你要使用的時候只要;
jq_GET["id"]
//就會是
268
```
```javascript
var jq_GET = (function(qurl) {
	if (qurl == "") return {};
	var b = {};
	for (var i = 0; i < qurl.length; ++i)
	{
		var p=qurl[i].split('=');
		if (p.length != 2) continue;
		b[p[0]] = decodeURIComponent(p[1].replace(/\+/g, " "));
	}
	return b;
})(window.location.search.substr(1).split('&'));
```

### hasAttr
jquery 有一些很常見的 hasClass 可以用，但是就是少了 hasAttr
```html
<div id="Demo1" class="edit">你好！</div>
```
```javascript
$('.edit').hasAttr('id')
true
$('.edit').hasAttr('info')
false
```
```javascript
$.fn.hasAttr = function(name) {
   return this.attr(name) !== undefined;
}
```