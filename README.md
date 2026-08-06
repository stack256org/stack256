# stack256.org

The Stack256 website. One static page, no build step, no dependencies —
`index.html` is the whole site.

## Editing it

Open `index.html` and edit it. There is nothing to install and nothing to
compile; the styles and the logo are inline, and the only images are none. Push
to `main` and GitHub Pages redeploys.

The logo is an inline SVG rather than a raster file, so it stays crisp at any
size, needs no request, and has exactly one definition — the favicon is the same
four paths as a `data:` URI. If the mark changes, change both.

## Things that are deliberate

**One dark theme, no light variant.** The mark is a saturated orange-red that
only holds its weight on near-black; on white it vibrates and stops reading as a
logo. Committing to the brand surface is the design decision.

**The progress bar is honest.** It is `2/256` — 0.78% — with a `min-width` so it
is visible at all. It is meant to look like the beginning of something, not a
nearly-finished loading bar.

**Product copy comes from the products.** Each card repeats that repository's own
description rather than inventing marketing for it, so the two cannot drift.

## Publishing

Served by GitHub Pages from `main`, at
<https://stack256org.github.io/stack256/> until the apex domain is attached.

### Attaching stack256.org

Not done yet, and it needs a DNS change first. The apex currently resolves to
Cloudflare and returns **525** (Cloudflare cannot complete TLS with its origin),
so nothing usable is being served there.

Do it in this order, because doing it backwards takes the site down: a `CNAME`
file makes Pages redirect its working `github.io` URL to the custom domain, so if
DNS is not ready yet, both stop working.

1. **DNS first.** Point the apex at GitHub Pages — either four `A` records to
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`,
   or a `CNAME` to `stack256org.github.io` if your provider supports apex
   flattening (Cloudflare does). If Cloudflare is proxying, set that record to
   **DNS only**: with the orange cloud on, GitHub cannot complete the HTTP-01
   check it uses to issue the certificate.
2. **Then** add a `CNAME` file containing `stack256.org` and push.
3. Confirm the certificate: `gh api repos/Stack256org/stack256/pages --jq
   '.https_certificate.state'` should reach `approved`. It cannot be requested
   before a deployment exists.
