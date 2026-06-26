# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies for development
pip install pyserial pyserial-asyncio-fast

# Run all tests
python -m unittest

# Run mypy type checking
mypy -p pymonoprice
```

## Release flow

1. Ensure all changes are merged to the default branch.
2. Push a git tag matching the new version: `git tag v0.6 && git push --tags`
3. Create a GitHub Release from that tag (UI or `gh release create v0.6 --generate-notes`).
4. The `release.yml` GitHub Actions workflow triggers automatically, builds the package, and publishes to PyPI.
5. Version is derived from the git tag by `setuptools-scm` — no source file needs editing.

## PyPI Trusted Publisher

The workflow publishes via OIDC (no stored credentials). If setting up on a new PyPI account or project, configure the Trusted Publisher at:
https://pypi.org/manage/project/pymonoprice/settings/publishing/
- Owner: OnFreund, Repo: pymonoprice, Workflow: release.yml, Environment: pypi

## PR Test Builds

Every PR automatically gets a pre-release wheel published to GitHub Releases (tagged `pr-{number}`). The bot posts an install command in the PR comment. The pre-release is deleted when the PR closes.
