
# SAC Custom Widgets – System Architecture

_Enterprise-Grade Widget Platform for SAP Analytics Cloud_

---

## 1. Purpose & Principles

This document is the definitive technical reference for the SAC Custom Widgets system. It merges best practices from enterprise widget development, security, and performance optimization.

**Key Principles:**
- **Security First:** All widgets follow enterprise security standards
- **Performance Optimized:** Designed for large datasets and fast rendering
- **Accessibility:** Full WCAG 2.1 compliance
- **Enterprise Ready:** Production deployment, monitoring, and support

---

## 2. Project Structure (Graphical)

```mermaid
graph TD
	A[Root: com.vic.sap.sac.widgets]
	A --> B(src/)
	B --> B1[com.vic.sap.sac.widget.{widgetName}/]
	B1 --> B2[{widget-name}.json]
	B1 --> B3[{widget-name}.js]
	B1 --> B4[{widget-name}_styling.js]
	B1 --> B5[{widget-name}_builder.js]
	B1 --> B6[dev/]
	B6 --> B7[preview.html]
	B6 --> B8[test-data.json]
	B6 --> B9[README.md]
	A --> C(scripts/)
	A --> D(documentation/)
	A --> E(archive/)
	A --> F(package.json)
```

---

## 3. Naming & Namespace Conventions

- **Namespace:** `com.vic.sap.sac.widget.{camelCaseName}`
- **Custom Element:** kebab-case (e.g., `<multi-select-dropdown>`) registered via `customElements.define()`
- **Files:**
	- Manifest: `{kebab-case-name}.json`
	- Main: `{kebab-case-name}.js`
	- Styling: `{kebab-case-name}_styling.js`
	- Builder: `{kebab-case-name}_builder.js`

---

## 4. Core Architecture & Lifecycle

- Each widget is a web component extending `VICWidgetBase`.
- Modular shared utilities: data, events, theming, performance.
- **Lifecycle hooks:**
	- `onCustomWidgetInit`
	- `onCustomWidgetBeforeUpdate`
	- `onCustomWidgetAfterUpdate`
	- `onCustomWidgetResize`
	- `onCustomWidgetDestroy`

**Extension Points:**
- Add new widgets via generator (`npm run create-widget`)
- Extend core via `src/shared/`

---

## 5. Design Decisions & Best Practices

- **Shadow DOM:** Used for non-UI5 widgets; UI5 widgets use light DOM for compatibility
- **Strict manifest type enforcement** (see REFERENCE.md)
- **Centralized event bus and data hub** for cross-widget communication
- **Dev-only files** must be in `dev/` subfolders

---

## 6. Security, Performance, and Accessibility

- **Security:**
	- Subresource Integrity (SRI) for all deployed assets
	- Code signing and obfuscation in production
- **Performance:**
	- Virtual scrolling for large datasets
	- Modular, lazy-loaded code
- **Accessibility:**
	- ARIA roles, keyboard navigation, color contrast

---

## 7. Environment & Tooling

- **Node.js 18+**, **npm**, **VS Code** (with recommended extensions)
- **SAC Tenant** with custom widget permissions
- **Scripts:**
	- `npm run dev` – Dev server
	- `npm run build` – Build widgets
	- `npm run deploy:<widget>` – Production deploy

---

## 8. Quick Reference

- See `REFERENCE.md` for manifest types, APIs, and troubleshooting
- See `DEVELOPMENT.md` for widget creation and coding standards
- See `BUILD_DEPLOY.md` for build and deployment
- See `TESTING.md` for test strategy