# AngularJS

## v1.2.19 http POST Format

##### 這是在開發 abc-mart 的時候遇到的問題，想要把登入的跟註冊的 data 換成 從 GET 換成 POST，但是一直遇到格式是 Request Payload, 不是 FormData 的問題

- [官方 Doc v1.2.19](https://code.angularjs.org/1.2.19/docs/api/ng/service/$http)

這是準備好的 data：
```javascript
var config = {
	params: {
		type: "email",
		email: self.user.username,
		password: self.user.password
	}
};
```

###### 原本的 code 如下：
這樣丟的時候，他會把 data 變成 query string 接在網址後面，詳細資料可以參考官方對於 [configuration object](https://code.angularjs.org/1.2.19/docs/api/ng/service/$http#usage) 的說明
```javascript
$http.post("./api/memberLogin.php", null, config)
.success(function (data, status, headers, config){
})
.error(function (data, status, headers, config){
});
```

###### 後來的 code 如下：
```javascript
$http({
	method: 'POST',
	url: './api/memberLogin.php',
	data: $.param(config.params),
	headers: {
		'Content-Type': 'application/x-www-form-urlencoded'
	}
})
.success(function (data, status, headers, config){
})
.error(function (data, status, headers, config){
});
```

##### 結論
> 完全是參考 [這篇文章](http://stackoverflow.com/questions/11442632/how-can-i-post-data-as-form-data-instead-of-a-request-payload) 的說明，主要的關鍵是要加上 `headers: {'Content-Type': 'application/x-www-form-urlencoded'}` 且要手動轉換 data 的格式。

> 本來 Angularjs 提供兩個功能可以轉換 `$httpParamSerializerJQLike` 跟 
`$httpParamSerializer` 但是 v1.2.19 太舊了，什麼都不能用。

> 最後只好用 jQuery 的 $.param() 功能去轉換！

