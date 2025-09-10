# Juice-Shop Write-up: Login Bender
<img width="421" height="218" alt="Screenshot 2025-09-10 212258" src="https://github.com/user-attachments/assets/47c1b9ce-c5b3-4bdd-b1c5-f5142ff2cdda" />

## Soal

**Judul:** Login Bender\
**kategori:** SQL Injection\
**kesulitan:** ⭐⭐⭐ (3/6)\
**link soal:** https://juice-shop.herokuapp.com/#/score-board?categories=Injection

Tantangan ini, dengan judul "Login Bender," mengharuskan peserta memanfaatkan celah SQL Injection pada proses autentikasi sebuah aplikasi web untuk mendapatkan akses tidak sah ke akun Bender.

Setelah menganalisis mekanisme login, ditemukan bahwa memasukkan karakter khusus seperti tanda kutip (`'`) ke dalam kolom input memicu munculnya error SQL. Hal ini mengindikasikan adanya potensi serangan SQL Injection.

<img width="473" height="444" alt="Screenshot 2025-09-10 211457" src="https://github.com/user-attachments/assets/5394f9c1-62a3-4e6c-9b75-787d599ab9bd" />
<img width="955" height="325" alt="Screenshot 2025-09-10 211508" src="https://github.com/user-attachments/assets/e40e83a7-b21b-49dd-b93a-73b2152f1cca" />

Pertama, kita harus mencari email dari bender di product review

<img width="620" height="644" alt="Screenshot 2025-09-10 212720" src="https://github.com/user-attachments/assets/fcc464e6-1950-44ed-9ad2-6c95ae7f609c" />

Kedua, setelah mendapatkan email user dari jim kita masukkan payload untuk membypass passwordnya dengan payload berikut `bender@juice-sh.op' --`



Dan kita berhasil masuk sebagai Bender

