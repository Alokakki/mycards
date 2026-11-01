# Vault Cards

A single-file web app (`index.html`) for saving wallet labels and addresses and checking balances.
No server, no build step, no secrets in the code.

## How the security works
- **No default login.** The first time you open the page you create a username and a master password (10+ characters).
- **Encrypted at rest.** Everything you save (labels, addresses, phrase keys) is encrypted in your browser with AES-256-GCM. The key is derived from your password with PBKDF2 (600,000 rounds). The password is never stored.
- **Your data is never in this repo.** It lives only in your own browser's storage. Anyone who opens the published page sees an empty app.
- **Auto-lock** after 5 minutes of inactivity or when the tab has been hidden for 2 minutes. The key is wiped from memory on lock.
- **Content-Security-Policy** limits the page to its own inline code and to the block-explorer / price APIs it needs.
- **Encrypted backups.** The Backup button downloads the encrypted vault file; Import restores it.

## Publish on GitHub (steps)
1. Turn on **two-factor authentication** (a passkey or authenticator app) on your GitHub account first.
2. Create a new repository. Choose **Private** if you only want to store the code.
3. Upload `index.html`, `README.md` and `.gitignore` (drag and drop on the repo page, or use git).
4. Optional hosting: **Settings → Pages → Deploy from a branch → main / (root)**. Tick **Enforce HTTPS**.
   Pages from a private repo needs a paid GitHub plan; the published site itself is always public. That is fine, because the code holds no secrets.
5. Open the site, create your vault, and add your first card.

## Do these as well
- **Keep real seed phrases offline** (hardware wallet or paper). The phrase field is optional. If you use it, the encrypted vault is only as strong as your master password.
- **Use a long passphrase** (5+ random words) and a password manager. A short password can be guessed offline if someone copies your vault.
- **Download a backup** after adding cards, and keep it somewhere private (not in the repo). Clearing browser data deletes the vault.
- **Use a clean browser profile** with few or no extensions. Extensions can read page content.
- **Lock the vault** (Lock button) before walking away. Don't use it on shared or public computers.
- **Never commit** exported backups, phrases or keys. If one ever lands in the repo, treat that wallet as compromised and move the funds to a new wallet. Git history keeps deleted files.

## Limits to know about
- Encryption protects the stored data. It does not protect you from malware on your device or from someone watching while the vault is unlocked.
- Balance lookups send your addresses to public services (publicnode, Blockscout, Blockstream, CoinGecko). They can see which addresses you check.
- If you edit the code in `index.html`, the Content-Security-Policy hash in the `<meta>` tag no longer matches and the page will stay blank. Remove that meta tag, or regenerate the hash.
- Forgot your password? There is no recovery. Use the backup, or erase the vault and start again.
