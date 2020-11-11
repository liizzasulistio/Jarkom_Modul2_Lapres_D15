# Jarkom_Modul2_Lapres_D15
Praktikum mata kuliah Jaringan Komputer Informatika ITS 2020 Kelompok D15
- Ammar Alifian Fahdan (05111840000007)
- Lii'zza Aisyah Putri Sulistio (05111840000073)

## Persiapan
- Buat file untuk topologi router, server, dan client dengan `nano topologi.sh` kemudian jalankan dengan `bash topologi.sh`.
- Buat file dengan `nano bye.sh` dan jalankan dengan `bash bye.sh` ketika ingin menghentikan UML.
- Setting masing-masing UML berdasarkan IP yang sudah ditentukan.
- Export proxy dengan cara membuat file `nano proxy.sh` yang dapat dijalankan dengan perintah `source proxy.sh` di masing-masing UML.
- Jalankan `apt-get update` di masing-masing UML.

## DNS
1. Buat sebuah website utama dengan alamat `http://semeruyyy.pw`
2. Website tersebut memiliki alias `http://www.semeruyyy.pw`
3. Subdomain `http://penanjakan.semeruyyy.pw` yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO
4. Buat reverse domain untuk domain utama
5. Buat DNS slave pada Mojokerto
6. Buat subdomain dengan alamat `http://gunung.semeruyyy.pw` yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO
7. Buat subdomain dengan alamat `http://naik.gunung.semeruyyy.pw` dan diarahkan pada IP Server PROBOLINGGO
