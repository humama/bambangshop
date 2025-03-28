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

1. Ya, Model struct `Subscriber` pada BambangShop lebih baik dijadikan interface/trait. Dengan menjadikan `Subscriber` sebuah trait, sebuah object pada codebase dapat bergantung kepada trait tersebut daripada implementasi concrete class yang ada sehingga perubahan yang terjadi pada concrete class tidak akan terlalu memengaruhi bagian lain pada codebase.
 
 2. Untuk kasus bambangshop ini, penggunaan `DashMap` sudah tepat karena terdapatt operasi `delete` berdasarkan `url`, dimana `url` bersifat unik.
 
 3. Ya, kita masih membutuhkan `DashMap` untuk memastikan operasi CRUD bersifat thread-safe. Lagipula, implementasi pattern singleton tetap membutuhkan sinkronisasi agar tidak terjadi hal yang tidak diinginkan dalam penerapan concurrency di Rust.

#### Reflection Publisher-2

1. Memisahkan Service dan Repository layer dari Model merupakan implementasi <i>separation of concerns</i> dimana Service, Repository, dan Model masing-masing memiliki fungsi yang berbeda. Implementasi tersebut sejalan dengan Single Responsibility Principle dan memastikan Service, Repository, dan Model tidak memiliki pekerjaan yang overlap. Dengan adanya <i>separation of concerns</i>, layer Service, Repository, dan Model dapat berfokus dengan tugasnya masing-masing tanpa harus mengkhawatirkan layer lainnya.
 
 2. Memasukkan seluruh pekerjaan Service layer dan Repository layer ke dalam Model layer tentu akan membuat Model memiliki tugas yang banyak. Selain menyimpan data, Model dituntut untuk menghandle business logic dan data access yang masing-masing seharusnya dihandle oleh Service dan Repository layer. Kode yang ditulis juga pastinya akan tertumpuk pada Model sehingga seiring bertambahnya fungsionalitas pada aplikasi, <i>maintenance</i> pada Model akan semakin sulit untuk dilakukan.
 
 3. Di samping menyediakan layanan untuk mengirim HTTP request, Postman memiliki beberapa fitur tambahan yang berguna untuk testing sebuah aplikasi. Salah satu fitur tersebut merupakan fitur collections yang dapat menyimpan konfigurasi HTTP request yang akan dikirim. Fitur collections menyimpan sebuah konfigurasi dengan susunan yang mirip dengan file system pada umumnya. Di Postman, sebuah konfigurasi dapat dianalogikan sebagai sebuah file dalam file system. Layaknya file, konfigurasi-konfigurasi pada Postman dapat dikelompokkan ke dalam sebuah direktori bersama konfigurasi maupun direktori lainnya. Selain itu, konfigurasi yang ada dapat dibagikan ke pengguna lain. Fitur <i>sharing</i> ini sangatlah berguna bagi organisasi/team yang sedang mengembangkan sebuah aplikasi sebab konfigurasi yang sama dapat dibagikan ke anggota organisasi/team lainnya untuk testing. Fitur ini merupakan salah satu fitur Postman yang kemungkinan besar akan sering saya gunakan ketika sedang mengembangkan sebuah aplikasi.

#### Reflection Publisher-3

1. Push model, sebab pada case bambangshop ini, `NotificationService` memanggil `notify()` pada event sebuah product ditambahkan, dihapus, atau dipromosikan melalui `ProductService`. Method `notify()` tersebut juga kemudian memanggil method `update()` pada `Subscriber` untuk melakukan log sebuah string yang menyatakan notifikasi berhasil diterima. Dalam kata lain, event yang terjadi pada publisher menjadi penyebab method `update()` pada subscriber dipanggil.
 
 2. Pada push model, publisher mengirimkan notifikasi yang berisi data kepada subscribernya. Selain itu, publisher juga menentukan data apa yang dikirimkan kepada subscriber. Sebaliknya, pada pull model, subscriber menentukan data apa yang akan diterimanya dan mengambil data tersebut dari publisher. Fleksibilitas menentukan data apa yang dapat diterima oleh subscriber merupakan salah satu kelebihan dari pull model. Akan tetapi, pada pull model ini, subscriber diharuskan untuk mengetahui bentuk data yang dapat diterimanya. Dalam kata lain, subscriber harus mengetahui beberapa informasi tentang publishernya, berpotensi menyebabkan coupling. Hal ini merupakan salah satu kekurangan dari pull model.
 
 3. Tanpa multi-threading, pengiriman notifikasi akan bersifat sinkronus. Setiap pengiriman notifikasi akan menunggu notifikasi tersebut selesai dikirimkan sebelum mengirimkan notifikasi lain. Hal tersebut berdampak pada peningkatan latency penerimaan notifikasi oleh subscriber, terutama subscriber yang berada pada urutan akhir.