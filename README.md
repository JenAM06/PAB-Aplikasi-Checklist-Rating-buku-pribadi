# JEJE BookShelf

Aplikasi manajemen koleksi buku pribadi mencatat, melacak progress membaca, dan memberikan rating pada setiap buku yang dimiliki. Disini saya tetap bisa memberikan rating dan catatan walaupun progres membaca saya belum selesai ^^

---

## Struktur Folder

```
lib/
├── main.dart                ← Setup app & theme
├── models/
│   ├── book.dart            ← Class Book (totalPages, currentPage, progress)
│   └── book_provider.dart   ← CRUD + state management
└── pages/
    ├── home_page.dart       ← Daftar buku + progress bar di kartu
    ├── detail_page.dart     ← Detail buku + progress bar besar
    └── form_page.dart       ← Form dengan 6 TextField
```

---

## Fitur Aplikasi

| # | Fitur | Status | Lokasi |
|---|-------|--------|--------|
| 1 | Tambah buku baru | ✅ | `form_page.dart` → `_simpan()` |
| 2 | Edit buku yang ada | ✅ | `form_page.dart` → `_isEditing` |
| 3 | Hapus buku (dengan konfirmasi) | ✅ | `home_page.dart`, `detail_page.dart` → `_konfirmasiHapus()` |
| 4 | Lacak progress membaca | ✅ | `book.dart` → `progress` getter |
| 5 | Progress bar visual | ✅ | `home_page.dart`, `detail_page.dart` → `LinearProgressIndicator` |
| 6 | Tag genre (multi-genre) | ✅ | `form_page.dart` → `_tambahGenre()` |
| 7 | Rating bintang (1–5) | ✅ | `form_page.dart`, `detail_page.dart` |
| 8 | Catatan / ulasan buku | ✅ | `form_page.dart` → `_notesCtrl` |
| 9 | Auto-pop saat buku dihapus | ✅ | `detail_page.dart` → `addPostFrameCallback` |
| 10 | Empty state (belum ada buku) | ✅ | `home_page.dart` → `books.isEmpty` |

---

## Widget yang Digunakan

| Kategori | Widget |
|----------|--------|
| Tampilan | `Text`, `Icon`, `LinearProgressIndicator` |
| Layout | `Scaffold`, `AppBar`, `Row`, `Column`, `Container`, `Padding`, `SizedBox`, `Expanded`, `ListView`, `Wrap`, `Card` |
| Interaksi | `InkWell`, `GestureDetector`, `ElevatedButton`, `OutlinedButton`, `TextButton`, `IconButton`, `FloatingActionButton`, `TextFormField`, `Form`, `AlertDialog`, `SnackBar` |
| Stateless/Stateful | `StatelessWidget` (HomePage, DetailPage, _BookCard), `StatefulWidget` (FormPage), `ChangeNotifierProvider` |
| Navigasi | `Navigator.push`, `Navigator.pop`, `MaterialPageRoute` |

---

## Home Page


### 1. `Teks`
**Penjelasan:** Widget dasar untuk menampilkan teks. Digunakan di seluruh halaman seperti judul buku, nama penulis, label, persentase progress, hingga pesan empty state.

```dart
Text(
  book.title,
  style: const TextStyle(
    color: Colors.white,
    fontSize: 16,
    fontWeight: FontWeight.bold,
  ),
),
```
### 2. `Icon`
**Kategori:** 🖼️ Tampilan

**Penjelasan:** Menampilkan ikon tong sampah. Digunakan sebagai tombol hapus di AppBar.

```dart
IconButton(
    icon: const Icon(
        Icons.delete_outline,
        color: Colors.white30,
        size: 20,),
```

---

### 3. `LinearProgressIndicator`
**Penjelasan:** Menampilkan progress membaca secara visual berupa bar horizontal. Digunakan di kartu buku (5px) dan juga halaman detail (10px).

```dart
ClipRRect(
  borderRadius: BorderRadius.circular(4),
  child: LinearProgressIndicator(
    value: progress,
    minHeight: 5,
    backgroundColor: Colors.white10,
    valueColor: const AlwaysStoppedAnimation(Color(0xFF6BFFD8)),
  ),
),
```
---

### 4. `Scaffold` & `AppBar`

**Penjelasan:** Kerangka untuk `AppBar`, `body`, dan `floatingActionButton`. Semua halaman (`HomePage`, `DetailPage`, `FormPage`) menggunakan `Scaffold`. Widget `AppBar` navigasi di bagian atas halaman. Menampilkan judul halaman, tombol kembali otomatis atau aksi tambahan seperti tombol edit. Di home page, isinya ada judul aplikasi dan jumlah buku yang disimpan di sudut kanan atas.

```dart
return Scaffold(
    appBar: AppBar(
    backgroundColor: const Color(0xFF1A1530),
        title: const Text(
          'Jeje BookShelf',
          style: TextStyle(
            color: Color(0xFF9B6BFF),
            fontWeight: FontWeight.bold,
            fontSize: 22,
          ),
        ),
        actions: [
          Padding(
            padding: const EdgeInsets.only(right: 12),
            child: Center(
              child: Text(
                '${books.length} buku',
                style: const TextStyle(color: Colors.white54, fontSize: 13),
              ),
            ),
          ),
        ],
      ),
```

---

### 5. `Row` & `Expanded`

**Penjelasan:** `Row` untuk menyusun widget secara horizontal. Didalamnya ada baris judul pada bagian kiri lalu tombol hapus di bagian kanan kartu buku. Jadi isi kartu tidak memanjang kebawah. `Expanded` Memaksa `Column` untuk mengambil seluruh sisa ruang horizontal dalam `Row` jadi judul & penulis bisa memanjang sampai mendekati ikon _delete_

```dart
Expanded(
    child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
            Text(
            book.title,
            style: const TextStyle(
                color: Colors.white,
                fontSize: 16,
                fontWeight: FontWeight.bold,
    ),
),
```

---

### 6. `Column`

**Penjelasan:** Menyusun widget secara vertikal. Fungsi di home page ada pada kartu untuk membuat nama dan penulis tersusun atas-bawah.

```dart
Column(
  crossAxisAlignment: CrossAxisAlignment.start,
  children: [
    Text(book.title, ...),
    const SizedBox(height: 4),
    Text(book.author, ...),
  ],
),
```

### 7. `Container`, `Padding`,  `SizedBox`

**Penjelasan:** Widget untuk styling. Memberikan warna latar, padding, border, dan border-radius. Widget ini membuat label kecil berbentuk kotak rounded dengan warna ungu transparan yang berisi teks genre pada home page. `Padding` menambah ruang di sekitar widget. `SizedBox` memberi jarak tetap antar widget atau membatasi ukuran. Keduanya digunakan di seluruh halaman.

```dart
Container(
  padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 5),
  decoration: BoxDecoration(
    color: const Color(0xFF9B6BFF).withValues(alpha: 0.15),
    borderRadius: BorderRadius.circular(10),
  ),
  child: Text(g, style: const TextStyle(color: Color(0xFF9B6BFF))),
),

// SizedBox sebagai spacer
const SizedBox(height: 16),
```

---

### 8. `ListView` & `ListView.builder`

**Penjelasan:** `ListView.builder` di `HomePage` untuk menampilkan daftar (_list_) skartu (_BookCard) per buku yang panjangnya dinamis atau besar (jadi scrollable).

```dart
ListView.builder(
  padding: const EdgeInsets.all(16),
  itemCount: books.length,
  itemBuilder: (context, index) {
    return _BookCard(book: books[index]);
  },
),
```

---

### 9. `Wrap`
**Kategori:** 📐 Layout

**Penjelasan:** Menyusun chip genre secara horizontal dan otomatis pindah ke baris berikutnya jika tidak cukup ruang. Jadi isi teks selanjutnya dibungkus row lagi.

```dart
Wrap(
  spacing: 6,
  runSpacing: 4,
  children: _genres.map((g) => Chip(...)).toList(),
),
```

---

### 10. `Card`. `InkWell`, `Navigator.push`, `MaterialPageRoute`

**Penjelasan:** Membungkus konten kartu buku dengan background berwarna dan sudut melengkung. Memberikan pemisahan visual yang jelas antar item di daftar. `InkWell` Digunakan bersama `Card` agar seluruh area kartu bisa ditekan untuk membuka Detail Page. `push` menambah halaman baru ke stack navigasi. `MaterialPageRoute` Mendefinisikan halaman tujuan dan animasi transisi standar Material Design (slide dari kanan). Digunakan setiap kali `Navigator.push` dipanggil.

```dart
Card(
      color: const Color(0xFF1A1530),
      margin: const EdgeInsets.only(bottom: 12),
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(16)),
      child: InkWell(
        borderRadius: BorderRadius.circular(16),
        onTap: () => Navigator.push(
          context,
          MaterialPageRoute(builder: (_) => DetailPage(bookId: book.id)),
        ),

```
### 10. `Navigator.pop`. `TextButton`, `AlertDialog`

**Penjelasan:**`pop` kembali ke halaman sebelumnya, di _home page_ digunakan untuk membatalkan buku yang akan dihapus lalu kembali pada _home page_. `TextButton` adalah Tombol tanpa latar belakang ("Hapus" dan "Batal"). `AlertDialog` yaitu Dialog konfirmasi sebelum menghapus buku. Digunakan di dua tempat (`HomePage` dan `DetailPage`) untuk mencegah penghapusan tidak sengaja.

```
//`Navigator.pop`
onPressed: () => Navigator.pop(context),
            child: const Text('Batal', style: TextStyle(color: Colors.white54)),
          ),

//AlertDialog
showDialog(
  context: context,
  builder: (_) => AlertDialog(
    backgroundColor: const Color(0xFF1A1530),
    title: const Text('Hapus Buku?', style: TextStyle(color: 

```

---

### 11. `StatelessWidget`
**Kategori:** 🧱 Stateless/Stateful

**Penjelasan:** Digunakan untuk halaman dan widget yang tidak memiliki state internal yang berubah. `HomePage`, `DetailPage`, dan `_BookCard` adalah `StatelessWidget`.
```dart
class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    final books = context.watch().books;
    // ...
  }
}
```

---





