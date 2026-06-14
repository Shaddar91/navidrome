# Navidrome (local self-hosted)

Self-hosted, Subsonic-compatible music streamer ("personal Spotify"). Runs locally via
docker compose. Web player on http://127.0.0.1:4533 (loopback only).

## Run

1. Drop your audio into the in-project `./music/` folder (the default mount target). To
   point at an external library instead, set `ND_MUSIC_HOST_DIR` to an absolute path.
2. `docker compose up -d`
3. Open http://127.0.0.1:4533 - the FIRST account you create becomes the admin
   (there is no admin password env var). Create it before doing anything else.

## Configuration (optional)

The stack runs with sensible defaults. To override, create a local `.env` (git-ignored,
never committed) with any of:

- `ND_MUSIC_HOST_DIR` - host path to the music library (default `./music`)
- `ND_LOGLEVEL` - `error|warn|info|debug|trace` (default `info`)
- `ND_SCANNER_SCHEDULE` - library rescan cadence, duration or cron (default `1h`)
- `ND_SESSIONTIMEOUT` - web UI idle session timeout (default `48h`)
- `ND_PASSWORDENCRYPTIONKEY` - optional long random string to encrypt stored
  external-service credentials with a key you control (leave blank to skip)

## Notes

- Data (`./navidrome-data`) holds the embedded SQLite DB (users, playlists, play counts).
  It MUST persist - it is bind-mounted read-write. There is no separate database.
- Your music (`/music`) is mounted READ-ONLY (`:ro`) - Navidrome never writes to it.
- Image is pinned to `deluan/navidrome:0.61.2`. Do not switch to `:latest`/`:develop`.
- Telemetry is disabled (`ND_ENABLEINSIGHTSCOLLECTOR=false`).
- LOCAL ONLY: the port is bound to 127.0.0.1. Do NOT expose this to the internet
  without a reverse proxy + TLS.
