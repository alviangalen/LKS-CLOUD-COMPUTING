# Panduan Persiapan Environment AWS CLI & WSL

Dokumen ini adalah panduan wajib sebelum Anda memulai project Serverless (AWS Lambda, API Gateway, dsb). Pastikan Anda mengikuti langkah-langkah di bawah ini secara berurutan agar proses *deployment* berjalan lancar.

## 1. Persiapan Terminal (Khusus Pengguna Windows)

Agar perintah CLI berjalan mulus dan seragam dengan standar industri (Linux), pengguna Windows **sangat direkomendasikan** menggunakan **WSL (Windows Subsystem for Linux)**.

**Langkah Instalasi WSL:**

1. Buka **PowerShell** (Klik kanan -> *Run as Administrator*).

2. Ketik perintah berikut dan tekan Enter:

   ```bash
   wsl --install
   ```

3. Tunggu proses instalasi selesai, lalu **Restart** komputer/laptop Anda.

4. Setelah *restart*, buka aplikasi **Ubuntu** dari Start Menu dan buat *username* serta *password* baru (password tidak akan terlihat saat diketik, ini normal).

5. Selanjutnya, gunakan terminal Ubuntu ini untuk semua pekerjaan *project* kita!

*(Bagi pengguna macOS atau Linux, silakan langsung gunakan terminal bawaan OS Anda).*

## 2. Instalasi AWS CLI

AWS Command Line Interface (CLI) adalah alat untuk mengelola layanan AWS langsung dari terminal Anda.

Jalankan perintah berikut secara berurutan di dalam terminal Linux/WSL/macOS Anda:

**Unduh file instalasi:**

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

**Ekstrak file:**
*(Jika belum memiliki `unzip`, jalankan `sudo apt install unzip` terlebih dahulu)*

```bash
unzip awscliv2.zip
```

**Jalankan instalasi:**

```bash
sudo ./aws/install
```

**Verifikasi Instalasi:**
Pastikan AWS CLI sudah terinstal dengan mengecek versinya:

```bash
aws --version
```

*(Jika muncul output seperti `aws-cli/2.x.x Python/...`, instalasi berhasil!)*

## 3. Mendapatkan AWS Access Key (Dari AWS Console)

Sebelum menghubungkan terminal dengan AWS, Anda harus membuat "kunci akses" dari akun Anda. Karena Anda memiliki akses manajemen IAM, ikuti langkah berikut:

1. Login ke **AWS Management Console**.
2. Di kolom pencarian atas, ketik dan pilih **IAM (Identity and Access Management)**.
3. Pada panel sebelah kiri, pilih menu **Users**, kemudian klik **nama user Anda**.
4. Pilih tab **Security credentials**.
5. Gulir ke bawah ke bagian **Access keys**, lalu klik tombol **Create access key**.
6. Pilih opsi **Command Line Interface (CLI)**, centang kotak konfirmasi peringatan di bawahnya, lalu klik **Next**.
7. (Opsional) Lewati bagian *tag* dengan langsung klik **Create access key**.
8. **⚠️ SANGAT PENTING:** Di layar ini, Anda akan melihat **Access Key ID** dan **Secret Access Key**. 
   * Segera salin keduanya, ATAU klik tombol **Download .csv file** untuk menyimpannya di komputer.
   * *Secret Access Key hanya ditampilkan SATU KALI pada halaman ini. Jika halaman ditutup, Anda tidak akan bisa melihatnya lagi dan harus membuat kunci baru.*

## 4. Konfigurasi Kredensial AWS (Login)

Setelah mendapatkan Access Key, saatnya menghubungkan terminal Anda ke AWS.

**⚠️ PERHATIAN PENTING - REGION:**
Setiap siswa **TIDAK BOLEH** menggunakan *Region* yang sama secara asal-asalan. Anda telah dialokasikan *Region* masing-masing oleh Instruktur (misal: `ap-southeast-1`, `us-east-1`, `ap-northeast-2`, dsb). **Pastikan Anda menggunakan Region milik Anda sendiri agar resource tidak bertabrakan!**

**Langkah Konfigurasi:**

1. Buka terminal (Ubuntu/WSL/macOS) Anda.
2. Jalankan perintah:

   ```bash
   aws configure
   ```

3. Terminal akan meminta 4 data. Isi sesuai dengan data dari langkah ke-3 dan arahan Instruktur:

   * **AWS Access Key ID:** `PASTE_ACCESS_KEY_ID_ANDA_DI_SINI`
   * **AWS Secret Access Key:** `PASTE_SECRET_ACCESS_KEY_ANDA_DI_SINI`
   * **Default region name:** `ISI_DENGAN_REGION_YANG_DIBAGIKAN_INSTRUKTUR` *(Contoh: ap-southeast-1)*
   * **Default output format:** `json`

**Verifikasi Login:**
Untuk memastikan Anda sudah login dengan benar, jalankan:

```bash
aws sts get-caller-identity
```

Jika muncul output JSON berisi `UserId`, `Account`, dan `Arn`, maka terminal Anda sudah siap digunakan untuk men-deploy aplikasi ke AWS!