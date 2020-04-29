## Một số hàm java script
- #### Lấy `sum` của 1 mảng
```javascript
[1, 2, 3, 4].reduce((a, b) => a + b, 0)
```
- #### Xửa lý `args` trên `form` truyền vào
```javascript
var obj = new Object();
obj.MaNhiemVu = "";
// Lấy ra
var args = window.frameElement.dialogArgs;
if (args!='' && args!=null && args !='undefined') {
		$('#MaNhiemVu').val(args.MaNhiemVu);
		$('#TenNhiemVu').val(args.TenNhiemVu);
}
```
- #### Filter gridview
```javascript
caml = '<Eq><FieldRef Name="" /><Value Type="Text"></Value></Eq> ';
var grid = gridview.getInstance();
grid.filter(caml);
```
- #### Custom datepicker
```javascript
function CustomDatepicker(id,format,viewMode,minViewMode){
    if(format) format = "yyyy";
    if(viewMode) viewMode = "years";
    if(minViewMode) minViewMode= "years";
	var dp = $('#'+id);
	$(dp).datepicker('remove');
	$(dp).datepicker({
		language: 'vi',
		format: format,
		viewMode: viewMode, 
		minViewMode: minViewMode
	}).on('changeDate', function(e){
		$(this).datepicker('hide');	
	});
}

```
- #### Convert Json to Object
```javascript
obj = JSON.parse(result);
jQuery.parseJSON(result);
```
- #### CustomCombobox
```javascript
function CustomCombobox(id,className)
{
	$("#"+id+"_combobox").parent().addClass(className);
}
```
- #### CamlAnd
```javascript
function CamlAnd(a,b){
	if(a=="")
		return b;
	else if(b=="")
		return a;
	else
		return "<And>"+a+b+"</And>";
}
```
- #### Call services
```javascript
var re = sysService.ImportXml(currentContext.webUrl,"CapPhepLaoDong",returnvalue);
WcfService.post(urlService, 'TongHopLaoDongNuocNgoai', '{"data":"' + dt + '", "inputXml":"","id":"","mabaocao":""}', false, false, function(e) {
        if (e == '1') {
            ShowToast('Thực hiện thành công!', 'success');
            gridview.getInstance().refresh()
        } else
            ShowToast('Đã có lỗi xảy ra!', 'error')
    })
CallWebService(cctc.svc, 'GetToDaiBieu' ,[$(e).attr('xvalue')] , function(e){ 
});
```
- #### DataTable
```javascript
$('#tableNXB').DataTable({
	    "paging":   true,
	    "ordering": false,
	    "info":     false,
	    "searching":false,
	    "pageLength": 5,
	    "pagingType": "numbers",
	    "lengthChange": false
	});
```
- #### ReplaceAll
```javascript
body = body.replace('{TT}', i + 1).replace(new RegExp('{KyHop}','g'), kyHop[i].KyHop);
```
- #### GenDataSearch
```javascript
function GetFieldSearch(id){
	return $('#'+id+'').val() + " ## " + Vn2EnText($('#'+id+'').val()) + " ## ";
}

function GenDataSearch(){
	var index = GetFieldSearch("Ten");
	index += GetFieldSearch("SoHopDong");
	$('#DataSearch').val(index);
}
```
- #### Check radio button
```javascript
function CheckRadio(id, value)
{
	var rd = $('#'+id).closest('div[ctype="RadioList"]');
	$(rd).find('input[name="'+id+'"]').each(function(){
		$(this).parent().removeClass('checked');
		$(this).prop('checked',false);
	});
	var input = $(rd).find('input[value="'+value+'"]').first();
	$(input).parent().addClass('checked');
	$(input).prop('checked',true).change();
}
```
- #### Open popup by url
```javascript
function ViewDanhSachDonQuery()
{
	var data = GetQueryString("ids").split(',').join('#');
	var params = {
		title: "Danh sách đơn thư",
		url: currentContext.webUrl + "/pages/popup/view-ho-so-tcd.aspx",
		width: 1000,
		height: 600,
		args:data,
		maximized: false                
	};
	ShowPopup(params);
}

$(window).load(function(){
	ExecuteOrDelayUntilScriptLoaded(ViewDanhSachDonQuery, 'sp.js');
});
```
- #### Get item in listitem
```javascript
sysService.GetListItemsJson(
	'/csdlchuyennganh/dvc',
	'BVTV-PhanBonQuyetDinh',
	'<Where><Eq><FieldRef Name="BVTV-PhanBonQuyetDinh "/><Value Type="Text">'+ma+'</Value></Eq></Where>',
	'QuyetDinh',
    100);
```
- #### ImportXml
```javascript
var IDDKT = sysService.ImportXml(currentContext.webUrl,'ListName','<data></data>');
```
- #### Xử lý ckeditor
```javascript
CKEDITOR.replace('cke_ctl00_PlaceHolderMain_editor_TenThanhPhan', {
			toolbar: [ ['Bold','Italic'], ['Subscript', 'Superscript']  ],
			width: '300px',
			height: '80px',
	    });
```
Tham khảo: [https://ckeditor.com/latest/samples/old/toolbar/toolbar.html](https://ckeditor.com/latest/samples/old/toolbar/toolbar.html)
- #### Lấy SiteCollection
```javascript
_spPageContextInfo.siteServerRelativeUrl

// Xem file trực tiếp
if (!IsNullOrEmpty(url)) {
    var arr = url.split("##");
    var content = "";
    for (i = 0; i < arr.length; i++) {
        var name = arr[i].split("/")[arr[i].split("/").length - 1];
        var type = arr[i].split(".")[arr[i].split(".").length - 1];
        if (type == "jpg" || type == "jpeg" || type == "png") {
            content += createPortlet(name, '<img src="' + arr[i] + '"/>');
        } else if (type == "pdf") {
            content += createPortlet(name, '<embed src="https://drive.google.com/viewerng/viewer?embedded=true&url=' + currentContext.siteUrl + arr[i] + '" width="100%" style="height:80vh">');
        } else {
            content += createPortlet(name, '<iframe id="iframe" src="https://view.officeapps.live.com/op/embed.aspx?src=' + currentContext.siteUrl + arr[i] + '" width="100%" style="height:80vh" frameborder="0"></iframe>');
        }

    }
    $("#content").html(content);
}

function createPortlet(name,content){
	return '<div class="portlet light bordered"><div class="portlet-title" style="padding:10px;"><div class="caption"><span id="PlaceHolderName">File: '+name+'</span></div></div><div class="portlet-body">'+content+'</div></div>';
}

```
- #### Call Ashx
```javascript
function callAshx(url){
	var xmlHttpReq= createXMLHttpRequest();
	xmlHttpReq.open("GET", url, false);
	xmlHttpReq.send(null);
	return xmlHttpReq.responseText;
}

function createXMLHttpRequest() {
   try { return new XMLHttpRequest(); } catch(e) {}
   try { return new ActiveXObject("Msxml2.XMLHTTP"); } catch (e) {}
   try { return new ActiveXObject("Microsoft.XMLHTTP"); } catch (e) {}
   alert("XMLHttpRequest not supported");
   return null;
}

```
- #### Làm tròn số
```javascript
function roundNumber(num, scale) {
    if (!("" + num).includes("e")) {
        return +(Math.round(num + "e+" + scale) + "e-" + scale);
    } else {
        var arr = ("" + num).split("e");
        var sig = ""
        if (+arr[1] + scale > 0) {
            sig = "+";
        }
        return +(Math.round(+arr[0] + "e" + sig + (+arr[1] + scale)) + "e-" + scale);
    }
}

```
- #### Cách truyền id và lấy id trong table
```html
<table id="data-table">
    <thead>
        <tr>
            <th>Tên</th>
            <th>Thao tác</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Tu</td>
            <td data-id="1" delete-button><button>Delete</button></td>
        </tr>
        <tr>
            <td>Hieu</td>
            <td data-id="2" delete-button><button>Delete</button></td>
        </tr>
    </tbody>
</table>
```
```javascript
// Từ bảng có id là data-table
// Truy cập vào những phần tử bên trong có thuộc tính delete-button
// Xử lý hàm onclick
$("#data-table").on('click', '[delete-button]', function(){
    // Lấy id từ trường data-id
    var id = $(this).attr("data-id");
    alert(id);
}) 
```