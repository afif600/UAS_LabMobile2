# UAS Pemrograman Mobile 2

## Profile

| #               | Biodata              |
| --------------- | -------------------- |
| **Nama**        | Afif Firmansyah      |
| **NIM**         | 312110232            |
| **Kelas**       | TI.21.A1             |
| **Mata Kuliah** | Pemrograman Mobile 2 |

## Daftar Isi

- [Profile](#profile)
- [Daftar Isi](#daftar-isi)
- [Requirements](#requirements)
- [Tutorial](#tutorial)

## Requirements

- [Flutter](https://docs.flutter.dev/get-started/install)

## Tutorial

- Pertama, Download Flutter sesuai dengan spesifikasi atau persyaratan komputer kalian.

| Operating System |                              URL                              |
| ---------------- | :-----------------------------------------------------------: |
| Windows          | [Link](https://docs.flutter.dev/get-started/install/windows)  |
| macOS            |  [Link](https://docs.flutter.dev/get-started/install/macos)   |
| Linux            |  [Link](https://docs.flutter.dev/get-started/install/linux)   |
| ChromeOS         | [Link](https://docs.flutter.dev/get-started/install/chromeos) |

- Kemudian, masuk ke halaman (JadwalSholat API) dan klik pada tombol "Get API Key" di bagian atas kanan halaman.
- Daftar dengan akun kalian atau masuk jika kalian sudah memiliki akun.
- Setelah mendaftar/masuk, kalian akan diarahkan ke dashboard JadwalSholatAPI.
- Kemudian, buat project baru dalam direktori anda dengan nama yang kalian inginkan. Contoh:

```bash
flutter create jadwalSholat-app
```

- Lalu, pada direktori lib > main.dart hapus semua kode, kemudian ubah dengan kode ini:

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() => runApp(PrayerTimesApp());

class PrayerTimesApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Prayer Times',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: PrayerTimesScreen(),
    );
  }
}

class PrayerTimesScreen extends StatefulWidget {
  @override
  _PrayerTimesScreenState createState() => _PrayerTimesScreenState();
}

class _PrayerTimesScreenState extends State<PrayerTimesScreen> {
  TextEditingController _cityController = TextEditingController();
  String _prayerTimes = '';

  @override
  void dispose() {
    _cityController.dispose();
    super.dispose();
  }

  void fetchPrayerTimes(String city) async {
    final response = await http.get(Uri.parse(
        'https://api.aladhan.com/v1/timingsByCity?city=$city&country=Indonesia'));

    if (response.statusCode == 200) {
      final data = jsonDecode(response.body);
      final timings = data['data']['timings'];

      setState(() {
        _prayerTimes = '';
        timings.forEach((key, value) {
          _prayerTimes += '$key: $value\n';
        });
      });
    } else {
      setState(() {
        _prayerTimes = 'Gagal mengambil jadwal sholat. Silakan coba lagi.';
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Jadwal Sholat'),
      ),
      body: LayoutBuilder(
        builder: (BuildContext context, BoxConstraints constraints) {
          return SingleChildScrollView(
            child: ConstrainedBox(
              constraints: BoxConstraints(minHeight: constraints.maxHeight),
              child: Padding(
                padding: EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.stretch,
                  children: <Widget>[
                    SizedBox(height: 16.0),
                    TextField(
                      controller: _cityController,
                      decoration: InputDecoration(
                        labelText: 'Kota',
                        border: OutlineInputBorder(),
                        prefixIcon: Icon(Icons.location_city),
                      ),
                    ),
                    SizedBox(height: 20.0),
                    ElevatedButton(
                      child: Text(
                        'Tampilkan Jadwal Sholat',
                        style: TextStyle(fontSize: 18),
                      ),
                      onPressed: () {
                        fetchPrayerTimes(_cityController.text);
                      },
                    ),
                    SizedBox(height: 16.0),
                    Card(
                      child: Padding(
                        padding: EdgeInsets.all(18.0),
                        child: Text(
                          _prayerTimes,
                          style: TextStyle(fontSize: 16),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
            ),
          );
        },
      ),
    );
  }
}
```

- Dan jangan lupa menambahkan Library http pada file `pubspec.yaml`.

```dart
dependencies:
  flutter:
    sdk: flutter
  http: ^0.13.3
```

- Buka terminal, Kemudian run command `flutter pub get`.
- Setelah selesai, jalankan programnya with debugging.
- Maka hasilnya akan seperti ini :>

![Output](img/output.png)

- Kode diatas dapat kalian improvisasi dengan kreasi kalian sendiri.

# Terima Kasih!!!
