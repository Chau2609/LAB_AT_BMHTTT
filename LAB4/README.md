BÁO CÁO THỰC HÀNH LAB 4: THU THẬP THÔNG TIN VÀ RÀ QUÉT HỆ THỐNG VỚI NMAP
- Họ và tên: Ngô Ngọc Bảo Châu
- Mã số sinh viên: 1150080044
- Lớp: K11_THMT
- Link Video thực hành: https://youtu.be/t_h8VnsMW4I

1. Môi trường thực hành
- Nền tảng ảo hóa: VMware Workstation 17.x Pro
- Cấu hình mạng: Card mạng ảo Host-only (VMnet1), dải IP: 172.16.16.0/24
- Máy tấn công (Attacker): Kali Linux (IP: 172.16.16.129, Nmap 7.99)
- Máy đích (Target): Metasploitable 2 (Linux, IP: 172.16.16.128)

 2. Cách dựng môi trường
1. Cấu hình card mạng của Kali Linux và Metasploitable 2 về chung mạng Host-only (VMnet1).
2. Kích hoạt DHCP trên VMnet1 qua Virtual Network Editor để cấp IP cho các máy ảo.
3. Kiểm tra kết nối mạng 2 chiều thông qua lệnh ping (0% packet loss).

3. Các kịch bản và tình huống đã thực hiện
| STT | Tình huống thực hành | Lệnh chính | Kết quả |
|---|---|---|---|
| 1 | Xác định IP máy Kali và Metasploitable 2 | `ip -br addr`, `ifconfig eth0` | PASS |
| 2 | Rà soát máy chủ hoạt động (Host Discovery) | `sudo nmap -sn 172.16.16.0/24` | PASS |
| 3 | Quét cổng TCP (Connect, SYN, cờ đặc biệt) | `nmap -sT`, `sudo nmap -sS`, `-sF`, `-sX`, `-sN`, `-sA` | PASS |
| 4 | Quét cổng UDP có kiểm soát | `sudo nmap -sU --top-ports 20` | PASS |
| 5 | Nhận diện phiên bản dịch vụ | `sudo nmap -sV 172.16.16.128` | PASS |
| 6 | Nhận diện hệ điều hành | `sudo nmap -O 172.16.16.128` | PASS |
| 7 | Khảo sát thông tin dịch vụ bằng NSE Script | `sudo nmap -p 445 --script smb-os-discovery` | PASS |
| 8 | Xuất kết quả đa định dạng & chuyển đổi HTML | `-oN`, `-oX`, `xsltproc`, `firefox bao_cao.html` | PASS |

 4. Lỗi gặp phải và cách khắc phục
- Lỗi 1 - Metasploitable 2 không nhận IP tự động:
  - Nguyên nhân:Dịch vụ DHCP của VMnet1 chưa được kích hoạt.
  - *Khắc phục:* Mở Virtual Network Editor tích chọn DHCP cho VMnet1 và chạy lại `sudo dhclient eth0`.
- Lỗi 2 - Sai tham số nhận diện hệ điều hành (`unrecognized option '-0'`):
  - *Nguyên nhân:* Gõ nhầm số 0 thay vì chữ O in hoa.
  - *Khắc phục:* Đổi lại thành `sudo nmap -O`.
- Lỗi 3 - Quét UDP bị treo lâu:
  - Nguyên nhân: Số lượng cổng quét quá lớn gặp cơ chế ICMP rate-limiting của Linux.
  - Khắc phục: Giới hạn số cổng quét qua `--top-ports 20`.
