# n8n SQLite backups

Nightly online backup of the production n8n database (`database.sqlite` in the `n8n_data` volume) **without stopping n8n**.

| File | Installed at | Purpose |
|---|---|---|
| `n8n-backup` | `/usr/local/bin/n8n-backup` | Backup script |
| `n8n-backup.service` | `/etc/systemd/system/` | One-shot unit that runs the script as `ec2-user` |
| `n8n-backup.timer` | `/etc/systemd/system/` | Runs it daily at 09:30 UTC (03:30 America/Mexico_City) |

## How it works

- It uses SQLite's online backup API (`node:sqlite`), which takes a consistent snapshot even while n8n is writing, including changes still in the `-wal` file. A plain `cp` of a live WAL database is not safe.
- It runs in a throwaway container from **the same image n8n is running**, with no network and as uid 1000 (`node`). No packages are needed on the host, and the ownership of files in the volume never changes.
- Each backup is checked with `PRAGMA quick_check`, gzipped, and written to `~/n8n-backups/daily/` with the instance's `config` file.
- Backups older than 14 days are deleted.

> **The `.config` file is the encryption key.** Without it, every stored credential in the backup is unreadable. Keep it with the `.sqlite.gz`, and treat both as secrets.

## Install

```bash
sudo install -m 755 deploy/backup/n8n-backup /usr/local/bin/n8n-backup
sudo install -m 644 deploy/backup/n8n-backup.service deploy/backup/n8n-backup.timer /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl start n8n-backup.service      # run once now and check it works
journalctl -u n8n-backup -n 5 --no-pager     # expect: "ok workflows=N credentials=N"
sudo systemctl enable --now n8n-backup.timer
```

## Check

```bash
systemctl list-timers n8n-backup.timer
ls -lh ~/n8n-backups/daily/
```

## Restore

Restoring replaces the live database, so take a fresh backup first.

```bash
cd ~/n8n
B=~/n8n-backups/daily/n8n-YYYYMMDD-HHMMSS     # pick one; no extension
docker-compose -f docker-compose.production.yml stop n8n
docker run --rm -v n8n_data:/data -v "$(dirname $B)":/b:ro alpine sh -c "
  rm -f /data/database.sqlite /data/database.sqlite-wal /data/database.sqlite-shm &&
  gunzip -c /b/$(basename $B).sqlite.gz > /data/database.sqlite &&
  cp /b/$(basename $B).config /data/config &&
  chown 1000:1000 /data/database.sqlite /data/config && chmod 600 /data/config"
docker-compose -f docker-compose.production.yml up -d --no-deps n8n
```

To test a backup without touching production, restore it into a scratch volume instead of `n8n_data`. Start a container from it with `--network none`, so its workflows can't fire webhooks, polls or messages. Then run `n8n export:credentials --all --decrypted`. If every credential decrypts, the database and the key match.
