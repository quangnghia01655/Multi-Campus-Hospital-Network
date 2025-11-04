Mục tiêu thiết kế
Mô hình phân tầng (Hierarchical Model): Áp dụng mô hình 3 lớp (Core, Distribution, Access) để dễ dàng quản lý, mở rộng và khắc phục sự cố.

Phân đoạn mạng (Network Segmentation): Sử dụng VLANs để chia mạng cho từng khoa/phòng ban riêng biệt (ví dụ: Khoa Cấp cứu, Khoa Xét nghiệm, Khu Văn phòng, Server Farm...). Điều này giúp tăng cường bảo mật và giảm thiểu broadcast.

Kết nối liên cơ sở (Site-to-Site Connectivity): Đảm bảo hai cơ sở tại 2 tỉnh có thể giao tiếp an toàn và tin cậy thông qua Internet.

Bảo mật (Security): Kiểm soát truy cập giữa các VLAN và bảo vệ tài nguyên mạng.

Dịch vụ mạng (Network Services): Cung cấp các dịch vụ thiết yếu cho hoạt động của bệnh viện.

Công nghệ & Giao thức chính
Dự án sử dụng các công nghệ và giao thức cốt lõi sau:

Định tuyến (Routing):

OSPF: Sử dụng làm giao thức định tuyến động nội vùng (IGP) trong mỗi cơ sở, cho phép mạng tự động học các tuyến đường và nhanh chóng hội tụ khi có thay đổi.

Chuyển mạch (Switching):

VLAN (Virtual LANs): Phân chia mạng logic theo chức năng (Khoa Khám bệnh, Khoa Nội trú, Phòng Lab, Văn phòng...).

Inter-VLAN Routing: Cấu hình trên Switch Layer 3 (SVI) để cho phép giao tiếp có kiểm soát giữa các VLAN.

Bảo mật (Security):

ASA Firewall: Triển khai 2 tường lửa Cisco ASA tại mỗi cơ sở (ASA1 và ASA2).

IPsec VPN Site-to-Site: Cấu hình đường hầm VPN an toàn giữa 2 ASA để mã hóa toàn bộ dữ liệu trao đổi giữa hai cơ sở qua Internet.

ACL (Access Control Lists): Áp dụng trên router và firewall để lọc gói tin, chỉ cho phép các truy cập hợp lệ, chặn các truy cập không mong muốn giữa các VLAN.

Dịch vụ (Services):

NAT (Network Address Translation): Cho phép các máy trạm trong mạng LAN truy cập Internet bằng cách sử dụng một địa chỉ IP public.

DHCP (Dynamic Host Configuration Protocol): Cấp phát địa chỉ IP động cho các máy trạm (PC, Laptop, Wifi-devices) trong từng VLAN.

Không dây (Wireless):

WLAN Controller (WLC): Triển khai WLC để quản lý tập trung các Access Point (AP), cung cấp mạng Wi-Fi cho bệnh nhân và nhân viên với các SSID riêng biệt.

Các dịch vụ triển khai (Server Farm)
Một khu vực Server Farm được thiết lập để cung cấp các dịch vụ trung tâm cho toàn bộ hệ thống bệnh viện:

DNS Server: Phân giải tên miền nội bộ (ví dụ: benhvien.local) và tên miền public.

Web Server (HTTP): Cổng thông tin nội bộ, tra cứu lịch làm việc, website bệnh viện.

Email Server: Hệ thống thư điện tử nội bộ.

FTP Server: Lưu trữ và chia sẻ file (ví dụ: file ảnh X-quang, tài liệu lớn).

Kết quả kiểm thử (Testing Results)
Kiểm thử kết nối nội bộ: Các máy trạm trong cùng một VLAN và khác VLAN (được cho phép bởi ACL) có thể giao tiếp thành công.

Kiểm thử kết nối liên cơ sở: Máy trạm tại Cơ sở 1 (ví dụ: 10.10.x.x) có thể ping và truy cập thành công các máy chủ tại Cơ sở 2 (ví dụ: 192.168.x.x) thông qua đường hầm VPN và ngược lại.

Kiểm thử dịch vụ:

Các máy trạm nhận IP từ DHCP thành công.

Truy cập Internet thành công (thông qua NAT).

Phân giải DNS và truy cập các dịch vụ Web, Email, FTP nội bộ thành công.

Độ ổn định: Hệ thống mạng phản hồi nhanh và ổn định, sẵn sàng cho việc triển khai ứng dụng y tế.
