# Tutorial 10 - Asynchronous Programming - Broadcast Chat

##### Winoto Hasyim - 2206025243 - Pemrograman Lanjut B

## Experiment 2.1: Original code, and how it run

![Experiment 2.1: Original code, and how it run](https://i.imgur.com/zJTRiJH.png)

How to run it:
- Server: `cargo run --bin server`
- Client: `cargo run --bin client`

Ketika text di-input di salah satu client, maka akan dibaca oleh `stdin.next_line()` di `client.rs`. Kemudian text tersebut dikirim ke server sebagai WebSocket message menggunakan `ws_stream.send(Message::text(line.to_string())).await?`. Di `server.rs`, function `ws_stream.next()` akan menerima message tersebut. Messagenya kemudian akan dikirim ke `bcast_tx` channel, dan ketika channel tersebut menerima messagenya, maka message tersebut akan dikirim ke semua client yang terhubung dengan channel tersebut menggunakan `ws_stream.send(Message::text(msg?)).await?`