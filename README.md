# PDA Flowchart Simulator (Pushdown Automata)

### 👥 Kelompok B03
*   **Anggota 1**: [Agil Lukman Hakim Muchdi] - [5025241037]
*   **Anggota 2**: [Berwyn Rafif Alvaro] - [5025241029]
*   **Anggota 3**: [Mahendra Agung Darmawan] - [5025241032]

---

Aplikasi web *Single Page Application* (SPA) interaktif untuk mensimulasikan mesin **Pushdown Automata (PDA)** menggunakan model flowchart grafis (**READ / PUSH / POP**). Proyek ini dirancang secara khusus untuk memenuhi tugas **Praktikum 3 Otomata & Teori Bahasa**.
Link : [https://darkdrago89.github.io/Praktikum3Otomata/](https://darkdrago89.github.io/Praktikum3Otomata/)

---

## 🌟 Fitur Utama

1. **Simulator PDA Interaktif**:
   - **Visualisasi Pita Input (Tape)**: Melacak penunjuk pembacaan secara visual (karakter aktif membesar dan menyala keemasan, sedangkan karakter yang telah diproses menciut dan dicoret).
   - **Visualisasi Stack 3D-like (Cylinder Jar)**: Menampilkan isi memori stack di dalam visual wadah kaca tabung silinder dengan neon glow biru pada simbol paling atas (top of stack).
   - **Highlighting Transisi Aktif**: Jalur panah/edge transisi yang sedang dieksekusi akan menyala keemasan secara real-time pada canvas flowchart saat dijalankan langkah demi langkah (*step-by-step*).
   
2. **Interactive Flowchart Builder**:
   - Mendesain mesin PDA sendiri secara langsung pada aplikasi.
   - Mendukung penambahan Node (**START, READ, PUSH, POP, ACCEPT, REJECT**) dan penarikan relasi Edge (Panah Transisi).
   - Dilengkapi fitur **Auto Layout** untuk merapikan tata letak node secara otomatis berbasis algoritma level hierarki.
   - Konfigurasi simbol pada node PUSH dan POP secara langsung dari formulir builder.

3. **Batch Testing (Uji Massal)**:
   - Menguji keanggotaan banyak string sekaligus (satu string per baris).
   - Menampilkan status keberhasilan (**ACCEPTED** atau **REJECTED**) serta statistik persentase string yang diterima.

---

## 🛠️ Panduan Penggunaan Simulator

1. **Memilih Mesin (Template)**:
   - Di tab **Simulator**, klik menu drop-down di pojok kanan atas bertuliskan **Preset**.
   - Pilih salah satu template (misal: *Palindrome (wXw^R)*). Flowchart mesin akan langsung ter-render pada canvas.

2. **Menguji Satu String (Mode Simulasi)**:
   - Tulis string yang ingin diuji pada input box **"Input String"** (misal: `abXba`).
   - **Mode Instan**: Klik **Run Simulation** untuk langsung melihat hasil akhir (`ACCEPTED` atau `REJECTED`).
   - **Mode Langkah-demi-Langkah**: Klik **Step Simulation**. Gunakan tombol **Next** dan **Back** untuk menelusuri jalannya mesin. Perhatikan perubahan pada pita input, stack silinder, dan highlight transisi keemasan pada canvas.

3. **Menguji Banyak String Sekaligus**:
   - Pindah ke tab **Batch Test**, tulis beberapa string (satu string per baris) di kotak teks kiri, lalu klik **Jalankan Semua**.

---

## 📋 Template Mesin, Sampel, dan Analisis Teori

Mesin PDA bekerja menggunakan kontrol state (flowchart) dengan bantuan memori tambahan berupa **Stack** yang diawali dengan simbol dasar awal `Z`.

### 1. Template: Palindrome ($wXw^R$)
Mesin ini mendeteksi string yang sama bila dibaca dari depan atau belakang, dipisahkan oleh karakter tengah `X`.
*   **Contoh ACCEPT:** `abXba`
*   **Contoh REJECT:** `abXab`

#### 🔬 Analisis Dasar Teori Keanggotaan:
*   **Kenapa `abXba` di-ACCEPT?**
    1. Mesin mulai di state `START`, stack berisi `['Z']`.
    2. Bagian pertama sebelum `X` dibaca (`a` lalu `b`). Setiap karakter input dimasukkan ke stack (**PUSH**): stack menjadi `['Z', 'a', 'b']`.
    3. Mesin membaca karakter pemisah `X`, beralih ke fase pencocokan.
    4. Setelah `X`, mesin harus melakukan **READ** input dan mencocokkannya dengan simbol teratas stack yang diambil (**POP**).
    5. Karakter berikutnya dibaca: `b`. Mesin melakukan **POP** dari stack, yang keluar adalah `b` (pencocokan sukses, stack menjadi `['Z', 'a']`).
    6. Karakter terakhir dibaca: `a`. Mesin melakukan **POP**, yang keluar adalah `a` (pencocokan sukses, stack menjadi `['Z']`).
    7. Input habis. Mesin melakukan **POP** simbol terakhir `Z`, lalu berpindah ke node `ACCEPT` (Diterima).
*   **Kenapa `abXab` di-REJECT?**
    1. Karakter sebelum `X` (`a` dan `b`) di-**PUSH** ke stack: stack berisi `['Z', 'a', 'b']`.
    2. Karakter `X` dibaca.
    3. Karakter setelah `X` yang dibaca pertama adalah `a`. Mesin mencoba melakukan **POP** dari stack. Namun, simbol teratas stack saat itu adalah `b`.
    4. Karena simbol input (`a`) tidak cocok dengan hasil pop stack (`b`), transisi tidak valid dan simulasi macet sebelum mencapai node `ACCEPT` (Ditolak).

---

### 2. Template: $a^n b^n$
Mesin untuk mencocokkan jumlah karakter `a` di awal dengan jumlah karakter `b` setelahnya.
*   **Contoh ACCEPT:** `aaabbb` (jumlah `a` = 3, `b` = 3)
*   **Contoh REJECT:** `aabbb` (jumlah `a` = 2, `b` = 3)

#### 🔬 Analisis Dasar Teori Keanggotaan:
*   **Kenapa `aaabbb` di-ACCEPT?**
    1. Setiap kali membaca `a` di fase pertama, mesin melakukan **PUSH A** ke stack. Karena ada tiga `a`, stack menjadi `['Z', 'A', 'A', 'A']`.
    2. Saat membaca `b` pertama, mesin berpindah ke fase pengosongan dan melakukan **POP A** (stack menjadi `['Z', 'A', 'A']`).
    3. Untuk dua `b` berikutnya, mesin terus melakukan **POP A** untuk setiap `b`. Stack tersisa `['Z']`.
    4. Input habis, mesin melakukan transisi kosong ($\lambda$) ke node `ACCEPT` karena stack tersisa `Z` (jumlah `a` dan `b` terbukti sama).
*   **Kenapa `aabbb` di-REJECT?**
    1. Membaca dua `a` ➔ stack menjadi `['Z', 'A', 'A']`.
    2. Membaca dua `b` pertama ➔ melakukan dua kali **POP A**, stack tersisa `['Z']`.
    3. Masih ada `b` ketiga pada input. Mesin mencoba melakukan **POP A** lagi dari stack, namun stack sudah tidak memiliki simbol `A` (hanya tersisa simbol dasar `Z`).
    4. Operasi gagal karena ketidakseimbangan jumlah, sehingga string ditolak (`REJECTED`).

---

### 3. Template: Balanced Parentheses (Tanda Kurung Seimbang)
Memastikan setiap kurung buka `(` memiliki pasangan kurung tutup `)` yang menutupnya secara teratur.
*   **Contoh ACCEPT:** `(())`
*   **Contoh REJECT:** `(()`

#### 🔬 Analisis Dasar Teori Keanggotaan:
*   **Kenapa `(())` di-ACCEPT?**
    1. Membaca `(` pertama ➔ **PUSH P** (stack: `['Z', 'P']`).
    2. Membaca `(` kedua ➔ **PUSH P** (stack: `['Z', 'P', 'P']`).
    3. Membaca `)` pertama ➔ **POP P** (stack: `['Z', 'P']`).
    4. Membaca `)` kedua ➔ **POP P** (stack: `['Z']`).
    5. Input habis dan stack kembali bersih berisi `Z`, mesin mencapai node `ACCEPT`.
*   **Kenapa `(()` di-REJECT?**
    1. Membaca dua `(` ➔ stack berisi `['Z', 'P', 'P']`.
    2. Membaca satu `)` ➔ **POP P** (stack tersisa `['Z', 'P']`).
    3. Input habis. Namun, stack masih menyisakan simbol `P` (kurung buka belum ditutup). Karena stack tidak kembali ke simbol dasar `Z` saat pembacaan selesai, transisi ke node `ACCEPT` tidak dapat diambil.

---


## 🎨 Teknologi & Desain Sistem

- **Struktur & Logika**: HTML5 & Javascript vanilla (ES6).
- **Rendering Grafis**: HTML5 Canvas API dengan dynamic DPR (Device Pixel Ratio) scaling agar teks dan garis tetap tajam pada layar resolusi tinggi (Retina/4K).
- **Styling & Tema**: Custom CSS Modern (Tailwind-free) dengan tema **Dark Glassmorphism**:
  - Font Utama: **Outfit** (modern sans-serif) via Google Fonts.
  - Font Monospace: **JetBrains Mono** (untuk log trace dan pita tape).
  - Backdrops blur filter, neon-glow borders, dan animasi transisi CSS mikro.
