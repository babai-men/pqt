# Security Policy — pQT Protocol

## Reporting a Vulnerability

If you believe you have found a security vulnerability in the pQT Protocol smart
contract (`programs/pqt_protocol`) or in any related project infrastructure
(website, deployment scripts, token authority configuration), please report it
**privately** before any public disclosure:

- Email: YOUR_EMAIL@example.com
- Telegram: @YOUR_HANDLE
- X (direct message): [@pQTprotocol](https://x.com/pQTprotocol)

Please do **not** open public GitHub issues or discuss the details in public
chats or social media until the issue has been fixed and confirmed.

When reporting, please include:

- A short description of the issue and its potential impact (loss of funds,
  denial of service, privilege bypass, etc.).
- Steps to reproduce or a proof of concept, if possible.
- The affected Program ID and network (mainnet-beta / devnet).
- Your contact details so we can follow up.

## Scope

In scope:

- The smart contract code in `programs/pqt_protocol` (the `harvest_and_distribute`
  instruction, the 80% / 15% / 5% distribution math, PDA derivation and
  account validation).
- Token authority configuration (withdraw-withheld authority, fee config
  authority, mint authority) when it allows bypassing the documented tokenomics.
- Client scripts in `/scripts` if a flaw can lead to loss of user funds during
  normal use.

Out of scope:

- Vulnerabilities in the Solana runtime, the Token-2022 program or the Anchor
  framework itself. Please report those directly to the respective projects.
- Phishing sites or impersonations that are not operated by this project.
- Issues that require physical access to a user's device or a compromised
  private key or seed phrase.

## Rewards

This project does not run a regular bug bounty program. At our sole discretion
we may reward a critical finding (for example, one that allows draining the
Vault or bypassing the fee distribution) after it has been verified and fixed.
The amount depends on severity and on the value at risk at the time of the
report.

A reward is only considered if details of the issue have not been shared with
third parties before a fix has been released and verified. Exploiting the issue
without our explicit consent is not permitted.

## Disclosure Process

We follow responsible disclosure:

1. You report the issue privately using the contacts above.
2. We acknowledge receipt and share status updates within a reasonable time.
3. Once a fix is released and verified, details may be published, in
   coordination with the reporter.

## Audits

No independent audit of the smart contract has been performed yet (the
`auditors` field in the on-chain security.txt is set to `None`). This document
and the security.txt will be updated if an audit is completed.

## More Information

- Source code: https://github.com/babai-men/pqt
- Machine-readable `security.txt` is embedded in the program binary and can be
  viewed in Solana Explorer on the program's Security tab
  (see [solana-security-txt](https://crates.io/crates/solana-security-txt)).
