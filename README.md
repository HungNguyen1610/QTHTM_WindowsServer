# QTHTM_WindowsServer
# Dự án: Xây dựng và Quản trị Hệ thống mạng Doanh nghiệp với Windows Server

Đây là dự án học tập nhằm mô phỏng việc triển khai và quản lý một hệ thống mạng hoàn chỉnh cho một doanh nghiệp nhỏ với 2 phòng ban, sử dụng các dịch vụ cốt lõi trên hệ điều hành Windows Server.

---

### **Mô hình hệ thống**

Hệ thống bao gồm một máy chủ Windows Server đóng vai trò Domain Controller và các máy trạm (client) thuộc 2 phòng ban khác nhau (Phòng A, Phòng B), cùng với một mạng riêng cho phòng IT.

<img width="759" height="238" alt="mohinhhethong" src="https://github.com/user-attachments/assets/462d64e6-5f4b-4e6c-87e6-c12d25e0b92a" />

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

<img width="234" height="249" alt="ou" src="https://github.com/user-attachments/assets/9801cdeb-abf8-4428-8a09-c5b18ea6ecf2" />


#### 2. Cấu hình DHCP Server
*   Thiết lập DHCP Server để cấp phát địa chỉ IP động cho các máy trạm trong từng dải mạng của Phòng A và Phòng B.

<img width="587" height="402" alt="ipleased_A" src="https://github.com/user-attachments/assets/23e52378-d531-4ec3-81cd-75172d1241dc" />
<img width="584" height="394" alt="ipleased_B" src="https://github.com/user-attachments/assets/cbee360d-ecdf-4483-abe5-716961d23f36" />
<img width="462" height="594" alt="dhcp_A" src="https://github.com/user-attachments/assets/da1f8a94-7488-4901-b7db-6ef05d2b5796" />
<img width="377" height="571" alt="dhcp_B" src="https://github.com/user-attachments/assets/e12360f6-e5b3-4b3a-b3a2-e8a098f3f3eb" />


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

<img width="1028" height="727" alt="policy_A" src="https://github.com/user-attachments/assets/31ff37a3-697d-4510-8af4-2eb0d22aac4e" />
<img width="1019" height="768" alt="policy_B" src="https://github.com/user-attachments/assets/7777d77f-fec4-4c5b-8a71-23cfb41d058c" />

#### 4. Dịch vụ Web (IIS)
*   Cài đặt và cấu hình IIS để host 2 website:
    *   `tensinhvien.vn`
    *   `mssv.vn`
*   Mỗi website có nội dung riêng và có thể được truy cập từ tất cả các máy trạm trong hệ thống.

<img width="1150" height="715" alt="web" src="https://github.com/user-attachments/assets/21edf77a-61e5-4efc-98f0-15a45edc056d" />


#### 5. Quản lý File Server (FSRM)
*   Tạo các thư mục chia sẻ `TENSINHVIEN` và `MSSV` với các cấu hình nâng cao:
    *   **Phân quyền (Permissions):** Người dùng Phòng A có thể thêm nhưng không thể xóa file; người dùng Phòng B chỉ có quyền đọc.
    *   **Hạn ngạch (Quota):** Giới hạn dung lượng lưu trữ cho từng thư mục (10GB và 5GB).
    *   **Lọc file (File Screening):** Chỉ cho phép tải lên các file có định dạng `.html`.
    *   **Mapping Network Drive:** Tự động ánh xạ các thư mục chia sẻ thành ổ đĩa mạng X: và Y: cho người dùng.
    *   **Thư mục ẩn:** Tạo một thư mục chia sẻ ẩn `IT-MSSV` chỉ dành cho phòng IT với quyền đọc.

<img width="920" height="588" alt="phanquyen" src="https://github.com/user-attachments/assets/ff313d53-f335-4047-afb3-598e345d41f3" />
<img width="886" height="797" alt="hanngach" src="https://github.com/user-attachments/assets/74918d83-066a-4dc8-babe-7803b0d50b05" />
<img width="1026" height="728" alt="mappingY" src="https://github.com/user-attachments/assets/46b028e6-d9cb-4f51-b7cf-a6114b9bfb91" />
<img width="1024" height="729" alt="mapping" src="https://github.com/user-attachments/assets/ed2ca062-1edf-4b2e-ba83-3136d0957379" />


#### 6. Sao lưu tự động (Scheduled Backup)
*   Cấu hình một tác vụ tự động sao lưu thư mục chứa website `tensinhvien.vn` vào mỗi ngày sau 17:30.

<img width="1057" height="757" alt="backup" src="https://github.com/user-attachments/assets/23d4f1e0-36bc-42e2-962f-0040b34134c7" />


---

Dự án này đã giúp tôi thực hành và nắm vững các kỹ năng quan trọng trong việc quản trị một hệ thống Windows Server từ khâu triển khai ban đầu đến vận hành và bảo mật.
