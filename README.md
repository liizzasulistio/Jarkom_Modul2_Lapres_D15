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

1. Buat sebuah website utama dengan alamat `http://semerud15.pw`
   Pada UML MALANG, buka file `etc/bind/named.conf.local` dan isikan konfigurasi domain sebagai berikut:
   ~~~
   zone "semerud15.pw {
        type master;
        file "/etc/bind/jarkom/semerud15.pw";
   }
   ~~~
2. dan 3. Website tersebut memiliki alias `http://www.semerud15.pw` dan Subdomain `http://penanjakan.semerud15.pw` yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO
   Setelah itu copy file db.local dengan cara berikut `cp /etc/bind/db.local /etc/bind/jarkom/semerud15.pw`
   Kemudian buka file `etc/bind/jarkom/semerud15.pw` dan isikan konfigurasi domain sebagai berikut:
   ~~~
   
   ~~~

4. Buat reverse domain untuk domain utama
5. Buat DNS slave pada Mojokerto
6. Buat subdomain dengan alamat `http://gunung.semerud15.pw` yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO
7. Buat subdomain dengan alamat `http://naik.gunung.semerud15.pw` dan diarahkan pada IP Server PROBOLINGGO
