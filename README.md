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
