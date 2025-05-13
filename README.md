# Advanced Programming - Tutorial 09
## Daniel Ferdiansyah, 2306275052

**a. What is AMQP?**

AMQP (Advanced Message Queuing Protocol) adalah *open standard* untuk *message-oriented middleware*. AMQP digunakan untuk mengirim pesan antar sistem atau aplikasi yang reliable, aman, dan asynchronus. AMQP (sangat) bisa digunakan untuk aplikasi chat real-time, sistem terdistribusi, sampai IOT. Salah satu implementasi AMQP yang populer adalah RabbitMQ yang akan digunakan in the future tutorial ini.

**b. What does guest:guest@localhost:5672 mean?**

guest:guest@localhost:5672 merupakan URI untuk server AMQP. Umumnya, formatnya seperti berikut

`amqp://<username>:<password>@<hostname>:<port>`

Sehingga dari URI tersebut, kita bisa simpulkan 

```python
Protokol: amqp

Username: guest

Password: guest

Host: localhost (komputer lokal)

Port: 5672 (default untuk AMQP)
```

Simuation of slow subscribers
![image](https://github.com/user-attachments/assets/88296aa9-2967-496d-906f-0279b64b892a)

Terlihat *queued messages* mencapai peak sampai 100 queue. Hal ini dikarenakan adanya `thread::sleep` selama satu detik untuk tiap pesan. Akhirnya, subscriber menjadi lambat dan queue meningkat drastis.

Three additional subscribers
![image](https://github.com/user-attachments/assets/ab767777-ba15-40e8-9ee9-4c7617661355)

![image](https://github.com/user-attachments/assets/f6b68a00-bc10-49bd-a069-0e761753e88a)

Terlihat bahwa spike pada queue turun lebih cepat, hal ini terjadi karena load didistribusikan ke 3 subscribers. Sehingga, beban untuk tiap subscribers turun menjadi 1/3 nya saja. Jika tetap dibebani delay selama satu detik untuk tiap pesan, optimasi yang mungkin bisa dilakukan adalah *parallelization* menggunakan Rayon. Sehingga, beberapa pesan bisa diproses secara paralel, misalnya dengan codeblock berikut pada `main` method di `main.rs` subscriber:
```rust
(0..8).into_par_iter().for_each(|_| {
        _ = listener.listen(
            "user_created".to_owned(),
            handler.clone(),
            crosstown_bus::QueueProperties {
                auto_delete: false,
                durable: false,
                use_dead_letter: true,
            },
        );
    });
```


