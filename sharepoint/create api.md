### Prerequisite
Trong **Nuget Package Manager**, cài 1 số thư viện sau:
- Microsoft.AspNet.WebApi `5.2.7`
- Newtonsoft.Json `11.0.2`
- System.Runtime.CompilerServices.Unsafe `4.5.2`
- Unity `5.10.0`


Nhớ thêm các **reference** sau:
- TD.Core.Api.Common
- TD.Core.Api.Common.Http
- TD.Core.Api.Mvc
- TD.Core.Api.Mvc.Integration
- System.Web 

### Tạo Controller
Tạo thư mục Controllers, trong thư mục Controllers, tạo file UsersController
```csharp
using System; 
using System.Collections.Generic; 
using System.Linq; 
using System.Text; 
using System.Text.RegularExpressions; 
using System.Threading.Tasks; 
using System.Web.Http; 
using TD.Core.Api.Mvc; 
using TD.Example.Library.Models; 
using TD.Example.Library.Repositories; 
using TD.Example.Library.ViewModels; 

namespace SharePointProject2.WebApi.Controller 
{ 
    public class UsersController : TDApiController 
    { 
        private readonly IUserRepository _UserRepository; 

        public UsersController(IUserRepository UserRepository) 
        { 
            _UserRepository = UserRepository; 
        } 

        // GET: webapi/Users/count 
        [Route("webapi/Users/count")] 
        [HttpGet] 
        public IHttpActionResult GetCountUser() 
        { 
            var count = _UserRepository.Count(); 
            if (count == 0) 
            { 
                return ApiNotFound(); 
            } 
            return ApiOk(count); 
        } 
    } 
} 
```

### Tạo Integration
Tạo thư mục Integration, sau đó, từ thư mục tạo file ApiModule.cs 
```csharp
using System.Collections.Generic; 
using TD.Core.Api.Mvc; 
using TD.Core.Api.Mvc.Integration; 
using Unity; 
using System.Web.Http; 
using System.Reflection; 
using TD.Example.Library.Repositories; 

namespace TD.Example.WebApi.Integration 
{ 
    public sealed class ApiModule : IApiModule 
    { 
        public string GetPrefix() 
        { 
            return "webapi"; 
        } 

        public ICollection<Assembly> GetAssemblies() 
        { 
            return new Assembly[] { typeof(ApiModule).Assembly }; 
        } 

        public void RegisterApiConfig(HttpRouteCollection routes) 
        { 
            routes.MapHttpRoute( 
                name: "qlUsers", 
                routeTemplate: "webapi/{controller}/{action}/{id}", 
                defaults: new { controller = "Home", action = "Index", id = RouteParameter.Optional } 
            ); 
        } 

        public void RegisterDIConfig(UnityContainer container)
        { 
            container.RegisterType<IUserRepository, UserRepository>(); 
            container.RegisterFactory<ICoreServicesProvider>(c => new DefaultContextCoreServicesProvider()); 
        } 
    } 
} 
```
### Tạo signing key with strong name
[Link](trick/create-Signal-Key-With-Strong-Name.md)
### Tạo public key
[Link](trick/get-Public-Token-Key.md)
### Thêm trong web.config
```xml
  <ApiIntegrationConfigs>
    <ApiIntegrations>
      <add name="BCApiModule" type="TD.BC.API.Integration.BCApiModule, TD.BC.API, Version=1.0.0.0, Culture=neutral, PublicKeyToken=2fcbd4e65ceb67c7" />
    </ApiIntegrations>
  </ApiIntegrationConfigs>
```
```xml
<section name="ApiIntegrationConfigs" type="TD.Core.Api.Mvc.Integration.ApiIntegrationConfigsSection, TD.Core.Api.Mvc.Integration, Version=1.0.0.0, Culture=neutral, PublicKeyToken=8a6dc52edcc09b88" />
```
