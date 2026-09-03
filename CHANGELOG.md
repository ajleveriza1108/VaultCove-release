# VaultCove Changelog

## 0.7.12 R1
- New encrypted `.vcvault` backup secrets are 256-bit random Base64URL tokens with no `VC`, `VC1-`, or `VaultCove` prefix. Legacy `VC1-...` secrets remain accepted for restore so existing backups are not broken. The missing prefix is camouflage only; security still comes from randomness and authenticated encryption.
- Added `.vcshare` v3 as a dedicated password-sharing file format. User 1 selects owned login credentials, chooses a trusted recipient, creates a separate package password, and must freshly verify User 1's own Master Password before export.
- `.vcshare` v3 uses recipient-bound P-256 ECDH/HKDF encryption plus sender ECDSA signatures, then wraps the recipient package in a second PBKDF2-HMAC-SHA-256 (600,000 iterations) + AES-256-GCM layer protected by User 1's package password.
- Import requires the `.vcshare` package password and a fresh verification of User 2's own VaultCove Master Password. After successful recipient verification/decryption, the credential is immediately resealed under User 2's local vault key.
- Imported shared credentials are **Use Only / immutable**: they cannot be revealed, copied, edited, favorited, reorganized as owned items, auto-login-enabled, password-change-captured, or reshared. The page handler offers Secure Login only.
- Added a per-shared-item AES-256-GCM sealed credential envelope with a fresh random 256-bit item key wrapped by User 2's vault key. Plain username/password fields are empty in the normal decrypted vault model until the background service materializes them transiently for Secure Login.
- Added optional 1-day, 7-day, 30-day, or no-expiration policy to new shared-access packages. Expired shared access is blocked.
- Added regression coverage for prefixless backup secrets, legacy backup-secret compatibility, package-password rejection, wrong-recipient rejection, shared-credential plaintext leakage, second-envelope materialization, and no-reshare/no-edit enforcement.

## 0.7.11 R1
- Fixed encrypted `.vcvault` restore: the restore flow now reads `restoreFile` and `restoreSecret`, snapshots both before Sensitive Access, and no longer dereferences the removed/nonexistent `importFile` control.
- Hardened encrypted `.vckey` import by snapshotting its selected file before fresh authentication.
- Added direct Chrome save-picker handling for `.vckey` export with pointer-lock release, visible cursor guard, user-cancel handling, and safe download fallback. The save destination is chosen before authentication but nothing is written until master-password verification and the separate `.vckey` password succeed.
- Added new-vault-only Security Essentials setup: generic security-notice email, auto-lock timing, Authenticator setup, Offline Recovery Kit creation, secure handler/capture defaults, and master-password recovery acknowledgement. Existing upgraded vaults are not interrupted.
- Added editable security-notice email in Settings and suppresses notice queueing until a valid recipient email is configured.
- Added automated regression gates for restore control integrity, save-picker ordering/pointer state, and first-run security setup.

## 0.7.10 R1
- Added a search box to **Vault Settings > Put logins in a folder**. Search matches login name/site, username, displayed account metadata, and current folder name; passwords are never included in organizer search.
- Reduced the visual width of **Open and Secure Login** on Vault cards and added a dedicated Trash icon to Grid and List views.
- Moving an item to Trash now requires an explicit confirmation explaining the lock and 30-day retention period.
- Replaced plaintext soft-delete records with **Sealed Trash**. A fresh random per-item AES-256-GCM key encrypts the original item; that key is itself wrapped by the active vault key in a separate authenticated envelope. The normal decrypted vault model keeps only a minimal tombstone and cannot expose username, password, URL, notes, TOTP secret, card/bank fields, or other original item fields while the item is in Trash.
- Trash items cannot be opened, edited, filled, copied, shared, or viewed. The only normal recovery path is **Restore**, which decrypts the sealed payload back into the active vault.
- Trash items are automatically purged after 30 days on the next unlock/load/save after their deadline. Existing pre-0.7.10 plaintext Trash records are automatically sealed on first load unless already past the 30-day deadline.
- Manual **Delete now** remains available in Trash behind Sensitive Access and a second irreversible-delete confirmation.
- Added cryptographic round-trip, plaintext-leak, legacy-hardening, 30-day expiry, UI-search, and Trash-action regression gates.

## 0.7.9 R1
- Recovery release for a canonical project that is already on VaultCove 0.7.7. The publisher now explicitly accepts and fingerprint-verifies the clean 0.7.7 baseline instead of stopping before backup.
- Carries forward the centered unlock/setup gate, live website-handler unlock acknowledgement, encrypted Vault folders with Vault Settings organization, and Google Authenticator-gated `.vckey` to TXT conversion intended for 0.7.8.
- Supports verified 0.7.7, 0.7.8, and 0.7.9 states so the updater can recover from the rejected/superseded update path without destructive resets or force-pushes.
- Creates a rollback backup before applying the clean 0.7.9 source and retains the exact two-file BAT + PS1 package contract.

## 0.7.8 R1
- Based the update on the accepted 0.7.6 source; 0.7.7 is not required.
- Fixed the idle-lock layout regression: the locked/setup gate owns the full viewport and remains centered regardless of Compact mode, theme, responsive sidebar breakpoint, browser size, or display scaling.
- Reworked the website-handler unlock handoff into an acknowledged live session refresh. Unlocking from a handler request broadcasts the new session state, refreshes the original page handler immediately, requires an acknowledgement, and only then closes the VaultCove unlock tab. No website refresh should be required.
- Broadcast lock state to open HTTPS handlers for manual and idle auto-lock so handler badges/panels do not keep stale unlocked state.
- Added encrypted Vault folders and **Vault Settings**. Users can create/rename/delete folders, select one or many saved logins, and move them into a folder or No Folder. Folder deletion preserves every credential.
- Added Folder selection to the encrypted login/item editor and folder filtering to Vault views.
- Added Google Authenticator-gated `.vckey` to TXT conversion. Conversion requires fresh Sensitive Access plus the existing separate `.vckey` password; there is no third TXT-conversion password. TXT output contains only public sharing identity material.
- Preserved the accepted 0.7.6 appearance stability and compact Dashboard icon fixes.

## 0.7.6 R1
- Fixed the appearance bootstrap regression that made the interface density change when a theme was selected. Saved theme and Compact interface state are now applied before the unlocked UI is shown.
- Split appearance updates into independent paths: changing Theme updates palette/surfaces only, while Compact interface alone controls sidebar width, topbar height, content spacing, and density.
- Added a regression guard that rejects theme palette blocks containing geometry properties, preventing future themes from changing layout, card sizing, gaps, or responsive breakpoints.
- Reworked the Dashboard summary icons highlighted in the UI audit: oversized circular badges are replaced by smaller consistent rounded-square icon tiles with equal sizing, neutral surfaces, and uniform alignment across Logins, Cards, Bank Accounts, Secure Notes, Identities, and TOTP Codes.
- Removed the one-off Secure Notes summary icon background so every summary tile follows the same visual hierarchy.
- Preserved the 0.7.5 password-age review, editable Favorites, Always-On Handler, LastPass HTTPS repair, recovery, sharing, licensing, and Testing Full Access behavior.

## 0.7.5 R1
- Added local password-age review using trustworthy `lastPasswordChangeAt` metadata. Credentials with known password age at least 90 days are flagged for review; 180 days receives higher priority. Imported LastPass/Zoho records remain `Age unknown` unless the source explicitly contains a password-change timestamp.
- Added direct editable Favorites controls to Grid and List views. Users can add or remove Favorites without opening the full credential editor; removing a Favorite never deletes the vault item.
- Added `Mark changed today` for credentials whose password was changed outside VaultCove so users can establish an accurate local age baseline.
- Replaced text placeholders such as LOGIN, BANK, MATCH, GOOD and decorative Quick Action glyphs with packaged SVG icons.
- Reworked Dashboard, empty states, Security Center password-age review, Backup/Restore layout and responsive breakpoints for compact desktop use without overlap.
- Rebuilt all six themes around one stable geometry contract. Theme changes modify palette and surface character only; sidebar width, card dimensions, control heights, gaps and breakpoints remain unchanged.
- Made AMOLED true black, Graphite neutral charcoal, Ocean teal, Warm Ivory warm light and Light Gray cool light across Dashboard, Settings, Vault, dialogs, status surfaces, tooltips and popup.
- Kept Testing Full Access with no trial enforcement.

## 0.7.4 R1
- Fixed the inline handler menu closing while its own search box receives focus or the page rescans.
- Always-On Handler now defaults on globally in the Developer build with a local per-site Off exception list.
- Added safe LastPass URL repair: old http URLs are upgraded to HTTPS, bare domains are normalized, and domain-like imported item names such as 9gag.com can provide a safe HTTPS fallback.
- VaultCove still refuses to fill passwords over plain HTTP.
- Reworked all six theme palettes so AMOLED, Graphite, Ocean, Warm Ivory, Light Gray, and Midnight use consistent surface, field, navigation, dialog, status, Vault-card, and tooltip colors.
- Extended the same theme contract to the toolbar popup.
- Added regression coverage for handler-menu persistence, per-site exclusions, legacy LastPass URL repair, and theme differentiation.

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
