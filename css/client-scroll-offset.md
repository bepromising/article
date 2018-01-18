# offsetWidth

1. 它包含了 `width` + `padding` + `border` + `scrollbar（滚动条）` 。
2.  ***offsetHeight*** 相对亦然 。
  
```
let offsetWidth = element.offsetWidth
```

# offsetLeft

1. 相对于父元素的向左偏移量，是一个整数 。
2. 在正常文档流和 `float`（浮动）： 父级的`padding` + 父级的`border` + 自身的`margin` 。
3. 当子元素 `position:absolute` 且父级 `position:relative` ：`left` + 父级的`border` + 自身的`margin` 。
4. ***offsetRight*** ， ***offsetTop*** 相对亦然 。

  
```
let offsetLeft = element.offsetLeft
```

# clientWidth

1. 包含：`padding` + `width` 。
2. ***clientHeight*** 相对亦然 。

  
```
let clientWidth = element.clientWidth
```

# clientLeft

1. 包含： `border` 。
2. ***clientRight*** 如果有滚动条，就包含 `scrollbar` 的宽度 。

  
```
let clientLeft = element.clientLeft;
```

# scrollWidth

1. 包含: 元素的内容高度 + `padding` 。
2. ***scrollHeight*** 相对亦然 。

