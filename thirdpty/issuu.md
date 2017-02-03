# Issuu Book Embed
有的網站會需要在網頁上呈現 PDF，且並非只要一個下載按鈕還會需要翻閱。
目前可行有兩種方式：
* 上傳到 Google Drive 並複製網址使用超連結打開。
* 上傳到 Issuu 在嵌入到網頁裡。

由於後者的介面比較漂亮，且幫客戶申請一個帳號之後，客戶也可以在 Issue 管理自己的出版物。不過今年 google pdf reader 也有變時尚了啦！只是仍然是散亂在 Drive 裡面，並不是一個專門管理著作的平台。

### 簡易嵌入：嵌入單一的著作。
```javascript
//到您的著作畫面，按下 share 在按下 embed 就可以得到嵌入碼。
<div data-configid="3883294/10271219" style="width:525px; height:341px;" class="issuuembed"></div><script type="text/javascript" src="//e.issuu.com/embed.js" async="true"></script>
```

### 著作清單切換
這是我在製作客戶網站時遇到的需求：
* 只有一個嵌入器的空間
* 且需要自動同步 issuu 帳號下的所有檔案
* 並且可以點擊切換顯示


首先，你必須先完成 HTML 的部分，以下是參考範例

```html
<!--HTML 的部分-->
<section class="issuu-wrapper">
	<div class="content">
		<div id="issuu-viewer"><!--這邊是主要顯示器-->
		</div>
		<div id="all-issuu-list" class="beauty-scroll">
			<ul class="no-list"><!--這邊是清單-->
			</ul>
		</div>
	</div>
</section>
```

```javascript
function getissuu() {
	var url = '這裡是最麻煩的下面會教';
	$.ajax({
		url: url,
		type: "GET",
		dataType: "json",
		cache: false,
		success: function(data){
			if(data != '' && data !=null ){
				magazinedata = data.rsp._content.result._content;
				
				if(magazinedata != null && magazinedata != ''){
					$.each(magazinedata,function(key,val){
						var thisDoc = val.document;
						$('#all-issuu-list ul').prepend('<li rel="http://issuu.com/'+thisDoc.username+'/docs/'+thisDoc.name+'">'+thisDoc.title+'</li>');
					});
					issuuClick();
					$('#all-issuu-list ul li').eq(0).trigger('click');
				}
			}
		}
	});
}

function issuuClick(){
	$('#all-issuu-list ul li').on('click',function(){
		$('#all-issuu-list ul li').removeClass('active');
		$(this).addClass('active');
		var thisMgz = $(this).attr('rel');

		if(winW < 1010){
			$('#issuu-viewer').html('<div data-url="'+thisMgz+'" style="width: 100%; height: 260px;" class="issuuembed"></div><script type="text\/javascript" src="http:\/\/e.issuu.com\/embed.js" async="true"><\/script>');
		} else {
			$('#issuu-viewer').html('<div data-url="'+thisMgz+'" style="width: 400px; height: 260px;" class="issuuembed"></div><script type="text\/javascript" src="http:\/\/e.issuu.com\/embed.js" async="true"><\/script>');
		}
	});
}
```

接下來必須準備抓取資料的 Function `getissuu()` 以及 點擊切換的功能 `issuuClick()`。
其中最麻煩的是抓取資料的 URL，要有這個 URL 首先你必須先去 issuu 的帳號下，create 一個 api 的 `key` & `secret`。
有了 `key` & `secret` 還不能抓到資料，你必須還要有一個用所有你要查詢的設定組合出來的 signature，
以下將教你如何組合 signature.

```javascript
//以我個人的 issuu 帳號為例：
	API key: {YOUR_APIKEY}
	API secret: {YOUR_APIECRET}

//我們需要查詢的東西是 http://developers.issuu.com/api/issuu.document.list.html
	action = issuu.documents.list
	apiKey = {YOUR_APIKEY}
	access = public
	responseParams = username,name,title,description,publishDate
	format = json

//把 每個 key和value 連在一起，重新按照字母排序，然後把 secret 排到前面
	{YOUR_APIECRET}
	accesspublic
	actionissuu.documents.list
	apiKey{YOUR_APIKEY}
	formatjson
	responseParamsusername,name,title,description,publishDate

//串成一行字
	{YOUR_APIECRET}accesspublicactionissuu.documents.listapiKey{YOUR_APIKEY}formatjsonresponseParamsusername,name,title,description,publishDate

//將上面的內容md5之後就是 signature 的值，至於怎麼 md5 大家在上網查查吧XD
	d8db683cdf485472da9e6d24b3286c2b

//傳送 request
http://api.issuu.com/1_0?action=issuu.documents.list \
    &apiKey={YOUR_APIKEY} \
    &access=public \
    &responseParams=username%2Cname%2Ctitle%2Cdescription%2CpublishDate \
    &format=json \
    &signature=d8db683cdf485472da9e6d24b3286c2b

//Request URL，注意最後這個網址是不需要 secret 的！
http://api.issuu.com/1_0?action=issuu.documents.list&apiKey={YOUR_APIKEY}&access=public&responseParams=username%2Cname%2Ctitle%2Cdescription%2CpublishDate&format=json&signature=d8db683cdf485472da9e6d24b3286c2b
```