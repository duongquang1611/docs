### Cài đặt các thư viện sau:
- `System.Runtime.CompilerServices.Unsafe` version `4.5.2`
- `Unity` version `5.10.0`
- `Z.EntityFramework.Classic.Community` version `7.0.0`

### Tạo file integration
- Tạo thư mục `Integration`
- Trong thư mục tạo 1 file để lưu thông tin DI, ví dụ ExamplePublishModule.cs
- Trong file thực hiện các khai báo, ví dụ như sau:
```csharp
    public class ExamplePublishModule
    {
        public void ConfigureContainer(IUnityContainer container)
        {
            container
                // DbContext
                .RegisterType<OpenDataContext>()
                // Repositories
                .RegisterType<IFieldRepository, FieldRepository>()
                .RegisterType<IOfficeRepository, OfficeRepository>()
                .RegisterType<IDatasetRepository, DatasetRepository>()
                // Another
                .RegisterFactory<ICoreServicesProvider>(c => new DefaultContextCoreServicesProvider());
        }
    }
```

### Sử dụng trong các file khác
```csharp
    public class OPDTService {
        private IUnityContainer _unityContainer;
        private IDatasetRepository _datasetRepository;

        public OPDTService () {
            _unityContainer = new UnityContainer().EnableDiagnostic();
            var integration= new ExamplePublishModule();
            integration.ConfigureContainer(_unityContainer);
            
            _datasetRepository = _unityContainer.Resolve<IDatasetRepository>();
        }
    }
```
