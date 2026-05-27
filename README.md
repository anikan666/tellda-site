# tellda-site

Static site for [`tellda.gottila.com`](https://tellda.gottila.com) — the Tell Da Android app's marketing + legal pages.

Hosted via **GitHub Pages** with a Cloudflare-DNS CNAME pointing `tellda.gottila.com` → `anikan666.github.io`.

## Files

| File | Path on live site |
|---|---|
| `index.html` | `/` |
| `privacy_policy.html` | `/privacy-policy.html` (and `/privacy_policy.html`) |
| `delete_my_account.html` | `/delete-my-account.html` (and `/delete_my_account.html`) |
| `CNAME` | (consumed by GitHub Pages for custom domain) |

## Updating the legal pages

The canonical source for the legal pages is in the main Tell Da app repo at `app/docs/`. After editing there, copy the two HTML files into this repo and push:

```bash
cp ~/tellda/app/docs/privacy_policy.html .
cp ~/tellda/app/docs/delete_my_account.html .
git add -A && git commit -m "sync legal pages from app repo" && git push
```

GitHub Pages redeploys automatically on push to `main`.
