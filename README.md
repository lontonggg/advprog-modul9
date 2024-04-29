# Advanced Programming - Module 9
#### Reyhan Zada Virgiwibowo - 2206081723 - Advanced Programming C

## Reflection

#### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

* **Unary RPC**: Client mengirimkan request ke server dan menunggu server memberikan response. Metode ini cocok untuk proses transfer data yang kecil dan simple serca tidak terjadi secara real-time.

* **Server Streaming RPC**: Client mengirimkan request ke server dan server akan memberikan response yang merupakan serangkaian pesan berurutan. Metode ini cocok untuk digunakan saat client ingin meminta response yang berupa data yang besar.

* **Bi-Directional Streaming RPC**: Client dan server mengirimkan serangkaian pesan secara independen dalam waktu yang bersamaan (real-time). Metode ini cocok digunakan ketika client dan server perlu bertukar data secara instan seperti Chat atau Call.

#### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

* **Authentication**: Mengimplementasikan mekanisme authenication merupakan hal yang krusial untuk memastikan hanya pengguna yang terautentikasi yang dapat mengakses service gRPC. gRPC sendiri mendukung penggunaan protokol mekanisme authentication seperti OAuth2, JWT (Json Web Token), dan TLS (Transport Layer Security).

* **Authorization**: Setelah user terautentikasi, authorization dapat diterapkan dengan Role Based Access Control (RBAC) atau Attribute Based Access Control (ABAC). Penerapan kontrol akses ini biasanya menggunakan pola middleware atau interceptor, yang akan memeriksa izin akses pengguna sebelum memproses permintaan tersebut.

* **Data Encryption**: Secara default, gRPC mengenkripsi data yang akan ditransmisikan menggunakan HTTP/2 yang mendukung enkripsi melalui TLS.

#### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Permasalahan dan isu yang mungkin muncul dalam menghandle bi-directional streaming adalah sebagai berikut :

- **Masalah Sinkronisasi**: Penting untuk memastikan sinkronisasi yang tepat antara pesan yang dikirim dan diterima oleh client dan server. Sinkronisasi yang tidak tepat dapat mengakibatkan pesan tidak sampai tepat waktu atau dalam urutan yang salah, yang dapat mengganggu pengalaman pengguna.

- **Penanganan Kesalahan dan Koneksi Terputus**: Koneksi mungkin mengalami putus atau terganggu, terutama dalam lingkungan jaringan yang tidak stabil. Penting untuk memiliki mekanisme penanganan kesalahan yang baik untuk mengatasi koneksi yang terputus atau masalah lain yang mungkin timbul selama streaming dua arah berlangsung.

- **Masalah Konkurensi**: Dalam aplikasi yang bersifat streaming dua arah, seperti aplikasi obrolan, perlu memperhatikan masalah konkurensi. Hal ini terutama penting dalam lingkungan Rust, di mana keselamatan memori dan konkurensi menjadi perhatian utama. Dengan memastikan bahwa konstruksi konkurensi yang digunakan aman dan efisien, dapat mencegah terjadinya deadlock atau race condition yang dapat mengganggu kinerja aplikasi.

#### 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

Terdapat beberapa kelebihan dalam penggunaan `tokio_stream::wrappers::ReceiverStream`. Pertama, ReceiverStream memungkinkan konversi Receiver menjadi Stream dengan mudah. Selain itu, kompatibilitasnya dengan trait Stream memungkinkan penggunaan dengan function dan library lain yang bekerja dengan Stream. ReceiverStream juga secara alami menangani backpressure, mengatur pengiriman pesan agar tidak melampaui kapasitas receiver. Namun, ada beberapa kekurangan. Pertama, Tidak ada mekanisme bawaan untuk menangani kesalahan, yang dapat menjadi tantangan dalam penanganan kesalahan saat menerima pesan. ReceiverStream hanya mendukung komunikasi satu arah dari pengirim ke penerima, dan memberikan kontrol terbatas atas kapan dan bagaimana underlying Receiver dipoll.

#### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Untuk meningkatkan reusability dan modularity  dalam proyek Rust gRPC, struktur kode dapat dibagi ke dalam modul-modul yang terpisah berdasarkan fungsionalitas, dengan pembungkusan fungsi-fungsi umum dan abstraksi interfaces menggunakan generics dan traits. Hal tersebut dapat dilakukan dengan memisahkan business logic dari implementasi gRPC, penggunaan Protobuf untuk mendefinisikan services, dan penggunaan Traits dalam Rust untuk solid abstraction antara services dan concrete implementations. Selain itu, code modularization, dependency injection, dan pemastian error handling juga menjadi faktor penting dalam mencapai tujuan tersebut.

#### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

Untuk menangani logika pemrosesan pembayaran yang lebih kompleks, beberapa langkah tambahan yang mungkin diperlukan adalah:

- **Validasi Input**: Melakukan validasi terhadap data masukan (input) yang diterima untuk memastikan kebenaran dan keamanannya. Contoh: Mengecek apakah amount > 0

- **Error Handling**: Menghandle error cases yang mungkin terjadi selama proses pembayaran, seperti penolakan transaksi, pemrosesan ulang, atau penanganan kegagalan transaks

- **Integrasi dengan Sistem Pembayaran Eksternal** :  Berinteraksi dengan sistem pembayaran eksternal atau penyedia layanan pembayaran untuk memproses transaksi secara nyata. Ini mungkin melibatkan komunikasi jaringan, integrasi dengan API pembayaran, atau bahkan koneksi langsung ke gateway pembayaran.

#### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

Pengadopsian gRPC sebagai protokol komunikasi memiliki dampak signifikan pada arsitektur dan desain sistem terdistribusi. Dalam hal interoperability dengan teknologi dan sistem lain, gRPC memiliki beberapa keuntungan dan tantangan. Keuntungannya adalah kemampuan untuk menghasilkan kode client dan server secara otomatis untuk berbagai bahasa pemrograman, sehingga memudahkan integrasi dengan teknologi yang berbeda. Namun, tantangannya adalah keterbatasan dukungan terhadap protokol komunikasi non-gRPC, yang dapat menyulitkan integrasi dengan sistem yang menggunakan teknologi komunikasi yang berbeda, misalnya berinteraksi dengan sistem yang hanya mendukung protokol REST.

#### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

- **Kelebihan**:
    * **Multiplexing**: HTTP/2 mendukung multiplexing, yang memungkinkan beberapa permintaan dan respons untuk diproses secara paralel melalui satu koneksi. Ini mengurangi latensi dan meningkatkan kinerja, terutama untuk aplikasi dengan banyak permintaan kecil.

    * **Header Compression**: HTTP/2 menggunakan kompresi header, yang mengurangi overhead penggunaan bandwidth dan meningkatkan efisiensi transmisi data.

    * **Server Push**: HTTP/2 memungkinkan server untuk mengirimkan data ke client tanpa permintaan sebelumnya, mengoptimalkan pengiriman konten dan mengurangi waktu muat.

- **Kekurangan**:
    * **Complexity**: Implementasi HTTP/2 lebih kompleks daripada HTTP/1.1, memerlukan lebih banyak sumber daya dan pemahaman tentang konsep-konsep seperti multiplexing dan server push.

    * **Backward Compatibility**: Meskipun dukungan untuk HTTP/2 semakin luas, masih ada client dan server yang tidak mendukung protokol ini secara penuh atau sama sekali. Hal ini dapat menyebabkan masalah kompatibilitas dan membatasi penggunaan HTTP/2 dalam beberapa lingkungan.

    * **Overhead CPU**: Meskipun HTTP/2 mengurangi overhead penggunaan bandwidth dengan menggunakan kompresi header, namun proses kompresi dan dekompresi header dapat meningkatkan beban CPU pada server dan client. Hal ini dapat menjadi masalah dalam lingkungan dengan sumber daya terbatas atau pada perangkat dengan kinerja rendah.

#### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

Model request-response dari REST APIs memungkinkan komunikasi unary, di mana client mengirim permintaan ke server dan menunggu respons dari server. Sementara itu, kemampuan bi-directional dari gRPC memungkinkan komunikasi real-time yang lebih responsif. Dengan gRPC, client dan server dapat mengirim dan menerima data secara langsung tanpa harus menunggu permintaan atau respons sebelumnya selesai, yang memungkinkan respons yang lebih cepat dan pengalaman pengguna yang lebih responsif dalam aplikasi real-time seperti chat dan call.

#### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Pendekatan berbasis skema dari gRPC menggunakan Protocol Buffers, dengan keuntungan ukuran payload yang lebih kecil, validasi data yang ketat, dan kinerja yang lebih baik, berbeda secara signifikan dengan sifat yang lebih fleksibel dan tanpa skema dari JSON dalam payload REST API. Protocol Buffers membutuhkan definisi skema yang ketat sebelum implementasi, menyediakan validasi data yang kuat, dan memungkinkan kinerja yang lebih efisien. Namun, JSON memungkinkan fleksibilitas dalam evolusi struktur data dan penggunaan yang lebih sederhana tanpa skema, meskipun dengan biaya kinerja yang mungkin sedikit lebih tinggi.