# Docker Nginx Reverse Proxy (FastCGI)

Setup ini membuat 1 container Nginx sebagai reverse proxy TLS untuk:
- `talenan.test` -> FastCGI ke `talenan_nginx:9000`
- `hitungin.test` -> FastCGI ke `hitungin_nginx:9000`

## Struktur

- `docker-compose.yml`
- `nginx/nginx.conf`
- `nginx/conf.d/talenan.test.conf`
- `nginx/conf.d/hitungin.test.conf`
- `certs/*.pem`

## Jalankan

1. Buat network bersama (sekali saja):

```bash
docker network create proxy_net
```

2. Jalankan reverse proxy:

```bash
docker compose up -d
```

## Integrasi project lain

Pastikan service target (`talenan_nginx`, `hitungin_nginx`) ada di network `proxy_net` dan listen FastCGI di port `9000`.

Contoh di compose project:

```yaml
services:
  talenan_nginx:
    image: php:8.3-fpm
    container_name: talenan_nginx
    networks:
      - proxy_net

networks:
  proxy_net:
    external: true
```

## Catatan penting

Jika service target sebenarnya adalah Nginx/HTTP (bukan PHP-FPM/FastCGI), ganti `fastcgi_pass` menjadi `proxy_pass http://<service>:80;`.
