# VaultCove Changelog

## 0.5.2 R1

- Fixed master-password retry handling so a failed attempt cannot race with or visually override the next correct attempt.
- Serialized unlock and Sensitive Access verification in the background service worker and added UI busy guards to reject duplicate concurrent submissions.
- Wrong-password failures now report `Incorrect master password.` instead of conflating a normal typo with vault damage.
- Added persistent inline errors on unlock and Sensitive Access prompts; failed entries are cleared and refocused for a clean retry.
- A failed Sensitive Access verification clears any stale Sensitive Access grant without changing the encrypted vault or unlocked vault-key session.
- Added explicit phishing-safe Google account-family matching for `accounts.google.com`, `mail.google.com`, `myaccount.google.com`, `gmail.com`, and `www.gmail.com`.
- Gmail now surfaces all matching saved/imported Google accounts even when LastPass stored them under `accounts.google.com` or another explicitly trusted Google account host.
- Google family matching is an exact reviewed-host allowlist; root `google.com`, arbitrary subdomains, suffix lookalikes, and attacker-controlled domains do not inherit access.
- Added regression tests for wrong-then-correct master-password recovery and Gmail multi-account matching.

## 0.5.1 R1

- Added encrypted new-login/signup capture with user-confirmed Save/Ignore review; no silent credential registration.
- New-login candidates are stored only inside the encrypted vault payload and are pruned after seven days if not reviewed.
- Existing site/account identifiers are excluded from new-login capture so password changes continue through the separate password-change review path.
- Added local new-login notifications and Security Center review cards with masked account identifiers.
- Added a Settings toggle for local new-login detection.
- Repaired STORE-preflight sanitization so the Developer GitHub update endpoint cannot survive in the Store package.
- Store preflight now validates removal of update host permission, runtime fetch, and raw GitHub endpoint strings before packaging.
- Update/publisher accepts the partially applied 0.5.0 state produced by the failed R1 build without requiring rollback to 0.4.1.

## 0.5.0 R1

- Replaced blanket persistent-handler UX with per-site HTTPS permission requests.
- Added compact popup actions and Secure Login.
- Added trusted-host matcher with safe `www.` alias and explicit subdomain trust.
- Added post-submit password-field clearing fallback for Secure Login.
- Added per-login master-password-before-fill protection.
- Preserved on-demand handler permission across same-origin navigation.
- Added scalable vault list/detail-drawer GUI and richer empty states.
- Added truthful Imported vs Updated activity labeling.
- Added VCShare v1 public identities, trusted recipients, recipient-encrypted signed `.vcshare` packages and encrypted local share history.
- Added signed Developer update metadata checks from `VaultCove-release`.
- Added DEV/STORE build separation; Store preflight removes the GitHub updater permission and network fetch code.
- Expanded core/static tests for phishing-safe matching, VCShare encryption and update-channel restrictions.

## 0.4.1 R1

- Sensitive Access hardening.
- Password-change candidate review.
- On-demand and persistent login handler repair.
