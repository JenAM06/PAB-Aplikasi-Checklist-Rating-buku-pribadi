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

### 1. `Text`
**Kategori:** Tampilan

**Penjelasan:** Widget dasar untuk menampilkan teks. Digunakan di seluruh halaman — judul buku, nama penulis, label, persentase progress, hingga pesan empty state.

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

