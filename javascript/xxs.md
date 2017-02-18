# XXS 處理

### 什麼是 XXS

跨網站指令碼（Cross-site scripting，通常簡稱為XSS）是一種網站應用程式的安全漏洞攻擊，是代碼注入的一種。它允許惡意使用者將程式碼注入到網頁上，其他使用者在觀看網頁時就會受到影響。這類攻擊通常包含了HTML以及使用者端腳本語言。
- 參考 wiki：https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC

### 怎麼處理 XXS
XXS 需要前後端一起防禦，用於在所有使用者可以打入資料，進行儲存的介面上。

可以分為儲存前的準備，儲存時的後端處理，跟讀取後的呈現。

#### 儲存前的準備
你的介面上如果有表單，在送出前應將你的系統不允許的標籤拿掉，像是 `<script></script>`, 或是 `<iframe></iframe>`，可以參考下面的 decodeHTMLTag 功能。

```javascript
function decodeHTMLTag(str) {
	var element = document.createElement('div');

	if(str && typeof str === 'string') {
		str = str.replace(/<script[^>]*>([\S\s]*?)<\/script>/gmi, '');
		str = str.replace(/&lt;script[^>]*&gt;([\S\s]*?)&lt;\/script&gt;/gmi, '');
		str = str.replace(/<iframe[^>]*>([\S\s]*?)<\/iframe>/gmi, '');
		str = str.replace(/&lt;iframe[^>]*&gt;([\S\s]*?)&lt;\/iframe&gt;/gmi, '');
	}

	return str;
}

$.each(文字框框,function(i, ele){
	var theVal = $(ele).val();
	theVal = decodeHTMLTag(theVal);
	
	//再組合成要 post 的資料。
});
```

但如果你用的是 ckeditor，你可以透過 ckeditor 本身的 config 檔案去設定 `extraAllowedContent`

- 一般用的： `config.extraAllowedContent = 'span[data-title,rel];img[src,alt,width,height];hr;a[!href,target];h1;h2;h3;h4;h5;h6;p;div;blockquote;strong;table;thead;tbody;tr;th;td;ul;li;ol;*[id];*[rel];*(*);*{*}'`;
- 這個的話他是除了這些允許的，其他的東西都不會允許，所以使用者如果打了 `<script></script>` 我們用 ckeditor get data 的時候會根本沒有那句。


#### 儲存時的處理
通常儲存的時候，後端會進行 `htmlentities` 的取代。但也會依照不同的情況，去做變動：

因為你送給後端的資料可能來自於下面兩種地方：
- 一般的 input/textarea
- 你有使用 ckeditor 的 textarea

因為 ckeditor 的資料如果你已經有做 ckeditor 的處理，他已經會先把 `<` 轉成 `&lt;` 了，所以後端如果再轉一次就會有轉兩次的問題，這邊要特別注意。 


#### 讀取後的呈現
若前兩個步驟，前後端都有一起配合執行了，則接下來你就會需要處理呈現於前台的資料。因為前台拿到的資料會是 `我 &amp; 你`，可是使用者打進去的明明是 `我 & 你`，他們就會抱怨說啊怎麼是亂碼。

這時候你就必須從 API 拿到資料後做 decode 再放到畫面上。目前我使用的是 https://github.com/mathiasbynens/he

而根據你要放的地方也會有不同的處理方式：
1. 放到 input 裡面，像是使用者資料的編輯畫面：
	- 要先 var kkk = he.decode(STING)
	- 然後再 `$(input).val(kkk)`
2. 要用 $(div).text(STRING) 的方式放到 div 裡：
	- 要先 var kkk = he.decode(STING)
	- 然後再 `$(div).text(kkk)`
3. 要用 $(div).html(STRING) 的方式放到 div 裡：
	- 因為瀏覽器會自己轉，所以你什麼都不用做，自然就會是 `我 & 你` 了。


---

### 後端常見的特殊字串或 XXS 處理方式：

#### htmlspecialchars
Convert special characters to HTML entities
- 用在於轉換一些特殊符號像是 &, <, >, ', "

#### htmlentities
Convert all applicable characters to HTML entities
- 比 htmlspecialchars 更強，除了那些符號以外其他的也都會轉。

#### htmlpurifier
一個 PHP 的套件，幫你做所有該做的 XXS 處理。





