# The Radar — hosted brief

The encrypted daily brief, served as a static page.

`index.html` is a self-contained page holding the brief as **AES-256-GCM
ciphertext**. Opening it asks for a passphrase; the key is derived in the
browser with PBKDF2-SHA256 (310,000 iterations). Nothing readable is stored
here, which is what makes it safe to publish from a public repo.

## Refreshing it after a new brief

Your Mac rebuilds the brief at 7:05am and 6:40pm. To publish the newest one:

```bash
bash ~/Radar/refresh-publish.sh
```

That re-encrypts the current brief and writes it to `~/Radar/publish/index.html`.
Upload that one file here (drag it onto this repo on github.com, or commit it),
and the site updates within a minute or so.

## Changing the passphrase

```bash
python3 ~/Radar/build/secure_page.py          # prompts for a new one
security add-generic-password -a radar -s radar-brief-passphrase -w 'NEW' -U
```

Update the Keychain entry too, or the next scheduled brief will re-encrypt with
the old passphrase. Devices that had "keep me unlocked" ticked will simply ask
again — a stale saved passphrase falls back to the form rather than hanging.

## What is safe to put here

Only `index.html`, `.nojekyll` and `robots.txt`. **Never commit
`~/Radar/radar.html`, `~/Radar/latest.json` or anything from `~/Radar/briefs/`** —
those are the brief in plain text.

## Notes

- `.nojekyll` stops GitHub Pages running the files through Jekyll.
- `robots.txt` plus a `noindex` meta keeps it out of search results. That is
  tidiness, not security — the security is the encryption.
- The published ciphertext is public, so it can be attacked offline. A long
  random passphrase is the whole defence. Do not shorten it.
