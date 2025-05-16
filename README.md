# SoftwareArchitectures-Publisher
# Tutorial 9

> How much data your publisher program will send to the message broker in one run?

Program mengirimkan 5 event, masing-masing bertipe UserCreatedEventMessage yang memiliki dua field: user_id dan user_name.

> The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?

Publisher dan Subscriber mengakses url yang sama agar Keduanya terhubung ke message broker yang sama

guest:guest → username dan password default RabbitMQ.
localhost → berarti broker RabbitMQ dijalankan di komputer lokal.
5672 → port default untuk protokol AMQP
Publisher mengirim pesan ke broker, dan subscriber menerima pesan dari broker yang sama

#### Screenshot RabbitMQ
![Screenshot tampilan RabbitMQ](/images/1_running_RabbitMQ.png)

#### Sending and processing event
![CargoRunSubscriber](/images/2_cargorun_subscriber.png)
![CargoRunPublisher](/images/3_cargorun_publisher.png)
Saat menjalankan perintah cargo run pada program publisher, aplikasi akan mengirim lima buah event bertipe UserCreatedEventMessage ke message broker RabbitMQ. Setiap event memuat data berupa user_id dan user_name, lalu dikirim ke queue bernama user_created yang telah dikonfigurasi di server RabbitMQ. Sementara itu, program subscriber yang sudah aktif akan mendengarkan queue yang sama, yaitu user_created. Ketika event diterima, subscriber akan memproses setiap pesan tersebut menggunakan UserCreatedHandler. Setelah diproses, informasi akan ditampilkan di konsol.

#### Monitoring chart based 
![Screenshot Monitoring chart](/images/4_RabbitMQ_run.png)
Spike pada grafik RabbitMQ terjadi karena setiap kali program publisher dijalankan, ia langsung mengirim lima pesan secara berurutan ke broker dalam waktu yang sangat singkat. Hal ini menyebabkan laju pengiriman pesan (publish rate) melonjak tajam dalam sekejap.

#### Simulation slow subscriber
![Simulation slow subscriber](/images/5_RabbitMQ_slow.png)
Lonjakan ini terjadi karena program publisher dijalankan sebanyak 4 kali dalam waktu yang berdekatan, dan setiap eksekusi publisher langsung mengirim 5 pesan secara beruntun ke RabbitMQ. Artinya, terdapat total 20 pesan yang masuk ke queue dalam waktu singkat. Karena subscriber memiliki delay pemrosesan sebesar 10 ms per pesan, maka tidak semua pesan dapat langsung diproses. Akibatnya, sebagian pesan menumpuk di dalam antrean. Setelah subscriber mulai memproses pesan satu per satu, laju consume naik hingga akhirnya antrean habis dan grafik kembali menurun.

#### Running at least three subscribers

![ManySubscribers1](/images/6_RabbitMQ_manysubs.png)
![ManySubscribers2](/images/7_cargorun_manysubs.png)
Saya menjalankan 3 subscriber secara paralel di terminal berbeda dan setiap subscriber terhubung ke queue yang sama di RabbitMQ. Ketika publisher dijalankan sebanyak 4 kali maka ia akan dilakukan pemrosesan message secara merata ke ketiga subscriber. Pembagian ini membuat pemrosesan message menjadi lebih cepat sesuai yang ditampilkan pada spike queued messages dibandingkan dengan menggunakan 1 subscriber seperti yang dilakukan sebelumnya.