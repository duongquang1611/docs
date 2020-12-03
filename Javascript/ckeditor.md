# CKeditor
## Giới thiệu
- Phiên bản sịn hơn của **TextArea**
- Cho phép chỉnh sửa văn bản gần như **Word**
- Cho phép lưu và hiển thị văn bản theo định dạng **HTML**
## Hướng dẫn sử dụng
### Khởi tạo
```html
<div id="editor"> 
  <p>Here goes the initial content of the editor.</p> 
</div>
```
```javascript
let editor;
tdcore.cke5.ClassicEditor
    .create( document.querySelector( '#editor' ) )
    .then( newEditor => {
        editor = newEditor;
    } )
    .catch( error => {
        console.error( error );
    } );
```
### Get data
```javascript
editor.getData();
```
### Set data
```javascript
editor.setData(data...);
```