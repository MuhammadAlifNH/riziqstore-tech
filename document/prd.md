# PRODUCT REQUIREMENT DOCUMENT (PRD)

| Atribut | Keterangan |
| :--- | :--- |
| **Nama Proyek** | riziqstore.idn (Web-Based Game Top Up MVP) |
| **Versi Dokumen** | **1.2 (Complete Enhanced MVP)** |
| **Status** | *Final Release* |
| **Tanggal** | 23 Desember 2025 |
| **Pemilik Produk** | Tim Pengembang riziqstore.idn |
| **Target Program** | P2MW 2025 (Program Pembinaan Mahasiswa Wirausaha) |

---

## 1. Pendahuluan (*Introduction*)

### 1.1 Ringkasan Eksekutif
**riziqstore.idn** adalah platform layanan *top-up game* berbasis web yang mengadopsi model **Hybrid Tech**. Dalam fase *Enhanced MVP* ini, sistem mendukung **Dual-Method Input** (Top Up via UID & Top Up via Login) serta dilengkapi fitur **"Smart Subscription Reminder"** untuk retensi pelanggan. Transaksi diselesaikan melalui integrasi *WhatsApp API Gateway* yang aman dan personal.

### 1.2 Tujuan Bisnis
1.  **Market Expansion:** Mengakomodasi segmen pasar yang membutuhkan layanan Top Up Login (Paket Bundle & Gift) yang seringkali tidak terlayani oleh kompetitor mainstream karena keterbatasan sistem otomatis.
2.  **Operational Trust:** Membangun kepercayaan melalui fitur *Real-Time Store Status* (Indikator Buka/Tutup).
3.  **Retention:** Meningkatkan pembelian ulang melalui notifikasi pengingat otomatis.

---

## 2. Masalah & Solusi (*Problem & Solution*)

### 2.1 Latar Belakang Masalah (*Pain Points*)
* **Keterbatasan Platform:** Banyak web top up hanya melayani UID, padahal mahasiswa sering membutuhkan akses login untuk membeli paket bundle khusus.
* **Uncertainty:** Pengguna sering kecewa order di jam malam tapi tidak diproses karena toko tutup tanpa pemberitahuan.
* **Subscription Amnesia:** Lupa perpanjang paket mingguan/bulanan yang menyebabkan hilangnya bonus *streak*.

### 2.2 Solusi Produk
* **Dual-Method Interface:** Satu halaman web untuk dua kebutuhan: Form UID (Top Up Kilat) dan Top Up Login (Form Login) yang aman.
* **Smart Store Status:** Banner otomatis yang mendeteksi jam (10.00-22.00) untuk memberi tahu status toko BUKA/TUTUP.
* **Interactive Feedback:** Notifikasi *Pop-up* visual saat pesanan berhasil dibuat sebelum dialihkan ke WhatsApp.

---

## 3. Profil Pengguna (*User Persona*)

| Atribut | Detail |
| :--- | :--- |
| **Nama** | **Rizky (21 Tahun)** |
| **Peran** | Mahasiswa / *Mid-core Gamer* |
| **Perilaku** | Sering berganti-ganti antara beli diamond receh dan membeli paket limited via login |
| **Goals** | Butuh platform yang "Palugada" (Apa Lu Mau Gua Ada): Bisa top up, bisa Top Up Login, respon cepat, dan aman. |

---

## 4. Spesifikasi Fitur (*Functional Requirements*)

### 4.1 Modul Katalog & Operasional
* **FR-01 (Store Logic):** Sistem mendeteksi waktu lokal pengguna. Tampilkan Banner Hijau (BUKA) pada jam 10.00-22.00, dan Merah (TUTUP) di luar jam tersebut.
* **FR-02:** Menampilkan katalog game populer (MLBB, FF, Genshin).

### 4.2 Modul Input Hibrida
* **FR-03 (Switch Method):** Tab opsi untuk memilih metode "Via UID" atau "Via Login".
* **FR-04 (Mode UID):** Input validasi hanya angka untuk User ID & Zone ID.
* **FR-05 (Mode Login):** Input Email, Password, dan Pilihan Login (Moonton/VK/FB/TikTok).

### 4.3 Modul Smart Reminder
* **FR-06:** Checkbox "Aktifkan Reminder (Gratis)".
* **FR-07:** Logic: Jika dicentang = Kolom WA Wajib Isi. Jika tidak = Kolom WA Hilang.

### 4.4 Modul Transaksi
* **FR-08 (Generator):** Membuat link WhatsApp dengan format pesan dinamis sesuai metode input (UID/Login).
* **FR-09 (Alert):** Menampilkan *SweetAlert* (Pop-up) sukses/gagal sebelum redirect.

---

## 5. Kebutuhan Non-Fungsional (*Non-Functional Requirements*)

* **NFR-01 Security (CRITICAL):** Data sensitif (Password Akun pada metode Login) **DILARANG** disimpan di database/cookie. Data harus diteruskan secara *live* ke WhatsApp Admin (End-to-End Encrypted) dan segera dihapus dari chat history setelah proses selesai.
* **NFR-02 Performance:** Load time < 1.5 detik (Static HTML).
* **NFR-03 UX:** Tema *Cyberpunk/Dark Mode* sesuai preferensi *gamers*.

---

## 6. Peta Perjalanan Pengguna (*User Journey Map*)

### Skenario: Order Jasa Top Up Login (Via Login)

1.  **Akses:** User buka web jam 14.00 (Banner: BUKA).
2.  **Input:** Pilih MLBB -> Klik Tab "Via Login" -> Isi Email/Pass -> Pilih Item "Top Up Login".
3.  **Action:** Klik "Bayar Sekarang".
4.  **Feedback:** Muncul Pop-up "Pesanan Dibuat!".
5.  **Finalisasi:** Masuk ke WhatsApp dengan pesan: *"Halo Admin, Order Top Up Login. Akun: [Email]. Item: [Item]. Nominal: [Nominal]."*

---

## 7. Arsitektur Data (*Data Architecture*) - MVP Phase

Karena sistem bersifat *Stateless* (Web Statis), pencatatan data dilakukan secara manual oleh Admin di Google Sheets setelah chat masuk.

| Entitas Data | Atribut yang Dicatat Admin | Keterangan Privasi |
| :--- | :--- | :--- |
| **Transaksi UID** | `Tgl`, `No_WA`, `Game_ID`, `Item`, `Nominal` | Aman disimpan untuk rekap. |
| **Transaksi Login** | `Tgl`, `No_WA`, `Item`, `Nominal` | **Email & Password TIDAK DICATAT** di database manapun demi keamanan. |
| **Reminder** | `No_WA`, `Tgl_Expired_Paket`, `Status_Notif` | Disimpan untuk jadwal pengingat. |

---

## 8. Metrik Keberhasilan (*Success Metrics*)

* **Conversion Rate:** > 5% pengunjung web berlanjut ke WhatsApp.
* **Method Split:** Memantau rasio penggunaan fitur (Target: 70% Via UID, 30% Via Login).
* **Reminder Adoption:** > 20% pembeli paket mingguan mengaktifkan fitur Reminder.
* **Error Rate:** 0% keluhan data login bocor (Berkat SOP penghapusan chat).

---

## 9. Roadmap Pengembangan (*Future Development*)

* **Bulan 1-2 (Launch):** Rilis Web v1.0 (Dual Method + Store Status). Validasi SOP keamanan data Login.
* **Bulan 3-4 (Growth):** Integrasi API Cek Nickname Game (agar nama player muncul otomatis).
* **Bulan 5-6 (Scale):** Jika transaksi login > 50/hari, pertimbangkan sistem enkripsi otomatis untuk data akun.