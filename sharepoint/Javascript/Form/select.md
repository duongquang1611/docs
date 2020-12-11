### Select
```html
<select class="form-control m-select2" name="TrangThaiDiemDanhID" tdf-type="select" data-ajax--url="/qlgdapi/danhmucs?nhomid=14" data-item-url="/qlgdapi/DanhMucs/{id}"                                data-item-result-field="result" data-ajax--page-size="20" data-result-field="result"                                data-text-field="TieuDe" data-value-field="ID" data-template-selector="#option-template"                                data-placeholder="Trạng thái điểm danh" data-allow-clear="true">
</select>

<script id="option-template" type="text/x-handlebars-template">
    <div class='select2-result-option clearfix'>
        <div class='select2-result-option__title' data-value='{{ID}}'>{{TieuDe}}</div>
    </div>
</script>

```