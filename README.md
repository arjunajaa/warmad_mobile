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