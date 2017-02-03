# 日期與時間

### Date.prototype.addHours
用來讓 Date 有 addHour 的功能。
```javascript
Date.prototype.addHours = function(h){
	this.setHours(this.getHours()+h);
	return this;
}
```

### getTimeFromstring
取得 SQL 的 timestamp 但是轉化成 javascript time 數字。
```javascript
//範例
getTimeFromstring('2015-12-15 00:50:12');
//結果
1450111812000
```
```javascript
function getTimeFromstring(time){
	var dpa=time.split(" ");
	var dpd=dpa[0].split("-");
	var dpt=dpa[1].split(":");
	var NEW_time = new Date(dpd[0],(dpd[1]-1),dpd[2],dpt[0],dpt[1],dpt[2]).getTime();
	return NEW_time;
}
```

### taiwanTimeString
取得 SQL 的 timestamp 並且加 8 為台灣時間後呈現。Format 維持不變。
##### **需搭配上方的 `getTimeFromstring` 以及 `Date.prototype.addHours`**

```javascript
//範例
taiwanTimeString('2015-12-15 00:50:12');
//結果
"2015-12-15 08:50:12"
```

```javascript
function taiwanTimeString(dateTimeString){
	if(dateTimeString != "" && dateTimeString != null){
		var d = new Date(getTimeFromstring(dateTimeString));
		d = d.addHours(8);

		cY = d.getFullYear();
		cM = padLeft(d.getMonth()+1,2);
		cD = padLeft(d.getDate(),2);
		cHour = padLeft(d.getHours(),2);
		cMin = padLeft(d.getMinutes(),2);
		cSec = padLeft(d.getSeconds(),2);
		var convertTimeStr = cY+'-'+cM+'-'+cD+' '+cHour+':'+cMin+':'+cSec;
		
		return convertTimeStr;
	} else {
		return dateTimeString;
	}
}

function getTimeFromstring(time){
	var dpa=time.split(" ");
	var dpd=dpa[0].split("-");
	var dpt=dpa[1].split(":");
	var NEW_time = new Date(dpd[0],(dpd[1]-1),dpd[2],dpt[0],dpt[1],dpt[2]).getTime();
	return NEW_time;
}
```

### trimTimeString
取得 SQL 的 timestamp 但是拿掉 `-` 跟 `:`。用來比較兩個時間的大小很方便。
```javascript
//範例
trimTimeString('2015-12-15 00:50:12');
//結果
"20151215005012"

//比大小範例
trimTimeString('2015-12-15 00:50:12') > trimTimeString('2015-12-28 00:50:42')
//結果
false
```

```javascript
function trimTimeString(string){
	if(string){
		var nowD = string.substr(0,4)+string.substr(5,2)+string.substr(8,2)+string.substr(11,2)+string.substr(14,2)+string.substr(17,2);		
		return nowD;
	}
}
```

### getNowTimeStamp
用來取得現在的 timestamp，但可以指定要加減多少小時。如果你的網站到時候會需要 Global 的話可能需要類似這樣功能的 function 去顯示時間。
```javascript
//範例 & 結果
getNowTimeStamp('ADD',8)
"20151215220308"
getNowTimeStamp('MINUS',2)
"20151215120353"
```
```javascript
function getNowTimeStamp(action,hours){
	var d = new Date();

	if(hours != null){
		if(action == 'ADD'){
			d.setHours(d.getHours() + hours );
		} else if (action == 'MINUS'){
			d.setHours(d.getHours() - hours );
		}
	}

	var n = d.toISOString();
	var nowD = n.substr(0,4)+n.substr(5,2)+n.substr(8,2)+n.substr(11,2)+n.substr(14,2)+n.substr(17,2);

	return nowD;
}
```

### getNowTimeString
跟 `getNowTimeStamp` 一樣的功能，但是回傳的型態不一樣。這兩個 function 其實可以整合啦～
```javascript
//範例 & 結果
getNowTimeString('ADD',8)
"2015-12-15 22:05:22"
getNowTimeString('MINUS',2)
"2015-12-15 12:05:12"
```
```javascript
function getNowTimeString(action,hours){
	var d = new Date();

	if(hours != null){
		if(action == 'ADD'){
			d.setHours(d.getHours() + hours );
		} else if (action == 'MINUS'){
			d.setHours(d.getHours() - hours );
		}
	}

	var n = d.toISOString();
	var nowD = n.substr(0,10)+' '+n.substr(11,8);

	return nowD;
}
```