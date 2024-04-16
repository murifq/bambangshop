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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Observer design pattern menggunakan konsep subscription untuk memberikan notifikasi kepada subscriber tentang apapun yang terjadi pada object yang mereka observing.

Pada kasus sejauh ini, tipe Subscriber hanya terdiri dari satu tipe. Tidak terdapat banyak jenis tipe subscriber. Oleh karena itu, tidak dibutuhkan interface atau trait.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Penggunaan Dashmap pada kasus ini lebih diuntungkan dibandingkan Vec (list). Hal tersebut karena id yang unik bisa dijadikan sebagai key, sehingga pengkasesan datanya akan jauh lebih mudah.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Penggunaan Dashmap tetap diperlukan. Karena dengan menggunakan Dashmap, program yang kita buat lebih aman (karena Dashmap menyediakan implementasi HashMap yang thread-safe). Selain itu, penggunaan Singleton Design Pattern juga tidak menjamin programnya thread-safe pada operasi multithreading. Sehingga, penggunaan DashMap tetap diperlukan.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

"Service" dan "Repository" harus dipisahkan karena kegunaannya yang berbeda. Model bertanggung jawab untuk mengatur hal yang berkaitan dengan data yang ada di database. Repository bertanggung jawab untuk mengelola penyimpanan dan pengambilan data. Sedangkan, Service bertanggung jawab untuk implementasi logika bisnis. Degan pemisahan tersebut, akan memudahkan kita untuk mengelola program kita kedepannya karena kode kita tidak tercampur aduk untuk fungsi yang berbeda. Selain itu, juga memudahkan kita untuk menguji bagian tertentu dari kode (_testing_).

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Apabila kita hanya menggunakan model tanpa pemisahan, maka kompleksitas dari kode sangat tinggi sekali. Sangat sulit untuk me-maintain kode kita kedepannya. Selain itu, apabila terdapat bug pada kode, akan sangat sulit untuk di-fix karena semua kode campur aduk kegunannya. Selain itu, juga akan sulit melakukan testing untuk bagian tertentu dari kode.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Ya, saya pernah menggunakan Postman sebelumnya pada matakuliah PBP. Dengan menggunakan aplikasi ini, saya dapat menguji endpoint dari web yang saya buat, dengan beberapa http method seperti GET, POST, PUT, DELETE, dan lainnya. Berikut adalah beberapa fitur Postman yang mungkin berguna kedepannya:
* Authentication dan Authorization : Postman dapat mendukung berbagai metode autentikasi seperti JWT, Bearer Token, dan lain-lain.
* Pengelompokkan : Postman dapat mengelompokkan API Request dalam koleksi dan folder yang berbeda-beda. Sehingga, memudahkan untuk melakukan pengetesan API.
* Pengujian otomatis : Dengan menggunakan Postman, kita dapat melakukakan pengujian otomatis dan memvalidasi respons dari API kita.

#### Reflection Publisher-3
