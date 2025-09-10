# Portswigger lab Write-up: SQL Injection

<img width="1138" height="518" alt="Screenshot 2025-09-10 235923" src="https://github.com/user-attachments/assets/4be2bf62-60f7-45d2-b78f-fa78c51849ef" />


## Challenge Overview

**Judul:** Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data\
**Kategori:** SQL Injection\
**Link Soal:** https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

Lab ini memiliki kerentanan SQL Injection pada filter kategori produk. Saat pengguna memilih kategori, aplikasi menjalankan query SQL seperti berikut:
```
SELECT * FROM products WHERE category = 'Gifts' AND released = 1;
```
Untuk menyelesaikan lab ini, lakukan serangan SQL Injection yang membuat aplikasi menampilkan satu atau lebih produk yang belum dirilis.

## Alat Yang Digunakan

- **Web Browser**: Untuk berinteraksi dengan antarmuka login aplikasi.
- **Burp Suite**: Digunakan untuk mencegat dan memodifikasi permintaan HTTP guna menyisipkan kode SQL.

## Pembahasan

Berikut adalah tampilan dari web yang akan di serang

<img width="1889" height="1018" alt="Screenshot 2025-09-10 235931" src="https://github.com/user-attachments/assets/5110f2e7-f442-4f92-87e2-e4017b97f9ea" />

di parameter category kita bisa modifikasi lalu query sql akan dieksekusi langsung di server

<img width="1493" height="862" alt="Screenshot 2025-09-11 000534" src="https://github.com/user-attachments/assets/ecc7def0-3213-4a15-a633-3cc4b1a141ee" />

Dengan menggunakan payload yang akan selalu true akan menampilkan semua hidden content yang ada
```
'+OR+1=1--
```

<img width="1870" height="1003" alt="Screenshot 2025-09-11 000521" src="https://github.com/user-attachments/assets/1a7e5b00-7c02-48bd-9b24-578816957fdb" />



