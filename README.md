# tellda-site

Static site for [`tellda.gottila.com`](https://tellda.gottila.com) — the Tell Da Android app's marketing + legal pages.

**Hosting:** self-hosted on the Pi via `nginx:alpine` in Docker, fronted by a Cloudflare Tunnel. Matches the WeightSync pattern (one container per site).

## Files

| File | URL on live site |
|---|---|
| `index.html` | `/` |
| `privacy_policy.html` | `/privacy_policy.html` |
| `delete_my_account.html` | `/delete_my_account.html` |
| `docker-compose.yml` | Deployment config — only used on the Pi |
| `CNAME` | (legacy — leftover from earlier GitHub Pages plan, harmless) |

## First-time deploy on the Pi

```bash
mkdir -p ~/tellda-host && cd ~/tellda-host
git clone https://github.com/anikan666/tellda-site.git
cd tellda-site
docker compose up -d
curl -I http://localhost:5053/
```

Then in the **Cloudflare Zero Trust dashboard** → Networks → Tunnels → your tunnel → Public Hostname → Add:
- **Subdomain:** `tellda`
- **Domain:** `gottila.com`
- **Service Type:** `HTTP`
- **URL:** `localhost:5053`

## Updating the site

Edit locally in WSL (or wherever the repo is checked out), `git push`, then on the Pi:

```bash
cd ~/tellda-host/tellda-site && git pull
# No restart needed — nginx serves files directly from the bind-mounted volume.
```

## Source of truth for legal pages

The canonical source for the privacy + deletion HTML is the main Tell Da app repo at `app/docs/`. After editing there:

```bash
cp ~/tellda/app/docs/privacy_policy.html ~/tellda-site/
cp ~/tellda/app/docs/delete_my_account.html ~/tellda-site/
cd ~/tellda-site && git add -A && git commit -m "sync legal pages from app repo" && git push
```

Then `git pull` on the Pi (above).
