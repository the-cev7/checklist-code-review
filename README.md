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

# :heavy_check_mark: [6] Self-describing test method
> Unit Test method names must be self-describing
>
> Also focus on naming style, keep the naming style consistent across all the test methods and tests.

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Test case là tài liệu
- Đọc vào tên method test có thể biết mục đích của test case

**Thực hiện**
- Tên test method không cần phải quá ngắn gọn
- Tên test method phải chỉ ra điều kiện và expect của test case
- Thống nhất convention trong project, mặc định visibility của 1 method trong class là `public` nên có thể loại bỏ từ `public` trong method test

Chọn một trong các convention sau:
1. **[Recommend]** Sử dụng prefix `test_`

    ```php
    function test_it_returns_false_when_input_number_is_odd()
    ```
2. Sử dụng annotation `@test` thì tên test method không cần phải bắt đầu bằng `test_`

    ```php
    /* @test */
    function it_returns_false_when_input_number_is_odd()
    ```
3. Sử dụng `camelCase` thay cho `snake_case`, chỉ nên sử dụng nếu trong project đã viết theo cách này trước đó

    ```php
    function testItReturnsFalseWhenInputNumberIsOdd()
    ```

**FAQ**
1. Q: Tên test method có cần bao gồm tên method của class đang test không?

    A: Có thể. Nhưng tốt hơn là xem class là Unit cần test, khi đó ta đang đi test chức năng hay hành vi của class, khi thực hiện refactor có thể thay đổi tên method nhưng không cần thay đổi tên test method
</details>


# :heavy_check_mark: [7] A3 (Arrange, Asset, Act)
A3 (Arrange, Asset, Act)
- Arrange: thiết lập trạng thái, khởi tạo object, giả lập mock
- Act: Chạy unit đang cần test (method under test)
- Assert: So sánh expected với kết quả trả về

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Nội dung test method rõ ràng dễ đọc, dễ viết

**Thực hiện**
- Chia nội dung test làm 3 phần
    ```php
    function test_validation_failed_when_value_exceed_max_length()
    {
        // Arrange
        $username = str_pad('a', UsernameValidation::MAX_LENGTH + 1);

        // Act
        $validation = new UsernameValidation;
        $isValidUsername = $validation->isValid($username);

        // Assert
        $this->assertFalse($isValidUsername);
    }
    ```
- Ngoại lệ khi test một method throw exception, do vấn đề kỹ thuật nên phải gọi `expectionException()` trước khi gọi method:
    ```php
    function test_it_throws_exception_when_input_is_not_a_number()
    {
        // Assert that
        $this->expectException(InvalidArgumentException::class);

        // Arrange
        $calculator = new Calculator;
        $input1 = 'i am a string';
        $input2 = 100;

        // Act
        $calculator->add($input1, $input2);
    }
    ```
</details>

# :heavy_check_mark: [8] Use sematic/proper assert method
> Keep assert method descriptive. Use proper assert method to improve the readability of code and the error log.

<details>
    <summary>:point_right: Chi tiết</summary>

Thực tế ta có thể chỉ dùng `assertTrue()`:

```php
// assertEquals
$this->assertTrue($expected == $actual);
// Failed asserting that false is true.
// vs. Failed asserting that $actual matches expected $expected.

// assertSame
$this->assertTrue($expected === $actual);

// assertContains
$this->assertTrue(in_array($actual, $expected);

// assertCount
$this->assertTrue(count($actual) == $expected);

// assertInstanceOf
$this->assertTrue($actual instanceOf ExpectedClass);
```

Nhưng việc dùng method assert thích hợp giúp cho việc đọc hiểu dễ hơn (không phải thực hiện phép so sánh) và message được generate dễ hiểu hơn nếu test case failed.
</details>

# :heavy_check_mark: [9] If you write code, write tests

<details>
    <summary>:point_right: Chi tiết</summary>

**Thực hiện**

Mọi PR đều phải chú ý đến test
- PR thêm feature => viết test cho feature mới
- PR fix bug => viết test để tránh bug xảy ra 1 lần nữa
- PR refactor => chạy, update test để đảm bảo không phát sinh ảnh hưởng
- Nên tích hợp CI để chạy test

**FAQ**:
1. Q: Thời điểm tốt nhất để viết test?

    A: Thời điểm tốt nhất là khi code còn mới! Thời điểm mà cả code và test đều có thể dễ dàng thay đổi. Tưởng tượng code giống như _đất sét_, khi còn mới thì nó mềm và dễ nặn, nếu để lâu thì nó sẽ cứng và dễ vỡ :smile:
</details>

# :heavy_check_mark: [10] My tests are fast!

<details>
    <summary>:point_right: Chi tiết</summary>

**Thực hiện**
- Ngoài việc chú trọng vào việc viết test case đúng, cần chú ý đến thời gian chạy test
- Hạn chế test database (integration), và nếu có thể thì dùng sqlite in-memory làm database test
- Test không gọi network hay api service ngoài
- Khi test làm việc với file, cân nhắc sử dụng [vfsStream](https://github.com/bovigo/vfsStream)
- Khi chạy phpunit để generate code coverage, sử dụng 1 trong 3 driver sau theo thứ tự ưu tiên từ trên xuống
    + `pcov` nếu PHPUnit version >= 8
        ```sh
        php -dextension=pcov.so -dpcov.enabled=1 -dpcov.directory=app ./vendor/bin/phpunit --coverage-text
        ```
        NOTE: `pcov.directory=app`, trong đó `app` là thư mục chứa source code
    + `phpdbg`
        ```sh
        phpdbg -qrr ./vendor/bin/phpunit --coverage-text
        ```
    + XDebug
        ```sh
        php -dzend_extension=xdebug.so ./vendor/bin/phpunit --coverage-text
        ```

**Tại sao?**
- Bạn sẽ phải chạy tests thường xuyên, lặp lại => Nếu tests chạy quá chậm sẽ làm ảnh hưởng đến tiến độ, tinh thần làm việc
- Dự án áp dụng CI để build, test và deploy => Nếu tests chạy quá chậm sẽ dẫn đến việc tích hợp cho cả team bị chậm. Thời gian build của CI mà quá 5 phút thì khó mà chấp nhận được
</details>

# :heavy_check_mark: [11] Quality over code coverage number!

<details>
    <summary>:point_right: Chi tiết</summary>

**Sự thật về code coverage**
- Không cần viết test đúng vẫn có thể đạt 100% coverage!
- Có trường hợp đã đạt 100% coverage rồi nhưng vẫn có khả năng lọt bug vì thiếu test case

=> Tham khảo thêm cách phpunit tính coverage => [link](https://github.com/sun7pro/phpunit-training-coverage/issues/2)

**Thực hiện**
- Chú trọng vào chất lượng test case, viết sao cho đủ test case? làm sao để test chạy nhanh hơn? làm sao để viết test dễ hơn, refactor code?
- Áp dụng mutation testing vào dự án nếu có thể, để có chỉ số đánh gía tốt hơn => [link](https://medium.com/@maks_rafalko/infection-mutation-testing-framework-c9ccf02eefd1)
</details>

# :heavy_check_mark: [12] Validate all data sent from the client

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Đảm bảo tất cả các dữ liệu gửi lên từ client đều được validate
- Tránh tình trạng người dùng sử dụng tool để chèn thêm các data không mong muốn
- Chỉ lấy các data có trong whitelist của client gửi lên, không lấy data ngoài whitelist

**Thực hiện**
- khi validate thì cần thực hiện validate các field có trong spec hoặc docs api
- Nếu là file thì cần validate mimes, type thật của file
- Sau khi validate chỉ được sử dụng các data đã được validate, đảm bảo không lấy các files chưa được validate

</details>

# :heavy_check_mark: [13] Output has no information about SQL query

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Đảm bảo không để lộ câu sql ra bên ngoài
- Tránh việc người dùng có thể thấy được câu sql
- Trong các đoạn xử lý exception thì ko throw ra sql nếu có

**Thực hiện**
- Khi code cần chú ý các đoạn xử lý try catch
- Kiểm tra với các dk lỗi xem sql có show ra bên ngoài hay không

</details>

# :heavy_check_mark: [14] The orWhere query must be placed in the group

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Đảm bảo câu query phải đúng tránh trường hợp orWhere loại bỏ các dk khác
- Tránh tình trạng lấy nhầm dữ liệu do câu orWhere gây ra làm lộ thông tin

**Thực hiện**
- Khi thực hiện câu truy vấn có orWhere phải đặt nó trong dấu `( )`
- Nếu sử dụng ORM thì cần phải show câu sql raw ra để mọi người cùng review

</details>

# :heavy_check_mark: [15] The file upload must be unique name

<details>
    <summary>:point_right: Chi tiết</summary>

**Mục đích:**
- Đảm bảo các file được upload phải có tên duy nhất
- Tránh tình trạng upload file trùng tên ghi đè mất file và lấy nhầm file gây ra lộ thông tin

**Thực hiện**
- Khi thao tác tới hàm upload file thì cần phải sử dụng uuid hoặc 1 chuỗi string unique kèm theo timestamp
- File name nên đảm bảo có độ dài từ 150 character trở lên
- File name nên đặt trong folder theo user_id hoặc id định danh của người thực hiện upload file

</details>


# :heavy_check_mark: [16] Resolved n + 1 query

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

# :heavy_check_mark: [17] Time open page < 1000 ms

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
