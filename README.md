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

1. Dalam pola Observer, Pelanggan biasanya direpresentasikan sebagai antarmuka atau trait untuk mendefinisikan kontrak yang harus dipatuhi oleh pelanggan konkret. Hal ini memungkinkan fleksibilitas dan loose coupling antara subjek (BambangShop dalam hal ini) dan pengamatnya (pelanggan). Dengan mendefinisikan antarmuka atau trait, subjek tidak perlu mengetahui detail implementasi spesifik dari pelanggannya, sehingga memudahkan penambahan atau penghapusan pelanggan tanpa harus memodifikasi subjek.

Dalam kasus BambangShop, menggunakan antarmuka atau trait untuk Pelanggan tetap bermanfaat. Ini akan memungkinkan berbagai jenis pelanggan untuk mengimplementasikan perilaku mereka sendiri untuk memperbarui ketika diberi tahu oleh BambangShop. Sebagai contoh, satu pelanggan mungkin memperbarui tampilannya, sementara yang lain mungkin mengirimkan email pemberitahuan. Dengan mendefinisikan antarmuka atau trait, BambangShop tetap terlepas dari implementasi spesifik pelanggannya.

2. Pilihan antara menggunakan Vec (daftar) atau DashMap (peta/dictionary) untuk menyimpan pelanggan tergantung pada kebutuhan aplikasi. Menggunakan Vec akan memungkinkan penyimpanan sederhana dari pelanggan dalam urutan berurutan. Namun, jika Anda perlu dengan cepat mengakses pelanggan berdasarkan pengidentifikasi unik mereka (id dalam Program, url dalam Pelanggan), maka menggunakan struktur data peta/dictionary seperti DashMap akan lebih efisien.

Jika Anda perlu mengambil pelanggan dengan efisien berdasarkan pengidentifikasi unik mereka, menggunakan DashMap atau implementasi peta serupa yang aman dari segi utas akan diperlukan. Ini memberikan akses cepat berdasarkan kunci dan memastikan keamanan utas saat mengakses dan memodifikasi peta secara bersamaan.

3. Di Rust, memastikan keselamatan utas sangat penting, terutama dalam program yang konkuren. Meskipun menggunakan variabel statis untuk daftar pelanggan (SUBSCRIBERS) mungkin terlihat nyaman, hal ini memperkenalkan potensi masalah dengan keselamatan utas, karena variabel statis dibagikan di semua utas.

Menggunakan DashMap atau struktur data yang serupa yang aman dari segi utas adalah pendekatan yang masuk akal untuk memastikan keselamatan utas saat berurusan dengan sumber daya bersama seperti daftar pelanggan. DashMap menyediakan akses konkuren ke HashMap yang mendasarinya, memungkinkan beberapa utas untuk membaca dan memodifikasi peta dengan aman.

Alternatifnya, Anda dapat mengimplementasikan pola Singleton untuk memastikan bahwa hanya satu instance dari daftar pelanggan ada di seluruh aplikasi. Namun, Anda masih perlu memastikan keselamatan utas saat mengakses dan memodifikasi instance singleton, yang bisa dicapai dengan menggunakan Mutex atau RwLock untuk menyinkronkan akses ke sumber daya bersama.


#### Reflection Publisher-2

1. Dalam pola MVC (Model-View-Controller), terdapat kebutuhan untuk memisahkan "Service" dan "Repository" dari "Model". Ini didasarkan pada prinsip pemisahan kepentingan yang menyatakan bahwa berbagai aspek dari aplikasi harus dipisahkan ke dalam modul atau lapisan yang berbeda. Dengan memisahkan "Service" dan "Repository" dari "Model":

        Lapisan Layanan (Service): Lapisan layanan menginkapsulasi logika bisnis aplikasi. Ini bertindak sebagai perantara antara controller dan repository. Dengan memisahkan logika bisnis ke dalam lapisan layanan, kita dapat mencapai pemisahan kepentingan yang bersih, membuat basis kode lebih mudah dipahami, diuji, dan dipelihara.

        Lapisan Repositori (Repository): Lapisan repositori bertanggung jawab atas akses dan manipulasi data. Ini mengabstraksi detail tentang bagaimana data disimpan dan diambil, memberikan antarmuka yang bersih untuk lapisan layanan berinteraksi. Memisahkan repositori dari model memungkinkan untuk beralih antara mekanisme penyimpanan data tanpa memengaruhi logika bisnis.

2. Jika kita hanya menggunakan Model tanpa memisahkan "Service" dan "Repository":

        Kompleksitas Kode: Tanpa memisahkan kepentingan, Model menjadi penuh dengan logika bisnis dan logika akses data. Ini menyebabkan peningkatan kompleksitas kode, pengurangan kemudahan pemeliharaan, dan kesulitan dalam pengujian. Ini juga melanggar prinsip tanggung jawab tunggal, membuat basis kode sulit dipahami dan diperluas.

        Kesulitan dalam Pengujian: Pengujian menjadi lebih sulit karena logika bisnis dan logika akses data terjalin erat dalam Model. Pengujian unit menjadi rumit, karena sulit untuk mengisolasi dan menguji komponen individu. Hal ini dapat mengakibatkan penurunan cakupan pengujian dan peningkatan risiko bug masuk ke produksi.

3. Postman adalah alat yang sangat berguna untuk pengembangan dan pengujian API. Beberapa fitur Postman yang saya temukan bermanfaat termasuk:

        Pembangunan Permintaan: Postman menyediakan antarmuka yang intuitif untuk membangun permintaan HTTP, memungkinkan pengguna untuk menentukan metode permintaan, header, parameter, dan payload dengan mudah.

        Koleksi dan Lingkungan: Koleksi memungkinkan pengguna untuk mengatur dan mengelompokkan permintaan terkait, sementara lingkungan memungkinkan konfigurasi variabel yang dapat digunakan di seluruh permintaan, membuat pengujian lebih efisien dan terorganisir.

        Pengujian dan Otomatisasi: Postman mendukung penulisan pengujian menggunakan JavaScript, yang dapat dieksekusi setelah mengirim permintaan untuk memvalidasi respons. Selain itu, alat ini menyediakan fitur untuk mengotomatisasi pengujian dan mengintegrasikannya ke dalam alur kerja CI/CD.

        Dokumentasi: Postman memungkinkan pengguna untuk menghasilkan dokumentasi API secara otomatis dari koleksi, memudahkan berbagi spesifikasi API dan petunjuk penggunaan dengan rekan tim atau klien.

Postman secara keseluruhan membantu dalam pengembangan dan pengujian API, membuatnya menjadi alat yang sangat berharga bagi insinyur perangkat lunak yang bekerja pada proyek berbasis API.

#### Reflection Publisher-3
