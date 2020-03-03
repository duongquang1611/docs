## Lưu ý khi viết API 

### Content-type 

`application/json`

### Các phương thức

*Ở đây ví dụ với đối tượng `Customer`*

#### `[GET]: customers`


Trả về list đối tượng `Customer`

Kết quả cần phân trang (Có các thuộc tính sau):
- `page`: Trang số mấy
- `pageSize`: Bao nhiêu phần tử 1 trang
- `sortField`: Sắp xếp theo trường nào
- `isAsc`: Sắp xếp tăng hay giảm

Trường hợp `Ok`: Trả về list đối tượng đã được phân trang (`Http Code 200`)

#### `[POST]: customers`

Tạo mới 1 đối tượng `Customer`

Kết quả trả về: `id` của đối tượng vừa được khởi tạo kèm mã (`Http Code 200` or `Http Code 201`)

#### `[GET]: customers/{id}`

Trả về đối tượng `Customer` theo `{id}`

Trường hợp khi đối tượng lấy theo `{id}` là `null`: Trả về `Http Code 404`

Trường hợp `Ok`: Trả về đối tượng (`Http Code 200`)

#### `[PUT]: customers/{id}`

Cập nhật đối tượng `Customer` theo `{id}`

Trường hợp lấy `Customer` theo `{id}` và kết quả trả về `null`: Trả về `Http code 404`

Trường hợp `Ok`: Trả về No Content (`Http Code 204`)

#### `[DELETE]: customers/{id}`

Xóa đối tượng `Customer` theo `{id}`

Trường hợp lấy đối tượng theo `{id}` và đối tượng `null`: Trả về `Http code 404`

Trường hợp `Ok`: Trả về No Content (`Http Code 204`)
