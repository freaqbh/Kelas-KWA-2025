# 🤖 Juice-Shop Write-up: Login Bender

<div align="center">

![Challenge Banner](https://github.com/user-attachments/assets/47c1b9ce-c5b3-4bdd-b1c5-f5142ff2cdda)

**🎯 SQL Injection Challenge - Berhasil Membobol Akun Bender!**

</div>

---

## 📋 Informasi Challenge

| **Aspek** | **Detail** |
|-----------|------------|
| 🏷️ **Nama Challenge** | Login Bender |
| 🎯 **Kategori** | SQL Injection |
| ⭐ **Tingkat Kesulitan** | ⭐⭐⭐ (3/6) |
| 🔗 **Link Challenge** | [Juice Shop - Injection Category](https://juice-shop.herokuapp.com/#/score-board?categories=Injection) |
| 👤 **Target** | Akun Bender |

---

## 🎯 Objektif Challenge

Tantangan "Login Bender" mengharuskan peserta untuk:
- 🔍 Mengidentifikasi celah keamanan SQL Injection pada sistem autentikasi
- 🎭 Memanfaatkan vulnerability tersebut untuk mendapatkan akses tidak sah
- 🚪 Berhasil login sebagai user "Bender" tanpa mengetahui password aslinya

---

## 🔍 Tahap Reconnaissance

### 🕵️ Analisis Mekanisme Login

Langkah pertama dalam serangan ini adalah menganalisis bagaimana sistem login bekerja. Dengan memasukkan karakter khusus seperti tanda kutip (`'`) ke dalam form login, kita dapat mengidentifikasi adanya potensi SQL Injection.

**🚨 Indikator Vulnerability:**
- Error SQL muncul ketika karakter khusus diinputkan
- Aplikasi tidak melakukan sanitasi input dengan proper
- Query SQL langsung mengeksekusi input user tanpa validasi

![SQL Error Detection](https://github.com/user-attachments/assets/5394f9c1-62a3-4e6c-9b75-787d599ab9bd)

![Error Message Detail](https://github.com/user-attachments/assets/e40e83a7-b21b-49dd-b93a-73b2152f1cca)

---

## 🎯 Tahap Exploitation

### 📧 Step 1: Information Gathering - Mencari Email Target

Sebelum melakukan serangan SQL Injection, kita perlu mencari tahu email address dari user "Bender". Informasi ini bisa ditemukan melalui product reviews yang ada di aplikasi.

**🔍 Teknik Pencarian:**
- Navigasi ke bagian product reviews
- Cari review yang dibuat oleh user "Bender"
- Catat email address yang tertera

![Email Discovery](https://github.com/user-attachments/assets/fcc464e6-1950-44ed-9ad2-6c95ae7f609c)

**✅ Email Ditemukan:** `bender@juice-sh.op`

### 🚀 Step 2: SQL Injection Attack - Bypass Authentication

Setelah mendapatkan email target, saatnya melakukan serangan SQL Injection untuk bypass autentikasi.

**💉 Payload yang Digunakan:**

![Attack](https://github.com/user-attachments/assets/355526bf-5aa9-455b-b6d8-8133136f4ad6)

```sql
bender@juice-sh.op' --
```

**🔬 Analisis Payload:**
- `bender@juice-sh.op` - Email address target yang valid
- `'` - Single quote untuk menutup string di query SQL
- `--` - SQL comment untuk mengabaikan sisa query (termasuk password check)

**⚙️ Cara Kerja Payload:**
1. Query SQL asli mungkin berbentuk:
   ```sql
   SELECT * FROM users WHERE email = 'INPUT' AND password = 'PASSWORD'
   ```

2. Setelah injection, query menjadi:
   ```sql
   SELECT * FROM users WHERE email = 'bender@juice-sh.op' -- ' AND password = 'PASSWORD'
   ```

3. Bagian password check diabaikan karena SQL comment (`--`)

---

## 🎉 Hasil Serangan

### ✅ Berhasil Login sebagai Bender!

![Success](https://github.com/user-attachments/assets/414882f8-bc86-4425-a955-8aefb28aa143)

Dengan menggunakan payload SQL Injection yang tepat, kita berhasil:
- 🔓 Bypass mekanisme autentikasi
- 🎭 Masuk ke akun Bender tanpa mengetahui password
- 🏆 Menyelesaikan challenge dengan sukses

**🎯 Challenge Completed!** ⭐⭐⭐

---

## 📚 Lessons Learned

### 🔒 Vulnerability yang Ditemukan:
- **SQL Injection** - Input tidak disanitasi dengan proper
- **Information Disclosure** - Email user terekspos di public reviews
- **Authentication Bypass** - Validasi password dapat diabaikan

### 🛡️ Mitigasi yang Disarankan:
1. **Prepared Statements** - Gunakan parameterized queries
2. **Input Validation** - Sanitasi semua input dari user
3. **Least Privilege** - Database user dengan permission minimal
4. **Error Handling** - Jangan expose error SQL ke user

---

## 🏷️ Tags

`#SQLInjection` `#WebSecurity` `#OWASP` `#JuiceShop` `#AuthenticationBypass` `#CyberSecurity`

---

<div align="center">

**🎓 Write-up by: Abhinaya Al Faruqi**  
*Kelas KWA 2025 - Week 2: Injection Attacks*

</div>

