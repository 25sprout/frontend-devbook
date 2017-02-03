# Instagram endpoint API

## 抓該 token 使用者自己的 ig data

##### 1. 先建好一個 instagram app，要記得 redirect url 要填好到你等等要收 token 的網址。
```
依此範例，我是設定到：https://wubaibai.blogspot.tw/
```
##### 2. 用網址取得該使用者的 token，修改下方的 `client_id` 跟 `redirect_uri` 地方，貼到瀏覽器
 - https://api.instagram.com/oauth/authorize/?client_id= **`e7012e22f4b04ec5881b8bb64561930b`** &redirect_uri=**`https://wubaibai.blogspot.tw/`**&response_type=token

##### 3. 網址會自動被導到你的 redirect url ，網址後面會有一串 token 記得複製下來。
 - https://wubaibai.blogspot.tw/ **`TOKEN`**

##### 4. 拿到 Token 後，使用 endpoint api 可以抓自己的或是某個 user 的
 - https://api.instagram.com/v1/users/self/media/recent/?access_token= **`TOKEN`**
