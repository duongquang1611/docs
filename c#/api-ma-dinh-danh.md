### Get Group
>/_api/web/lists/getbytitle('Groups')/items?$select=ID,GroupName,GroupCode,OfGroup/ID,OfGroup/GroupCode,OfGroup/GroupName&$expand=OfGroup/ID,OfGroup/GroupCode,OfGroup/GroupName&$top=1000
```csharp
string urlGetGroups = UrlBaseMaDinhDanh + PathGetAllGroups;
var req = new HttpRequestMessage(HttpMethod.Get, urlGetGroups) {};
req.Headers.Add("Accept", "application/json;odata=verbose");
var res = client.SendAsync(req);
var resStr = res.Result.Content.ReadAsStringAsync().Result;
```
### Get User
>_api/web/lists/getbytitle('UserPositions')/items?$select=Title,UserProfile/Account,Group/GroupCode,Group/GroupName,UserOffice/GroupCode,UserOffice/GroupName&$expand=UserProfile/Account,Group/GroupCode,Group/GroupName,UserOffice/GroupCode,UserOffice/GroupName&$top=10000
```csharp
string urlGetUsers = UrlBaseMaDinhDanh + PathGetAllUsers;
var req = new HttpRequestMessage(HttpMethod.Get, urlGetUsers) {};
req.Headers.Add("Accept", "application/json;odata=verbose");
var res = client.SendAsync(req);
var resStr = res.Result.Content.ReadAsStringAsync().Result;
```
