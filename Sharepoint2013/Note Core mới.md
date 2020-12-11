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
- [Input](https://git.tandan.com.vn/tubs/docs/-/blob/master/Sharepoint2013/Javascript/Form/input.md)
- [Text Area](https://git.tandan.com.vn/tubs/docs/-/blob/master/Sharepoint2013/Javascript/Form/textarea.md)
- [Select](https://git.tandan.com.vn/tubs/docs/-/edit/master/Sharepoint2013/Javascript/Form/select.md)
- [Date Picker](https://git.tandan.com.vn/tubs/docs/-/edit/master/Sharepoint2013/Javascript/Form/date-picker.md)
- [Spinner](https://git.tandan.com.vn/tubs/docs/-/blob/master/Sharepoint2013/Javascript/Form/spinner.md)
- [Checkbox](https://git.tandan.com.vn/tubs/docs/-/blob/master/Sharepoint2013/Javascript/Form/checkbox.md)
- [Combobox](https://git.tandan.com.vn/tubs/docs/-/blob/master/Sharepoint2013/Javascript/Form/combobox.md)
- [Đính kèm](https://git.tandan.com.vn/tubs/docs/-/blob/master/Sharepoint2013/Javascript/Form/attached-files.md)
- [Lenged](https://git.tandan.com.vn/tubs/docs/-/edit/master/Sharepoint2013/Javascript/Form/lenged.md)
- [Popup](https://git.tandan.com.vn/tubs/docs/-/edit/master/Sharepoint2013/Javascript/Form/popup.md)
- [Multi select](https://git.tandan.com.vn/tubs/docs/-/edit/master/Sharepoint2013/Javascript/Form/multi-select.md)

