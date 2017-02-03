# Bootstrap

### tooltip data-tile update
改完 attr title 後 在叫一次 `tooltip('fixTitle')`，就可以 update tooltip 的 title
```javascript
$(element).tooltip('hide')
          .attr('data-original-title', newValue)
          .tooltip('fixTitle')
          .tooltip('show');



```