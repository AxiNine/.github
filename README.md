# .github

Org-wide GitHub defaults for **AxiNine**:

- `profile/README.md` — the public/org landing page shown at github.com/AxiNine
- `pull_request_template.md` — default PR template inherited by repos without their own
- `ISSUE_TEMPLATE/` — default issue templates
- `SECURITY.md` — default vulnerability-reporting policy inherited by repos without their own
  (⚠️ still carries a placeholder contact address)
- `workflow-templates/` — starter workflows offered in the Actions tab of every AxiNine repo

Per-repo `CODEOWNERS`, branch protection, and CI live in each repository.

## Why CI is per-repo and not a reusable workflow

The obvious alternative was one reusable workflow here, called by all nine repos. It was not taken:

- A private reusable workflow needs the org setting that makes this repo accessible to the others. Get it
  wrong and **every** repository's CI fails at once, with an error that points here rather than at the
  cause.
- One file to edit is also one file to break. The blast radius of a bad commit in a shared workflow is the
  whole organisation.
- The gates genuinely differ per stack — `--locked-mode` for .NET, `npm ci --ignore-scripts` for npm,
  digest pinning for compose. What is actually shared is about twenty lines of secret scanning, which is
  cheap to duplicate and easy to read in place.

So the secret-scanning baseline lives in `workflow-templates/` as a *starting point new repos copy*, not
as a runtime dependency they inherit. Duplication here is the deliberate choice.

## Rules that apply to every workflow in the org

1. **Actions are pinned to a commit SHA**, with the version in a trailing comment:
   `uses: actions/checkout@3d3c42e5… # v7.0.1`. A tag can be moved by whoever owns the action.
   Dependabot's `github-actions` ecosystem understands this convention and updates both parts.
2. **No `continue-on-error` on a gate.** A check that reports success while doing nothing is worse than an
   absent one — the dashboard then argues against the person who suspects a problem. Report-only steps
   are allowed, but they are named as reports and never counted as controls.
3. **`permissions: contents: read` at file level**, widened only in the job that needs it (the SBOM jobs
   need `id-token: write` and `attestations: write`; nothing else does).

Full rationale and the current OWASP Top 10:2025 status:
[`AxiNine/docs` §8.5](https://github.com/AxiNine/docs/blob/develop/axinine_platform_architecture_v3.2_master.md).
