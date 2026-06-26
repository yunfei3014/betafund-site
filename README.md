# Beta Fund — betafund.ai

Marketing site for **Beta Fund**, the fund for the agent-native era.
$200K first check, up to $2M across pre-seed and seed. Global.

## Architecture

Plain **static multi-page** site — no build step, no framework, no runtime.
Every page is a standalone `.html` file sharing one stylesheet and one script.

```
index.html        Home — hero, conviction film, explore cards, CTA
thesis.html       Thesis — token-native / agent-native / self-improving
companies.html    Portfolio — 4 themes, 12 companies
fellowship.html   The Fellowship — "more than capital" + what's included
apply.html        Application form (posts to the Butterbase apply fn)
css/site.css      Shared design system (mobile-first)
js/site.js        Shared behavior — mobile nav, reveals, canvas motifs, form
assets/           Images
```

This replaces the previous single-page Butterbase DC-runtime build. It now
works on any static host (GitHub Pages, Butterbase static, S3, Netlify…) with
no client-side runtime dependency, and is fully responsive with a real mobile menu.

## Local preview

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

## Application form

`apply.html` POSTs JSON to the Butterbase function
`https://api.butterbase.ai/v1/app_oh23tcj73owo/fn/apply`, which writes to the
`lbk_applications` table and mirrors to the LBK Fellowship Applications Google
Sheet. Field id → payload key mapping lives in `js/site.js` (`MAP`).

## Deploy (Butterbase static)

Zip the site contents at root and deploy to app `app_oh23tcj73owo`, framework
`static`:

```bash
cd betafund-site-mpa
zip -r frontend.zip index.html thesis.html companies.html fellowship.html apply.html css js assets
# create_frontend_deployment {app_id, framework:"static"} -> uploadUrl
# curl -X PUT "<uploadUrl>" -H "Content-Type: application/zip" --data-binary @frontend.zip
# manage_frontend {action:"start_deployment", deployment_id}
```

## Design tokens

Yellow editorial. Edit in `css/site.css` `:root`:
`--accent:#FFD473` · `--gold-deep:#8A6A00` · `--ink:#000` · `--paper:#F6F4F1`.
Type: Helvetica Neue (display/body) + Space Mono (labels/mono).
