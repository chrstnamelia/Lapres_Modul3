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
