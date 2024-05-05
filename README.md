# Tutorial 10 - Asynchronous Programming - Broadcast Chat

##### Winoto Hasyim - 2206025243 - Pemrograman Lanjut B

## Experiment 2.1: Original code, and how it run

![Experiment 2.1: Original code, and how it run](https://i.imgur.com/zJTRiJH.png)

How to run it:
- Server: `cargo run --bin server`
- Client: `cargo run --bin client`

Ketika text di-input di salah satu client, maka akan dibaca oleh `stdin.next_line()` di `client.rs`. Kemudian text tersebut dikirim ke server sebagai WebSocket message menggunakan `ws_stream.send(Message::text(line.to_string())).await?`. Di `server.rs`, function `ws_stream.next()` akan menerima message tersebut. Messagenya kemudian akan dikirim ke `bcast_tx` channel, dan ketika channel tersebut menerima messagenya, maka message tersebut akan dikirim ke semua client yang terhubung dengan channel tersebut menggunakan `ws_stream.send(Message::text(msg?)).await?`

## Experiment 2.2: Modifying port

![Experiment 2.2: Modifying port](https://i.imgur.com/GMsm7PK.png)

Code yang harus di-modify:
- `client.rs`:
![client.rs](https://i.imgur.com/QiJb3Jf.png)
- `server.rs`:
![server.rs](https://i.imgur.com/Qnwq2gl.png)


Karena port pada `client.rs` diubah menjadi `8080`, agar terjalin suatu koneksi antara client dan server maka port pada `server.rs` harus diubah menjadi `8080`. Dengan ini, message akan dikirim oleh salah satu client ke server, dan kemudian server akan mengembalikan message tersebut ke semua client yang terhubung melalui port yang sama.

## Experiment 2.3: Small changes, add IP and Port

![Experiment 2.3: Small changes, add IP and Port](https://i.imgur.com/3Z5YnpT.png)

Changes:
![Change 1](https://i.imgur.com/J2cQk3F.png)
![Change 2](https://i.imgur.com/IU0wnPc.png)

Ketika sebuah koneksi baru diterima oleh `TcpListener` di function `main` pada `server.rs`, IP address dan port dari client akan diteruskan sebagai argumen ke function `handle_connection` dengan nama `addr`. `addr` ini bisa dipakai untuk menyertakan IP address dan port dari client ketika memformat message yang dikirim ke client, dan juga ketika memformat message yang diterima dari client sebelum message tersebut dikirim ke semua client yang terhubung pada channel. Ini adalah alasan mengapa perubahan kode hanya dilakukan di `server.rs`