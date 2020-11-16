### Thêm mới phần tử trong List
```javascript
    function add(siteUrl, listName, itemProperties, success, failure) {
        var url = `${siteUrl}/_vti_bin/listdata.svc/${listName}`;
        $.ajax({
            url: url,
            type: "POST",
            processData: false,
            contentType: "application/json; odata=verbose",
            data: JSON.stringify(itemProperties),
            headers: {
                Accept: "application/json; odata=verbose",
            },
            success: function (data) {
                success(data.d);
            },
            error: function (data) {
                failure(data.responseJSON.error);
            },
        });
    }
```

### Lấy 1 phần tử trong List
```javascript
    function getById(siteUrl, listName, itemId, success, failure) {
        var url = `${siteUrl}/_vti_bin/listdata.svc/${listName}(${itemId})`;
        $.ajax({
            url: url,
            method: "GET",
            headers: { Accept: "application/json; odata=verbose" },
            success: function (data) {
                success(data.d);
            },
            error: function (data) {
                failure(data.responseJSON.error);
            },
        });
    }
```

### Cập nhật 1 phần tử
```javascript
    function update(siteUrl, listName, itemId, itemProperties, success, failure) {
        getById(siteUrl, listName, itemId,
            function (item) {
                $.ajax({
                    type: "POST",
                    url: item.__metadata.uri,
                    contentType: "application/json",
                    processData: false,
                    headers: {
                        Accept: "application/json;odata=verbose",
                        "X-HTTP-Method": "MERGE",
                        "If-Match": item.__metadata.etag,
                    },
                    data: Sys.Serialization.JavaScriptSerializer.serialize(itemProperties),
                    success: function (data) {
                        success(data);
                    },
                    error: function (data) {
                        failure(data);
                    },
                });
            },
            function (error) {
                failure(error);
            })
    }
```

### Xóa 1 phần tử
```javascript
    function _delete(siteUrl, listName, itemId, success, failure) {
        getById(siteUrl, listName, itemId,
            function (item) {
                $.ajax({
                    url: item.__metadata.uri,
                    type: "POST",
                    headers: {
                        Accept: "application/json;odata=verbose",
                        "X-Http-Method": "DELETE",
                        "If-Match": item.__metadata.etag,
                    },
                    success: function (data) { success(); },
                    error: function (data) { failure(data.responseJSON.error) },
                });
            },
            function (error) { failure(error) }
        )
    }
```