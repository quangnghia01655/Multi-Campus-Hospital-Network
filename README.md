1. Kiến trúc Mạng
Hệ thống được thiết kế phân tầng và phân chia theo chức năng tại từng khu vực.

📍 Khu A (Trụ sở chính) 

Khu A được chia thành 3 tòa nhà chính với vai trò rõ rệt:

Tòa I (Vận hành & Chuyên môn): Gồm 5 tầng, phục vụ các khoa chuyên môn và quản lý.

Tầng 1: Quản lý 

Tầng 2: Khu phòng bệnh 

Tầng 3: Khoa cấp cứu 

Tầng 4: Khoa ngoại 

Tầng 5: Khoa nội 

Tòa II (Trung tâm máy chủ): Là nơi đặt các máy chủ cung cấp dịch vụ cho toàn hệ thống.

Tòa III (Bảo mật & Lưu trữ): Tập trung các thiết bị mạng lõi như Router, Firewall và máy chủ lưu trữ.

📍 Khu B (Chi nhánh) 
Khu B là cơ sở chi nhánh, kết nối về Khu A thông qua VPN Site-to-Site qua Internet. Tòa nhà gồm 4 tầng:
Tầng 1: Quản lý 

Tầng 2: Khu phòng bệnh 

Tầng 3: Khoa mắt 

Tầng 4: Khoa tai mũi họng 

2. Công nghệ & Giao thức chính
Dự án triển khai một loạt các công nghệ và giao thức tiêu chuẩn để đảm bảo hiệu suất và bảo mật.

🌐 Phân đoạn mạng (VLAN)
Hệ thống sử dụng VLAN để phân chia mạng logic, tăng cường bảo mật và quản lý. Các VLAN chính bao gồm:

VLAN 10: MANAGEMENT

VLAN 20: LAN (Trụ sở A)

VLAN 50: WLAN (Trụ sở A)

VLAN 60: BLAN (LAN Chi nhánh B)

VLAN 90: BWLAN (WLAN Chi nhánh B)

VLAN 199: Blackhole (Không sử dụng)

🗺️ Định tuyến (Routing)
OSPF: Được sử dụng làm giao thức định tuyến động nội vùng (IGP) chính. OSPF được cấu hình trên các Router (HQ-ISP, BRANCH) , ASA Firewall (HQ-FWL, B-FWL) và Switch Layer 3 (HQ-SW1, HQ-SW2, B-SW1, B-SW2).
Static Routing: Cấu hình default route trên ASA Firewall (HQ-FWL, B-FWL) để trỏ ra router ISP, cho phép truy cập Internet.
🔒 Bảo mật (Security)
ASA Firewall: Triển khai 2 thiết bị Cisco ASA (HQ-FWL, B-FWL) làm tường lửa biên. Các cổng được phân vùng security-level: OUTSIDE (0), DMZ (70), INSIDE1 (100), INSIDE2 (100).
VPN Site-to-Site: Cấu hình IPsec VPN (IKEv1, 3DES, SHA) giữa HQ-FWL và B-FWL để mã hóa lưu lượng truy cập giữa hai cơ sở.
ACL (Access Control Lists):
VPN-ACL: Định nghĩa traffic được phép đi qua đường hầm VPN (ví dụ: ip 192.168.10.0 255.255.255.0 172.17.0.0 255.255.0.0).
RES-ACCESS: Cho phép các dịch vụ thiết yếu (ICMP, DNS, WWW, SMTP...) từ bên ngoài vào vùng DMZ và OUTSIDE.
NAT (Network Address Translation): Cấu hình NAT động (dynamic interface) trên ASA, cho phép các mạng LAN nội bộ (INSIDE1, INSIDE2) truy cập Internet qua địa chỉ IP của cổng OUTSIDE.
Bảo mật SSH: Cấu hình SSH version 2, username/password, và sử dụng access-class để giới hạn chỉ IP từ dải 192.168.10.0 (VLAN Management) mới được phép SSH vào thiết bị.

🚀 Tính sẵn sàng cao (High Availability)

HSRP (Hot Standby Router Protocol): Cấu hình HSRP cho các cặp Switch Layer 3 (HQ-SW1/HQ-SW2 và B-SW1/B-SW2) để cung cấp default gateway dự phòng cho tất cả các VLAN.

EtherChannel (LACP): Gộp 3 cổng (GigabitEthernet1/0/21-23) thành Port-channel 1 (LACP active-passive) giữa HQ-SW1 và HQ-SW2, và Port-channel 2 giữa B-SW1 và B-SW2 để tăng băng thông và dự phòng.

📡 Mạng không dây (Wireless)

WLC (Wireless LAN Controller): Triển khai một WLC (IP: 10.10.0.15) để quản lý tập trung các Access Point (AP).

SSIDs: Cung cấp nhiều SSID cho các đối tượng người dùng khác nhau:

NHANVIEN WIFI

BENHVIEN WIFI

PHONGBENH_A WIFI

PHONGBENH_B WIFI 

3. Dịch vụ Mạng (Server Farm)
Khu A (Tòa II) tập trung các máy chủ cung cấp dịch vụ cho toàn hệ thống.

DHCP: 2 máy chủ DHCP (DHCP-1: 10.20.20.5, DHCP-2: 10.20.20.6) cấp phát IP cho các pool HQLAN, HGMGT, HQWLAN, BRLAN, BRWLAN. Các Switch L3 được cấu hình ip helper-address để chuyển tiếp yêu cầu DHCP.

DNS: Máy chủ DNS (10.20.20.7) phân giải tên miền nội bộ health.vn (10.20.20.8) và google.com (8.8.8.8).

Web Server (HTTP): Cung cấp cổng thông tin http://health.vn.

Email Server: Cung cấp dịch vụ email nội bộ với tên miền gmail.com.

FTP Server: Dịch vụ lưu trữ và trao đổi file (10.20.20.10).
