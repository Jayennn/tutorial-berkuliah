# Tutorial Berkuliah
Dibangun dengan **Next.js**, **Fumadocs**, dan **Bun**.

---

## Daftar Isi

- [Tutorial Berkuliah](#tutorial-berkuliah)
  - [Daftar Isi](#daftar-isi)
  - [Tech Stack](#tech-stack)
  - [Prasyarat](#prasyarat)
  - [Cara Menjalankan](#cara-menjalankan)
  - [Script yang Tersedia](#script-yang-tersedia)
  - [Struktur Proyek](#struktur-proyek)
  - [Cara Menambah / Mengedit Artikel](#cara-menambah--mengedit-artikel)
    - [1. Buat file baru dengan nama yang benar](#1-buat-file-baru-dengan-nama-yang-benar)
    - [2. Isi frontmatter + konten](#2-isi-frontmatter--konten)
    - [3. Daftarkan ke sidebar (jika perlu)](#3-daftarkan-ke-sidebar-jika-perlu)
    - [4. Tambahkan gambar (jika perlu)](#4-tambahkan-gambar-jika-perlu)
    - [5. Pakai komponen Callout untuk tips](#5-pakai-komponen-callout-untuk-tips)
  - [Alur Kontribusi (Pull Request)](#alur-kontribusi-pull-request)
    - [Konvensi commit](#konvensi-commit)
  - [Troubleshooting](#troubleshooting)

---

## Tech Stack

| Teknologi | Kegunaan |
| --- | --- |
| [Next.js 16](https://nextjs.org/) | Framework React + rendering dokumentasi |
| [Fumadocs](https://fumadocs.dev/) | Engine dokumentasi (sidebar, search, MDX) |
| [Tailwind CSS 4](https://tailwindcss.com/) | Styling |
| [Biome](https://biomejs.dev/) | Linter + formatter |
| [Bun](https://bun.sh/) | Package manager & runtime (satu-satunya yang dipakai) |

---

## Prasyarat

- **Git**
- **Bun ≥ 1.3.14** — cek dengan `bun --version`
- **Node.js** (dibutuhkan Next.js di balik layar; versi LTS terbaru disarankan)

> Proyek ini memakai `bun` secara eksklusif (`packageManager: bun@1.3.14` di `package.json`).
> Jangan install dengan `npm`, `pnpm`, atau `yarn` agar lockfile tidak tercampur.
> Satu-satunya lockfile yang berlaku adalah `bun.lock`.

---

## Cara Menjalankan

```bash
# 1. Clone repo
git clone https://github.com/Research-Technology-HMIF-ITK/tutorial-berkuliah.git
cd tutorial-berkuliah

# 2. Install dependensi
bun install

# 3. Jalankan server pengembangan
bun dev
```

Buka di browser:

- Landing page: [http://localhost:3000](http://localhost:3000)
- Dokumentasi: [http://localhost:3000/docs](http://localhost:3000/docs)

Build produksi untuk memastikan tidak ada yang rusak sebelum push:

```bash
bun run build
bun start   # menjalankan hasil build secara lokal
```

---

## Script yang Tersedia

| Perintah | Kegunaan |
| --- | --- |
| `bun dev` | Menjalankan server pengembangan |
| `bun run build` | Build produksi (wajib lolos sebelum PR) |
| `bun start` | Menjalankan hasil `build` secara lokal |
| `bun run types:check` | Cek tipe TypeScript (`next typegen && tsc --noEmit`) |
| `bun run lint` | Cek lint + format dengan Biome |
| `bun run format` | Format otomatis dengan Biome |

Checklist sebelum push:

```bash
bun run build && bun run types:check && bun run lint
```

---

## Struktur Proyek

```text
.
├── app/                    # Routing Next.js (layout, halaman docs, API search)
│   ├── layout.tsx          # Root layout + provider Fumadocs
│   └── (docs)/             # Layout & renderer halaman dokumentasi
├── content/
│   └── docs/               # ← SEMUA KONTEN ARTIKEL ADA DI SINI
│       ├── meta.json       # Sidebar root (daftar section)
│       ├── index.mdx       # Halaman pembuka /docs
│       ├── Akademik/       # Section: FRS, SIAKAD, jadwal, SIMKUR, layanan fakultas
│       ├── SIKAP/          # Section: SK2PM / SIKAP
│       ├── LMS/            # Section: LMS kuliah.itk.ac.id
│       └── Survival-Kit/   # Section: Google Calendar, Notion, Obsidian, dsb.
├── lib/                    # Konfigurasi source Fumadocs (source.ts, layout, cn)
├── public/                # Aset gambar (diakses sebagai /nama-file.webp)
├── WRITTING_STYLE.md       # Panduan gaya penulisan (wajib dibaca kontributor konten)
└── package.json            # Dependensi + script (packageManager: bun)
```

Setiap folder section berisi `meta.json` (pengatur sidebar section tersebut) dan file-file `.mdx` (artikel).

---

## Cara Menambah / Mengedit Artikel

Semua artikel adalah file `.mdx` di dalam `content/docs/`. Tidak perlu menyentuh kode React untuk menambah artikel.

### 1. Buat file baru dengan nama yang benar

Nama file = URL artikel. Aturannya:

- Huruf kecil semua
- Kata dipisah `-` (bukan `_` atau spasi)
- Tanpa prefix angka, deskriptif 2–4 kata

```text
Baik: content/docs/Akademik/frs.mdx
Baik: content/docs/Akademik/jadwal-perkuliahan.mdx
Buruk: content/docs/Akademik/FRS.mdx
Buruk: content/docs/Akademik/01_jadwal.mdx
```

### 2. Isi frontmatter + konten

Setiap artikel wajib diawali frontmatter `title` dan `description`:

```mdx
---
title: "Formulir Rencana Studi (FRS)"
description: Kenalan dengan FRS, SKS, dan cara mengisi rencana studi di Gerbang ITK.
---

Paragraf pembuka di sini...

## Heading Bagian

Isi artikel...
```

Aturan frontmatter:

- `title` singkat dan mudah dicari (dipakai sidebar, bukan tempat hook/clickbait)
- `description` satu kalimat jelas tentang isi artikel

### 3. Daftarkan ke sidebar (jika perlu)

- **Artikel baru di dalam section yang `meta.json`-nya berisi `"..."`** (misal `Akademik/`) → otomatis muncul di sidebar, tidak perlu konfigurasi tambahan.
- **Section/folder baru** → daftarkan manual di `content/docs/meta.json`:

```json
{
  "title": "Tutorial Berkuliah",
  "root": true,
  "pages": [
    "---Mulai di Sini---",
    "index",
    "informasi-resmi",
    "---Sistem Akademik---",
    "Akademik",
    "SIKAP",
    "LMS",
    "---Tips & Tricks---",
    "Survival-Kit"
  ]
}
```

Tulis nama file/folder tanpa ekstensi `.mdx`. Format `"---Judul---"` membuat judul pemisah di sidebar.

### 4. Tambahkan gambar (jika perlu)

1. Simpan gambar di `public/`, misal `public/Pengisian_FRS_1.webp`
2. Referensikan dengan path absolut + alt text deskriptif:

```mdx
![Halaman login Gerbang ITK](/Pengisian_FRS_1.webp)
```

Alt text wajib deskriptif (bukan `![Steps]` atau `![gambar]`).

### 5. Pakai komponen Callout untuk tips

Jangan pakai blockquote (`>`) untuk tips/catatan. Pakai komponen `<Callout>` (tersedia global, tanpa import):

```mdx
<Callout title="Tips Praktis" type="info">

Isi tips di sini.

</Callout>
```

| Tipe | Kegunaan |
| --- | --- |
| `info` | Info penting, syarat wajib, tips praktis |
| `warn` | Peringatan yang bisa bikin gagal (batas SKS, salah isi data) |
| `success` | Status berhasil / validasi |

> Selalu beri baris kosong setelah tag pembuka dan sebelum tag penutup agar ter-render benar.


## Alur Kontribusi (Pull Request)

Proyek ini memakai fork workflow. Repo utama: `Research-Technology-HMIF-ITK/tutorial-berkuliah`.

```bash
# 1. Fork repo utama di GitHub, lalu clone fork kamu
git clone https://github.com/<username>/tutorial-berkuliah.git
cd tutorial-berkuliah

# 2. Buat branch baru dari main
git checkout -b docs/judul-perubahan

# 3. Kerjakan perubahan, lalu pastikan lolos cek
bun run build && bun run types:check && bun run lint

# 4. Commit + push
git add -A
git commit -m "docs(akademik): rewrite jadwal guide in senior-student tone"
git push origin docs/judul-perubahan
```

Lalu buka Pull Request dari branch kamu ke `main` repo utama.

### Konvensi commit

Proyek ini mengikuti [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/):

```text
<type>[scope]: <deskripsi singkat huruf kecil, tanpa titik>

[body opsional: apa yang berubah dan kenapa]
```

| Type | Dipakai untuk |
| --- | --- |
| `docs` | Rewrite / update isi artikel |
| `feat` | Section atau artikel baru |
| `fix` | Perbaikan fakta, typo, link rusak |
| `chore` | Rename file, restrukturisasi tanpa ubah isi |

Contoh:

```text
docs(akademik): rewrite frs guide with senior-student tone
feat(survival-kit): add google calendar guide
fix(akademik): correct ips-to-sks table
chore(docs): rename numbered folders to clean slugs
```

---

## Troubleshooting

| Masalah | Solusi |
| --- | --- |
| Port 3000 sudah dipakai | Next otomatis pindah ke 3001, atau jalankan `bun dev --port 3002` |
| `bun install` gagal / lockfile berubah sendiri | Pastikan hanya pakai `bun`. Hapus `node_modules` lalu `bun install` ulang |
| Halaman docs 404 setelah tambah file | Pastikan file berekstensi `.mdx`, ada frontmatter `title`, dan (untuk section baru) terdaftar di `meta.json` |
| Gambar tidak muncul | Pastikan file ada di `public/` dan path diawali `/`, misal `/Pengisian_FRS_1.webp` |
| Callout tidak ter-render | Pastikan ada baris kosong setelah tag pembuka dan sebelum tag penutup |

---

Ada pertanyaan atau nemu info yang sudah kedaluwarsa? Buka issue atau langsung kirim PR — semua kontribusi dari mahasiswa ITK sangat diterima.
