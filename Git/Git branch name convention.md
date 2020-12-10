### Code Flow Branches  
Những nhánh mà mong đợi sẽ có sẵn vĩnh viễn trên kho lưu trữ theo dòng thay đổi mã bắt đầu từ khi phát triển cho đến production.
- **Development** (dev)  
Tất cả các tính năng mới và sửa lỗi sẽ được đưa đến nhánh `dev`. Giải quyết xung đột mã nên được thực hiện sớm nhất tại đây.
- **QA/Test** (test)  
Chứa tất cả các mã sẵn sàng để QA testing
- **Staging** (staging)  
Nó chứa các tính năng đã được thử nghiệm mà các bên liên quan muốn có sẵn cho bản demo hoặc bản đề xuất trước khi đưa vào production. Các quyết định được đưa ra ở đây nếu một tính năng cuối cùng nên được đưa vào mã production.
- **Master** (master)  
Nhánh production, nếu kho lưu trữ được publish, đây là nhánh mặc định đang được trình bày

Ngoại trừ các Hotfix, mã sẽ tuân theo hợp nhất một chiều bắt đầu từ dev> test> stagging> master.
### Temporary Branches
Đây là các nhánh dùng một lần có thể được tạo và xóa theo nhu cầu của nhà phát triển hoặc người triển khai.
- **Feature**  
Bất kỳ thay đổi mã nào cho một mô-đun hoặc ca sử dụng mới nên được thực hiện trên một nhánh `feature`. Nhánh này được tạo dựa trên nhánh `dev` hiện tại. Khi tất cả các thay đổi được thực hiện xong, cần có một `Pull request / Merge Request` để đưa tất cả những thay đổi này vào nhánh `dev`.
Ví dụ:  
    - feature/integrate-swagger
    - feature/JIRA-1234
    - feature/JIRA-1234_support-dark-theme      
Nên sử dụng các chữ cái thường và dấu gạch ngang để mô tả các tên thông thường, trừ trường hợp ID hoặc tên các trường hợp đặc biệt (Issue, JIRA). Dấu gạch dưới dùng để phân tách ID và mô tả
- **Bugfix**
Nếu các thay đổi code từ nhánh `feature` bị refect sau khi release, demo. Mọi sửa đổi sau đó sẽ được thực hiện trên nhánh `bugfix`.  
Ví dụ:
    - bugfix/more-gray-shades
    - bugfix/JIRA-1444_gray-on-blur-fix
- **Hotfix**
Nếu cần một bản vá tức thời hay các thay đổi sẽ được áp dụng ngay lập tức trên production, thay đổi code nên được lưu trong `hotfix`.  
Ví dụ:
    - hotfix/disable-endpoint-zero-day-exploit
    - hotfix/increase-scaling-threshold
- **Experimental**
Bất kỳ một tính năng hay ý tưởng nào không thuộc về release hay spint.  
Ví dụ:
    - experimental/dark-theme-support
- **Build**
Một nhánh để xử lý việc build các phiên bản mới hay thực hiện hoạt động chạy mã.  
Ví dụ:
    - build/jacoco-metric
- **Realease**
Một nhánh để gắn thẻ phiên bản phát hành cụ thể
Ví dụ:
    - release/myapp-1.01.123  
Git cũng hỗ trợ gắn thẻ tag cho những commit cụ thể của kho lưu trữ. Một nhánh phát hành được sử dụng nếu có nhu cầu cung cấp mã để checkout hoặc sử dụng.
- **Merging**
Một nhánh tạm thời để giải quyết xung đột hợp nhất, thường là giữa bản `dev` mới nhất và một `feature` hoặc nhánh `hotfix`
Điều này cũng có thể được sử dụng nếu hai nhánh của một `feature` đang được nhiều nhà phát triển làm việc cần được hợp nhất, xác minh và hoàn thiện.  
Ví dụ:
    - merge/dev_lombok-refactoring
    - merge/combined-device-support
