### Hàm chính
```csharp

public string Function ()
{
    //Sử dụng HttpClient để gọi API
    using (var client = new HttpClient())
    {
        //Gọi hàm xác thực sharepoint
        string strErrorCode = AuthenSharepoint(client);
        if (strErrorCode == "NoError") //Xác thực thành công
        {
            //Dùng tiếp client ở trên sau khi đã xác thực thành công.
            client.SendAsync(...)
        }
    }
}
```
### Xác thực sharepoint
```csharp
private string AuthenSharepoint(HttpClient client)
{
    StringContent dataReqAuth = GetContentAuthentication();
    string urlAuthenSP = UrlBase + "/_vti_bin/Authentication.asmx";
    var reqAuth = new HttpRequestMessage(HttpMethod.Post, urlAuthenSP) { Content = dataReqAuth };
    var resAuth = client.SendAsync(reqAuth);
    string strResAuth = resAuth.Result.Content.ReadAsStringAsync().Result;
    XDocument xDocAuth = XDocument.Parse(strResAuth);
    XElement elementErrorCode = GetElement(xDocAuth, "ErrorCode");
    return elementErrorCode.Value;
}

```
### Lấy dữ liệu xác thực (truyền vào user + pass)
```csharp

private StringContent GetContentAuthentication()
{
    string valuePost = "<?xml version=\"1.0\" encoding=\"utf-8\"?>";
    valuePost += "<soap:Envelope xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\" xmlns:soap=\"http://schemas.xmlsoap.org/soap/envelope/\">";
    valuePost += "<soap:Body><Login xmlns=\"http://schemas.microsoft.com/sharepoint/soap/\">";
    valuePost += "<username>"+ User + "</username>";
    valuePost += "<password>"+ Pass + "</password>";
    valuePost += "</Login> </soap:Body> </soap:Envelope>";
    StringContent data = new StringContent(valuePost, Encoding.UTF8, "text/xml");
    return data;
}
```
### Đọc dữ liệu xml hàm xác thực trả về
```csharp

private XElement GetElement(XDocument doc, string elementName)
{
    foreach (XNode node in doc.DescendantNodes())
    {
        if (node is XElement)
        {
            XElement element = (XElement)node;
            if (element.Name.LocalName.Equals(elementName))
                return element;
        }
    }
    return null;
}
```