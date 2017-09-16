# 從 Less 到 PostCss

剛好在調整 SurveyCake Theme 時，漸漸把 Less 改成 PostCss，就順便紀錄一下怎麼轉換。

### Color Function

##### lighten v.s tint || lightness

從 lighten 轉換成 tint 百分比要重新調整，但若用 lightness 則數值可以沿用。

LESS

```
color: lighten(@color-primary, 10%);
```

PostCss

```
color: color(var(--primary) tint(20%));
color: color(var(--primary) lightness(+10%));
```

##### darken v.s shade || lightness

從 darken 轉換成 shade 百分比要重新調整，但若用 lightness 則數值可以沿用。

LESS

```
color: darken(@color-primary, 10%);
```

PostCss

```
color: color(var(--primary) shade(20%));
color: color(var(--primary) lightness(-10%));
```

##### fadeout v.s alpha

fadeout 2% 代表 opacity 變成 0.98

LESS

```
color: fadeout(@color-primary, 2%);
```

PostCss

```
color: color(var(--primary) a(-2%));
```
