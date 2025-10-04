# ECC-NIN-ChatAgent-Nurazlina-Batch_10
----
## Extract PDF Table Workflow (n8n)
----
Workflow ini dibuat untuk mengekstrak tabel dari file PDF yang dikirim melalui Telegram, memproses isinya menggunakan Google Gemini AI, lalu mengonversinya menjadi file CSV dan mengirimkannya kembali ke pengguna lewat Telegram.
Proses ini mempermudah konversi data dari dokumen PDF ke format data terstruktur (CSV) secara otomatis, tanpa perlu coding manual.
----
### Cara Kerja Workflow
1. Pemicu (Trigger)
Telegram Trigger
Workflow akan aktif setiap kali ada pesan masuk di Telegram yang berisi file PDF.
Node ini berfungsi untuk:
Menangkap pesan dan metadata dari Telegram.
Mengambil file PDF yang dikirim pengguna.

2. Pengambilan dan Pengunggahan File
Terdapat beberapa node berturut-turut yang menangani file PDF:
a. Get a File
Mengambil file PDF yang dikirim pengguna dari pesan Telegram.
b. Upload File
Mengunggah file PDF tersebut ke server atau endpoint API (biasanya menggunakan URL upload khusus, misalnya https://api.n8n.ai/upload).
c. Get Signed URL
Node ini meminta signed URL dari server agar file dapat diakses secara aman.
(Signed URL digunakan untuk otentikasi sementara agar file dapat diunduh/diakses AI model.)
d. Get File
Mengambil kembali file PDF dari signed URL tadi agar bisa diproses lebih lanjut.

3. Pemrosesan Awal
Split Out
Memecah data file agar bisa diolah per bagian (misalnya jika PDF terdiri dari beberapa halaman atau tabel).
Node ini memastikan bahwa data yang dikirim ke AI lebih mudah diproses.
Edit Fields
Digunakan untuk menyiapkan field atau variabel input yang akan diberikan ke model AI — misalnya menentukan nama kolom, format, atau instruksi manual untuk ekstraksi tabel.

4. Ekstraksi Informasi Menggunakan AI
Information Extractor + Google Gemini Chat Model
Information Extractor:
Node ini berfungsi untuk mengekstrak informasi penting dari isi PDF menggunakan bantuan AI.
Ia mengambil teks hasil parsing dari file PDF, lalu mengirimnya ke Google Gemini Chat Model.
Google Gemini Chat Model:
Model AI ini bertugas membaca konten PDF (terutama tabel atau teks terstruktur), lalu:
Mengidentifikasi bagian tabel.
Mengubah hasilnya menjadi format JSON atau teks siap konversi.
Membersihkan data agar mudah diubah ke CSV.

4. Ekstraksi Informasi Menggunakan AI
Information Extractor + Google Gemini Chat Model
Information Extractor:
Node ini berfungsi untuk mengekstrak informasi penting dari isi PDF menggunakan bantuan AI.
Ia mengambil teks hasil parsing dari file PDF, lalu mengirimnya ke Google Gemini Chat Model.
Google Gemini Chat Model:
Model AI ini bertugas membaca konten PDF (terutama tabel atau teks terstruktur), lalu:
Mengidentifikasi bagian tabel.
Mengubah hasilnya menjadi format JSON atau teks siap konversi.
Membersihkan data agar mudah diubah ke CSV.

6. Output ke Telegram
Send a Document (Telegram)
Mengirimkan kembali hasil konversi (file CSV) ke pengguna di Telegram.
Pengguna akan menerima file dengan pesan otomatis seperti pada gambar berikut:

----
### 📌 Contoh Alur Kerja Singkat
Kamu kirim file PDF ke bot Telegram.
Bot otomatis menjalankan workflow.
AI membaca dan mengekstrak tabel di PDF.
File hasil konversi dikirim balik ke Telegram dalam format .csv.
----
### Catatan
Workflow ini berjalan secara event-based — artinya hanya aktif saat ada pesan baru ke bot.
File PDF sebaiknya berisi tabel teks (bukan gambar) agar hasil ekstraksi lebih akurat.
Model Google Gemini AI digunakan untuk menafsirkan struktur tabel yang kompleks.
Kamu bisa memperluas workflow ini dengan:
Menambahkan validasi data.
Menggabungkan hasil ke Google Sheets.
Menyimpan log ke database (misalnya PostgreSQL atau Airtable).

