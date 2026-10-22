# TALKINGAI_ACCOUNT_MEMORY_DESIGN.md

**Phiên bản:** 0.1  
**Ngày:** 22/10/2026  
**BUILD:** TALKINGAI-03A  
**Trạng thái:** Thiết kế nền tảng, chưa triển khai mã nguồn  
**Repository:** `ThamKTM155/talkingai-website`

---

## 1. Mục đích

Tài liệu xác định thiết kế ban đầu cho hệ thống tài khoản, trí nhớ cá nhân và trí nhớ gia đình của TalkingAI.

Mục tiêu là xây dựng một nền tảng có thể phát triển từ thử nghiệm của người sáng tạo, đến thử nghiệm riêng tư trong gia đình, rồi mới mở rộng thành dịch vụ thương mại.

Các yêu cầu cốt lõi:

1. Mỗi thành viên có danh tính và không gian dữ liệu riêng.
2. Trí nhớ cá nhân không tự động trở thành trí nhớ chung.
3. Trí nhớ gia đình chỉ chứa nội dung được chia sẻ theo quyền.
4. Mọi quyền truy cập dữ liệu phải được kiểm tra ở backend.
5. Người dùng có thể quản lý dữ liệu theo chính sách đã công bố.
6. Hệ thống có thể kiểm thử, sao lưu, phục hồi và mở rộng.
7. Không tích hợp hoặc sửa đổi AutoYouTube hay MemoryAI trong BUILD này.

Tài liệu mô tả trạng thái mục tiêu. Không coi bất kỳ chức năng nào là đã triển khai nếu chưa có mã nguồn và kết quả kiểm thử chứng minh.

## 2. Lộ trình người dùng

### Giai đoạn A: Người sáng tạo thử nghiệm

Đối tượng: người sáng tạo TalkingAI và các tài khoản được cho phép phục vụ kiểm thử.

Phạm vi:

- Xây dựng cơ chế tài khoản.
- Xây dựng vùng dữ liệu cá nhân.
- Kiểm tra tạo, đọc, cập nhật và xóa dữ liệu.
- Chỉ cho phép người sáng tạo sử dụng chức năng trò chuyện AI thử nghiệm.
- Chưa mở chức năng trò chuyện AI cho mọi tài khoản.
- Chưa cho người ngoài đăng ký sử dụng công khai.

Việc hạn chế trò chuyện phải được thực thi ở backend. Ẩn nút trò chuyện trên giao diện không được xem là biện pháp bảo vệ đầy đủ.

### Giai đoạn B: Thử nghiệm riêng tư trong gia đình

Chỉ bắt đầu sau khi các kiểm thử bảo mật và cách ly dữ liệu ở giai đoạn A đạt yêu cầu.

Phạm vi:

- Mời thành viên tham gia.
- Mỗi thành viên có tài khoản riêng.
- Mỗi thành viên có vùng trí nhớ riêng.
- Tạo vùng trí nhớ gia đình.
- Cho phép chia sẻ có chủ đích.
- Kiểm tra việc thu hồi quyền và xử lý thành viên rời nhóm.

Không mặc định mọi thành viên đều có quyền đọc mọi nội dung chung. Quyền đọc, đóng góp, chỉnh sửa và quản trị phải được xác định riêng.

### Giai đoạn C: Mở rộng ra bên ngoài

Chỉ triển khai sau khi có đánh giá riêng về:

- Bảo mật và quyền riêng tư.
- Đăng ký và xác thực người dùng.
- Điều khoản dịch vụ và chính sách dữ liệu.
- Hạ tầng, chi phí và hỗ trợ.
- Các yêu cầu pháp lý và OAuth áp dụng.
- Khả năng sao lưu, phục hồi và xử lý sự cố.

Đây là cổng phát hành có điều kiện, không phải thời hạn tự động.

## 3. Mô hình tài khoản

### 3.1. Account

Đại diện cho một danh tính người dùng trong TalkingAI.

Các trường logic dự kiến:

- `account_id`: định danh nội bộ duy nhất.
- `status`: trạng thái tài khoản.
- `created_at`: thời điểm tạo.
- `updated_at`: thời điểm cập nhật.
- `deleted_at`: thời điểm bắt đầu quy trình xóa, nếu áp dụng.

Email, tên hiển thị và phương thức xác thực được thiết kế thành các thuộc tính riêng phù hợp với nhà cung cấp xác thực đã chọn.

Không sử dụng email làm khóa chính vì email có thể thay đổi.

Không lưu mật khẩu dạng văn bản thuần. Ưu tiên nhà cung cấp xác thực đã được đánh giá phù hợp, thay vì tự xây dựng hệ thống mật khẩu từ đầu.

### 3.2. Authentication Identity

Một tài khoản có thể liên kết với phương thức xác thực được hỗ trợ.

Thông tin liên kết dự kiến:

- Nhà cung cấp xác thực.
- Định danh người dùng do nhà cung cấp cấp.
- Trạng thái liên kết.
- Thời điểm liên kết.

Việc đăng nhập bằng Google và việc cấp quyền truy cập YouTube là hai mục đích khác nhau. Đăng nhập Google không tự động cấp quyền YouTube.

Không lưu token OAuth trong mã nguồn, tệp công khai hoặc nhật ký.

### 3.3. Family Group

Đại diện cho một nhóm gia đình.

Các trường logic:

- `family_id`: định danh nhóm.
- `name`: tên nhóm.
- `status`: trạng thái nhóm.
- `created_at`: thời điểm tạo.

Một tài khoản có thể được tham gia nhóm theo chính sách đã thiết kế. Quan hệ tài khoản và nhóm được quản lý bằng bảng thành viên riêng, không gộp quyền nhóm vào dữ liệu trí nhớ cá nhân.

### 3.4. Family Membership

Lưu quan hệ giữa tài khoản và nhóm gia đình.

Các trường logic:

- `family_id`.
- `account_id`.
- `role`.
- `membership_status`.
- `joined_at`.
- `revoked_at`, nếu quyền thành viên bị thu hồi.

Vai trò thành viên không tự động cấp quyền truy cập vào mọi dữ liệu của nhóm hoặc mọi trí nhớ cá nhân.

## 4. Mô hình trí nhớ

TalkingAI sử dụng ba vùng dữ liệu logic riêng biệt.

### 4.1. Personal Memory

Trí nhớ cá nhân thuộc quyền kiểm soát của một tài khoản.

Các trường logic:

- `memory_id`: định danh bản ghi.
- `owner_account_id`: tài khoản sở hữu.
- `memory_type`: loại trí nhớ.
- `content`: nội dung hoặc tham chiếu đến nội dung.
- `source_reference`: tham chiếu nguồn đáng tin cậy nếu có.
- `created_at`.
- `updated_at`.
- `deleted_at`, nếu áp dụng.

Nguyên tắc:

1. Mặc định chỉ chủ sở hữu được truy cập.
2. Không cho phép thành viên khác đọc trực tiếp.
3. Không tự động sao chép sang trí nhớ gia đình.
4. Chia sẻ phải là thao tác riêng, có kiểm tra quyền.
5. Phải hỗ trợ quản lý và xử lý yêu cầu xóa theo chính sách.
6. Chỉ ghi nhớ dữ liệu phù hợp với mục đích và sự cho phép của người dùng.

Không tạo `evidence_id` hoặc vị trí nguồn giả khi bản ghi nguồn không cung cấp định danh ổn định hay vị trí tham chiếu đáng tin cậy.

### 4.2. Family Memory

Trí nhớ gia đình chứa nội dung được cho phép chia sẻ vào nhóm.

Các trường logic:

- `memory_id`.
- `family_id`.
- `contributor_account_id`.
- `memory_type`.
- `content` hoặc tham chiếu nội dung.
- `source_reference`, nếu có.
- `created_at`.
- `updated_at`.
- `sharing_status`.

Mỗi bản ghi phải gắn với nhóm cụ thể.

Không dùng một cờ đơn giản như `is_shared = true` để thay thế toàn bộ mô hình quyền truy cập. Hệ thống phải xác định nội dung được chia sẻ với nhóm nào và ai có quyền thực hiện từng thao tác.

### 4.3. Memory Sharing Grant

Việc chia sẻ từ trí nhớ cá nhân sang nhóm gia đình được xem là một hành động có quyền riêng.

Thông tin logic cần có:

- Đối tượng được chia sẻ.
- Chủ sở hữu hoặc người có quyền chia sẻ.
- Nhóm nhận.
- Phạm vi quyền được cấp.
- Thời điểm cấp quyền.
- Trạng thái quyền.
- Thời điểm thu hồi, nếu có.

Chỉ người có quyền hợp lệ mới được tạo hoặc thay đổi quyền chia sẻ.

Thu hồi quyền phải ngăn truy cập mới theo chính sách đã công bố. Các bản sao đã được tạo trước đó, bản sao lưu hoặc nội dung do người khác lưu lại phải được xử lý theo chính sách dữ liệu cụ thể, không được giả định rằng thu hồi quyền tự động xóa mọi bản sao.

### 4.4. Platform Knowledge

Kho tri thức nền tảng chứa tài liệu, hướng dẫn và kiến thức được phép sử dụng chung.

Kho này tách biệt với trí nhớ cá nhân và trí nhớ gia đình.

Không đưa dữ liệu riêng của người dùng vào kho tri thức nền tảng chỉ vì dữ liệu đó đã được lưu trong hệ thống.

## 5. Mô hình quyền truy cập

### 5.1. Nguyên tắc

- Mặc định từ chối nếu chưa xác minh được quyền.
- Kiểm tra quyền ở backend cho từng thao tác.
- Không tin dữ liệu `account_id`, `family_id` hoặc vai trò do trình duyệt tự gửi mà chưa xác thực.
- Không dựa riêng vào việc ẩn nút hoặc ẩn trang.
- Không cho phép truy cập chéo bằng cách thay đổi ID trong URL hoặc API.
- Giới hạn quyền của dịch vụ và tài khoản kỹ thuật.
- Ghi nhật ký thao tác nhạy cảm mà không làm lộ bí mật.

### 5.2. Ma trận quyền mục tiêu

| Đối tượng | Trí nhớ cá nhân của mình | Trí nhớ cá nhân của người khác | Trí nhớ gia đình |
|---|---|---|---|
| Thành viên | Đọc và quản lý theo chính sách | Từ chối mặc định | Theo quyền thành viên |
| Quản trị viên gia đình | Chỉ dữ liệu của mình | Từ chối mặc định | Theo quyền quản trị |
| Chủ sở hữu nền tảng | Theo tài khoản của mình | Không tự động được phép | Không tự động được phép |
| Dịch vụ backend | Theo quyền tối thiểu cần thiết | Chỉ trong luồng được ủy quyền | Theo quyền và phạm vi nhóm |

Ma trận này là mục tiêu thiết kế. Quyền chính xác cho từng thao tác cần được chốt trước khi triển khai.

Quản trị hệ thống không đồng nghĩa với quyền đọc dữ liệu riêng tư. Các quy trình hỗ trợ hoặc xử lý sự cố, nếu cần quyền đặc biệt, phải có cơ sở hợp lệ, giới hạn phạm vi và được kiểm toán.

### 5.3. Quyền theo thao tác

Mỗi loại dữ liệu cần xác định riêng quyền:

- Create: tạo.
- Read: đọc.
- Update: cập nhật.
- Delete: xóa.
- Share: chia sẻ.
- Revoke: thu hồi quyền chia sẻ.
- Export: xuất dữ liệu.
- Administer: quản lý nhóm hoặc cấu hình.

Không suy ra quyền xóa, chia sẻ hoặc quản trị chỉ từ quyền đọc.

## 6. Vòng đời dữ liệu

### 6.1. Tạo

Dữ liệu chỉ được tạo khi có mục đích rõ ràng, quyền hợp lệ và cấu trúc dữ liệu phù hợp.

### 6.2. Đọc

Mọi yêu cầu đọc phải xác thực danh tính, xác định tài nguyên và kiểm tra quyền ở backend.

### 6.3. Cập nhật

Chỉ người có quyền cập nhật được thay đổi bản ghi. Hệ thống phải bảo toàn quan hệ sở hữu và phạm vi nhóm.

### 6.4. Chia sẻ

Chia sẻ là hành động rõ ràng, không phải hệ quả tự động của việc lưu trí nhớ.

Phải xác minh người chia sẻ có quyền, đối tượng nhận hợp lệ và phạm vi chia sẻ đã được xác định.

### 6.5. Thu hồi chia sẻ

Khi quyền chia sẻ bị thu hồi, hệ thống phải chặn các yêu cầu truy cập mới không còn được phép và xử lý cache hoặc bản sao theo chính sách.

### 6.6. Xóa

Cần phân biệt:

- Xóa theo yêu cầu người dùng.
- Xóa do tài khoản hoặc nhóm kết thúc.
- Xóa do chính sách lưu giữ.
- Xóa khỏi hệ thống đang hoạt động.
- Xử lý bản sao lưu và nhật ký.

Không tuyên bố xóa hoàn toàn tức thời nếu bản sao lưu vẫn còn theo lịch lưu giữ đã công bố.

### 6.7. Xuất dữ liệu

Thiết kế cơ chế xuất dữ liệu mà người dùng có quyền truy cập, xác minh yêu cầu và bảo vệ tệp xuất.

Không đưa dữ liệu của thành viên khác vào bản xuất.

### 6.8. Sao lưu và phục hồi

Quy trình sao lưu phải có quyền truy cập hạn chế, lịch lưu giữ và kiểm thử phục hồi.

Khôi phục dữ liệu không được làm sống lại quyền truy cập đã bị thu hồi mà không qua kiểm tra chính sách.

## 7. Thiết kế lưu trữ dự kiến

Các thực thể logic ban đầu:

1. `accounts`
2. `auth_identities`
3. `family_groups`
4. `family_memberships`
5. `personal_memories`
6. `family_memories`
7. `memory_sharing_grants`
8. `audit_events`
9. `data_export_requests`
10. `data_deletion_requests`

Danh sách này chưa phải schema cơ sở dữ liệu đã được triển khai. Kiểu dữ liệu, khóa ngoại, chỉ mục, ràng buộc và chính sách phân quyền ở cấp cơ sở dữ liệu cần được thiết kế sau khi chọn nền tảng lưu trữ.

Mọi bản ghi trí nhớ phải có quan hệ sở hữu hoặc phạm vi nhóm xác định.

## 8. Kiểm soát bảo mật

Các biện pháp cần có trước khi sử dụng dữ liệu thật:

- Xác thực người dùng ở backend.
- Kiểm tra quyền trên từng API.
- Cách ly dữ liệu theo tài khoản và nhóm.
- Quản lý bí mật bằng cấu hình bảo mật của môi trường triển khai.
- Không ghi token, mật khẩu hoặc nội dung nhạy cảm vào log.
- Giới hạn tần suất yêu cầu và xử lý lỗi phù hợp.
- Bảo vệ các thao tác mời thành viên, chia sẻ, xuất và xóa.
- Theo dõi các thao tác quản trị nhạy cảm.
- Có quy trình xử lý sự cố và thu hồi quyền.
- Kiểm tra quyền truy cập sau khi thành viên rời nhóm.

## 9. Tiêu chí kiểm thử trước khi triển khai

### 9.1. Kiểm thử tài khoản

- Tạo tài khoản hợp lệ.
- Xử lý tài khoản trùng lặp theo chính sách.
- Xác minh danh tính trước khi cấp phiên truy cập.
- Thu hồi hoặc hết hạn phiên theo thiết kế.
- Từ chối yêu cầu chưa xác thực.

### 9.2. Kiểm thử cách ly trí nhớ cá nhân

Tạo hai tài khoản thử nghiệm A và B.

- A đọc được dữ liệu thuộc A khi có quyền.
- B không đọc được dữ liệu của A.
- Thay ID trong URL hoặc API không làm lộ dữ liệu.
- B không thể cập nhật hoặc xóa trí nhớ của A.
- B không thể xuất dữ liệu của A.
- Yêu cầu không đủ quyền phải bị từ chối.

### 9.3. Kiểm thử trí nhớ gia đình

- Thành viên chỉ truy cập nhóm mà họ có quyền.
- Thành viên ngoài nhóm không truy cập được dữ liệu nhóm.
- Quản trị viên không tự động đọc trí nhớ cá nhân.
- Nội dung chưa được chia sẻ không xuất hiện trong vùng chung.
- Thu hồi quyền chặn truy cập mới.
- Thành viên rời nhóm mất quyền truy cập theo chính sách.
- Một nhóm không truy cập được dữ liệu nhóm khác.

### 9.4. Kiểm thử vòng đời dữ liệu

- Tạo, đọc, cập nhật và xóa dữ liệu đúng quyền.
- Xử lý yêu cầu xuất dữ liệu đúng phạm vi.
- Không xuất dữ liệu của tài khoản khác.
- Xử lý cache và bản sao theo chính sách.
- Kiểm thử sao lưu và phục hồi.
- Không vô tình khôi phục quyền đã bị thu hồi.

### 9.5. Kiểm thử bảo mật

- Không lộ bí mật trong mã nguồn hoặc nhật ký.
- Backend từ chối yêu cầu không đủ quyền.
- Lỗi không tiết lộ dữ liệu riêng của tài khoản khác.
- API không dựa vào vai trò do người dùng tự khai báo.
- Thao tác chia sẻ và quản trị được kiểm tra quyền.
- Không mở chức năng trò chuyện AI cho tài khoản ngoài phạm vi thử nghiệm ban đầu.

### 9.6. Điều kiện PASS

Chỉ đánh dấu PASS khi có kết quả kiểm thử có thể tái hiện, ghi rõ:

- Tên bài kiểm thử.
- Môi trường chạy.
- Kết quả thực tế.
- Kết quả mong đợi.
- Bằng chứng hoặc log đã loại bỏ dữ liệu nhạy cảm.

Nếu có lỗi cách ly dữ liệu, lỗi truy cập trái phép hoặc lộ bí mật, không phát hành cho nhóm người dùng tiếp theo.

## 10. Cổng phát hành

### Cổng A: Người sáng tạo

Yêu cầu:

- Có tài khoản thử nghiệm được bảo vệ.
- Có trí nhớ cá nhân.
- Có kiểm soát truy cập backend.
- Chức năng trò chuyện giới hạn ở người sáng tạo.
- Có kết quả kiểm thử cơ bản.

### Cổng B: Gia đình

Yêu cầu bổ sung:

- Có cơ chế mời và thu hồi thành viên.
- Có trí nhớ chung với quyền rõ ràng.
- Đạt kiểm thử truy cập chéo.
- Có quy trình xử lý dữ liệu cá nhân.
- Có sao lưu và phục hồi đã được kiểm tra.

### Cổng C: Người dùng bên ngoài

Yêu cầu bổ sung:

- Đánh giá bảo mật và quyền riêng tư.
- Chính sách và điều khoản phù hợp với vận hành thật.
- Kiểm tra năng lực và chi phí hạ tầng.
- Quy trình hỗ trợ, xử lý sự cố và yêu cầu dữ liệu.
- Đánh giá các yêu cầu pháp lý và xác minh OAuth liên quan.
- Phê duyệt phát hành có ghi nhận.

## 11. Các quyết định còn mở

Trước khi lập trình cần chốt:

1. Nhà cung cấp xác thực.
2. Cơ sở dữ liệu và nơi lưu trữ.
3. Phạm vi và thời gian lưu hội thoại.
4. Mô hình dữ liệu trí nhớ.
5. Quyền cụ thể của từng vai trò.
6. Quy trình xuất và xóa dữ liệu.
7. Chính sách sao lưu và phục hồi.
8. Quy tắc chia sẻ, thu hồi và xử lý bản sao.
9. Cách giới hạn trò chuyện AI ở giai đoạn thử nghiệm.
10. Cách quản lý chi phí và hạn mức.

Không chọn công nghệ chỉ vì tiện triển khai nếu chưa đánh giá quyền riêng tư, chi phí, khả năng phục hồi và khả năng bàn giao lâu dài.

## 12. Quy tắc triển khai

- Không sửa mã nguồn trước khi thiết kế được rà soát.
- Kiểm tra tệp hiện tại trước khi ghi đè.
- Sao lưu trước các thay đổi có ảnh hưởng.
- Ưu tiên thay đổi nhỏ và có thể quay lại.
- Không đưa dữ liệu thật vào kiểm thử cách ly khi chưa có biện pháp bảo vệ phù hợp.
- Không đánh dấu PASS khi chưa có bằng chứng.
- Không thay đổi AutoYouTube hoặc MemoryAI trong phạm vi BUILD này.

## 13. Bước tiếp theo

Sau khi tài liệu được lưu vào GitHub:

1. Rà soát các quyết định còn mở.
2. Chọn mô hình xác thực và lưu trữ phù hợp.
3. Thiết kế schema dữ liệu cụ thể.
4. Xây bộ kiểm thử cách ly trước khi kết nối dữ liệu thật.
5. Chỉ sau đó mới bắt đầu triển khai tài khoản và trí nhớ.

**Kết luận:** Quyền riêng tư, cách ly dữ liệu và khả năng bàn giao là yêu cầu kiến trúc nền tảng, không phải tính năng bổ sung về sau.
