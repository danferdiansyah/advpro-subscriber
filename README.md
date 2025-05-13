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
