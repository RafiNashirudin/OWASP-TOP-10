# SQL Injection Proof of Concept (PoC) Using SQLMap

## Deskripsi
Dokumen ini adalah *Proof of Concept (PoC)* untuk menguji kerentanan SQL Injection pada endpoint `http://192.168.1.5/users/login.php` menggunakan `sqlmap`. Uji coba ini bertujuan untuk mengeksplorasi database, menemukan tabel pengguna, dan meninjau cara penyimpanan password pengguna di database.

> **Peringatan**: Ini adalah PoC yang hanya boleh digunakan pada sistem yang Anda miliki atau memiliki izin resmi untuk diuji. Mengakses atau memodifikasi data tanpa izin melanggar hukum.

---

## Prasyarat
1. **sqlmap** terinstal di sistem Anda. Untuk mengunduh, kunjungi [sqlmap.org](https://sqlmap.org/).
2. Akses ke endpoint `http://192.168.1.5/users/login.php`.

---

## Langkah-Langkah Eksekusi

### 1. Menemukan Database yang Tersedia
Gunakan perintah berikut untuk menemukan database yang tersedia di server:

```bash
sqlmap -u "http://192.168.1.5/users/login.php" --data="username=admin&password=password" --dbs
```

#### Penjelasan Parameter
- `-u "http://192.168.1.5/users/login.php"`: Menentukan URL target.
- `--data="username=admin&password=password"`: Mengirim data POST dengan parameter `username` dan `password`.
- `--dbs`: Meminta `sqlmap` untuk menampilkan daftar database yang tersedia di server.

#### Output
Output dari langkah ini akan menampilkan daftar database yang tersedia, misalnya:

![alt text](https://github.com/RafiNashirudin/OWASP-TOP-10/blob/main/A02%3A2021-Cryptographic%20Failures/Image/cryptographi.png?raw=true)


## Analisis Kelemahan: SQL Injection
### Penjelasan
Menyimpan password dalam bentuk teks biasa merupakan contoh *Cryptographic Failures*, di mana data sensitif seperti password tidak dilindungi dengan metode kriptografi yang tepat. Kelemahan ini dapat menyebabkan risiko besar bagi keamanan data pengguna.

### Dampak
1. **Kebocoran Data**: Penyerang dapat membaca data sensitif seperti informasi pelanggan, kata sandi, dan data keuangan dari database.
2. **Modifikasi atau Penghapusan Data**: Penyerang dapat mengubah atau menghapus data, yang dapat menyebabkan hilangnya integritas data.
3. **Pengambilalihan Sistem**: Jika server memiliki hak istimewa tinggi, penyerang dapat mengeksploitasi kerentanan SQL Injection untuk menjalankan perintah pada sistem operasi, yang dapat memberikan mereka kontrol penuh atas server.
4. **Kerugian Finansial**: Kebocoran data atau hilangnya integritas data dapat menyebabkan kerugian finansial langsung dan merusak reputasi perusahaan.
5. **Kompliansi Regulasi**: Pelanggaran terhadap data pelanggan dapat melanggar peraturan privasi seperti GDPR, HIPAA, atau PCI-DSS, yang dapat mengakibatkan denda atau sanksi lainnya.

### Rekomendasi
Untuk mencegah kelemahan ini:
- **Gunakan Prepared Statements dan Parameterized Queries**: Prepared statements memastikan bahwa data dan kueri SQL dipisahkan. Dengan parameterized queries, input pengguna diperlakukan sebagai data murni, bukan sebagai kode SQL.
- **Gunakan ORM (Object Relational Mapping)**: ORM memetakan objek dari kode aplikasi ke database, yang menghilangkan kebutuhan untuk menulis perintah SQL mentah.
- **JValidasi dan Filter Input Pengguna**: Terapkan validasi ketat pada semua input pengguna untuk memastikan bahwa hanya data yang diharapkan yang diterima.
- **Gunakan Hak Akses Minimum untuk Database**: Batasi hak akses yang diberikan kepada aplikasi pada database. Dengan menggunakan prinsip least privilege, aplikasi hanya dapat mengakses data yang diperlukan.
- **Hindari Menggunakan Input Langsung dalam Kueri SQL**: Jangan pernah menggabungkan input pengguna langsung dengan kueri SQL.

---

## Output yang Diharapkan dengan Penanganan yang Benar
Jika password disimpan dengan hashing yang benar, hasil dump seharusnya menunjukkan string acak yang terenkripsi, misalnJika penanganan SQL Injection dilakukan dengan benar, serangan menggunakan SQLmap (alat otomatis untuk menemukan dan mengeksploitasi SQL Injection) akan sulit berhasil, karena input berbahaya tidak akan dieksekusi sebagai kueri SQL. Berikut adalah hasil yang diharapkan jika aplikasi sudah terlindungi dengan langkah-langkah pencegahan SQL Injection:

```plaintext
[CRITICAL] unable to find SQL injection vulnerability
```

---

## Referensi
- [SQLMap Documentation](https://sqlmap.org/)

---

> **Catatan Penting**: Pastikan Anda memiliki izin untuk melakukan pengujian ini. Semua pengujian dan eksplorasi dalam dokumen ini hanya untuk tujuan pembelajaran di lingkungan yang diizinkan.
```
