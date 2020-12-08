### Lấy thông tin process wp đang chạy
- Chạy cmd với quyền admin
- Chạy lệnh sau
```cmd
    > cd C:\Windows\System32\inetsrv
    > appcmd list wp
```
- Nhớ **ID** của cổng đang chạy để xuống bước sau
### Attach process để debug
- Trong Visual Studio. Chọn **Debug** -> **Attach To Process**
- Tìm ô **Filter Process** nhập `w3p`
- Chọn đúng **ID** ở bước trước
- Chọn **Attach**
- Debug như bình thường