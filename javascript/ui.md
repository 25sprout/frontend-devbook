# 網頁 Flow 上的小幫手

平常雖然會搭配jquery，但是有一些功能卻很長花時間在寫一樣的 Code，所以決定整理起一些常用的 Function，可以快速解決一些 UI 上的問題。


---

### 判斷 UserAgent
可以用 userAgent 去做一些 UI的變化或是 Bug 的修正。
```javascript
//javascript 用法
navigator.userAgent;
//結果
"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.106 Safari/537.36"
//加上一些判斷
if(navigator.userAgent.indexOf("MSIE 8") != -1 || navigator.userAgent.indexOf("MSIE 6") != -1 || navigator.userAgent.indexOf("MSIE 7") != -1) {
         //如果是IE 8,IE 6,IE 7
} else {
     //如果是其他的
}
```
或是用 PHP 去判斷 User Agent。
```html
<?php
	$useragent=$_SERVER['HTTP_USER_AGENT'];

	$deviceType = 'unknow';
	$osType = 'unknow';
	$browserType = 'unknow';
	$androidVersion = 'unknow';
	$iosVersion = 'unknow';
	$appleWebKitVersion = 'unknow';

	if(preg_match('/(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|mobile.+firefox|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows (ce|phone)|xda|xiino/i',$useragent)||preg_match('/1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1 u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp( i|ip)|hs\-c|ht(c(\-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac( |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc\-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-/i',substr($useragent,0,4))){
		$deviceType = 'mobile';
	} else {
		$deviceType = 'desktop';
	}

	if (strpos($useragent, 'iPad') !== false || strpos($useragent, 'GT-N8000') !== false || strpos($useragent, 'Android') !== false || strpos($useragent, 'iPhone') !== false || strpos($useragent, 'iPad') !== false ) {
		// ipad 很怪的被分為 desktop;
		// samsung galaxy note 10.1 也是;
		$deviceType = 'mobile';
	}

	if ( $deviceType == 'mobile'){
		if(strpos($useragent, 'iPhone') !== false || strpos($useragent, 'iPad') !== false){
			$osType = 'ios';
			//取得是不是 ios 7.0 以上;
			$iosString = '/(\d+)_(\d+)_(\d+) like Mac/';
			$iosMatch = preg_match($iosString, $useragent, $match);
			if( $iosMatch ){
				if($match[1] > 6){
					$iosVersion ='yes';
				} else {
					$iosVersion ='no';
				}
			}

		} else if (strpos($useragent, 'Android')!== false){
			$osType = 'android';
			//取得是不是 Android 4.0 以上;
			$androidString = '/Android\s(\d+)\.(\d+)/';
			$androidMatch = preg_match($androidString, $useragent, $match2);
			if( $androidMatch ){
				if($match2[1] >= 4){
					$androidVersion ='yes';
				} else {
					$androidVersion ='no';
				}
			}
		} else {
			$osType = 'other';
		}

		if(strpos($useragent, 'Chrome') !== false || strpos($useragent, 'CriOS') !== false){
			$browserType = 'Chrome';
		} else if ( strpos($useragent, 'Safari') !== false){
			$browserType = 'Safari';
		} else {
			$browserType = 'other';
		}
	} else if ($deviceType == 'desktop'){
		if(strpos($useragent, 'Mac OS X') !== false ){
			$osType = 'Mac';
		} else if (strpos($useragent, 'Windows') !== false ){
			$osType = 'Windows';
		} else {
			$osType = 'other';
		}

		if(strpos($useragent, 'Chrome') !== false ){
			$browserType = 'Chrome';
		} else {
			$browserType = 'notChrome';
		}

		if(strpos($useragent, 'MSIE') !== false ){
			header('Location: ie-message.php');
		}
	}

	if($browserType == 'Chrome'){
		$webkitString = '/AppleWebKit\/(\d+)/';
		$webkitMatch = preg_match($webkitString, $useragent, $match3);
		if( $webkitMatch ){
			if($match3[1] >= 537){
				$appleWebKitVersion ='yes';
			} else {
				$appleWebKitVersion ='no';
			}
		}
	}
?>

<script type="text/javascript">
	var agent = '<?php echo $useragent ?>';
	var deviceType = '<?php echo $deviceType ?>';
	var osType = '<?php echo $osType ?>';
	var browserType = '<?php echo $browserType ?>';
	var androidVersion = '<?php echo $androidVersion ?>';
	var iosVersion = '<?php echo $iosVersion ?>';
	var appleWebKitVersion = '<?php echo $appleWebKitVersion ?>';
</script>
```
### jquery Index 連不一樣的東西都算進去
```html
<div class="hellopwrap">
    <p class="hellop"></p>
    <p class="hellop"></p>
    <h1>Yo</h1>
    <p class="hellop"></p>
    <p class="hellop"></p>
    <p class="hellop"></p>
    <p class="hellop"></p>
</div>
```
```javascript
$('.hellop').each(function(){
    var thisIndex = $(this).closest('.hellopwrap').find('.hellop').index(this);
});
```


### scrollToEle
網頁很常用的滑到某個 Section 功能，有時候想要動畫，有時候不想要，有時候想要滑到該物件在網上一段距離，不要貼邊。這些繁瑣的事情都可以一次搞定。

注意！因為 IE 跟 Firfox 不支援 `body` scrollTop，所以務必要 html,body都寫。

```javascript
//範例
scrollToEle($("#happyzone"),true);
scrollToEle($("#happyzone"),false);
scrollToEle($("#happyzone"),false,1500);
```

```javascript
function scrollToEle(element,animate,posY){
	if(element.length > 0){
		if(!posY){
			//自己的位置;
			posY = element.offset().top-20;
		} else { //指定的位置;
		}

		if(animate){
			$('html, body').animate({"scrollTop":posY},200);
		} else {
			$('html, body').scrollTop(posY); 
		}
	} else {
		return false;
	}
}
```

### overflowBody
網頁剛進來在 Loading 的時候你可能不想要先看到捲軸，想要 Loading 畫面 FadeOut 之後才出現右方捲軸，此時可以派上用場。

```javascript
//範例
overflowBody(true);
overflowBody(false);
```

```javascript
function overflowBody(status){
	winW = $(window).width();
	winH = $(window).height();
	if(status){
		$('body').css({
			'height':winH,
			'width':winW,
			'overflow':'hidden'
		});
	} else {
		$('body').css({
			'height':'auto',
			'width':'100%',
			'overflow':'auto'
		});
	}
}
```

### loadExtraFile
有一些檔案你可能是按下某個按鈕，或是切換某個選項才要導入某個 CSS 或是 JS 檔，所以不能放在 head 也不能放在某一頁。

```javascript
//範例
loadExtraFile("theme/JP.css", "css",function(){console.log("done");});
loadExtraFile("lang/EN.js", "js",function(){console.log("done");});
```

```javascript
function loadExtraFile(filename, filetype, callback){
	var thisCallback = callback;
	var timeStamp = createTimeStamp();
	filename = filename + '?time='+timeStamp;

	if (filetype == "js"){ //if filename is a external JavaScript file
		var fileref = document.createElement('script');
		fileref.setAttribute("type","text/javascript");
		fileref.setAttribute("src", filename);
	}
	else if (filetype == "css"){ //if filename is an external CSS file
		var fileref = document.createElement("link");
		fileref.setAttribute("rel", "stylesheet");
		fileref.setAttribute("type", "text/css");
		fileref.setAttribute("href", filename);
	}

	if (typeof fileref != "undefined"){
		document.getElementsByTagName("head")[0].appendChild(fileref);

		var load_done = false;
		fileref.onload = fileref.onreadystatechange = function() {
			if ( !load_done && (!this.readyState ||
				this.readyState === "loaded" || this.readyState === "complete") ) {
				load_done = true;
				if(thisCallback && typeof(thisCallback) == "function"){
					thisCallback();
				}
			}
		};
	}
}
```
### svdebug
IE8有 console.log 會掛掉，所以上線後會很難 Debug，此時可以再上線前將所有 `console.log()` 換成自己寫的 log 工具 `ex : svdebug()` 。

```javascript
//打開console
sysDebug = true;
//接下你做的每個動作就有 console 了。
```
```javascript
var sysDebug = false;
function svdebug(string){
	if(sysDebug == true){
		console.debug(string);
	}
}
```

### 離開網頁偵測 setLeavePageJS
這邊其實本來很簡單，但因為 iOS 叫做 `pagehide` 不叫 `beforeunload` 所以只好自己客製化判斷。

```javascript
function setLeavePageJS(){
	var eventName = isIOS ? "pagehide" : "beforeunload";
	$(window).on(eventName, function(e) {
		var e = e || window.event;
		var theStr = "";
		//你也可以 String 都一樣我只是要示範說可以不一樣。
		if(eventName == "beforeunload"){
		    var theStr = "確定要離開網頁嗎？";
		} else if (eventName == "pagehide"){
		    var theStr = "確定要離開 iOS 的網頁嗎？";
		}
		
		if(theStr != ""){ return theStr;}
	});
}
```

### 偵測 iframe post 的 Message
這比較常用在登入的 iframe 之類的地方，例如你利用 iframe 登入，成功後外面的網頁要 reload.

```javascript
//當登入成功後 iframe 裡的 js 呼叫
window.parent.postMessage('login_reload', '*');
```

```javascript
// iframe 外層的 parent 就會接收到 message
window.addEventListener("message", receiveMessage, false);
function receiveMessage(event){
	if (event.origin !== serverhost){
		return;
	}else{
		if(event.data=='login_reload'){
			window.top.location.reload();
		}
	}
}
```

### animate height From ? to auto
這個在 Q & A 的 Layout 非常常用到。
```javascript
$('.qa-expand').on('click',function(){
    var qa = $(this).closet('.qa-wrap');
    var curHeight = qa.height();
    var autoHeight = qa.css('height', 'auto').height();
    
    qa.height(curHeight).animate({height: autoHeight}, 1000);
});
```

### 每 N 個一樣的東西，包一個 DIV
```javascript
var totalAD = $('.ad');
for(var i = 0; i < totalAD.length; i+=4) {
    totalAD.slice(i, i+4).wrapAll('<div class="ad-row center-1000"></div>');
}
```

### searchInDOM
這個會用在你的網頁有一堆 Grid、Card、List，但是數量不多，你不想透過資料庫查詢，只想要把使用者想找的顯示其他隱藏，可以搭配一個 search input 使用。
```javascript
//範例
$('.search-input').on('keyup',function(){
	qty = $(this).val();
	searchInDOM('.cards-wrapper .cards',qty);
});
```
```javascript
function searchInDOM(element,findSTR){
	var impactContent = $(element);
	var findSTRArr = findSTR.split(" ");

	if(impactContent.length > 0){
		impactContent.each(function(){
			var string = $(this).text().toLowerCase().replace(/([\n|\t])/g, '');
			var isSearched = true;
			$.each(findSTRArr,function(kk,vv){
				var findSTRTEXT = vv.toLowerCase();
				if(string.search(findSTRTEXT) != -1){
				} else {
					isSearched = false;
				}
			});

			if(isSearched){
				$(this).addClass('selectedITEM');
			} else {
				$(this).addClass('selectedNONE');
			}
		});
	}
}
```

### Click to Fullscreen
```javascript
$('.fullscreen-btn').on('click',function(){
	var elem = document.getElementById("content");
	$(elem).addClass('fullscreenMode');
	if (elem.requestFullscreen) {
		elem.requestFullscreen();
	} else if (elem.msRequestFullscreen) {
		elem.msRequestFullscreen();
	} else if (elem.mozRequestFullScreen) {
		elem.mozRequestFullScreen();
	} else if (elem.webkitRequestFullscreen) {
		elem.webkitRequestFullscreen();
	}
});

function toggleFullScreen() {
	if ((document.fullScreenElement && document.fullScreenElement !== null) ||   
	(!document.mozFullScreen && !document.webkitIsFullScreen)) {
		if (document.documentElement.requestFullScreen) { 
			document.documentElement.requestFullScreen(); 
		} else if (document.documentElement.mozRequestFullScreen) { 
			document.documentElement.mozRequestFullScreen(); 
		} else if (document.documentElement.webkitRequestFullScreen) { 
			document.documentElement.webkitRequestFullScreen(Element.ALLOW_KEYBOARD_INPUT); 
		} 
	} else { 
		if (document.cancelFullScreen) { 
			document.cancelFullScreen(); 
		} else if (document.mozCancelFullScreen) { 
			document.mozCancelFullScreen(); 
		} else if (document.webkitCancelFullScreen) { 
			document.webkitCancelFullScreen(); 
		} 
	} 
}
```

### 偵測裝置轉向
```javascript
window.onorientationchange = function(){
	var orientation = window.orientation;
	if (orientation === 0){
		// is in Portrait mode.
	}
	else if (orientation === 90){
		// is in Landscape mode. The screen is turned to the left.
	}
	else if (orientation === -90){
		// is in Landscape mode. The screen is turned to the right.
	}
}
```

### Delay to addClass
```javascript
$('.target').delay(1000).queue(function(next){
	$(this).removeClass('red');
	next();
});
```
