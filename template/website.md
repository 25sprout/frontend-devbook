# Website Template 及其使用說明


## 2016 年末
明年希望可以全部導入 webpack 的開發環境，去除不必要的 php 使用。

* `frontend/pug-starter` (at internal gitlab)
  * Develop by Eddie
  * Build With Webpack & Pug
  * Support Multi Language
* `frontend/react-starter` (at internal gitlab)
  * Develop by Jessy
  * Build With Webpack & React

---

## 2016 年大進展
今年因為開發方式的轉換，所以有兩個新的 template 出現。

* [Web_Starter](https://github.com/25sprout/web-starter)
  * Develop by Kai
  * Build With Webpack & React
* [Web_Starter_Kit](https://github.com/EddieWen-Taiwan/web)
  * Develop by Eddie
  * Build With Webpack & Pug

---

## Before 2016

25sprout 有一套 [Website Template](http://nas25lol.myqnapcloud.com:10088/25sproutTemplate/25Website)（25sprout Only）。以下將簡單介紹 Template 的使用時機與架構。

#### 使用時機
* 製作一個新專案時

#### 資料假結構說明
* **css**：自己寫的 .css / .less 檔案，不含 plugin 的
* **dist** min 的 .css 或 .js 檔案
* **images**：網站的圖片們，可以再分 folder
* **js**：自己寫的 js，不含 plugin 的
* **library**：所有的 plugin，每個 plugin 一個 Folder
* **source**：head navi footer 很多畫面都會用到的檔案
* **package.json**, **gulpfile.js**, **.gitignore**, **README.md** ：開發用檔案，正式上線的時候不需要
* **另外**：apo / api / obj … 等 Folder 是 Backend 的物件。

#### 已包含的Framework清單
* **bootstrap**：[`ver 2.3.2`](http://getbootstrap.com/) 如果你的專案客戶沒有要求 IE8 ，歡迎你使用 bootstrap 3.0，而且聽說 4.0 也要出了喲。
* **datepicker**：包含[`bootstrap-datepicker.js`](http://www.eyecon.ro/bootstrap-datepicker) 跟 [`bootstrap-datetimepicker.js`](http://www.malot.fr/bootstrap-datetimepicker)
* **[font-awesome](https://fortawesome.github.io/Font-Awesome/icons/)**：時尚潮流 `Font-face` 服務，可以少做很多Icon，若你有自己喜愛的 `@font-face` 也可以置換成自己常用的，例如[其他的](http://blog.25sprout.com/2012/11/font-face/)
* **jquery**：`ver 1.9.0`，如果你的專案客戶沒有要求 IE8，就可以用更新的版本。
* **jquery-ui**：需搭配jquery才能使用，目前配備的是 `ver 1.10.0`。
* **jquery-autosize**：[`ver 1.7`](http://www.jacklmoore.com/autosize/)在textarea裡面打字時，框框高度會跟著內容多寡伸展。
* **smooth-anchors**：輕鬆使 `href="#ID"` 滑到該物件的位置。
* **twzipcode**：[`ver 1.6.7`](https://code.essoduke.org/twzipcode/) 台灣縣市、郵遞區號下拉選單。
* **jquery.validate.js**：[`ver 1.14.0`](http://jqueryvalidation.org/) 表單驗證工具，且可以自訂多種規則，還可以自訂錯誤文字訊息，最強還可以接查詢api

#### 要注意唷

除了基本的 META 設定以外，以下幾個項目還請大家多留意。

##### Meta
```html
<!--定義語言-->
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<!--意思是 IE 用該使用者電腦可接受的最新版, Chrome 也用最新的-->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
<!--這個是有用 webmaster 才會需要-->
<meta name="google-site-verification" content="" />

<!--以下是一些基本的 Meta, 就BJ4了-->
<meta name="description" content="務必輸入的網站描述 for Google Search">
<meta name="author" content="25sprout, LLC. 新芽網路有限公司">
<meta name="copyright" content="網站版權聲明">
<meta name="keywords" content="網站關鍵字" />
<meta name="URL" content="網站連結">
<meta name="image" content="網站縮圖" />
<meta property="og:image" content="網站縮圖">

<!--主要的網頁標題-->
<title>25sprout Ultra Web Template｜網站架構模型</title>

<!--瀏覽器icon，也可以用 png 唷，建議做 16px ~ 32px 之間。-->
<!--如果你發現你改了，但是瀏覽器一直沒有更新，不妨換個檔名試試看，這個cache 超級嚴重-->
<link rel="shortcut icon" href="images/favicon.ico" >

```

##### Cache
你可以利用 meta 設定不要有 cache，但是很吃流量，上線前要記得關掉

``` html
<!-- 開發階段不要 cache 上線前關掉--> 	
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="expires" content="0">
```

##### Other Icon
除了 share image 你還可以設計 Add to Home的 Icon.

``` html
<!-- Chrome Add to Homescreen -->
<link rel="shortcut icon" sizes="196x196" href="images/addtohome.png">
<!-- iOS icons -->
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="images/addtohome_ios.png">
```

##### GA Code
雖然 head 裡面有 GA code，但是每個專案都需要有自己的GA，大部份會申請在新芽的google帳號下，但也有的客戶會給我們他們的 Google 帳號請我們幫他們申請

``` html
<!-- Google Analytics ***** 務必更新 GA Account No# *****-->
<script type="text/javascript">
	var _gaq = _gaq || [];
	_gaq.push(['_setAccount', '這裏一定要換掉']);
	_gaq.push(['_trackPageview']);

	(function() {
	var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
	ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
	var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
	})();
</script>
```

##### IE Super
有時候用些先進的東西會導致 IE 跑不出來，這時候你可能會需要這些小幫手。另外你也可以搭配這篇文章：[搞懂這些，你也可以大聲說“我搞定IE8”了](http://blog.25sprout.com/2015/03/fixed_ie8_tech/)，注意！他本身就是長得像註解的樣子，這樣IE才會讀得到，其中`lt`、`lte`是用來標示`小於`、`小於等於`等判斷。想要更了解不妨查查 `IE 條件式註解`

```html
<!-- 這個是讓 IE9 以下的 IE 也可以跑 html5 -->
<!--[if lt IE 9]>
    <script src="//html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->

<!-- IE9 以下的 cross site ajax -->
<!--[if lte IE 9]>
	<script type='text/javascript' src='<?php echo $tempServerHost;?>/s/library/jquery.xdomainrequest.min.js'></script>
<![endif]-->
```





