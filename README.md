# robby-business-stack

Stack bisnis kecil: Postgres + Evolution API (WA Gateway) + n8n (otomatisasi).
Dibuat simpel biar gampang di-tracking polanya.

## Pola (reverse engineering)

```text
n8n (:5678) <--> Evolution API (:8080) <--> WhatsApp
   |
   v
Postgres (:5432)
```

- **n8n** = otaknya. Workflow baca/tulis DB, panggil Evolution API.
- **Evolution API** = tangannya. Kirim/terima pesan WA, simpan session di `instances/`.
- **Postgres** = ingatannya. Simpan pelanggan, jadwal, petugas.

## Versi

- n8n: 2.20.9
- PostgreSQL: 18.4
- Evolution API: v2.3.7

Lihat juga: `versi software.txt`, `docker-compose.yml`.

## Struktur file

Yang masuk repo (aman):

```text
README.md
docker-compose.yml
.gitignore
versi software.txt
```

Yang TIDAK masuk repo (penting, di-ignore):

```text
.env              -> kunci Evolution API, jangan push
instances/        -> session WA, 15MB+, jangan push
credential.json   -> kredensial n8n, jangan push
backup_db_mas.sql -> dump DB berisi data pelanggan, jangan push
My workflow.json  -> workflow n8n 146 nodes, jangan push
pgdata/           -> data Postgres, jangan push
.n8n/             -> data n8n, jangan push
.n8n-files/       -> file n8n, jangan push
```

Kenapa? Semua yang di atas itu kunci / data asli. Pernah bocor ke history, sudah dibersihkan total + repo dibuat ulang. Jangan di-push lagi.

## Cara jalanin

Wajib: Docker + Docker Compose sudah terinstall.

1. Set password Postgres (wajib, tidak ada default di repo):

```bash
export POSTGRES_PASSWORD='ganti_dengan_password_baru_yang_kuat'
```

2. Pastikan file `.env` ada di folder ini (untuk Evolution API, tidak ikut repo).

3. Cek config (tanpa jalanin):

```bash
docker compose config
```

4. Jalanin:

```bash
docker compose up -d
```

5. Cek status:

```bash
docker ps
```

Harus ada 3 container UP: `postgres`, `evolution_api_mas_obi`, `n8n`.

## Port

- Postgres: `5432`
- n8n: `http://localhost:5678`
- Evolution API: `http://localhost:8080` (lihat `SERVER_PORT` di `.env`)

## Perintah harian

```bash
docker ps
docker compose logs -f
docker compose down
docker compose up -d
```

## Keamanan (wajib baca)

1. Jangan pernah `git add` file yang di-ignore di atas.
2. Kalau tidak sengaja ke-add, batalkan dengan `git rm --cached <nama-file>`, jangan `rm`.
3. Semua password / API key / private key yang pernah ke-push dianggap hangus, harus revoke:
   - Password Postgres
   - `AUTHENTICATION_API_KEY` di `.env`
   - Google Service Account key
   - Header Auth key
   - Logout + scan ulang semua session WA di `instances/`
4. Repo ini private saat setup. Boleh public kalau sudah yakin isinya cuma 4 file aman. Verifikasi dengan `git ls-files`.
