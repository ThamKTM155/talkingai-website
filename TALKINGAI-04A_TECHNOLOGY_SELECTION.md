# TALKINGAI-04A_TECHNOLOGY_SELECTION.md

**Phiên bản:** 0.1  
**Ngày:** 22/10/2026  
**BUILD:** TALKINGAI-04A  
**Trạng thái:** Đề xuất lựa chọn công nghệ, chờ người sáng lập duyệt  
**Repository đích:** `ThamKTM155/talkingai-website`  
**Tài liệu liên quan:** `TALKINGAI_SYSTEM_ARCHITECTURE.md`, `TALKINGAI_DATA_INVENTORY.md`, `TALKINGAI_ACCOUNT_MEMORY_DESIGN.md`, `TALKINGAI-03B_DESIGN_DECISIONS.md`

---

## 1. Mục tiêu

Chọn nền tảng khởi đầu cho tài khoản và trí nhớ TalkingAI theo các nguyên tắc:

- TalkingAI là một sản phẩm độc lập.
- Không dùng chung cơ sở dữ liệu, tài khoản dịch vụ, bí mật, token hoặc vòng đời triển khai với AutoYouTube, MemoryAI, ThamAI hay dự án khác.
- Mỗi thành viên có danh tính và vùng dữ liệu riêng.
- Chỉ chia sẻ trí nhớ gia đình khi có quyền rõ ràng.
- Bắt đầu nhỏ, ưu tiên gói miễn phí, có thể mở rộng và có đường xuất dữ liệu.
- Chưa triển khai tài khoản thật, backend hoặc dữ liệu nhạy cảm trong BUILD này.

## 2. Đề xuất lựa chọn

### Phương án khuyến nghị để thử nghiệm: Supabase cho TalkingAI riêng

- **Xác thực:** Supabase Auth.
- **Cơ sở dữ liệu:** PostgreSQL do Supabase quản lý.
- **Kiểm soát truy cập dữ liệu:** PostgreSQL Row Level Security (RLS), kết hợp quyền bảng và chính sách theo từng thao tác.
- **Website:** tiếp tục dùng repository riêng `talkingai-website` và dự án Vercel hiện có; chưa cần thay giao diện.
- **Backend nghiệp vụ:** chỉ bổ sung sau khi xác nhận mô hình dữ liệu và các chính sách RLS; thao tác quản trị nhạy cảm thực hiện phía máy chủ.
- **Lưu tệp:** chưa cần trong giai đoạn tài khoản/trí nhớ ban đầu; nếu sau này cần, đánh giá riêng và áp dụng quyền truy cập tương ứng.

Lý do: mô hình quan hệ phù hợp với quan hệ tài khoản, nhóm gia đình, tư cách thành viên, quyền chia sẻ và nhật ký kiểm toán. RLS cho phép đặt hàng rào ở tầng cơ sở dữ liệu, nhưng phải cấu hình đúng quyền bảng và chính sách; RLS không tự động bảo vệ bảng nếu grants còn mở.

### Phương án thay thế: Firebase Authentication + Cloud Firestore

Firebase cũng có xác thực và quy tắc bảo mật. Tuy nhiên, mô hình tài khoản-gia đình-quyền chia sẻ của TalkingAI có nhiều quan hệ cần kiểm tra chéo. Vì vậy, PostgreSQL là lựa chọn ban đầu dễ diễn đạt bằng ràng buộc quan hệ và truy vấn kiểm toán hơn. Firebase vẫn là phương án hợp lệ nếu người sáng lập ưu tiên hệ sinh thái Google và chấp nhận thiết kế, kiểm thử Firestore Security Rules cẩn thận.

## 3. So sánh ngắn

| Tiêu chí | Supabase Auth + PostgreSQL | Firebase Auth + Firestore |
|---|---|---|
| Đăng nhập | Có | Có |
| Dữ liệu quan hệ | PostgreSQL, quan hệ và khóa ngoại | Mô hình tài liệu, cần thiết kế tham chiếu |
| Cách ly dữ liệu | RLS + grants + backend | Firestore Security Rules + backend |
| Trí nhớ cá nhân/gia đình | Có thể biểu diễn bằng bảng quan hệ và quyền chia sẻ | Có thể biểu diễn bằng collections/documents và rules |
| Xuất/chuyển dữ liệu | PostgreSQL thuận tiện cho SQL và xuất dữ liệu có cấu trúc | Có công cụ xuất, nhưng cách tổ chức dữ liệu ảnh hưởng quá trình chuyển |
| Gói miễn phí | Có giới hạn sử dụng; dự án miễn phí có thể bị tạm dừng khi không hoạt động | Có quota miễn phí; một số tính năng cần bật billing |
| Rủi ro chính | RLS/grants cấu hình sai; khóa bí mật bị lộ | Rules quá rộng; logic quyền phức tạp hoặc bị trùng quy tắc |
| Đánh giá | **Ưu tiên đề xuất cho thử nghiệm** | Phương án dự phòng |

Thông tin gói miễn phí có thể thay đổi. Trước khi đăng ký hoặc nâng cấp cần kiểm tra giá, quota, điều kiện tạm dừng, khu vực lưu trữ, sao lưu và yêu cầu thanh toán tại thời điểm thực hiện.

## 4. Ranh giới độc lập bắt buộc

TalkingAI phải có các tài nguyên riêng:

- Repository: `ThamKTM155/talkingai-website`.
- Dự án triển khai Vercel riêng.
- Dự án Supabase riêng, chỉ dành cho TalkingAI.
- Bộ biến môi trường riêng.
- Danh tính dịch vụ, quyền truy cập và bí mật riêng.
- Quy trình sao lưu, khôi phục, nhật ký và phát hành riêng.
- Tài liệu quyết định và lịch sử BUILD riêng.

Không dùng lại database, service-role key, token OAuth, tài khoản dịch vụ, biến môi trường hay bảng dữ liệu của AutoYouTube, MemoryAI, ThamAI hoặc dự án khác. Không kết nối trực tiếp các hệ thống này trong BUILD-04A.

## 5. Nguyên tắc bảo mật bắt buộc

1. Bật RLS cho mọi bảng chứa dữ liệu người dùng được truy cập qua API.
2. Kiểm tra cả quyền bảng (grants) lẫn chính sách RLS; không giả định chỉ bật RLS là đủ.
3. Không đưa secret/service-role key vào HTML, JavaScript chạy trên trình duyệt, GitHub hoặc nhật ký.
4. Chỉ dùng dữ liệu giả trong thử nghiệm đầu tiên.
5. Tài khoản A không được đọc/sửa/xóa dữ liệu của tài khoản B bằng cách đổi ID trong request.
6. Thành viên gia đình không mặc nhiên được đọc trí nhớ riêng của thành viên khác.
7. Quyền quản trị nền tảng không tự động đồng nghĩa quyền đọc nội dung riêng tư.
8. Chưa lưu nội dung trò chuyện thật hoặc kết nối Google/YouTube trong BUILD này.
9. Sao lưu và khôi phục phải được kiểm thử trước khi đưa dữ liệu gia đình thật vào hệ thống.
10. Chưa đưa ra cam kết pháp lý hoặc chính sách quyền riêng tư cuối cùng cho đến khi quy trình dữ liệu thực tế được xác định.

## 6. Kế hoạch thử nghiệm theo giai đoạn

### Giai đoạn 0: Duyệt thiết kế
- Người sáng lập duyệt lựa chọn Supabase.
- Xác nhận khu vực lưu dữ liệu, phương án tài khoản và quy tắc xóa/xuất.
- Chưa tạo project hoặc dữ liệu thật.

### Giai đoạn 1: Sandbox kỹ thuật
- Tạo project thử nghiệm riêng cho TalkingAI sau khi được duyệt.
- Dùng dữ liệu giả và tài khoản thử nghiệm.
- Thiết lập schema tối thiểu, RLS và grants thông qua migration có thể tái lập.
- Kiểm thử quyền cho phép và từ chối trước khi nối website.

### Giai đoạn 2: Tài khoản người sáng tạo
- Chỉ bật đăng nhập cho người sáng tạo.
- Kiểm thử phiên hết hạn, đăng xuất, thu hồi phiên và khôi phục tài khoản.
- Không bật trò chuyện AI cho người dùng bên ngoài.

### Giai đoạn 3: Gia đình
- Chỉ mời thành viên đã đồng ý tham gia.
- Kiểm thử cách ly tài khoản, cách ly nhóm, chia sẻ và thu hồi.
- Chỉ dùng dữ liệu thật sau khi kiểm thử bảo mật và sao lưu đạt.

### Giai đoạn 4: Người dùng bên ngoài
- Chỉ bắt đầu sau khi đánh giá bảo mật, quyền riêng tư, chi phí, hỗ trợ, điều khoản dịch vụ và các yêu cầu pháp lý áp dụng.
- Không tự động mở đăng ký công khai chỉ vì ứng dụng đã chạy.

## 7. Tiêu chí nghiệm thu BUILD-04A

- [ ] Tài liệu đề xuất công nghệ đã được lưu trong repository riêng.
- [ ] Ranh giới độc lập với AutoYouTube, MemoryAI, ThamAI và dự án khác được ghi rõ.
- [ ] Chưa có bí mật hoặc dữ liệu cá nhân thật trong GitHub.
- [ ] Chưa tuyên bố đã tạo database, tài khoản hoặc backend.
- [ ] Lựa chọn nhà cung cấp được người sáng lập duyệt trước khi tạo tài nguyên.
- [ ] Bước tiếp theo có kế hoạch kiểm thử RLS/grants, không chỉ kiểm tra giao diện.

## 8. Quyết định cần người sáng lập xác nhận

**Đề xuất:** chọn Supabase Auth + PostgreSQL cho sandbox TalkingAI độc lập; giữ Vercel và repository hiện tại; chưa kết nối dịch vụ khác.

Chỉ sau khi duyệt mới chuyển sang BUILD-04B để tạo sandbox và migration thử nghiệm. Nếu chưa muốn chọn Supabase, dừng ở tài liệu này và so sánh thêm trước khi tạo tài nguyên.

---

**Kết luận:** BUILD-TALKINGAI-04A là quyết định công nghệ ở mức tài liệu. Nó chưa tạo project, chưa cấu hình xác thực và chưa triển khai cơ sở dữ liệu.
