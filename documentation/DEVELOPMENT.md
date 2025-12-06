# Development Guide

_Comprehensive workflow, standards, and generator usage for SAC Custom Widgets._

---

## 1. Environment Setup

- **Node.js 18+**, **npm**, **VS Code** (with recommended extensions)
- **SAC Tenant** with custom widget permissions

**Setup Steps:**
```bash
git clone <repository-url>
cd com.vic.sap.sac.widgets
npm install
npm run dev
```

**Recommended VS Code Extensions:**
- ESLint, Prettier, Tailwind CSS, JSON, YAML

---

## 2. Widget Creation Workflow

**ALWAYS use the generator:**
```bash
npm run create-widget "My Widget" --styling --builder
# or for data binding widgets:
npm run create-widget "My Chart" --data-binding --styling
```

**Key Flags:**
- `--styling` (styling panel)
- `--builder` (builder panel)
- `--data-binding` (SAC dataBindings, no builder panel)
- `--ui5` (UI5 integration)

**Generator Features:**
- Enforces manifest shape, panel conventions, signing, and UI5 rules
- Produces: manifest JSON, main JS, panel JS, dev/ preview

---

## 3. Coding Standards & Patterns

- **ES6+ syntax**
- **Namespace:** `com.vic.sap.sac.widget.{camelCaseName}`
- **Custom element:** kebab-case (e.g., `<multi-select-dropdown>`)
- **Dev-only files:** must be in `dev/` subfolders
- **Extend `VICWidgetBase`** for all widgets
- **Lifecycle hooks:**
  - `onCustomWidgetInit`, `onCustomWidgetBeforeUpdate`, `onCustomWidgetAfterUpdate`, `onCustomWidgetResize`, `onCustomWidgetDestroy`

**Panel & Data Binding Rules:**
- **Builder panels and SAC dataBindings are mutually exclusive** (SAC limitation)
- **Styling panel:** for visual properties only
- **Builder panel:** for functional/configuration properties
- **Data binding widgets:** all config in styling panel

---

## 4. Property, Method, and Event Types

**Supported Manifest Types:**
- `string`, `number`, `integer`, `boolean`
- Arrays: `string[]`, `number[]`, etc.
- `enum` (with `values` array)
- `object` (with explicit properties)

**Example:**
```json
"chartType": { "type": "enum", "values": ["bar","line","pie"], "default": "bar" },
"seriesColors": { "type": "string[]", "default": ["#FF0000","#00FF00"] },
"axisConfig": { "type": "object", "properties": { "xLabel": { "type": "string" }, "yLabel": { "type": "string" } }, "default": { "xLabel": "Year", "yLabel": "Revenue" } }
```

**Methods & Events:**
- Use same types as properties for parameters/payloads
- Keep all payloads JSON-serializable

---

## 5. Panels: Implementation Patterns

**Builder Panel:**
- For configuration/functional properties
- Implements debounced property updates
- No visual properties

**Styling Panel:**
- For appearance only (theme, font, color)
- Uses expandable sections for organization

**Panel Communication:**
```javascript
this.dispatchEvent(new CustomEvent('propertiesChanged', { detail: { properties: newProperties } }));
```

---

## 6. Best Practices & Checklist

1. Use the generator for all new widgets
2. Never duplicate properties between panels
3. Always set sensible defaults in manifest
4. Validate manifest types before packaging
5. Use dev/ for all non-production files
6. Keep method/event payloads simple and documented
7. Test panel communication and property sync

---

## 7. References

- See `ARCHITECTURE.md` for system overview
- See `REFERENCE.md` for manifest types and troubleshooting
- See `BUILD_DEPLOY.md` for build and deployment