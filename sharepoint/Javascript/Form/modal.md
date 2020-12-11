### Open Modal
```javascript
tdcore.modals
    .modal(name) // tên
    .iframe(link) //link
    .size(w,h)
    .okCancel() //btnCancel()//btnOK(): nút trong form
    .show()
    .then()  //xử lý sau khi đóng
```
### Truyển args
```javascript
// cách 1
modals.modal("Modal title")
    .iframe('url/to/page.html', myArgs);

```
```javascript
// cách 2
modals.modal("Modal title")
    .iframe('url/to/page.html')
    .args(myArgs);
```
### Lấy args
```javascript
// Lấy Args
tdcore.modals.getDialogArgs()
```