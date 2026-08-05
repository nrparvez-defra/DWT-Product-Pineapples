# DWT prototypes — public publishing repo

This repository is the **public, encrypted publishing site** for Digital Waste Tracking working drafts, served via GitHub Pages. It follows the pattern already in use elsewhere in Defra (public Pages URL) but with one improvement: **every sensitive page is encrypted at rest**, so being in a public repo exposes only ciphertext. Visitors see a password page; the password decrypts the content in their browser.

## The two-repo pattern

| Repo | Visibility | Contains |
|---|---|---|
| **Source repo** (e.g. `dwt-spider-map`) | Private | The generator scripts, unencrypted HTML, README working notes. Never public. |
| **This repo** (e.g. `dwt-prototypes`) | Public | The landing page, and one folder per prototype containing only the *encrypted* `index.html`. |

Keep that separation strict: nothing unencrypted and sensitive ever gets committed here, because everything here is world-readable.

## Structure

```
index.html        ← landing page listing all prototypes (public, deliberately bland)
spider/index.html ← encrypted: ecosystem map (the spider moment)
encrypt.sh        ← helper that encrypts any prototype the same way
```

## Publishing a new prototype

1. Build the prototype in its own **private** source repo.
2. From this repo's root, encrypt it into a new folder:
   ```bash
   ./encrypt.sh path/to/prototype.html <folder-name> "<password>"
   ```
3. Add a card for it to `index.html` (copy an existing card in the marked block; set the title, one-line description, link and chips).
4. Commit and push. Pages updates in a minute or two.
5. Share the URL and the password **in separate messages/channels**.

## Passwords

- One password per prototype (or one shared across the set — simpler, weaker; decide per audience).
- To rotate: re-run `encrypt.sh` with the new password and push. The old password stops working immediately.
- The password page includes a 7-day "remember me" option so regular viewers aren't retyping it.
- This is working-draft-grade protection: strong encryption, but a shared secret with no audit trail. Anything above that sensitivity doesn't belong on a public URL at all.

## Enabling GitHub Pages (once)

Repository **Settings → Pages → Build and deployment → Deploy from a branch**, branch `main`, folder `/ (root)`. The site appears at `https://<org>.github.io/<repo-name>/`.

## Landing page conventions

- Titles and one-line descriptions on the landing page are **public** — keep them bland (no numbers, no positions, no dates beyond a version stamp).
- Chips: EXPLORATORY DRAFT / PASSWORD REQUIRED / version + month. Add an OPEN chip (green) only for pages that have been explicitly agreed for public viewing.
