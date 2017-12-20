# Hướng dẫn cấu hình OpenVPN Client trên Ubuntu 16.04

## Thực hiện trên host Remote, kết nối VPN
  - Trên host Remote, cài đặt OpenVPN
    ```sh
    apt-get update -y
    appt-get install openvpn
    ```

  - Giải nén gói cấu hình OpenVPN download từ PFsense vào thư mục /etc/openvpn
  	```sh
  	apt-get install unzip -y
  	unzip pfSense-UDP4-1195-client1-config.zip -d /etc/openvpn
  	```
  	Sau khi giải nén, sẽ có các file
  	```sh
  	pfSense-UDP4-1195-client1.p12 
  	pfSense-UDP4-1195-client1-tls.key 
  	pfSense-UDP4-1195-client1.ovpn
  	```

  - Move các file key vào 1 thư mục riêng
  	```sh
  	cd /etc/openvpn
  	mkdir pfSense-UDP4-1195-client1
  	mv pfSense-UDP4-1195-client1.p12 pfSense-UDP4-1195-client1-tls.key pfSense-UDP4-1195-client1
  	```

  - Đổi tên file
  	```sh
  	cd /etc/openvpn
  	mv pfSense-UDP4-1195-client1.ovpn pfsense.conf
  	```

  - Chỉnh sửa lại file pfsense.conf như sau

  	```sh
  	# Khai báo cơ chế kết nối VPN (Trong bài lab này sử dụng cơ chế tap của OpenVPN nên khai báo `dev tap`, nếu dùng tun thì khai báo `dev tun`)
  	dev tap
	persist-tun
	persist-key
	cipher AES-128-CBC
	auth SHA256
	tls-client
	client
	resolv-retry infinite
	# Khai báo IP của OpenVPN Server kết nối tới và port
	remote [ip_provider_openvpn] 1194
	proto tcp-client
	verify-x509-name "server-cert" name
	# File khai báo user, password đăng nhập VPN
	auth-user-pass /etc/openvpn/pfSense-UDP4-1195-client1/pass
	# Khaibáo file pkcs12
	pkcs12 /etc/openvpn/pfSense-UDP4-1195-client1/pfSense-UDP4-1195-client1.p12
	# Khai báo file key
	tls-auth /etc/openvpn/pfSense-UDP4-1195-client1/pfSense-UDP4-1195-client1-tls.key 1
	remote-cert-tls server
	```	

  - Tạo file /etc/openvpn/pfSense-UDP4-1195-client1/pass để chứa username và password của user
  	```sh
  	client1
	password
  	```

  - Kết nối VPN
  	```sh
  	openvpn --config pfsense.conf
  	```

  - Kiểm tra bằng lệnh `ip a`, host đã nhận IP của Tunnel
  	```sh
  	tap0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 100
    link/ether 62:5a:62:23:ec:75 brd ff:ff:ff:ff:ff:ff
    inet 172.16.68.26/24 brd 172.16.68.255 scope global tap0
       valid_lft forever preferred_lft forever
    inet6 fe80::605a:62ff:fe23:ec75/64 scope link 
       valid_lft forever preferred_lft forever
  	```

  - Kiểm tra ping vào tới GW của dải 172.16.68.0/24
  	```sh
  	ping 172.16.68.1
  	```