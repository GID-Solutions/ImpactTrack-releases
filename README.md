# ImpactTrack-releases
Deploying a new build

Phase 1 — Bump the version
  npm version prerelease --preid=beta --no-git-tag-version
  # verify:
  node -p "require('./package.json').version"

  Phase 2 — Commit and tag
  VERSION=$(node -p "require('./package.json').version")
  git add package.json package-lock.json
  git commit -m "version bump"
  git tag "v$VERSION"
  git push origin main --tags

  Phase 3 — Build + notarize
  npm run build:mac           # arm64 only (faster, for testing)
  npm run build:mac:universal # arm64 + x64 — use this for public releases
  Requires APPLE_API_KEY, APPLE_API_KEY_ID, and APPLE_API_ISSUER in your shell. Notarization takes 1–5 min per DMG.

  Phase 4 — Publish to GitHub Releases
  npm run release:mac
  This creates the release on GID-Solutions/ImpactTrack-releases, uploads all DMGs/ZIPs/blockmaps and latest-mac.yml.

  Quick checklist before starting:
  - All changes committed and on main
  - Notarization env vars set (source ~/.zshrc if needed)
  - gh CLI authenticated (gh auth status)
