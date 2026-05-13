# besda.app static site — privacy + support

Four HTML files, ready to host at the URLs Apple App Review will hit.

## Files

| File | Hosted URL |
|---|---|
| `privacy.html` | `https://besda.app/privacy` |
| `privacy-es.html` | `https://besda.app/privacy-es` |
| `support.html` | `https://besda.app/support` |
| `support-es.html` | `https://besda.app/support-es` |

> **Why not `?lang=es`?** Cloudflare Pages `_redirects` doesn't support query-string matching — the `?lang=es` part gets stripped during rule parsing, causing every `/privacy*` request to rewrite to the Spanish file. Clean `/privacy-es` paths use Pages' default extensionless routing instead and don't need any `_redirects` rules.

## Hosting paths (pick one — 5 min each)

### Option 1 — Cloudflare Pages (recommended)
1. Push these files to a GitHub repo (e.g., `dereksanz/besda-site`).
2. Cloudflare dashboard → Pages → Create → connect the repo.
3. Build settings: framework `None`, build command empty, output directory `/`.
4. After first deploy, custom domain → `besda.app`. Add the DNS records Cloudflare gives you to whoever runs DNS for besda.app.
5. Routing: Cloudflare Pages' default extensionless routing handles everything. `/privacy` → `privacy.html`, `/privacy-es` → `privacy-es.html`. No `_redirects` file needed (and adding one for `?lang=es` actively breaks things — see warning above).

### Option 2 — Netlify
1. Drag-and-drop the four files into the Netlify dashboard's "Sites" tab.
2. Add a custom domain (`besda.app`); follow Netlify's DNS instructions.
3. No `_redirects` needed — Netlify's extensionless routing serves `/privacy-es` from `privacy-es.html` automatically. (Netlify _redirects does support query params if Derek wants `?lang=es` later, but for v1 launch the clean path is simpler.)

### Option 3 — Static hosting Derek already has
If besda.app already has hosting somewhere (Vercel, GitHub Pages, S3, etc.), just upload all four files. The `?lang=es` rewrite needs whatever the host's redirect-rule syntax is.

## Verification (do this before submitting to App Store Connect)

From a phone (not the dev machine — Apple App Review checks from outside Derek's network):

- [ ] `https://besda.app/privacy` returns 200, English text renders
- [ ] `https://besda.app/privacy-es` returns 200, Spanish text renders
- [ ] `https://besda.app/support` returns 200, English text renders
- [ ] `https://besda.app/support-es` returns 200, Spanish text renders
- [ ] In the privacy English page, paste-search for "Effective May 3, 2026" — must match `PrivacyPolicyView.effectiveDate` exactly
- [ ] Privacy English body text matches `PrivacyPolicyView.swift` section-for-section (the bot Apple uses for §5.1.2 mismatch detection compares paragraph-level)

## Known gaps to disclose to App Review if asked

The in-app `PrivacyPolicyView.swift` ships English-only — the long-form legal text is hardcoded in Swift and was not localized in V2 Steps 51–62 (only short UI strings via `Localizable.xcstrings` were). The hosted Spanish version is a courtesy translation for Mexican Spanish-speaking testers; the English version remains the legally-binding one (footer notes this explicitly).

If App Review flags this as a §5.1.2 / §4.5.4 issue, the fix is to add a Spanish-localized PrivacyPolicyView in a v1.0.1 patch (estimated 2 hours of Swift work + Localizable.xcstrings update). The same applies to TermsOfServiceView.

## Why not just inline the Spanish in PrivacyPolicyView?

Because that's a 2-hour Swift change that requires re-running snapshot tests and isn't covered by the existing V2 Step 51–62 PRs. The hosted page is the App-Review-required artifact; making in-app match perfectly is a v1.0.1 polish item.
