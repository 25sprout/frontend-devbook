# 表單相關

### js 修改 checkbox & Radio Button Value

>Checkbox

1. 先取得這個的 `checked` 狀態。
2. 然後反向動作塞回去。
3. 注意！！這個跟 attr(‘checked’) 沒有關係，他是跟這個 checkbox post出去，是有沒有勾起來有關係。所以千萬不要用 attr('checked') 去判斷！

```javascript
var checked = $('input[type="checkbox"]').is(":checked");
$('input[type="checkbox"]').prop("checked", !checked);
```

>RadioButton

1. trigger 他鉤起來，因為同群組的東西，網頁會自動選擇最後勾的那一個。

```javascript
$('input[type="radio"]').prop("checked",true);
```


### input type list
以下是所有的 input type，建立表單時，請正確指定該格式。
1. text
2. password
3. datetime
4. datetime-local
5. date
6. month
7. time
8. week
9. number
10. email
11. url
12. search
13. tel
14. color
