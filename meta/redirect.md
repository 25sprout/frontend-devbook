# Redirect Bots with htaccess

由於目前 starter 都是靜態網頁，所以內容並無法使用 php 產生變動的 meta。像是單一行程 Title / Description / Cover Image 等等...

所以使用 htaccess 來轉導到另一頁 php 製作出 meta，以下簡單說明怎麼達成。

## .htaccess 撰寫

先加上 RewriteCond 有點像在寫程式時的 `if (condition) { ... }` ，以下判斷式可以協助你解決 facebook, linkedin, twitter, pintrest, google

```
# redirect bots to meta file
RewriteCond %{HTTP_USER_AGENT} (facebookexternalhit/[0-9]|LinkedInBot|Twitterbot|Pinterest|Google.*)
```

接下來寫符合規則的人，要做哪些轉導的動作，這邊要看你的網站架構自己摸索一下 :555:，只能從我修改的分享一些筆記：

1. QSA 代表把前一個網址的 query string 原封不動帶給下一個網址。
2. R=302 是叫瀏覽器網址要改變。
3. L 代表是這個 condition 最後一行。
4. `$1` `$2`：這些是變數，來自於你前面括號的地方，編號的順序就是由左至右。
5. 左方是符合網址，右方是轉導網址，兩個都要以目前這隻 htaccess 放置的位置來寫路徑。
6. 在測試的時候可以把 RewriteCond 先註解掉，測成功之後再加上去，就不用一直去 fb debug。

#### 範例一

把 `/tw/featureinfo?f=FILENAME` -> `/static/meta.php?language=tw&type=featureinfo&f=FILENAME`

```
RewriteRule ^((tw|en)/)?featureinfo https://%{HTTP_HOST}/static/meta.php?language=$2&type=featureinfo [QSA,R=302,L]
```

#### 範例二

把 `/index.php?id=YOURID` -> `/PATH/TO/meta_ext.php?id=YOURID`

```
RewriteRule ^index.php https://%{HTTP_HOST}/PATH/TO/meta_ext.php [QSA,L]
```

#### 範例三

把多個 Type 網址： `/tw/TYPE1/YOURID` -> `http://YOUR_DOMAIN/meta.php?language=$1&type=$2&id=$3`

```
RewriteRule ^(tw|en)/(TYPE1|TYPE2|TYPE3)/id/([0-9]+)$ http://YOUR_DOMAIN/meta.php?language=$1&type=$2&id=$3 [L]
```

## meta php 撰寫

基本上這支檔案要放在 BACKEND 用的環境下，請 BACKEND 撰寫(上面 htaccess 其實也是啦)。

這裡還是提供一個範例如下。撰寫時要非常注意一個重點， `$theURL`，一定要跟你剛剛轉過來的網址一模一樣。否則 BOTS 會以最後一個不一樣的為主要 og 網址，就會失敗。

像是你剛剛是從 `/tw/featureinfo?f=FILENAME` 來，你就要把 `$theURL` 填回去 `/tw/featureinfo?f=FILENAME` 一字不差。

```php
<?php
	$theTitle = '';
	$theDesc = '';
	$theURL = '';
	$theImage = '';

	/* YOUR SQL QUERY and REPLACE these keys */
?>

<!DOCTYPE html>
<html>

<head>
	<meta http-equiv='Content-Type' content='text/html; charset=UTF-8' />
	<meta http-equiv='X-UA-Compatible' content='IE=edge,chrome=1'>

	<meta property='og:type' content='metadata'>
	<link rel='origin' href='<?= $theURL ?>'>

	<title><?= $theTitle ?></title>
	<meta name='description' content='<?= $theDesc ?>'>
	<meta name='url' content='<?= $theURL ?>'>
	<meta name='image' content='<?= $theImage ?>'>

	<meta property='og:title' content='<?= $theTitle ?>'>
	<meta property='og:description' content='<?= $theDesc ?>'>
	<meta property='og:url' content='<?= $theURL ?>'>
	<meta property='og:image' content='<?= $theImage ?>'>
</head>

<body></body>

</html>

```

