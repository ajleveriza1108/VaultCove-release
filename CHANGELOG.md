# VaultCove 0.7.67 R1

- Permanently pins the official Chrome Web Store item ID to `dpbmpmnibbpimedfmanoabihipdfdfko` in the release contract.
- Adds a release-boundary test that fails if the official Store ID drifts or if an unsupported `manifest.id` field is introduced.
- Keeps the Web Store package free of a fabricated manifest `key`; the existing Developer Dashboard listing remains the authority for the permanent Store ID.
- Documents that sideloaded/unpacked builds may use a different local extension ID until the matching Chrome Web Store public key is captured and embedded deliberately.
- Preserves the permanent 132-character manifest-description gate and all 0.7.66 security/licensing behavior.

# VaultCove 0.7.66 R1

- Added a permanent Chrome Web Store manifest-description release gate.
- Build/package validation now rejects any manifest description over 132 characters before release ZIP creation.
- Preserved Web Store update compatibility and the separate Buyer sideload package.

# VaultCove 0.7.65 R1

- Premium account header updates live without page refresh.
- Licensed Devices refreshes immediately after Premium activation/lease changes.
- Background metadata-only security-email notices now cover password changes and protected Cards, Bank Accounts, Identities, and Secure Notes changes.
- Cards, Bank Accounts, Identities, and Secure Notes require master-password authorization on a fixed five-minute protected-category window.
- Universal theme readability/contrast pass.

# VaultCove 0.7.65 R1

- Changes the private owner-admin `VCA-...` entitlement from seven devices to **unlimited devices**. Standard Payhip Premium remains capped at seven devices.
- Uses `licenseType=admin`/`source=admin` as the authoritative unlimited policy, so an already-provisioned owner-admin row becomes unlimited immediately after the updated Apps Script is deployed; no new admin key is required.
- Uses signed/API `maxDevices: 0` only as the owner-admin unlimited sentinel while retaining the protocol-v3 signed lease shape.
- Prevents owner-admin activation and restore flows from entering seven-slot/reclaim logic. Existing active/retained admin devices never block activation of another admin device.
- Keeps every admin device in the server ledger so **Licensed devices** can show all installations and the owner can Release or Revoke unfamiliar devices after the existing email-verification management gate.
- Adds editor-only Apps Script owner controls: `vcAdminListOwnerDevices()`, plus Release/Revoke/Restore wrappers using temporary `VC_ADMIN_TARGET_DEVICE_ID`; these helpers are not exposed through the public web-app `doPost()` API.
- `vcInitializeLicenseSheets()` now normalizes existing admin-license `maxDevices` cells to the `0` unlimited sentinel for clear Sheet state while standard licenses remain `7`.
- Updates License & Devices UI to display **Owner-admin - unlimited devices** and actual active/retained counts instead of `x of 7`.
- Preserves 0.7.63 live website login refresh, compact popup, 0.7.62 Identity protection/readability, 0.7.61 hard Master-password lock routing, Free Forever boundaries, encryption, and full-source publishing.

# VaultCove 0.7.63 R1

- Removes the **Always-On Handler** permission/status banner from the extension-icon popup. Website-handler configuration remains in Settings; the compact popup focuses only on vault unlock, search, trusted-site matches, Secure Login, and adding a login.
- Adds a live encrypted-vault change notification path from the dashboard to the background worker. Adding, editing, merging, or importing login credentials now refreshes already-open VaultCove website handlers immediately.
- Open webpages no longer need to be refreshed before newly added/imported matching logins are recognized. Existing Strict Secure Login hostname matching and encrypted-at-rest storage remain unchanged.

# VaultCove 0.7.62 R1

- Improves **Warm Ivory readability across the extension and dashboard** by raising muted-text, placeholder, disabled-control, Premium locked-card, migration-preview, helper-text, warning/error, and secondary-label contrast while preserving the Warm Ivory palette.
- Replaces visible dialog-header **Close** text buttons with compact accessible **X** buttons; `aria-label="Close"` remains for screen readers.
- Renames Migration Center **Supported today** to **Supports the following Password Managers**.
- Hardens **Identities** as a protected view: entering Identities requires a fresh master-password Sensitive Access verification, direct URL navigation cannot bypass the gate, and the view automatically closes when Sensitive Access expires.
- Identity records are excluded from the generic All-vault/global-search results and Dashboard Recent Activity so names/email metadata are not exposed outside the authenticated Identities view.
- Existing Identity details continue to live only inside the encrypted VaultCove envelope; opening/editing an Identity remains protected by Sensitive Access.
- Preserves 0.7.61 hard Master-password lock routing, 0.7.60 durable setup completion, 0.7.59 Premium/Free backup boundaries, secure sharing, local-only search, Warm Ivory default, licensing, and full-source publishing.

# VaultCove 0.7.61 R1

- Fixes the remaining **lock-screen race** directly at the UI-routing layer. A normal Lock or idle auto-lock now establishes a hard `locked` UI state before any later asynchronous setup/import callback can render.
- Adds a central `enterLockedUi()` path used by both manual Lock and inactivity auto-lock. It clears decrypted UI, invalidates older UI transitions, and always selects the existing-vault **Master password** gate.
- Adds `uiRouteEpoch` transition invalidation so asynchronous first-run creation/Finish Setup work that started before a lock cannot repaint **Set up VaultCove** or **Finish setting up VaultCove** after the session has expired.
- Adds a pure `resolveVaultGatePanel()` routing invariant: an established vault without an active decrypted session resolves `setupPanel` to `unlockPanel`.
- Adds a CSS defense-in-depth rule that makes `#setupPanel` impossible to display while the dashboard session state is `locked`, even if stale code toggles the setup panel's classes.
- Fresh installations with no encrypted vault still use the normal setup wizard. An unfinished setup can still resume **after the user first enters the existing master password**; the locked state itself never shows onboarding.
- Preserves 0.7.60 atomic Finish Setup, 0.7.59 Premium boundaries/Free Login-only backups/Warm Ivory default, and all existing encrypted vault data.

# VaultCove 0.7.60 R1

- Fixes the remaining first-run/lock race: **Finish Setup now commits the permanent setup-complete marker and all validated startup settings atomically before optional import/merge work can be interrupted by auto-lock.**
- Adds `completeFirstRunSetup()` as the single durable transaction for setup completion; the marker and settings can no longer be split across two writes.
- Repairs installations left in the old partial-commit state where the encrypted vault exists and `firstRunSecurityComplete` is already true but the permanent setup marker is still false. Those installations are promoted automatically and route to the Master password screen after lock.
- Explicitly refreshes the idle session at the start of Finish Setup. If the session has already expired, VaultCove routes to the Master password gate instead of continuing setup work against a dead session.
- Once validated Finish Setup is durably committed, later optional startup import/merge failure or auto-lock cannot reopen onboarding. The user unlocks with the existing master password and returns to the normal dashboard.
- Reapplies the user's startup profile/security choices after any optional backup merge so imported portable settings cannot override the choices made in the current setup window.
- Preserves 0.7.59 Premium boundaries, Free Login-only `.vcvault` backups, Warm Ivory default theme, distinct backup verification action, encrypted vault mirror, exact inactivity auto-lock, local search, Share workspace, and full-source publishing.

# VaultCove 0.7.59 R1

- Makes the startup `.vcvault` **Verify for merge** action permanently visually distinct from Cancel and other secondary controls across every theme, including Warm Ivory.
- Moves **Cards, Secure Notes, TOTP Codes, Favorites, Security Center, and encrypted `.vckey` to TXT conversion** into Premium Lifetime. Free Forever keeps login credentials, bank accounts, identities, password generator, import/restore, Secure Login, email sharing up to five recipients, settings, and the local encrypted vault.
- Premium gates are enforced at navigation, item-open/edit, item-save, Favorites mutation, Security Center entry, TOTP field editing/generation, card/note creation, and `.vckey` plaintext-conversion actions. Premium data already stored locally is never deleted if Premium becomes inactive; it remains encrypted and becomes accessible again after Premium is restored.
- Free Forever encrypted `.vcvault` creation is now **Login credentials only**. All other backup component selectors remain visible but disabled with Premium labeling. Runtime export enforcement also forces Login-only selection even if the page DOM is modified.
- Free `.vcvault` files no longer include private sharing identity, trusted-share recovery history, VaultCove authenticator configuration, device-recovery token, folders, cards, banks, identities, notes, TOTP overlays, Trash, Favorites, or portable settings. Premium Lifetime keeps the full selectable backup model and unified recovery material.
- Changes the default VaultCove appearance for new/unconfigured installations to **Warm Ivory**. Existing users keep their explicitly saved theme. Invalid/missing theme values now normalize to Warm Ivory.
- Updates the Premium catalog and Free/Premium disclosure text to match the new feature boundary.
- Preserves 0.7.58 post-lock Master-password routing, permanent setup state, redundant encrypted vault-envelope mirror, exact inactivity auto-lock, full local search, Share workspace, and full-source publishing.

# VaultCove 0.7.58 R1

- Fixes the critical post-lock routing bug: once a real VaultCove vault has been created, normal Lock, inactivity auto-lock, Chrome restart, and dashboard reopen must route to the existing-vault **Master password** unlock gate instead of the first-run **Set up VaultCove** screen.
- Adds a redundant encrypted local vault-envelope mirror. Every legitimate encrypted-envelope write now updates the primary and mirror atomically; if the primary key is unexpectedly absent while the mirror remains, VaultCove restores the primary automatically before deciding whether setup is required.
- A completed-installation marker can no longer be converted back into first-run setup by a stray initialize/setup request. `beginFirstRunSetup()` now refuses to run when an encrypted vault already exists or setup was already completed.
- `initializeVault()` also refuses to create a replacement vault when the permanent completed-installation marker already exists, preventing an accidental setup path from overwriting an established installation.
- `markFirstRunSetupComplete()` now verifies that a real encrypted vault exists before committing the permanent setup-complete marker.
- If the permanent setup-complete marker exists but both encrypted envelope copies are unexpectedly unavailable, VaultCove still classifies the installation as **locked**, never as first-run. This prevents the startup wizard from appearing after a lock and surfaces the problem through the master-password path instead of offering destructive re-setup.
- Extends the session/setup regression suite with lock-routing, encrypted-envelope persistence, mirror self-healing, and completed-installation no-restart checks.
- Preserves 0.7.57 full local email/username search, centered dashboard summary icons, exact inactivity auto-lock, Share workspace, Free Forever, Premium Lifetime, backups, TOTP, Secure Login, and existing encrypted vault data.

# VaultCove 0.7.57 R1

- Fixes popup credential search so typing the third and later characters of a saved email/username continues matching correctly. The search is now performed locally in the background against the full decrypted in-memory username/email while the popup still receives only the existing masked public summary.
- `GET_ACTIVE_TAB_LOGINS` now accepts a bounded local `query`, uses the same full credential search logic as the website handler, and never exposes the full username/password to the popup.
- Removes popup filtering against `usernameMasked`, which was the exact two-character regression: masked email summaries intentionally reveal only the first two local-part characters, so a third typed character could never match.
- Keeps Strict Secure Login site-bound: popup search only filters credentials already trusted for the current HTTPS site; unrelated credentials are not offered.
- Reasserts dashboard search against full local username/email, website URL, tags, cardholder and bank fields while excluding passwords and TOTP secrets from the search haystack.
- Hardens dashboard summary-card icon geometry so every icon container and SVG glyph is centered horizontally and vertically inside its own box across themes and compact mode.
- Preserves 0.7.56 exact idle auto-lock, permanent setup completion, Share workspace, Free Forever, Premium Lifetime, encrypted backups, TOTP, Secure Login, and all existing vault data.

# VaultCove 0.7.56 R1

- Fixes **idle auto-lock** so the dashboard locks according to the configured inactivity timer even when the dashboard tab remains open or Chrome throttles the page timer.
- Auto-lock is now scheduled as an exact one-shot Chrome alarm whenever the vault is unlocked or real user activity refreshes the session.
- Expired sessions can no longer be resurrected by a late mouse/keyboard event: `touchSession()` refuses to extend a session whose deadline already passed.
- Dashboard activity now recognizes pointer, keyboard, wheel, scroll, touch, focus, and visibility-return activity while remaining throttled to avoid unnecessary writes.
- Adds a permanent local **setup-complete marker** stored outside the portable backup settings. Once Finish Setup succeeds, normal vault locking/unlocking can never send that installation back through first-run setup.
- Existing installations with a completed setup are migrated automatically to the permanent local marker.
- `.vcvault` restore/import no longer carries `firstRunSecurityComplete`, so restoring a backup cannot reset this device's onboarding status.
- Once setup is complete, attempts to write `firstRunSecurityComplete:false` are normalized back to complete; the marker disappears only when Chrome removes the extension's local storage.
- Preserves VaultCove 0.7.55 Share workspace, email-recipient sharing, Free Forever, Premium Lifetime, seven-device licensing, encrypted backups, TOTP, Secure Login, and all existing vault data.

# VaultCove 0.7.55 R1

- Rebuilds VaultCove as the normal self-contained **FULL-SOURCE UPDATE + TEST + BUILD + TWO-REPO PUBLISHER**.
- Replaces the technical Shared screen with a simple **Share** workspace containing **Email Recipients**, **Share Passwords**, **Shared Passwords**, and **Convert encrypted .vckey to TXT**.
- Email recipients are stored inside the encrypted local vault and can be added, edited, or removed. Free Forever supports up to 5 saved recipients; Premium Lifetime removes that limit.
- Removes personal sample names from recipient forms.
- Removes the user-facing recipient `.vckey` exchange and Sharing Identity ceremony from normal password sharing. Legacy `.vckey` support remains only for the separate encrypted `.vckey` to TXT conversion and older compatibility paths.
- New `.vcshare` v4 files are bound to the intended recipient email without storing the readable recipient email in the package. Import requires the exact assigned email plus the recipient's own local VaultCove master-password authorization.
- Keeps received credentials sealed locally as immutable Secure-Login-only Shared Keys.
- Cleans up Premium Lifetime card sizing and wording; Secure Sharing itself is Free Forever while Premium provides unlimited saved recipients.
- Preserves the 12-character strong master password, Free Forever core vault, Premium Lifetime entitlement, seven-device Premium licensing, startup import/merge, encrypted backups, Authenticator protection, and existing legacy share import compatibility.

# VaultCove 0.7.53 R1

- Reworks **Security Health** so weak passwords explicitly count as risk.
- Adds a unique **At-risk credentials** count: a login that is both weak and reused is counted once in the at-risk total, while both risk findings remain visible.
- Separates the numeric **Security score /100** from **At-risk credentials** and **Risk findings**, eliminating the confusing `0 At risk` presentation.
- Adds an **At risk** Security Center filter for credentials that are weak, reused, or both.
- Security score severity now changes between Protected, Needs attention, At risk, and High risk.
- Free users still see Password Age Review as a locked Premium feature with a purpose tooltip, without exposing Premium-only result counts.
- Redesigns the Security Center overview into a compact risk dashboard.
- Redesigns the Premium tab into grouped **Advanced Security**, **Secure Sharing**, and **Premium Devices** sections with a clearer Lifetime/no-subscription/7-device summary.
- Replaces repetitive disabled `Premium required` buttons with cleaner locked-state indicators.
- Adds accessible hover/focus purpose tooltips to visible locked Premium features.
- Redesigns the locked Shared page as a four-step secure-sharing workflow.
- Keeps the Premium tab hidden after Premium Lifetime activation.
- Preserves Free Forever, 12-character strong master password, startup import/restore behavior, protected Shared Keys, duplicate-aware merge, and the compact profile cropper.

# VaultCove 0.7.52 R1

- Makes the profile-photo cropper substantially more compact while keeping the crop preview, Zoom control, privacy note, Cancel, Close, and **Use cropped photo** controls visible and non-overlapping.
- Constrains the crop dialog to the available viewport height and removes the excess empty space shown in compact extension windows.
- Premium features now remain visible to Free users instead of disappearing completely. Locked features are visibly unavailable and expose a tooltip explaining the purpose of that specific feature.
- Adds a dedicated **Premium** navigation tab for Free users with a complete Lifetime Premium feature catalog.
- The Premium tab automatically disappears as soon as Premium Lifetime is active.
- The Shared page now previews its Premium tools while locked rather than replacing the whole page with only an upgrade message.
- The Security Center now shows locked previews for Password age review and Encrypted activity history while keeping Free weak/reused-password checks fully usable.
- Adds a Premium marker and purpose tooltip to the locked Shared navigation item.
- Preserves Free Forever, Premium Lifetime, 7-device Premium entitlement, 12-character strong master password, required Security notice email, startup CSV/.vcvault merge behavior, Enter-to-create-vault, and the non-overlapping startup Merge action.

# VaultCove 0.7.51 R1

- Fixes the startup password-manager **Merge** button overlap after CSV detection.
- Moves the source-aware Merge action above the import preview/statistics so a long preview cannot cover it or push it against the bottom scroll boundary.
- Removes the sticky-bottom implementation that made the action appear clipped or almost invisible.
- Gives the active Merge action a full-width, high-contrast primary-button treatment.
- Keeps the disabled pre-vault Merge action visible but clearly inactive; creating the encrypted vault still automatically applies the prepared import.
- Startup import auto-scroll now prioritizes the visible Merge action.
- The label remains dynamic for every supported detected password manager.
- Preserves Enter-to-create-vault, duplicate-aware merge, Free Forever, Premium Lifetime, Shared Keys, required Security notice email, and the always-visible Finish Setup footer.

# VaultCove 0.7.50 R1

- Adds **Enter-to-create** on first master-password setup in both the extension popup and the full-page startup screen. When all strong-password requirements pass and both password fields match, pressing Enter activates **Create vault** without requiring a mouse click.
- Makes the password-manager merge CTA universal: every successfully detected supported CSV import now exposes a visible **Merge <detected source>** action, not only a particular manager.
- Before the encrypted vault exists, the detected-source merge action remains visible but disabled with an explanation; creating the vault automatically applies the prepared import.
- After the encrypted vault exists, the same detected-source merge action is enabled immediately and uses the existing duplicate-aware merge engine.
- Keeps the merge CTA sticky inside the scrollable startup import column so it cannot disappear below a long import preview.
- Preserves Free Forever, Premium Lifetime, 12-character strong master-password policy, required Security notice email, automatic startup CSV detection, duplicate-aware merge, protected Shared Keys, and always-visible Finish Setup.
- No Apps Script redeployment is required.

# VaultCove 0.7.49 R1

- **Startup import commit repair:** a password-manager CSV selected and detected during startup is now automatically applied before **Finish Setup** opens the dashboard.
- A verified `.vcvault` selected during startup is likewise applied automatically on **Finish Setup** if the user has not already used the explicit merge button.
- **Finish Setup is now the transaction boundary for all startup choices:** profile name, cropped profile photo, security notice email, email-notice preference, auto-lock interval, Authenticator state, and prepared import/restore are committed before the dashboard opens.
- If a selected CSV preview is stale or missing, Finish Setup re-detects it locally before applying it. If a selected `.vcvault` or Backup Key changed, Finish Setup re-verifies the backup before applying it.
- User-selected startup profile photo and locally configured Authenticator take precedence over imported portable settings, preventing a backup from silently undoing choices made in the current startup screen.
- Successful imports performed during initial vault creation are consumed immediately so they cannot be applied a second time when Finish Setup is clicked.
- Startup import previews now explicitly state that the prepared import will be applied automatically by Finish Setup.
- Preserves Free Forever, Premium Lifetime, 12-character strong master password, required valid Security notice email, protected Shared Keys, duplicate-aware merge, and the 0.7.48 critical Finish Setup repair.

# VaultCove 0.7.48 R1

- **Critical Finish Setup repair:** removes the stale `acknowledgeTrialNotice()` call that caused `acknowledgeTrialNotice is not defined` after completing startup.
- **Finish Setup now completes normally** under the Free Forever + Premium Lifetime model, saves first-run security settings, and opens the dashboard.
- Removes the obsolete restore/import runtime error that still required an active trial or license; encrypted `.vcvault` restore remains a Free Forever core feature.
- Removes remaining active-runtime 7-day-trial wording from the first-run completion path and plan badge.
- Keeps the uninstall/encrypted-backup acknowledgement required, with wording that matches the current Free Forever model.
- Adds release-blocking regression checks so any future `acknowledgeTrialNotice` reference fails static security validation.
- Expands stale-trial checks to reject `7-day trial`, `active trial`, and `trial or license` runtime wording.
- Preserves the 12-character strong master password, required valid Security notice email, always-visible Finish Setup footer, duplicate-aware import/merge, protected Shared Keys, Premium Lifetime gates, and seven-device Premium licensing.
- No Apps Script redeployment is required.

# VaultCove 0.7.47 R1

- Replaces the expiring 7-day trial with **VaultCove Free Forever**.
- Free Forever never locks the user's local encrypted vault and keeps core editing, Secure Login/Autofill, TOTP, generator, import/export, `.vcvault` backup/restore, duplicate-aware merge, and weak/reused-password Security Center available.
- Existing valid signed Standard/owner-admin licenses are treated as **Premium Lifetime** with no annual subscription and up to 7 Premium device slots.
- Premium Lifetime unlocks **Advanced Security Center** password-age review and tamper-evident encrypted activity history.
- Premium Lifetime unlocks **Secure Sharing and Shared Keys** (`.vckey` / `.vcshare`) while received use-only shared credentials remain protected inside the Shared Keys system folder.
- Removes trial welcome/last-day/expiry notifications, trial expiry redirects, trial feature lockouts, and the trial modal.
- Replaces the startup trial badge with **Free Forever** and keeps the uninstall/encrypted-backup acknowledgement as a required setup safety disclosure.
- Deletes obsolete local trial timing/anchor metadata during normal initialization.
- Updates License & Devices to show Free Forever or Premium Lifetime clearly and preserves the existing privacy-preserving seven-device Premium management workflow.
- Chrome Web Store build remains Store-managed for updates and contains no Developer GitHub updater.
- No Apps Script redeployment or signing-key rotation is required.

# VaultCove 0.7.46 R1

- Makes **Finish Setup permanently visible** in the first-run rectangular window, even when `.vcvault` verification, preview, duplicate analysis, or merge controls expand the left column.
- Restructures the desktop setup card into three invariant rows: header, bounded setup content, and a reserved footer containing both acknowledgements, errors, **Finish Setup**, and privacy indicators.
- Setup columns now scroll internally only when their content exceeds the available viewport height, so no startup element is clipped or pushed outside the window.
- `.vcvault` verification and merge results automatically scroll into view inside the Profile/Backup/Import column without moving the Finish Setup footer.
- Adds a sticky footer fallback on narrow windows so Finish Setup remains reachable there as well.
- Preserves required security notice email validation, Shared Keys organization, support email copy, 12-character strong master-password policy, duplicate-aware merge, anchored 7-day trial, licensing, and seven-device management.
- No Apps Script redeployment is required.

# VaultCove 0.7.45 R1

- Adds a **copy icon** beside the developer support email. Clicking it copies only `leveriza.aj08@gmail.com` to the clipboard; the normal email link remains available.
- Adds an automatic protected **Shared Keys** system folder for received use-only `.vcshare` login access.
- Every newly received shared login is placed directly in **Shared Keys**.
- Existing received shared logins from earlier VaultCove versions are automatically organized into **Shared Keys** the next time the unlocked vault starts.
- **Shared Keys** cannot be renamed or deleted, and normal owned logins cannot be manually moved into it.
- Makes **Security notice email** mandatory during first-run setup.
- **Finish Setup** remains disabled until the encrypted vault exists, the security notice email has a valid local email syntax, and both required acknowledgements are checked.
- Improves local email validation for address length, local-part rules, domain labels, repeated dots, and top-level domain format. VaultCove does not perform online mailbox verification.
- Renames **Finish security setup** to **Finish Setup**.
- Preserves 12-character strong master-password policy, duplicate-aware import/merge, one-window startup, dashboard setup gate, anchored 7-day trial, licensing, and seven-device management.
- No Apps Script redeployment is required.

# VaultCove 0.7.44 R1

- Changes the VaultCove **master password minimum from 16 characters to 12 characters**.
- Retains every strong-master-password rule: lowercase, capital letter, number, special character, no 3 repeated characters, no common word/sequence, at least 8 unique characters, and confirmation match.
- Applies the 12-character minimum consistently to popup first-vault creation, full-page first-run creation, and **Change master password**.
- Keeps ordinary saved-login password health separate; this change is specifically for the VaultCove master password.
- Scrutinizes and improves startup typography, spacing, control height, helper text, security cards, acknowledgement text, and CTA readability.
- Fixes the previous low-height desktop rule that made fonts unnecessarily tiny on common displays around 720–760px tall.
- Preserves the wide one-window startup layout, always-available `.vcvault`/password-manager imports, automatic duplicate-aware merge, dashboard setup gate, anchored 7-day trial, licensing, and seven-device management.
- No Apps Script redeployment is required.

# VaultCove 0.7.43 R1

- Keeps **Restore .vcvault** and **Import password-manager backup** active after the master-password vault has already been created.
- Adds a visible **Duplicate-aware merge** policy and explicit **Merge into this vault** action.
- Password-manager imports automatically detect likely duplicates and merge matching records instead of creating duplicates.
- Login duplicate matching normalizes username + website host, so equivalent URLs such as `github.com/login` and `www.github.com/` match when the username is the same.
- Duplicate merges add missing metadata, union URLs/tags/custom fields, and combine notes.
- Existing conflicting passwords, TOTP secrets, card numbers, security codes, bank identifiers, PINs, and government IDs are preserved rather than overwritten automatically.
- Re-importing the same source is idempotent and reports already-matched records.
- Verified `.vcvault` files can be merged into the already-created startup vault without replacing existing current credentials.
- Migration Center now uses the same automatic duplicate-aware merge policy; the old duplicate-handling checkbox is removed.
- Improves startup GUI with **Finish setting up VaultCove**, active import cards, merge statistics, stronger hierarchy, and reduced dead space.
- Preserves one-window startup, profile/photo crop, security settings, both acknowledgements, dashboard setup gate, anchored 7-day trial, licensing, and seven-device management.
- No Apps Script redeployment is required.

# VaultCove 0.7.42 R1

- After the encrypted vault/master password has been successfully created or restored, the redundant **Master password** column is removed from the remaining first-run setup.
- The same rectangular startup window remains open and automatically redistributes **Profile & existing passwords** and **Security essentials** into two balanced columns.
- The redundant **Encrypted vault is ready** status line is hidden after vault creation.
- The dashboard remains blocked until setup is complete.
- Preserves name/profile-photo setup, `.vcvault` restore, automatic password-manager import detection, security email, auto-lock, Authenticator, backup reminder, secure defaults, both acknowledgements, anchored 7-day trial, licensing, and seven-device management.
- No Apps Script redeployment is required.

# VaultCove 0.7.41 R1

- Blocks Open dashboard while VaultCove is still in master-password setup state.
- Fixes the startup width regression caused by the stronger legacy `html.vaultLocked .gateCard` 470px rule.
- Forces the unified startup to a real rectangular desktop window up to 1320px wide.
- Spreads Profile/Import, Master Password, and Security Essentials across three balanced columns.
- Preserves name, profile-photo crop, .vcvault restore, automatic password-manager import, password requirements, security email, auto-lock, Authenticator, backup reminder, secure defaults, both acknowledgements, and Finish security setup in one window.
- Preserves the anchored 7-day trial and licensing/recovery behavior.
- No Apps Script redeployment is required.

# VaultCove 0.7.40 R1

- Replaces the two-screen first-run flow with one rectangular startup window.
- Keeps profile name, cropped profile photo, direct .vcvault restore, automatic password-manager CSV import, master-password creation, security email, auto-lock, Authenticator status, backup reminder, secure defaults, both acknowledgements, and Finish security setup in the same window.
- Fresh start remains automatic; no radio-mode selection is shown.
- Create encrypted vault no longer navigates to a second startup screen. The same window remains visible and unlocks the final security completion step.
- Finish security setup requires both acknowledgements and a successfully created/restored local vault.
- Desktop layout uses three columns and compact height handling to fit a normal rectangular browser window without vertical scrolling; narrow windows fall back safely to scrolling.
- Preserves anchored 7-day trial, unified .vcvault recovery, automatic CSV source detection, Payhip licensing, and seven-device management.
- No Apps Script redeployment is required.

# VaultCove 0.7.39 R1

- Repairs the full-source updater baseline gate that incorrectly omitted installed version 0.7.37.
- The publisher now accepts every supported VaultCove baseline from 0.7.22 through 0.7.39, including 0.7.36, 0.7.37, and 0.7.38.
- Adds a publisher self-check that confirms the immediately previous release is present in the accepted baseline list before any project mutation begins.
- Preserves all 0.7.38 product behavior: two-screen startup, local name/profile-photo setup, cropped photo persistence, direct .vcvault restore, automatic password-manager import detection, anchored 7-day trial, acknowledgement gate, Payhip licensing, and seven-device management.
- No Apps Script redeployment, private-key change, pepper change, or licensing reset is required.

# VaultCove 0.7.38 R1

- Fixes the startup-layout regression: the 0.7.36/0.7.37 override was written into `vault.css` with literal escaped-newline text, so Chromium ignored the intended layout rules. This caused native file inputs to stay visible, restore/import button text to overlap, and the second-screen two-column layout not to apply.
- Rebuilds startup as visibly labeled Step 1 of 2 and Step 2 of 2.
- Step 1 keeps automatic fresh start and only two direct optional actions: Restore .vcvault and Import password-manager backup. No radio buttons and no default fresh-start pseudo-selection card.
- Step 2 explicitly retains Your name, Profile photo, local crop/persistence behavior, Security notice email, Auto-lock, Authenticator, secure defaults, and both required acknowledgements.
- Restores truly hidden native file inputs so only the styled restore/import buttons are visible.
- Uses wide two-column desktop layouts with larger typography and compact-height handling so both screens fit a normal desktop viewport without vertical scrolling. Small windows safely fall back to a scrollable single column.
- Preserves anchored 7-day trial, unified .vcvault recovery, automatic CSV source detection, Payhip licensing, and seven-device management.
- No Apps Script redeployment is required.

# VaultCove 0.7.37 R1

- Repairs the self-contained Windows update/publish package: restores the missing `Write-Step`, `Write-Pass`, `Write-Info`, and `Write-Warn` helpers before the pipeline starts.
- Rebuilds the BAT launcher as UTF-8 without BOM so `@echo off` is honored by Windows CMD and command lines are no longer echoed.
- Adds publisher-package regression validation so every `Write-*` logging helper referenced by the PS1 must be defined before release packaging.
- Preserves the complete 0.7.36 product behavior: direct startup Restore `.vcvault` / Import password-manager backup actions, automatic fresh start, automatic CSV source detection, two-screen readable Security Essentials, required acknowledgement gate, anchored 7-day trial, unified `.vcvault` recovery, Payhip licensing, and device management.
- No Apps Script redeployment or signing-key rotation is required.

# VaultCove 0.7.36 R1

- Removed the first-run radio/selection chooser. Starting fresh is now automatic when the user creates a vault without importing anything.
- Added two direct startup actions: **Restore .vcvault** and **Import password-manager backup**. Clicking either immediately opens the relevant file picker.
- Password-manager CSV source detection now runs automatically as soon as the user chooses an export; there is no source selector and no extra detect button.
- .vcvault restore remains locally authenticated with its unique Backup Key before the backup can be applied.
- Rebuilt first-run Security Essentials into a two-column second screen so the startup flow fits within one normal desktop viewport without requiring vertical scrolling.
- Increased first-run headings, labels, controls, explanatory text, acknowledgements, and action-card font sizes for readability instead of shrinking the UI to fit.
- Preserved the acknowledgement gate: Finish security setup remains disabled until both required acknowledgements are checked.
- Preserved the anchored 7-day trial, unified .vcvault recovery, cropped profile-photo persistence, automatic migration detection, and the production licensing backend unchanged.

# VaultCove 0.7.35 R1

- Added an immutable local trial anchor so the original 7-day trial start/expiry survives normal reloads and source/version updates and cannot be restarted by loss/reset of the mutable trial-state record.
- Added visible trial start/end timestamps in License & Devices.
- Reworked first-run creation into three explicit paths: Start fresh, Restore .vcvault, or Import password-manager logins.
- Restore/import selections must be verified locally before Create vault can continue.
- Finish security setup is disabled until both required acknowledgement checkboxes are selected.
- Preserved unified .vcvault recovery, automatic CSV source detection, cropped-profile persistence, and overlap-safe backup preview.
- No Apps Script/licensing backend change is required.

# VaultCove 0.7.34 R1

- Consolidated backup and disaster recovery into encrypted `.vcvault`; removed separate recovery-container runtime.
- Backup snapshot v3 automatically includes protected sharing identity, trusted-share recovery metadata, VaultCove authenticator configuration, and device-recovery token.
- Every new `.vcvault` uses a fresh random 256-bit Unique Backup Key required for preview/restore.
- Added startup `.vcvault` restore and password-manager CSV import.
- Removed Migration Center source-format selector and added automatic supported CSV layout detection.
- Fixed first-run cropped profile-photo persistence/preview.
- Fixed `.vcvault` restore preview overlap with wrapping/overflow-safe layout.
- Maintains restore compatibility with backup snapshot v2 `.vcvault` files.

# VaultCove 0.7.32 R1 — Activated-license cleanup + automatic device monitoring

- Removes the VaultCove serial, License email, device-name fields, and activation button after the current installation is already licensed.
- Keeps only the signed license status plus Refresh signed license and Release this device slot actions on an activated installation.
- Automatically shows all device records associated with the same VaultCove license using the already encrypted device refresh credential and license serial.
- Keeps device monitoring read-only by default; Retain, Release, Revoke, and Restore continue to require a fresh email OTP and short-lived management session.
- Adds the protocol-v3 `listOwnDevices` read-only action. It returns device labels, platform/app version, status, and first/last-seen times only; it never returns refresh-token hashes, recovery-token hashes, the raw serial, or vault data.
- Removes a duplicate JSON parse in the license client response path.
- Rolls the canonical Apps Script source forward to the production backend with explicit UTF-8 RSA signing and the current server-performance fixes.
- Preserves 7-day full trial, Lifetime/no-annual-subscription licensing, 7-device Standard/Admin limits, local encrypted credential storage, Payhip email binding, OTP, and Strict Secure Login.

# VaultCove 0.7.31 R1 — Production Licensing Connected

- Connects the approved Google Apps Script production `/exec` licensing endpoint.
- Embeds only the matching RSA-3072 **public** lease-verification key; server private signing material remains outside the extension.
- Enables real Payhip buyer-email + license-key activation, email OTP, signed seven-day offline leases, and the VaultCove 7-device ledger.
- Keeps both standard customer licenses and owner/admin licenses at a maximum of 7 devices.
- Keeps the 7-day full trial, Lifetime/no-annual-subscription model, encrypted local license-key/refresh-token storage, Support email, password-health drill-down, and damaged-Trash backup resilience.
- Adds production-config tests that reject an unconfigured endpoint/key or a malformed/non-RSA-3072 public key.
- No Payhip Product Secret, RSA private key, license pepper, raw owner/admin key, master password, vault key, or vault contents are embedded in the runtime.

# VaultCove 0.7.31 R1

- Clarifies the Support tab with a visible **Email me for app improvements** action to `leveriza.aj08@gmail.com`.
- Adds defense-in-depth backup recovery: damaged authenticated Trash records are skipped inside the backup snapshot, and the UI now retries without Trash if an older/edge Trash authentication error escapes the first isolation layer.
- The customer is explicitly told when damaged Trash was omitted; healthy Logins, Cards, Bank Accounts, Identities, Secure Notes, TOTP data, folders, favorites, and selected settings remain exportable.
- Adds a regression test proving the final encrypted portable export completes even with a deliberately damaged Trash envelope.
- Preserves the 7-day full trial, Lifetime/no-annual-subscription licensing, Payhip protocol v3, 7-device customer/owner limits, local encrypted license storage, Strict Secure Login, and offline-first vault boundary.
- Version advanced from 0.7.29 to 0.7.31.

# VaultCove 0.7.29 R1

- Fixed encrypted backup so an unauthenticatable/damaged legacy Trash envelope no longer blocks backup of healthy selected vault data; the damaged deleted item is explicitly skipped and counted.
- Added an app-improvement email card to Support using `mailto:leveriza.aj08@gmail.com` with a prefilled subject and a warning never to email secrets.
- Preserved Payhip and PayPal support links, local password-health drill-down, 7-day trial, 7-device licensing, and offline-first privacy boundaries.

# VaultCove Changelog

## 0.7.28 R1 — Password-health credential drill-down + Support tab

- Makes Dashboard **Weak Passwords** and **Reused Passwords** interactive; clicking either opens Security Center filtered to the exact affected credential records.
- Adds a dedicated **Password strength and reuse** section in Security Center with per-login strength bars, local Weak/Fair/Strong/Very strong labels, reuse counts, and credential-detail actions.
- Adds clickable Weak/Reused statistic cards inside Security Center.
- Keeps passwords hidden in the health list; opening credential details continues to use the existing protected item workflow and Sensitive Access for secrets.
- Adds a **Support** navigation tab.
- Adds the official Payhip store link: `https://payhip.com/AJCoderDigitalStore`.
- Adds **Buy me a decaf** PayPal support link: `https://paypal.me/ajleveriza`.
- Adds the Payhip link at the bottom-right beside **VaultCove is offline by design.**
- Support links are user-initiated only and do not send vault data, passwords, site lists, master passwords, or security scores.
- Adds no new Chrome permissions, analytics, telemetry, cloud-vault storage, or background support requests.
- Preserves the 7-day full trial, Lifetime/no-annual-subscription licensing model, protocol v3, and 7-device Standard/Owner-Admin limits.
- Version advanced from 0.7.27 to 0.7.28.

## 0.7.27 R1 — Payhip protocol v3 + encrypted license credentials + 7-device owner-admin key

- Advances the licensing protocol from v2 to v3.
- Keeps the 7-day full-feature trial and uninstall data-loss disclosure.
- Standard Payhip licenses remain limited to 7 active/retained device slots.
- Owner-admin licensing is changed from 20 devices to exactly 7 devices.
- Adds a locally generated private owner-admin key; the publisher stores its backup only as Windows-user DPAPI-protected data outside the Git repositories.
- Adds per-device AES-256-GCM protection for the license key and refresh token using a non-extractable Web Crypto key persisted as a CryptoKey in extension-origin IndexedDB.
- Removes plaintext license keys and refresh tokens from persistent Chrome local storage; legacy plaintext client state is migrated to ciphertext.
- Removes raw Payhip license-key persistence from Apps Script Script Properties. The server now keeps only HMAC hashes and requires the device-encrypted key during lease revalidation.
- Adds one-time owner-admin provisioning through temporary Script Properties; the raw owner key property is automatically deleted after provisioning.
- Payhip refund/disable state is rechecked during signed-lease refresh and before device-management authorization.
- Updates Apps Script acceptance tooling, security gates, documentation, and package publisher for protocol v3.
- Version advanced from 0.7.26 to 0.7.27.

## 0.7.26 R1 — Full-source update/publish package + 7-day trial release hardening

- Preserves the 7-day full-feature trial and clear uninstall data-loss disclosure from 0.7.25.
- Adds the required self-contained release workflow: one BAT launcher plus one PowerShell publisher, with the complete source embedded in the publisher payload.
- Updates the canonical project at `D:\\Windows Projects\\VaultCove`, creates a rollback backup first, runs all mandatory tests, builds signed developer/store/update artifacts, then updates both VaultCove repositories.
- Includes the source-only Google Apps Script licensing backend in the canonical source while keeping it out of runtime extension packages.
- Adds release-boundary testing to the publisher gate and keeps source/release publication fail-closed.
- Version advanced from 0.7.25 to 0.7.26.

## 0.7.25 R1 — 7-day full trial + uninstall data-loss disclosure

- Replaces permanent testing Full Access with a real 7-day full-feature trial.
- Trial starts when the extension is first initialized and runs for exactly seven days.
- All normal VaultCove features remain available during the active trial.
- After expiry, normal vault operations are disabled while encrypted Backup & Restore and License & Devices remain available.
- Adds trial countdown labels in the dashboard and popup.
- Adds first-run, dashboard, browser-notification, final-day, and expiry notices.
- Explicitly warns that removing the Chrome extension permanently deletes credentials stored in extension-local storage; explicitly exported encrypted backup files remain separate.
- Adds local clock-rollback resistance so moving the system clock backwards does not extend the observed trial window.
- Keeps the vault offline: trial state uses chrome.storage.local and never chrome.storage.sync.
- Fixes the background signed-license refresh alarm so an existing refresh token is actually retried against the build-managed endpoint.
- Version advanced from 0.7.24 to 0.7.25.

## 0.7.24 R1 — Final pre-licensing boundary hardening + cropped-profile Settings repair

- Repairs the remaining profile-photo visual regression by rendering the persisted crop through a dedicated `<img>` element in Settings while retaining the compact header avatar path.
- Adds profile-image decompression/dimension limits before cropping.
- Upgrades the pre-licensing client/server request envelope to protocol v2 with request IDs, cryptographic nonces, timestamps, replay rejection, response-size limits, and client timeouts.
- Adds release-boundary scanning for dangerous permissions, private-key/payment-secret patterns, dynamic code execution, source maps, unexpected externally-connectable/web-accessible surfaces, and unreviewed licensing endpoints.
- Store preflight now forcibly disables Testing Full Access and fails if a Store package can bypass production licensing.
- Developer GitHub updates remain signed and developer-only until Chrome Web Store distribution is ready.

## 0.7.22 R1 — Universal Login Compatibility + Vault Action Repair

- Fixed the selected-item drawer actions that appeared clickable but did nothing. `Open or Edit`, `Add/Remove Favorites`, and `Mark changed today` now use real CSS selector wiring instead of the ID-only helper.
- Expanded safe saved-site discovery so a credential saved for an apex domain such as `bdo.com.ph` can be recognized on its own HTTPS subdomains such as `online.bdo.com.ph`, including common multi-label public suffixes. A credential already scoped to a subdomain still cannot automatically expand into a deeper subdomain.
- Added private-hosting tenant isolation for common shared hosting suffixes such as `github.io`, `pages.dev`, `vercel.app`, `netlify.app`, and similar services so one tenant is not treated as another tenant.
- Hardened Strict Secure Login compatibility for React/Angular/Vue-style controlled login forms: username/email and password are set together, framework input/change state is synchronized only inside the one-shot Secure Login transaction, the site's visible Login/Sign in button is preferred over raw `requestSubmit()`, and the password is still cleared aggressively after submission or immediately on failure.
- Preserved one-use five-second Secure Login capabilities, exact HTTPS/tab/top-frame authorization, HTTP capture-only behavior, encrypted locked capture, sealed Trash, Use-Only sharing, selective backup, Recovery Kit, and the unconfigured commercial license transport.

## 0.7.21 R1 — Core Security Hardening

- Added one-use Secure Login capability tokens. A website handler first requests a short-lived authorization bound to one credential, one tab, the top frame, and the exact HTTPS origin; the token is deleted before the credential is released and expires after five seconds.
- Added strict runtime-message schemas and source checks. Web-handler messages carry a protocol version and request ID, unknown fields are rejected, and extension-only actions cannot be invoked by page handlers.
- Added locked/new-login capture abuse controls: bounded usernames/passwords/page metadata, a 10-minute rate window, a 20-capture per-origin limit, and a 100-capture global limit.
- Added an encrypted, tamper-evident local activity history. Security-relevant events are hash-chained inside the encrypted vault and can be verified from Security Center without storing passwords or secret values in the log.
- Added mandatory Restore Preview for `.vcvault` restores. VaultCove decrypts the selected backup after Sensitive Access, summarizes components/counts/settings, and will not enable restore until that exact file+secret combination has been previewed.
- Hardened migration parsing with a 25 MB file limit, 50,000-row limit, 256-column limit, 65,536-character field limit, 160-character header limit, and rejection of malformed unterminated quoted CSV fields.
- Added explicit vault cryptographic profile/component version metadata and downgrade checks while retaining backward compatibility with existing VaultCove envelopes that predate the profile marker.
- Preserved the 0.7.20 local profile crop, HTTP/HTTPS registration capture, HTTPS-only Strict Secure Login, folders, sealed Trash, selective backup, Recovery Kit, `.vckey`, `.vcshare`, and platform-neutral licensing groundwork.

## 0.7.20 R1 — Profile crop + HTTP/HTTPS locked capture + username/email pairing

- Added a local profile-photo crop editor with drag positioning and 1x-3x zoom before saving the cropped photo.
- The original uploaded profile image is never copied into VaultCove; only the locally generated 256 x 256 WebP crop is stored.
- Developer handler coverage now includes both HTTP and HTTPS pages so submitted registrations/logins can be detected on legacy HTTP sites.
- Locked-vault new-login capture is no longer suppressed by the content handler; it can be sealed into the public-key-encrypted local capture inbox for review after unlock.
- Strict Secure Login remains HTTPS-only and explicitly refuses to submit a saved password over HTTP.
- Strengthened username/email field heuristics for email inputs, username/email autocomplete values, labels/placeholders, and the nearest identifier field before a password field.
- Secure Login continues to inject both username/email and password when both fields are present, while password Fill remains removed.

## 0.7.19 R1 — Local profile photo + locked capture inbox + launch defaults

- Adds an optional local profile photo during first-run Security Essentials and in Settings. VaultCove resizes it on-device and stores it only in local extension storage; it is never uploaded or synced.
- Adds a public-key-sealed locked capture inbox. After the capture identity is provisioned, submitted login/registration credentials can be encrypted while the vault is locked and reconciled into the encrypted vault on the next unlock.
- Keeps use-only shared credentials out of the locked capture reconciliation path so shared secrets are not converted into owned captures.
- Removes the user-editable Apps Script license-service URL. Licensing transport is build-managed and remains unconfigured until the Payhip/Apps Script setup; Payhip/API secrets remain server-side.
- Keeps Grid as the default Vault card view.
- Makes Login credentials a mandatory component of every encrypted `.vcvault` backup; optional components can still be selected independently.
- Centers each Dashboard summary icon inside its own summary section.

## 0.7.18 R1 — Recovery verification + personalized final-stage Vault UI
- Fixes Offline Recovery Kit creation/restoration so the current Master Password is verified inside the Recovery Kit dialog instead of chaining a separate Sensitive Access dialog with the Recovery Password dialog.
- Recovery Password remains a separate 16+ character premium-strength password and cannot equal the current Master Password.
- Hiding an already revealed sharing identity is immediate and never asks for the Master Password; only Reveal Identity requires fresh verification.
- Rewords the use-only notice around Shared Key(s).
- Removes the Website thumbnails Settings card while keeping Chrome-local favicons enabled by default.
- Adds a local display name during first-run Security Essentials and in Settings; the name personalizes the top-right profile and intentional public-sharing identity exports.
- Replaces the Offline Only sidebar VC placeholder with the VaultCove shield mark.
- Vault cards/list rows are directly selectable; the right-side panel starts neutral and follows the selected item.
- Login selection adds local-only password health: Weak/Fair/Strong/Very strong, local entropy estimate, reuse status, and obvious issues without revealing the password.

# Changelog

## 0.7.17 R1

- Fixed the Backup & Restore render crash `attr is not defined`. The selective-backup option renderer and per-site handler exception rows referenced a helper that existed only inside the item-editor function, so the async Backup page could fail before creating its controls.
- Added one global `escapeAttr()` helper beside `escapeHtml()` and routed backup component keys, handler host attributes, and item-editor input values through the same defined escaping path.
- Added a regression gate that rejects the old undefined `${attr(key)}` / `${attr(host)}` markup so Backup & Restore cannot silently regress to a dead route again.
- Preserved 0.7.16 security behavior, Strict Secure Login, premium password setup, selective encrypted backups, default local website thumbnails, sealed Trash, sharing, recovery, and the Store favicon permission repair.

## 0.7.16 R1

- Fixed the Chrome Web Store package preflight that incorrectly expected `favicon` in `optional_permissions` even though VaultCove 0.7.15 intentionally makes Chrome-local website thumbnails a default premium feature.
- The Developer and Store packages now keep Chrome's local `favicon` permission as a required local API permission; the Store build still converts HTTPS site access to `optional_host_permissions` and removes Developer-only static content-script registration.
- Added an explicit Store-manifest regression gate that rejects both a missing required `favicon` permission and an accidental duplicate optional `favicon` permission.
- No vault-data migration or credential re-encryption is required; this release advances the already-applied 0.7.15 source after the publisher stopped safely in Phase 5.

## 0.7.15 R1

- Removed the saved-password **Fill** action from both the website handler and extension popup. Saved passwords can now be used on websites only through **Strict Secure Login**.
- Strict Secure Login requires a verified top-level HTTPS destination, blocks unapproved ports and unrelated subdomains, injects the password only for immediate submission, avoids normal password `input`/`change` events, and clears the field if safe submission cannot be found or shortly after submission.
- Expanded premium password creation to eight live security checks: minimum length, lowercase, capital, number, special character, no three repeated characters, no obvious/common word or sequence, and at least eight unique characters, plus matching confirmation. Added Show/Hide controls and disabled creation until every requirement passes.
- Removed the `CHANGE MASTER PASSWORD` typed phrase. Master-password changes still require the current Master Password, a valid strong replacement, confirmation, and Authenticator/recovery verification when enabled.
- Fixed Backup & Restore navigation so the sidebar route awaits its async view render and surfaces a visible Retry card if rendering fails instead of appearing to do nothing.
- Enabled Chrome-local website thumbnails by default for a premium Vault-card appearance. No third-party favicon service is contacted.
- Removed custom button tooltips and native button title attributes; icon-only controls retain `aria-label` accessibility text.
- New vaults created from the extension popup now continue directly into the one-time Security Essentials wizard instead of bypassing critical first-run setup.
- Added regression tests for handler/popup Fill removal, exact HTTPS Secure Login origin validation, top-frame-only enforcement, premium password checks, Backup navigation, default thumbnails, master-change phrase removal, and tooltip removal.

## 0.7.14 R1

- Added selective encrypted `.vcvault` backups. Login credentials are selected by default; users can opt into Folders, Cards, Bank accounts, Identities, Secure notes, TOTP codes, Trash, Favorites, and Other settings.
- New component restores replace only the selected components contained in the backup and preserve unrelated categories already in the destination vault. Legacy full `.vcvault` backups remain compatible and retain full-vault restore semantics.
- Made Folders, Favorites, and TOTP secrets independent backup overlays so a Login-only backup does not silently include them.
- Added a portable-settings allowlist that excludes device/license identity, private signing data, and platform-specific state.
- Added safe Trash portability: selected Trash records are opened only inside the authorized encrypted-backup operation and are re-sealed under the destination vault key during restore; expired Trash remains deleted.
- Excluded recipient-bound Use Only shared credentials from portable component backups so delegated login access cannot be promoted into an owned portable credential.
- Added `Login only`, `Select all`, and `Clear` backup presets plus selection count and dependency guidance in Backup & Restore.
- Rebuilt Settings as independent responsive stacked columns to remove the large blank spaces caused by shared CSS grid rows when one card, especially Change Master Password, is much taller than its neighbors.
- Added regression coverage for component selection, partial restore preservation, Trash re-sealing, settings allowlisting, Use Only exclusions, and gap-free Settings layout.

## 0.7.13 R1

- Replaced the `.vckey` File System Access save-picker path with Chromium's browser-owned `downloads.download(..., saveAs:true)` flow to fix the Windows case where the pointer could hover but render invisibly inside Save As.
- `.vckey` export now completes fresh Master Password/Sensitive Access and file-password protection before opening Save As; cancelled Save As writes nothing.
- Added the narrowly-scoped `downloads` permission for explicit user-requested encrypted exports and documented the permission for Chrome Web Store review.
- Kept restore/open-file behavior unchanged because the normal Open dialog already renders the pointer correctly.
- Added premium-style live password setup guidance for every newly created security password: the UI shows minimum length, lowercase, capital, number, special-character, confirmation-match, and a live completion meter.
- New Master Passwords and Recovery Passwords require 16+ characters; new `.vckey` and `.vcshare` package passwords require 14+ characters. Existing encrypted files remain importable with their original passwords.
- Creation/continue buttons stay disabled until the displayed requirements and confirmation match are satisfied, while import/unlock forms do not retroactively reject older passwords.

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
