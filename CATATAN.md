# Catatan

Hal pertama yang akan kita bahas adalah widget FutureBuilder. Widget ini akan membangun widget berdasarkan interaksi terbaru dengan objek Future.

Seperti yang kita tahu bahwa Dart mendukung proses asynchronous menggunakan Future. 

Proses asynchronous dapat mengembalikan nilai ketika prosesnya selesai dijalankan.

```
FutureBuilder<String>(
 future:
    DefaultAssetBundle.of(context).loadString('assets/articles.json'),
 builder: (context, snapshot) {
   final List<Article> articles = parseArticles(snapshot.data);
   return ListView.builder(
     itemCount: articles.length,
     itemBuilder: (context, index) {
       return _buildArticleItem(context, articles[index]);
     },
   );
 },
),
```

Dalam kasus ini proses asynchronous yang dijalankan adalah membaca String dari aset articles.json. Setelah nilai Future telah didapatkan, selanjutnya parameter builder akan menjalankan fungsinya.

---

Parameter builder membutuhkan sebuah fungsi dengan dua argumen, yaitu BuildContext dan AsyncSnapshot

Context akan mendefinisikan lokasi widget di dalam widget tree dan snapshot berisi data yang didapat dari Future. 

Data dari snapshot di sini berupa String karena kita mendefinisikannya secara eksplisit menggunakan generic type FutureBuilder<String>.

---

Tahap selanjutnya adalah JSON parsing. Tahap ini sederhananya adalah mengubah nilai json yang masih dalam bentuk String menjadi bentuk kompleks yang dimengerti oleh bahasa pemrograman.

Berkas json yang kita gunakan merupakan sebuah JSON array sehingga kita mengonversinya menjadi bentuk List.

```
final List parsed = jsonDecode(json);
```

Untuk konversinya kita menggunakan fungsi jsonDecode yang merupakan fungsi bawaan dari Dart dengan melakukan import library dart:convert.

```
import 'dart:convert';
```

Nah, JSON (JavaScript Object Notation) sendiri adalah sebuah penyimpanan data dengan format key-value. 

---

Pada Dart format key-value disimpan dengan tipe data Map, karena itulah kita mengubah lagi format Map menjadi object yang telah kita buat.

```
factory Article.fromJson(Map<String, dynamic> article) => Article(
      author: article['author'],
      title: article['title'],
      description: article['description'],
      url: article['url'],
      urlToImage: article['urlToImage'],
      publishedAt: article['publishedAt'],
      content: article['content'],
    );
```

Key yang berada di dalam kurung siku harus sama persis dengan key yang ada pada file json.

```
author = article['author']; // Key JSON
```


Hal terakhir yang kita bahas adalah widget WebView. Widget ini merupakan widget tambahan di luar framework Flutter. Anda telah berhasil menambahkannya dengan menginstal package-nya melalui website pub.dev dan file pubspec.yaml.


```
WebView(
  initialUrl: url,
),
```


Widget ini berfungsi untuk menampilkan halaman web Anda ke tampilan mobile. Secara default fitur JavaScript pada web Anda akan dinonaktifkan. Untuk mengaktifkan JavaScript, Anda dapat menambahkan parameter javascriptMode seperti berikut.

```
javascriptMode: JavascriptMode.unrestricted,
```

Done

**Ihsanunot** Flutter 3.0 | Dart 3.0.0

















