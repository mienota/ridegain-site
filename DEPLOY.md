# Deploy runbook — Cloudflare Pages + ridegain.app

Hosting: **Cloudflare Pages** (Git integration) on the domain **ridegain.app**.
Cloudflare account: `mieno.tk0909@gmail.com` (account id `ff3acd37637579e0ea878c1febe384db`).

Only Step 1 and Step 2 need dashboard clicks (no API for new domain registration
or GitHub OAuth). Everything else is automatic once those are done.

## Step 1 — Register ridegain.app (dashboard, ~2 min)

1. https://dash.cloudflare.com → left sidebar **Domain Registration → Register Domains**.
2. Search `ridegain.app` (~$14/yr) → add to cart → checkout.
3. On completion the zone `ridegain.app` is added to the account automatically with
   Cloudflare nameservers. No DNS delegation step needed.

## Step 2 — Create the Pages project with Git (dashboard, ~2 min)

1. https://dash.cloudflare.com → **Workers & Pages → Create → Pages → Connect to Git**.
2. Authorize GitHub (first time only) and select repo **`mienota/ridegain-site`**.
3. Settings:
   - Project name: `ridegain-site`
   - Production branch: `main`
   - Framework preset: **None**
   - Build command: *(empty)*
   - Build output directory: `/`
4. **Save and Deploy** → first deploy publishes to `https://ridegain-site.pages.dev/`.

After this, every push to `main` auto-deploys.

## Step 3 — Attach the custom domain (dashboard, ~1 min)

1. In the `ridegain-site` Pages project → **Custom domains → Set up a custom domain**.
2. Enter `ridegain.app`. Since the zone lives in the same account, Cloudflare creates
   the DNS record and provisions the TLS certificate automatically (a few minutes).
3. (Optional) Also add `www.ridegain.app` and redirect it to the apex.

Verify:
- https://ridegain.app/ → landing page
- https://ridegain.app/privacy.html → privacy policy

## Step 4 — Update external references to the new URL

- **App Store Connect / Google Play Console**: set the Privacy Policy URL to
  `https://ridegain.app/privacy.html`.
- Any in-app links to the old `mienota.github.io/ridegain-site/...` URL.

## Step 5 — (optional) Retire GitHub Pages

Once ridegain.app is live, disable the old GitHub Pages site to avoid duplicate content:
GitHub repo → Settings → Pages → set Source to **None**.

## Updating the site later

Just edit `index.html` / `privacy.html`, commit, and `git push`. Cloudflare Pages
rebuilds and deploys automatically. No build step, files served from repo root.
