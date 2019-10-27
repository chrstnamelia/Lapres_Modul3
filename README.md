# Lapres_Modul3

topologi.sh
![image](https://user-images.githubusercontent.com/37492916/67633899-5d202680-f8e8-11e9-96bb-57cde245aae0.png)

Install isc-dhcp-server di MEWTWO 
`apt-get install isc-dhcp-server`

Install isc-dhcp-relay di PIKACHU
`apt-get install isc-dhcp-relay`

-Setelah berhasil, masukkan IP MEWTWO (10.151.73.67) sesuai yang diminta DHCP relay.

-Masukkan interface yang diminta dengan `eth1 eth2 eth3`

1. Client di subnet 2 mendapatkan peminjaman alamat IP dengan range 192.168.0.2 s.d. 192.168.0.10 dan 192.168.0.20 s.d. 192.168.0.30 dengan netmask 255.255.255.0.

2. Client di subnet 3 mendapatkan peminjaman alamat IP dengan range dari 192.168.1.2 s.d.
192.168.1.99 dengan netmask 255.255.255.0.

3. Client di subnet 2 mendapatkan peminjaman alamat IP selama 10 menit dan client di
subnet 3 mendapatkan peminjaman alamat IP selama 5 menit.

4. Masing-masing client harus mendapatkan konfigurasi DNS / nameserver yang terdiri atas
IP ARTICUNO , 202.46.129.2 , dan 10.151.36.7 secara otomatis.

5. Client PSYDUCK selalu mendapatkan IP 192.168.1.28, TANPA menggunakan
konfigurasi IP Statis.

Jawab: Pada MEWTWO

-Buka `nano /etc/default/isc-dhcp-server` dan sesuaikan dengan gambar dibawah.
![image](https://user-images.githubusercontent.com/37492916/67634136-aec9b080-f8ea-11e9-885f-1c0cdb4be1e6.png)

-Buka `nano /etc/dhcp/dhcpd.conf` lalu ketikkan config seperti dibawah.

```
subnet 10.151.73.64 netmask 255.255.255.248 {
} 

//NOMOR 1
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.2 192.168.0.10;
    range 192.168.0.20 192.168.0.30;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.73.66, 202.46.129.2, 10.151.36.7; //NOMOR 4
    default-lease-time 600; //NOMOR 3
    max-lease-time 7200;
} 

//NOMOR 2
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.2 192.168.1.99;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 10.151.73.66, 202.46.129.2, 10.151.36.7; //NOMOR 4
    default-lease-time 300; //NOMOR 3
    max-lease-time 7200;
}

//NOMOR 5
host psyduck {
    hardware ethernet ea:24:88:06:08:3b;
    fixed-address 192.168.1.28;
} 
```

-Setting semua IP Client di `nano /etc/network/interface` seperti berikut

```
auto eth0
iface eth0 inet dhcp
```

(khusus Client PSYDUCK tuliskan settingan seperti berikut)

```
auto eth0
iface eth0 inet dhcp
hwaddress ether ea:24:88:06:08:3b 
```

Restart service isc-dhcp-server dengan perintah
`service isc-dhcp-server restart`

Jika terjadi failed!, maka stop dulu, kemudian start kembali

```
service isc-dhcp-server stop
service isc-dhcp-server start
```

Nomor 1

![image](https://user-images.githubusercontent.com/37492916/67634429-64e2c980-f8ee-11e9-8675-cf4a98d4d583.png)

Nomor 2

![image](https://user-images.githubusercontent.com/37492916/67634448-99ef1c00-f8ee-11e9-93f7-a4d8e2fe7603.png)

![image](https://user-images.githubusercontent.com/37492916/67634467-cc991480-f8ee-11e9-8cd3-176669538124.png)

Nomor 4

![image](https://user-images.githubusercontent.com/37492916/67634484-18e45480-f8ef-11e9-8209-e64f4cc9bdbb.png)

![image](https://user-images.githubusercontent.com/37492916/67634406-1d5c3d80-f8ee-11e9-9895-092a437b25f5.png)

![image](https://user-images.githubusercontent.com/37492916/67634486-36192300-f8ef-11e9-9855-a4d04805986a.png)

Nomor 5

![image](https://user-images.githubusercontent.com/37492916/67634448-99ef1c00-f8ee-11e9-93f7-a4d8e2fe7603.png)


6. Prof. Oak meminta anda membuatkan user otentikasi dengan format:

● User : yy_moltres_user

● Password : yy_password

Note: yy adalah nama kelompok masing-masing. Contoh : a1_moltres_user


Jawab:

- Install squid3 pada UML MOLTRES `apt-get install squid3`

- Install apache2-utils pada UML MOLTRES `apt-get install apache2-utils`

- Backup terlebih dahulu file konfigurasi default yang disediakan squid. Ketikkan perintah berikut untuk melakukan backup:

`mv /etc/squid3/squid.conf /etc/squid3/squid.conf.bak`

- Buat user dan password baru. Ketikkan:

`htpasswd -c /etc/squid3/passwd a7_moltres_user` dan `password: a7_password`

-Buat konfigurasi baru dengan mengetikkan:

`nano /etc/squid3/squid.conf`

Tambahkan konfigurasi seperti di bawah ini

```
http_port 7777
visible_hostname mewtwo

auth_param basic program /usr/lib/squid3/ncsa_auth /etc/squid3/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

- Restart squid dengan cara mengetikkan perintah:

`service squid3 restart`


7. Untuk Client pada subnet 2 HANYA BISA mengakses internet pada hari Senin
s.d. Jumat pukul 11.00 s.d. 13.00 WIB

8. Untuk Client pada subnet 3 HANYA BISA mengakses internet pada hari Senin
s.d. Jumat pukul 20.00 s.d. 07.00 WIB pada keesokan harinya.

Jawab:

Buka `nano /etc/squid3/squid.conf` lalu ubah seperti berikut:

```
http_port 8888
visible_hostname moltres

auth_param basic program /usr/lib/squid3/ncsa_auth /etc/squid3/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

acl AKSES_T src 192.168.0.0/24
acl AKSES_P src 192.168.1.0/24
acl AKSES time MTWHF 11:00-13:00
acl AKSES_1 time MTWHF 20:00-24:00
acl AKSES_2 time TWHFA 00:00-07:00

http_access allow USERS AKSES_T AKSES
http_access allow USERS AKSES_P AKSES_1
http_access allow USERS AKSES_P AKSES_2

```

9. Setiap ada koneksi dari subnet AJK (10.151.36.0/24) yang mengakses google.com akan langsung dialihkan
menuju duckduckgo.com.

Jawab:

Buka `nano /etc/squid3/squid.conf` lalu tambahkan seperti berikut:

```
http_port 8888
visible_hostname moltres

auth_param basic program /usr/lib/squid3/ncsa_auth /etc/squid3/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

acl AKSES_T src 192.168.0.0/24
acl AKSES_P src 192.168.1.0/24
acl AKSES time MTWHF 11:00-13:00
acl AKSES_1 time MTWHF 20:00-24:00
acl AKSES_2 time TWHFA 00:00-07:00

acl roket dstdomain .google.com
acl sub_ajk src 10.151.36.0/24
deny_info http://duckduckgo.com sub_ajk
http_access deny roket sub_ajk

http_access allow USERS AKSES_T AKSES
http_access allow USERS AKSES_P AKSES_1
http_access allow USERS AKSES_P AKSES_2
http_access allow USERS
```

11. Karena menurut Prof. Oak menghafalkan IP Proxy kota Pallet cukup merepotkan, kalian diminta untuk mempermudah trainer dan penduduk dalam menggunakan Proxy yaitu cukup dengan mengetikkan proxy.yy.com dan memasukkan port 8888.

Jawab:

-Install aplikasi bind9 pada ARTICUNO dengan perintah: `apt-get install bind9 -y`

-Lakukan perintah pada ARTICUNO. Isikan seperti berikut: `nano /etc/bind/named.conf.local`

-Isikan konfigurasi domain proxy.a7.com sesuai dengan syntax berikut:

```
zone "proxy.a7.com"{
	type master;
	file "/etc/bind/jarkom/proxy.a7.com";
	};
 ```
 
-Buat folder jarkom di dalam /etc/bind `mkdir /etc/bind/jarkom`
 
-Copykan file db.local pada path /etc/bind ke dalam folder proxy.a7.com yang baru saja dibuat dan diubah namanya menjadi jarkom `cp /etc/bind/db.local /etc/bind/jarkom/proxy.a7.com`

-Buka `nano /etc/bind/jarkom/proxy.a7.com` dan ubah seperti gambar di bawah ini:

![image](https://user-images.githubusercontent.com/37492916/67634888-bd689580-f8f3-11e9-900c-f430525307b4.png)
 
-Restart bind9 dengan perintah `service bind9 restart`
