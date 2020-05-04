#### netsh winhttp reset proxy
Vấn đề:
- chưa lưu được file trong repeater
- cho phép public API ra ngoài
- API chỉ lấy đúng các trường cần thiết

## Javascript core mới
#### Open form
```javascript
tdcore.modals
.modal({headerTitle:""}) //tên
.iframe(''):link
.size(w,h)
.okCancel({cls:"",text:""}) //btnCancel()//btnOK(): nút trong form
.show()
.then() //xử lý sau khi đóng

```

#### ShowToast
```javascript
toastr.success("Thực hiện thành công");
toastr.warning("");
toastr.error("");
toastr.info("");
```

#### Reload table
```javascript
var table = $('.td-datatable').DataTable();
table.ajax.reload();
```

#### Hiển thị ngày tháng trong table dạng dd/MM/yyyy
```javascript
{
	title: "Ngày ban hành",
	data: "PublishDate",
	type: "date",
	render: function (data, type, row) {
		if (data && (type === 'display' || type === 'filter')) {
			var d = new Date(data);
			return d.format("dd/MM/yyyy");
		}
		return data;
	},
	searchable: false,
	width:"120px"
}
```

#### Hiển thị ảnh
```javascript
data: "Anh",
               render: function (data, type, row) {
                   if (data && (type === 'display' || type === 'filter')) {
                       data = encodeURI(data);
                       var imgs = data.split("%0A");
                       var str = "";
                       for(var i=0;i<imgs.length;i++){
                           str += "<img src="+imgs[i].split(",")[0]+" style='height:100px;margin:3px;'/>"
                       }
                       return str;
                   }
                   return data;
               },
```

#### Test Add new item
```javascript
var dt = new Object();
dt.HuongDanVienID=1;
dt.Active=dt.Active[0]||false;
new td.qldl.TheHuongDanViens().add(dt)
```

#### Check required
```javascript
dlg.addCmd('OK', function (dlg, prevResult) {
var form = forms.Widget.ofElement($("#form")[0]);
            return form.tryValidate().then(function(){
                    var val = form.getData();
                    if (val) {
                                dlg.data = val;
                            } else {
                                toastr.warning('Chưa nhập giá trị');
                                return false;
                            }
                            return prevResult;
                    });
            return false;
});
```

#### Call ajax
```javascript
$.ajax({
        type: "GET",
        url: '/{API}/{Controller}?{params}',
        success: function (re) {
            // Code here
        }
    }).then(function(){});
```

#### Đổ dữ liệu vào form
```javascript
var data = new Object();
            data.HocSinh = sessionStorage.HocSinh;
            data.NamHoc = sessionStorage.NamHoc;
            data.HocKy = sessionStorage.HocKy;
            tdcore.forms.Widget.waitForWidgetInit(document.getElementById('formFilter'))
                .then(function (form) {
                    form.setData(data);
                })

```

#### Auto chọn select: với những giá trị nào đã từng chọn trước đó
```javascript
$("[name=TruongHoc]").val(currentContext.truongHocID).trigger("change");
```

#### Truyền args
```javascript
// cách 1
modals.modal("Modal title")
    .iframe('url/to/page.html', myArgs);

// cách 2
modals.modal("Modal title")
    .iframe('url/to/page.html')
    .args(myArgs);
Lấy args
tdcore.modals.getDialogArgs()
```

#### Promise.All
```javascript
Promise.all([pr1, pr2]).then(values => {
            toastr.success("Xếp lớp thành công!");
            $('.td-datatable').DataTable().ajax.reload();
        });
```

## TypeScript
#### Data Query
```javascript
HocSinhByLopID() {
        return new DataQuery(
            this.http,
            this.processUrl(urlCombine(this.actionUrl, "HocSinhByLopID")));
    }
```
#### Promise
```javascript
XepLopEdit(entitiesEdit: any): Promise<HttpResponse> {
        return this.http.post(
        	urlCombine(this.actionUrl, "XepLopEdit"), 
            JSON.stringify(entitiesEdit));
}
```

## Form
- ##### Hiden
  ```html
  <input type="hidden" id="ThuocNhom" name="ThuocNhom" value="Xếp hạng">
  ```
- ##### input text
  ```html
  <input type="text" name="Name" class="form-control" placeholder="Phân hệ" data-val="true" data-val-required="Tên là bắt buộc!" >
  required
  <input type="text" name="Account" class="form-control" 
        placeholder="Tài khoản" 
        data-val="true"
        data-val-required="Tài khoản là bắt buộc!">
 <div class="form-control-feedback" data-valmsg-for="Account" data-valmsg-replace="true"></div>
 ```
 
- ##### TextArea
```html
<textarea type="text" tdf-type="textarea"
  		data-maxlength-threshold="10" maxlength="1000" name="Description"
  		data-maxlength-warning-class="m-badge m-badge--primary m-badge--rounded m-badge--wide"
  		data-maxlength-separator=" of "
  		data-maxlength-pre-text="You have "
  		data-maxlength-post-text=" chars remaining."
  		data-maxlength-limit-reached-class="m-badge m-badge--brand m-badge--rounded m-badge--wide"
  		data-autosize="true"
  		class="form-control"
  		placeholder="Mô tả">
</textarea>
```
- ##### Select
```html
<select class="form-control m-select2" name="TrangThaiDiemDanhID" tdf-type="select"                                data-ajax--url="/qlgdapi/danhmucs?nhomid=14" data-item-url="/qlgdapi/DanhMucs/{id}"                                data-item-result-field="result" data-ajax--page-size="20" data-result-field="result"                                data-text-field="TieuDe" data-value-field="ID" data-template-selector="#option-template"                                data-placeholder="Trạng thái điểm danh" data-allow-clear="true">
</select>
     <script id="option-template" type="text/x-handlebars-template">
          <div class='select2-result-option clearfix'>
                <div class='select2-result-option__title' data-value='{{ID}}'>{{TieuDe}}</div>
          </div>
     </script>

```
- ##### DatePicker
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
- ##### Spinner
```html
<label class="col-form-label col-sm-2 col-xl-1">Thứ tự</label>
       <div class="col-sm-4 col-lg-3">
           <input type="text" class="form-control" name="Order" placeholder="Thứ tự"
                    tdf-type="spinner"
                    data-min="0"
                    data-max="1000"
                    data-step="1"
                    data-maxboostedstep="10">
       </div>
```
- ##### Checkbox
```html
    <label class="m-checkbox m-checkbox--state-success">
    	<input type="checkbox" name="Active" value="true" checked>
    	<span></span>
    </label>
```
- ##### Combobox
```html
<select tdf-type="combobox" class="form-control" name="Sex">
           <option value="Nam">Nam</option>
           <option value="Nữ">Nữ</option>
</select>
```
- ##### Đính kèm
```html
<div class="form-group m-form__group">
   <label>Đính kèm văn bản</label>
   <div class="m-dropzone m-dropzone--primary dz-icon-compact"
       tdf-type="dropzone"
       data-field="Documents"
       data-plugins="spdoclibupload"
       data-sp-list-title="Documents"
       data-folder="{UnitCode}/{Year}/{Month}/{Day}/{Random}"
       data-parallel-uploads="1"
       data-add-remove-links="true"
       >
       <div class="m-dropzone__msg dz-message needsclick">
           <h3 class="m-dropzone__msg-title">
               Thả tệp tin hoặc nhấp chuột để tải lên.
           </h3>
           <span class="m-dropzone__msg-desc">
               Văn bản đính kèm.
           </span>
       </div>
   </div>
</div>
```
- ##### Lenged
```html
<div class="m-form__heading">
	<h3 class="m-form__heading-title">
		Thông tin hướng dẫn viên
	</h3>
</div>
```
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
- ##### Multiselect
```html
<select name="LopHoc" class="form-control m-select2" tdf-type="select" data-width="100%"
                        data-placeholder="Chọn lớp học" data-ajax-url="" 
                        data-ajax-page-size=10 data-result-field=result
                        data-value-field="ID" data-text-field="TieuDe" data-val="true"
                        data-item-url="/qlgdapi/LopHocs/{id}" data-item-result-field="result"
                        data-allow-clear="true" multiple>
</select>
```

