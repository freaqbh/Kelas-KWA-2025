# Juice-Shop Write-up: Login Admin
<img width="424" height="222" alt="image" src="https://github.com/user-attachments/assets/05a4903a-90fb-4dd4-94d5-8a1004dc9823" />

## Soal

**Judul:** Login Jim\
**kategori:** SQL Injection\
**kesulitan:** ⭐⭐⭐ (3/6)\
**link soal:** https://juice-shop.herokuapp.com/#/score-board?categories=Injection

Tantangan ini, dengan judul "Login Jim," mengharuskan peserta memanfaatkan celah SQL Injection pada proses autentikasi sebuah aplikasi web untuk mendapatkan akses tidak sah ke akun Jim.

Setelah menganalisis mekanisme login, ditemukan bahwa memasukkan karakter khusus seperti tanda kutip (`'`) ke dalam kolom input memicu munculnya error SQL. Hal ini mengindikasikan adanya potensi serangan SQL Injection.

<img width="473" height="444" alt="Screenshot 2025-09-10 211457" src="https://github.com/user-attachments/assets/5394f9c1-62a3-4e6c-9b75-787d599ab9bd" />
<img width="955" height="325" alt="Screenshot 2025-09-10 211508" src="https://github.com/user-attachments/assets/e40e83a7-b21b-49dd-b93a-73b2152f1cca" />

Pertama, kita harus mencari email dari jim di product review

<img width="606" height="550" alt="Screenshot 2025-09-10 211751" src="https://github.com/user-attachments/assets/4c4e6165-99e7-44e8-827a-a01141469f1e" />

Kedua, setelah mendapatkan email user dari jim kita masukkan payload untuk membypass passwordnya dengan payload berikut `jim@juice-sh.op' --`

<img width="470" height="426" alt="Screenshot 2025-09-10 211905" src="https://github.com/user-attachments/assets/91f760e4-dfcf-4408-ba95-0a3c3659b062" />

Dan kita berhasil masuk sebagai jim

<img width="586" height="423" alt="Screenshot 2025-09-10 211915" src="https://github.com/user-attachments/assets/ad61e19e-d0c7-4111-bb66-ff05d98790ef" />
