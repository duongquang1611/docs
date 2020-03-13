## GetAllPaginated

#### Bước 1: Nhìn giao diện tạo ra `ViewModel` tương ứng

Lưu ý: 
- Chỉ tạo ra các thuộc tính **cần thiết** dùng để hiển thị
- Trường hợp muốn lấy thông tin chi tiết hơn từ 1 `id` nào đó: gọi `GetById`

Ví dụ: `NoteBookViewModel` chỉ chứa 3 thuộc tính dùng để hiển thị: `Id`, `Name`, `Note` (). Ở đây tên lớp là `General`

```Csharp
    public class General
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Note { get; set; }
    }
```

#### Bước 2: Vào `Controller` khai báo các tham số dùng để lọc và phân trang

**Nên** có đủ các tham số sau:
- `q`: Từ để tìm kiếm
- `page`: Trang số mấy
- `pageSize`: Số bản ghi tối đa mỗi trang
- `sortField`: Trường dùng để sắp xếp
- `isAsc`: Sắp xếp tăng hay giảm

#### Bước 3: Viết `Specification` tương ứng với các tham số ở trên

- 

#### Bước 4: Viết `Service` cho `Controller`

- Điền các tham số vào phần đầu hàm
- Viết `specification` và lấy dữ liệu tương ứng 
- Map dữ liệu

#### Bước 5: Vào `IService` khai báo `service` kia

- 

#### Bước 6: Vào `Controller` gọi `service` kia

- ez done
