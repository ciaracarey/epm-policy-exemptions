# EPM Policy Exemptions (GitOps Workflow)

This repository demonstrates a GitOps workflow for managing
Cloudsmith Enterprise Policy Manager (EPM) exemptions.

Instead of editing policies manually, exemptions are stored in Git,
reviewed via Pull Requests, and automatically applied to Cloudsmith.

---

## Workflow

1. Developer adds an exemption to:

   exemptions/allow.json

2. Opens a Pull Request.

3. DevOps/Security reviews and approves.

4. When merged to `main`, GitHub Actions:

   - regenerates the Rego policy
   - uploads it to Cloudsmith
   - applies the updated exemption list

---

## Exemption Format

Entries use:
- format:name:version

### Example:

python:requests:2.6.4
npm:left-pad:1.3.0

---

## Policy Behaviour

The generated policy allows explicitly approved package versions
to bypass security enforcement policies.

Each exemption produces:
Explicit exemption approved: format:name:version

---

## GitHub Secrets Required

Configure the following repository secrets:

| Secret | Description |
|---|---|
| CLOUDSMITH_WORKSPACE | Workspace slug |
| ALLOW_POLICY_SLUG | Policy slug |
| CLOUDSMITH_TOKEN | API token |

---

## Local Testing
export CLOUDSMITH_WORKSPACE=example
export ALLOW_POLICY_SLUG=abc123
export CLOUDSMITH_TOKEN=token

python exemptions/update_policy.py

---

## Notes

This workflow exists because EPM policies currently embed data
directly in Rego. Managing exemptions via Git provides:

- auditability
- approvals
- rollback
- reproducibility
- scalable exemption management
