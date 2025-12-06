# Build & Deployment Guide

_Complete instructions for building, validating, and deploying SAC custom widgets._

---

## 1. Build Pipeline & Scripts

- **Development build:**
  - `npm run build` – Fast build for local development
  - `npm run build:complete` – Full validation and build (recommended before PR)
- **Production build:**
  - `npm run build:secure` – Production build with obfuscation, minification, and security (MANDATORY for deploy)
- **Validation:**
  - `npm run validate` – Lint, format, and manifest schema checks

**Key Build Steps:**
1. Format and lint code
2. Compile and bundle widget files
3. Obfuscate and minify for production
4. Generate integrity hashes
5. Package for deployment

---

## 2. Deployment Process

- **Deploy single widget:**
  - `npm run deploy:<widgetName>` – Builds, signs, and packages for SAC upload
- **Bulk deploy all widgets:**
  - `npm run deploy:all-widgets`
- **Never edit integrity hashes manually.**
- **Always increment widget version in manifest when changing any widget file.**

**Deployment Steps:**
1. Run production build (`npm run build:secure`)
2. Locate deployment files in `00-final-upload/<widget>/deploy/`
3. Upload `.json` manifest first, then `.zip` package to SAC
4. Validate widget loads and passes integrity checks

---

## 3. Security & Integrity

- **Subresource Integrity (SRI):** All deployed assets are signed with SHA-384 hashes
- **Code signing:** Automated in build pipeline
- **Obfuscation:** Enabled by default for production
- **Versioning:** Use semantic versioning (MAJOR.MINOR.PATCH)

**Critical:**
- If widget files change, increment version and rebuild to avoid SAC integrity errors
- Do not include dev/ files in deployment packages

---

## 4. Troubleshooting & Best Practices

- If SAC reports integrity/hash mismatch, ensure you:
  - Incremented version in manifest
  - Rebuilt and redeployed all files
  - Uploaded matching `.json` and `.zip` from the same build
- Use `npm run validate` before every deploy
- Keep deployment packages clean: only production files, no dev/ or preview.html

---

## 5. References

- See `DEVELOPMENT.md` for widget creation workflow
- See `ARCHITECTURE.md` for system overview
- See `REFERENCE.md` for manifest types and troubleshooting