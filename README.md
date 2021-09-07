# :heavy_check_mark: [1] Pull request has been self-reviewed

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Pull request phải được self review kỹ trước khi đẩy pull
- Phải pass normal case trước khi được merged vào
- Không xảy ra các lỗi syntax cơ bản

**Thực hiện**
- Tại local update code mới nhất (merge branch base về `feature.xxx`).
- Thực hiện việc self test check các case normal (ít nhất phải pass chức năng đang làm).
- Sử dụng git diff để kiểm tra lại các đoạn code thay đổi có syntax nào sai không.

**FAQ**
1. Q: Việc test đã có QA lo vậy dev có cần phải self test không?

    A: Có! Nếu dev không self test thì khi assign ticket sang cho QA, sẽ phát sinh ra các bug normal, Thời gian log bug và fix bug cho những bug normal này sẽ tốn rất nhiều thời gian.
</details>

# :heavy_check_mark: [2] Pull request has been write Unit Test

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Để đảm bảo pull request không bị ảnh hưởng logic khác thì cần phải viết UT trong pull

**Thực hiện**
- Viết UT cho chức năng đang code
- UT phải có coverage > 80%

**FAQ**
1. Q: Nếu thời gian làm ticket lớn hơn trong estimate và khách hàng yêu cầu gấp thì có cần viết UT không?

    A: Nếu dự án đang trong giai đoạn căng thẳng, cần đẩy pull gấp thì có thể không check và ticket sẽ chỉ được để 80%.
</details>

# :heavy_check_mark: [3] Check impacted areas

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Khi đẩy pull request lên phải nắm được những vùng ảnh hưởng
- Cho người khác biết được các vùng ảnh hưởng trong pull để review dễ hơn

**Thực hiện**
- Kiểm tra các file thay đổi trước khi đẩy pull
- Trong ticket phải notes chi tiết các màn hình có ảnh hưởng cho QA hiểu được

**FAQ**
1. Q: Làm thế nào notes ra ảnh hưởng hiệu quả?

    A: Nhìn vào các file controller, Gạch đầu dòng các màn hình tương ứng, khu vực tương ứng để QA hoặc dev có thể review kỹ khu vực đó.
</details>

# :heavy_check_mark: [4] Pull request has a meaningful description of its purpose

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Phải mô tả chức năng mình đang làm
- Phải mô tả các setting, cài đặt, chạy seeder,... để khi deployment có thể biết được

**Thực hiện**
- Notes nội dung trong ticket cần làm
- Nếu logic phức tạp có thể vẽ sequence và uplen
- Nếu trong pull cần phải config server, app thì phải note

**FAQ**:
1. Q: Tại sao lại phải notes khi trong ticket có rồi?

    A: Để phục vụ cho việc deployment và điều tra dễ dàng hơn thì trong pull request phải có notes. Có thể copy từ ticket sang.
</details>

# :heavy_check_mark: [5] All commits are accompanied by meaningful commit messages

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Commit phải đánh số ID của ticket
- Commit phải có ý nghĩa để có thể trace lại lịch sử
- Nếu commit không có ID của ticket thì sau này sẽ ko biết được tại sao lại xử lý code như vậy.

**Thực hiện**
- Khi commit code phải đính kèm ID ticket (ex: refs #12345 Change text username for user detail screen)
- Nếu commit nhầm phải update lại đúng ID của ticket
- 1 Pull request có thể có nhiều commit nhưng bắt buộc mỗi commit phải có ID của ticket và notes lại rõ ràng.

**FAQ**:
1. Q: Nếu không notes thì có sao không?

    A: Nếu không notes lại thì sau này khi nhìn vào code sẽ ko hiểu được tại sao lại code logic như vậy
</details>

# :heavy_check_mark: [6] Resolved n + 1 query

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Đảm bảo hệ thống hoạt động performance tốt
- Tránh việc xử lý quá nhiều query trên 1 màn hình
- Nếu không tối ưu query thì dữ liệu lớn sẽ không chịu được tải

**Thực hiện**
- Khi code trong vòng lặp cần phải chú ý, mỗi vòng lặp không nên query thêm nữa vào DB
- Dùng tool hoặc logs để kiểm tra các query tại màn hình cần làm
- Hiểu rõ câu query đang thực hiện kể cả dùng ORM
- Dựa vào các query đó suy nghĩ xem có thể tối ưu thêm được nữa không.

**FAQ**:
1. Q: Nếu logic phải xử dụng query n, n+1 thì phải làm thế nào?

    A: Cần dump ra câu SQL raw, sau đó nhờ mọi người review support, nếu không tìm được giải phải thì cần trao đổi lại spec đặt limit nhỏ hơn.
</details>

# :heavy_check_mark: [7] Time open page < 1000 ms

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Đảm bảo hệ thống khi scale lên vẫn chạy ổn định
- Nếu thời gian load màn hình lâu sẽ khiến người sử dụng không hài lòng
- Nếu thời gian load màn hình lâu đồng nghĩa với việc code chưa được tối ưu

**Thực hiện**
- Đo đạc thời gian của 1 request
- Kiểm tra thời gian thực hiện câu query
- Kiểm tra lại các vòng lặp logic xử lý
- Sử dụng quá nhiều vòng lặp, điều kiện cũng ảnh hưởng tới thời gian load

**FAQ**:
1. Q: Thời gian này là đo ở local phải không?

    A: Chính xác, tại local dữ liệu không nhiều, nếu thời gian > 1000ms có khả năng lên production sẽ còn mất nhiều thời gian hơn.
</details>

# Code review
Tham khảo https://github.com/sun7pro/.github/blob/master/PULL_REQUEST_TEMPLATE.md để áp dụng pull request template cho dự án.
