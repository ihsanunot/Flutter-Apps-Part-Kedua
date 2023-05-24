# Belajar UI Flutter News App

Di dalam project Anda, buatlah berkas styles.dart. Di sini kita akan menyimpan data theme untuk digunakan nantinya.

Perubahan pertama yang akan kita lakukan adalah warna. Aplikasi Fortnightly memiliki dua varian warna yang digunakan dalam aplikasinya. 

Kita akan gunakan kedua warna ini ke dalam aplikasi News App kita. Anda bebas untuk mencoba kombinasi warna lainnya.


Tambahkan variabel Coloruntuk menyimpan warna dalam berkas styles.dart.

```
import 'package:flutter/material.dart';
 
const Color primaryColor = Color(0xFFFFFFFF);
const Color secondaryColor = Color(0xFF6B38FB);
```

**Color didefinisikan dengan nilai hexadecimal setelah 0x.**

Dua digit pertama setelah 0x menunjukkan nilai alpha atau transparansi dari warna di mana nilainya mulai dari rentang 00 (transparan) hingga FF (jelas atau non-transparan). 

Enam digit berikutnya mewakili nilai RGB dengan rentang yang sama.

Buka berkas main.dart lalu tambahkan parameter ThemeData dan colorScheme seperti berikut pada MaterialApp.

```
theme: ThemeData(
  colorScheme: Theme.of(context).colorScheme.copyWith(
        primary: primaryColor,
        onPrimary: Colors.black,
        secondary: secondaryColor,
      ),
),
```
Simpan perubahan lalu jalankan hot restart. 

Harusnya aplikasi Anda sudah berubah menjadi warna putih dengan efek scrolling berwarna ungu (efek scrolling hanya muncul pada perangkat Android).

Selanjutnya kita akan mengubah tipografi aplikasi. Typography ada dua jenis font yang bisa digunakan, yaitu untuk font untuk display dan untuk dibaca.

 Fortnightly menggunakan font Merriweather untuk teks display dan Libre Franklin untuk konten bacaan utama. Untuk memudahkan, Anda dapat memanfaatkan fitur type scale dari Material Design untuk mendapatkan kode tema untuk TextTheme.

 ```
 final TextTheme myTextTheme = TextTheme(
  headline1: GoogleFonts.merriweather(
      fontSize: 92, fontWeight: FontWeight.w300, letterSpacing: -1.5),
  headline2: GoogleFonts.merriweather(
      fontSize: 57, fontWeight: FontWeight.w300, letterSpacing: -0.5),
  headline3:
      GoogleFonts.merriweather(fontSize: 46, fontWeight: FontWeight.w400),
  headline4: GoogleFonts.merriweather(
      fontSize: 32, fontWeight: FontWeight.w400, letterSpacing: 0.25),
  headline5:
      GoogleFonts.merriweather(fontSize: 23, fontWeight: FontWeight.w400),
  headline6: GoogleFonts.merriweather(
      fontSize: 19, fontWeight: FontWeight.w500, letterSpacing: 0.15),
  subtitle1: GoogleFonts.merriweather(
      fontSize: 15, fontWeight: FontWeight.w400, letterSpacing: 0.15),
  subtitle2: GoogleFonts.merriweather(
      fontSize: 13, fontWeight: FontWeight.w500, letterSpacing: 0.1),
  bodyText1: GoogleFonts.libreFranklin(
      fontSize: 16, fontWeight: FontWeight.w400, letterSpacing: 0.5),
  bodyText2: GoogleFonts.libreFranklin(
      fontSize: 14, fontWeight: FontWeight.w400, letterSpacing: 0.25),
  button: GoogleFonts.libreFranklin(
      fontSize: 14, fontWeight: FontWeight.w500, letterSpacing: 1.25),
  caption: GoogleFonts.libreFranklin(
      fontSize: 12, fontWeight: FontWeight.w400, letterSpacing: 0.4),
  overline: GoogleFonts.libreFranklin(
      fontSize: 10, fontWeight: FontWeight.w400, letterSpacing: 1.5),
);
 ```

**Catatan : Jika Anda menyalin dari type scale, Anda perlu menambahkan package google_fonts.**

 - https://pub.dev/packages/google_fonts

lalu tinggal import aja :

- import 'package:google_fonts/google_fonts.dart';

Tambahkan myTextThemesebagai parameter dalam ThemeData

```
theme: ThemeData(
  ...
  textTheme: myTextTheme,
),
```

Jalankan perubahan. Saat ini teks dalam aplikasi seharusnya telah berubah.


Selanjutnya kita ubah elevation untuk efek bayangan dari App Bar. Kita tambahkan parameter appBarThemepada ThemeData.

```
theme: ThemeData(
  ...
  appBarTheme: AppBarTheme(elevation: 0),
),
```
Selanjutnya kita akan ubah bentuk tampilan dari Button. 

Di dalam ThemeData terdapat parameter elevatedButtonTheme untuk mengatur ini. Petama, kita ubah warna button dengan secondaryColor.

```
theme: ThemeData(
  ...
  elevatedButtonTheme: ElevatedButtonThemeData(
    style: ElevatedButton.styleFrom(
      backgroundColor: secondaryColor,
      foregroundColor: Colors.white,
      textStyle: const TextStyle(),
    ),
  ),
),
```

Kita gunakan parameter textStyle untuk mengatur gaya teks di dalam button.

Kemudian kita ubah bentuk button sesuai saran dari studi kasus Fortnightly, yaitu dengan memberikan sudut radius sebesar 0 dp.

```
theme: ThemeData(
  ...
  elevatedButtonTheme: ElevatedButtonThemeData(
    style: ElevatedButton.styleFrom(
      backgroundColor: secondaryColor,
      foregroundColor: Colors.white,
      textStyle: const TextStyle(),
      shape: const RoundedRectangleBorder(
        borderRadius: BorderRadius.all(
          Radius.circular(0),
        ),
      ),
    ),
  ),
),
```

Terakhir, kita akan buat custom app bar.


Kita akan letakkan app bar tersebut pada halaman yang menggunakan web view agar tetap memunculkan kesan di dalam aplikasi .

Meskipun sedang membuka web browser.

Buat folder baru untuk menampung custom widget yang kita buat. Klik kanan pada folder lib -> New -> Package. 

Berikan nama folder widgets. Di dalam folder tersebut buat berkas baru custom_scaffold.dart. 

Di sini buatlah widget baru. 

Widget ini akan berperan menggantikan Scaffold dengan App Bar bawaan yang akan kita rancang. Sehingga, widget ini membutuhkan parameter berupa widget lain yang akan menjadi body-nya.

```
class CustomScaffold extends StatelessWidget {
  final Widget body;
 
  const CustomScaffold({Key? key, required this.body}) : super(key: key);
 
  @override
  Widget build(BuildContext context) {
    return Scaffold();
  }
}
```

Untuk susunan layout-nya, kita akan buat tumpukan (stack) dengan nilai argumen body di tumpukan paling bawah.

```
class CustomScaffold extends StatelessWidget {
  final Widget body;
 
  const CustomScaffold({Key? key, required this.body}) : super(key: key);
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Stack(
          children: [
            body,
          ],
        ),
      ),
    );
  }
}
```

Pada kode di atas kita menggunakan widget SafeArea untuk memposisikan widget di dalamnya pada lokasi yang aman dari status bar atau notch dari device.

Kemudian di children kedua dari Stack atau di atas body, kita akan mulai susun custom app bar-nya. App bar ini menggunakan komponen material Card.

```
return Scaffold(
  body: SafeArea(
    child: Stack(
      children: [
        body,
        Card(
          child: Row(
            children: [
              IconButton(
                icon: Icon(Icons.arrow_back),
                onPressed: () {
                  Navigator.pop(context);
                },
              )
            ],
          ),
        ),
      ],
    ),
  ),
);
```

Widget Card memiliki margin bawaan yang memberikan jarak terhadap widget lain. Hilangkan margin ini karena kita tidak membutuhkannya dalam app bar.

```
Card(
  margin: const EdgeInsets.all(0),
  child: Row(...),
),
```

Selanjutnya di samping ikon back, tambahkan logo aplikasi atau teks yang merepresentasikan aplikasi.

```
child: Row(
  children: [
    IconButton(
      icon: const Icon(Icons.arrow_back),
      onPressed: () {
        Navigator.pop(context);
      },
    ),
    Padding(
      padding: const EdgeInsets.only(right: 16.0),
      child: Text(
        'N',
        style: Theme.of(context).textTheme.headline6,
      ),
    ),
  ],
),
```

Saat ini Card memiliki ukuran lebar sesuai ukuran layar. Ini disebabkan karena widget Row akan mengambil ukuran maksimal. Kita bisa mengaturnya untuk menggunakan ukuran minimum.

```
child: Row(
  mainAxisSize: MainAxisSize.min,
  children: [...],
),
```

Terakhir, bukalah kembali berkas custom_scaffold.dart. Saatnya kita ubah bentuk app bar agar sesuai contoh desain dari Fortnightly. Tambahkan parameter shapepada Card untuk mengatur bentuknya.

```
return Card(
  margin: const EdgeInsets.all(0),
  child: Row(...),
  shape: const BeveledRectangleBorder(
    borderRadius: BorderRadius.only(
      bottomRight: Radius.circular(16.0),
    ),
  ),
);
```
Atur sudut dari Card dengan parameter borderRadius.

Gunakan CustomScaffold pada halaman ArticleWebViewuntuk menggantikan Scaffold bawaan Flutter.

```
class ArticleWebView extends StatelessWidget {
  static const routeName = '/article_web';
 
  final String url;
 
  const ArticleWebView({Key? key, required this.url}) : super(key: key);
 
  @override
  Widget build(BuildContext context) {
    final controller = WebViewController()..loadRequest(Uri.parse(url));
    return CustomScaffold(
      body: WebViewWidget(
        controller: controller,
      ),
    );
  }
}
```

### Refactoring Time!

Di dalam pengembangan aplikasi refactoring adalah satu hal yang penting. 

Refactoring adalah proses mengubah sistem dari perangkat lunak tanpa mengubah fungsionalitas dari perangkat lunak tersebut.

Sederhananya, tujuan dari refactoring adalah untuk meningkatkan kualitas kode dengan memperbaiki rancangan atau struktur kode supaya lebih baik dan lebih mudah ke depannya.

Hal pertama yang akan kita refactor dari project News App adalah memisahkan kode berdasarkan berkas yang relevan. Pada Dart atau Flutter kita bisa menuliskan seluruh kode aplikasi di dalam satu berkas. Namun, semakin banyak baris kode yang ditulis tentunya semakin sulit untuk dibaca dan dikelola bukan? Oleh karena itu, kita akan memisahkan kode berdasarkan fungsionalitasnya.

Sebenarnya beberapa kode telah dipisahkan dengan baik, misalnya berkas styles.dart untuk menyimpan variabel terkait tema aplikasi dan yang terakhir adalah custom_scaffold.dart untuk menyimpan custom scaffold widget.

Sekarang buka berkas main.dart. Pada berkas tersebut masih terdapat kode untuk menampilkan daftar artikel. Mari buat berkas baru bernama list_page.dart dan pindahkan kode widget Anda ke file tersebut. Anda mungkin perlu meng-import beberapa package jika kode Anda mengalami eror.

```
import 'package:dicoding_news_app/article.dart';
import 'package:dicoding_news_app/detail_page.dart';
import 'package:flutter/material.dart';
```

Selanjutnya kita akan me-refactor kode di dalam CustomScaffold. Saat ini widget CustomScaffold menampilkan tumpukan dari body widget dan widget Card yang telah diatur sedemikian rupa. Misalkan, Anda baru saja berlibur selama beberapa minggu lalu kembali lagi untuk melanjutkan project News App, apakah Anda masih ingat untuk apa widget Card tersebut dibuat? Atau, ada rekan kerja yang ikut berkolaborasi dalam project ini namun juga bingung apa tujuan widget Card tersebut dibuat. Penamaan yang jelas tentunya akan sangat membantu. Anda bisa mengeluarkan widget tersebut dan menjadikannya sebuah fungsi atau membuatnya menjadi widget atau fungsi sendiri yang memiliki nama.

Karena custom app bar kita tidak membutuhkan banyak parameter dan hanya digunakan di satu tempat, maka saat ini lebih baik kita memindahkannya menjadi fungsi tersendiri. Dengan Visual Studio Code Anda dapat melakukan hal tersebut dengan mengarahkan kursor ke widget Card lalu klik Ctrl + . kemudian Extract Method. Setelah itu berikan nama widget sesuai yang Anda inginkan. Untuk IDE Android Studio pilih menu Flutter Outline (biasanya ada di sebelah kanan IDE) lalu klik kanan pada widget Card dan pilih Extract Method.

Anda akan diminta mengisi nama widget baru tersebut.

Widget baru akan terbuat dan sekarang kode Anda menjadi lebih mudah untuk dibaca.

```
class CustomScaffold extends StatelessWidget {
  final Widget body;
 
  CustomScaffold({required this.body});
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Stack(
          children: [
            body,
            _buildShortAppBar(context),
          ],
        ),
      ),
    );
  }
}
```

Selain memberikan nama agar mudah dibaca, meng-extract widget juga sangat berguna ketika Anda perlu menampilkan widget tersebut di beberapa tempat yang berbeda.

Selamat! Anda telah berhasil me-refactor kode Anda pertama kali. Sekarang kode aplikasi News App Anda sudah lebih baik. 


### Refactoring

Refactoring adalah proses mengubah sistem dari perangkat lunak tanpa mengubah fungsionalitas dari perangkat lunak tersebut. 

Sederhananya, tujuan dari refactoring adalah untuk meningkatkan kualitas kode dengan memperbaiki rancangan atau struktur kode supaya lebih baik dan lebih mudah ke depannya.




---
**Referensi:**
- https://api.flutter.dev/flutter/painting/BorderRadius-class.html

- https://material.io/design/material-studies/fortnightly.html

- https://api.flutter.dev/flutter/painting/BoxDecoration/shape.html

- https://pub.dev/packages/webview_flutter

- https://api.flutter.dev/flutter/material/TextTheme/headline6.html