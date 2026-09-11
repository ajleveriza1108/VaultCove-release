# VaultCove 0.9.5 Release Checklist

- [ ] Private source repo committed and clean
- [ ] `npm test`
- [ ] `npm run e2e`
- [ ] SBOM generated/verified
- [ ] No private keys/secrets/source maps/remote executable code in runtime
- [ ] DEV ZIP is a real ZIP and extracts cleanly
- [ ] Full-source updater is a real ZIP with intended BAT+PS1 layout
- [ ] Store preflight ZIP is a real ZIP
- [ ] Store manifest has no unintended mandatory broad host access
- [ ] Store content-script/injection strategy matches CWS packaging policy
- [ ] SHA-256 computed over each actual artifact
- [ ] Signatures verify against actual artifact hashes
- [ ] `release.json` matches artifact names/hashes
- [ ] `latest.json` matches final release and download URL
- [ ] Public README/changelog updated only after the artifacts exist
