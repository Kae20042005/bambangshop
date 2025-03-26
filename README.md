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
-   [V] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [V] Commit: `Create Subscriber model struct.`
    -   [V] Commit: `Create Notification model struct.`
    -   [V] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [V] Commit: `Implement add function in Subscriber repository.`
    -   [V] Commit: `Implement list_all function in Subscriber repository.`
    -   [V] Commit: `Implement delete function in Subscriber repository.`
    -   [V] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [V] Commit: `Create Notification service struct skeleton.`
    -   [V] Commit: `Implement subscribe function in Notification service.`
    -   [V] Commit: `Implement subscribe function in Notification controller.`
    -   [V] Commit: `Implement unsubscribe function in Notification service.`
    -   [V] Commit: `Implement unsubscribe function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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

1) Single struct saja tanpa trait sebenernya sudah dapat bekerja apabila hanya akan ada satu tipe Subscriber. Namun,
dari sisi scalabilitynya akan menghasilkan desain yang lebih sulit untuk dikembangkan. Sehingga, lebih baik jika
menggunakan interface (trait) tambahan.<br />
2) Menggunakan Vec sebagai penyipanan sudah cukup, namun kurang efektif dari segi waktu karena apabila kita mencari
data dalam Vec perlu melakukan iterasi satu persatu. Sehingga, untuk penyimpanan data lebih direkomendasikan menggunakan
DashMap yang lebih cepat untuk melakukan pencarian data karena tinggal melakukan referensi dari key yang ingin dicari. <br />
3) Masih memerlukan DashMap karena agar program kita thread-safe kita perlu mengimplementasikan DashMap pada penyimpanan
data. Penggunaan singleton pattern tidak akan membuat program thread-safe. <br />

#### Reflection Publisher-2

1) Untuk memenuhi single responsibility principle (SRP), membuat aplikasi memiliki scalability dan maintainability yang baik,
dapat mempermudah untuk melakukan testing dengan melakukan mock database dan logic test secara mandiri. <br />
2) Ketika ada perubahan pada logic database maka akan mempengaruhi business logic, sehingga kode lebih susah untuk di
rawat. Kemudian, apabila semua logic dalam Model yang sama ada kemungkinan ketika kita ingin mengakses salah satu model
saja diperlukan untuk mengambil model lainnya sehingga menambah kompleksitas kode. <br />
3) Postman sangat membantu dalam melakukan API Testing. Dengan adanya Postman, saya dapat melakukan testing Endpoints
dengan mudah karena dapat melakukan pengiriman request GET, POST, PUT, DELETE untuk memeriksa kerja API. Fitur yang menarik
bagi saya adalah fitur Newman, yang dapat membuat saya melakukan tests ketika sedang melakukan integrasi CI/CD. <br />

#### Reflection Publisher-3

1) Tutorial kali ini menggunakan variasi Observer Pattern Push Model (publisher push data to subscribers). <br />
2) Keuntungan menggunakan pull model adalah Overhead data lebih sedikit karena publisher hanya menyediakan data yang
diminta secara eksplisit dan Subscribers memiliki konrol yang lebih banyak karena subscriber dapat menentukan kapan
ingin melakukan pull uodate. Kerugian menggunakan pull model adalah latensi lebih tinggi karena subscribers bisa saja
tidak mendapat update secara langsung dan subscriber berpotensi menggunakan data yang sudah lama karena subscriber
belum memeriksa update, maka aplikasi menggunakan data yang masih lama. <br />
3) Proses pengiriman notifikasi akan menjadi sangat lama sehingga ada delay karena sistem harus mengirim notifikasi
satu per satu ke setiap subscriber. Kemudian, sistem notifikasi yang tidak multi-threading juga memiliki Scalability
yang buruk karena semakin banyak subscriber sistem akan semakin lambat.
