# VaultCove 0.9.5 R1 Publication State

The canonical 0.9.5 R1 handoff was reconciled against the current source/test state.

Validated artifact identities:

- VaultCove-0.9.5-R1-BUYER.zip
  SHA-256: 3ED3F2FC25503B2BC0112ABF9FEDF5FC383C5E3CA2C174C1CCC3C03D77F852EB
- VaultCove-0.9.5-R1-WEB-STORE.zip
  SHA-256: 4C5BB357CC8D6C6BE145081F55191BE5ADF483129E58F5B0B4A5C3CD977E307B
- VaultCove-0.9.5-R1-FULL-SOURCE-UPDATE-BUILD-1BAT-1PS1.zip
  SHA-256: CAE993189D5DFD68B259B49227AB89B748F0D66F09BFF9D17F5B982012A9EDA6
- VaultCove-0.9.5-Code.gs
  SHA-256: DC3B9E45653ED5A678AFFFEC4D7F03D1173B5170C3ACDCD8DD06F3E1BC3A29E9

CI distinction:
- local/runtime/package PASS claims remain PASS only where the canonical validation says executed.
- CodeQL, OSV and Puppeteer E2E remain CONFIGURED unless separate run evidence exists.

This file does not itself sign or promote latest.json.
