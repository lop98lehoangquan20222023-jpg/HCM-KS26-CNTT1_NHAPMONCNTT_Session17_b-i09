## PHẦN A – THIẾT LẬP PROFILE VÀ BOOKMARK CHUẨN DEV

### 1. Hệ quả khi dùng chung 1 Profile cho cả GitHub cá nhân và GitHub công ty
* Nếu dùng chung một Profile duy nhất, trình duyệt sẽ lưu trữ chung một **Cookies** và **Session Storage**.
* Khi đăng nhập song song, xác thực giữa tài khoản cá nhân và tài khoản công ty sẽ bị ghi đè hoặc xung đột định danh (Session Conflict), dẫn đến việc liên tục bị văng đăng xuất khỏi hệ thống công ty.

### 2. Mô hình 3 Profile chuẩn Developer

| Profile | Tên đặt | Mục đích sử dụng |
| :--- | :--- | :--- |
| **1** | `Personal` | Dành cho mạng xã hội, giải trí và tài khoản GitHub cá nhân ngoài giờ làm việc. |
| **2** | `Work` | Dành riêng cho công việc, mã nguồn dự án công ty và GitHub công ty. |
| **3** | `Test` | Dành cho việc thử nghiệm tính năng, kiểm thử và chạy thử mã lệnh độc lập. |

### 3. Thiết kế cấu trúc Bookmark gồm 3 thư mục
* **📁 `01_Company`**
  1. [LMS Portal](https://lms-student.rikkei.edu.vn) *(Hệ thống học tập và quản lý nội bộ)*
  2. [GitHub](https://github.com) *(Kho chứa mã nguồn dự án)*
* **📁 `02_Dev_Tools`**
  1. [Google Fonts](https://fonts.google.com) *(Tra cứu thông số CSS)*
  2. [ChatGPT](https://chatgpt.com) *(Hỗ trợ tra cứu, giải quyết lỗi và viết code nhanh)*
* **📁 `03_Learning_Docs`**
  1. [Tự học](https://tuhoc.cc) *(Tài liệu chuẩn kỹ thuật web)*
  2. [Google Search](https://www.google.com) *(Công cụ tìm kiếm thông tin và tài liệu lập trình phổ biến nhất)*

### 4. Cho đồng nghiệp mượn máy 5 phút
* **Giải pháp:** Sử dụng **Guest Profile (Chế độ Khách)**.
* **Lý do:** Chế độ Khách tạo ra một không gian duyệt web hoàn toàn cô lập, không lưu lịch sử, không truy cập được vào mật khẩu hay cookie của các Profile chính, giúp bảo mật tuyệt đối dữ liệu cá nhân.

---

## PHẦN B – CHỌN ĐÚNG EXTENSION HỖ TRỢ

### 1. Lựa chọn Extension xử lý đường link API JSON thô
* **Nhóm Extension:** Thuộc nhóm **Dev Suite**.
* **Tên tiện ích:** **JSON Viewer**.
* **Lý do loại trừ:** Nhóm *Security Suite* chuyên về bảo mật/mật khẩu, còn nhóm *Productivity Suite* chuyên quản lý tác vụ/ghi chú, không có tính năng định dạng cấu trúc dữ liệu JSON.

### 2. Cấu hình quyền Site Access
* **Cấu hình:** Chọn **`On click`** (Chỉ chạy khi nhấp vào).
* **Giải thích:** Tuân thủ nguyên tắc tối thiểu hóa quyền hạn (Least Privilege), tránh việc extension chạy ngầm trên mọi trang web gây tốn tài nguyên và tiềm ẩn rủi ro bảo mật.

---

## PHẦN C – PHÂN TÍCH LUỒNG HTTP REQUEST

### 1. 4 giai đoạn khi gõ `test.internal-tool.com` và nhấn Enter
1. **Phân giải tên miền (DNS Lookup):** Tra cứu địa chỉ IP từ tên miền.
2. **Thiết lập kết nối (TCP Handshake & TLS):** Thiết lập kênh truyền dữ liệu an toàn với máy chủ.
3. **Gửi HTTP Request:** Gửi yêu cầu HTTP lên máy chủ.
4. **Nhận HTTP Response & Render:** Nhận dữ liệu trả về và dựng hình giao diện lên màn hình.

### 2. So sánh mã trạng thái và hướng debug

| Mã trạng thái | Ý nghĩa | Lỗi nằm ở đâu? | Hướng debug ban đầu |
| :--- | :--- | :--- | :--- |
| `404 Not Found` | Không tìm thấy tài nguyên yêu cầu trên máy chủ. | Phía Client (sai đường dẫn URL hoặc resource đã bị xóa). | Kiểm tra lại chính tả URL và cấu hình route trên frontend/backend. |
| `500 Internal Server Error` | Lỗi máy chủ nội bộ bất ngờ trong quá trình xử lý. | Phía Server (lỗi code backend hoặc cơ sở dữ liệu). | Kiểm tra log phía backend và thông báo lỗi từ server. |

### 3. Tab DevTools xem mã trạng thái HTTP
* **Tab được chọn:** **Network Tab**.
* **Lý do không chọn tab khác:** Các tab *Elements*, *Console*, *Application* quản lý giao diện, log mã nguồn hoặc storage cục bộ, không chuyên trách liệt kê danh sách request mạng và status code như tab Network.

---

## PHẦN D – CHẨN ĐOÁN CACHE VÀ COOKIES

### 1. Sửa lỗi F5 vẫn thấy giao diện CSS cũ
* **Nguyên nhân:** Trình duyệt đang tải tệp cũ được lưu trữ trong **Browser Cache**.
* **Thao tác khắc phục:** Thực hiện **Hard Reload** bằng tổ hợp phím **`Ctrl` + `F5`** (hoặc `Shift` + `F5`) để ép tải lại toàn bộ tài nguyên mới nhất từ server.

### 2. Rủi ro bảo mật khi Cookie thiếu dấu tích `HttpOnly`
* **Rủi ro:** Mã độc JavaScript có thể đọc được giá trị `session_id` thông qua câu lệnh `document.cookie`.
* **Tên loại tấn công:** Tấn công **Cross-Site Scripting (XSS)** kết hợp đánh cắp phiên làm việc (Session Hijacking).

### 3. Đăng xuất nhưng giữ nguyên Cache
* Mở **Tab Application** $\rightarrow$ Chọn mục **Cookies / Local Storage** của trang web và tiến hành xóa riêng phần phiên đăng nhập, giữ nguyên phần Cache Storage để tài nguyên tĩnh tiếp tục được lưu trữ.

---

## PHẦN E – DEBUG BẰNG INSPECT ELEMENT VÀ NETWORK TAB

### 1. Sửa màu nút "Đăng nhập" trực tiếp trên trình duyệt
* Nhấp chuột phải vào nút $\rightarrow$ Chọn **Inspect** $\rightarrow$ Tại bảng **Styles**, chỉnh sửa trực tiếp mã màu (`background-color`) để kiểm chứng trực quan ngay trên trình duyệt trước khi yêu cầu Dev sửa mã nguồn gốc.
