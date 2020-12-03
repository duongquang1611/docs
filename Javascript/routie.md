# Routie
## Giới thiệu
Giả sử bạn muốn tùy chỉnh một đường dẫn
- Khi trạng thái bằng **Chưa duyệt** thì xử lý kiểu chưa duyệt. 
- Khi trạng thái là **Đã duyệt** thì xử lý kiểu đã duyệt

Bạn muốn điều chỉnh nó ngay trong **url**
> url#trangthai=**trangthai**

Routie là giải pháp cho vấn đề này
## Hướng dẫn sử dụng nhanh
### Cài đặt
Import file js sau:
- [Phiên bản development](https://raw.githubusercontent.com/jgallen23/routie/master/dist/routie.js)
- [Phiên bản production](https://raw.githubusercontent.com/jgallen23/routie/master/dist/routie.min.js)

Khi sử dụng Core:
- Đã tích hợp
- Sử dụng như bình thường
### Sử dụng
```javascript
routie('trangthai=:trangthai', function(trangthai){
   // Khi url là  url#trangthai=**trangthai**
   // Hàm này sẽ được kích hoạt
   // Xử lý hàm
})
```
