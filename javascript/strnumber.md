# 字串與數字

這可能是繼 `日期與時間` 之後，用最頻繁的一些功能。網頁不外乎的就是呈現資訊，呈現資訊，然後再呈現資訊。而資訊就是由`字串與數字`拼湊而成。



---

### javascript nl2br
```javascript
val2 = val2.replace(/\n/g, "<br/>");
```

### 千分位
```javascript
//範例
formatThousand(1500);
1,500
```
```javascript
function formatThousand(num) {
    num = num + "";
    var re = /(-?\d+)(\d{3})/;
    while (re.test(num)) {
        num = num.replace(re, "$1,$2");
    }
    return num;
}
```

### validateEmail 驗證 Email 格式。
通常表單驗證我會使用 jQuery Validate, 但如果你這個網頁就只有一個 Email 要驗證，那裝 Plugin 就太浪費了。

```javascript
//範例 & 結果
validateEmail("http://www.google.com");
false
validateEmail("abcdefg@kk.com");
true
```

```javascript
function validateEmail(email) {
	var re = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
	return re.test(email);
}
```

### validateURL 驗證 URL 格式。
通常表單驗證我會使用 jQuery Validate, 但如果你這個網頁就只有一個 URL 要驗證，那裝 Plugin 就太浪費了。

```javascript
//範例 & 結果
validateURL("http://www.google.com");
true
validateURL("abcdefg@kk.com");
false
```

```javascript
function validateURL(url) {
	var pattern = /^(https?|s?ftp):\/\/(((([a-z]|\d|-|\.|_|~|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])|(%[\da-f]{2})|[!\$&'\(\)\*\+,;=]|:)*@)?(((\d|[1-9]\d|1\d\d|2[0-4]\d|25[0-5])\.(\d|[1-9]\d|1\d\d|2[0-4]\d|25[0-5])\.(\d|[1-9]\d|1\d\d|2[0-4]\d|25[0-5])\.(\d|[1-9]\d|1\d\d|2[0-4]\d|25[0-5]))|((([a-z]|\d|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])|(([a-z]|\d|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])([a-z]|\d|-|\.|_|~|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])*([a-z]|\d|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])))\.)+(([a-z]|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])|(([a-z]|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])([a-z]|\d|-|\.|_|~|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])*([a-z]|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])))\.?)(:\d*)?)(\/((([a-z]|\d|-|\.|_|~|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])|(%[\da-f]{2})|[!\$&'\(\)\*\+,;=]|:|@)+(\/(([a-z]|\d|-|\.|_|~|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])|(%[\da-f]{2})|[!\$&'\(\)\*\+,;=]|:|@)*)*)?)?(\?((([a-z]|\d|-|\.|_|~|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])|(%[\da-f]{2})|[!\$&'\(\)\*\+,;=]|:|@)|[\uE000-\uF8FF]|\/|\?)*)?(#((([a-z]|\d|-|\.|_|~|[\u00A0-\uD7FF\uF900-\uFDCF\uFDF0-\uFFEF])|(%[\da-f]{2})|[!\$&'\(\)\*\+,;=]|:|@)|\/|\?)*)?$/i;
	return pattern.test(url);
}
```

### formatFloat 取到小數第 N 位
```javascript
//範例 & 結果
formatFloat(1478.78923,3)
1478.789
formatFloat(1478.78923,0)
1479
```
```javascript
function formatFloat(num, pos){
	var size = Math.pow(10, pos);
	return Math.round(num * size) / size;
}
```

### padLeft 字串補 0 
通常用在日期或是時間的顯示上面。
```javascript
//範例 & 結果
padLeft(12,2);
12
padLeft(12,3);
"012"
padLeft(5,2);
"05"
```
```javascript
function padLeft(str,lenght){
    while(str.toString().length < lenght){
        str = "0" + str;
	}
    return str;
}
```

### isInt 判斷整數
```javascript
//範例 & 結果
isInt(99.0);;
true
padLeft(99.22);
false
```
```javascript
function isInt(n) {
	return n % 1 === 0;
}
```

### numberToABC
這個用的機會應該比較少，這是我用在產生題號的規則，因為英文字母只有 26 個，所以當題號大於 26 時就必需變成 AA。

```javascript
//範例 & 結果
numberToABC(13,true);
"m"
numberToABC(13,false);
"M"
numberToABC(33,true);
"ag"
numberToABC(33,false);
"AG"
```

```javascript
function numberToABC(number,lower){
	var quotient = number;
	var remainder;
	var finalArr = [];
	var returnString = '';
	
	while(quotient !== 0){
		remainder = quotient % indentArr.length;
		quotient = parseInt(quotient / indentArr.length);
		if(remainder === 0){ quotient--; }
		finalArr.push(remainder);
	}

	$.each(finalArr,function(key,val){
		if(val === 0){
			returnString = indentArr[indentArr.length-1] + returnString;
		} else {
			returnString = indentArr[val-1] + returnString;
		}
	});
	
	if(lower){ returnString = returnString.toLowerCase();}
	return returnString;
}
```

### Random Number in a range
```javascript
//範例 & 結果
getRandomArbitrary(5,99);
69.84792656032369
getRandomInt(5,99);
47
```
```javascript
/** * Returns a random number between min (inclusive) and max (exclusive) */ 
function getRandomArbitrary(min, max) { 
    return Math.random() * (max - min) + min; 
}

/** * Returns a random integer between min (inclusive) and max (inclusive) * Using Math.round() will give you a non-uniform distribution! */ 
function getRandomInt(min, max) { 
    return Math.floor(Math.random() * (max - min + 1)) + min; 
}
```
