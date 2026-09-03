# VaultCove Changelog

## 0.7.2 R1
- Developer toolbar refresh no longer records an on-demand injection session, preventing later navigation from injecting a second copy of the manifest handler.

- Fixed the dashboard ES-module parse failure caused by an unescaped apostrophe in the imported .vckey help text. This was the direct reason VaultCove remained at Loading with an empty dashboard.
- Added explicit ES-module syntax validation for every runtime JavaScript file so Chrome-module parse failures cannot pass the release gate.
- Removed dynamic content-script registration from the Developer runtime. The Always-On Handler is now owned only by the manifest, eliminating the `vaultcove-inline-handler` duplicate-ID registration path.
- Developer on-demand refresh now messages the already-installed handler instead of injecting a second copy.
- Added a pre-module startup watchdog that shows a real startup failure instead of leaving the UI permanently at Loading if a module ever fails again.
- Store-preflight remains permission-minimal and uses activeTab/on-demand filling while this handler registration repair is validated.

## 0.7.1 R2

- Repaired the Store-preflight publisher so a Developer manifest that intentionally omits `optional_host_permissions` is handled safely under PowerShell StrictMode.
- The Store transform now creates `optional_host_permissions` when the property is absent instead of attempting to assign a non-existent PSCustomObject property.
- Added a publisher regression probe for absent optional manifest properties before any build or repository publication step.
- No VaultCove runtime behavior or vault cryptography changed from the tested 0.7.1 R1 runtime repair.

## 0.7.1 R1

- Fixed the dashboard startup path so handler-permission status cannot leave VaultCove stuck at Loading. A visible startup error/retry gate now appears if initialization genuinely fails.
- Developer Always-On Handler now uses a manifest-declared HTTPS content script instead of a dynamic script ID, eliminating the duplicate `vaultcove-inline-handler` registration race. Store builds continue to use optional dynamic website permission.
- Removed redundant Google Apps Script and GitHub host entries from the Developer manifest because the required Developer HTTPS test permission already covers those origins.
- Handler-initiated unlock now opens a dedicated VaultCove unlock tab, closes that tab automatically after successful verification, and notifies the original login page immediately so no page refresh is required.
- Added regression checks for manifest permission redundancy, static Developer handler registration, no duplicate dynamic registration in Developer mode, handler unlock completion, and startup failure visibility.

## 0.7.0 R1

- Added encrypted offline `.vcrecovery` Recovery Kits for restoring the VaultCove sharing identity and device recovery token without cloud storage. Recovery Passwords are separate from the master password and `.vckey` passwords.
- Added recovery-token based device re-identification so a licensed installation can reclaim the same device record after a reset when the user restores the Recovery Kit. No hardware fingerprint is used.
- Clicking a login in Grid or List view now opens its saved HTTPS website and performs an explicit one-time Secure Login for that selected credential.
- Added six built-in themes: VaultCove Midnight, AMOLED Black, Graphite, Ocean, Warm Ivory, and Light Gray.
- Standardized compact responsive spacing and removed decorative Unicode control glyphs from the main dashboard navigation and vault browsing controls.
- Expanded Apps Script licensing with Standard 7-device and Admin 20-device serial classes plus Release, Revoke, Retain, and Restore device actions. Testing builds remain Full Access with no trial enforcement.
- Retained-device recovery can reclaim the same server device record after verified email activation. A restored Recovery Kit matches by its random device-recovery token; if that local token was lost, the verified owner can select a retained record instead of consuming a duplicate slot.
- Added Apps Script sheet-header migration so new licensing columns can be appended safely without depending on a fixed historical column order.
- Added Android-ready recovery/device protocol documentation so future VaultCove Android can exchange the same `.vckey`, `.vcshare`, `.vcvault`, and `.vcrecovery` formats.


## 0.6.1 R1

- Fixed Chrome `Duplicate script ID 'vaultcove-inline-handler'` by serializing browser-integration changes and atomically updating an existing dynamic content-script registration instead of racing unregister/register calls.
- Developer build now keeps the HTTPS login handler always-on so supported fields receive VaultCove automatically without first clicking the toolbar icon. The Store preflight converts this broad developer permission back to optional website access.
- Added masked-handler account count plus a local search box for sites with many matching credentials. Search runs inside the trusted extension background and returns only masked summaries; passwords never enter the page menu.
- Sharing identity Share ID/fingerprint are masked by default and require Sensitive Access to reveal temporarily.
- Replaced plaintext public `.vckey` export/import with password-encrypted `.vckey` v2 using PBKDF2-HMAC-SHA-256 (600,000 iterations) and AES-256-GCM. The `.vckey` password is separate from the VaultCove master password.
- Importing another `.vckey` now requires Sensitive Access plus the separate `.vckey` password before fingerprint confirmation/trust. Private sharing keys remain inside the encrypted vault and are never exported.
- Preserved VCShare v2 multi-account selection with visual website cards and local visited-site favicon rendering.
- Added `License & Devices` UI and Apps Script reference backend for email-bound serial activation, email OTP verification, RSA-signed license leases, seven active device slots, Release/Revoke/Restore controls and last-seen device metadata.
- Licensing uses a random VaultCove installation ID and user device label; it never sends vault contents, saved websites, usernames, passwords, TOTP secrets, cards, bank records, secure notes, MAC addresses, hardware serials, IMEI or browser history.
- Added a platform-neutral licensing/share design so a future VaultCove Android app can use the same `.vckey`, `.vcshare`, `.vcvault` and license-device protocol.
- Added regression tests for the duplicate-handler race, handler search privacy, encrypted `.vckey` wrong-password rejection, licensing privacy boundaries and seven-device Apps Script controls.

## 0.6.0 R1

- Added a one-time optional **Always-On Handler** HTTPS permission. After the user approves it, a persistent dynamic content script makes VaultCove appear automatically on supported login/password fields without first clicking the extension icon.
- Preserved `activeTab` on-demand filling for users who do not grant Always-On access. Broad HTTPS access remains optional, never mandatory.
- Added inline cryptographic password generation for signup, new-password and password-change forms. Generated values can fill matching New/Confirm Password fields directly.
- Preserved separate encrypted review paths for new-account capture and password-change candidates; no silent permanent save/overwrite.
- Added visual Grid/List Vault browsing modeled after website-card password managers, including visited-site favicon cards and private lock-card fallbacks.
- Added optional Chrome `favicon` permission. Icons are read only from Chrome's local favicon cache and displayed only after a matching saved site has been visited while VaultCove is active; no third-party icon lookup is used.
- Added global explanatory dashboard tooltips with hover/focus behavior and accessibility labels.
- Hardened master-password changes: current-password verification, strong/different replacement requirement, exact confirmation phrase, optional authenticator/recovery verification, verify-before-commit key re-wrap, and immediate lock after success.
- Added Google Authenticator-compatible local TOTP confirmation with manual setup URI/key plus one-time recovery codes. Google receives no password/vault data and no Google account connection is created.
- Added an explicit security disclosure that local TOTP is an access-confirmation gate, not a separate cloud-held encryption key against full local-device compromise.
- Upgraded VCShare to v2 multi-account bundles: up to 50 selected login cards in one recipient-encrypted, sender-signed `.vcshare`; v1 import compatibility is retained.
- Added encrypted visited-site visual metadata and local mark-on-visit handling for saved matching logins only, not a general browsing-history log.
- Expanded automated regression tests for Always-On permission architecture, inline generation, TOTP/recovery codes, atomic master rotation, VCShare bundles, tooltips, local favicon privacy, and encrypted site-visual metadata.

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
