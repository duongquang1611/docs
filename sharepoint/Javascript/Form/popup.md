- ##### Popup
```html
<div class="form-group m-form__group row">
	<label class="col-form-label col-sm-3 col-md-3 col-lg-3">Tên đơn vị</label>
	<div class="col-sm-8 col-md-8 col-lg-8">
		<div class="input-group">
			<input type="text" name="Name" class="form-control" placeholder="Tên đơn vị" data-val="true"/>
			<div class="input-group-append">
				<a href="javascript:;" class="btn btn-brand" btn-popup>
					<i class="la la-external-link"></i>
				</a>
			</div>
		</div>
	</div>
</div>
```
```javascript
// call popup
function popup() {
	modals.modal('Chọn đơn vị')
		.content($('#popup-template').html())
		.size(600, 470)
		.ready(function (mdl) {
			forms.WidgetActivator.parse(mdl.panel.content)
				.then(function () {
				});
		})
		.okCancel()
		.addCmd('OK', function (_result) {
			var items = $('#table_popup').DataTable().rows({ selected: true }).data();
			if (items.length > 1) {
				toastr.error('Bạn chỉ được chọn 1 đơn vị!');
				return false;
			}
			if (items.length == 0) {
				toastr.error('Bạn chưa chọn đơn vị nào!');
				return false;
			}
			//var form = forms.Widget.findWidgets(mdl.panel.content, false, forms.Form)[0];
			_result.data = items[0]
			return _result;
		})
		.show()
		.then(function (e) {
			if (e.result == 'OK') {
				$("input[name='Name']").val(e.data.Name);
				$("input[name='Code']").val(e.data.Code);
			}
		});
}
```