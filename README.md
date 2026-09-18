# ridegain-site

Public web pages for the **Ridegain** indoor cycling training app — landing page,
privacy policy and terms. The app source lives in a separate private repository.

Static site, no build step. Files are served from the repository root as-is.

| Page | File | URL |
|---|---|---|
| Landing | `index.html` | https://ridegain.app/ |
| Privacy Policy | `privacy.html` | https://ridegain.app/privacy.html |
| Terms of Service | `terms.html` | https://ridegain.app/terms.html |
| Account deletion | `delete-account.html` | https://ridegain.app/delete-account.html |

`privacy.md` / `terms.md` are the Japanese working drafts. The published English
HTML is the authoritative version — when you change one, change the other too.

## Hosting

**Cloudflare Workers static assets** (not Pages), configured by `wrangler.toml`
(`name = "ridegain-site"`, `[assets] directory = "./"`, no Worker script).
Cloudflare's Git integration is connected to this repo, so every push to `main`
runs `wrangler deploy` and publishes automatically. No build command.

`.assetsignore` keeps repository-only files off the public site
(`README.md`, `DEPLOY.md`, `*.md` drafts, `wrangler.toml`, `.git/`, `.github/`).
Verify with `curl -I https://ridegain.app/README.md` → must be `404`.

### `.html` URLs redirect

Workers static assets serve extensionless paths, so `/privacy.html` answers
`307 → /privacy` (which returns `200`). Browsers follow this, so the URLs
registered with Play Console / Strava keep working — but `/privacy` is the
canonical path if you need one without a redirect hop.

See [DEPLOY.md](DEPLOY.md) for the deploy runbook.
