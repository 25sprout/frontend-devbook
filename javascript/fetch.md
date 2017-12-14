# Fetch

使用 Fetch 搭配 Promise 可以使用 `resolve` 或是 `reject` 來 `繼續下去` 或 `catch`。

```js

function loadFeatureInfo(filename) {
	const url = `/${commonset.lang}/static/${commonset.lang}/feature/${filename}.html`;
	fetch(url)
		.then((res) => {
			if (res.status >= 200 && res.status < 300) {
				return Promise.resolve(res);
			}
			return Promise.reject(res);
		})
		.then((response) => response.text())
		.then((body) => {
			$('.featureinfo-block-container').html(body);
			afterLoadFeatureInfo(filename);
		})
		.catch((res) => {
			svdebug(res);
		});
}

```

## Fetch Cookie

使用 fetch 時預設是不會再發出 request 時自動攜帶 cookie。透過 credentials 的設定，可以讓發出的 request 自帶 cookie。

credentials 有三種情況
- `omit`: (預設值) Never send cookies.
- `include`：local 開發環境
- `same-origin`: production 環境

```js
const getUserStatus = () => (
	fetch(`${YOUR_API}`,
		{
			credentials: process.env.NODE_ENV === 'DEV' ? 'include' : 'same-origin',
		},
	)
		.then((res) => {
			if (res.status >= 200 && res.status < 300) {
				return Promise.resolve(res);
			}
			return Promise.reject(res);
		})
);
```

使用 credentials 時，也會需要後端配合設定 HEADER `Access-Control-Allow-Credentials: true`
- [文件](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)

接著在搭配 proxy 設定 Response 的 HEADER，並且要請後端不要再開發用的主機設定 `Access-Control-Allow-Origin` 統一由前端 proxy 設定。這樣就可以達成在本地端開發跟 server cookie 相關的功能。

```js
proxyServer.on('proxyRes', (proxyRes, req, res) => {
	res.setHeader('Access-Control-Allow-Origin', `http://localhost:8080`);
});
```

另外，並非只有需要傳送 cookie 的 api 需要加上 credentials，只加一隻的話，送其他的 api 時，後端又會重新給一個 SESSION ID，你就會對照不起來是同一個人了。

尤其在有會員 base 的網站，需要特別注意。
