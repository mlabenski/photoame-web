# Photoame website (photoame.com)

Static site for App Store compliance: a landing page, **Privacy Policy**
(`/privacy/`), and **Support** page (`/support/`). No build step — plain HTML/CSS.

Final URLs once deployed:
- https://photoame.com/ (landing)
- https://photoame.com/privacy/ (privacy policy — put this in App Store Connect)
- https://photoame.com/support/ (support page)
- support@photoame.com (support email — see Email section)

---

## 1. Put it on GitHub

```bash
cd photoame-web
git init
git add .
git commit -m "Photoame marketing + privacy/support site"
gh repo create photoame-web --public --source=. --remote=origin --push
# (or create the repo in the GitHub UI and: git remote add origin … && git push -u origin main)
```

## 2. Enable GitHub Pages

In the repo: **Settings → Pages**
- **Source:** Deploy from a branch
- **Branch:** `main`, folder `/ (root)` → Save

The included `CNAME` file already sets the custom domain to `photoame.com`, and
`.nojekyll` tells Pages to serve the files as-is. After DNS (below) resolves,
tick **Enforce HTTPS**.

## 3. DNS — point photoame.com at GitHub Pages

Add these records at whoever manages your DNS (recommended: move the domain's
nameservers to **Cloudflare** — free — so DNS + email live in one place).

**Apex `photoame.com` → four A records (GitHub Pages IPs):**

| Type | Name | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

Optional IPv6 (AAAA, same `@`): `2606:50c0:8000::153`, `2606:50c0:8001::153`,
`2606:50c0:8002::153`, `2606:50c0:8003::153`

**`www` subdomain → CNAME:**

| Type | Name | Value |
|------|------|-------|
| CNAME | www | `mlabenski.github.io` |

> On Cloudflare, set these DNS records to **DNS only** (grey cloud), not proxied,
> while GitHub verifies the domain and issues the TLS cert. You can enable the
> orange proxy afterwards if you like.

DNS can take from minutes to a few hours. Verify with:
```bash
dig +short photoame.com
curl -I https://photoame.com/privacy/
```

---

## 4. Email — support@photoame.com → your Gmail (Cloudflare, free)

Requires the domain's nameservers to be on Cloudflare (add the site to Cloudflare
first; it's free).

1. Cloudflare dashboard → your domain → **Email → Email Routing** → Enable.
2. Cloudflare adds the required **MX** and **TXT (SPF)** records automatically —
   approve them.
3. **Destination addresses** → add your Gmail → confirm via the email Cloudflare sends.
4. **Routing rules** → Custom address: `support@photoame.com` → forward to your Gmail.
   (Optionally add a catch-all → your Gmail.)

Test by emailing support@photoame.com; it should land in your Gmail.

> To *send* replies as support@photoame.com from Gmail: Gmail → Settings → Accounts
> → "Send mail as" → add support@photoame.com. This needs an SMTP relay; simplest
> is to keep replying from your Gmail for now, or add an SMTP provider later.

**No-Cloudflare alternative:** [ImprovMX](https://improvmx.com) offers free
forwarding by adding MX + TXT records at your current DNS host, without moving
nameservers.

---

## 5. Put the URLs in App Store Connect
- **Privacy Policy URL:** `https://photoame.com/privacy/`
- **Support URL:** `https://photoame.com/support/`
- **Support email / contact:** `support@photoame.com`

The app's in-app links (Settings → Privacy policy / Support) already point here.
