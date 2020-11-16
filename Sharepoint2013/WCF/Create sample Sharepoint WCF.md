### Tạo project Sharepoint 2013
Đặt tên project: **TD.SOC.Publish**
### Tạo thư mục liên kết thư mục ISAPI
Chuột phải **Project** -> **Add** -> **Sharepoint Mapped Folder** -> Chọn **ISAPI** --> Chọn **OK**
### Tạo file .svc trong ISAPI
- Trong **Project**, chuột phải thư mục **ISAPI** -> Chọn **Add** -> **New folder**.
- Đặt tên là **TD.SOC.Publish**
- Tạo file **SOCService.svc** như sau:
```
<%@ ServiceHost Language="C#" Debug="true"
    Service="TD.SOC.Publish.SOCService, TD.SOC.Publish, Version=1.0.0.0, Culture=neutral, PublicKeyToken=a58ef5ef51ee00c0"
    CodeBehind="SOCService.svc.cs"
    Factory="Microsoft.SharePoint.Client.Services.MultipleBaseAddressWebServiceHostFactory,
    Microsoft.SharePoint.Client.ServerRuntime, Version=15.0.0.0, Culture=neutral,
    PublicKeyToken=71e9bce111e9429c" %>
```
- Để lấy PublicToken chính xác. Chạy **Tools** -> **Get PublicKey**
### Tạo file interface khai báo các api và file thực thi kế thừa interface đó
- Tạo thư mục Interfaces
- Trong thư mục Interfaces tạo file **ISOCService.svc.cs**
```csharp
using System.ServiceModel;
using System.ServiceModel.Web;

namespace TD.SOC.Publish
{
    [ServiceContract]
    public partial interface ISOCService
    {
        [OperationContract]
        [WebGet(UriTemplate = "HelloWorld", RequestFormat = WebMessageFormat.Json, ResponseFormat = WebMessageFormat.Json)]
        string HelloWorld();
    }
}
```
- Trong thư mục **ISAPI/TD.SOC.Publish**, tạo file **SOCService.svc.cs**
```csharp
using System.ServiceModel;
using System.ServiceModel.Activation;

namespace TD.SOC.Publish
{
    [AspNetCompatibilityRequirements(
    RequirementsMode = AspNetCompatibilityRequirementsMode.Required)]
    [ServiceBehavior(IncludeExceptionDetailInFaults = true)]
    public partial class SOCService : ISOCService
    {
        public string HelloWorld()
        {
            return "Hello world";
        }
    }
}
```
### Publish project
- Chuột phải **Project** -> **Publish**
- Vào thư mục **Documents** trong **File Explorer**, ta sẽ thấy file **TD.SOC.Publish.wsp**
- Đổi tên **TD.SOC.Publish.wsp** thành **TD.SOC.Publish.zip**
- Copy **TD.SOC.Publish.dll** vào thư mục **bin** trong **Sharepoint**
- Copy thư mục **ISAPI/TD.SOC.Publish** vào thư mục **_vti_bin** trong **Sharepoint**
### Chạy thử
- [https://{siteurl}/_vti_bin/TD.SOC.Publish/SOCService.svc/HelloWorld](https://baocao.hanhchinhcong.net/_vti_bin/TD.SOC.Publish/SOCService.svc/HelloWorld)