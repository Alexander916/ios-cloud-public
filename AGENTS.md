# Project instructions

This public repository prepares Codex development with GitHub Actions Linux, Windows and macOS environments.

- This repository is public. Commit only code and materials intended for public access.
- Use standard hosted runners; do not enable paid larger runners or change billing settings.
- Store signing credentials and tokens in GitHub Actions Secrets. Never commit them or print them in logs.
- Keep untrusted pull-request checks read-only and independent of signing secrets.
- `Xcode environment check` checks the iOS SDK and SwiftUI compiler. It does not build or sign an app.
- The check runs for changes to `ci/` and its workflow on main and pull requests, and supports manual runs.
- `Linux and Windows environment check` verifies Linux tools and Windows tools; manual runs can select either platform or both. It does not build an application.
- Windows cannot run Xcode. Verify Xcode-dependent changes through Actions and report actual run results.
- When adding an app project, add actual app build/test commands and update workflow path filters to cover its sources.
