# besda.app static site

Hosts privacy policy + support pages for the besda iOS app at App Store submission time.

## What's here

- `index.html` — landing page (covers besda.app root)
- `privacy.html` — privacy policy (English)
- `privacy-es.html` — privacy policy (Spanish, courtesy translation)
- `support.html` — support page (English)
- `support-es.html` — support page (Spanish, courtesy translation)
- `_redirects` — Cloudflare Pages / Netlify rewrite rules for `?lang=es`

## Hosting setup (Cloudflare Pages)

1. Connect this repo to Cloudflare Pages: dashboard → Pages → Create application → Connect to Git → pick `dereksanz/besda-site`
2. Build settings: Framework `None`, build command empty, output directory `/` (root)
3. Deploy. Cloudflare gives you a `*.pages.dev` URL — confirm everything loads there first.
4. Custom domain: Pages project → Custom domains → Set up custom domain → `besda.app`. Cloudflare prints the DNS records needed; add them to your domain registrar (or use Cloudflare DNS if you've already moved the domain there).
5. Verification: from a phone (not the dev machine):
   - `https://besda.app/privacy` returns 200, renders English text
   - `https://besda.app/privacy?lang=es` renders Spanish
   - `https://besda.app/support` and `?lang=es` both work
6. Source-of-truth for the HTML: `AppStore/site/` in the main `dereksanz/besda` repo. If those files change, copy them into this repo and push.

## License

All content © 2026 Derek Sanz.
