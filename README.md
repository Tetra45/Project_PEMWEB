# Jaga Pesisir (FUAMBA)
FUAMBA - Facts & Understanding: Aquatic, Mangrove & Beach Archive

Website Konservasi Laut dan Pantai
SDG 13 dan SDG 14

## Anggota Kelompok
1. Komang Hendra Swantara - 2515091013 - Navigasi, footer, homepage(setengah), & informasi page.
2. Fransiskus Asisi Nathaniel Yudhistira Dheasaputra - 2515091063 - Edukasi & Panduan.
3. I Gusti Ngurah Agung Rangga Putrasena -2515091073 - tentang, homepage(setengah), & back to top button. 
4. Ananda Gede Isvara Vidhi Sairam - 2515091088 - artikel1, artikel2 & artikel3.

## Deskripsi Website
Tujuan : Menyajikan fakta-fakta ilmiah tentang keanekaragaman hayati pesisir serta isu SDG 13 & 14.

Manfaat:
-Menyediakan informasi yang mudah dipahami dan dapat ditelusuri sumbernya.
-Menjelaskan dampak perubahan iklim terhadap laut dan pesisir.
-Mendorong tindakan nyata untuk menjaga ekosistem laut.


Sasaran pengguna: Masyarakat/khalayak luas.

## Halaman Website
Beranda, Edukasi, Panduan, Informasi, Artikel1, artikel2, artikel3, Tentang.

## Teknologi
HTML5, CSS3, Flexbox/Grid, CSS Media Queries.

## Penjelasan Implementasi

Website FUAMBA terdiri dari 5 halaman utama (`index.html`, `edukasi.html`, `informasi.html`, `panduan.html`, tentang.html) dan 3 halaman artikel (`artikel-1`. sampai `artikel-3.html`). Setiap halaman punya file CSS sendiri di folder css/, tetapi komponen bersama (navbar, footer, tombol kembali ke atas, font, dan variabel warna) memakai kode yang sama di semua halaman supaya tampilannya konsisten.

### 1. Navigasi
**a. Navbar dan tautan antarhalaman**

Navbar memakai Flexbox: logo di kiri, menu di kanan.

```css
nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 4rem;
    position: relative;
    z-index: 2;
}
.menu { display: flex; gap: 2rem; }
nav a:hover { border-bottom: 3px solid var(--teks-gelap); }
```


Fungsi: `justify-content: space-between` mendorong logo dan menu ke dua sisi. `position: relative` pada `nav` dibutuhkan agar menu versi HP bisa diposisikan terhadap navbar. Efek hover berupa garis bawah menandai menu yang sedang disorot.

Path tautan menyesuaikan lokasi file. `index.html` ada di root, sehingga tautannya `html/edukasi.html`. Halaman lain ada di folder `html/`, sehingga tautannya `../index.html` untuk kembali ke beranda.

**b. Menu hamburger tanpa JavaScript**

Menu di HP dibuat dengan teknik *checkbox hack*: checkbox tersembunyi menyimpan status buka/tutup, dan CSS membaca statusnya lewat `:checked`.

```html
<input type="checkbox" id="menu-toggle" class="menu-toggle">
<label for="menu-toggle" class="hamburger">
  <span></span><span></span><span></span>
</label>
<div class="menu"> ... </div>
```

```css
.menu-toggle { position: absolute; opacity: 0; pointer-events: none; }
.hamburger { display: none; }              /* tersembunyi di desktop */

@media (max-width: 768px) {
    .hamburger { display: flex; }
    .menu { display: none; position: absolute; top: 100%; left: 0; right: 0;
            flex-direction: column; }
    .menu-toggle:checked ~ .menu { display: flex; }
}
```

Fungsi: `label for="menu-toggle"` membuat klik pada ikon hamburger mencentang checkbox. Selector `~` (sibling) lalu menampilkan `.menu` saat checkbox tercentang. Tiga garis `span` juga berubah menjadi tanda X lewat `.menu-toggle:checked + .hamburger span:nth-child(...)` yang memutar garis dengan `transform: rotate(...)`. 

**C. Tombol kembali ke atas**

```html
<a href="#top" class="back-to-top">
    <img src="../assets/images/arrkey.png" class="arrow-icon">
</a>
```

```css
html { scroll-behavior: smooth; }
.back-to-top {
    position: absolute; bottom: 20px; right: 20px;
    width: 50px; height: 50px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
}
```

Fungsi: `href="#top"` membawa pengguna ke bagian atas halaman, dan `scroll-behavior: smooth` membuat gulirannya halus. Tombol dibuat bulat dengan `border-radius: 50%`, dan ikon panah dipusatkan dengan Flexbox.

# Layout 

**a. Reset dan variabel warna**
```css
* { margin: 0; padding: 0; box-sizing: border-box; }
:root {
    --teks-gelap: #1D3557;
    --biru: #457B9D;
    --biru-muda: #A8DADC;
    --putih-bg: #F1FAEE;
}
```
Fungsi: reset menghilangkan margin dan padding bawaan browser. `box-sizing: border-box` membuat padding dan border dihitung di dalam lebar elemen, sehingga ukuran lebih mudah diprediksi. Variabel CSS menyimpan palet warna di satu tempat, jadi mengganti warna tema cukup di satu baris.


### 3. Responsive Design
Tata letak menyesuaikan lewat media query:

`max-width: 768px` | Semua | Menu berubah menjadi hamburger |
`max-width: 768px` | Beranda | Hero rata tengah, `.SDG` jadi satu kolom, `.grid-box` jadi `1fr` |
`max-width: 1100px` / `820px` | Informasi | `.explain` dari 3 kolom menjadi 2, lalu 1 kolom |
`max-width: 900px` | Edukasi | Section menjadi satu kolom (`title`, `media`, `cards`) |
`max-width: 600px` | Edukasi | Kartu menjadi 1 kolom, ukuran font disesuaikan |

Contoh di beranda, urutan SDG 13 dibalik saat satu kolom agar gambar tidak selalu di bawah:

```css
@media (max-width: 768px) {
    .SDG { flex-direction: column; gap: 2rem; }
    .SDG-13 .SDG { flex-direction: column-reverse; }
    .grid-box { grid-template-columns: 1fr; }
}
```
Di halaman Panduan, kartu berselang-seling di desktop. Saat menjadi satu kolom, `order` dipakai agar gambar selalu tampil sebelum teksnya:
```css
@media (max-width: 768px) {
    .steps-container { display: flex; flex-direction: column; }
    .steps-container > :nth-child(3) { order: 4; }
    .steps-container > :nth-child(4) { order: 3; }
}
```

## Struktur Folder
>Project_PEMWEB
    >assets
        >fonts
        >images
    >css
        >artikel.css
        >edukasi.css
        >indexx.css
        >informasi.css
        >panduan.css
        >tentang.css
    >html
        >artikel1.html
        >artikel2.html
        >artikel3.html
        >edukasi.html
        >informasi.html
        >panduan.html
        >tentang.html
    >index.html
    >Readme.md

## Website dan Repository
https://tetra45.github.io/Project_PEMWEB/.

## Referensi
gambar dan assets : unsplash dan AI generated
informasi: artikel
