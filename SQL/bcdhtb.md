### Kiểm tra đã lấy được dữ liệu đến ngày tháng nào
```sql
SELECT Count(ID)
, NgayBanHanh
  FROM [BCDHTBContext].[dbo].[VanBanDen]
  GROUP BY NgayBanHanh
  ORDER BY NgayBanHanh desc
```

### Xóa dữ liệu theo ngày tháng
```sql
DELETE
  FROM [BCDHTBContext].[dbo].[VanBanDen]
    WHERE MONTH(NgayBanHang) = 1 and DAY(NgayBanHanh) = 1
```