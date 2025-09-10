# 👑 Juice-Shop Write-up: Login Admin

<div align="center">

![Challenge Banner](https://github.com/user-attachments/assets/7be97b98-f536-4f4c-aac1-42faffd51ad1)

**🎯 SQL Injection Challenge - Menembus Akses Administrator!**

</div>

---

## 📋 Informasi Challenge

| **Aspek** | **Detail** |
|-----------|------------|
| 🏷️ **Nama Challenge** | Login Admin |
| 🎯 **Kategori** | SQL Injection |
| ⭐ **Tingkat Kesulitan** | ⭐⭐ (2/6) |
| 🔗 **Link Challenge** | [Juice Shop - Injection Category](https://juice-shop.herokuapp.com/#/score-board?categories=Injection) |
| 👤 **Target** | Akun Administrator |

---

## 🎯 Objektif Challenge

Tantangan "Login Admin" mengharuskan peserta untuk:
- 🔍 Mengidentifikasi celah keamanan SQL Injection pada sistem login
- 👑 Memanfaatkan vulnerability untuk mendapatkan akses administrator
- 🚪 Berhasil login sebagai admin tanpa kredensial yang valid

---

## 🔍 Tahap Reconnaissance

### 🕵️ Analisis Form Login

![Login Form Analysis](https://github.com/user-attachments/assets/ecc1ad24-f9c0-4d97-b005-01c40e1e1886)

Langkah pertama adalah menganalisis mekanisme autentikasi aplikasi. Dengan melakukan testing pada form login menggunakan karakter khusus, kita dapat mengidentifikasi potensi SQL Injection.

**🚨 Testing Vulnerability:**
- Input karakter khusus seperti tanda kutip (`'`) pada field email
- Observasi response aplikasi terhadap input yang tidak biasa
- Identifikasi error message yang mengindikasikan SQL vulnerability

### 💥 Konfirmasi SQL Injection

![SQL Error Detection](https://github.com/user-attachments/assets/3dc640df-90c6-4f2a-8b90-3629df776f24)

**✅ Indikator Positif:**
- Error SQL muncul ketika karakter `'` diinputkan
- Aplikasi tidak melakukan proper input sanitization
- Database error terekspos ke user interface

---

## 🎯 Tahap Exploitation

### 🧪 Percobaan Payload Awal

**💉 Payload Pertama:**
```sql
' OR true
```

**❌ Hasil:** Gagal login sebagai admin

**🔍 Analisis Kegagalan:**
- Query SQL masih memiliki bagian lain yang perlu dihandle
- Kondisi `true` saja tidak cukup untuk bypass complete authentication
- Perlu mengabaikan sisa query yang mungkin mengganggu

### 🚀 Payload Final - Berhasil!

**💉 Payload yang Berhasil:**
```sql
' OR true--
```

**🔬 Analisis Payload:**
- `'` - Single quote untuk menutup string di query SQL
- `OR true` - Kondisi yang selalu bernilai true
- `--` - SQL comment untuk mengabaikan sisa query

**⚙️ Cara Kerja Payload:**

1. **Query SQL Asli:**
   ```sql
   SELECT * FROM users WHERE email = 'INPUT' AND password = 'PASSWORD'
   ```

2. **Setelah Injection:**
   ```sql
   SELECT * FROM users WHERE email = '' OR true-- ' AND password = 'PASSWORD'
   ```

3. **Query yang Dieksekusi:**
   ```sql
   SELECT * FROM users WHERE email = '' OR true
   ```
   - Bagian `AND password = 'PASSWORD'` diabaikan karena comment (`--`)
   - Kondisi `OR true` membuat query mengembalikan semua user
   - User pertama dalam database (biasanya admin) akan di-return

---

## 🎉 Hasil Serangan

### ✅ Berhasil Masuk sebagai Administrator!

![Successful Login](https://github.com/user-attachments/assets/243e3199-88bb-4349-8955-e7ea93c24878)

![Admin Dashboard Access](https://github.com/user-attachments/assets/055234e8-f334-4407-ac7f-4c18858fd233)

**🎯 Achievement Unlocked:**
- 🔓 Bypass sistem autentikasi berhasil
- 👑 Akses administrator diperoleh tanpa kredensial valid
- 🏆 Challenge level 2 diselesaikan dengan sukses

---

## 📚 Technical Deep Dive

### 🔍 Mengapa Payload Ini Berhasil?

**1. SQL Logic Manipulation:**
- Kondisi `OR true` membuat WHERE clause selalu bernilai true
- Database mengembalikan record pertama yang ditemukan
- Dalam kebanyakan sistem, user pertama adalah administrator

**2. Comment Injection:**
- `--` adalah syntax comment di SQL
- Semua text setelah `--` diabaikan oleh database
- Password validation ter-bypass karena di-comment out

**3. Authentication Bypass Pattern:**
```sql
-- Original intended query:
WHERE email = 'user@email.com' AND password = 'userpassword'

-- After injection:
WHERE email = '' OR true-- AND password = 'anything'

-- Effectively becomes:
WHERE email = '' OR true
```

---

## 🛡️ Security Analysis

### 🔒 Vulnerability Assessment

**📊 CVSS Score:** High (7.5+)

**🚨 Risk Factors:**
- **Confidentiality Impact:** HIGH - Akses ke data sensitif
- **Integrity Impact:** HIGH - Kemampuan modify data
- **Availability Impact:** MEDIUM - Potensi DoS melalui malicious queries

### 🎯 Attack Vector Analysis

**🔍 Attack Complexity:** LOW
- Tidak memerlukan tools khusus
- Dapat dilakukan melalui browser biasa
- Payload sederhana dan mudah dieksekusi

**👤 Required Privileges:** NONE
- Tidak perlu akun yang sudah ada
- Public access point (login form)
- No authentication required untuk testing

---

## 📚 Lessons Learned

### 🔒 Vulnerability yang Ditemukan:
- **SQL Injection** - Input tidak disanitasi dengan proper
- **Authentication Bypass** - Logic flaw dalam query construction
- **Information Disclosure** - Database errors terekspos ke user
- **Privilege Escalation** - Direct access ke admin account

### 🛡️ Mitigasi yang Disarankan:

**1. Immediate Actions:**
- ✅ Implementasi prepared statements/parameterized queries
- ✅ Input validation dan sanitization
- ✅ Error handling yang tidak expose system details

**2. Long-term Security Measures:**
- 🔐 Implementasi Web Application Firewall (WAF)
- 📊 Security logging dan monitoring
- 🧪 Regular penetration testing
- 👥 Security awareness training untuk developers

**3. Code Example - Secure Implementation:**
```python
# VULNERABLE CODE:
query = f"SELECT * FROM users WHERE email = '{email}' AND password = '{password}'"

# SECURE CODE:
query = "SELECT * FROM users WHERE email = ? AND password = ?"
cursor.execute(query, (email, hashed_password))
```

---

## 🏷️ Tags

`#SQLInjection` `#AuthenticationBypass` `#WebSecurity` `#OWASP` `#JuiceShop` `#AdminAccess` `#CyberSecurity`

---

<div align="center">

**🎓 Write-up by: Abhinaya Al Faruqi**  
*Kelas KWA 2025 - Week 2: Injection Attacks*

**⭐ Challenge Rating: 2/6 - Intermediate**

</div>
