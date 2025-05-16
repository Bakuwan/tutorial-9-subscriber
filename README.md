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