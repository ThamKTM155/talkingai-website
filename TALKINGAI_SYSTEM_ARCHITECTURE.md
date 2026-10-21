
# TALKINGAI_SYSTEM_ARCHITECTURE.md

**Phiên bản:** 0.1  
**Ngày:** 22/10/2026  
**Trạng thái:** Bản thiết kế nền tảng, chưa phải kiến trúc triển khai hoàn chỉnh

## 1. Tầm nhìn và mục tiêu

TalkingAI được định hướng trở thành nền tảng trợ lý AI lâu dài cho gia đình, có khả năng mở rộng thành dịch vụ thương mại.

Các mục tiêu chính:

1. Mỗi thành viên có tài khoản, trợ lý và trí nhớ riêng.
2. Gia đình có kho tri thức chung với cơ chế chia sẻ được kiểm soát.
3. Dữ liệu riêng tư được bảo vệ bằng quyền truy cập được kiểm tra ở máy chủ.
4. Người dùng có thể xem, chỉnh sửa, xuất và yêu cầu xóa dữ liệu của mình theo chính sách.
5. Nền tảng có thể mở rộng số người dùng, mô hình AI và phương thức kinh doanh.
6. Mã nguồn, tài liệu, dữ liệu và quy trình vận hành có thể được bảo trì, sao lưu và bàn giao lâu dài.

## 2. Phạm vi và ranh giới hệ thống

### 2.1 TalkingAI

TalkingAI là sản phẩm độc lập, chịu trách nhiệm về:

- Giao diện người dùng.
- Tài khoản và xác thực.
- Phân quyền và quản lý nhóm gia đình.
- Trợ lý AI và điều phối công cụ.
- Trí nhớ cá nhân và trí nhớ chung.
- Quản lý dữ liệu, quyền riêng tư và kiểm toán.
- Các chức năng thương mại trong tương lai.

### 2.2 AutoYouTube

AutoYouTube tiếp tục là hệ thống riêng biệt.

Không gộp repository, mã nguồn, pipeline, token OAuth, dữ liệu sản xuất hoặc cấu hình vận hành AutoYouTube vào TalkingAI.

Việc tích hợp giữa hai hệ thống chỉ được xem xét khi có nhu cầu cụ thể, thiết kế giao diện tích hợp và đánh giá an toàn riêng.

### 2.3 MemoryAI

MemoryAI hiện có không bị sửa đổi trong phạm vi bản thiết kế này.

Khả năng tái sử dụng hoặc tích hợp MemoryAI trong tương lai phải được đánh giá dựa trên hợp đồng dữ liệu, quyền truy cập, tính tương thích và kiểm thử. Không mặc định rằng hai hệ thống đã được kết nối.

## 3. Kiến trúc logic mục tiêu

Các lớp logic dự kiến:

1. **Giao diện:** website, giao diện di động và bảng quản lý.
2. **API và xác thực:** tiếp nhận yêu cầu, xác thực danh tính và kiểm tra quyền.
3. **Điều phối AI:** quản lý hội thoại, lựa chọn mô hình và gọi công cụ được cho phép.
4. **Quản lý trí nhớ:** lưu, truy xuất, cập nhật, chia sẻ và xóa dữ liệu trí nhớ.
5. **Dữ liệu:** cơ sở dữ liệu, kho tệp, metadata và bản sao lưu.
6. **Bảo mật và kiểm toán:** nhật ký sự kiện quan trọng, giám sát và xử lý sự cố.
7. **Dịch vụ thương mại:** hạn mức, gói dịch vụ và thanh toán, triển khai ở giai đoạn sau.

Đây là các ranh giới logic. Chưa quyết định mỗi lớp phải là một dịch vụ triển khai riêng biệt.

## 4. Ba vùng dữ liệu

### 4.1 Trí nhớ cá nhân

Mỗi bản ghi phải gắn với chủ sở hữu và phạm vi truy cập.

Dữ liệu có thể gồm lịch sử hội thoại, thông tin người dùng chủ động lưu, tài liệu cá nhân và kiến thức được phép ghi nhớ.

Quy tắc:

- Mặc định chỉ chủ tài khoản được truy cập.
- Mọi thao tác đọc và ghi đều phải được kiểm tra quyền ở máy chủ.
- Không đưa dữ liệu cá nhân vào trí nhớ chung nếu chưa có quyền chia sẻ hợp lệ.
- Hỗ trợ xem, chỉnh sửa, xuất và yêu cầu xóa dữ liệu theo chính sách.
- Không tạo bằng chứng hoặc định danh nguồn giả khi nguồn không cung cấp định danh ổn định hay vị trí tham chiếu đáng tin cậy.

### 4.2 Trí nhớ chung gia đình

Chứa nội dung được chia sẻ có chủ đích, chẳng hạn câu chuyện gia đình, kinh nghiệm hoặc tài liệu chung.

Mỗi nội dung cần xác định:

- Nguồn và người đóng góp.
- Phạm vi chia sẻ.
- Người hoặc nhóm được phép truy cập.
- Thời điểm và trạng thái cho phép chia sẻ.
- Lịch sử cập nhật và cách xử lý khi quyền chia sẻ bị rút lại.

Việc chia sẻ một mục không đồng nghĩa chia sẻ toàn bộ lịch sử hội thoại của người đóng góp.

### 4.3 Kho tri thức nền tảng

Chứa tài liệu hướng dẫn TalkingAI, quy tắc vận hành và nội dung được phép sử dụng chung.

Kho này không được mặc nhiên chứa dữ liệu riêng của các thành viên hoặc khách hàng.

## 5. Danh tính và quyền hạn

Các vai trò mục tiêu:

- **Chủ sở hữu nền tảng:** quản lý sản phẩm, cấu hình, vận hành và kế hoạch bàn giao.
- **Quản trị viên gia đình:** quản lý thành viên và nội dung chung trong phạm vi được giao.
- **Thành viên:** quản lý không gian riêng và các quyền chia sẻ của mình.
- **Khách hàng thương mại:** sử dụng không gian dịch vụ theo tài khoản và gói đã đăng ký.

Quy tắc bắt buộc:

- Vai trò quản trị không tự động cấp quyền đọc trí nhớ cá nhân.
- Quyền phải được kiểm tra tại máy chủ, không chỉ dựa vào giao diện.
- Mỗi yêu cầu truy cập dữ liệu phải xác định danh tính, tài nguyên và quyền tương ứng.
- Nếu không thể xác minh quyền, hệ thống phải từ chối truy cập.
- Các trường hợp ngoại lệ phải được thiết kế, công bố và kiểm toán rõ ràng.

Việc sử dụng dịch vụ bởi trẻ vị thành niên cần có đánh giá riêng về độ tuổi, quyền riêng tư và sự đồng ý của người giám hộ khi pháp luật yêu cầu.

## 6. Bảo mật và quyền riêng tư

Các yêu cầu thiết kế:

1. Thu thập ít dữ liệu nhất có thể để thực hiện chức năng đã công bố.
2. Không lưu bí mật trong mã nguồn hoặc repository công khai.
3. Bảo vệ thông tin xác thực và token bằng cơ chế lưu trữ phù hợp.
4. Sử dụng kết nối bảo mật và phân quyền theo nguyên tắc tối thiểu.
5. Có nhật ký kiểm toán cho các thao tác nhạy cảm mà không ghi lộ nội dung bí mật.
6. Có quy trình sao lưu, phục hồi và xử lý sự cố.
7. Công bố rõ mục đích sử dụng, thời gian lưu giữ, chia sẻ và xóa dữ liệu.
8. Không dùng dữ liệu riêng cho mục đích khác ngoài phạm vi đã thông báo và được phép.
9. Xây dựng quy trình xử lý yêu cầu xuất dữ liệu, xóa dữ liệu và thu hồi quyền truy cập.
10. Kiểm thử cách ly dữ liệu giữa các thành viên trước khi mở dịch vụ cho người dùng bên ngoài.

## 7. AI và tích hợp dịch vụ ngoài

TalkingAI cần một lớp trung gian giữa ứng dụng và các nhà cung cấp AI.

Lớp này dự kiến quản lý:

- Cấu hình mô hình.
- Hạn mức và chi phí.
- Xử lý lỗi và thời gian chờ.
- Quyền sử dụng công cụ.
- Chính sách truyền dữ liệu ra bên ngoài.
- Ghi nhận mức sử dụng cần thiết cho vận hành và thanh toán.

Không mặc định kết nối Google, YouTube hoặc dịch vụ ngoài ngay từ phiên bản đầu tiên.

Mỗi tích hợp cần xác định mục đích, phạm vi OAuth, dữ liệu được truy cập, quyền người dùng và phương án thu hồi quyền.

## 8. Hạ tầng và lưu trữ

Các lựa chọn cần đánh giá khi triển khai:

- Website: Vercel.
- Mã nguồn và quản lý thay đổi: GitHub.
- Backend API: chọn sau khi xác định yêu cầu vận hành.
- Cơ sở dữ liệu: đánh giá PostgreSQL và cơ chế phân quyền phù hợp.
- Kho tệp: chọn dịch vụ có kiểm soát truy cập và chính sách lưu trữ.
- Bản sao lưu: quy định tần suất, thời gian lưu, quyền truy cập và kiểm thử phục hồi.

Không coi việc có bản sao lưu là đủ nếu chưa từng kiểm tra khả năng phục hồi.

## 9. Khả năng thương mại hóa

Các hướng có thể nghiên cứu:

- Gói miễn phí với hạn mức xác định.
- Gói cá nhân nâng cao.
- Gói gia đình.
- Gói dành cho khách hàng thương mại.

Trước khi triển khai thanh toán phải xác định chi phí mô hình AI, lưu trữ, băng thông, hỗ trợ, bảo mật, thuế và các nghĩa vụ pháp lý liên quan.

Không cam kết doanh thu hoặc lợi nhuận khi chưa có dữ liệu thực tế.

## 10. Tính liên tục và bàn giao thế hệ sau

TalkingAI cần có:

- Tài liệu kiến trúc và hướng dẫn vận hành.
- Quy trình sao lưu, phục hồi và xuất dữ liệu.
- Danh mục dịch vụ bên ngoài và chi phí liên quan.
- Quản lý quyền sở hữu tài khoản, miền và repository.
- Quy trình chuyển giao quyền quản trị an toàn.
- Hướng dẫn thay đổi nhà cung cấp AI hoặc hạ tầng khi cần.

Thông tin bí mật, mật khẩu và khóa khôi phục không được ghi trong tài liệu công khai.

## 11. Lộ trình BUILD

### BUILD-01: Website độc lập
Trạng thái: đã triển khai website giới thiệu TalkingAI trên Vercel.

### BUILD-02: Kiến trúc và chính sách
- Lưu tài liệu kiến trúc.
- Kiểm kê chức năng thực tế và luồng dữ liệu.
- Hoàn thiện chính sách quyền riêng tư phù hợp.
- Đánh giá các điều kiện OAuth cần thiết.

### BUILD-03: Tài khoản và kiểm soát truy cập
- Thiết kế mô hình tài khoản và nhóm.
- Xây dựng API xác thực.
- Kiểm thử quyền truy cập và cách ly dữ liệu.

### BUILD-04: Trí nhớ cá nhân và gia đình
- Thiết kế schema và nguồn gốc dữ liệu.
- Xây dựng cơ chế lưu, truy xuất, chia sẻ, cập nhật và xóa.
- Kiểm thử ranh giới quyền riêng tư.

### BUILD-05: Thương mại và bàn giao
- Hạn mức và gói dịch vụ.
- Thanh toán khi đã sẵn sàng.
- Sao lưu, phục hồi, xuất dữ liệu và bàn giao quyền quản trị.

Mỗi BUILD cần có phạm vi rõ ràng, tệp liên quan, bản sao lưu khi sửa mã, kiểm thử và tiêu chí PASS/FAIL.

## 12. Quy tắc thay đổi kiến trúc

- Không thay đổi cấu trúc hệ thống đang chạy khi chưa có lý do và đánh giá tác động.
- Ưu tiên thay đổi nhỏ, có thể kiểm tra và quay lại.
- Không coi tính năng đã đề xuất là tính năng đã triển khai.
- Không đánh dấu PASS khi chưa có bằng chứng kiểm thử.
- Mọi quyết định ảnh hưởng quyền riêng tư, dữ liệu hoặc chi phí phải được ghi lại.
- Giữ TalkingAI độc lập với AutoYouTube và MemoryAI cho tới khi có quyết định tích hợp cụ thể.

## 13. Trạng thái tài liệu

Tài liệu này là bản kiến trúc định hướng ban đầu. Các chi tiết như schema cơ sở dữ liệu, mô hình triển khai, nhà cung cấp xác thực và chính sách lưu giữ cần được xác nhận trước khi triển khai.

**Bước tiếp theo:** rà soát tài liệu, lưu vào GitHub và xây dựng danh mục yêu cầu cùng tiêu chí kiểm thử cho BUILD-03.
