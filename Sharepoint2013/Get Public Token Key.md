### Get public token key
#### Tạo
- Chon **Tool** →  **External Tool** →  **Add**
- Tạo tên: Giả sử ở đây là **GetPublicKey**
- Sửa command: `C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\Bin\x64\sn.exe`
- Sửa arguments: `-Tp $(TargetPath)`
- Chọn **OK**

#### Lấy
- Chọn **Tool** -> **GetPublicKey**
- Check **User Output Windows**