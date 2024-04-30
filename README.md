> Fari Hafizh Ramadhan - 2206083691

# Modul 9: High Level Networking

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
   - Pada **Unary RPC**, klien mengirimkan permintaan ke server dan kemudian menunggu respon dari server. Cocok untuk kasus di mana klien hanya membutuhkan satu kali respons dari server untuk setiap permintaan yang dikirim.
   - Pada **Server Streaming RPC**, klien mengirimkan permintaan ke server, dan server mengirimkan respon dalam bentuk data stream yang berkelanjutan. Cocok digunakan ketika klien memerlukan respons yang terdiri dari serangkaian data yang diberikan secara bertahap oleh server.
   - Pada **Bi-directional Streaming RPC**, baik klien maupun server dapat mengirim dan menerima data secara bersamaan melalui saluran komunikasi yang sama. Cocok untuk skenario di mana komunikasi antara klien dan server memerlukan pertukaran data yang kontinu dan simultan.
<br><br>
2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
   - **Authentication**, Penting untuk memastikan bahwa hanya klien yang sah yang dapat mengakses layanan gRPC. Rust menggunakan berbagai mekanisme otentikasi, seperti token JWT (JSON Web Tokens), TLS (Transport Layer Security), OAuth2.
   - **Authorization**, Setelah mengotentikasi klien, perlu dipastikan bahwa klien tersebut memiliki izin yang sesuai untuk mengakses sumber daya atau melakukan operasi tertentu di layanan gRPC. Rust menyediakan berbagai pustaka yang dapat membantu dalam mengimplementasikan logika otorisasi, seperti actix-web atau rocket.
   - **Data Encryption**, Sangat penting untuk mengenkripsi data yang dikirimkan antara klien dan server untuk melindungi kerahasiaan dan integritas data. Secara default gRPC menggunakan HTTP/2, yang mendukung enkripsi melalui TLS.
<br><br>
3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?<br>
Beberapa isu yang mungkin timbul adalah ketika mengelola pembacaan dan penulisan stream, terutama dalam menghadapi pemutusan koneksi yang tak terduga dan memastikan koneksi dapat dipulihkan dengan cepat karena klien dapat terhubung dan terputus kapan pun. Selain itu, manajemen konkurensi (sinkronisasi pesan) juga menjadi tantangan, karena banyak klien dapat mengirim dan menerima pesan secara bersamaan. Oleh karena itu, penting untuk mengelola respons dari berbagai klien dengan cermat.
<br><br>
4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?<br>
**Kelebihan:**
   - ReceiverStream menyediakan antarmuka yang mudah digunakan untuk mengubah tokio::sync::mpsc::Receiver menjadi stream yang dapat digunakan dalam konteks asinkron.
   - penggunaan ReceiverStream memanfaatkan infrastruktur yang sama dengan Tokio, sehingga kompatibel.

   **Kekurangan**:
   - ReceiverStream mungkin tidak efisien dalam mengelola banyak koneksi secara bersamaan karena penggunaannya membutuhkan satu alur kerja Tokio untuk setiap koneksi.
   - Jika terjadi error ketika menerima message, ReceiverStream tidak memiliki built-in mechanism yang dapat membantu mengatasi hal tersebut.
<br><br>
5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?<br>
   Kita bisa mengoptimalkan kode Rust gRPC untuk bisa digunakan kembali dan meningkatkan modularitas dengan berbagai cara. Salah satunya adalah dengan memisahkan modul antara kode Rust gRPC dengan logika bisnisnya. Selain itu, kita bisa memanfaatkan library eksternal untuk fungsi umum yang bisa digunakan kembali. Menggunakan traits dan generics juga bisa membantu membuat komponen yang fleksibel, yang pada gilirannya meningkatkan kemudahan pemeliharaan dan kemampuan untuk dikembangkan lebih lanjut.
<br><br>
6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
Beberapa hal yang mungkin bisa ditambahkan:
   - Lakukan validasi tambahan terhadap data pembayaran.
   - Buat mekanisme untuk melacak dan mengelola transaksi pembayaran.
   - Interaksi dengan database secara langsung untuk mengecek balance user, update balance setelah payment, dan merecord transaction tersebut.
<br><br>
7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
   Penggunaan gRPC mempermudah interkoneksi antar modul dengan mengelola panggilan metode antarsistem secara otomatis melalui definisi yang ada dalam file proto. Hal ini mengurangi kebutuhan untuk melakukan konfigurasi metode HTTP secara manual. Karena gRPC menggunakan protokol HTTP/2, yang mendukung komunikasi yang cepat dan efisien antara layanan, termasuk kemampuan streaming yang handal dan overhead yang minim, maka interaksi antarmodul menjadi lebih efisien.
<br><br>
8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
**Kelebihan:**
   - Multiplexing: HTTP/2 mendukung multiplexing, yang memungkinkan multiple requests dan responses untuk dikirimkan secara bersamaan di satu koneksi TCP. 
   - Header Compression: HTTP/2 menggunakan kompresi header, yang mengurangi ukuran header yang dikirim dalam setiap permintaan, menghemat bandwidth dan meningkatkan efisiensi jaringan.
   - Server Push: HTTP/2 mendukung server push, yang memungkinkan server untuk mengirimkan sumber daya ke klien tanpa permintaan sebelumnya.
   - Prioritization: HTTP/2 memungkinkan klien untuk memberikan prioritas terhadap permintaan yang lebih penting.

   **Kekurangan**
   - Implementasi yang Kompleks: Implementasi HTTP/2 bisa lebih kompleks daripada HTTP/1.1, terutama karena fitur-fitur seperti multiplexing dan kompresi header.
   - Keterbatasan Jaringan Lama: Meskipun HTTP/2 dirancang untuk meningkatkan kinerja di jaringan modern, penggunaan multiplexing dapat memperburuk kinerja di jaringan yang lambat atau tidak stabil karena adanya overhead yang meningkat.
<br><br>
9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
   REST API beroperasi dengan pola respons-request, dimana klien mengirim permintaan ke server dan server memberikan respons dengan data yang diminta. Berbeda dengan itu, gRPC menyediakan dukungan untuk streaming dua arah, yang memungkinkan klien dan server untuk bertukar pesan secara asinkron melalui satu koneksi. Hal ini memungkinkan komunikasi real-time di mana keduanya dapat bertukar data tanpa harus menunggu permintaan atau respons.
<br><br>
10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
    Pendekatan gRPC yang berbasis skema dengan menggunakan Protocol Buffers memberikan validasi data yang ketat, otomatisasi kode, dan kecocokan yang kuat antara client dan server. Di sisi lain, JSON dalam REST API menawarkan fleksibilitas yang lebih besar, memungkinkan penanganan data yang lebih dinamis dan perubahan struktur tanpa perlu pembaruan skema atau kode. Pilihan antara keduanya tergantung pada kebutuhan spesifik masing-masing pengguna.