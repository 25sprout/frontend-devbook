# IE8補充
有一些 `低階` 的瀏覽器會不支援一些應該要有的功


---


### Date.prototype.toISOString

```javascript
if( !Date.prototype.toISOString ){
	(function() {
		function pad(getnumber) {
			var r = String(getnumber);
			if ( r.length === 1 ) {
				r = '0' + r;
			}
			return r;
		}
		Date.prototype.toISOString = function() {
			return this.getUTCFullYear()
				+ '-' + pad( parseInt(this.getUTCMonth()) + 1 )
				+ '-' + pad( parseInt(this.getUTCDate()) )
				+ 'T' + pad( parseInt(this.getUTCHours()) )
				+ ':' + pad( parseInt(this.getUTCMinutes()) )
				+ ':' + pad( parseInt(this.getUTCSeconds()) )
				+ '.' + String( (this.getUTCMilliseconds()/1000).toFixed(3) ).slice( 2, 5 )
				+ 'Z';
		};       
	}());
}
```

### Array.prototype.find

```javascript
if (!Array.prototype.find) {
	Array.prototype.find = function(predicate) {
		if (this === null) {
			throw new TypeError('Array.prototype.find called on null or undefined');
		}
		if (typeof predicate !== 'function') {
			throw new TypeError('predicate must be a function');
		}
		var list = Object(this);
		var length = list.length >>> 0;
		var thisArg = arguments[1];
		var value;

		for (var i = 0; i < length; i++) {
			value = list[i];
			if (predicate.call(thisArg, value, i, list)) {
				return value;
			}
		}
		return undefined;
	};
}
```

### Array.prototype.indexOf
```javascript
if (!Array.prototype.indexOf){
	Array.prototype.indexOf = function(elt /*, from*/)
	{
		var len = this.length >>> 0;

		var from = Number(arguments[1]) || 0;
		from = (from < 0)
				 ? Math.ceil(from)
				 : Math.floor(from);
		if (from < 0)
			from += len;

		for (; from < len; from++)
		{
			if (from in this &&
					this[from] === elt)
				return from;
		}
		return -1;
	};
}
```


### Array.prototype.map

```javascript
if (!Array.prototype.map) {
	Array.prototype.map = function(callback, thisArg) {
		var T, A, k;
		if (this == null) {
			throw new TypeError(' this is null or not defined');
		}
		var O = Object(this);
		var len = O.length >>> 0;
		if (typeof callback !== 'function') {
			throw new TypeError(callback + ' is not a function');
		}
		if (arguments.length > 1) {
			T = thisArg;
		}
		A = new Array(len);
		k = 0;
		while (k < len) {
			var kValue, mappedValue;
			if (k in O) {
				kValue = O[k];
				mappedValue = callback.call(T, kValue, k, O);
				A[k] = mappedValue;
			}
			k++;
		}
		return A;
	};
}
```