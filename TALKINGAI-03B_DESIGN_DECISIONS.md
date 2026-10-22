TALKINGAI-03B_DESIGN_DECISIONS.md
Phiên bản: 0.1
Ngày: 22/10/2026
BUILD: TALKINGAI-03B
Trạng thái: Bản đề xuất chốt thiết kế; chưa triển khai mã nguồn
Repository: ThamKTM155/talkingai-website
Tài liệu nền: TALKINGAI_ACCOUNT_MEMORY_DESIGN.md (TALKINGAI-03A)
1. Mục tiêu
Cụ thể hóa các quyết định còn mở trong TALKINGAI-03A trước khi viết backend. BUILD này chỉ bổ sung tài liệu thiết kế; không sửa index.html, không tạo backend, không kết nối Google/YouTube, không thay đổi AutoYouTube hoặc MemoryAI.

Các quyết định dưới đây là đề xuất để người sáng lập duyệt, chưa phải chức năng đã triển khai hoặc kiểm thử.
2. Quyết định thiết kế đề xuất
D-01. Xác thực người dùng
- Dùng nhà cung cấp xác thực được quản lý thay vì tự viết hệ thống mật khẩu.
- Backend xác minh phiên/token trước khi xử lý dữ liệu riêng tư.
- account_id nội bộ là định danh chính; không dùng email làm khóa chính.
- Liên kết Google để đăng nhập và cấp quyền YouTube là hai luồng độc lập.
- Chưa chọn nhà cung cấp cụ thể. Cần so sánh chi phí, bảo mật, khả năng xuất dữ liệu, lưu giữ và khả năng chuyển đổi.
D-02. Cơ sở dữ liệu
- Ưu tiên cơ sở dữ liệu quan hệ có khóa ngoại, giao dịch, chỉ mục và khả năng kiểm soát quyền.
- Chưa chọn nhà cung cấp cụ thể.
- Không lưu dữ liệu tài khoản/trí nhớ trong tệp tĩnh website hoặc repository GitHub.
- Không đưa dữ liệu thật, token, mật khẩu hoặc bí mật vào GitHub.
- Mọi bản ghi trí nhớ phải có chủ sở hữu hoặc phạm vi nhóm rõ ràng.
D-03. Ba vùng dữ liệu
1. Trí nhớ cá nhân: thuộc owner_account_id; mặc định chỉ chủ sở hữu truy cập.
2. Trí nhớ gia đình: thuộc family_id; chỉ thành viên có quyền phù hợp truy cập.
3. Tri thức nền tảng: tài liệu dùng chung được phép sử dụng; không tự động nhập trí nhớ riêng.
Không dùng một cờ is_shared duy nhất để thay thế đối tượng nhận và quyền cụ thể.
D-04. Quyền truy cập
- Backend xác thực danh tính và kiểm tra quyền trên từng thao tác.
- Không tin account_id, family_id hoặc vai trò do trình duyệt gửi lên nếu chưa xác minh.
- Quyền đọc, tạo, sửa, xóa, chia sẻ, thu hồi, xuất và quản trị là các quyền riêng.
- Quản trị viên gia đình không mặc nhiên đọc trí nhớ cá nhân.
- Người sáng lập/quản trị nền tảng không mặc nhiên đọc trí nhớ riêng.
- Dịch vụ chỉ được cấp quyền tối thiểu cần thiết.
- Khi không xác minh được quyền, phải từ chối yêu cầu.
D-05. Vai trò gia đình
Vai trò đề xuất:
- owner: quản lý nhóm và thành viên theo chính sách.
- member: thành viên thông thường.
- viewer (nếu cần): chỉ xem nội dung gia đình được phép chia sẻ.
Vai trò không tự động cấp quyền đọc mọi trí nhớ cá nhân. Quyền từng bản ghi vẫn phải được kiểm tra riêng.
D-06. Chia sẻ trí nhớ
Mỗi lần chia sẻ phải xác định bản ghi, người chia sẻ, nhóm nhận, phạm vi quyền, thời điểm cấp và trạng thái thu hồi. Chia sẻ là hành động chủ động, không tự động đưa hội thoại hoặc trí nhớ cá nhân vào kho gia đình.

Thu hồi phải chặn truy cập mới không còn được phép. Chính sách phải mô tả riêng cache, bản xuất và bản sao lưu; không tuyên bố thu hồi sẽ tự động xóa mọi bản sao đã tồn tại.
D-07. Vòng đời và xóa dữ liệu
- Xác thực và kiểm tra quyền khi nhận yêu cầu xóa hoặc xuất.
- Phân biệt dữ liệu đang hoạt động, nhật ký và bản sao lưu.
- Giao diện phải nói rõ trạng thái yêu cầu xóa và giới hạn của quy trình.
- Không tuyên bố xóa hoàn toàn tức thời nếu bản sao lưu còn tồn tại theo lịch lưu giữ.
- Khôi phục không được làm sống lại quyền thành viên đã bị thu hồi.
D-08. Nhật ký kiểm toán
Ghi các thao tác nhạy cảm như thay đổi vai trò, mời/loại thành viên, chia sẻ/thu hồi, xuất/xóa và hành động quản trị. Không ghi mật khẩu, access token, refresh token, client secret hoặc toàn bộ nội dung trí nhớ riêng.
D-09. Trò chuyện AI theo giai đoạn
- Giai đoạn A: chỉ người sáng tạo được dùng chức năng thử nghiệm AI.
- Giai đoạn B: chỉ thành viên gia đình được mời và được cho phép.
- Giai đoạn C: chỉ mở rộng bên ngoài sau khi qua cổng phát hành và đánh giá bảo mật, quyền riêng tư, chi phí, hỗ trợ.
Ẩn nút trên giao diện không đủ; backend phải từ chối yêu cầu ngoài phạm vi.
D-10. Google và YouTube
- Đăng nhập Google không tự động cấp quyền YouTube.
- Chỉ xây dựng kết nối YouTube khi có nhu cầu và thiết kế luồng đồng ý, phạm vi quyền, lưu token và thu hồi riêng.
- Không lưu token OAuth trong GitHub, mã frontend hoặc nhật ký.
- OAuth YouTube không thuộc phạm vi BUILD-03B.
3. Mô hình dữ liệu logic
Thực thể	Mục đích	Quan hệ bắt buộc
accounts	Hồ sơ tài khoản nội bộ	account_id duy nhất
auth_identities	Danh tính nhà cung cấp đăng nhập	Gắn với một tài khoản
family_groups	Nhóm gia đình	family_id duy nhất
family_memberships	Thành viên và vai trò	Gắn family_id với account_id
personal_memories	Trí nhớ cá nhân	Có owner_account_id
family_memories	Trí nhớ chia sẻ trong nhóm	Có family_id và người đóng góp
memory_sharing_grants	Quyền chia sẻ có thể thu hồi	Gắn bản ghi, nhóm nhận và quyền
audit_events	Nhật ký thao tác nhạy cảm	Không chứa bí mật
data_export_requests	Yêu cầu xuất dữ liệu	Gắn người yêu cầu và trạng thái
data_deletion_requests	Yêu cầu xóa dữ liệu	Gắn người yêu cầu và trạng thái


Đây là mô hình logic, chưa phải schema SQL. Trước khi viết migration phải chốt kiểu dữ liệu, khóa ngoại, ràng buộc, chỉ mục, giao dịch và chính sách truy cập.
4. Tiêu chí kiểm thử bắt buộc
Tài khoản và phiên
- Từ chối yêu cầu không có phiên hợp lệ.
- Từ chối phiên hết hạn hoặc bị thu hồi.
- Không dùng email làm định danh chính.
- Không cho phép giả mạo account_id.
Cách ly trí nhớ cá nhân
Dùng tối thiểu hai tài khoản thử nghiệm A và B:
- A đọc/sửa/xóa/xuất dữ liệu của A theo quyền.
- B không thể đọc/sửa/xóa/xuất dữ liệu của A.
- Thay ID trong URL, body hoặc query không vượt qua kiểm soát.
- Lỗi không tiết lộ nội dung riêng của A.
Cách ly nhóm gia đình
- Thành viên nhóm A không truy cập nhóm B nếu chưa có quyền.
- Người ngoài nhóm không đọc dữ liệu nhóm.
- Quản trị viên gia đình không tự động đọc trí nhớ cá nhân.
- Nội dung chưa chia sẻ không xuất hiện trong vùng gia đình.
- Thu hồi thành viên hoặc quyền chia sẻ chặn truy cập mới.
- Không thể tự nâng vai trò bằng cách sửa request.
Vòng đời, nhật ký và bí mật
- Chia sẻ cần quyền hợp lệ; thu hồi được áp dụng ở backend.
- Bản xuất chỉ chứa dữ liệu người yêu cầu được phép lấy.
- Xóa và sao lưu tuân theo chính sách đã công bố.
- Khôi phục không làm sống lại quyền đã thu hồi.
- Không có mật khẩu/token/client secret trong log hoặc repository.
- Nhật ký đủ kiểm toán nhưng không sao chép nội dung riêng không cần thiết.
Điều kiện PASS
Mỗi bài kiểm thử phải ghi tên, môi trường, kết quả mong đợi, kết quả thực tế và bằng chứng đã loại bỏ dữ liệu nhạy cảm. Không đánh dấu PASS khi chưa chạy. Bất kỳ lỗi truy cập chéo hoặc rò rỉ bí mật nào cũng chặn việc chuyển giai đoạn.
5. Cổng triển khai tiếp theo
Không viết backend trước khi người sáng lập duyệt các quyết định D-01 đến D-10, đặc biệt:
1. Nhà cung cấp xác thực cụ thể.
2. Cơ sở dữ liệu và nơi triển khai.
3. Quyền chính xác của từng vai trò.
4. Quy tắc lưu giữ và xóa dữ liệu.
5. Phạm vi thử nghiệm AI của giai đoạn A.
Sau khi duyệt, BUILD tiếp theo mới lập kế hoạch schema cơ sở dữ liệu và bộ kiểm thử quyền truy cập.
6. Phạm vi không thuộc BUILD-03B
- Không sửa index.html.
- Không triển khai backend hoặc cơ sở dữ liệu.
- Không tạo tài khoản người dùng thật.
- Không kết nối Google/YouTube hoặc lưu OAuth token.
- Không thay đổi AutoYouTube hoặc MemoryAI.
- Không tuyên bố chức năng tài khoản/trí nhớ đã hoạt động.
Kết luận: TALKINGAI-03B chuyển nguyên tắc của TALKINGAI-03A thành các quyết định thiết kế có thể duyệt và kiểm thử. Đây vẫn là tài liệu thiết kế, chưa phải bằng chứng triển khai.
