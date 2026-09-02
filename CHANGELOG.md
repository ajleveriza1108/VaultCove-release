# VaultCove Changelog

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
