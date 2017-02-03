# 25Lab Template 及其使用說明

Maintain owner：Cathy

25sprout 有一套 [25Lab Template](https://drive.google.com/a/25sprout.com/file/d/0B4FiKSJ3us2LOEdLN05BOEpsYWM/view?usp=sharing)（25sprout Only）。以下將簡單介紹 25Lab Template 的使用時機與架構。


## 使用時機
* 想要在 [25lab](lab.25sprout.com) 分享自己的小工具，或是 demo source code 時。

## 資料假結構說明
* **路徑**：lab.25sprout.com/`YOUR_PROJECT_NAME`/
* **資源**：`index.php` 只要有 include `../source/source_head.php` 就已經有包含很多東西了。
* **另外**：可以自己建立自己需要的 `js`,`images`,`source`,`json` 等Folder.

## 已包含的Framework清單
* **jquery-1.9.0.min.js**
* **jquery-ui-1.10.0.custom.css**
* **jquery-ui-1.10.0.custom.min.js**
* **bootstrap.min.css**
* **bootstrap.min.js**
* **lab google analysitic**
* **author copyright favicon**
* **http://lab.25sprout.com/css/lab_site.css**
* **google fonts**
* **paste code pre tag with prettify**

##我放檔案到 Lab 一定要套用 Template 嗎？

既然你都問了，我就來說明一下有套跟沒有套差異。首先我們先個舉一個例子給你理解一下什麼是有套，什麼是沒有套。

#### [有套的]
* [pureTable](http://lab.25sprout.com/pureTable/)
* [RANDOM NUMBER GENERATOR](http://lab.25sprout.com/nrprnd/)

#### [沒套的]
* [Yen Time](http://lab.25sprout.com/pureTable/)
* [Wish Pool](http://lab.25sprout.com/wishpool/)

你應該可以發現，**[有套的]** 上方都會有一個一致的導覽列。就像你去看 codedrop 網站，他們每個 demo site 左上角或是右上角都會有看起來一樣的東西。

所以當 Lab 大改版的時候，只有有套到 Template 的 Demo site 會被一起改到共用的主視覺。所以大家可以自己決定要不要套版型。

##我做好了一個 Lab 除了開一個資料夾傳上去以外，還有什麼要注意的嗎？

### 安全性檢查
由於 Lab 是一個公開的網域，所以在你公開一個小工具之前，別忘了確認你的網站有沒有可能被透。

### 首頁露出
除此之外，別忘了在首頁露出你的小工具，並加上適當的 filter 條件（你的名字、語言的類別）。





