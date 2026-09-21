# Golden Hour Violin

Static site for Golden Hour Violin, a wedding violinist. It replaces the previous Carrd page and is meant to be served by GitHub Pages, including on the custom domain `goldenhourviolin.com`.

The public page stays short on purpose. Packages, the retainer, and how booking works are here. Music choices are settled on the phone after someone books.

## Edit the site

Content lives in `index.html`. Styles are in `styles.css`. The small menu script is `script.js`.

When you change a price or the booking link, update every place it appears:

- Package names and prices are the four cards in the Packages section.
- The retainer line is “A $75 nonrefundable retainer holds the date and is applied to your balance.” It also appears in the hero and in How it works. Keep the wording the same in each place.
- Every Book button uses the Square Appointments link:
  `https://book.squareup.com/appointments/0f2juwg2oo81ss/location/L2WKE6Z5AJVSV`
- The contact address is `goldenhourviolin@gmail.com`.

Photos in `assets/images/` are the pictures from the previous site. They have had metadata removed. Replace a photo by keeping the same filename, or change the `src` in `index.html`. The social sharing image is `assets/og.jpg`.

Preview locally from the repository root:

```bash
python3 -m http.server 8080
```

Then open `http://127.0.0.1:8080/`.

### Leave this off the public site

These belong on the call after booking, not on the page:

- Set list
- Own-sound details
- Faith notes
- Personal-song add-on
- Package timing (how many minutes, when a piece starts, and similar)

Do not add a street address. The service is mobile. Do not add testimonials, venues, or photos that are not actually from this business.

## GitHub Pages

The site is plain HTML, CSS, and JS at the repository root, with a `.nojekyll` file so GitHub Pages serves the files as they are.

Publish from the `main` branch, folder `/` (root):

1. Open the repository on GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Branch: `main`. Folder: `/ (root)`. Save.

After `main` contains `index.html`, the project URL is:

`https://coastbeekeeper-afk.github.io/goldenhourviolin/`

`CNAME` is in the root and contains only `goldenhourviolin.com`. Once that file is on the publishing branch, GitHub Pages treats the custom domain as the site address and redirects the `github.io` URL to it. Until DNS points at GitHub, that redirect still opens the current Carrd site. Use the local preview, or temporarily clear the custom domain in Pages settings, when you need to review the new site before cutover.

## Point goldenhourviolin.com at GitHub Pages

DNS is at Namecheap (`dns1.registrar-servers.com` and `dns2.registrar-servers.com`). The apex currently has an A record `172.66.0.70` (Carrd, via Cloudflare). `www` is a CNAME to `goldenhourviolin.com`.

The domain also has email DNS. Leave these records in place:

- MX `mx1.privateemail.com` and `mx2.privateemail.com`
- TXT `v=spf1 include:spf.privateemail.com ~all`

Changing only the website records keeps Namecheap Private Email working.

### Apex (`goldenhourviolin.com`)

In Namecheap → Domain List → Manage → Advanced DNS, remove the Carrd A record (`172.66.0.70`) and add all four A records:

| Type | Host | Value |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |

IPv6, optional but recommended:

| Type | Host | Value |
| --- | --- | --- |
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |

If the DNS host supports `ALIAS` or `ANAME` on the apex, you can use one record instead of the A/AAAA set:

| Type | Host | Value |
| --- | --- | --- |
| ALIAS or ANAME | `@` | `coastbeekeeper-afk.github.io` |

Namecheap Basic DNS uses the A records above.

### www

Replace the current `www` CNAME (it points at `goldenhourviolin.com`) with:

| Type | Host | Value |
| --- | --- | --- |
| CNAME | `www` | `coastbeekeeper-afk.github.io` |

Do not include `https://` and do not add the repository name. GitHub will redirect `www` to the apex because `CNAME` in this repo is `goldenhourviolin.com`.

### After DNS updates

1. Merge this site to `main` and confirm Pages is deploying that branch from `/`.
2. In **Settings → Pages**, the custom domain should read `goldenhourviolin.com` (taken from the `CNAME` file). Save if GitHub asks.
3. Wait until the DNS check succeeds. This is often within an hour and can take longer.
4. Turn on **Enforce HTTPS** once the certificate is ready. If the box stays disabled, remove the custom domain, save, add `goldenhourviolin.com` again, and wait.
5. Visit `https://goldenhourviolin.com` and `https://www.goldenhourviolin.com`. Both should show this site, with `www` redirecting to the apex.
6. Leave the Carrd site in place until that check looks right, then unpublish it so the domain is no longer tied to Carrd.

If the domain is ever moved behind Cloudflare’s proxy, set the GitHub records to DNS only (grey cloud). An orange-cloud proxy blocks the Pages certificate.

Useful checks:

```bash
dig goldenhourviolin.com A +short
dig www.goldenhourviolin.com CNAME +short
dig goldenhourviolin.com MX +short
```

The A answers should be the four GitHub addresses. MX should still be `privateemail.com`.

## Fonts

Cormorant Garamond and Outfit are self-hosted in `assets/fonts/` under the SIL Open Font License. See `assets/fonts/OFL.txt`.
