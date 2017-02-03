# jQuery Validation Plugin

如果你的專案有使用 jQuery，又剛好表單驗證套件想要使用 [jQuery Validation Plugin](https://jqueryvalidation.org/)，則你可以參考這篇內容所介紹的小技巧與驗證規則。

## 章節簡介

- [基本的表單](#基本的表單)
- [基本的驗證](#基本的驗證)
- [修改錯誤文字](#修改錯誤文字)
- [進階 Rules](#進階-rules)
- [注意事項](#注意事項)
- [其他好用功能](#其他好用功能)


## 基本的表單

身為一個表單不能少不了的就是 `<form></form>` 的 Tag 包圍的範圍。基本的 form 會需要 action 屬性(POST or GET)，如果有 file upload 才會需要 `enctype="multipart/form-data"` 的屬性。

一個簡單的表單大致上會包含 input, textarea, button… 等 element。有時候為了排版需要你會加上一些 `<label>` 或是 `<div>` ，如同下面的 Html 我把每個 input 都用 `"form-group"` 包起來，個搭配一個說明的 `<label>`，這些並不影響本來 Form 的功能。

舉例來說你的表單可能會長這樣

```javascript
<form method="POST" action="POST的網址" enctype="multipart/form-data" id="myform">
   <div class="form-group">
      <label>Email address</label>
      <input type="email" placeholder="Enter email">
   </div>
   <div class="form-group">
      <label>Password</label>
      <input type="password" placeholder="Password">
   </div>
   <div class="form-group">
		<label>姓名<span>（必須要大於 3 個字，且小於 8 個字，必填）</span></label>
		<input type="text" name="user_name" class="required" minlength="3" maxlength="8" >
	</div>
	<div class="form-group">
		<label>Email<span>（規則：Email 格式，必填）</span></label>
		<input type="email" name="user_mail" class="required" >
	</div>
	<div class="form-group">
		<label>最喜歡的網站<span>（規則：URL格式）</span></label>
		<input type="url" name="website">
	</div>
	<div class="form-group">
		<label>出生年份</label>
		<input type="tel" class="digits" name="user_birthy">
	</div>
	<div class="form-group">
		<label>密碼</label>
		<input type="password" name="user_pwd" id="user-pwd-input" >
	</div>
	<div class="form-group">
		<label>一樣的密碼<span>（規則：要跟上一格寫一樣）</span></label>
		<input type="password" name="user_pwd_again" >
	</div>
	<div class="form-group">
		<label>複雜密碼<span>（規則：6～20個字元的英文字母、數字混合，但不含空白鍵及標點符號。）</span></label>
		<input type="password" class="checkpwdhard" name="pwdhard" >
	</div>
	<div class="form-group">
		<label>有規則的姓名<span>（規則：姓名可為中文（最少兩字）或英文（最少三字），英文請勿使用除了空白、底線、DASH以外的符號）</span></label>
		<input type="text" class="checkname" name="namehard" >
	</div>
	<div class="form-group">
		<label>台灣手機號碼<span>（規則：僅允許09開頭的10碼數字，且會自動將使用者寫的 8869 改成 09）</span></label>
		<input type="tel" class="mobileTaiwan" name="mobileTaiwanInput" >
	</div>
	<div class="form-group">
		<label>一般市話<span>（規則：僅接受數字、#-()等符號）</span></label>
		<input type="tel" class="phoneStyle" name="phoneInput" >
	</div>
	<div class="form-group">
		<label>身分證字號<span>（規則：一個大寫英文字開頭，且接續一個 1 或 2 數字的10個字字串）</span></label>
		<input type="text" class="chkPid" name="user_pid" maxlength="10">
	</div>
	<div class="form-group">
		<label>民國出生年月日<span>（規則：民國0~104年之間）</span></label>
		<input type="tel" class="taiwanBirth" name="user_birth" minlength="6" maxlength="7">
	</div>
   <button type="submit">送出</button>
</form>
```

## 基本的驗證

當你要對 `#myform` 做資料格式驗證時，你可能會用以下的語法：

```javascript
myFormValidator = $("#myform").validate({
	ignore: ".ignore-class",
	submitHandler: function(form) {
	   //驗證成功之後就會進到這邊：
	   //方法一：直接把表單 POST 或 GET 到你的 Action URL
	   //方法二：讀取某些欄位的資料，ajax 給別的 API。
	   //此處測試方法一的寫法如下：
	   form.submit();
	},
	errorPlacement: function(error, element) {
	   //你可以自己決定錯誤訊息要放在什麼地方
	   //預設的是 element.after(error);
	   element.closest('.form-group').append(error);
	},
	invalidHandler: function(form, validator) {		var errors = validator.numberOfInvalids();
		if (!errors) {
			return;
		} else {
			var firstErrorInput = $(validator.errorList[0].element);
		}
	},
	rules: {
		//你可以加上特殊的規則
		//格式為  name:{規則名稱: true}，
		//下方舉例的是 name == password 的 input 需要通過名為 hasUppercase 的規則驗證
		//更簡單的作法是直接把 hasUppercase 的 class 加在該 input 的 html 上。
		password: {
			hasUppercase: true
		},
		user_pwd_again: {
			equalTo : '#user-pwd-input'
		}
	}
});
```

我們針對以下幾個部分來做說明：

##### - myFormValidator

把 validate 做出來的物件存到一個變數，並命名，在某些 function 使用上會需要。

##### - ignore

動態的在某些 input 上加上自己定義要略過的 class 像是 `ignore-class` 就可以略過這個 input 的驗證。

##### - submitHandler

當使用者按下送出按鈕，並且你所有設定的規則都過關的時候，就會進到這裡。

你可以直接把 form.submit() 給 action url 之外，另外也可以用 javascript 取得某些欄位的資料處理後，在透過 ajax / fetch 傳送資料給 api，後者就不需要 form.submit() 這一行。

##### - errorPlacement

這是讓你用來控制某個欄位的格式錯誤時，出現錯誤訊息的地方。我比較常用的做法是：每一組 input + label 都會再用一個 form-group div 包起來，所以就可以把錯誤顯示在離 input 最近的地方。

#### - invalidHandler

這個是每次 validate 驗證的時候只要有錯誤就會進來的地方。你可以在這個地方計算目前有幾個錯誤 `numberOfInvalids()`，顯示在網頁上給使用者提醒。

或是可以抓到所有錯誤的 element `errorList` 去做一些 UI 的變化。

##### - rules

jQuery Validation Plugin 有很多已經內建好的 rules 詳列如下：

- required – Makes the element required.
	- 針對必填的欄位 input 或 textarea，你必須在這些 element 加上 class="required" 
- rangelength – Makes the element require a given value range.
- min – Makes the element require a given minimum.
- max – Makes the element require a given maximum.
- range – Makes the element require a given value range.
- step – Makes the element require a given step.
- minlength – Makes the element require a given minimum length.
- maxlength – Makes the element require a given maximum length.
	- 你可以在 input 或 textarea 上加入 min="" max="" minlength="" maxlength="" 等屬性，控制字串長度或值的大小
- email – Makes the element require a valid email
- url – Makes the element require a valid url
- date – Makes the element require a date.
- dateISO – Makes the element require an ISO date.
- number – Makes the element require a decimal number.
- digits – Makes the element require digits only.
 -  當你在 input 或 textarea 中打字時，validate 會幫你驗證你打的內容是不是符合你設定的 type=""，ex email。
 -  針對純數字的欄位，你可以為他加上 digits 的 class，需為網址的欄位你可以加上 url 的 class
- remote – Requests a resource to check the element for validity.
- equalTo – Requires the element to be the same as another one

除了這些預設的 Error 以外，你也可以定義自己的 rule, 我們將在後面的進階規則做說明。

## 修改錯誤文字

針對上面的預設規則，你可以修改他出現的錯誤訊息。

```javascript
var custom_msg  = {
	required: "此欄位必填.",
	remote: "Please fix this field.",
	email: "請輸入正確的 Email 信箱.",
	url: "請輸入正確的網址.",
	date: "請輸入正確的日期.",
	dateISO: "請輸入正確的 (ISO) 日期格式.",
	number: "本欄位請填入數字.",
	digits: "本欄位請填入數字.",
	creditcard: "請輸入正確的信用卡號.",
	equalTo: "請再次輸入相同的值.",
	maxlength: $.validator.format("至多輸入 {0} 個字."),
	minlength: $.validator.format("至少輸入 {0} 個字."),
	rangelength: $.validator.format("請輸入 {0} 到 {1} 個字."),
	range: $.validator.format("請輸入 {0} 到 {1} 的數字."),
	max: $.validator.format("請輸入小於等於 {0} 的值."),
	min: $.validator.format("請輸入大於等於 {0} 的值.")
};

$.extend($.validator.messages, custom_msg);
```

## 進階 Rules

接下來教大家怎麼定義自己的規則，基本上有兩種辦法一個是 `jQuery.validator.addMethod` 一個是 `jQuery.validator.addClassRules` 兩個用的時機不太一樣。

##### - jQuery.validator.addMethod

先介紹第一種方法自訂規則，須按照下方的 Sample Code 進行修改：

```javascript 
jQuery.validator.addMethod("規則名稱英文", function( value, element ) {
	var str = value;
	var result = false;
	if(驗證通過){
		result = true;
	} else {
		result = false;
	}
	return result;
}, "驗證不通過的錯誤訊息");

// Html 部分
<input type="text" name="欄位的name" />

// validation 的設定
rules: {
	欄位的name: {
		規則名稱英文: true
	}
}

// Html 部分 + Validation 設定可以簡化將規則名稱，直接加到物件 class 上即可。
<input type="text" name="欄位的name" class="規則名稱英文" />

```

透過這個我們可以做出很多種驗證，像是：

- 複雜密碼
	- 規則：6～20個字元的英文字母、數字混合，但不含空白鍵及標點符號。

```javascript
jQuery.validator.addMethod("checkpwdhard", function( value, element ) {
	var str = value;
	var result = false;
 
	if(str.length > 0){
		var patt = /^[a-zA-Z0-9]{6,20}$/;
		var result1 = patt.test(str);
		//先測試是否有英文
		var pattEN = /[a-zA-Z]{1,}/;
		result2 = pattEN.test(str);
		//先測試是否有數字
		var pattDigit = /[0-9]{1,}/;
		result3 = pattDigit.test(str);
 
		if(result1 == true && result2 == true && result3 == true){
			result = true;
		} else{
			result = false;
		}
	} else {
		result = true;
	}
	return result;
}, "密碼為 6～20個字元的英文字母、數字混合，但不含空白鍵及標點符號。");
```
 
- 有規則的姓名
	- 規則：姓名可為中文（最少兩字）或英文（最少三字），英文請勿使用除了空白、底線、DASH以外的符號

```javascript
jQuery.validator.addMethod("checkname", function( value, element ) {
	var str = value;
	var result = false;
 
	if(str.length > 0){
		//先測試是否有中文
		var pattCH = /[\u4e00-\u9fa5]{1,}/;
		result1 = pattCH.test(str);
		//先測試是否有英文
		var pattEN = /[a-zA-Z]{1,}/;
		result2 = pattEN.test(str);
		//檢查先生小姐
		testa = str.search("先生");
		testb = str.search("小姐");
 
		//整段內容只接受 (英文或中文)或空白、dash、underline
		var pattSimbo = /^[\u4e00-\u9fa5a-zA-Z\-_\s]{1,}$/;
		result3 = pattSimbo.test(str);
 
		//整段內容是否有空白、dash、underline
		var pattHasSimbo = /[\-_\s]{1,}/;
		result4 = pattHasSimbo.test(str);
 
		if(result1 && result2){
			// console.log('有中文也有英文');
			result = false;
		} else {
			if(result1){
				// console.log('有中文');
				if(str.length >= 2){
					// console.log('至少兩個字');	
					if(testa == -1 && testb == -1){
						// console.log('沒有先生也沒有小姐');
						if(result3){
							// console.log('符號 合規則');
							if(result4){
								result = false;
							} else {
								result = true;
							}
						} else {
							// console.log('符號不合規則');
							result = false;
						}
					} else {
						// console.log('有先生或小姐');
						result = false;
					}
				} else {	
					result = false;
				}
			} else {
				// console.log('沒有中文');
				result = false;
				if(result2){
					// console.log('有英文');
					if(str.length >= 3){
						if(result3){
							// console.log('符號 合規則');
							result = true;
						} else {
							// console.log('符號不合規則');
							result = false;
						}
					} else {
						result = false;
					}
				} else {
					// console.log('沒有英文');
					result = false;
				}
			}
		}
	} else {
		result = true;
	}
	return result;
}, "姓名可為中文（最少兩字）或英文（最少三字），英文請勿使用除了空白、底線、DASH以外的符號");
```

- 台灣手機號碼
	- 規則：僅允許09開頭的10碼數字，且會自動將使用者寫的 8869 改成 09

```javascript
jQuery.validator.addMethod("mobileTaiwan", function( value, element ) {
	var str = value;
	var result = false;
	if(str.length > 0){
		//是否只有數字;
		var patt_mobile = /^[\d]{1,}$/;
		result = patt_mobile.test(str);
 
		if(result){
			//檢查前兩個字是否為 09
			//檢查前四個字是否為 8869
			var firstTwo = str.substr(0,2);
			var firstFour = str.substr(0,4);
			var afterFour = str.substr(4,str.length-1);
			if(firstFour == '8869'){
				$(element).val('09'+afterFour);
				if(afterFour.length == 8){
					result = true;
				} else {
					result = false;
				}
			} else if(firstTwo == '09'){
				if(str.length == 10){
					result = true;
				} else {
					result = false;
				}
			} else {
				result = false;
			}
		}
	} else {
		result = true;
	}
	return result;
}, "手機號碼不符合格式，僅允許09開頭的10碼數字");
```
 
- 一般市話
	- 規則：僅接受數字、#-()等符號

```javascript
jQuery.validator.addMethod("phoneStyle", function( value, element ) {
	var str = value;
	var result = false;
 
	if(str.length > 0){
		var patt_phone = /^[\d\-\(\)\#]{1,}$/;
		result = patt_phone.test(str);
	} else {
		result = true;
	}
 
	return result;
}, "電話號碼不符合格式，僅接受數字、#-()等符號");
```

- 身分證字號
	- 規則：一個大寫英文字開頭，且接續一個 1 或 2 數字的10個字字串

```javascript
jQuery.validator.addMethod("chkPid", function(value, element) {
	var people_id = value.replace(/\s+/g, "");
	return (
		this.optional(element) ||/^[A-Z]{1}[1-2]{1}[0-9]{8}$/.test(people_id)
	);
}, "身分證字號需為一個大寫英文字開頭，且接續一個 1 或 2 數字的10個字字串");
```

- 民國出生年月日
	- 規則：民國0~104年之間

```javascript
jQuery.validator.addMethod("taiwanBirth", function(value, element) {
	var str = value;
	var result = false;
	if(str.length > 0){
		//是否只有數字;
		var patt_digits = /^[\d]{1,}$/;
		result = patt_digits.test(str);
 
		if(result){
			if(str.length == 7){
				var firstThree = parseInt(str.substr(0,3));
				var middleTwo = parseInt(str.substr(3,2));
				var lastTwo = parseInt(str.substr(5,2));
 
				if(firstThree <= 104){
					var setY = firstThree+1911;
					var setM = middleTwo-1;
					var setD = lastTwo;
					
					var testDate = new Date(setY,setM,setD);
					if(testDate.getMonth() == setM && testDate.getDate() == setD){
						result = true;
					} else {
						result = false;
					}
				} else {
					result = false;
				}
			} if(str.length == 6) {
				var firstTwo = parseInt(str.substr(0,2));
				var middleTwo = parseInt(str.substr(2,2));
				var lastTwo = parseInt(str.substr(4,2));
 
				var setY = firstTwo+1911;
				var setM = middleTwo-1;
				var setD = lastTwo;
				
				var testDate = new Date(setY,setM,setD);
				if(testDate.getMonth() == setM && testDate.getDate() == setD){
					result = true;
				} else {
					result = false;
				}
			}
		}
	} else {
		result = true;
	}
	return result;
}, "生日不符合格式");
```

##### - jQuery.validator.addClassRules

第二種方法 `addClassRules` 是用在你需要跟遠端資料庫串接驗證的時候用的，情境有可能是這些：

1. 註冊的 Email 不能與現有會員重複
2. 兌獎的手機不能已經兌換過

在這些情境的時候就會需要用 Remote 的方式來查詢是否驗證通過，而他的 sample 如下：

```javascript
jQuery.validator.addClassRules("textOnece", {
	minlength: 1,
	remote: {
		url: "查詢的 API",
		type: "GET",
		data: {
			// 送給 API 的 data;
		},
		dataFilter: function(data){
			// 從 API 拿回的 Data
			var theData = JSON.parse(data);
			
			// 覆蓋已經存在的 remote error msg
			$.validator.messages["remote"] = "您的 Email 已經註冊過了";

			if(要提醒 Error){
				return false;
			} else {
			   // pass 
				return true;
			}
		}
	}
});
```

> ###### 為什麼要覆寫 remote error msg?
因為 validation plugin 只有針對 remote 提供一種 Erorr msg，但你同時網站可能會有 3 個 remote 的驗證，如果你不覆蓋，你會出現上一個 remote 驗證的錯誤訊息。


## 注意事項

1. 每個要驗證的欄位一定都要有 `name`，否則你的表單驗證提醒文字會一直出現在第一個 input 附近，不會分散在每個 input。
2. 要送出表單的按鈕一定要有 `type="submit"`，否則你怎麼按那個按鈕都不會 validate 的。
3. submitHandler 裡面的 `form.submit()` 絕對不能寫成 $(form).submit 這樣會一直重複 trigger validate。


## 其他好用功能

#### - valid()

我就是不想要用 Button type="submit" 做送出按鈕啊，我想用 Div 可不可以。

> 可以的，但是你會需要透過 `$(表單).valid()` 驅動驗證。

```javascript
$('#myform-div-button').on('click', function() {
	if( $('#myform').valid() === false ){
		// 沒有通過驗證
		return;
	} else {
		// 通過驗證
	}
}
```

#### - form()

還記得前面有把 Valiadate 做出來的物件 `myFormValidator ` 存起來嗎？使用這個就可以知道是否所有驗證都過關了，會回傳 true / false。

通常會用在表單還沒有要送出，但可能要換到別的 tab 的時候。

```javascript
myFormValidator.form();
```

#### - resetForm()

可以用來重新去掉表單上所有的驗證訊息，像是 Modal 重新打開的時候就會很需要這個。

```javascript
myFormValidator.resetForm();
```

#### - rules('remove / add')

如果你的表單已經有套上 Valiadate 了，你也可以選到某一個 input 來做 rules 的動態變化，當然你也可以 addClass / removeClass 啦。是一樣的效果。

但像是 max 這種不是設定在 class 上的，可能是直接用 rules 塞進去的，就不能透過 Class 變化去調整了。

```javascript
$(某個 input).rules("remove","max");
$(某個 input).rules("add",{
	max: 5
});
```


