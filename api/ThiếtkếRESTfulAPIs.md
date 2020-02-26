# Thiết kế RESTful APIs

<img src="https://i.imgur.com/gSaBTwh.jpg" />

**Hôm rồi thấy anh bạn share document về RESTful APIs (Đọc thêm RESTful API là gì?) của Shoptify, tự dưng nghĩ đến hình như chưa có bài viết nào bằng tiếng Việt về cách xây dựng RESTful APIs mặc dù vấn đề này đã khá phổ biến. Vậy nên tôi viết bài này, chia sẻ một số best practices mà chúng tôi đã thảo luận và áp dụng khi thiết kế và xây dựng APIs cho sản phẩm của mình.**

Tại sao API lại quan trọng trong mỗi sản phẩm? Để hiểu đơn giản, bạn có thể tham khảo bài viết này, dù không mới nhưng có một tiêu đề và nội dung rất hay. “Trong thế giới kết nối như bây giờ, một sản phẩm không thể đứng độc lập, và sản phẩm nào không có APIs, giống như máy tính không được kết nối Internet vậy”.

## RESTful

Tôi không phân tích tại sao lại là `RESTful`, vì chủ đề này có lẽ cần một bài viết khác, nhưng nếu có cũng thành thừa vì gần như chúng ta đều đồng ý với nhau rằng `APIs` trong thời đại ngày nay sử dụng tiêu chuẩn `RESTful`.

Nhưng nếu chúng ta đã quyết định sử dụng `RESTful` cho `APIs`, thì nên hiểu đúng và làm đúng. `RESTful` có thể tóm lược như sau:

- `URI` như tên gọi (`Uniform Resource Identifier`), chỉ định địa chỉ của mỗi tài nguyên (resource). VD: *api.shop.me/customer* hoặc *api.shop.me/order*
- Các `method` của `HTTP` được sử dụng tương ứng với những hành động `CRUD` trên `resource` tương ứng. Thông thường: *Create – POST, Read – GET, Update – PUT / PATCH, Delete – DELETE*

Hầu hết những webservice cũ chỉ sử dụng 2 phương thức GET và POST và để đơn giản với người sử dụng; nhưng lại sử dụng URI cho cả entity lẫn function. Đừng mang tư tưởng đó khi thiết kế RESTful.

## Tại sao là entity?

`Webservice` theo chuẩn cũ coi `resource` gồm `business entity` và `method` / `function` nhưng thực sự thì `method` / `function` không nên là resource, chỉ `business entity` là `resource` như `RESTful` định nghĩa mà thôi. Nhưng tại sao lại “quy hoạch” theo `business entity`? Vì dễ hơn. Mọi lập trình viên đều dễ làm việc với entity hơn. Chẳng phải ai cũng bắt đầu với một nghiệp vụ với `CRUD` một `entity` đó sao? Trong một hệ thống thông thường, các `entity` có số lượng ít và “đơn giản”, dễ hiểu hơn các nghiệp vụ (method / function). Cung cấp danh sách các `entity` cùng ít hoạt động `CRUD` nhằm ẩn đi những nghiệp vụ phức tạp (thường là lời gọi tới hàng chục `method`), giúp các lập trình viên sử dụng đơn giản hơn.

Ví dụ, để cập nhật một đơn hàng, chúng ta chỉ đơn giản gọi tới API *http://api.shop.me/orders/10* với method PUT kèm theo những thông tin mới, thay vì phải đắn đo giữa một loạt những API như *http://api.shop.me/updateOrder, http://api.shop.me/replaceOrder…* với hàng loạt những ràng buộc về nghiệp vụ.

## JSON only?

Hầu hết những nhóm viết `RESTful API` giờ đây đều chọn `JSON` là format chính thức nhưng rất nhiều nhóm vẫn phân vân với câu hỏi “chỉ `JSON` hay hỗ trợ thêm `XML`?”. Tất nhiên, có hàng tá lý do để chúng ta hỗ trợ thêm những format khác, đặc biệt là `XML`. Nhưng theo tôi, chỉ cần `JSON` là đủ. Tôi không tin rằng ngày nay có lập trình viên hay nền tảng nào không biết hoặc không hỗ trợ `JSON`. Hỗ trợ nhiều định dạng chỉ làm cho việc kiểm thử `API` thêm phức tạp.

## gzip và binary format?

Có chăng chúng ta nên nghĩ tới việc sử dụng những format dạng `binary` để tiết kiệm dữ liệu khi truyền tải qua mạng như `BSON`, `MessagePack`… Tuy vậy, hãy để chúng là optional, và người dùng có thể định nghĩa format nhận thông qua `HTTP Header`.

Tương tự là câu hỏi “`formatted` hay compressed `JSON`? Có nên `enable gzip`?” (`formatted` hay `pretty`) `JSON` là `JSON` được thể hiện ở định dạng “đẹp”, được format bởi tab…; ngược lại `compressed JSON` loại bỏ hết các dấu `tab`, `space`, `enter`.. nhằm giảm dung lượng). `Compressed JSON` và `gzip` rất tuyệt vời vì có thểm giảm từ 30-70% dung lượng truyền tải. Đồng nghĩa với server sẽ ít tốn tài nguyên và thời gian để xử lý việc nén. Và với tốc độ mạng ngày càng được cải thiện như hiện nay, có lẽ đặt `formatted JSON` là mặc định thì tốt hơn. Tuỳ vào điều kiện `network` (VD trên mobile…), client chỉ định định dạng muốn nhận về thông qua `HTTP Header` (`Content-Type` hoặc `Accept`).

## snake_case hay camelCase?

Well, câu trả lời muôn thủa, kiểu như “thế giới có 2 loại người…”. Việc sử dụng `snake case` hay `camel case` chủ yếu do sở thích của lập trình viên thôi, không có lý do gì để phân định được. `Camel case` tiết kiệm hơn `snake case` (1 ký tự, `orderName` so với `order_name`) là lý do không đủ thuyết phục.

```
// snake_case
{
    customer_id: 100,
    full_name: "Paul Scholsy"
}
 
// camelCase
{
    customerId: 100,
    fullName: "Paul Scholsy"
}
```

## dash hay underscore (- hay_)?

Hmm, tương tự như trên. Theo bạn thì *http://api.shop.me/orders/last-updated* hay *http://api.shop.me/orders/last_updated* sẽ tốt hơn? Tất nhiên, *http://api.shop.me/orders/lastUpdated* thì quá tệ, đừng đưa nó vào danh sách lựa chọn của bạn.

## Số ít hay số nhiều?

Tương tự như trên. Để liệt kê các đơn đặt hàng, chúng ta sẽ đặt URI là *http://api.shop.me/order* hay *http://api.shop.me/orders* ?

Chuyện này giống như chúng ta cãi nhau về cách đặt tên bảng trong CSDL quan hệ vậy, `Order` hay `Orders`? `Best practice` cho việc đặt tên bảng trong CSDL quan hệ là sử dụng số ít: `Order`; nhưng trong api là số nhiều: `/orders`. Lý do là nó tường minh và gợi nhớ hơn cho người dùng rằng đây là “danh sách đơn hàng”. Rõ ràng, nó tường minh hơn /order (danh sách đơn hàng) và /profile (thông tin người dùng); cùng là dạng số ít nhưng cấu trúc dữ liệu trả về lại khác nhau.

Câu chuyện phức tạp hơn một chút với `/persons` hay `/people` :). Hãy sử dụng `/persons`.

## Quan hệ

Đây là vấn đề đau đầu nhất. Bài toán thế này: Một `đơn hàng` thuộc về một `khách hàng` và có nhiều `sản phẩm`. Vậy thông tin `đơn hàng` trả về sẽ có những gì? Nếu bạn sử dụng CSDL quan hệ thì chắc chắn là bạn đang lưu những thông tin này trong 3 bảng: `Order`, `Customer` và `Product`. Trả về một `entity` có những thông tin giống hệt như bảng `Order` là điều ngu ngốc. Trả về một `entity` có  những thông tin được `JOIN` từ 3 bảng trên còn ngu ngốc hơn. Tại sao? Bởi vì nếu thể hiện đúng data model thì chúng ta cần API để làm gì? Thử tưởng tượng dạng đơn giản nhất của entity Order thế này:

```
{
    customer_id: 10,
    product_id: [100, 101, 102]
}
```
Và để hiển thị thông tin `đơn hàng` có `tên` và `địa chỉ khách hàng`, `tên sản phẩm`, `client` (`APIs consumer`) cần gọi thêm hàng chục `API` nữa. Bạn cũng hiểu tại sao không nên trả về thông tin `đơn hàng` được `JOIN` từ 3 bảng: có hàng trăm thông tin không cần thiết. Việc cân bằng, tìm ra những thông tin nào nên được trả về trong tình huống này không thực sự đơn giản. Ý tưởng ở đây là API nên trả về thông tin “thiết yếu, liên quan trực tiếp tới nghiệp vụ”. Ví dụ trong tình huống này là:

*http://api.shop.me/orders/10*

```
{
    customer: {
        id: 105,
        full_name: "Cristiano Messi"
    }
    products: [
        {
            id: 101,
            name: "iPhone"
        },
        {
            id: 102,
            name: "iPad"
        }
    ]
}
```

Nếu client cần thêm thông tin về khách hàng, gọi tới API *http://api.shop.me/customers/105*

Nếu client cần thêm thông tin về sản phẩm cho order này, gọi tới API *http://api.shop.me/orders/10/products*

```
{
    meta: {
        page: 1,
        total: 2,
    }
    products: [
        {
            id: 101,
            name: "iPhone"
            sku: "42432",
        },
        {
            id: 102,
            name: "iPad",
            sku: "23112",
        }
    ]
}
```

Và theo trí tưởng tượng phong phú, bạn có thể nghĩ tới việc để client gọi tới API *http://api.shop.me/orders/10/products/101* để lấy thông tin chi tiết của sản phẩm có mã 101.

Câu trả lời là không. Nếu cần thông tin đầy đủ, client hãy gọi tới API *http://api.shop.me/products/101*.

Ở đây tôi chỉ dám đưa ra ý tưởng và ví dụ cơ bản, bởi “thông tin bổ sung” trong quan hệ của những `entity` là vấn đề thực sự không đơn giản và phụ thuộc rất nhiều vào từng nghiệp vụ cụ thể.

## Pagination

Hãy luôn sử dụng `pagination`. Trả về đầy đủ danh sách khách hàng qua API *http://api.shop.me/customers* là việc tốn kém tài nguyên, đồng thời không hữu dụng. Bởi client cũng sẽ giới hạn lại danh sách này nhằm đáp ứng một giao diện dễ nhìn cho người dùng.

Hãy để thêm những `param` cố định trong mỗi API trả về một danh sách dữ liệu như `/customers`, `/orders: page, page_size…` để client có thể chỉ định *http://api.shop.me/orders?page=4&page_size=10*. Luôn sử dụng `page_size` mặc định để giới hạn dữ liệu trả về ngay cả khi người dùng không chỉ định rõ trong lời gọi API.

Tất nhiên, `pagination` thì có hàng chục cách, trên đây chỉ là một ví dụ.

## Filter và dynamic field, dynamic query

Có một cách để giải quyết vấn đề trả về những dữ liệu quan hệ như trên là sử dụng `dynamic field`. Ví dụ *http://api.shop.me/orders?fields=id,time,customer.fullname,product.id*

Tương tự với `dynamic query` để thực hiện `filter` với những điều kiện truy vấn xác định. Trước đây, chúng tôi sử dụng OData và nó khá hiệu quả.

Ngoài ra, còn 1 khái niệm nữa, là `embed`: gắn thêm những thông tin liên quan. Ví dụ:

*http://api.shop.me/order/10*

```
{
    customer_id: 105,
    product_id: [101, 102]
}
```

*http://api.shop.me/orders/10?embed=customer*

```
{
    customer: {
        id: 105,
        full_name: "Cristiano Messi"
    }
    product_id: [101, 102]
}
```

Nhưng embed là một vấn đề khó, đừng chú tâm vào nó nếu không cần thiết.

Luôn trả về metadata với dữ liệu dạng danh sách, dựa vào đó client cũng biết được các truy xuất vào những thông tin khác. Ví dụ: *http://api.shop.me/orders*

```
{
    meta: {
        page: 1,
        page_size: 10,
        total: 2,
    }
    product: [
        {
            id: 101,
        },
        {
            id: 102,
        }
    ]
}
```

## Partial update?

Một `đơn hàng` (`entity Order`) chứa 10 `field`, điều gì xảy ra nếu chúng ta chỉ muốn chỉnh sửa 2 `field`? Có 2 cách thông thường:

- Giữ nguyên dữ liệu được trả về từ `API` gọi qua `GET`, với 2 `field` được chỉnh sửa và gọi `API` qua `PUT`.
- Set  dữ liệu đúng cho 2 field này, set các field còn lại thành null (hoặc zero, empty… – dấu hiệu nhận biết là field này không thay đổi) và gọi `API` qua `PUT`

Cả 2 cách này đều cần gửi lên đầy đủ format của `entity`. Một cách khác, chỉ cần gửi lên 1 object với đúng 2 `field` trên, gọi là `partial update`, thường sử dụng method `PATCH`.

Partial update rất hữu dụng, nhưng thường có 2 vấn đề:

- Việc `deserialise` ở phía server để ra đúng object không quá đơn giản
- Đôi khi gây bối rối cho người sử dụng API, đặc biệt với các required field.

Do đó, partial update nên được coi là phần advanced của API và nên xuất hiện ở version 2, 3.. Trước đây chúng tôi sử dụng OData khá tốt (nhưng không thực sự hoàn hảo) cho partial update. Quan tâm tới vấn đề này ngay từ ngày đầu thiết kế API khiến chúng tôi mất khá nhiều thời gian.

## Status code

Luôn sử dụng `HTTP code` cho `catched exception`. Thay vì trả về message “Unauthorised” với `HTTP code` = 200, hãy trả về 401. Bạn có thể tìm hiểu thêm ở đây: *http://www.restapitutorial.com/httpstatuscodes.html*

Nói chung, `RESTful APIs` chỉ nên dùng những `HTTP code` 4xx cho những `catched exception` (`unauthorised`, `validation`…). Nếu những lỗi này là không đủ, hãy mở rộng chúng và đặt tham chiếu trong tài liệu `API`. Ví dụ, lỗi `validation` thường sử dụng HTTP code là 400 (`bad request`) hoặc 409 (`conflict`), cho lời gọi PUT *http://api.shop.me/orders/10* trả về HTTP code = 409 cùng message (10xx dùng để validate order, tương tự 11xx cho customer, 24xx cho product…)

```
{
    message: "Cannot save order status",
    errors: [
        {
            code: 4091023,
            field: state,
            description: "Order state is incorrect. Please use OPEN, DELIVERING or DONE"
        }
        {
            code: 4091086,
            field: customer_id,
            description: "Customer isn't existing"
        }
    ]
}
```

## Authorization

Bởi `RESTful` là `stateless HTTP` nên đừng làm nóng `server` và `client` bởi `session` hay `cookies`. Cách đúng đắn nhất là sử dụng `Authorization Header`. `OAuth 2` là chuẩn được khuyến nghị cho `RESTful APIs`.

## Versioning

Vấn đề này với `RESTful APIs` phải nói là “hay dữ dội”. Sự phức tạp này đến từ việc khác nhau giữa `client`-side APIs` và `server-side APIs`: khi sử dụng thư viện trên `client`, chúng ta hoàn toàn chủ động trong việc chọn `version` “gắn chặt” nó với chương trình, nhưng gọi qua `RESTful` thì không phải như vậy. Khi một version mới của `RESTful API` ra đời, version cũ sẽ sớm “chết”.

Có 3 cách thường dùng để `versioning RESTful API`: 2 trong số đó là versioning qua URI: qua sub-domain (*http://v1.api.shop.me/orders*) hoặc resource path (*http://api.shop.me/orders/v1*). Cả 2 cách đều dở như nhau vì nó khiến phía client bị hard-code. Cách versioning qua resource path tệ hơn, vì /v1 vốn không phải là 1 property của /orders. Còn nếu bạn dùng *http://api.shop.me/v1/orders* thì hãy dùng sub-domain, nó còn tuyệt hơn.

Cách thứ 3 là `versioning` thông qua `HTTP Header` (VD: API-Version=v1.1) sẽ tốt hơn nhưng ít người sử dụng, chỉ bởi việc phải đặt thêm `HTTP header` khiến người dùng không dễ `copy-paste URI` vào `web browser` để test.

Một cách tiếp cận khác là “không sử dụng `versioning`”, nghĩa là cố gắng hỗ trợ việc tương thích ngược hoặc xác định rõ thời điểm cung cấp `API` mới và yêu cầu người dùng phải `migrate` lên `version` mới trong một khoảng thời gian xác định. Đây là cách Google, Facebook.. đang làm. Nhưng phải nói thật là, chỉ khi nào “chúng ta lớn” hoặc có sản phẩm cùng `RESTful APIs` là trung tâm cuộc chơi về kết nối, tích hợp thì mới dễ thực hiện theo phương pháp này.

Tuy nhiên cũng chẳng có vấn đề gì nếu bạn bỏ qua việc `versioning` từ phiên bản `APIs` đầu tiên, hoặc `versioning` thông qua `HTTP Header` (nếu API-Version không được chỉ định, sử dụng version mới nhất). Nếu chúng ta không làm tốt `version` đầu tiên thì chắc gì đã cần tới v2, v3… thậm chí là v1.1 ?

## HATEOAS

`HATEOAS` là một trong những chuẩn được khuyến nghị cho `RESTful APIs` và bạn nên tận dụng. Ví dụ một bài toán: Khi lấy thông tin chi tiết của 1 đơn hàng (`entity Order`), người dùng hiện tại (nhận biết qua `Access Token`) chỉ được xem và cập nhật, không được xoá; làm sao `client` nhận biết được những `action` này? Cách đơn giản là sử dụng `HATEOAS`, gắn những `hyperlink` trong giá trị trả về tương ứng với những hành động có thể thực hiện trên `entity`.

*http://api.shop.me/orders/10*

```
{
    customer_id: 10,
    product_id: [100, 101, 102],
    links: [
        {
            rel: "self",
            href: "http://api.shop.me/orders/10",
            method: "GET"
        },
        {
            rel: "order.update",
            href: "http://api.shop.me/orders/10",
            method: "PATCH"
        },
        {
            rel: "order.update",
            href: "http://api.shop.me/orders/10",
            method: "PUT"
        } 
    ]
}
```

HATEOAS rất hữu dụng nhưng như bạn thấy, bandwidth dành cho nó cũng không dễ chịu chút nào; tương tự với việc xử lý để tính toán ra những hành động, hyperlink tương ứng. Nếu không cần thiết, hãy hỗ trợ HATEOAS là optional thông qua header.

## Overriding

Đừng ngần ngại overriding nếu cần thiết. HATEOAS không được liệt kê trong default HTTP header, hãy tự định nghĩa. Tương tự với HTTP method nếu thấy cần thiết. Nhưng hãy nhớ là cần thiết, đừng override Authorization thành shop.me-Authorization nếu bạn không biết mình đang làm gì.

## Tài liệu

RESTful APIs phải có khả năng self-described hay self-documented; nghĩa là người dùng nhìn vào là biết cách dùng ngay. Tuy vậy, vẫn có những thứ bạn cần viết ra thành tài liệu như: authorization, HTTP header, filter, pagination… và nếu gói gọn trong Getting Started là tốt nhất. Đẹp nhất là người dùng có thể gọi thành công 1 API trong dưới 15 phút thông qua những công cụ đơn giản như curl, Fiddler, REST Console… ngay khi đọc xong Getting Started.

Có những ngôn ngữ / công cụ rất hay như Swagger để viết APIs document, thậm chí là generate document từ code và cho phép người dùng hand-on với APIs. Nhưng hãy cẩn thận, có lẽ bạn cần sandbox environment để tránh người dùng quá “hứng thú” trong việc khám phá APIs của mình.

## Cuối cùng

Trên đây tôi chỉ điểm qua một số best practices bạn nên lưu ý khi thiết kế RESTful APIs. Bạn có thể đọc thêm cuốn sách này, không quá hay đâu nhưng có nhiều góc nhìn hơn.

Tuy vậy, trên tất cả, hãy nhớ rằng “Với một lập trình viên backend, thiết kế APIs chính là thiết kế giao diện. Đừng đi ngược xu thế UX, hãy thiết kế chúng thật đơn giản, hữu dụng và mạnh mẽ”. Những lập trình viên có thể kiên trì hơn người dùng cuối, nhưng bạn cũng chỉ có không quá 30 phút để cho họ hiểu cách sử dụng APIs của mình.

















