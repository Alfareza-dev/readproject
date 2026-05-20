# 📖 Panduan Alur Simpel — UKSTM-32 Digital Invitation

> Panduan ini menjelaskan alur kerja aplikasi secara sederhana.

---

## 🏗️ Gambaran Besar Sistem

Sistem ini terdiri dari **4 bagian utama** yang saling bekerja sama:

```
Pengguna (Browser) → Backend (Otak Utama) → Database (Penyimpanan Data)
                                           → Worker (Pengirim Pesan WhatsApp)
```

```mermaid
graph LR
    A["👤 Pengguna"] --> B["🖥️ Backend"]
    B --> C["🗄️ Database"]
    B --> D["📲 Worker WhatsApp"]
    D --> E["💬 WhatsApp"]

    style A fill:#4ECDC4,color:#fff
    style B fill:#E8734A,color:#fff
    style C fill:#336791,color:#fff
    style D fill:#F5A623,color:#fff
    style E fill:#25D366,color:#fff
```

---

## 👤 1. Pendaftaran & Masuk

### Daftar Akun Baru

```
Isi Form (nama, email, HP, password)
  → Sistem cek email sudah dipakai atau belum
    → Kalau sudah → ❌ Ditolak "Email sudah ada"
    → Kalau belum → ✅ Akun dibuat → Dapat token akses → Masuk ke aplikasi
```

```mermaid
graph TD
    A["📝 Isi form pendaftaran"] --> B{"Email sudah dipakai?"}
    B -->|Sudah| C["❌ Ditolak"]
    B -->|Belum| D["✅ Akun berhasil dibuat"]
    D --> E["🔑 Dapat token akses"]
    E --> F["🏠 Masuk ke halaman utama"]

    style C fill:#e74c3c,color:#fff
    style D fill:#2ecc71,color:#fff
```

### Login

```
Isi email & password
  → Sistem cocokkan data
    → Kalau salah → ❌ "Kredensial tidak valid"
    → Kalau benar → Cek akun aktif
      → Tidak aktif → ❌ "Akun nonaktif"
      → Aktif → ✅ Login berhasil → Dapat token
        → Super Admin → Halaman kelola pengguna & bot
        → User biasa → Halaman daftar acara
        → Event Admin → Langsung ke halaman operasional acara
```

```mermaid
graph TD
    A["🔐 Isi email & password"] --> B{"Data cocok?"}
    B -->|Salah| C["❌ Login gagal"]
    B -->|Benar| D{"Akun aktif?"}
    D -->|Tidak| E["❌ Akun nonaktif"]
    D -->|Ya| F["✅ Login berhasil"]
    F --> G{"Peran pengguna?"}
    G -->|Super Admin| H["👑 Dashboard Admin"]
    G -->|User| I["📅 Daftar Acara"]
    G -->|Event Admin| J["🎫 Halaman Operasional"]

    style C fill:#e74c3c,color:#fff
    style E fill:#e74c3c,color:#fff
    style F fill:#2ecc71,color:#fff
```

### Ganti Password

```
Isi password lama + password baru
  → Sistem cek password lama cocok
    → Tidak cocok → ❌ "Password lama salah"
    → Cocok → ✅ Password diperbarui
```

```mermaid
graph LR
    A["🔑 Isi password lama & baru"] --> B{"Password lama cocok?"}
    B -->|Tidak| C["❌ Ditolak"]
    B -->|Ya| D["✅ Password diubah"]

    style C fill:#e74c3c,color:#fff
    style D fill:#2ecc71,color:#fff
```

---

## 🎭 2. Tiga Peran Pengguna

```
👑 Super Admin  → Kelola semua pengguna + Bot WhatsApp
🧑 User         → Buat acara (maks 10) + Kelola tamu + Kirim undangan
🎫 Event Admin  → Check-in tamu + Bagi snack (di lokasi acara)
```

```mermaid
graph TD
    SA["👑 Super Admin"] --> SA1["Kelola pengguna"]
    SA --> SA2["Kelola bot WhatsApp"]
    SA --> SA3["Assign Event Admin"]

    US["🧑 User / Pemilik Acara"] --> US1["Buat acara (maks 10)"]
    US --> US2["Tambah & impor tamu"]
    US --> US3["Kirim undangan WhatsApp"]
    US --> US4["Lihat laporan & statistik"]

    EA["🎫 Event Admin"] --> EA1["Check-in tamu (QR/Manual)"]
    EA --> EA2["Bagi snack (QR/Manual)"]
    EA --> EA3["Lihat statistik acara"]

    style SA fill:#e74c3c,color:#fff
    style US fill:#3498db,color:#fff
    style EA fill:#f39c12,color:#fff
```

---

## 📅 3. Buat & Kelola Acara

### Buat Acara Baru

```
Klik "Buat Acara" → Isi judul, tanggal, lokasi, deskripsi
  → Sistem cek jumlah acara user
    → Sudah 10 acara → ❌ "Maksimal 10 acara"
    → Belum 10 → ✅ Acara berhasil dibuat
```

```mermaid
graph TD
    A["📅 Buat acara baru"] --> B{"Sudah punya 10 acara?"}
    B -->|Ya| C["❌ Batas tercapai"]
    B -->|Belum| D["✅ Acara dibuat"]

    style C fill:#e74c3c,color:#fff
    style D fill:#2ecc71,color:#fff
```

### Kelola Acara

```
Lihat daftar acara → Pilih acara → Edit / Hapus
  → Hapus = Soft Delete (data masih tersimpan, masuk ke "Sampah")
```

### Daftarkan Event Admin

```
Pemilik acara isi data admin (nama, email, HP, password)
  → Sistem cek email belum dipakai
    → Sudah ada → ❌ "Email sudah terdaftar"
    → Belum → ✅ Akun Event Admin dibuat & ditautkan ke acara
```

---

## 📋 4. Jadwal Acara (Rundown)

### Tambah Rundown

```
Isi judul, deskripsi, waktu mulai, waktu selesai
  → Sistem otomatis tentukan nomor urut (urutan terakhir + 1)
  → ✅ Rundown ditambahkan
```

> **Penting:** Nomor urut ditentukan otomatis oleh sistem, bukan oleh pengguna. Ini mencegah nomor urut ganda.

### Ubah Urutan Rundown

```
Pindahkan rundown ke posisi baru
  → Kalau posisi sudah diisi → Geser yang lain ke bawah
  → ✅ Urutan diperbarui
```

### Hapus Rundown

```
Hapus rundown
  → Sistem otomatis rapatkan urutan (tidak ada celah)
  → Contoh: hapus no.2 → no.3 jadi no.2, no.4 jadi no.3, dst.
```

```mermaid
graph TD
    subgraph "Tambah"
        A1["Isi data rundown"] --> A2["Urutan otomatis = MAX + 1"]
        A2 --> A3["✅ Ditambahkan"]
    end

    subgraph "Ubah Urutan"
        B1["Pindah ke posisi baru"] --> B2{"Posisi sudah terisi?"}
        B2 -->|Ya| B3["Geser yang lain"]
        B2 -->|Tidak| B4["Langsung pindah"]
        B3 --> B5["✅ Urutan diperbarui"]
        B4 --> B5
    end

    subgraph "Hapus"
        C1["Hapus rundown"] --> C2["Rapatkan urutan otomatis"]
        C2 --> C3["✅ Selesai"]
    end

    style A3 fill:#2ecc71,color:#fff
    style B5 fill:#2ecc71,color:#fff
    style C3 fill:#2ecc71,color:#fff
```

---

## 👥 5. Kelola Tamu

### Perjalanan Status Tamu

Ini adalah siklus hidup status setiap tamu dalam sistem:

```
NEW (baru didaftarkan)
  → INVITED (pesan WA terkirim)
    → OPENED (buka link undangan)
      → GOING (RSVP: hadir) ← bisa bolak-balik maks 3x → NOT_GOING (RSVP: tidak hadir)
```

```mermaid
graph LR
    A["🆕 NEW"] -->|Pesan WA terkirim| B["📨 INVITED"]
    B -->|Buka link undangan| C["👀 OPENED"]
    C -->|RSVP: hadir| D["✅ GOING"]
    C -->|RSVP: tidak hadir| E["❌ NOT_GOING"]
    D <-->|Ubah RSVP maks 3x| E

    style A fill:#95a5a6,color:#fff
    style B fill:#3498db,color:#fff
    style C fill:#f39c12,color:#fff
    style D fill:#2ecc71,color:#fff
    style E fill:#e74c3c,color:#fff
```

### Tambah Tamu Satu per Satu

```
Isi nama + nomor HP
  → Sistem cek ke WhatsApp: "Nomor ini aktif?"
    → Tidak aktif → ❌ Ditolak "Nomor tidak terdaftar di WA"
    → Aktif → Cek nomor sudah dipakai di acara ini?
      → Sudah → ❌ "Nomor sudah terdaftar"
      → Belum → ✅ Tamu ditambahkan (status: NEW)
                   + Dapat link undangan unik
```

```mermaid
graph TD
    A["👤 Isi data tamu"] --> B["📱 Cek nomor di WhatsApp"]
    B --> C{"Nomor aktif di WA?"}
    C -->|Tidak| D["❌ Ditolak"]
    C -->|Ya| E{"Sudah terdaftar di acara ini?"}
    E -->|Ya| F["❌ Duplikat"]
    E -->|Tidak| G["✅ Tamu ditambahkan"]
    G --> H["🔗 Dapat link undangan unik"]

    style D fill:#e74c3c,color:#fff
    style F fill:#e74c3c,color:#fff
    style G fill:#2ecc71,color:#fff
```

### Impor Tamu dari Excel

Format kolom Excel: **Nama | No. Handphone | Grup**

```
Upload file Excel (.xlsx)
  → Sistem baca baris per baris
    → Baris tanpa nama/HP → ❌ Masuk daftar error
    → Nomor tidak aktif di WA → ❌ Masuk daftar error
    → Nomor duplikat di file → ❌ Masuk daftar error
    → Nomor sudah ada di database → ❌ Masuk daftar error
    → Semua lolos → ✅ Tamu disimpan ke database
  → Hasil: "45 berhasil, 5 gagal" + daftar error per baris
```

```mermaid
graph TD
    A["📤 Upload Excel"] --> B["🔍 Baca baris per baris"]
    B --> C{"Nama & HP lengkap?"}
    C -->|Tidak| D["❌ Error: data tidak lengkap"]
    C -->|Ya| E{"Nomor aktif di WA?"}
    E -->|Tidak| F["❌ Error: nomor tidak aktif"]
    E -->|Ya| G{"Duplikat di file?"}
    G -->|Ya| H["❌ Error: duplikat"]
    G -->|Tidak| I{"Sudah ada di DB?"}
    I -->|Ya| J["❌ Error: sudah terdaftar"]
    I -->|Tidak| K["✅ Tamu disimpan"]

    D --> L["📊 Laporan Akhir"]
    F --> L
    H --> L
    J --> L
    K --> L

    style D fill:#e74c3c,color:#fff
    style F fill:#e74c3c,color:#fff
    style H fill:#e74c3c,color:#fff
    style J fill:#e74c3c,color:#fff
    style K fill:#2ecc71,color:#fff
    style L fill:#3498db,color:#fff
```

### Edit Tamu — Ganti Nomor HP

```
Edit data tamu → Ganti nomor HP
  → Kalau nomor baru berbeda dari yang lama
    → Status tamu otomatis kembali ke NEW
    → (Karena nomor baru belum pernah dapat undangan)
```

```mermaid
graph LR
    A["✏️ Ganti nomor HP"] --> B{"Nomor berubah?"}
    B -->|Tidak| C["Simpan biasa"]
    B -->|Ya| D["Status reset → NEW"]
    D --> E["Simpan"]

    style D fill:#f39c12,color:#fff
```

---

## 📢 6. Kirim Undangan WhatsApp (Broadcast)

### Alur Lengkap Pengiriman

```
1. User tulis template pesan (pakai variabel {{ name }}, {{ invitation_link }}, dll.)
2. User lihat preview → Sistem tampilkan contoh pesan dengan data tamu asli
3. User klik "Kirim" → Sistem cek:
   → Bot WA nyala? → Tidak → ❌ "Bot belum siap"
   → Ada siaran yang masih jalan? → Ya → ❌ "Tunggu siaran sebelumnya selesai"
   → Ada tamu yang belum diundang? → Tidak → ❌ "Semua tamu sudah diundang"
   → Semua lolos → ✅ Pesan masuk antrean
4. Worker kirim pesan satu per satu (jeda 5-15 detik per pesan)
5. Setiap pesan terkirim → Status tamu berubah: NEW → INVITED
6. Semua selesai → Batch siaran ditandai "COMPLETED"
```

```mermaid
graph TD
    A["✍️ Tulis template pesan"] --> B["👁️ Preview pesan"]
    B --> C["📤 Klik Kirim"]
    C --> D{"Bot WA nyala?"}
    D -->|Tidak| E["❌ Bot belum siap"]
    D -->|Ya| F{"Ada siaran<br/>masih jalan?"}
    F -->|Ya| G["❌ Tunggu selesai dulu"]
    F -->|Tidak| H{"Ada tamu<br/>belum diundang?"}
    H -->|Tidak| I["❌ Semua sudah diundang"]
    H -->|Ya| J["✅ Pesan masuk antrean"]
    J --> K["⏳ Worker kirim<br/>satu per satu"]
    K --> L["📨 Status tamu:<br/>NEW → INVITED"]
    L --> M["✅ Siaran selesai"]

    style E fill:#e74c3c,color:#fff
    style G fill:#e74c3c,color:#fff
    style I fill:#e74c3c,color:#fff
    style M fill:#2ecc71,color:#fff
```

### Variabel Template

```
{{ name }}            → Nama tamu (contoh: "Budi Santoso")
{{ event_title }}     → Judul acara (contoh: "Wisuda SMK Telkom 2026")
{{ event_date }}      → Tanggal acara (contoh: "Selasa, 15 Juli 2026")
{{ invitation_link }} → Link undangan unik tamu
```

### Status Siaran (Batch)

```
PENDING → PROCESSING → COMPLETED
                     → PAUSED (kalau bot mati)
                     → FAILED (gagal total)
```

```mermaid
graph LR
    A["⏳ PENDING"] --> B["🔄 PROCESSING"]
    B --> C["✅ COMPLETED"]
    B --> D["⏸️ PAUSED"]
    B --> E["❌ FAILED"]

    style A fill:#95a5a6,color:#fff
    style B fill:#f39c12,color:#fff
    style C fill:#2ecc71,color:#fff
    style D fill:#3498db,color:#fff
    style E fill:#e74c3c,color:#fff
```

### Follow-up (Tindak Lanjut)

```
Siapa yang dapat follow-up?
  → Tamu berstatus INVITED atau OPENED
  → TIDAK termasuk: tamu yang sudah GOING / NOT_GOING

Alur:
  Lihat daftar penerima follow-up → Tulis pesan baru → Kirim
  → Sama seperti broadcast biasa (cek bot, cek spam lock, dll.)
```

```mermaid
graph TD
    A["📬 Kirim Follow-up"] --> B["Filter tamu"]
    B --> C["✅ INVITED yang belum buka"]
    B --> D["✅ OPENED yang belum RSVP"]
    B --> E["❌ GOING — dikecualikan"]
    B --> F["❌ NOT_GOING — dikecualikan"]
    C --> G["📤 Kirim pesan follow-up"]
    D --> G

    style C fill:#3498db,color:#fff
    style D fill:#f39c12,color:#fff
    style E fill:#95a5a6,color:#fff
    style F fill:#95a5a6,color:#fff
```

---

## 💌 7. Tamu Buka Undangan & RSVP

### Buka Link Undangan

```
Tamu klik link undangan → Halaman undangan terbuka
  → Tampilkan: nama tamu, detail acara, jadwal rundown
  → Kalau status masih NEW/INVITED → Otomatis berubah ke OPENED
  → Waktu dibuka (openedAt) dicatat
```

```mermaid
graph TD
    A["🔗 Tamu klik link undangan"] --> B["📄 Halaman undangan terbuka"]
    B --> C{"Status tamu?"}
    C -->|NEW / INVITED| D["Status → OPENED<br/>Catat waktu dibuka"]
    C -->|Sudah OPENED/lainnya| E["Tetap status sekarang"]
    D --> F["🎉 Tampilkan undangan<br/>+ jadwal acara<br/>+ form RSVP"]
    E --> F

    style D fill:#f39c12,color:#fff
    style F fill:#2ecc71,color:#fff
```

### RSVP (Konfirmasi Kehadiran)

```
Tamu isi form RSVP (hadir/tidak + pesan opsional)
  → Cek: sudah ubah RSVP berapa kali?
    → Sudah 3 kali → ❌ "Batas perubahan tercapai"
    → Belum 3 kali → ✅ RSVP disimpan
      → Hadir → Status → GOING + Dapat QR Code
      → Tidak hadir → Status → NOT_GOING
```

```mermaid
graph TD
    A["📝 Tamu isi RSVP"] --> B{"Sudah ubah 3 kali?"}
    B -->|Ya| C["❌ Batas tercapai"]
    B -->|Belum| D{"Hadir?"}
    D -->|Ya| E["✅ Status → GOING"]
    D -->|Tidak| F["Status → NOT_GOING"]
    E --> G["🎫 Dapat QR Code"]
    G --> H["QR untuk check-in<br/>di lokasi acara"]

    style C fill:#e74c3c,color:#fff
    style E fill:#2ecc71,color:#fff
    style F fill:#e74c3c,color:#fff
    style G fill:#9b59b6,color:#fff
```

---

## 🎫 8. Hari-H: Operasional di Lokasi Acara

### Check-in Tamu

```
Tamu tiba di lokasi → Tunjukkan QR Code ke Event Admin
  → Admin scan QR (atau cari manual)
    → Tamu valid & belum check-in → ✅ Check-in berhasil
    → Tamu sudah check-in → ❌ "Sudah check-in sebelumnya"
    → Tamu tidak dikenal → ❌ "Data tidak valid"
```

```mermaid
graph TD
    A["🚶 Tamu tiba"] --> B["📱 Tunjukkan QR Code"]
    B --> C{"Scan QR / Cari Manual"}
    C --> D{"Tamu valid?"}
    D -->|Tidak| E["❌ Data tidak valid"]
    D -->|Ya| F{"Sudah check-in?"}
    F -->|Ya| G["❌ Sudah check-in"]
    F -->|Belum| H["✅ Check-in berhasil!"]

    style E fill:#e74c3c,color:#fff
    style G fill:#f39c12,color:#fff
    style H fill:#2ecc71,color:#fff
```

### Klaim Snack (Makanan Ringan)

```
Tamu mau ambil snack → Admin scan QR
  → Cek: sudah check-in belum?
    → BELUM check-in → ❌ "Check-in dulu baru boleh ambil snack!"
    → SUDAH check-in → Cek: sudah ambil snack?
      → Sudah → ❌ "Sudah ambil sebelumnya"
      → Belum → ✅ Snack diberikan
```

> ⚠️ **Aturan Ketat:** Tamu WAJIB check-in dulu sebelum boleh ambil snack. Tidak ada pengecualian.

```mermaid
graph TD
    A["🍪 Tamu mau ambil snack"] --> B["📱 Admin scan QR"]
    B --> C{"Sudah check-in?"}
    C -->|Belum| D["❌ DITOLAK!<br/>Check-in dulu!"]
    C -->|Sudah| E{"Sudah ambil snack?"}
    E -->|Ya| F["❌ Sudah ambil"]
    E -->|Belum| G["✅ Snack diberikan"]

    style D fill:#e74c3c,color:#fff
    style F fill:#f39c12,color:#fff
    style G fill:#2ecc71,color:#fff
```

### Alur Lengkap di Lokasi

```mermaid
graph LR
    A["🚶 Tamu tiba"] --> B["✅ Check-in"]
    B --> C["🍪 Ambil Snack"]
    C --> D["🎁 Ambil Suvenir"]

    style A fill:#3498db,color:#fff
    style B fill:#2ecc71,color:#fff
    style C fill:#f39c12,color:#fff
    style D fill:#9b59b6,color:#fff
```

---

## 📊 9. Statistik & Laporan

### Dashboard Statistik

```
Buka halaman statistik → Sistem tampilkan:
  → Total tamu terdaftar
  → Total yang RSVP hadir (GOING)
  → Total yang sudah check-in
  → Total yang sudah ambil snack/suvenir
```

```mermaid
graph TD
    A["📊 Buka Dashboard"] --> B["Total Tamu: 150"]
    A --> C["RSVP Hadir: 98"]
    A --> D["Sudah Check-in: 75"]
    A --> E["Sudah Ambil Snack: 60"]

    style B fill:#3498db,color:#fff
    style C fill:#2ecc71,color:#fff
    style D fill:#f39c12,color:#fff
    style E fill:#9b59b6,color:#fff
```

### Unduh Laporan Excel

```
Klik "Export" → Sistem buat file Excel dengan 2 sheet:
  → Sheet 1: Data Tamu (semua detail per tamu)
  → Sheet 2: Ringkasan (total per status, tingkat kehadiran, dll.)
  → Browser otomatis download file .xlsx
```

---

## 🤖 10. Bot WhatsApp (Super Admin)

### Status Bot

```
DISCONNECTED (mati)
  → CONNECTING (sedang nyambung)
    → QR_REQUIRED (scan QR dulu)
      → CONNECTED (nyala & siap kirim pesan)
        → DISCONNECTED (kalau mati / di-disconnect admin)
```

```mermaid
graph LR
    A["🔴 MATI"] --> B["🟡 NYAMBUNG..."]
    B --> C["📷 SCAN QR"]
    C --> D["🟢 NYALA"]
    D --> A

    style A fill:#e74c3c,color:#fff
    style B fill:#f39c12,color:#fff
    style C fill:#9b59b6,color:#fff
    style D fill:#2ecc71,color:#fff
```

### Watchdog (Penjaga Otomatis)

```
Setiap 1 menit, sistem cek:
  → Bot nyala & heartbeat terakhir < 5 menit → ✅ Sehat
  → Bot nyala tapi heartbeat > 5 menit → ⚠️ Bermasalah!
    → Otomatis reset → Worker akan nyambung ulang sendiri
```

---

## 🔄 11. Alur Lengkap Dari Awal Sampai Akhir

Ini adalah perjalanan lengkap dari pendaftaran sampai acara selesai:

```mermaid
graph TD
    A["1️⃣ User daftar akun"] --> B["2️⃣ Login"]
    B --> C["3️⃣ Buat acara baru"]
    C --> D["4️⃣ Isi jadwal rundown"]
    D --> E["5️⃣ Tambah/impor tamu"]
    E --> F["6️⃣ Kirim undangan WA"]
    F --> G["7️⃣ Tamu buka undangan"]
    G --> H["8️⃣ Tamu isi RSVP"]
    H --> I{"Hadir?"}
    I -->|Ya| J["9️⃣ Dapat QR Code"]
    I -->|Tidak| K["Tercatat tidak hadir"]
    J --> L["🔟 Hari H: Check-in pakai QR"]
    L --> M["1️⃣1️⃣ Ambil snack"]
    M --> N["1️⃣2️⃣ Ambil suvenir"]
    N --> O["📊 Lihat statistik & unduh laporan"]

    style A fill:#3498db,color:#fff
    style F fill:#9b59b6,color:#fff
    style J fill:#f39c12,color:#fff
    style L fill:#2ecc71,color:#fff
    style O fill:#e67e22,color:#fff
```

### Versi Teks Sederhana

```
📝 Daftar Akun
  → 🔐 Login
    → 📅 Buat Acara
      → 📋 Atur Jadwal (Rundown)
        → 👥 Tambah Tamu (manual / Excel)
          → 📱 Validasi nomor HP di WhatsApp
            → 📢 Kirim Undangan via WhatsApp
              → 💌 Tamu buka undangan (status: OPENED)
                → 📝 Tamu isi RSVP (hadir/tidak, maks ubah 3x)
                  → 🎫 Kalau hadir: dapat QR Code
                    → ✅ Hari H: Check-in pakai QR
                      → 🍪 Ambil snack (harus sudah check-in!)
                        → 🎁 Ambil suvenir
                          → 📊 Unduh laporan Excel
```

---

## 📌 Catatan Penting untuk Tim Frontend

| No | Hal Penting | Penjelasan |
|----|-------------|------------|
| 1 | **Nomor HP otomatis diformat** | `081xxx` → `+6281xxx` (dilakukan di backend) |
| 2 | **Status tamu berubah otomatis** | NEW → INVITED (saat WA terkirim), INVITED → OPENED (saat buka link) |
| 3 | **RSVP dibatasi 3x perubahan** | Tampilkan peringatan saat sisa perubahan tinggal sedikit |
| 4 | **Snack butuh check-in dulu** | Tampilkan pesan error yang jelas jika belum check-in |
| 5 | **Spam Lock pada broadcast** | Kalau ada siaran berjalan, tombol "Kirim" harus di-disable |
| 6 | **Export Excel = download file** | Bukan JSON biasa, gunakan `window.open()` atau blob download |
| 7 | **Event Admin punya eventId** | Saat login, simpan `eventId` dari response untuk navigasi otomatis |
| 8 | **QR Code dibuat di frontend** | Backend kirim data `{ hash }`, frontend generate QR dari data tersebut |
| 9 | **Maksimal 10 acara per user** | Tampilkan peringatan saat mendekati batas |
| 10 | **Soft delete dimana-mana** | Data tidak benar-benar dihapus, ada fitur "Sampah / Trash" |

---

## 📄 License

UNLICENSED — Internal project for SMK Telkom Malang.
