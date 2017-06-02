# CKEditor

### 超連結 Dialog 在 Bootstrap modal 裡面會不能打字
做 SurveyCake 的時候發現 CKEditor 的超連結 Dialog 在 Bootstrap Modal 裡面會沒辦法 focus input 打字。

##### 解法一

看到一個人在 [stackoverflow 上分享](https://stackoverflow.com/a/41344264) 但都沒有人 improve 他，但一試之下居然可以

他說：`TabIndex prevents focus fields. everything else css positions`, 看起來是因為 Bootstrap 的 modal 有 tabindex=-1，所以導致 focus event 無效？

```javascript
$('#modal').removeAttr('tabindex');

$('#modal').focusout(function(){
    $(this).css({'position':'relative'});
});

$('#modal').focusin(function(){
    $(this).css({'position':'fixed'});
});
```

但我有點不想要動 modal position，所以用了解法二

##### 解法二

- https://gist.github.com/Reinmar/b9df3f30a05786511a42#ckeditor-inside-bootstrap-modals
- https://gist.github.com/kaddopur/9996231
