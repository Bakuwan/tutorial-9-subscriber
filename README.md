## a. What is amqp?

AMQP (Advanced Message Queuing Protocol) adalah protokol untuk messaging middleware yang memungkinkan aplikasi bertukar pesan secara asynchronous dan reliable. Protokol ini menjadi jembatan komunikasi antar komponen yang terdistribusi dalam sistem perangkat lunak. AMQP banyak digunakan dalam arsitektur event-driven dan microservices. Pesan-pesan dikirim ke message broker seperti RabbitMQ untuk diatur pengantaran ke konsumen. Broker ini mendukung pola komunikasi point-to-point maupun publish-subscribe.

## b. Apa arti dari format guest:guest@localhost:5672, apa maksud dari guest yang pertama, guest yang kedua, dan localhost:5672?

Format guest:guest@localhost:5672 adalah string koneksi (connection string) yang dipakai untuk mengakses message broker AMQP (misalnya RabbitMQ). Penjelasannya adalah sebagai berikut:

guest yang pertama adalah username yang digunakan untuk login ke broker.

guest yang kedua adalah password untuk username tersebut.

localhost:5672 adalah alamat server broker dan port yang dipakai, di mana:

localhost berarti broker berjalan di komputer yang sama dengan aplikasi (localhost = IP 127.0.0.1).

5672 adalah port standar default yang digunakan RabbitMQ untuk koneksi AMQP.

Jadi, string ini memberitahu aplikasi "gunakan username guest dengan password guest untuk mengakses RabbitMQ yang berjalan di mesin lokal pada port 5672."


------
Saat melakukan simulasi slow subscriber, jumlah pesan di antrean mencapai 25 karena subscriber memproses pesan lebih lambat dibandingkan kecepatan publisher mengirim pesan. Pesan-pesan baru terus masuk ke queue, namun subscriber belum selesai memproses pesan sebelumnya sehingga terjadi penumpukan.

![image](static\images\image.png)

------
Setelah menjalankan tiga subscriber secara bersamaan, grafik tanpa queued messages muncul karena ketiga subscriber tersebut mampu berbagi beban konsumsi pesan secara efektif. Dengan adanya tiga konsumen yang aktif, pesan-pesan yang dikirim publisher dapat segera diambil dan diproses secara paralel, sehingga tidak ada penumpukan di antrean. Kecepatan gabungan ketiga subscriber dalam memproses pesan lebih tinggi daripada kecepatan penerbitan pesan, sehingga antrean tetap kosong.

![image](static\images\image1.png)
![image](static\images\image2.png)
![image](static\images\image3.png)
![image](static\images\image4.png)

---
### c. Take a look at the code of publisher and subscriber, do you see something to improve?
Pertama, method get_handler_action() belum diimplementasi sehingga bisa menyebabkan panic jika dipanggil. Kedua, penggunaan unwrap() pada koneksi berisiko membuat program crash jika broker tidak tersedia. Oleh karena itu, sebaiknya ditambahkan error handling yang lebih aman untuk menghindari kegagalan mendadak. Ketiga, aplikasi saat ini tidak memiliki mekanisme reconnect otomatis jika koneksi terputus. Menambahkan fitur reconnect akan meningkatkan kestabilan aplikasi.