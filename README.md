# AI Anvil — landing site

Static, public landing page for **AI Anvil**, plus the About, Privacy Policy, and Terms
pages used for the BigBrain Google OAuth consent screen.

No build step, no JavaScript, no external fonts, no analytics. GitHub Pages serves the
repository root directly.

## Pages

| URL | File |
| --- | --- |
| `https://aianvil.app/` | `index.html` |
| `https://aianvil.app/about/` | `about/index.html` |
| `https://aianvil.app/privacy/` | `privacy/index.html` |
| `https://aianvil.app/terms/` | `terms/index.html` |
| `https://aianvil.app/404.html` | `404.html` |

The site deliberately does **not** link to any `*.aianvil.app` subdomain. Keep it that way:
the household apps are unadvertised and live behind Cloudflare Access. Only the apex
`aianvil.app` is public.

## Local preview

```bash
python3 -m http.server 8080
# http://localhost:8080
```

Clean URLs (`/about/`, `/privacy/`, `/terms/`) work because each is a directory with an
`index.html`.

## Deploy

GitHub Pages is configured for this repository from the `main` branch, root directory
(`/`). Pushing to `main` deploys automatically.

If Pages is not enabled yet:

```bash
gh api --method POST repos/ZapoVerde/aianvil-site/pages \
  -f 'source[branch]=main' -f 'source[path]=/'
```

The `CNAME` file already contains `aianvil.app`.

## DNS (Cloudflare)

`aianvil.app` currently has no apex DNS record. Add these records in the Cloudflare DNS
dashboard, **DNS only** (grey cloud) so GitHub Pages can issue its certificate:

| Type | Name | Value |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |

Leave `*` and `www` alone unless you specifically want them public — `*.aianvil.app` does
not cover the bare apex, so this site stays public while subdomains remain behind
Cloudflare Access.

After DNS resolves, enable HTTPS enforcement in the repository's **Settings → Pages**
(or via the API once the certificate is issued):

```bash
gh api --method PUT repos/ZapoVerde/aianvil-site/pages -F https_enforced=true
```

Verify:

```bash
dig +short aianvil.app
curl -I https://aianvil.app/
curl -I https://aianvil.app/privacy/
```

## Google OAuth branding values

Use these in Google Auth Platform → Branding:

- Application home page: `https://aianvil.app/`
- Application privacy policy link: `https://aianvil.app/privacy/`
- Application terms of service link: `https://aianvil.app/terms/`
- Authorized domain: `aianvil.app`
- App logo: leave blank (uploading a logo can trigger verification)
- User support email: required by Google Cloud and shown on the consent screen;
  it is not published anywhere on this site. Use a real, monitored address.

The site intentionally publishes no personal name and no email address. Contact
for household members is handled through the private channel they already use.
