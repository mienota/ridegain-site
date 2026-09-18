# Deploy runbook — ridegain.app

**Hosting**: Cloudflare **Workers static assets** (a Worker named `ridegain-site`
with no script, serving the repo root). Configured entirely by `wrangler.toml`.
**Domain**: `ridegain.app`, registered through Cloudflare Registrar in the same account.

Account / billing / owner details are kept out of this public repo — see
`docs/APP_MANAGEMENT.md` in the private app repository.

## Updating the site

1. Edit `index.html` / `privacy.html` / `terms.html` / `delete-account.html`.
   When you change a policy, update the Japanese draft (`privacy.md`, `terms.md`)
   and the published English HTML **together**, and bump the "Last updated" date.
2. Commit and `git push` to `main`.
3. Cloudflare's Git integration runs `wrangler deploy` automatically. No build step.
4. Verify (see below).

Manual deploy from a workstation, if the Git integration is ever unavailable:

```sh
npx wrangler deploy
```

## Verifying a deploy

```sh
curl -sL -o /dev/null -w '%{url_effective} %{http_code}\n' https://ridegain.app/privacy.html
curl -s -o /dev/null -w 'README.md must be 404: %{http_code}\n' https://ridegain.app/README.md
curl -s -o /dev/null -w 'terms.md must be 404: %{http_code}\n'  https://ridegain.app/terms.md
```

- Public pages must return `200`. Note that `/privacy.html` answers `307 → /privacy`
  because Workers static assets serve extensionless paths; following the redirect
  must reach `200`. The `.html` URLs registered with Play Console / Strava are fine.
- Anything listed in `.assetsignore` must return `404`. If a repo-only file starts
  returning `200`, it is missing from `.assetsignore` — add it and push.

## External references to keep in sync

| Where | Value |
|---|---|
| Google Play Console — Privacy Policy URL | `https://ridegain.app/privacy.html` |
| Google Play Console — Account deletion URL | `https://ridegain.app/delete-account.html` |
| Strava API application | privacy policy + terms URLs on the same domain |

## One-time setup (already done — kept for reference)

1. **Domain**: Cloudflare dashboard → Domain Registration → Register Domains →
   `ridegain.app`. The zone is added to the account automatically with Cloudflare
   nameservers; no DNS delegation step.
2. **Worker + Git integration**: Workers & Pages → Create → Workers → connect the
   GitHub repo `mienota/ridegain-site`. Production branch `main`, no build command;
   `wrangler.toml` supplies the rest.
3. **Custom domain**: attach `ridegain.app` to the Worker. Because the zone lives in
   the same account, Cloudflare creates the DNS record and provisions TLS automatically.
4. **Retired**: the old GitHub Pages site at `mienota.github.io/ridegain-site`
   (repo → Settings → Pages → Source = None).
