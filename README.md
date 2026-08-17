# doparise.com

Static site for the DopaRise app: landing page, password-reset fallback, and
verified App / Universal Links.

## Public URLs

| File | URL |
| --- | --- |
| `index.html` | `https://doparise.com/` |
| `reset-password.html` | `https://doparise.com/reset-password` |
| `.well-known/assetlinks.json` | `https://doparise.com/.well-known/assetlinks.json` |
| `.well-known/apple-app-site-association` | `https://doparise.com/.well-known/apple-app-site-association` |

The AASA file has **no** `.json` extension. Hosts must serve it as
`Content-Type: application/json` with **no redirect**.

## Deploy

This repo is a static site. Connect it to **Vercel**, **Netlify**, or
**Cloudflare Pages** and attach `doparise.com`.

Configs included:

- `vercel.json` — Vercel rewrites + headers
- `_headers` / `_redirects` — Netlify and Cloudflare Pages

After DNS is live, check:

```bash
curl -I https://doparise.com/.well-known/assetlinks.json
curl -I https://doparise.com/.well-known/apple-app-site-association
curl -I https://doparise.com/reset-password
```

All three should return `200` (not `301`/`302`). AASA must be `application/json`.

## After deploy

1. **Supabase → Authentication → URL Configuration**
   - Site URL: `https://doparise.com`
   - Redirect URLs: `https://doparise.com/reset-password`
2. Point the Flutter app `PASSWORD_RESET_REDIRECT_TO` / default to
   `https://doparise.com/reset-password` and rebuild.
3. Before Play Store, add the **release / Play App Signing** SHA-256 to
   `.well-known/assetlinks.json`.
