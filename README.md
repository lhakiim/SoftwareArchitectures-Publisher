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

