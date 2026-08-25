# Tutorial Berkuliah

Aplikasi dokumentasi dan panduan perkuliahan untuk mahasiswa **Institut Teknologi Kalimantan** yang dibangun menggunakan **Next.js** dan **Fumadocs**.

---

## Memulai (Quick Start)

Jalankan server pengembangan lokal:

```bash
npm run dev
# atau
pnpm dev
```

Buka [http://localhost:3000](http://localhost:3000) (atau port 3001) di browser Anda untuk melihat situs. Halaman dokumentasi berada di rute `/docs`.

---

## Panduan Menulis & Mengatur Konten (Fumadocs)

Semua konten tulisan dan dokumentasi disimpan di dalam folder `content/docs/`.

### 1. Struktur Folder `content/docs/`

```text
content/docs/
├── meta.json                    <-- 1. Pengatur Menu Utama / Sidebar Root
├── index.mdx                    <-- 2. Halaman Beranda Dokumen (/docs)
└── pengenalan-maba/             <-- 3. Folder Kategori Bab (/docs/pengenalan-maba)
    ├── meta.json                <-- 4. Pengatur Urutan Menu Sub-Folder Ini
    ├── index.mdx                <-- Halaman Utama Kategori
    ├── sistem-akademik.mdx      <-- Halaman Sub-Bab (sistem-akademik)
    └── kehidupan-kampus.mdx     <-- Halaman Sub-Bab (kehidupan-kampus)
```

---

### 2. Fungsi `meta.json` (Pengatur Sidebar)

`meta.json` bertindak sebagai **Daftar Isi / Pengatur Sidebar**. Tanpa file ini, Fumadocs akan mengurutkan halaman berdasarkan abjad secara otomatis. Dengan `meta.json`, Anda dapat menentukan **judul bab** dan **urutan halaman** yang tampil di menu samping.

#### Contoh `content/docs/meta.json` (Root):
```json
{
  "title": "Tutorial Berkuliah",
  "root": true,
  "pages": [
    "---Pengenalan---",
    "index",
    "---Panduan Maba ITK---",
    "pengenalan-maba"
  ]
}
```

#### Contoh `content/docs/pengenalan-maba/meta.json` (Sub-Folder):
```json
{
  "title": "Pengenalan Maba ITK",
  "pages": [
    "index",
    "sistem-akademik",
    "kehidupan-kampus"
  ]
}
```
> **Catatan:** Cukup tuliskan nama file tanpa ekstensi `.mdx` (contoh: tulis `"sistem-akademik"`, bukan `"sistem-akademik.mdx"`). Untuk membuat judul pemisah di sidebar, gunakan format `"---Judul Pemisah---"`.

---

### 3. File Structure `.mdx`

File `.mdx` terdiri dari 2 bagian utama: **Frontmatter** (metadata paling atas) dan **Body Konten**.

```mdx
---
title: Judul Halaman                <-- FRONTMATTER (Wajib di awal file)
description: Deskripsi singkat bab ini
---

# Judul Utama Halaman (H1)          <-- BODY KONTEN (Markdown & MDX)

Ini adalah paragraf biasa. Anda dapat menulis teks **tebal** atau *miring*.

## Sub Judul Bab (H2)

- Poin 1
- Poin 2

### Komponen Spesial Fumadocs (Opsional)

<Cards>
  <Card title="Judul Kartu" href="/docs/tujuan" description="Penjelasan singkat" />
</Cards>
```

---

### 4. Langkah-Langkah Menambah Halaman Baru

Misalkan Anda ingin menambah halaman baru **"Informasi Beasiswa"** di bawah kategori Maba:

1. **Buat file `.mdx` baru:**  
   `content/docs/pengenalan-maba/beasiswa.mdx`
2. **Isi konten file:**
   ```mdx
   ---
   title: Informasi Beasiswa ITK
   description: Daftar beasiswa KIP-K, Djarum, dan Pemprov untuk mahasiswa ITK.
   ---

   # Beasiswa ITK
   ...
   ```
3. **Daftarkan ke `meta.json`:**  
   Buka `content/docs/pengenalan-maba/meta.json` dan tambahkan nama file `"beasiswa"` di daftar `pages`:
   ```json
   {
     "title": "Pengenalan Maba ITK",
     "pages": [
       "index",
       "sistem-akademik",
       "kehidupan-kampus",
       "beasiswa"
     ]
   }
   ```

Halaman baru akan otomatis muncul di menu navigasi sidebar!

---

## Arsitektur Proyek

| Rute / Lokasi | Keterangan |
| :--- | :--- |
| `content/docs/` | Semua file `.mdx` dan `meta.json` konten tulisan. |
| `app/(home)` | Halaman landing page / depan utama aplikasi. |
| `app/docs` | Layout dan renderer dokumentasi Fumadocs. |
| `lib/source.ts` | Konfigurasi Loader & Adapter Fumadocs MDX. |
