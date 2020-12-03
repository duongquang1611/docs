### Lenged
```html
<div class="m-form__heading">
    <h3 class="m-form__heading-title">
    	Thông tin hướng dẫn viên
    </h3>
</div>
```
### Tạo lenged có khung
```html
<fieldset class="scheduler-border">
            <legend class="scheduler-border">Name</legend>  
</fieldset>
```
```css
fieldset.scheduler-border {
    border: 1px groove #ddd !important;
    padding: 0 1.4em 1.4em 1.4em !important;
    margin: 0 0 1.5em 0 !important;
    -webkit-box-shadow: 0px 0px 0px 0px #000;
    box-shadow: 0px 0px 0px 0px #000;
}
legend.scheduler-border {
    width: inherit;
    /* Or auto */
    padding: 0 10px;
    /* To give a bit of padding on the left and right */
    border-bottom: none;
}

```