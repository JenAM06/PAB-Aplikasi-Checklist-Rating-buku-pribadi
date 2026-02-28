# JEJE BookShelf

Aplikasi manajemen koleksi buku pribadi untuk mencatat, melacak progress membaca, dan memberikan rating pada setiap buku yang dimiliki. Disini saya tetap bisa memberikan rating dan catatan walaupun progres membaca saya belum selesai ^^

> Jen Agresia Misti | 2409116007 | A'24 | MINPRO 1 PAB

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

> Saya jelaskan alur disini ദ്ദി◝ ⩊ ◜.ᐟ

Pada tampilan awal, sudah diperlihatkan pada AppBar ada nama aplikasi dan jumlah buku di sudut kanan atas. Juga ada tulisan bahwa belum ada widget 'card' atau simpelnya tampilan buku pribadi saya. Klik tanda `+` sebagai **fitur** `Create` agar halaman menuju `Form page`. Hasil `Create` akan mengembalikan pada halaman _home page_ dan saya juga bisa menggunakan **fitur** `hapus` dengan klik ikon tong sampah pada sudut kanan tiap card buku yang telah dibuat.

| Kosongan | Tambah Buku | Hapus Buku |
|----------|------------|-----------|
| <img src="https://github.com/user-attachments/assets/d0c935ac-445b-4400-956a-e366cdb3cec5" width="150"/> | <img src="https://github.com/user-attachments/assets/ab4e424f-5443-40a4-8c95-2ca352880fc5" width="150"/> | <img src="https://github.com/user-attachments/assets/8ae6f8d4-efdb-490c-bc00-7420fd13025b" width="150"/> |



<details>
    <summary> Deskripsi Implementasi Widget</summary>

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
### 2. `Icon` dan `IconButton`

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

**Penjelasan:** Kerangka untuk `AppBar`, `body`, dan `floatingActionButton`. Semua halaman (`HomePage`, `DetailPage`, `FormPage`) menggunakan `Scaffold`. Widget `AppBar` adalah navigasi di bagian atas halaman. Menampilkan judul halaman, tombol kembali otomatis atau aksi tambahan seperti tombol edit. Di home page, isinya ada judul aplikasi dan jumlah buku yang disimpan di sudut kanan atas.

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

### 12. `SnackBar`

**Penjelasan:** Notifikasi ringan di bagian bawah layar setelah aksi berhasil atau gagal validasi. Menggunakan `SnackBarBehavior.floating` agar terlihat melayang.

```dart
ScaffoldMessenger.of(context).showSnackBar(
  SnackBar(
    content: const Text('Buku berhasil ditambahkan!'),
    backgroundColor: const Color(0xFF9B6BFF),
    behavior: SnackBarBehavior.floating,
    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
  ),
);
```

---

### 13. `FloatingActionButton.extended`

**Penjelasan:** Tombol aksi utama di `HomePage` untuk navigasi ke halaman tambah buku. Versi *extended* menampilkan ikon dan label teks secara bersamaan.

```dart
FloatingActionButton.extended(
  backgroundColor: const Color(0xFF9B6BFF),
  onPressed: () => Navigator.push(
    context,
    MaterialPageRoute(builder: (_) => const FormPage()),
  ),
  icon: const Icon(Icons.add, color: Colors.white),
  label: const Text('Tambah Buku'),
),
```

---

</details>

## Detail Page

> Saya jelaskan alur disini ◝(ᵔᗜᵔ)◜

Halaman detail bisa diakses dengan klik card buku yang sudah dibuat. Detail memperlihat catatan atau review yang tidak diperlihatkan pada homepage. Halaman detail sebagai tempat pilihan untuk **fitur** `edit` (klik ikon pensil sudut kanan atas) atau menghapus buku yang telah dipilih. **Fitur** `hapus` di halaman detail sengaja tidak dibuat mencolok agar saya sendiri juga bisa fokus pada detail dari isi buku-bukunya

| Tampilan | Tampilan Hapus | Hapus Buku | Balik Home Page |
|----------|------------|-----------|-----------|
| <img src="https://github.com/user-attachments/assets/2fcc1426-614c-4763-a627-43fd344a3d9e" width="150"/> | <img src="https://github.com/user-attachments/assets/f0474134-5e7a-423e-8d9c-49a0b9d189bd" width="150"/> | <img src="https://github.com/user-attachments/assets/79591258-6ba1-41f3-9874-b1fd379c1a29" width="150"/> | <img src="https://github.com/user-attachments/assets/5e953e1b-c4c5-4b60-b9b2-fcfa168dc831" width=150/>


<details>
    <summary> Deskripsi Implementasi Widget </summary>

### 1. `OutlinedButton`

**Penjelasan:** Tombol hapus buku di `DetailPage` menggunakan border merah tanpa fill untuk membedakannya secara visual dari aksi primer. _Widget_ tambahan agar tidak terlalu mencolok dibandingkan tombol utama (seperti ElevatedButton).

```dart
OutlinedButton.icon(
  onPressed: () => _konfirmasiHapus(context, book),
  style: OutlinedButton.styleFrom(
    foregroundColor: Colors.redAccent,
    side: const BorderSide(color: Colors.redAccent),
    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
  ),
  icon: const Icon(Icons.delete_outline),
  label: const Text('Hapus Buku'),
),
```

---

</details>

## Form Page

> Saya jelaskan alur disini („• ֊ •„)੭

Halaman `Form` bisa diakses dari **fitur** `edit` di halaman `detail` atau dari fitur `create`. Terdapat 6 `TextFields` yang diisi (detailnya ada pada deskripsi widget) dan 2 **fitur** unik yaitu `progres membaca` dan `rating bintang`. Bar persen pada card di halaman page berasan dari jumlah halaman yang sudah kita baca dari total halaman buku, berguna jika kehilangan penanda buku atau sekedar melihat progres membaca. Saya juga dapat memberi rating dengan klik ikon bintang pada halaman `form`. Saya mengatur agar tetap bisa memberi rating walaupun belum 100% selesai membaca untuk mempertimbangkan jika sudah tidak ingin melanjutkan buku tersebut dan menaruh alasannya di catatan.

| Tampilan | Tampilan Isi | Validasi (1) | Validasi(2) | POV fitur edit | Balik ke Home page
|----------|------------|-----------|-----------|-----------|-----------|
| <img src="https://github.com/user-attachments/assets/2d9a4e9e-f37d-4a4e-9eae-c89c8d032586" width="150"/> |<img src="https://github.com/user-attachments/assets/7b339667-a1b6-48a6-98de-6632ef3898b6" width="150"/> | <img src="https://github.com/user-attachments/assets/29bce5db-6fbf-4bbe-a2f0-a2a4602af254" width="150"/> | <img src="https://github.com/user-attachments/assets/d0bda3e3-6ded-4306-92d6-d7354693b3a4" width=150/>|<img src="https://github.com/user-attachments/assets/73b67212-1e6e-4374-9279-4d8b534365dc" width="150"/> | <img src="https://github.com/user-attachments/assets/b1107d22-0922-49bd-a631-22659a6c3ec4"  width="150"/>


<details>
    <summary> Deskripsi Implementasi Widget </summary>

### 1. `StatefulWidget`

**Penjelasan:** Digunakan untuk `FormPage` karena perlu menyimpan state lokal: isi controller, daftar genre, dan nilai rating yang berubah saat pengguna mengisi form. Jadi tidak reload halaman, langsung update contohnya ikon bintang yang langsung _full_ kuning ketika dikllik.

```dart
class FormPage extends StatefulWidget {
  final Book? bookToEdit;
  const FormPage({super.key, this.bookToEdit});

  @override
  State createState() => _FormPageState();
}
```

---

### 2. `GestureDetector`

**Penjelasan:** Mendeteksi tap pada setiap bintang rating dan tombol `+` genre. Lebih ringan dari `InkWell` karena tidak membutuhkan efek visual ripple.

```dart
GestureDetector(
  onTap: () => setState(() => _rating = i + 1.0),
  child: Icon(
    i < _rating ? Icons.star : Icons.star_border,
    color: const Color(0xFFFFD700),
    size: 34,
  ),
),
```

---

### 4. `ElevatedButton`

**Penjelasan:** Tombol utama untuk menyimpan data buku di `FormPage` dengan background ungu sebagai yang paling menonjol.

```dart
ElevatedButton(
  onPressed: _simpan,
  style: ElevatedButton.styleFrom(
    backgroundColor: const Color(0xFF9B6BFF),
    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(14)),
  ),
  child: Text(
    _isEditing ? 'Simpan Perubahan' : 'Tambah ke Koleksi',
    style: const TextStyle(color: Colors.white, fontWeight: FontWeight.bold),
  ),
),
```

### 5. `TextFormField`

**Penjelasan:** Input teks dengan dukungan validasi terintegrasi dalam `Form`. Digunakan untuk 6 input di `FormPage`, masing-masing dikonfigurasi melalui helper `_buildTextField()`.

| # | Field | Tipe Input | Validasi |
|---|-------|-----------|---------|
| 1 | Judul | Teks | Wajib diisi |
| 2 | Penulis | Teks | Wajib diisi |
| 3 | Genre | Teks + tombol `+` | Min. 1 genre |
| 4 | Total Halaman | Angka | Opsional |
| 5 | Halaman Ke- | Angka | Tidak boleh > total |
| 6 | Catatan | Multiline | Opsional |

### 6. `Form` + `GlobalKey<FormState>`

**Penjelasan:** Membungkus seluruh input di `FormPage` dalam satu unit validasi. `GlobalKey` digunakan untuk memicu validasi semua field sekaligus saat tombol simpan ditekan.

```dart
final _formKey = GlobalKey();

// Saat simpan:
if (!_formKey.currentState!.validate()) return;
```

---

</details>
