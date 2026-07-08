# ridegain-site

Public web pages for the **Ridegain** indoor cycling training app (privacy policy, landing page).
Served via Cloudflare Pages (Git integration). The app source lives in a separate private repository.

- Privacy Policy: `privacy.html`
- Landing: `index.html`

Static site, no build step. Files are served from the repository root as-is.

## Cloudflare Pages

Deployed via Cloudflare Pages connected to this GitHub repo. Every push to `main`
triggers an automatic deploy (no build command; output directory = `/`).

- Production: https://ridegain.app/
- Privacy Policy: https://ridegain.app/privacy.html
- Pages default URL: https://ridegain-site.pages.dev/

### Dashboard settings

- Project name: `ridegain-site`
- Production branch: `main`
- Framework preset: None
- Build command: *(empty)*
- Build output directory: `/`
- Custom domain: `ridegain.app` (also `www` → apex redirect if desired)
