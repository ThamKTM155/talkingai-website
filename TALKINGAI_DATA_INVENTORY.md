# TALKINGAI_DATA_INVENTORY.md

**Phiên bản:** 0.1  
**Ngày:** 22/10/2026  
**BUILD:** TALKINGAI-02B.1  
**Trạng thái:** Bản kiểm kê ban đầu, cần xác nhận bằng kiểm tra hệ thống thực tế.

## 1. Mục đích

Tài liệu này xác định các loại dữ liệu TalkingAI có thể xử lý, mục đích sử dụng, phạm vi truy cập và các yêu cầu bảo vệ dữ liệu.

Không coi tính năng dự kiến là tính năng đã triển khai. Mọi chức năng mới phải được rà soát trước khi đưa vào sử dụng.

## 2. Trạng thái chức năng hiện tại

| Chức năng | Trạng thái |
|---|---|
| Website giới thiệu TalkingAI | Đã triển khai |
| Tài liệu kiến trúc nền tảng | Đã tạo bản 0.1 |
| Đăng nhập và quản lý tài khoản | Chưa xác nhận đã triển khai |
| Trí nhớ AI cá nhân | Chưa xác nhận đã triển khai |
| Trí nhớ chung gia đình | Chưa xác nhận đã triển khai |
| Phân quyền nhiều thành viên | Chưa xác nhận đã triển khai |
| Kết nối Google hoặc YouTube | Chưa triển khai trong phạm vi website đã kiểm tra |
| Thanh toán và gói dịch vụ | Chưa xác nhận đã triển khai |
| Xuất và xóa dữ liệu người dùng | Chưa xác nhận đã triển khai |

## 3. Phân loại dữ liệu dự kiến

### 3.1. Dữ liệu tài khoản

Có thể gồm mã tài khoản, thông tin đăng nhập, email và tùy chọn cá nhân.

Yêu cầu:
- Chỉ thu thập thông tin cần thiết.
- Không lưu mật khẩu dạng văn bản thuần.
- Không công khai dữ liệu tài khoản.
- Chỉ nhân sự hoặc hệ thống được ủy quyền mới được xử lý dữ liệu cần thiết cho nhiệm vụ của họ.

### 3.2. Trí nhớ cá nhân

Có thể gồm thông tin do người dùng chủ động lưu, hội thoại được chọn để ghi nhớ, tài liệu cá nhân và tùy chọn trợ lý.

Yêu cầu:
- Mặc định chỉ chủ tài khoản được truy cập.
- Không tự động chia sẻ sang tài khoản khác.
- Kiểm tra quyền tại máy chủ mỗi khi đọc hoặc ghi.
- Hỗ trợ chỉnh sửa, xuất và yêu cầu xóa theo chính sách được công bố.

### 3.3. Trí nhớ chung gia đình

Chỉ chứa nội dung được phép chia sẻ vào không gian chung.

Mỗi mục cần xác định:
- Nguồn nội dung.
- Người đóng góp.
- Nhóm được phép truy cập.
- Quyền chỉnh sửa và quản lý.
- Trạng thái chia sẻ.
- Lịch sử cập nhật và cách xử lý khi quyền chia sẻ bị thu hồi.

Không suy diễn rằng quyền quản trị gia đình đồng nghĩa với quyền đọc toàn bộ trí nhớ cá nhân.

### 3.4. Tài liệu và tệp đính kèm

Nếu chức năng tải tệp được triển khai, cần xác định:
- Chủ sở hữu.
- Mục đích xử lý.
- Phạm vi truy cập.
- Thời gian lưu giữ.
- Cơ chế xóa và sao lưu.
- Các dịch vụ bên ngoài có thể nhận dữ liệu.

### 3.5. Dữ liệu vận hành và bảo mật

Có thể gồm thời điểm đăng nhập, sự kiện bảo mật, lỗi hệ thống và mức sử dụng dịch vụ.

Yêu cầu:
- Chỉ ghi những gì cần thiết.
- Không ghi mật khẩu, token hoặc khóa bí mật vào nhật ký.
- Giới hạn người có quyền đọc nhật ký.
- Xác định thời gian lưu giữ phù hợp.

### 3.6. Dữ liệu thương mại

Nếu triển khai thanh toán, có thể xử lý trạng thái gói dịch vụ, hạn mức, lịch sử giao dịch và thông tin cần thiết để hỗ trợ khách hàng.

Ưu tiên nhà cung cấp thanh toán phù hợp để giảm lượng dữ liệu thanh toán nhạy cảm mà TalkingAI trực tiếp lưu giữ.

## 4. Nguyên tắc phân quyền

1. Mặc định từ chối khi chưa xác minh được quyền.
2. Kiểm tra quyền tại máy chủ, không dựa riêng vào giao diện.
3. Tách dữ liệu theo người dùng và nhóm gia đình.
4. Không chia sẻ dữ liệu cá nhân nếu chưa có quyền hợp lệ.
5. Ghi nhật ký các thao tác nhạy cảm mà không làm lộ nội dung bí mật.
6. Kiểm thử truy cập chéo giữa hai tài khoản trước khi mở dịch vụ.
7. Việc xóa dữ liệu phải xem xét cả bản sao lưu, thời hạn lưu giữ hợp pháp và các bản sao do nhà cung cấp xử lý.

## 5. Vòng đời dữ liệu

Trước khi một chức năng được đưa vào hoạt động, phải xác định:

- Dữ liệu được tạo hoặc thu thập như thế nào.
- Mục đích sử dụng.
- Nơi lưu trữ và người được phép truy cập.
- Có truyền dữ liệu đến nhà cung cấp bên ngoài hay không.
- Thời gian lưu giữ.
- Cách chỉnh sửa, xuất, xóa và xử lý bản sao lưu.

Không công bố thời hạn lưu giữ cụ thể cho đến khi hệ thống thực tế hỗ trợ và thực hiện được thời hạn đó.

## 6. Các câu hỏi cần giải quyết trước khi phát hành

- Nhà cung cấp xác thực nào sẽ được sử dụng?
- Cơ sở dữ liệu và kho tệp sẽ đặt ở đâu?
- Hội thoại nào được lưu và trong bao lâu?
- Người dùng kiểm soát trí nhớ của mình bằng cách nào?
- Quyền quản trị gia đình được giới hạn ra sao?
- Dữ liệu được truyền tới nhà cung cấp AI nào?
- Có sử dụng dữ liệu cho huấn luyện hoặc cải thiện dịch vụ không?
- Quy trình xử lý yêu cầu xuất và xóa dữ liệu hoạt động thế nào?
- Có cơ chế sao lưu và kiểm thử phục hồi chưa?
- Những yêu cầu pháp lý nào áp dụng cho từng nhóm người dùng?

## 7. Tiêu chí hoàn thành BUILD-02B

- Có danh mục các loại dữ liệu và mục đích xử lý.
- Phân biệt rõ chức năng hiện có và chức năng dự kiến.
- Mỗi loại dữ liệu có phạm vi truy cập dự kiến.
- Xác định các câu hỏi chưa có câu trả lời.
- Chưa đưa ra tuyên bố quyền riêng tư vượt quá khả năng thực tế.
- Chưa thay đổi mã nguồn ứng dụng trong BUILD kiểm kê này.

## 8. Bước tiếp theo

Rà soát nội dung website và repository, xác nhận chức năng thực tế, sau đó cập nhật tài liệu trước khi thiết kế chính sách quyền riêng tư chính thức.
