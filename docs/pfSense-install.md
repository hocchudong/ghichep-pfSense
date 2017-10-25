## Hướng dẫn cài đặt PFSense

### Mục tiêu LAB

Bài lab hướng dẫn việc cài đặt PFSense (sử dụng máy ảo VMWare WorkStation) và cấu hình card mạng cho PFSense.

## Mô hình 
- Sử dụng mô hình dưới để cài đặt
![img](../images/PFSense.jpg)

## IP Planning
- Phân hoạch IP cho các máy chủ trong mô hình trên
![img](../images/ip-planning-pfsense.jpg)

## Chuẩn bị và môi trường LAB
Hệ thống VMWare Workstation với các card mạng sau
- VMNet8: NAT ra Internet.
- VMnet3: LAN2 (giả lập mạng private trong PFSense)
- VMnet1: Host-only (quản lý PFSense)
![img](../images/vmware_net.jpg)
 

## Tạo máy ảo PFSense trên VMWare
- Cấu hình máy ảo và card mạng như sau:
![img](../images/configuration.jpg)
Lưu ý: OS chọn Other/FreeBSD (32 hoặc 64 tùy phiên bản)
![img](../images/free_bsd_version.jpg)

## Cài đặt PFSense
- Sau khi khởi động VM
![img](../images/pfsense_1.jpg)

- Vào mode cài đặt pfSense
![img](../images/pfsense_2.jpg)

- Lựa chọn cấu hình bàn phím sử dụng
![img](../images/pfsense_3.jpg)

- Phân vừng ổ cứng tự động, có thể lựa chọn Manual partion (không khuyến khích)
![img](../images/pfsense_4.jpg)
![img](../images/pfsense_5.jpg)

- Sau khi phân vùng sau, lựa chọn khởi động lại VM
![img](../images/pfsense_6.jpg)
![img](../images/pfsense_7.jpg)

## Cấu hình card mạng cho PFSense
- Sau khi VM khởi động lại, không tạo VLAN cho PFSense
![img](../images/pfsense_7_1.jpg)

- Lựa chọn card mạng cho WAN và LAN (mặc định PFSense shell chỉ cho phép khai báo 1 card LAN, card LAN2 sẽ khai báo trên PFsense web)
![img](../images/pfsense_8.jpg)
Xác nhận thông tin card mạng và chọn 'y'
![img](../images/pfsense_9.jpg)

- Card mạng đã nhận IP (nếu có DHCP Server), tiếp tục chọn '2' để set ip static cho card
![img](../images/pfsense_10.jpg)

- Cấu hình IP cho card mạng WAN như sau (từ trên xuống dưới)
	- Chọn '1' để set IP cho IP WAN
	- Chọn 'no' để không nhận IP từ DHCP
	- Đặt IP tĩnh
	- Đặt subnet mask
	- Đặt Gateway
	- Chọn 'no' để không nhận IP từ DHCPv6
	- 'Enter' để không đặt IPv6
![img](../images/pfsense_11.jpg)

- IP đã được đặt cho WAN, "Enter" để tiếp tục đặt IP cho LAN
![img](../images/pfsense_11_1.jpg)


- Cấu hình IP cho card mạng LAN, thực hiện tương tự như WAN.
Lưu ý: không đặt gateway và IPv6 cho card LAN
![img](../images/pfsense_12.jpg)

- Truy cập vào PFSense Web thông qua IP LAN
![img](../images/pfsense_13.jpg)
![img](../images/pfsense_14.jpg)

## Cấu hình LAN2 trên PFSense Web
- Tại tab Interfaces/InterfaceAssignments, add port 'em3'
![img](../images/pfsense_15.jpg)

- Chỉnh sửa thông số của interface mới thêm.
	- Enable interface
	- Đổi tên interface là LAN2
	- Lựa chọn Static Ipv4
	- Đặt IP cho interface và subnet mask tương ứng
![img](../images/pfsense_16.jpg)




Tham khảo:

[1] - http://www.iamasuperuser.com/install-pfsense/

[2] - https://www.tecmint.com/how-to-install-and-configure-pfsense/2/

