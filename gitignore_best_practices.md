# .gitignore Best Practices

## Why Exclude Sensitive Files?
- **Security**: Prevent accidental exposure of API keys, passwords, and other secrets.
- **Portability**: Keep the repository clean and reproducible for other developers.
- **Compliance**: Meet organizational policies and industry regulations regarding credential handling.

## Common Sensitive Files to Exclude
- Environment variable files: `.env`, `.env.*`, `.envrc`
- Secret/key files: `*.key`, `*.pem`, `secrets/`
- Local configuration: `*.local`, `config/*.local`
- IDE/editor settings: `.vscode/`, `.idea/`, `*.sublime-*`
- OS generated files: `.DS_Store`, `Thumbs.db`
- Build artifacts and dependencies: `node_modules/`, `dist/`, `build/`
- Logs and temporary files: `*.log`, `*.tmp`, `*.bak`

## Recommended Workflow
1. **Store secrets in environment variables**
   - Use a `.env` file locally and load it with a library like `dotenv` (Node) or `python-dotenv` (Python).
   - Never commit the `.env` file. Add it to `.gitignore`.
2. **Add a template file**
   - Create a `.env.example` (or similar) that contains the required variable names **without** values. Commit this file to guide contributors.
3. **Use CI/CD secret management**
   - Store production secrets in your CI/CD platform (GitHub Actions Secrets, GitLab CI variables, etc.) and inject them at runtime.
4. **Review before committing**
   - Run `git status` and `git diff --cached` to ensure no secret files are staged.
   - Optionally use tools like `git-secrets` or `detect-secrets` to scan for accidental secrets.
5. **Rotate secrets regularly**
   - If a secret is ever exposed, rotate it immediately and update the environment files.

## Additional Tips
- **Never store passwords in plain text**; use secret managers (e.g., HashiCorp Vault, AWS Secrets Manager).
- **Avoid hard‑coding credentials** in source code; always reference environment variables.
- **Document the required environment variables** in your project README so new developers know what to set.
- **Use `.gitignore` templates** from GitHub's collection (e.g., `github.com/github/gitignore`).

---
*This guide is intended for the `git_security` project but applies broadly to any codebase.*
