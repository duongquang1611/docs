## Lưu ý khi viết API 

### Content-type 

`application/json`

### Các phương thức

*Ở đây ví dụ với đối tượng `Customer`*

#### `[GET]: customers`


Trả về list đối tượng `Customer`

Nên trả các thông tin cơ bản, đủ dùng

Kết quả cần phân trang (Có các thuộc tính sau):
- `isPaging`: Có phân trang hay không
- `offset`
- `limit`
- `sortField`: Sắp xếp theo trường nào
- `isAsc`: Sắp xếp tăng hay giảm

Ngoài ra, đối tượng trả về cần thêm thuộc tính `count` (Tổng số đối tượng trong List đối tượng thỏa mãn)

Trường hợp `Ok`: Trả về list đối tượng đã được phân trang kèm `count` kèm `Http Code 200`

Ví dụ:

```
    {
        __count: 12,
        results: [...]
    }
```

#### `[POST]: customers`

Tạo mới 1 đối tượng `Customer`

Kết quả trả về: `Http Code 201`

#### `[GET]: customers/{id}`

Trả về đối tượng `Customer` theo `{id}`

Nên trả về thông tin đầy đủ tất cả thuộc tính

Trường hợp khi đối tượng lấy theo `{id}` là `null`: Trả về `Http Code 404`

Trường hợp `Ok`: Trả về đối tượng kèm `Http Code 200`

#### `[PUT]: customers/{id}`

Cập nhật đối tượng `Customer` theo `{id}`

Trường hợp lấy `Customer` theo `{id}` và kết quả trả về `null`: Trả về `Http code 404`

Trường hợp `Ok`: Trả về `Http Code 204` (`No Content`)

#### `[DELETE]: customers/{id}`

Xóa đối tượng `Customer` theo `{id}`

Trường hợp lấy đối tượng theo `{id}` và đối tượng `null`: Trả về `Http code 404`

Trường hợp `Ok`: Trả về `Http Code 204` (`No Content`)
