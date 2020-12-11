### Bối cảnh
- Đã thực hiện sẵn 1 project **TD.Example.Data** để lưu thông tin Db, migration
- 2 project cùng gọi đến db như sau:
```csharp
    using (var db = new ExampleDbContext())
    {
        var servers = db.Servers.ToList();
        
    }
```
- Kết quả trả ra
    - Gọi trên web trả ra kết quả đầy đủ
    - Gọi trên console app thực hiện không có lỗi, nhưng kết quả trả ra là `null` (**count** = 0)
### Nguyên nhân
- Trong project **TD.Example.Data** đã có khai báo **connectString** trong **app.config**
- Trong web đã khai báo **connectString** trong **web.config**
- Trong console app chưa khai báo **connecString** này -> Đây là nguyên nhân, vì khi reference đến **TD.Example.Data**, nó không hề nhận được **config** từ project này
### Giải pháp
- Thêm **connectString** vào project console app