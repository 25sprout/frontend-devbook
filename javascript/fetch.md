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