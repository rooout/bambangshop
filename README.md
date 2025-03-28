# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Penggunaan Struct dalam Pola Observer pada BambangShop
Dalam pola Observer, secara tradisional Subscriber didefinisikan sebagai sebuah interface. Namun, dalam konteks BambangShop, kita dapat menggunakan trait untuk mendefinisikan perilaku Subscriber. Meskipun demikian, penggunaan struct Model sudah cukup karena:

Implementasi Subscriber bersifat tunggal dan konsisten.

Semua subscriber berinteraksi dengan sistem dengan cara yang sama, yaitu melalui notifikasi HTTP.

Perilaku yang dibutuhkan cukup sederhana sehingga tidak diperlukan beberapa variasi implementasi Subscriber.

Menggunakan struct secara langsung menyederhanakan kode dan sesuai dengan prinsip Rust, yang lebih mengutamakan tipe konkret ketika abstraksi tidak diperlukan.

2. Alasan Menggunakan DashMap daripada Vec
Pemilihan DashMap (struktur map/dictionary) dibandingkan dengan Vec (struktur list) dalam kasus ini didasarkan pada beberapa alasan:

Diperlukan mekanisme yang efisien untuk memastikan ID dan URL tetap unik.

Pencarian subscriber berdasarkan ID merupakan operasi yang sering dilakukan dan harus memiliki waktu akses yang cepat.

DashMap memungkinkan pencarian dengan kompleksitas O(1), jauh lebih baik dibandingkan O(n) pada Vec.

Operasi penambahan dan penghapusan yang sering dilakukan lebih efisien dengan DashMap dibandingkan Vec.

DashMap menawarkan thread-safety, yang sangat penting dalam konteks pengembangan server web.

3. Keunggulan DashMap dibandingkan Pola Singleton
Penggunaan DashMap lebih disarankan dibandingkan dengan menerapkan pola Singleton karena:

Sistem kepemilikan (ownership system) dalam Rust membuat implementasi Singleton secara tradisional menjadi lebih rumit.

DashMap sudah menyediakan mekanisme thread-safety secara bawaan, yang jika menggunakan Singleton harus diimplementasikan secara manual.

DashMap menangani akses bersamaan (concurrent access) secara efisien, yang sangat penting dalam pengembangan aplikasi web.

Menggunakan pustaka yang telah teruji seperti DashMap dapat mengurangi risiko bug terkait concurrency.

Rust lebih mendorong penggunaan sistem tipe dan model kepemilikan daripada pola desain berbasis OOP tradisional, seperti Singleton, jika tidak benar-benar diperlukan.

#### Reflection Publisher-2

1. Alasan Memisahkan "Service" dan "Repository" dari Model
Memisahkan Service dan Repository dari Model penting untuk menjaga desain sistem yang bersih dan terstruktur. Berikut beberapa alasannya:

Single Responsibility Principle (SRP): Dengan pemisahan ini, setiap komponen memiliki peran yang jelas. Repository berfokus pada akses data, sementara Service menangani aturan bisnis dan logika pemrosesan.

Separation of Concerns: Memisahkan penyimpanan data, logika bisnis, dan presentasi membuat sistem lebih modular dan lebih mudah dipahami.

Testability: Dengan pemisahan yang jelas, pengujian bisa dilakukan lebih mudah menggunakan mock, tanpa harus menguji seluruh sistem secara bersamaan.

Maintainability: Perubahan pada database tidak akan mempengaruhi logika bisnis, dan sebaliknya. Hal ini memungkinkan pengembangan lebih fleksibel dan paralel.

Scalability: Setiap lapisan dapat diskalakan secara independen sesuai kebutuhan, misalnya peningkatan koneksi database atau pemrosesan bisnis.

2. Dampak Jika Model Menangani Semuanya
Jika semua fungsi, seperti penyimpanan data dan logika bisnis, diletakkan langsung dalam Model, maka akan muncul beberapa masalah:

Coupling yang Tinggi: Model Program, Subscriber, dan Notification akan saling terikat secara erat, menyebabkan ketergantungan yang sulit dikelola.

Kode Menjadi Kompleks: Model akan menjadi terlalu besar karena menangani banyak fungsi sekaligus, dari penyimpanan hingga pengelolaan notifikasi.

Kohesi yang Menurun: Setiap model akan kehilangan fokus dan menjadi tidak spesifik dalam perannya.

Pengujian Sulit: Pengujian memerlukan seluruh sistem berjalan sekaligus, dibandingkan dengan pendekatan yang lebih modular di mana setiap komponen bisa diuji secara terpisah.

3. Manfaat Mengeksplorasi Postman untuk Pengujian
Menggunakan Postman sangat membantu dalam menguji sistem dengan lebih terstruktur dan efisien:

Request Collections: Mengelompokkan permintaan API terkait membantu mengelola pengujian berbagai komponen dengan lebih rapi.

Environment Variables: Memungkinkan pengujian dalam berbagai lingkungan (misalnya, pengembangan dan produksi) tanpa perlu mengganti URL secara manual.

Automated Testing: Memungkinkan penulisan skrip untuk memastikan endpoint berfungsi sesuai harapan.

Request Chaining: Menggunakan data dari satu respons untuk permintaan berikutnya memungkinkan pengujian alur kerja yang realistis, misalnya menguji proses berlangganan dan penerimaan notifikasi.

Mock Servers: Postman memungkinkan pembuatan server tiruan yang membantu pengembangan frontend tanpa harus menunggu backend selesai.

Team Collaboration: Koleksi API dapat dibagikan ke tim, sehingga lebih mudah untuk memahami dan menguji sistem bersama.

#### Reflection Publisher-3

1. Model Push dalam Pola Observer pada BambangShop
Dalam tutorial ini, kita menerapkan variasi model Push dari Pola Observer. Publisher (BambangShop) secara aktif mengirimkan notifikasi berisi data yang relevan kepada subscriber setiap kali terjadi perubahan status, seperti pembuatan produk, penghapusan, atau promosi. Dalam pendekatan ini, publisher menentukan kapan dan data apa yang dikirim dalam payload notifikasi, sementara subscriber hanya bertindak sebagai penerima pasif yang memproses data yang diterima.

2. Keunggulan dan Kelemahan Model Pull
Keunggulan Model Pull:
Subscriber hanya mengambil informasi yang dibutuhkan, sehingga dapat mengurangi penggunaan bandwidth.

Subscriber dapat melakukan pembaruan sesuai dengan jadwal mereka sendiri, sehingga mencegah kelebihan beban selama periode aktivitas tinggi.

Hubungan antara publisher dan subscriber lebih longgar, karena publisher tidak perlu mengetahui secara spesifik data apa yang dibutuhkan setiap subscriber.

Memiliki toleransi kesalahan yang lebih baik, karena jika subscriber sementara tidak tersedia, notifikasi tidak akan terlewat begitu saja.

Kelemahan Model Pull:
Latensi lebih tinggi karena subscriber melakukan polling pada interval tertentu, bukan menerima pembaruan secara langsung.

Beban sistem meningkat akibat polling yang dilakukan oleh banyak subscriber secara bersamaan.

Implementasi lebih kompleks, karena subscriber harus menyimpan statusnya sendiri untuk mendeteksi perubahan.

Potensi inkonsistensi data jika subscriber melakukan polling pada waktu yang berbeda-beda.

Penggunaan sumber daya tidak efisien, terutama jika tidak ada perubahan data tetapi polling tetap dilakukan.

3. Konsekuensi Tidak Menggunakan Multi-Threading dalam Notifikasi
Jika proses notifikasi dilakukan tanpa multi-threading, beberapa masalah dapat muncul:

Bottleneck kinerja: Notifikasi diproses secara berurutan, sehingga thread utama akan terhambat saat menunggu setiap permintaan HTTP selesai.

Request timeout: Subscriber yang lambat atau tidak responsif dapat menyebabkan seluruh proses notifikasi tertunda.

Skalabilitas rendah: Waktu pengiriman notifikasi meningkat secara linier seiring bertambahnya jumlah subscriber.

Penurunan responsivitas: Aplikasi bisa terasa lambat atau bahkan tidak responsif selama proses penyiaran notifikasi berlangsung.

Potensi deadlock: Jika subscriber mencoba berkomunikasi dengan publisher saat notifikasi masih berlangsung, bisa terjadi kondisi deadlock.

Risiko dampak kegagalan yang lebih besar: Jika ada notifikasi yang gagal, maka prosesnya bisa tertunda atau bahkan menghambat notifikasi ke subscriber lain.