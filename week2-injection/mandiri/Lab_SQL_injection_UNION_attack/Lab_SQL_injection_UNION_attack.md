# Portswigger lab Write-up: SQL Injection

<img width="1140" height="707" alt="Screenshot 2025-09-11 002933" src="https://github.com/user-attachments/assets/a68cc561-569c-4d3a-92f1-19ccb455d34d" />

## Challenge Overview

**Judul:** Lab: SQL injection UNION attack, retrieving data from other tables\
**Kategori:** SQL Injection\
**Link Soal:** [https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables)

Lab ini memiliki kerentanan SQL Injection pada filter kategori produk. Hasil query ditampilkan dalam respons aplikasi, sehingga memungkinkan penggunaan serangan UNION untuk mengambil data dari tabel lain.

Database memiliki tabel berbeda bernama users dengan kolom username dan password.

Untuk menyelesaikan lab, lakukan serangan SQL Injection UNION guna mengambil seluruh data username dan password, kemudian gunakan kredensial tersebut untuk login sebagai administrator.

## Alat Yang Digunakan

- **Web Browser**: Untuk berinteraksi dengan antarmuka login aplikasi.

## Pembahasan

Berikut adalah tampilan dari web yang akan di serang

<img width="1644" height="708" alt="Screenshot 2025-09-11 002243" src="https://github.com/user-attachments/assets/32997b91-e0f2-4a01-85bb-6394ae41e9bc" />


di parameter category kita bisa modifikasi lalu query sql akan dieksekusi langsung di server


Dengan menggunakan payload yang akan selalu true akan menampilkan table user untuk mendapatkan credential dari admin
```
'+UNION+SELECT+username,+password+FROM+users--
```

<img width="1285" height="56" alt="Screenshot 2025-09-11 002406" src="https://github.com/user-attachments/assets/4f56a325-5cb0-4307-b764-bafa1e1f7aa1" />


maka kita akan mendapatkan table user yang akan ditampilkan

<img width="1584" height="536" alt="Screenshot 2025-09-11 002414" src="https://github.com/user-attachments/assets/af119074-4a9c-42da-afda-a912ba7e4e60" />

setelah mendapatkan credential admin, langsung saja login sebagai admin

<img width="1069" height="391" alt="Screenshot 2025-09-11 002445" src="https://github.com/user-attachments/assets/de3a935c-8871-4ebf-b37f-eaf990398c59" />


