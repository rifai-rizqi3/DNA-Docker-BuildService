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
> Masuk ke file docker compose nya biasanya format nya .yml
> Tambahkan code service di dalam yml 
```
payment:
    build: /root/simotor/service_dev/payment
    container_name: simotor_service_payment
    image: simotor_service_payment
    ports:
      - 8825:8825 < Port Static
    restart: always
    networks:
      simotor:
        ipv4_address: 172.20.0.25 < IP Static
    volumes:
      - "/etc/timezone:/etc/timezone:ro"
      - "/etc/localtime:/etc/localtime:ro"
      - "/root/simotor/service_dev/payment:/logs"
```
> Lalu setting haproxynya di folder haproxy
> Tambahkan code di file config haproxy
```
acl PATH_payment path_beg -i /api/payment
```
```
use_backend backend_payment if  PATH_payment
```
```
backend backend_payment
 balance roundrobin
    option forwardfor
    http-request set-header X-Forwarded-Port %[dst_port]
    http-request add-header X-Forwarded-Proto https if { ssl_fc }
    server s1 172.20.0.25:8825 check
```
> Lalu build compose nya
```
docker-compose up -d --build <nama-folder>
```
> Pastikan berhasil dengan perintah
```
docker-compose logs -f <nama-folder>
```
> Lalu restart haproxynya
```
docker restart <namacontainerlengkap>
```
> Finish!
# Note
Perintah penting docker :
```
Melihat list Container      	    : docker ps -a
Melihat List Container Status       : docker container ls -a
Melihat Image Docker                : docker images
Stop service 						: docker-compose stop <nama-folder>
Build Service 						: docker-compose up -d --build <nama-folder>
Log									: docker-compose logs -f <nama-folder>
restart haproxy	    				: docker restart <namacontainerlengkap>
```
