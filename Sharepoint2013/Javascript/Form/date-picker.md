### DatePicker
```html
<label class="col-form-label col-sm-2">Ngày sinh</label>
<div class="col-sm-4 col-lg-3">
    <input type="hidden" id="Birthday" name="Birthday"
        tdf-type="datepicker"
        data-ui-element="+div>input"
        data-output-format="YYYY-MM-DDTHH:mm:ssZ"
        data-input-format="DD/MM/YYYY"
        data-autoclose="true"
        data-autocomplete="true" />
    <div class="m-input-icon m-input-icon--left">
        <span class="m-input-icon__icon m-input-icon__icon--left">
            <span><i class="la la-calendar-check-o"></i></span>
        </span>
        <input type="text" class="form-control m-input" placeholder="Chọn ngày" />
    </div>
</div>
```