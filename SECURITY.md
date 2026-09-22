# Security Policy

## Secrets and Sensitive Data

This repository must never contain:

- SMTP passwords, API keys, or Google App Passwords
- GoPhish administrator passwords
- Private keys or session cookies
- Real recipient lists or personal information
- GoPhish databases or unredacted logs
- Unique live campaign links or tracking identifiers
- Real passwords, MFA codes, or recovery codes

If a secret is committed, revoke or rotate it immediately before removing it from Git history. Deleting the visible file alone is not sufficient.

## Responsible Use

Run this lab only with explicit authorization and accounts, domains, and infrastructure you own. Do not use the templates or deployment notes to impersonate real organizations or target third parties.

## Reporting a Repository Issue

If you find sensitive data in this public repository, contact the repository owner privately rather than opening a public issue containing the exposed value.

