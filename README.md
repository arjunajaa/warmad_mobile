# Tugas 7

## Perbedaan utama antara stateless dan stateful widget dalam konteks pengembangan aplikasi Flutter
- Stateless Widget :
    - Tidak dapat berubah setelah dibuat (widget yang tidak perlu update data)
    - Ketika data yang dimiliki widget berubah, kita perlu membuat widget baru karena stateless widget tidak memiliki state yang dapat kita ubah
    - Tampilan statis dan komponen yang hanya bergantung pada input yang diberikan saat widget dibuat (contohnya teks, tombol, gambar)

- Stateful Widget :
    - Memiliki state yang dapat dirubah selama aplikasi berjalan
    - Ketika data yang dimiliki widget berubah, kita dapat merubah/memperbaruinya melalui state yang dimiliki oleh widget
    - Memiliki 2 class terkait yaitu `StatefulWidget` yang merupakan widget itu sendiri dan `State` yang menyimpan data yang nantinya juga bisa diubah
    - Contohnya form dan scrollable untuk menampilkan konten

## Widget yang digunakan dalam tugas dan fungsinya masing-masing
- `MyHomepage (StatelessWidget)` sebagai widget utama untuk mengantur layout dan struktur tampilan home page
- `Scaffold` memberikan struktur basic dari aplikasi, termasuk app bar dan body dari page
- `AppBar` untuk app bar pada bagian atas aplikasi dengan text dan background color
- `SingleChildScrollView` agar content dapat discroll jika melebihi ukuran layar
- `Padding` untuk apply padding
- `Collumn` mengatur konten secara vertikal
- `Text` menampilkan tulisan pada aplikasi
- `GridView.count` membuat layout grid dengan jumlah collumn tertentu, pada aplikasi ini sebanyak 3 karena terdapat 3 card
- `ShopCard (StatelessWidget)` sebagai widget untuk tiap card pada grid, memiliki text, icon, dan warnanya sendiri (bonus)
- `Material` mengatur tampilan dan action dari card
- `Inkwell` untuk membuat card responsive terhadap action sentuhan dan mengatur responnya
- `Container` untuk menyimpan dan mengatur posisi widget `Text` dan `Icon` pada card

## Implementasi checklist
1. Membuat program Flutter baru dengan tema inventory
- Inisiasi awal :
    - Menjalankan perintah `flutter create warmad_mobile` pada direktori yang ingin digunakan sebagai folder project/program
    - Masuk ke root folder program dan jalankan `git init` untuk membuatnya sebagai folder git
    - Menyambungkan folder dengan repository dengan perintah `git remode add origin`

- Mengatur struktur project :
    - Menambahkan file `menu.dart` pada folder `lib`
    - Memindahkan class `MyHomePage` dan `_MyHomePageState` dari `main.dart` ke `menu.dart`
    - Import `menu.dart` ke `main.dart` untuk mengimport `MyHomePage` yang sudah dipindah
        ```dart
        import 'package:flutter/material.dart';
        ```
    - Mengubah `home` pada `main.dart` agar dapat menampilkan widget dengan tepat saat memulai aplikasi
        ```dart
        ...
        home: MyHomePage(),
        ...
        ```

- Mengubah widget menu page menjadi stateless :
    - Menghapus state `MyHomePage` dari `menu.dart` yaitu `_MyHomePageState`
    - Merubah widget `MyHomePage` menjadi stateless dengan merubah kode menjadi berikut
        ```dart
        class MyHomePage extends StatelessWidget {
            MyHomePage({Key? key}) : super(key: key);

            // This widget is the home page of your application. It is stateful, meaning
            // that it has a State object (defined below) that contains fields that affect
            // how it looks.

            // This class is the configuration for the state. It holds the values (in this
            // case the title) provided by the parent (in this case the App widget) and
            // used by the build method of the State. Fields in a Widget subclass are
            // always marked "final".


            @override
            Widget build(BuildContext context) {
                return Scaffold(
                    .....
                )
            }
        }
        ```

2. Membuat tiga tombol sederhana dengan ikon dan teks
- Inisiasi dan pembuatan class baru :
    - Membuat class `ShopItem` untuk button pada homepage dengan attribute nama, icon, dan warna (bonus)
        ```dart
        class ShopItem {
          final String name;
          final IconData icon;
        
          ShopItem(this.name, this.icon);
        }
        ```
    - Membuat class `ShopCard` untuk mengatur tampilan dan action dari button
        ```dart
        class ShopCard extends StatelessWidget {
          final ShopItem item;
        
          const ShopCard(this.item, {super.key}); // Constructor
        
          @override
          Widget build(BuildContext context) {
            return Material(
              color: Colors.indigo,
              child: InkWell(
                // Area responsive terhadap sentuhan
                onTap: () {
                  // Memunculkan SnackBar ketika diklik
                  ScaffoldMessenger.of(context)
                    ..hideCurrentSnackBar()
                    ..showSnackBar(SnackBar(
                        content: Text("Kamu telah menekan tombol ${item.name}!")));
                },
                child: Container(
                  // Container untuk menyimpan Icon dan Text
                  padding: const EdgeInsets.all(8),
                  child: Center(
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Icon(
                          item.icon,
                          color: Colors.white,
                          size: 30.0,
                        ),
                        const Padding(padding: EdgeInsets.all(3)),
                        Text(
                          item.name,
                          textAlign: TextAlign.center,
                          style: const TextStyle(color: Colors.white),
                        ),
                      ],
                    ),
                  ),
                ),
              ),
            );
          }
        }
        ```
- Menampilkan ketiga tombol pada home page
    - Membuat list `items` berisi `ShopItem` pada class `MyHomePage`
        ```dart
        final List<ShopItem> items = [
          ShopItem("Lihat Item", Icons.checklist),
          ShopItem("Tambah Item", Icons.add_shopping_cart),
          ShopItem("Logout", Icons.logout),
        ];
        ```
3. Memunculkan Snackbar untuk action tombol
- Telah dilakukan dengan menambahkan `onTap()` di `InkWell` pada `ButtonCard`
    ```dart
    child: InkWell(
                // Area responsive terhadap sentuhan
                onTap: () {
                // Memunculkan SnackBar ketika diklik
                ScaffoldMessenger.of(context)
                    ..hideCurrentSnackBar()
                    ..showSnackBar(SnackBar(
                        content: Text("Kamu telah menekan tombol ${item.name}!")));
                },
    ```
<br><br>

# Tugas 8

## `Navigator.push()` dan `Navigator.pushReplacement()`
`Navigator.push()` dan `Navigator.pushReplacement()` adalah dua metode yang berbeda dalam Flutter untuk menavigasi antara halaman. Berikut adalah perbedaan antara keduanya:

- `Navigator.push()` digunakan untuk menambahkan halaman baru ke dalam tumpukan halaman yang ada. Ketika pengguna menekan tombol kembali, halaman terakhir akan dihapus dari tumpukan dan pengguna akan kembali ke halaman sebelumnya. Contoh penggunaan: 

```dart
Navigator.push(context, MaterialPageRoute(builder: (context) => HalamanBaru()));
```

- `Navigator.pushReplacement()` digunakan untuk mengganti halaman teratas dalam tumpukan halaman dengan halaman baru. Halaman sebelumnya akan dihapus dari tumpukan dan tidak dapat diakses lagi. Contoh penggunaan:

```dart
Navigator.pushReplacement(context, MaterialPageRoute(builder: (context) => HalamanBaru()));
```

Kedua metode ini dapat digunakan dalam berbagai skenario, tergantung pada kebutuhan aplikasi. Sebagai contoh, `Navigator.push()` dapat digunakan ketika pengguna ingin menavigasi ke halaman baru dan masih ingin dapat kembali ke halaman sebelumnya. Sedangkan `Navigator.pushReplacement()` dapat digunakan ketika pengguna menyelesaikan tugas tertentu dan tidak perlu kembali ke halaman sebelumnya.

Contoh penggunaan yang tepat dari kedua metode ini tergantung pada kebutuhan aplikasi. Sebagai contoh, `Navigator.push()` dapat digunakan ketika pengguna ingin menavigasi ke halaman detail dari daftar item, dan masih ingin dapat kembali ke halaman daftar item. Sedangkan `Navigator.pushReplacement()` dapat digunakan ketika pengguna menyelesaikan tugas seperti login dan tidak perlu kembali ke halaman login lagi. 
## *Layout* widget pada Flutter 

1. **Single-child layout widgets**
   - `Container`: Widget yang dapat digunakan untuk mengatur tampilan widget lainnya seperti padding, margin, dan background color.
   - `Center`: Widget yang digunakan untuk menempatkan widget lainnya di tengah-tengah layar.
   - `Align`: Widget yang digunakan untuk menempatkan widget lainnya pada posisi yang ditentukan.
   - `FractionallySizedBox`: Widget yang digunakan untuk menentukan ukuran widget anak sebagai fraksi dari ruang yang tersedia.
   - `AspectRatio`: Widget yang digunakan untuk menentukan rasio aspek widget anak.

2. **Multi-child layout widgets**
   - `Row` dan `Column`: Widget yang digunakan untuk menempatkan widget anak secara horizontal atau vertikal.
   - `Stack`: Widget yang digunakan untuk menumpuk widget anak di atas satu sama lain.
   - `IndexedStack`: Widget yang digunakan untuk menumpuk widget anak di atas satu sama lain, tetapi hanya menampilkan satu widget pada satu waktu.
   - `Flow`: Widget yang digunakan untuk menempatkan widget anak dalam bentuk aliran.
   - `Wrap`: Widget yang digunakan untuk menempatkan widget anak dalam bentuk wrapping.

3. **Sliver widgets**
   - `SliverAppBar`: Widget yang digunakan untuk menampilkan app bar yang dapat di-scroll.
   - `SliverList` dan `SliverGrid`: Widget yang digunakan untuk menampilkan daftar atau grid yang dapat di-scroll.
   - `SliverToBoxAdapter`: Widget yang digunakan untuk menempatkan widget lainnya di dalam `CustomScrollView` .

Konteks penggunaan masing-masing layout widget pada Flutter dapat bervariasi tergantung pada kebutuhan aplikasi yang dibangun. Sebagai contoh, `Row` dan `Column` dapat digunakan untuk menampilkan widget child secara horizontal atau vertikal, sedangkan `Stack` dapat digunakan untuk menumpuk widget child di atas satu sama lain. Selain itu, `SliverList` dan `SliverGrid` dapat digunakan untuk menampilkan daftar atau grid yang dapat di-scroll.

## Elemen input pada form
Pada tugas ini elemen input form yang saya gunakan hanya `TextFormField` dengan validasi input. Saya menggunakan `TextFormField` karena memungkinkan user untuk mengisi semua data sesuai keinginan dia sendiri. Saya juga menambahkan `inputFormatters` pada bagian input integer agar user hanya bisa memasukkan angka saja.
## *Clean architecture* pada aplikasi Flutter
Clean Architecture adalah sebuah konsep arsitektur perangkat lunak yang bertujuan untuk memisahkan kode untuk business-logic dengan kode yang berhubungan dengan platform seperti UI, state management, dan sumber data eksternal. Dalam konteks Flutter, clean architecture akan membantu kita untuk memisahkan kode untuk business-logic dengan kode yang berhubungan dengan platform seperti UI, state management, dan sumber data eksternal. Selain itu, kode yang kita tulis pun dapat lebih mudah untuk diuji (testable) secara independen.

Clean Architecture pada Flutter membagi project ke dalam 3 lapisan (layer) yaitu:
- Lapisan data & platform: Lapisan data terletak pada lapisan paling luar. Lapisan ini terdiri dari kode sumber data seperti Rest API, local database, Firebase, atau sumber lainnya. Juga kode platform yang membangun tampilan aplikasi (widgets).
- Lapisan presentation: Lapisan presentation terletak di tengah-tengah. Lapisan ini terdiri dari kode yang mengatur tampilan aplikasi (widgets) dan state management.
- Lapisan domain: Lapisan domain terletak pada lapisan paling dalam. Lapisan ini terdiri dari kode yang berisi business-logic aplikasi.
## Cara Implementasi Checklist
1. Membuat halaman baru formulir untuk menambah item
- Membuat file baru pada folder `lib` bernama `shoplist_form.dart`
- Memakai tiga elemen input :
    - `TextFormField` untuk input field `name` dengan validasi agar tidak kosong :
        ```dart
        import 'package:flutter/material.dart';
        import 'package:warmad_mobile/widgets/left_drawer.dart';

        class ShopFormPage extends StatefulWidget {
          const ShopFormPage({super.key});

          @override
          State<ShopFormPage> createState() => _ShopFormPageState();
        }

        class _ShopFormPageState extends State<ShopFormPage> {
          final _formKey = GlobalKey<FormState>();
          String _name = "";
          int _price = 0;
          String _description = "";
          @override
          Widget build(BuildContext context) {
            return Scaffold(
              appBar: AppBar(
                title: const Center(
                  child: Text(
                    'Form Tambah Produk',
                  ),
                ),
                backgroundColor: Colors.indigo,
                foregroundColor: Colors.white,
              ),
              drawer: const LeftDrawer(),
              body: Form(
                key: _formKey,
                child: SingleChildScrollView(
                    child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: TextFormField(
                        decoration: InputDecoration(
                          hintText: "Nama Produk",
                          labelText: "Nama Produk",
                          border: OutlineInputBorder(
                            borderRadius: BorderRadius.circular(5.0),
                          ),
                        ),
                        onChanged: (String? value) {
                          setState(() {
                            _name = value!;
                          });
                        },
                        validator: (String? value) {
                          if (value == null || value.isEmpty) {
                            return "Nama tidak boleh kosong!";
                          }
                          return null;
                        },
                      ),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: TextFormField(
                        decoration: InputDecoration(
                          hintText: "Harga",
                          labelText: "Harga",
                          border: OutlineInputBorder(
                            borderRadius: BorderRadius.circular(5.0),
                          ),
                        ),
                        onChanged: (String? value) {
                          setState(() {
                            _price = int.parse(value!);
                          });
                        },
                        validator: (String? value) {
                          if (value == null || value.isEmpty) {
                            return "Harga tidak boleh kosong!";
                          }
                          if (int.tryParse(value) == null) {
                            return "Harga harus berupa angka!";
                          }
                          return null;
                        },
                      ),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: TextFormField(
                        decoration: InputDecoration(
                          hintText: "Deskripsi",
                          labelText: "Deskripsi",
                          border: OutlineInputBorder(
                            borderRadius: BorderRadius.circular(5.0),
                          ),
                        ),
                        onChanged: (String? value) {
                          setState(() {
                            _description = value!;
                          });
                        },
                        validator: (String? value) {
                          if (value == null || value.isEmpty) {
                            return "Deskripsi tidak boleh kosong!";
                          }
                          return null;
                        },
                      ),
                    ),
                    Align(
                      alignment: Alignment.bottomCenter,
                      child: Padding(
                        padding: const EdgeInsets.all(8.0),
                        child: ElevatedButton(
                          style: ButtonStyle(
                            backgroundColor: MaterialStateProperty.all(Colors.indigo),
                          ),
                          onPressed: () {
                            if (_formKey.currentState!.validate()) {
                              showDialog(
                                context: context,
                                builder: (context) {
                                  return AlertDialog(
                                    title: const Text('Produk berhasil tersimpan'),
                                    content: SingleChildScrollView(
                                      child: Column(
                                        crossAxisAlignment: CrossAxisAlignment.start,
                                        children: [
                                          Text('Nama: $_name'),
                                          Text('Harga: $_price'),
                                          Text('Deskripsi: $_description'),
                                        ],
                                      ),
                                    ),
                                    actions: [
                                      TextButton(
                                        child: const Text('OK'),
                                        onPressed: () {
                                          Navigator.pop(context);
                                        },
                                      ),
                                    ],
                                  );
                                },
                              );
                            }
                            _formKey.currentState!.reset();
                          },
                          child: const Text(
                            "Save",
                            style: TextStyle(color: Colors.white),
                          ),
                        ),
                      ),
                    ),
                  ],
                )),
              ),
            );
          }
        }
        ```

- Memiliki tombol save yang akan menyimpan item ke list (bonus) dan menampilkan popup detail item :
    ```dart
    ....

      child: ElevatedButton(
                  style: ButtonStyle(
                    backgroundColor: MaterialStateProperty.all(Colors.blueGrey),
                  ),
                  onPressed: () {
                    if (_formKey.currentState!.validate()) {
                      showDialog(
                        context: context,
                        builder: (context) {
                          return AlertDialog(
                            title: const Text('Item berhasil tersimpan'),
                            content: SingleChildScrollView(
                              child: Column(
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                  Text('Nama: $_name'),
                                  Text('Jumlah: $_amount'),
                                  Text('Deskripsi: $_description'),
                                ],
                              ),
                            ),
                            actions: [
                              TextButton(
                                child: const Text('OK'),
                                onPressed: () {
                                  Navigator.pop(context);
                                  _formKey.currentState!.reset();
                                  Item.itemList.add(Item(_name, _amount, _description));
                                },
                              ),
                            ],
                          );
                        },
                      );
                    }
                    _formKey.currentState!.reset();
                  },
                  child: const Text(
                    "Save",
                    style: TextStyle(color: Colors.white),
                  ),
                ),

    ....
    ```
2. Mengarahkan pengguna ke halaman form tambah item baru ketika menekan tombol `Tambah Item` pada halaman utama :
- Menambahkan kode berikut pada bagian `onTap()` milik class `ButtonCard` :
    ```dart
    if (item.name == "Tambah Item"){
        Navigator.push(context,
            MaterialPageRoute(builder: (context) => const ShopFormPage()));}
    ```

3. Memunculkan data sesuai isi dari formulir yang diisi dalam sebuah pop-up setelah menekan tombol Save pada halaman formulir tambah item baru.
    ```dart
    ...

    onPressed: () {
                    if (_formKey.currentState!.validate()) {
                      showDialog(
                        context: context,
                        builder: (context) {
                          return AlertDialog(
                            title: const Text('Item berhasil tersimpan'),
                            content: SingleChildScrollView(
                              child: Column(
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                  Text('Nama: $_name'),
                                  Text('Jumlah: $_amount'),
                                  Text('Deskripsi: $_description'),
                                ],
                              ),
                            ),

    ...

    ```

4. Membuat drawer pada aplikasi
- Membuat file `left_drawer.dart` pada direktori baru `lib/widgets` :
    - Drawer memiliki tiga buah opsi yaitu `Main Page`, `Tambah Item`, dan `Daftar Item`
        ```dart
        ...

          ListTile(
            leading: const Icon(Icons.home_outlined),
            title: const Text('Main Page'),
            // Bagian redirection ke MyHomePage
            onTap: () {
              Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(
                    builder: (context) => MyHomePage(),
                  ));
            },
          ),
          ListTile(
            leading: const Icon(Icons.list_sharp),
            title: const Text('Daftar Item'),
            // Bagian redirection ke ShopFormPage
            onTap: () {
              Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(
                    builder: (context) => ShowItems(itemList: Item.itemList),
                  )
              );
            },
          ),
          ListTile(
            leading: const Icon(Icons.add_task),
            title: const Text('Add Item'),
            // Bagian redirection ke ShopFormPage
            onTap: () {
              Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const ShopFormPage(),
                  )
              );
            },
          ),
        
        ...
        ```
    - Pada tiap bagian, jika diklik maka user akan diarahkan ke page sesuai bagian tersebut
 # Tugas 9: Integrasi Layanan Web Django dengan Aplikasi Flutter

## Pengambilan Data JSON Tanpa Model
**Apakah bisa kita melakukan pengambilan data JSON tanpa membuat model terlebih dahulu?**
Ya bisa, kita dapat mengakalinya dengan menggunakan sebuah variabel yang menyimpan sebuah *dictionary* berisi data.

**Apakah hal tersebut lebih baik daripada membuat model sebelum melakukan pengambilan data JSON?**
Tidak, justru model mempermudah kita dalam melakukan pengambilan data karena dengan model kita dapat memastikan suatu objek memiliki semua nilai atribut pada kelas dibanding dengan *dictionary* yang bisa saja kita melewatkan suatu atribut.

## `CookieRequest`
Salah satu *class* pada *package* `pbp_django_auth.dart`.
### Fungsi
Dilihat dari isi *class*-nya, menurut saya, fungsi dari *class* `CookieRequest` ini yaitu:
* Menyediakan fungsi untuk inisialisasi sesi, login, dan logout yang memungkinkan aplikasi untuk melacak status login dan sesi pengguna.
* Cookies berupa informasi sesi tersebut disimpan secara lokal.
* Melakukan permintaan HTTP dengan metode GET dan POST.
### `CookieRequest` pada Semua Komponen Aplikasi
`CookieRequest` perlu dibagikan ke semua komponen di aplikasi Flutter agar status login atau sesi (cookies) **konsisten**. Jadi, jika status login atau sesi diubah dalam suatu komponen atau aplikasi, maka di komponen atau aplikasi lain akan berubah juga.

## Mekanisme Pengambilan Data JSON
Berikut adalah mekanisme pengambilan data dari JSON.
1. **Membuat model kustom**
Manfaatkan website [Quicktype](http://app.quicktype.io/) untuk membuat data JSON yang didapat dari *endpoint* `/json` pada tugas Django.
2. **Menambahkan dependensi HTTP**
Pada proyek Flutter, tambahkan dependensi `http` dan tambahkan kode `<uses-permission android:name="android.permission.INTERNET" />` pada `android/app/src/main/AndroidManifest.xml` untuk memperbolehkan akses internet.
3. **Melakukan Fetch Data**
Pada salah satu *file* `lib/screens` yang ingin melakukan *fetch* data, implementasi fungsi asinkronus dan mengirim permintaan HTTP.
4. **Menampilkan Data**
Pada tugas ini, saya menampilkan data menggunakan widget `FutureBuilder` yang dimonitori nilai `future:`. Jadi, ketika fungsi `fetchProduct()` dipanggil dan sudah selesai melakukan proses, maka `snapshot` akan berisi `list_product` yang di-*return* pada fungsi tersebut. Setelah itu, `snapshot.data` ini akan diolah untuk ditampilkan pada `ListView.builder`.

## Autentikasi
Mekanisme autentikasi pengguna dijelaskan sebagai berikut.
1. Pengguna **memasukkan data akun** yaitu `username` dan `password` pada laman `LoginPage`.
2. Tombol *login* ditekan dan fungsi `login` pada `CookieRequest` terpanggil yang **mengirimkan HTTP *request*** dengan *endpoint* URL proyek Django.
3. Pada Django, **dilakukan autentikasi** seperti `user = authenticate(username=username, password=password)` pada `views.py` milik `authentication`.
4. Setelah itu dicek, **apakah `user is not None` dan `user.is_active:`**?
5. Kembali ke Flutter, jika `request.loggedIn`, **pengguna diarahkan ke `MyHomePage`** dan muncul tampilan selamat datang menggunakan `SnackBar`.
6. 
## Widget yang dipakai pada tugas ini dan fungsinya masing-masing.

- `Scaffold`: Struktur dasar halaman

- `Container` : Bisa mengatur padding, margin, background, ukuran, dll. Berfungsi untuk menyimpan elemen-elemen secara flexible

- `AppBar`: Menampilkan judul aplikasi dan mengatur warna latar belakang serta teks.

- `ElevatedButton`: Membuat tombol dengan tampilan yang lebih tinggi.

- `Form`: Membungkus elemen-elemen input untuk validasi dan manipulasi data.

- `TextFormField`: Mengambil input teks dari pengguna.

- `Text`: Menampilkan teks.

- `Drawer`: Menyediakan struktur untuk menu navigasi.

- `Column`: Menempatkan widgets atau item secara vertikal.

- `ListView`: Mengatur agar semua widget-widget dapat di-scroll.

- `ListTile`: Item dalam ListView

- `Navigator`: Menangani perpindahan halaman saat item menu dipilih.

- `MaterialPageRoute`: Mengatur transisi antar halaman.

- `Material`: Lapisan dasar untuk InkWell dengan efek elevasi dan respons sentuhan.

- `InkWell`: Area responsif terhadap sentuhan dengan efek visual.

- `Container`: Menyimpan ikon dan teks dengan memberikan jarak dan mengatur tata letak.

- `Card`: Mengelompokkan informasi item dalam tampilan kartu dengan bayangan dan sudut bulat.

- `CircularProgressIndicator`: Indikator loading sementara data produk diambil.

- `SingleChildScrollView`: Memberikan kemampuan scroll pada halaman.

- `GridView.count`: Tata letak grid dengan jumlah kolom yang dapat diatur.

- `Future`: Representasi nilai atau kesalahan yang mungkin terjadi di masa depan.

- `FutureBuilder`: Mengelola state Future untuk menangani data asynchronous.

## Implementasi Tugas
- Membuat `django-app` baru bernama `authentication` dan menambahkan app tersebut ke dalam `INSTALLED_APPS` pada settings.py di folder project utama
- Menginstall `django-cors-headers` dan menambahkannya juga ke `INSTALLED_APPS` pada settings.py di folder project utama
- `Menambahkan `'corsheaders.middleware.CorsMiddleware'` dan juga variabel-variabel berikut ke settings.py
```python
CORS_ALLOW_ALL_ORIGINS = True
CORS_ALLOW_CREDENTIALS = True
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SAMESITE = 'None'
SESSION_COOKIE_SAMESITE = 'None'
``` 
- Membuat `views.py` pada app authentication yang berisi function untuk login
```python
from django.shortcuts import render
from django.contrib.auth import authenticate, login as auth_login
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def login(request):
    username = request.POST['username']
    password = request.POST['password']
    user = authenticate(username=username, password=password)
    if user is not None:
        if user.is_active:
            auth_login(request, user)
            # Status login sukses.
            return JsonResponse({
                "username": user.username,
                "status": True,
                "message": "Login sukses!"
                # Tambahkan data lainnya jika ingin mengirim data ke Flutter.
            }, status=200)
        else:
            return JsonResponse({
                "status": False,
                "message": "Login gagal, akun dinonaktifkan."
            }, status=401)

    else:
        return JsonResponse({
            "status": False,
            "message": "Login gagal, periksa kembali email atau kata sandi."
        }, status=401)
```
- Membuat file `urls.py` untuk URL routing pada folder `authentication` dan menambahkan path dari `urls.py` pada authentication ke urls.py pada folder project utama

urls.py pada folder authentication
```python
from django.urls import path
from authentication.views import login

app_name = 'authentication'

urlpatterns = [
    path('login/', login, name='login'),
]
```

urls.py pada folder project utama
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('main.urls')),
    path('auth/', include('authentication.urls')),
    path('admin/', admin.site.urls)
]
```

- Pada aplikasi Flutter, instal package berikut :
```
flutter pub add provider
flutter pub add pbp_django_auth
```

- Memodifikasi root widget pada main.dart agar `CookieRequest` tersedia untuk semua child widgets menggunakan `Provider`.
```dart
class MyApp extends StatelessWidget {
    const MyApp({Key? key}) : super(key: key);

    @override
    Widget build(BuildContext context) {
        return Provider(
            create: (_) {
                CookieRequest request = CookieRequest();
                return request;
            },
            child: MaterialApp(
                title: 'Warung Madura',
                theme: ThemeData(
                    colorScheme: ColorScheme.fromSeed(seedColor: Colors.lightGreen),
                    useMaterial3: true,
                ),
                home: LoginPage(),
            ),
        );
    }
}
```

- Membuat file `login.dart` pada folder screen sebagai halaman untuk login
```dart
// ignore_for_file: use_build_context_synchronously, library_private_types_in_public_api

import 'package:warmad_mobile/screens/menu.dart';
import 'package:flutter/material.dart';
import 'package:pbp_django_auth/pbp_django_auth.dart';
import 'package:provider/provider.dart';

void main() {
    runApp(const LoginApp());
}

class LoginApp extends StatelessWidget {
const LoginApp({super.key});

@override
Widget build(BuildContext context) {
    return MaterialApp(
        title: 'Login',
        theme: ThemeData(
            primarySwatch: Colors.blue,
    ),
    home: const LoginPage(),
    );
    }
}

class LoginPage extends StatefulWidget {
    const LoginPage({super.key});

    @override
    _LoginPageState createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
    final TextEditingController _usernameController = TextEditingController();
    final TextEditingController _passwordController = TextEditingController();

    @override
    Widget build(BuildContext context) {
        final request = context.watch<CookieRequest>();
        return Scaffold(
            appBar: AppBar(
                title: const Text('Login'),
            ),
            body: Container(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                        TextField(
                            controller: _usernameController,
                            decoration: const InputDecoration(
                                labelText: 'Username',
                            ),
                        ),
                        const SizedBox(height: 12.0),
                        TextField(
                            controller: _passwordController,
                            decoration: const InputDecoration(
                                labelText: 'Password',
                            ),
                            obscureText: true,
                        ),
                        const SizedBox(height: 24.0),
                        ElevatedButton(
                            onPressed: () async {
                                String username = _usernameController.text;
                                String password = _passwordController.text;

                                // Cek kredensial
                                // Untuk menyambungkan Android emulator dengan Django pada localhost,
                                // gunakan URL http://10.0.2.2/
                                final response = await request.login("https://arju-naja-tugas.pbp.cs.ui.ac.id/auth/login/", {
                                'username': username,
                                'password': password,
                                });
                    
                                if (request.loggedIn) {
                                    String message = response['message'];
                                    String uname = response['username'];
                                    Navigator.pushReplacement(
                                        context,
                                        MaterialPageRoute(builder: (context) => MyHomePage()),
                                    );
                                    ScaffoldMessenger.of(context)
                                        ..hideCurrentSnackBar()
                                        ..showSnackBar(
                                            SnackBar(content: Text("$message Selamat datang, $uname.")));
                                    } else {
                                    showDialog(
                                        context: context,
                                        builder: (context) => AlertDialog(
                                            title: const Text('Login Gagal'),
                                            content:
                                                Text(response['message']),
                                            actions: [
                                                TextButton(
                                                    child: const Text('OK'),
                                                    onPressed: () {
                                                        Navigator.pop(context);
                                                    },
                                                ),
                                            ],
                                        ),
                                    );
                                }
                            },
                            child: const Text('Login'),
                        ),
                    ],
                ),
            ),
        );
    }
}
```
### Membuat model kustom sesuai dengan proyek aplikasi Django.

- Mengambil endpoint JSON pada tugas sebelumnya dengan menjalankan `localhost:8000/json` setelah menjalankan server, dan menggunakan website `Quicktype` untuk mengubahnya kedalam bahasa Dart.

- Membuat folder baru bernama `models` dan membuat file `item.dart` sebagai model dan menambahkan kode dari website `Quicktype`:

```dart
// To parse this JSON data, do
//
//     final Item = ItemFromJson(jsonString);

import 'dart:convert';

List<Item> itemFromJson(String str) => List<Item>.from(json.decode(str).map((x) => Item.fromJson(x)));

String itemToJson(List<Item> data) => json.encode(List<dynamic>.from(data.map((x) => x.toJson())));

class Item {
    String model;
    int pk;
    Fields fields;

    Item({
        required this.model,
        required this.pk,
        required this.fields,
    });

    factory Item.fromJson(Map<String, dynamic> json) => Item(
        model: json["model"],
        pk: json["pk"],
        fields: Fields.fromJson(json["fields"]),
    );

    Map<String, dynamic> toJson() => {
        "model": model,
        "pk": pk,
        "fields": fields.toJson(),
    };
}

class Fields {
    int user;
    String name;
    int amount;
    String description;

    Fields({
        required this.user,
        required this.name,
        required this.amount,
        required this.description,
    });

    factory Fields.fromJson(Map<String, dynamic> json) => Fields(
        user: json["user"],
        name: json["name"],
        amount: json["amount"],
        description: json["description"],
    );

    Map<String, dynamic> toJson() => {
        "user": user,
        "name": name,
        "amount": amount,
        "description": description,
    };
}
```
- ### Membuat halaman yang berisi daftar semua item yang terdapat pada endpoint JSON di Django yang telah kamu deploy.

- Menambahkan package http dengan menjalankan `flutter pub add http` pada terminal Flutter

- Menambahkan kode berikut untuk memperbolehkan akses Internet pada aplikasi Flutter yang akan dibuat :

```xml
...
    <application>
    ...
    </application>
    <!-- Required to fetch data from the Internet. -->
    <uses-permission android:name="android.permission.INTERNET" />
...
```

- Memuat file `list_item.dart` pada `lib/screens` dan import library yang diperlukan sehingga berisi seperti berikut :

  ```dart
    // ignore_for_file: library_private_types_in_public_api, non_constant_identifier_names

  import 'package:flutter/material.dart';
  import 'package:http/http.dart' as http;
  import 'dart:convert';
  import 'package:warmad_mobile/models/item.dart';
  import 'package:warmad_mobile/screens/detail_item.dart';
  import 'package:warmad_mobile/widgets/left_drawer.dart';

  class ItemPage extends StatefulWidget {
      const ItemPage({Key? key}) : super(key: key);

      @override
      _ItemPageState createState() => _ItemPageState();
  }

  class _ItemPageState extends State<ItemPage> {
  Future<List<Item>> fetchitem() async {
      var url = Uri.parse(
          'https://arju-naja-tugas.pbp.cs.ui.ac.id/json/');
      var response = await http.get(
          url,
          headers: {"Content-Type": "application/json"},
      );

      // melakukan decode response menjadi bentuk json
      var data = jsonDecode(utf8.decode(response.bodyBytes));

      // melakukan konversi data json menjadi object item
      List<Item> list_item = [];
      for (var d in data) {
          if (d != null) {
              list_item.add(Item.fromJson(d));
          }
      }
      return list_item;
  }

  @override
  Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: const Center(
            child: Text(
            'Item Kamu',
            )
          ),
          backgroundColor: Colors.lightGreen,
          foregroundColor: Colors.white,
        ),
        drawer: const LeftDrawer(),
        body: FutureBuilder(
          future: fetchitem(),
          builder: (context, AsyncSnapshot snapshot) {
            if (snapshot.data == null) {
                return const Center(child: CircularProgressIndicator());
            } else {
                if (!snapshot.hasData) {
                return const Column(
                    children: [
                    Text(
                        "Tidak ada item.",
                        style:
                            TextStyle(color: Color(0xff59A5D8), fontSize: 20),
                    ),
                    SizedBox(height: 8),
                    ],
                );
            } else {
                return ListView.builder(
                  itemCount: snapshot.data!.length,
                  itemBuilder: (_, index) => Card(
                    margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
                    child: InkWell(
                      onTap: () => {
                        Navigator.push(
                          context,
                          MaterialPageRoute(builder: (context) => DetailItem(snapshot.data![index])),
                        )
                      },
                      child: Padding(
                        padding: const EdgeInsets.all(20.0),
                        child: Column(
                          mainAxisAlignment: MainAxisAlignment.start,
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              "${snapshot.data![index].fields.name}",
                              style: const TextStyle(
                                fontSize: 18.0,
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                            const SizedBox(height: 10),
                            Text("Amount : ${snapshot.data![index].fields.amount}"),
                          ],
                        ),
                      ),
                    ),
                  ),
                );
              }
            }
        }));
      }
  }
  ```
 - Mengubah fungsi tombol `Lihat Produk` pada `inventory_button.dart` agar melakukan redirection ke halaman `ProductPage` :

  ```dart
  else if(item.name == "Item Kamu") {
              Navigator.push(context, MaterialPageRoute(builder: (context) => const ItemPage()));
            }
  ```

  -  ### Tampilkan name, amount, dan description dari masing-masing item pada halaman ini.
  ```dart
    child: Padding(
      padding: const EdgeInsets.all(20.0),
      child: Column(
        mainAxisAlignment: MainAxisAlignment.start,
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            "${snapshot.data![index].fields.name}",
            style: const TextStyle(
              fontSize: 18.0,
              fontWeight: FontWeight.bold,
            ),
          ),
          const SizedBox(height: 10),
          Text("Amount : ${snapshot.data![index].fields.amount}"),
        ],
      ),
    ),
  ```

- ### Membuat halaman detail untuk setiap item yang terdapat pada halaman daftar Item.
  - Membuat file baru pada folder `screens` bernama `detail_item.dart` yang berisi details dari item:
  ```dart
    // ignore_for_file: unnecessary_string_interpolations, prefer_const_constructors

  import 'package:flutter/material.dart';
  import 'package:warmad_mobile/models/item.dart';
  import 'package:warmad_mobile/screens/list_item.dart';
  import 'package:warmad_mobile/widgets/left_drawer.dart';

  class DetailItem extends StatelessWidget {
    final Item item;

    const DetailItem(this.item, {Key? key}) : super(key: key);

    @override
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: const Center(
            child: Text(
              'warmad_mobile',
            ),
          ),
          backgroundColor: Colors.lightGreen,
          foregroundColor: Colors.white,
        ),
        drawer: const LeftDrawer(),
        body: Center(
          child: Card(
            margin: const EdgeInsets.all(16),
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                mainAxisSize: MainAxisSize.min,
                children: [
                  Text(
                    "${item.fields.name}",
                    style: const TextStyle(
                      fontSize: 18.0,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  Text("Amount: ${item.fields.amount}"),
                  Text("Category: ${item.fields.category}"),
                  Text("Date Added: ${item.fields.dateAdded}"),
                  const SizedBox(height: 10),
                  Text("${item.fields.description}"),
                  const SizedBox(height: 10),
                  Align(
                    alignment: Alignment.bottomCenter,
                    child: ElevatedButton(
                      style: ButtonStyle(
                        backgroundColor:
                            MaterialStateProperty.all(Colors.lightGreen),
                      ),
                      onPressed: () {
                        Navigator.push(
                          context,
                          MaterialPageRoute(builder: (context) => ItemPage()),
                        );
                      },
                      child: const Text(
                        "Back",
                        style: TextStyle(color: Colors.white),
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      );
    }
  }
  ```
  - ### Halaman ini dapat diakses dengan menekan salah satu item pada halaman daftar Item.
  
  - Membuat tombol untuk pergi ke halaman `detail_item.dart` pada `list_item.dart`:
  ```dart
  return ListView.builder(
    itemCount: snapshot.data!.length,
    itemBuilder: (_, index) => Card(
      margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
      child: InkWell(
        onTap: () => {
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => DetailItem(snapshot.data![index])),
          )
        },
  ```
  - ### Tampilkan seluruh atribut pada model item kamu pada halaman ini.
    ```dart
    child: Card(
          margin: const EdgeInsets.all(16),
          child: Padding(
            padding: const EdgeInsets.all(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              mainAxisSize: MainAxisSize.min,
              children: [
                Text(
                  "${item.fields.name}",
                  style: const TextStyle(
                    fontSize: 18.0,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                Text("Amount: ${item.fields.amount}"),
                Text("Date Added: ${item.fields.dateAdded}"),
                const SizedBox(height: 10),
                Text("${item.fields.description}"),
                const SizedBox(height: 10),
                Align(
                  alignment: Alignment.bottomCenter,
                  child: ElevatedButton(
                    style: ButtonStyle(
                      backgroundColor:
                          MaterialStateProperty.all(Colors.lightGreen),
                    ),
                    onPressed: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (context) => ItemPage()),
                      );
                    },
                    child: const Text(
                      "Back",
                      style: TextStyle(color: Colors.white),
                    ),
                  ),
                ),
              ],
            ),
          ),
        ),
    ```
  - ### Tambahkan tombol untuk kembali ke halaman daftar item.
  ```dart
    Align(
      alignment: Alignment.bottomCenter,
      child: ElevatedButton(
        style: ButtonStyle(
          backgroundColor:
              MaterialStateProperty.all(Colors.lightGreen),
        ),
        onPressed: () {
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => ItemPage()),
          );
        },
        child: const Text(
          "Back",
          style: TextStyle(color: Colors.white),
        ),
      ),
    ),
  ```
</details>
