# Docker-BuildService
Tutorial Membuat Service baru di docker dengan compose dan haproxy di Simotor.

> Buat folder baru contoh payment
> Buat folder Config dan buat json didalamnya
```
{
    "port": <"port service nya contoh :8825">,
    "enableGinConsoleLog": true,
    "enableGinFileLog": true,

    "logFilename": "logs/server.log",
    "logMaxSize": 10,
    "logMaxBackups": 10,
    "logMaxAge": 30,

    "jwtSecretPassword": "decandTampan",
    "issuer": "dnadeveloper",

    "db_addrs_transaksi": "172.22.0.2",
    "db_port_transaksi": "5432",
    "db_name_transaksi": "simotor_transaksi",
    "db_username_transaksi": <"username db">,
    "db_password_transaksi": <"password db">,

    "xendit_key" : <Isi keygen xendit>

}
```
> Buat file Dockerfile
```
FROM ubuntu:18.04
COPY payment payment
COPY config config
COPY logs logs
CMD ["./payment"]

```
