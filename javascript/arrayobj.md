# 物件與陣列

### Array 可以取 length, Object 就是不行！
你可能會想說應該不需要用到 Object 的 size 吧！基本上是這樣沒錯，但我在 `$.each(某個Object,function(){});` 都會先檢查 Object 的 size，避免 Error 的發生。

```javascript
//範例
var objA = {
"name":"Jason Ray",
"age":30,
"sexual":"MALE"
}
objectSize(objA)
//結果
3
```
```javascript
function objectSize(the_object) {
	var object_size = 0;
	for (key in the_object){
		if (the_object.hasOwnProperty(key)) {
			object_size++;
		}
	}
	return object_size;
}
```

### stringToArr
用來把以逗點隔開的字變成array
```javascript
//範例
stringToArr("12,18,19");
//結果
["12", "18", "19"]
```
```javascript
function stringToArr(theStr){
	var theArr = [];
	if(theStr != "" && theStr != null){
		theArr = theStr.split(',');
	}
	return theArr;
}
```

### rndArr
可以把 array 裡面的 Object 重新排序，且可以固定把 `last == 1` 的擺在最後。
```javascript
//範例
var sample = [
    {"name":"Hello","last":0},
    {"name":"少女","last":0},
    {"name":"美男","last":0},
    {"name":"阿囉哈","last":1}
];
rndArr(sample);
sample;
//結果
[
    {"name":"Hello","last":0},
    {"name":"美男","last":0},
    {"name":"少女","last":0},
    {"name":"阿囉哈","last":1}
]
```
```javascript
function rndArr(Arr){
	Arr.sort(function(a,b){
		if(a['last'] == 1){
			return 1;
		} if(b['last'] == 1){
			return -1;
		} else {
			var rndNum = 0.5 - Math.random();
			return rndNum; 
		}
	});
}
```

### arrAddData
將某值加到 Array 後面，若重複則會移除重複的。
```javascript
//範例 & 結果
var kk = [12,13,19,22];
arrAddData(kk ,12);
[13, 19, 22, 12] //kk

var kk = [12,13,19,22];
arrAddData(kk ,88);
[12, 13, 19, 22, 88] //kk
```
```javascript
function arrAddData(theArr,theData){
	if(theArr != null){
		if(theArr.indexOf(theData) != -1){
			theArr.splice(theArr.indexOf(theData), 1);
		}
		theArr.push(theData);
	}

	return theArr;
}
```

### arrDelData
可以把 array 裡面的某一個刪掉，且不會留下 null 的空位。
```javascript
//範例 & 結果
var kk = [12,13,19,22];
arrDelData(kk ,12);
[13, 19, 22] //kk

var kk = [12,13,19,22];
arrDelData(kk ,88);
[12, 13, 19, 22] //kk
```
```javascript
function arrDelData(theArr,theData){
	if(theArr != null){
		if(theArr.indexOf(theData) != -1){
			theArr.splice(theArr.indexOf(theData), 1);
		}
	}

	return theArr;
}
```

### arrRemoveIndex
可以把 array 裡面的第N個刪掉，且不會留下 null 的空位。
```javascript
//範例
var kk = [12,13,19,22];
arrRemoveIndex(kk ,2);

//結果
[12, 13, 22]
```
```javascript
function arrRemoveIndex(theArr,theIndex){
	return theArr.splice(theIndex, 1);
}
```

### arrInsertArr
將另一個 A Array 插入到 B Array 的第 N 個位置。使用這樣的方式才能達到不是 `[12, 13, ["AA", "BB", "CC", "DD"], 19, 22]`

你可能不太清楚什麼時候會用到這個，我來舉個例子：`A Array` 可能是一個系統的資料，但需要被取代成某個範本，這個 `B範本` 可能是很多個像`A`這樣的 Array 共用的，所以你不會想要資料庫裡有千萬個 B 佔空間。

所以你打算只在 A 裡面存一個讀出來時要取代的 `"SAMPLE"`，所以當你需要把 A 中的 `SAMPLE` 位置換成 `B SAMPLE` 時，就會用到了！
```javascript
//範例
var A = [12,13,"SAMPLE",19,22];
var B = ["AA","BB","CC","DD"];
var findSample = A.indexOf("SAMPLE");
arrRemoveIndex(A,findSample);
arrInsertArr(A,findSample,B);

//結果
[12, 13, "AA", "BB", "CC", "DD", 19, 22]
```
```javascript
function arrInsertArr(theArr,theIndex,insertArr){
	var theIndex = Math.min(theIndex, theArr.length);
	theArr.splice.apply(theArr,[theIndex,0].concat(insertArr));

	return theArr;
}
```
### 找最大值或最小值
```javascript
function findMax(numArray) {
    return Math.max.apply(null, numArray);
}

function findMin(numArray) {
    return Math.min.apply(null, numArray);
}
```

### 排序 Array 裡的 Object 們，依照某個 key 的 Value
參考來源：http://blog.dreasgrech.com/2009/11/custom-sorting-arrays-in-javascript.html
```javascript
//預設資料
var data = [
    {"ID":"64","Name":"David"},
    {"ID":"123","Name":"Cathy"},
    {"ID":"96","Name":"Evan"}
];

//範例 & 結果1
data.sort(byDESC("ID"));
data = [
    {"ID":"123","Name":"Cathy"},
    {"ID":"96","Name":"Evan"}
    {"ID":"64","Name":"David"},
];
//範例 & 結果2
data.sort(byASC("Name"));
data = [
    {"ID":"123","Name":"Cathy"},
    {"ID":"64","Name":"David"},
    {"ID":"96","Name":"Evan"}
];
```

```javascript
var byDESC = function (name, minor) {
	return function (o, p) {
		var a, b;
		if (typeof o === 'object' && typeof p === 'object' && o && p) {
			a = o[name];
			b = p[name];
			if (a === b) {
				return typeof minor === 'function' ? minor(o, p) : o;
			}
			if (typeof a === typeof b) {
				return a > b ? -1 : 1;
			}
			return typeof a > typeof b ? -1 : 1;
		} else {
			throw {
				name: 'Error',
				message: 'Expected an object when sorting by ' + name
			};
		}
	}
};

var byASC = function (name, minor) {
	return function (o, p) {
		var a, b;
		if (typeof o === 'object' && typeof p === 'object' && o && p) {
			a = o[name];
			b = p[name];
			if (a === b) {
				return typeof minor === 'function' ? minor(o, p) : o;
			}
			if (typeof a === typeof b) {
				return a < b ? -1 : 1;
			}
			return typeof a < typeof b ? -1 : 1;
		} else {
			throw {
				name: 'Error',
				message: 'Expected an object when sorting by ' + name
			};
		}
	}
};
```