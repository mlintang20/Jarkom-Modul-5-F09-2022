# Jarkom-Modul-4-F09-2022

## Anggota Kelompok

<table>
    <tr>
	    <th>Nama</th>
        <th>NRP</th>
    </tr>
    <tr>
        <td>Muhammad Lintang Panjerino</td>
        <td>5025201045</td>
    </tr>
    <tr>
        <td>Sayid Ziyad Ibrahim Alaydrus</td>
        <td>5025201147</td>
    </tr>
    <tr>
        <td>Wahyu Tri Saputro</td>
        <td>5025201217</td>
    </tr>
<table>

### C

Soal: Anya, putri pertama Loid, juga berpesan kepada anda agar melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung

Jawab: Routing yang dilakukan adalah _static routing_ yang dilakukan pada router Strix sebagai router utama. Routing diarahkan ke semua subnet di router Westalis dan router Ostania yang tidak terhubung secara langsung dengan router Strix. Hal ini dilakukan agar semua node dapat terhubung internet.

```
route add -net 10.33.4.0 netmask 255.255.252.0 gw 10.33.0.2
route add -net 10.33.0.128 netmask 255.255.255.128 gw 10.33.0.2
route add -net 10.33.0.8 netmask 255.255.255.248 gw 10.33.0.2

route add -net 10.33.2.0 netmask 255.255.254.0 gw 10.33.0.6
route add -net 10.33.1.0 netmask 255.255.255.0 gw 10.33.0.6
route add -net 10.33.0.16 netmask 255.255.255.248 gw 10.33.0.6
```

### D

Soal: Tugas berikutnya adalah memberikan ip pada subnet Forger, Desmond, Blackbell, dan Briar secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya

Jawab: Untuk melakukan pembagian IP dengan cara DHCP maka diperlukan beberapa setting di DHCP Server dan DHCP Relay. Pada DHCP Server diperlukan pengaturan pada file `/etc/default/isc-dhcp-server` dan file `/etc/dhcp/dhcpd.conf`. Sedangkan pada DHCP Relay dilakukan setting pada file `/etc/default/isc-dhcp-relay`.

Pada `/etc/default/isc-dhcp-server`:

```
# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
# Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACES="eth0"
```

Pada `/etc/dhcp/dhcpd.conf`:

```
# subnet A7
subnet 10.33.0.8 netmask 255.255.255.248{
}

# subnet A3
subnet 10.33.0.128 netmask 255.255.255.128 {
    range 10.33.0.130 10.33.0.254;
    option routers 10.33.0.129;
    option broadcast-address 10.33.0.255;
    option domain-name-servers 10.33.0.10;
    default-lease-time 360;
    max-lease-time 7200;
}

# Subnet A6
subnet 10.33.4.0 netmask 255.255.252.0 {
    range 10.33.4.2 10.33.7.254;
    option routers 10.33.4.1;
    option broadcast-address 10.33.7.255;
    option domain-name-servers 10.33.0.10;
    default-lease-time 360;
    max-lease-time 7200;
}

# Subnet A5
subnet 10.33.2.0 netmask 255.255.254.0 {
    range 10.33.2.2 10.33.3.254;
    option routers 10.33.2.1;
    option broadcast-address 10.33.3.255;
    option domain-name-servers 10.33.0.10;
    default-lease-time 360;
    max-lease-time 7200;
}

# Subnet A4
subnet 10.33.1.0 netmask 255.255.255.0 {
    range 10.33.1.2 10.33.1.254;
    option routers 10.33.1.1;
    option broadcast-address 10.33.1.255;
    option domain-name-servers 10.33.0.10;
    default-lease-time 360;
    max-lease-time 7200;
}
```

Setelah itu jangan lupa restart service DHCP Server

```
service isc-dhcp-server restart
```

Pada file `/etc/default/isc-dhcp-relay`:

```
# What servers should the DHCP relay forward requests to?
SERVERS="10.33.0.11"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```

Setelah itu jangan lupa restart service DHCP Relay

```
service isc-dhcp-relay restart
```
