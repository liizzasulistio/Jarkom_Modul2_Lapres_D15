# Jarkom_Modul2_Lapres_D15
Praktikum mata kuliah Jaringan Komputer Informatika ITS 2020 Kelompok D15
- Ammar Alifian Fahdan (05111840000007)
- Lii'zza Aisyah Putri Sulistio (05111840000073)

## Persiapan
- Buat file untuk topologi router, server, dan client dengan `nano topologi.sh` kemudian jalankan dengan `bash topologi.sh`.
- Buat file dengan `nano bye.sh` dan jalankan dengan `bash bye.sh` ketika ingin menghentikan UML.
- Setting masing-masing UML berdasarkan IP yang sudah ditentukan di `/etc/network/interfaces`.
- Jalankan `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16` pada UML SURABAYA.
- Export proxy dengan cara membuat file `nano proxy.sh` yang dapat dijalankan dengan perintah `source proxy.sh` di masing-masing UML.
- Jalankan `apt-get update` di masing-masing UML.
- Install `apt-get install bind9` pada UML MALANG dan MOJOKERTO.

## DNS
Karena `yyy` merupakan nama kelompok, maka `semeruyyy.pw` diubah menjadi `semerud15.pw`

Konfigurasi UML MALANG dan MOJOKERTO secara keseluruhan:

<img width="496" alt="Screen Shot 2020-11-15 at 10 42 15" src="https://user-images.githubusercontent.com/58472359/99178776-3af28400-2749-11eb-8498-092b018f1623.png">
<img width="512" alt="Screen Shot 2020-11-15 at 10 44 28" src="https://user-images.githubusercontent.com/58472359/99178784-45ad1900-2749-11eb-9bd4-1d502ac8b84a.png">
<img width="496" alt="Screen Shot 2020-11-15 at 10 42 44" src="https://user-images.githubusercontent.com/58472359/99178778-3c23b100-2749-11eb-9c61-1b72a85c7459.png">
<img width="512" alt="Screen Shot 2020-11-15 at 10 54 46" src="https://user-images.githubusercontent.com/58472359/99178794-5e1d3380-2749-11eb-80ec-cef5ff712257.png">

<img width="512" alt="Screen Shot 2020-11-15 at 10 57 37" src="https://user-images.githubusercontent.com/58472359/99178839-e996c480-2749-11eb-8498-d7ce15e6358e.png">
<img width="496" alt="Screen Shot 2020-11-15 at 10 59 31" src="https://user-images.githubusercontent.com/58472359/99178841-eac7f180-2749-11eb-96b7-bd7e60fecef2.png">
<img width="496" alt="Screen Shot 2020-11-15 at 11 02 29" src="https://user-images.githubusercontent.com/58472359/99178853-016e4880-274a-11eb-9577-552fcc57c2f3.png">

### 1. Buat sebuah website utama dengan alamat `http://semerud15.pw`
   - Pada UML MALANG, buka file `etc/bind/named.conf.local` dan isikan konfigurasi domain sebagai berikut:
   ~~~
   zone "semerud15.pw {
        type master;
        file "/etc/bind/jarkom/semerud15.pw";
   };
   ~~~
   - Pengecekan dapat dilihat pada no. 2 dan 3
   
### 2. dan 3. Website tersebut memiliki alias `http://www.semerud15.pw` dan Subdomain `http://penanjakan.semerud15.pw` yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO
  - Setelah itu copy file db.local dengan cara berikut `cp /etc/bind/db.local /etc/bind/jarkom/semerud15.pw`
  - Kemudian buka file `etc/bind/jarkom/semerud15.pw` ubah dan isikan konfigurasi domain sebagai berikut:
   ~~~
   ...
   @           IN SOA         semerud15.pw. root.semerud15.pw.(
   ...
   @           IN NS    semerud15.pw.
   @           IN A     10.151.79.132  ;IP PROBOLINGGO
   www         IN CNAME semerud15.pw.
   penanjakan  IN A     10.151.79.132   ;IP PROBOLINGGO
   ~~~
 - Cek dengan cara melakukan `ping [domain]` pada GRESIK dan SIDOARJO, jika sudah menunjukkan IP Server PROBOLINGGO maka jawaban sudah benar.
 - Lakukan `ping semerud15.pw` dan`ping www.semerud15.pw`
 <img width="512" alt="Screen Shot 2020-11-15 at 18 27 45" src="https://user-images.githubusercontent.com/58472359/99183924-21b1fd80-2772-11eb-9d14-563c2052a3fc.png">
 - Lakukan `ping penanjakan.semerud15.pw`
 <img width="496" alt="Screen Shot 2020-11-15 at 11 58 13" src="https://user-images.githubusercontent.com/58472359/99183926-28407500-2772-11eb-878d-e649ace40ad4.png">

### 4. Buat reverse domain untuk domain utama
   - Buka file `etc/bind/named.conf.local` kemudian tambahkan konfigurasi berikut ini:
   ~~~
   zone "79.151.10.in-addr.arpa" {
        type master;
        file "/etc/bind/jarkom/79.151.10.in-addr.arpa";
   };
   ~~~
   - Kemudian buka juga file `/etc/bind/jarkom/79.151.10.in-addr.arpa` dan isikan konfigurasi berikut ini:
   ~~~
   ...
   @           IN SOA         semerud15.pw. root.semerud15.pw.(
   ...
   79.151.10.in-addr.arpa  IN NS    semerud15.pw.
   132                     IN PTR   semerud15.pw.  ;BYTE KE-4 IP PROBOLINGGO
   ~~~
   - Lakukan pengecekan dengan cara `host -t PTR 10.151.79.132`
   <img width="496" alt="Screen Shot 2020-11-15 at 10 55 12" src="https://user-images.githubusercontent.com/58472359/99183935-3a221800-2772-11eb-8e3c-e4422a859b17.png">
   
   
### 5. Buat DNS slave pada MOJOKERTO
   - Pada MALANG ubah lalu tambahkan konfigurasi berikut ini di file `/etc/bind/named.conf.local`
   ~~~
    zone "semerud15.pw {
        type master;
        notify yes;
        also-notify { 10.151.79.131; }; //IP MOJOKERTO
        allow-transfer { 10.151.79.131; }; //IP MOJOKERTO
        file "/etc/bind/jarkom/semerud15.pw";
   };
   ~~~
   - Kemudian buka file `/etc/bind/named.conf.local` dan tambahkan konfigurasi berikut ini:
   ~~~
   zone "semerud15.pw {
       type slave;
       masters { 10.151.79.130; }; //IP MALANG
       file "/var/lib/bind/semerud15.pw";
       allow-transfer { any; };
   };
   ~~~
   - Lakukan pengecekan dengan menjalankan `service bind9 stop` pada MALANG dan `ping semerud15.pw` di GRESIK atau SIDOARJO.
   - Jika ping dapat dilakukan, maka pembuatan DNS slave berhasil.
   <img width="496" alt="Screen Shot 2020-11-15 at 12 07 02" src="https://user-images.githubusercontent.com/58472359/99183972-62aa1200-2772-11eb-8255-c05529f2560d.png">
<img width="496" alt="Screen Shot 2020-11-15 at 11 56 59" src="https://user-images.githubusercontent.com/58472359/99183975-6c337a00-2772-11eb-82d7-f4c98aca616b.png">
   
### 6. dan 7. Buat subdomain dengan alamat `http://gunung.semerud15.pw` yang didelegasikan pada server MOJOKERTO dan Buat subdomain dengan alamat `http://naik.gunung.semerud15.pw` yang keduanya diarahkan pada IP Server PROBOLINGGO
   - Pada MALANG tambahkan konfigurasi berikut ini di file `/etc/bind/jarkom/semerud15.pw`
   ~~~
   ...
   ns1      IN A   10.151.79.131  ;IP MOJOKERTO
   gunung   IN NS  ns1
   ...
   ~~~
   - Kemudian di MOJOKERTO, buka file `/etc/bind/named.conf.local` dan tambahkan konfigurasi berikut ini:
   ~~~
   zone "gunung.semerud15.pw" {
       type master;
       file "/etc/bind/delegasi/gunung.semerud15.pw";
       allow-transfer { any; };
   };
   ~~~
   - Setelah itu buka juga file `/etc/bind/delegasi/gunung.semerud15.pw` dan tambahkan konfigurasi berikut ini:
   ~~~
   ...
   @     IN SOA   gunung.semerud15.pw. root.gunung.semerud15.pw.(
   ...
   @     IN NS    gunung.semerud15.pw.
   @     IN A     10.151.79.132  ;IP PROBOLINGGO
   naik  IN A     gunung.semerud15.pw.
   ~~~
   - Lakukan pengecekan dengan cara `ping gunung.semerud15.pw` dan `ping naik.gunung.semerud15.pw`
   <img width="496" alt="Screen Shot 2020-11-15 at 11 49 50" src="https://user-images.githubusercontent.com/58472359/99183985-82d9d100-2772-11eb-8a9c-18988f724238.png">
<img width="512" alt="Screen Shot 2020-11-15 at 11 02 40" src="https://user-images.githubusercontent.com/58472359/99184004-95540a80-2772-11eb-8165-febd0b9a6893.png">

   
## Web Server
### 8.
