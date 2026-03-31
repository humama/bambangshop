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
-   [✓] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✓] Commit: `Create Subscriber model struct.`
    -   [✓] Commit: `Create Notification model struct.`
    -   [✓] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [✓] Commit: `Implement add function in Subscriber repository.`
    -   [✓] Commit: `Implement list_all function in Subscriber repository.`
    -   [✓] Commit: `Implement delete function in Subscriber repository.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [✓] Commit: `Create Notification service struct skeleton.`
    -   [✓] Commit: `Implement subscribe function in Notification service.`
    -   [✓] Commit: `Implement subscribe function in Notification controller.`
    -   [✓] Commit: `Implement unsubscribe function in Notification service.`
    -   [✓] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [✓] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Apakah masih perlu interface (trait) untuk Subscriber?

Dalam Observer Pattern, biasanya Subscriber (Observer) dibuat sebagai interface agar:

- Bisa memiliki banyak implementasi berbeda
- Lebih fleksibel (polymorphism)

Namun, pada kasus BambangShop, penggunaan trait tidak terlalu diperlukan karena:

- Semua subscriber memiliki struktur yang sama (URL dan nama)
- Tidak ada variasi perilaku (behavior) antar subscriber
- Subscriber hanya digunakan sebagai data holder (model struct)

Jadi, menggunakan satu struct Subscriber saja sudah cukup untuk kebutuhan saat ini.

Namun, jika ke depannya:

- Ada banyak tipe subscriber (misalnya EmailSubscriber, WebhookSubscriber, dll)
- Atau masing-masing punya cara notifikasi berbeda

Maka penggunaan trait akan lebih tepat untuk mendukung extensibility.

2. Apakah Vec cukup atau perlu DashMap?

Dalam konteks ini:

- id pada Program dan url pada Subscriber harus unik
- Kita juga butuh operasi:
  - tambah (add)
  - ambil semua (list)
  - hapus (delete)

Jika menggunakan Vec (list):

- Tidak menjamin keunikan data secara langsung
- Pencarian dan penghapusan butuh iterasi (O(n))
- Kurang efisien untuk data yang besar

Sedangkan DashMap (map/dictionary):

- Menjamin keunikan key (misalnya URL sebagai key)
- Operasi lebih cepat (O(1))
- Lebih efisien untuk lookup, insert, dan delete

Jadi, menggunakan DashMap lebih tepat dibandingkan Vec untuk kasus ini.

3. Apakah masih perlu DashMap atau cukup Singleton?

Dalam design pattern:

- Singleton memastikan hanya ada satu instance dari suatu objek
- DashMap adalah struktur data yang thread-safe

Pada kode ini:

```rust
lazy_static! {
    static ref SUBSCRIBERS: DashMap<...> = DashMap::new();
}
```

Kita sebenarnya sudah menerapkan:

- Singleton pattern → karena SUBSCRIBERS hanya satu instance global
- Thread-safe collection → menggunakan DashMap

Jika hanya menggunakan Singleton tanpa DashMap:

- Tidak otomatis thread-safe
- Bisa terjadi race condition saat akses bersamaan

Sedangkan dengan DashMap:

- Aman untuk concurrent access (multi-thread)
- Tidak perlu manual locking (seperti Mutex)

Jadi:

- Singleton saja tidak cukup
- DashMap tetap diperlukan untuk menjamin thread safety

Kesimpulan

- Struct Subscriber sudah cukup, trait belum diperlukan saat ini
- DashMap lebih cocok dibanding Vec karena efisiensi dan keunikan data
- Kombinasi Singleton + DashMap adalah solusi terbaik untuk thread-safe global state

#### Reflection Publisher-2

1. Mengapa perlu memisahkan Service dan Repository dari Model?

Dalam pola Model-View-Controller (MVC) klasik, Model memang mencakup:

- Data (state)
- Business logic

Namun, dalam praktik pengembangan modern, kita sering menerapkan prinsip:

- Separation of Concerns
- Single Responsibility Principle (SRP)

Dengan memisahkan:

- Model → hanya merepresentasikan struktur data (misalnya Subscriber, Notification)
- Repository → menangani akses data (CRUD, penyimpanan, pengambilan data)
- Service → menangani business logic (aturan bisnis, validasi, orkestrasi proses)

Keuntungannya:

- Kode lebih modular dan mudah dipahami
- Mudah diuji (testing lebih terisolasi)
- Mudah dikembangkan (scalable)
- Mengurangi ketergantungan antar komponen

Jadi, pemisahan ini membuat arsitektur lebih rapi dan maintainable dibandingkan semua logika ditumpuk di Model.

2. Apa yang terjadi jika hanya menggunakan Model?

Jika semua logic dimasukkan ke dalam Model (misalnya Program, Subscriber, Notification), maka:

Masalah yang akan muncul:

- Model menjadi terlalu kompleks (God Object)
- Satu model harus:
  - Menyimpan data
  - Mengelola data
  - Menjalankan business logic
- Interaksi antar model jadi saling bergantung (tight coupling)

Contoh:

- Subscriber harus tahu cara menyimpan dirinya sendiri
- Notification harus tahu cara mengambil subscriber
- Program harus tahu cara mengirim notifikasi

Akibatnya:

- Kode sulit dibaca
- Sulit di-maintain
- Sulit di-debug
- Sulit untuk testing (karena semua logic bercampur)

Dengan pemisahan:

- Model tetap sederhana
- Service mengatur alur
- Repository mengurus data

Jadi kompleksitas berkurang dan struktur lebih jelas.

3. Bagaimana Postman membantu dalam testing?

Saya sudah mencoba menggunakan Postman, dan tool ini sangat membantu dalam menguji API yang saya buat.

Manfaat utama:

- Mengirim HTTP request dengan mudah (GET, POST, dll)
- Tidak perlu membuat frontend dulu
- Bisa langsung melihat response dari server
- Mempermudah debugging API

Contoh penggunaan pada project ini:
- Menguji endpoint:
  - ```/notification/subscribe/<product_type>```
  - ```/notification/unsubscribe/<product_type>```
- Mengirim data JSON subscriber
- Melihat apakah data berhasil ditambahkan atau dihapus

Fitur Postman yang menarik:
- Collections
→ Menyimpan kumpulan API request untuk project
- Environment Variables
→ Menyimpan base URL atau parameter (memudahkan reuse)
- History
→ Melihat request yang pernah dikirim
- Automated Testing
→ Bisa menambahkan script untuk validasi response
- Mock Server (opsional)
→ Simulasi API tanpa backend

Kesimpulan

- Pemisahan Model, Service, dan Repository membuat kode lebih terstruktur dan scalable
- Menggunakan hanya Model akan meningkatkan kompleksitas dan menyulitkan maintenance
- Postman sangat membantu dalam proses testing API dan debugging selama development

#### Reflection Publisher-3
