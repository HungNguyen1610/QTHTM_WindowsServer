# QTHTM_WindowsServer
# Dự án: Xây dựng và Quản trị Hệ thống mạng Doanh nghiệp với Windows Server

Đây là dự án học tập nhằm mô phỏng việc triển khai và quản lý một hệ thống mạng hoàn chỉnh cho một doanh nghiệp nhỏ với 2 phòng ban, sử dụng các dịch vụ cốt lõi trên hệ điều hành Windows Server.

---

### **Mô hình hệ thống**

Hệ thống bao gồm một máy chủ Windows Server đóng vai trò Domain Controller và các máy trạm (client) thuộc 2 phòng ban khác nhau (Phòng A, Phòng B), cùng với một mạng riêng cho phòng IT.

`[Chèn ảnh mô hình mạng tổng quan của bạn tại đây]`

---

### **Các công nghệ và dịch vụ sử dụng**

*   **Hệ điều hành:** Windows Server 2016 (hoặc phiên bản bạn đã dùng)
*   **Dịch vụ Mạng:** Active Directory Domain Services (AD DS), DHCP, DNS
*   **Bảo mật & Chính sách:** Group Policy Object (GPO)
*   **Dịch vụ Web:** Internet Information Services (IIS)
*   **Quản lý Lưu trữ:** File Server Resource Manager (FSRM)
*   **Sao lưu Dữ liệu:** Windows Server Backup

---

### **Chi tiết các cấu hình chính**

#### 1. Active Directory & Quản lý người dùng
*   Xây dựng một Domain Controller với tên miền `tensinhvien.vn`.
*   Tạo cây cấu trúc Organizational Units (OUs) để quản lý riêng biệt người dùng và máy tính cho từng phòng ban (Phòng A, Phòng B).
*   Tạo các tài khoản người dùng (user) tương ứng cho mỗi phòng.

`[Chèn ảnh cây OUs trong Active Directory]`

#### 2. Cấu hình DHCP Server
*   Thiết lập DHCP Server để cấp phát địa chỉ IP động cho các máy trạm trong từng dải mạng của Phòng A và Phòng B.

`[Chèn ảnh DHCP IP Leased]`

#### 3. Triển khai Group Policy (GPO)
*   **Chính sách chung (Default Policies):** Áp dụng các chính sách về mật khẩu (Account Policies) theo tiêu chuẩn bảo mật.
*   **Chính sách cho Phòng A:**
    *   Chặn truy cập vào tất cả các ổ đĩa.
    *   Chỉ cho phép chạy các ứng dụng được chỉ định (trình duyệt web, máy tính).
    *   Ngăn người dùng lưu dữ liệu trên Desktop.
    *   Cấm thay đổi hình nền và độ phân giải màn hình.
*   **Chính sách cho Phòng B:**
    *   Kế thừa toàn bộ chính sách từ Phòng A.
    *   Mở rộng quyền, cho phép sử dụng thêm phần mềm `mspaint.exe`.

`[Chèn ảnh minh họa một cấu hình GPO tiêu biểu]`

#### 4. Dịch vụ Web (IIS)
*   Cài đặt và cấu hình IIS để host 2 website:
    *   `tensinhvien.vn`
    *   `mssv.vn`
*   Mỗi website có nội dung riêng và có thể được truy cập từ tất cả các máy trạm trong hệ thống.

`[Chèn ảnh trang web tensinhvien.vn đang chạy]`

#### 5. Quản lý File Server (FSRM)
*   Tạo các thư mục chia sẻ `TENSINHVIEN` và `MSSV` với các cấu hình nâng cao:
    *   **Phân quyền (Permissions):** Người dùng Phòng A có thể thêm nhưng không thể xóa file; người dùng Phòng B chỉ có quyền đọc.
    *   **Hạn ngạch (Quota):** Giới hạn dung lượng lưu trữ cho từng thư mục (10GB và 5GB).
    *   **Lọc file (File Screening):** Chỉ cho phép tải lên các file có định dạng `.html`.
    *   **Mapping Network Drive:** Tự động ánh xạ các thư mục chia sẻ thành ổ đĩa mạng X: và Y: cho người dùng.
    *   **Thư mục ẩn:** Tạo một thư mục chia sẻ ẩn `IT-MSSV` chỉ dành cho phòng IT với quyền đọc.

`[Chèn ảnh minh họa FSRM Quota hoặc File Screening]`

#### 6. Sao lưu tự động (Scheduled Backup)
*   Cấu hình một tác vụ tự động sao lưu thư mục chứa website `tensinhvien.vn` vào mỗi ngày sau 17:30.

`[Chèn ảnh cấu hình tác vụ trong Windows Server Backup]`

---

Dự án này đã giúp tôi thực hành và nắm vững các kỹ năng quan trọng trong việc quản trị một hệ thống Windows Server từ khâu triển khai ban đầu đến vận hành và bảo mật.
