# Dokploy WordPress Hybrid Stack

Stack ini berisi:
- WordPress 6.9.4 + PHP 8.3 Apache
- MariaDB 10.11
- Redis 7.2 untuk object cache
- Adminer untuk utility database

## Isi repo
- `docker-compose.yml`
- `Dockerfile.wp`
- `files/uploads.ini`
- `files/servername.conf`
- `.env.example`

## Langkah deploy di Dokploy
1. Upload repo ini ke GitHub.
2. Copy `.env.example` menjadi `.env` lalu isi credential sesuai kebutuhan.
3. Buat project baru di Dokploy dari repo GitHub.
4. Pastikan build context membaca root repo.
5. Deploy.

## Catatan
- WordPress memakai volume `wp_app`, database di `wp_db`, Redis di `wp_redis`.
- `DISABLE_WP_CRON` aktif, jadi idealnya buat cron eksternal tiap 5 menit.
- Adminer sebaiknya jangan dibuka publik tanpa proteksi tambahan.
- Redis di sini diset untuk object cache, bukan persistent data store utama.

## Cron WordPress
Contoh cron eksternal:

```bash
*/5 * * * * docker exec <container_wp> php /var/www/html/wp-cron.php > /dev/null 2>&1
```

## Saran hardening
- Letakkan di balik reverse proxy HTTPS.
- Batasi akses Adminer.
- Gunakan password DB yang kuat.
- Aktifkan plugin Redis Object Cache di WordPress.
