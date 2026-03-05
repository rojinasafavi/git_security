# Investigation: Risks of Committed Secrets

## Common Vulnerabilities from Committed Secrets

| Vulnerability | How It Happens | Typical Impact |
|---------------|----------------|----------------|
| **Credential Leakage** | API keys, passwords, or certificates are pushed to a repository (public or internal). | Attackers gain direct access to cloud services, databases, or third‑party APIs → data theft, service abuse, financial loss. |
| **Privilege Escalation** | Secrets belonging to privileged accounts (e.g., admin AWS keys, GitHub tokens with `repo` scope) are exposed. | Attackers can take over the entire infrastructure, modify code, delete repos, create resources. |
| **Supply‑Chain Compromise** | Secrets used in CI/CD pipelines (Docker registry credentials, deployment tokens) are leaked. | Malicious binaries or containers are injected into the build pipeline, affecting downstream users. |
| **Data Exfiltration** | Database connection strings or encryption keys are committed. | Sensitive data (PII, financial records) can be downloaded or decrypted. |
| **Denial‑of‑Service (DoS)** | Secrets for rate‑limited services (SendGrid, Twilio) are exposed. | Attackers can exhaust quotas, causing service outages for the legitimate project. |
| **Reputation Damage** | Public disclosure of internal credentials or private keys. | Loss of trust, compliance violations (GDPR, HIPAA, PCI‑DSS). |

### Why These Vulnerabilities Matter
1. **Immediate Exploitation** – Many secrets remain *live* when discovered, giving attackers a short window to act before rotation.
2. **Automation** – Bots continuously scan public repos for patterns like `AKIA…` (AWS keys). A leaked secret can be harvested within minutes.
3. **Compliance Risks** – Storing personal or regulated data without protection can trigger legal penalties.
4. **Cost** – Unauthorized use of cloud resources can generate unexpected bills (e.g., large EC2 instances).

### Mitigation Strategies
- **Never commit secrets** – Use `.gitignore` and secret‑management tools (Vault, AWS Secrets Manager, env vars).
- **Pre‑commit hooks** – Tools like `git‑secrets`, `detect‑secrets`, or `pre‑commit` can block secret patterns.
- **Rotate regularly** – If a secret is ever exposed, revoke it immediately and generate a new one.
- **Audit history** – Run `git log -S <secret>` or use services (GitHub secret scanning, GitGuardian) to find past leaks.
- **Limit scope** – Issue the least‑privileged credentials (e.g., read‑only API keys).

---

## Private vs. Public Repository

| Aspect | Private Repository | Public Repository |
|--------|-------------------|-------------------|
| **Visibility** | Only users granted explicit access can view or clone the repo. | Anyone on the internet can view, clone, and fork the repo. |
| **Typical Use Cases** | Proprietary code, internal tooling, pre‑release work, or any code that must stay confidential. | Open‑source projects, documentation, examples, or anything the author wants to share freely. |
| **Access Control** | Managed via platform permissions (GitHub Teams, GitLab groups, Azure DevOps permissions). | No authentication required for read access; write access is still controlled (maintainers only). |
| **Risk of Secret Exposure** | Still possible if the repo is accidentally made public or if internal members leak it. | Higher risk because the code is indexed by search engines and scanned by automated secret‑detection bots. |
| **Cost (on hosted services)** | Often requires a paid plan or a limited‑free private tier. | Usually free for unlimited public repositories. |

**Bottom line:** A *private* repo limits who can see the code, reducing the chance that a committed secret is discovered by the public. A *public* repo is openly accessible, so any secret accidentally committed will be quickly found by automated scanners and can be exploited almost immediately.
