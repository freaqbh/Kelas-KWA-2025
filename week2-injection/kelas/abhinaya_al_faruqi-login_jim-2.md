# 🎯 Juice-Shop Write-up: Login Jim

<div align="center">

![Challenge Banner](https://github.com/user-attachments/assets/05a4903a-90fb-4dd4-94d5-8a1004dc9823)

**🎯 SQL Injection Challenge - Infiltrasi Akun Jim!**

</div>

---

## 📋 Informasi Challenge

| **Aspek** | **Detail** |
|-----------|------------|
| 🏷️ **Nama Challenge** | Login Jim |
| 🎯 **Kategori** | SQL Injection |
| ⭐ **Tingkat Kesulitan** | ⭐⭐⭐ (3/6) |
| 🔗 **Link Challenge** | [Juice Shop - Injection Category](https://juice-shop.herokuapp.com/#/score-board?categories=Injection) |
| 👤 **Target** | Akun Jim |

---

## 🎯 Objektif Challenge

Tantangan "Login Jim" mengharuskan peserta untuk:
- 🔍 Mengidentifikasi celah keamanan SQL Injection pada sistem autentikasi
- 🕵️ Melakukan reconnaissance untuk menemukan informasi target
- 🎭 Memanfaatkan vulnerability untuk mendapatkan akses ke akun Jim
- 🚪 Berhasil login sebagai user "Jim" tanpa mengetahui password aslinya

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

**✅ Konfirmasi Vulnerability:**
- Database error terekspos ke user interface
- Input validation tidak ada atau tidak memadai
- Sistem vulnerable terhadap SQL injection attacks

---

## 🎯 Tahap Information Gathering

### 📧 Step 1: Pencarian Email Target - Jim

Sebelum melakukan serangan SQL Injection, kita perlu mencari tahu email address dari user "Jim". Informasi ini bisa ditemukan melalui product reviews yang ada di aplikasi.

**🔍 Teknik OSINT (Open Source Intelligence):**
- 🛍️ Navigasi ke bagian product reviews
- 👀 Scan semua review yang tersedia
- 🎯 Identifikasi review yang dibuat oleh user "Jim"
- 📝 Catat email address yang tertera

![Email Discovery Process](https://github.com/user-attachments/assets/4c4e6165-99e7-44e8-827a-a01141469f1e)

**✅ Intelligence Gathered:**
- **Target Email:** `jim@juice-sh.op`
- **Source:** Product review section
- **Confidence Level:** High (direct observation)

---

## 🎯 Tahap Exploitation

### 🚀 Step 2: SQL Injection Attack - Authentication Bypass

Setelah mendapatkan email target melalui reconnaissance, saatnya melakukan serangan SQL Injection untuk bypass autentikasi.

**💉 Payload yang Digunakan:**
```sql
jim@juice-sh.op' --
```

**🔬 Analisis Payload:**
- `jim@juice-sh.op` - Email address target yang valid (dari intelligence gathering)
- `'` - Single quote untuk menutup string di query SQL
- `--` - SQL comment untuk mengabaikan sisa query (termasuk password validation)

**⚙️ Cara Kerja Payload:**

1. **Query SQL Asli (Estimasi):**
   ```sql
   SELECT * FROM users WHERE email = 'INPUT' AND password = 'PASSWORD'
   ```

2. **Setelah Injection:**
   ```sql
   SELECT * FROM users WHERE email = 'jim@juice-sh.op' -- ' AND password = 'PASSWORD'
   ```

3. **Query yang Dieksekusi:**
   ```sql
   SELECT * FROM users WHERE email = 'jim@juice-sh.op'
   ```
   - Bagian `AND password = 'PASSWORD'` diabaikan karena SQL comment (`--`)
   - Query hanya mengecek email, password validation ter-bypass
   - User Jim berhasil di-authenticate tanpa password

### 🎯 Eksekusi Serangan

![SQL Injection Execution](https://github.com/user-attachments/assets/91f760e4-dfcf-4408-ba95-0a3c3659b062)

**📝 Langkah Eksekusi:**
1. Masukkan payload `jim@juice-sh.op' --` di field email
2. Isi password dengan string random (akan diabaikan)
3. Klik login button
4. Observe response dari aplikasi

---

## 🎉 Hasil Serangan

### ✅ Berhasil Login sebagai Jim!

![Successful Login as Jim](https://github.com/user-attachments/assets/ad61e19e-d0c7-4111-bb66-ff05d98790ef)

**🎯 Achievement Unlocked:**
- 🔓 Bypass sistem autentikasi berhasil
- 🎭 Masuk ke akun Jim tanpa mengetahui password
- 🏆 Challenge level 3 diselesaikan dengan sukses
- 🕵️ Kombinasi OSINT + SQL Injection berhasil dieksekusi

---

## 📚 Technical Deep Dive

### 🔍 Mengapa Strategi Ini Berhasil?

**1. Information Gathering Phase:**
- Memanfaatkan information disclosure melalui public reviews
- OSINT (Open Source Intelligence) untuk target identification
- Social engineering aspect - menggunakan informasi yang tersedia publik

**2. SQL Injection Technique:**
- **Comment Injection:** Menggunakan `--` untuk comment out password check
- **Targeted Attack:** Spesifik ke user tertentu (bukan generic bypass)
- **Precision Strike:** Tidak mengganggu user lain atau sistem secara keseluruhan

**3. Attack Chain Analysis:**
```
Reconnaissance → Information Gathering → Payload Crafting → Exploitation → Access Gained
      ↓                    ↓                    ↓               ↓              ↓
   Find email         jim@juice-sh.op      jim@...op' --    SQL Injection   Login Success
```

---

## 🛡️ Security Analysis

### 🔒 Vulnerability Assessment

**📊 CVSS Score:** High (7.8+)

**🚨 Multiple Vulnerabilities Identified:**

1. **SQL Injection (CWE-89)**
   - **Impact:** HIGH
   - **Exploitability:** HIGH
   - **Attack Vector:** Network

2. **Information Disclosure (CWE-200)**
   - **Source:** Public product reviews
   - **Exposed Data:** User email addresses
   - **Privacy Impact:** MEDIUM

3. **Authentication Bypass (CWE-287)**
   - **Method:** SQL comment injection
   - **Privilege Impact:** HIGH
   - **Account Takeover:** Possible

### 🎯 Attack Complexity Analysis

**🔍 Attack Complexity:** MEDIUM
- Requires reconnaissance phase
- Need to identify target email first
- Multi-step attack chain
- Combines OSINT + SQL injection

**👤 Required Skills:**
- Basic SQL knowledge
- OSINT techniques
- Web application testing
- Understanding of comment injection

---

## 📚 Lessons Learned

### 🔒 Security Issues Identified:

**1. Input Validation Failures:**
- SQL queries constructed using string concatenation
- No input sanitization or validation
- Direct user input execution

**2. Information Disclosure:**
- User emails exposed in public reviews
- No privacy controls on user-generated content
- Potential for social engineering attacks

**3. Authentication Weaknesses:**
- Password validation can be bypassed
- No additional security layers (2FA, rate limiting)
- Single point of failure in authentication logic

### 🛡️ Comprehensive Mitigation Strategy:

**1. Immediate Security Fixes:**
```python
# VULNERABLE CODE:
query = f"SELECT * FROM users WHERE email = '{email}' AND password = '{password}'"

# SECURE CODE:
query = "SELECT * FROM users WHERE email = ? AND password = ?"
cursor.execute(query, (email, hash_password(password)))
```

**2. Data Privacy Improvements:**
- 🔒 Mask or anonymize user emails in public sections
- 👤 Implement user privacy controls
- 🛡️ Review all public data exposure points

**3. Authentication Hardening:**
- 🔐 Implement multi-factor authentication (MFA)
- ⏱️ Add rate limiting on login attempts
- 📊 Implement login anomaly detection
- 🚨 Add security logging and monitoring

**4. Application Security Controls:**
- 🛡️ Web Application Firewall (WAF) implementation
- 🧪 Regular security testing and code review
- 📚 Security awareness training for developers
- 🔍 Automated security scanning in CI/CD pipeline

---

## 🏷️ Tags

`#SQLInjection` `#OSINT` `#AuthenticationBypass` `#InformationGathering` `#WebSecurity` `#JuiceShop` `#CyberSecurity`

---

<div align="center">

**🎓 Write-up by: Abhinaya Al Faruqi**  
*Kelas KWA 2025 - Week 2: Injection Attacks*

**⭐ Challenge Rating: 3/6 - Advanced Intermediate**

</div>
