# Reflection Module 9 gRPC

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
    - RPC unary, klien mengirimkan satu permintaan ke server dan menerima satu respons. Metode ini cocok untuk operasi sederhana dan satu kali di mana klien perlu mengambil data atau melakukan suatu tindakan tanpa memerlukan komunikasi berkelanjutan. Contohnya termasuk mengambil informasi pengguna atau mengirimkan formulir.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
    -  Potensi keamanan yang perlu dikonsiderasi adalah dalam penggunaan gRPC, setiap bagian data yang dikirim memerlukan proses validasi otentikasi dan otorisasi untuk menjaga keamanannya. Berbeda dengan REST yang hanya membutuhkan validasi sekali per permintaan, gRPC membutuhkan validasi berulang kali untuk setiap stream permintaan. Ketika datanya dienkripsi, baik server maupun klien harus mengenkripsi setiap bagian data secara terpisah untuk menjaga kerahasiaan data.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
    - Tantangan yang kemungkinan muncul adalah menghindari race condition dan memastikan sinkronisasi data yang akurat. Selain itu, koneksi yang berlangsung lama bisa menyulitkan load balancing karena banyak koneksi yang tidak dapat diputus, memberatkan server.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
    - Keuntungan yang diperoleh adalah mempercepat pengolahan data dengan asinkronus dan mudahnya integrasi dengan modul tokio lainnya. Lalu, kerugian yang diperoleh adalah program menjadi lebih kompleks dan autentikasi/autorisasi perlu dilakukan untuk tiap data.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
    - Rust gRPC menyediakan metode untuk meningkatkan kemudahan pemeliharaan dan fleksibilitas karena perubahan dan penambahan fitur dapat dilakukan dengan lebih mudah. Dengan menggunakan proto, kita membuat sebuah antarmuka yang dapat diimplementasikan oleh kelas dalam Rust, memfasilitasi perluasan kode.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
    - Pada implementasi MyPaymentService, untuk handle proses pembayaran yang kompleks dapat dibuat menjadi server streaming dan tidak lagi unary agar pengiriman data-data yang kompleks dapat dilakukan dengan lebih cepat dan mengurangi overhead pembuatan koneksi antara client dan server.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
    - Dengan penggunaan gRPC, kita tidak perlu lagi memikirkan bagaimana suatu modul dapat diakses dengan metode HTTP secara langsung. Hal ini dikarenakan gRPC secara otomatis menangani koneksi dan pemanggilan method yang diinginkan, didasarkan pada pemahaman yang sama yang didefinisikan dalam file proto. Dengan demikian, klien dapat dengan mudah memanggil fungsi-fungsi dari server, menyederhanakan konektivitas dan operasi antara berbagai teknologi, platform, dan sistem yang terdistribusi.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
    - HTTP/2 adalah mampu untuk melakukan banyak permintaan dan respon dalam satu koneksi tanpa perlu menutup koneksi. Namun, kerugiannya dibandingkan dengan HTTP/1.1 terletak pada biaya overhead yang lebih tinggi dalam kinerja dan penggunaan memori karena HTTP/2 mempertahankan satu koneksi untuk banyak permintaan dan respon. Hal ini dapat mengakibatkan biaya yang lebih besar untuk pengiriman data kecil dibandingkan dengan HTTP/1.1, meskipun HTTP/2 lebih efisien untuk pengiriman data yang besar.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
    - REST APIs memerlukan waktu lebih lama kebanding gRPC untuk real-time communication. Pada REST APIs tiap pengiriman request diperlukan pembuatan koneksi baru dari client ke server dan untuk gRPC hanya memerlukan 1 koneksi sampai semua request selesai. Oleh karena itu, gRPC lebih optimal untuk real-time communication and responsiveness kebanding REST APIs karena kecepatannya yang lebih baik.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
    - Protocol Buffers mengharuskan pengguna menentukan skema data secara eksplisit melalui file proto, yang kemudian digunakan untuk menghasilkan kode stub untuk klien dan server. Ini menghasilkan kode yang lebih terstruktur dan ketat, memungkinkan validasi data yang ketat dan penanganan error yang lebih baik. Proses serialisasi dan deserialisasi data juga berjalan otomatis, memastikan penggunaan data yang terjamin dan konsisten dalam tipe data.
    - Sebaliknya, JSON dalam REST API tidak memerlukan definisi skema yang kaku, memberikan kebebasan dalam struktur data. Namun, hal ini juga dapat menyebabkan ketidakpastian dalam format data dan kesulitan dalam validasi data secara konsisten di seluruh aplikasi. Dampaknya, ada risiko bahwa data yang dikirimkan mungkin tidak dapat digunakan. Dengan demikian, meskipun gRPC dengan Protocol Buffers memberikan kejelasan dan validasi yang ketat, REST API dengan JSON menawarkan fleksibilitas yang lebih besar dalam pertukaran data.