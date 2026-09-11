# VaultCove 0.9.5 Release Preparation State

VaultCove 0.9.5 is currently a development snapshot. Do not advertise it as the current public release until build/package/signature/store-preflight gates are complete.

Required before publication:
- immutable source commit;
- full `npm test` pass;
- browser E2E pass;
- real DEV/updater/Store ZIP artifacts;
- artifact SHA-256 and signature verification;
- Store manifest least-privilege verification;
- `release.json`/`latest.json` generated from the exact artifact bytes.
