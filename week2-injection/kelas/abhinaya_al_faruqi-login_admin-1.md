# Juice-Shop Write-up: Login Admin
<img width="436" height="252" alt="Screenshot 2025-09-10 204936" src="https://github.com/user-attachments/assets/7be97b98-f536-4f4c-aac1-42faffd51ad1" />
## Soal

**Judul:** Login Admin\
**kategori:** SQL Injection\
**kesulitan:** ⭐⭐ (2/6)\
**link soal:** https://juice-shop.herokuapp.com/#/score-board?categories=Injection

Tantangan ini, dengan judul "Login Admin," mengharuskan peserta memanfaatkan celah SQL Injection pada proses autentikasi sebuah aplikasi web untuk mendapatkan akses tidak sah ke akun administrator.

<img width="469" height="438" alt="Screenshot 2025-09-10 205348" src="https://github.com/user-attachments/assets/ecc1ad24-f9c0-4d97-b005-01c40e1e1886" />

Setelah menganalisis mekanisme login, ditemukan bahwa memasukkan karakter khusus seperti tanda kutip (`'`) ke dalam kolom input memicu munculnya error SQL. Hal ini mengindikasikan adanya potensi serangan SQL Injection.
<img width="1001" height="383" alt="Screenshot 2025-09-10 205100" src="https://github.com/user-attachments/assets/3dc640df-90c6-4f2a-8b90-3629df776f24" />

Dengan menggunakan payload `' OR true` belum berhasil untuk masuk sebagai admin karena query yang lain masih terbaca dan kita harus mengcomment, dengan payload final `' OR true--` di email kita berhasil masuk sebagai admin
<img width="484" height="702" alt="Screenshot 2025-09-10 205909" src="https://github.com/user-attachments/assets/243e3199-88bb-4349-8955-e7ea93c24878" />

<img width="672" height="651" alt="Screenshot 2025-09-10 210800" src="https://github.com/user-attachments/assets/055234e8-f334-4407-ac7f-4c18858fd233" />
