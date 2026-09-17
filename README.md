# robby-business-stack

Catatan buat gue sendiri. Stack bisnis kecil: Postgres + Evolution API (WA) + n8n.
Gue bikin simpel biar gue gampang tracking polanya.

## Pola yang gue pake

```text
n8n (:5678) <--> Evolution API (:8080) <--> WhatsApp
   |
   v
Postgres (:5432)
```

- n8n = otaknya. Workflow gue baca/tulis DB, panggil Evolution API.
- Evolution API = tangannya. Buat kirim/terima WA, session kesimpen di `instances/`.
- Postgres = ingatannya. Nyimpen pelanggan, jadwal, petugas.

## Versi yang gue pake

- n8n: 2.20.9
- PostgreSQL: 18.4
- Evolution API: v2.3.7

Detail ada di `versi software.txt` sama `docker-compose.yml` kalau gue lupa.

## File-file gue

Yang boleh masuk repo (aman):

```text
README.md
docker-compose.yml
.gitignore
versi software.txt
```

Yang jangan sampe masuk repo (pengingat buat gue, udah di-ignore):

```text
.env              -> kunci Evolution API gue
instances/        -> session WA gue, 15MB+
credential.json   -> kredensial n8n gue
backup_db_mas.sql -> dump DB isi data pelanggan gue
My workflow.json  -> workflow n8n gue (146 nodes)
pgdata/           -> data Postgres gue
.n8n/             -> data n8n gue
.n8n-files/       -> file n8n gue
```

Pelajaran buat gue: semua di atas itu pernah bocor ke history. Udah gue bersihin total + bikin repo ulang. Jangan sampe kepush lagi.

## Kalau gue mau jalanin lagi

Docker + Compose harus kepasang dulu ya Rob.

1. Gue harus set password dulu (nggak ada default di repo):

```bash
export POSTGRES_PASSWORD='ganti_dengan_password_baru_yang_kuat'
```

2. Pastiin `.env` ada di folder ini (buat Evolution, nggak ikut repo).

3. Cek config dulu (nggak langsung jalanin):

```bash
docker compose config
```

4. Baru jalanin:

```bash
docker compose up -d
```

5. Cek:

```bash
docker ps
```

Harusnya ada 3 yang UP: `postgres`, `evolution_api_mas_obi`, `n8n`. Kalau kurang, berarti ada yang mati.

## Port langganan gue

- Postgres: `5432`
- n8n: `http://localhost:5678`
- Evolution: `http://localhost:8080` (cek `SERVER_PORT` di `.env` kalau lupa)

## Perintah yang sering gue pake

```bash
docker ps
docker compose logs -f
docker compose down
docker compose up -d
```

## Pengingat keamanan buat gue

1. Rob, jangan pernah `git add` file yang di-ignore di atas.
2. Kalau nggak sengaja ke-add, batalin pake `git rm --cached <nama-file>`, bukan `rm`.
3. Semua kunci yang pernah kepush anggap aja hangus, harus gue revoke:
   - Password Postgres gue
   - `AUTHENTICATION_API_KEY` di `.env` gue
   - Google Service Account key gue
   - Header Auth key gue
   - Logout + scan ulang semua session WA di `instances/` gue
4. Repo ini gue bikin private pas setup. Kalau mau gue public-in, pastiin dulu isinya cuma 4 file aman. Cara cek: `git ls-files`.
