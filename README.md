# Belajar News App

Belajar membuat aplikasi portal berita webview:

1. Mengambil data dari berkas JSON lalu ditampilkan sebagai list.
2. Mengimplementasikan named routes untuk navigasi halaman.
3. Menambahkan package webview untuk menampilkan konten web.

---

Jalanin Project nya :
- flutter run -d chrome --web-renderer html
- flutter run -d chrome --web-renderer canvaskit

## Part 1

Jika kode di jalankan menggunakan **fitur null safety dart 2.12,** berarti anda harus install versi terbaru nya, lalu edit versi minimal sdk di bagian **pubspec.yaml** nya,menjadi :

```
environment:
  sdk: '>=2.12.0 <3.0.0'
```
atau bisa juga dengan **perintah dart migrate**
- https://dart.dev/null-safety/migration-guide


Disini kita akan coba pakai data local dari JSON nya:

```
assets/articles.json
```

Lalu kita tambahkan ke pubspec.yaml nya :

```
...
 
# To add assets to your application, add an assets section, like this:
assets:
  - assets/articles.json
```

## Part 2

Untuk membaca berkas json yang tadi, kita harus membuat **Class** yang sesuai untuk menjadi **Blueprint** dari objek artikel.

Buat berkas baru bernama **article.dart**. Kemudian tambahkan beberapa propertinya. Sesuaikan properti ini dengan berkas json Anda.


**Selanjutnya masih di dalam kelas Article tambahkan juga named constructor untuk mengonversi format json menjadi bentuk object Article.**

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

Lalu kita akan rapihkan di main.dart, kita membuat class NewsListPage().

Dimana nanti untuk judul bisa di scaffold dan appbar nya
dan daftar artikel nya bisa pakai ListView.

Tetapi karena kita harus baca aset json nya dulu dan juga dia default nya berjalan asynchronous

Maka, kita akan memanfaatkan widget **FutureBuilder**.

Contoh :

```
class NewsListPage extends StatelessWidget {
  static const routeName = '/article_list';
 
  const NewsListPage({Key? key}) : super(key: key);
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('News App'),
      ),
      body: FutureBuilder<String>(
        future:
            DefaultAssetBundle.of(context).loadString('assets/articles.json'),
        builder: (context, snapshot) {},
      ),
    );
  }
}
```

DefaultAssetBundle pada dasarnya juga merupakan sebuah widget. 

Widget ini akan membaca String dari berkas aset yang kita tentukan.

Karena Future kita mengembalikan String, maka kita perlu mengonversinya menjadi objek yang kita siapkan. 

Proses konversi ini juga dikenal dengan json parsing. Mari buat fungsi baru di dalam berkas article.dart.


```
List<Article> parseArticles(String? json) {
  if (json == null) {
    return [];
  }
 
  final List parsed = jsonDecode(json);
  return parsed.map((json) => Article.fromJson(json)).toList();
}
```

Lalu, panggil fungsi parseArticles() tersebut dari dalam fungsi builder

```
builder: (context, snapshot) {
  final List<Article> articles = parseArticles(snapshot.data);
},
```

Tampilkan widget ListViewAnda dengan item dari list Article.

```
builder: (context, snapshot) {
  final List<Article> articles = parseArticles(snapshot.data);
  return ListView.builder(
    itemCount: articles.length,
    itemBuilder: (context, index) {
      return _buildArticleItem(context, articles[index]);
    },
  );
},
```

buatlah method baru bernama _buildArticleItem(). Method ini akan menampilkan widget dari masing-masing artikel.

taruh di main.dart di bawag class MyApp.

```
Widget _buildArticleItem(BuildContext context, Article article) {
  return ListTile(
    contentPadding:
        const EdgeInsets.symmetric(horizontal: 16.0, vertical: 8.0),
    leading: Image.network(
      article.urlToImage,
      width: 100,
      errorBuilder: (ctx, error, _) => const Center(child: Icon(Icons.error)),
    ),
    title: Text(article.title),
    subtitle: Text(article.author),
  );
}
```

Di sini kita menggunakan widget ListTile. Widget ini merupakan widget built-in dari Flutter yang pada dasarnya merupakan susunan tiga kolom dan bisa diatur sedemikian rupa.

## Part 3

Fitur terakhir yang akan kita kembangkan selanjutnya adalah menampilkan detail dari artikel yang dipilih. Buat berkas baru bernama **detail_page.dart**

Halaman ArticleDetailPage membutuhkan data artikel yang dipilih sehingga perlu kita kirimkan melalui constructor.

```
class ArticleDetailPage extends StatelessWidget {
  static const routeName = '/article_detail';
 
  final Article article;
  const ArticleDetailPage({Key? key, required this.article}) : super(key: key);
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(article.title),
      ),
      body: SingleChildScrollView(
        child: Column(
          children: [
            Image.network(article.urlToImage),
            Padding(
              padding: const EdgeInsets.all(10),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(article.description),
                  Divider(color: Colors.grey),
                  Text(
                    article.title,
                    style: const TextStyle(
                      color: Colors.black,
                      fontWeight: FontWeight.bold,
                      fontSize: 24,
                    ),
                  ),
                  const Divider(color: Colors.grey),
                  Text('Date: ${article.publishedAt}'),
                  const SizedBox(height: 10),
                  Text('Author: ${article.author}'),
                  const Divider(color: Colors.grey),
                  Text(
                    article.content,
                    style: const TextStyle(fontSize: 16),
                  ),
                  const SizedBox(height: 10),
                  ElevatedButton(
                    child: const Text('Read more'),
                    onPressed: () {},
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Halaman ArticleDetailPage membutuhkan data artikel yang dipilih sehingga perlu kita kirimkan melalui constructor.

Jangan lupa untuk menambahkan route ke halaman ArticleDetailPage.

```
routes: {
  NewsListPage.routeName: (context) => const NewsListPage(),
  ArticleDetailPage.routeName: (context) => ArticleDetailPage(
        article: ModalRoute.of(context)?.settings.arguments as Article,
      ),
},
```



Selanjutnya buat item yang diklik agar membuka halaman detail. ListTile telah dilengkapi dengan parameter onTapuntuk menjalankan sebuah fungsi ketika item tersebut diklik.


```
onTap: () {
  Navigator.pushNamed(context, ArticleDetailPage.routeName,
      arguments: article);
},
```

Masukkan data artikel yang dipilih sebagai argumen navigasi.

Terakhir, kita akan menambahkan fitur agar pengguna dapat membaca berita lebih lengkap dengan mengarahkan ke tautan aslinya. Karena artikel kita berbasis web, maka kita harus pakai **package WebView**.

```
flutter pub add webview_flutter
```

disini yang saya pakai **webview_flutter: ^4.2.1** bisa cek di pubspec.yaml

jangan lupa untuk run :

```
flutter pub get
```

sebelum menggunakan webview nya

baru abis itu kita bisa :

```
import 'package:webview_flutter/webview_flutter.dart';
```
di halaman **detail_page.dart** nya

Setelah package terinstal, buatlah widget baru yang akan menampilkan detail artikel. 
WebViewWidget adalah sebuah widget. 

Karena setiap komponen dalam Flutter adalah Widget, maka kita juga dapat melakukan kustomisasi pada halaman web dengan menambahkan widget lain seperti AppBar, FloatingActionButton, dan lainnya.

```
class ArticleWebView extends StatelessWidget {
  static const routeName = '/article_web';
 
  final String url;
 
  const ArticleWebView({Key? key, required this.url}) : super(key: key);
 
  @override
  Widget build(BuildContext context) {
    final controller = WebViewController()..loadRequest(Uri.parse(url));
    return Scaffold(
      appBar: AppBar(
        title: Text('News App'),
      ),
      body: WebViewWidget(
        controller: controller,
      ),
    );
  }
}
```

Perhatikan bahwa **WebViewController** membutuhkan url dari web yang akan ditampilkan, sehingga Anda perlu mengirimkan nilai url ke ArticleWebView.

```
ElevatedButton(
  child: const Text('Read more'),
  onPressed: () {
    Navigator.pushNamed(context, ArticleWebView.routeName,
        arguments: article.url);
  },
),
```

Jangan lupa untuk mendaftarkan routes untuk navigasi.

```
routes: {
  NewsListPage.routeName: (context) => const NewsListPage(),
  ArticleDetailPage.routeName: (context) => ArticleDetailPage(
        article: ModalRoute.of(context)?.settings.arguments as Article,
      ),
  ArticleWebView.routeName: (context) => ArticleWebView(
        url: ModalRoute.of(context)?.settings.arguments as String,
      ),
},
```

Done.

---
**Referensi :**
- https://api.flutter.dev/flutter/widgets/ListView-class.html
- https://api.flutter.dev/flutter/material/ListTile-class.html
- https://api.flutter.dev/flutter/material/Divider-class.html
- https://docs.flutter.dev/data-and-backend/json
- https://pub.dev/packages/webview_flutter

```
Ihsanunot | Dart 3.0 | Flutter 3.0 | JDK 20
```