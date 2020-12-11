### Get public token key
#### Tạo
- Chon **Tool** →  **External Tool** →  **Add**
- Tạo tên: Giả sử ở đây là **GetPublicKey**
- Sửa command: `C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\Bin\x64\sn.exe`
- Sửa arguments: `-Tp $(TargetPath)`
- Chọn **OK**
<img src="https://i.ibb.co/f1Lhd62/pk11.png">

#### Lấy
- Chọn **Tool** -> **GetPublicKey**
- Check **User Output Windows**
<img src="https://i.ibb.co/KjQk3td/pk2.png">