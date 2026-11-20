# Status

Last checked: 2026-11-20 (github-audit pass 02). Docs-only pass: nothing was installed or run.

## What works

- Unknown. The Ruby and C++ scripts are short and look complete by reading; they were not run.

## What's broken or unknown

- No tests anywhere.
- The Makefile builds only `worker`; `queuer` has no target.
- Ruby queuer and worker use tube `my-tube`, the C++ pair uses `test`, so they do not interoperate.
- `c++/queuer` and `c++/worker` are committed macOS binaries; `c++/.worker.cpp.swp` is a committed vim swap file.
- `web-admin` is Rails 4.1.0 and very unlikely to install on current Ruby.
- `web-admin/config/secrets.yml` is committed with literal `secret_key_base` values for development and test (production reads `SECRET_KEY_BASE`). They look like the random keys `rails new` writes **(inference)**, so they only matter if this app was ever run with them. Git history holds them either way; whether to rotate or clean history is Steven's call. The values are not copied here.
- No license is declared.

## What's next

- Steven's call: archive this repo (it is a small spike with no ongoing value) or keep as a reference.
- If kept: remove the binaries and swap file, add a `.gitignore`.

## Not yet from the standard

- CONTRIBUTING.md
- SECURITY.md
- `.editorconfig`
- `.github/` issue and pull request templates
- Dependabot config
- `ci.yml` with a gitleaks secret scan
- Rename the default branch from `master` to `main`
- CloudEvents catalog (only if the repo ever emits events)
- Product ONE-PAGER
- LICENSE: none declared, so none was added. Choosing one is Steven's decision.
