### Lấy thông tin trang
```javascript
    function getFormDigest(siteUrl) {
        return new Promise(function(resolve, reject) {
            $.ajax({
                url: siteUrl + '/_api/contextinfo',
                method: 'POST',
                headers: { 'Accept': 'application/json; odata=verbose' },
                success: function(data) {
                    resolve(data.d.GetContextWebInformation.FormDigestValue)
                },
                error: reject
            });
        });
    }
```
## Lấy một phần tử
```javascript
    function getItem(site, key) {
        return new Promise(function(resolve, reject) {
            $.ajax({
                url: site + "/_api/web/lists/GetByTitle('Config')/items",
                data: {
                    $filter: "Title eq '" + key + "'",
                },
                headers: {
                    "accept": "application/json;odata=verbose",
                    "content-type": "application/json;odata=verbose",
                }
            })
            .done(function(res){
                resolve(res && res.d && res.d.results && res.d.results[0]);
            })
            .fail(reject)
        });
    }
```
### Thêm mới một phần tử
```javascript
    function addItem(site, props) {
        return getFormDigest(site)
        .then(function(digest) {
            return new Promise(function(resolve, reject) {
                $.ajax({
                    url: site + "/_api/web/lists/GetByTitle('Config')/items",
                    type: "POST",
                    headers: {
                        "Accept": "application/json;odata=verbose",
                        "Content-Type": "application/json;odata=verbose",
                        "X-RequestDigest": digest,
                    },
                    data: JSON.stringify($.extend({}, {'__metadata': { 'type': 'SP.Data.ConfigListItem' }}, props)),
                    success: resolve,
                    error: reject
                });
            });
        });
    }
```
### Cập nhật một phần tử
```javascript
    function updateItem(site, id, props) {
        return getFormDigest(site)
        .then(function(digest) {
            return new Promise(function(resolve, reject) {
                $.ajax({
                    url: site + "/_api/web/lists/GetByTitle('Config')/items("+id+")",
                    type: "POST",
                    data: JSON.stringify($.extend({}, {'__metadata': { 'type': 'SP.Data.ConfigListItem' }}, props)),
                    headers: {
                        "accept": "application/json;odata=verbose",
                        "content-type": "application/json;odata=verbose",
                        "X-RequestDigest": digest,
                        "IF-MATCH": "*",
                        "X-Http-Method": "PATCH"
                    },
                    success: resolve,
                    error: reject
                });
            });
        });
    }
```
### Cách sử dụng
```javascript
    function get(site, key) {
        return getItem(site, key)
            .then(function(item) {
                return item && item.Value;
            });
    }
```
```javascript
    function set(site, key, val) {
        return getItem(site, key)
            .then(function(item) {
                if (item) {
                    return updateItem(site, item.ID, { Value: val });
                }

                return addItem(site, { Title: key, Value: val });
            });
    }
```
#### Lưu ý:
- Mỗi list nên tạo một file này riêng để dễ đọc hiểu hơn