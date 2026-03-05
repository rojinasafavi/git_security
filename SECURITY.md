# Sensitive Information – Never Commit

The following types of data **must never be pushed** to any public repository or shared with others:

- **API keys** (e.g., Google, AWS, Stripe, SendGrid)
- **Private keys** (SSH, RSA, ECDSA, PGP)
- **Database credentials** (usernames, passwords, connection strings)
- **Environment variable files** (e.g., `.env`, `.env.local`, `config/*.env`)
- **OAuth / access tokens** (GitHub, Google, Facebook, etc.)
- **JWT secret keys** and other signing secrets
- **Certificates and keystores** (PKCS12, `.p12`, `.jks`)
- **Password files** or any plaintext password storage
- **Sensitive configuration files** containing secrets (e.g., `settings.py` with passwords, `config.yml` with secrets)

## Recommended Practices

1. **Add to `.gitignore`** – Ensure all files containing secrets are listed.
2. **Use secret management** – Store credentials in environment variables or secret vaults.
3. **Rotate keys regularly** – If a secret is accidentally committed, revoke and replace it.
4. **Run pre‑commit hooks** – Tools like `git-secrets` can prevent accidental commits.
5. **Review commits** – Scan diffs for secret patterns before pushing.

> **Never** commit any of the above directly into source control.
