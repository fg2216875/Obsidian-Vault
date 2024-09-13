##### 調整bootstrap的modal寬度:

**1.使用自訂的 CSS**
你可以通過覆蓋 Bootstrap 的樣式來調整 modal 的寬度。具體做法是定義一個自訂的 CSS 類並將其應用到 modal 上。
```html
<!-- HTML -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
  <div class="modal-dialog custom-modal-width" role="document">
    <div class="modal-content">
      <!-- Modal content -->
    </div>
  </div>
</div>
```

```css
/* CSS */
.custom-modal-width {
  max-width: 80%; /* 調整這裡的值來改變寬度，例如 80% 或 600px */
}
```

**2.使用 Bootstrap 內建類別**
Bootstrap 4 和 5 提供了內建的類別來調整 modal 的大小。這些類別是 `modal-sm`、`modal-lg` 和 `modal-xl`，分別用於小、中和大的 modal。
```html
<!-- 小尺寸 modal -->
<div class="modal fade" id="smallModal" tabindex="-1" role="dialog" aria-labelledby="smallModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-sm" role="document">
    <div class="modal-content">
      <!-- Modal content -->
    </div>
  </div>
</div>

<!-- 大尺寸 modal -->
<div class="modal fade" id="largeModal" tabindex="-1" role="dialog" aria-labelledby="largeModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-lg" role="document">
    <div class="modal-content">
      <!-- Modal content -->
    </div>
  </div>
</div>

<!-- 超大尺寸 modal -->
<div class="modal fade" id="extraLargeModal" tabindex="-1" role="dialog" aria-labelledby="extraLargeModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-xl" role="document">
    <div class="modal-content">
      <!-- Modal content -->
    </div>
  </div>
</div>
```