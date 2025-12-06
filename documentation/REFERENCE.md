# Reference Guide

_Manifest types, core APIs, compliance rules, and troubleshooting for SAC custom widgets._

---

## 1. Manifest Property, Method, and Event Types

- **Allowed types:** `string`, `number`, `integer`, `boolean`, arrays (e.g., `string[]`), `enum` (with `values`), and `object` (with explicit properties)
- **Methods:** Only use simple getter/setter bodies in manifest JSON (e.g., `return this.caption;`)
- **Events:** Use same types as properties for event payloads; keep all payloads JSON-serializable
- **Unsupported types:** SAC runtime objects (e.g., `ResultSet`, `DataSource`) must be represented as arrays/objects or serialized strings

**Example manifest snippet:**
```json
{
  "properties": {
    "caption": { "type": "string", "default": "Widget Caption" },
    "theme": { "type": "string", "default": "#007AFF" },
    "width": { "type": "integer", "default": 300, "min": 50, "max": 2000 },
    "enabled": { "type": "boolean", "default": true }
  },
  "methods": {
    "getCaption": { "returnType": "string", "parameters": [] },
    "setCaption": { "parameters": [{ "name": "caption", "type": "string" }] }
  },
  "events": {
    "onDataPointClick": {},
    "onSelectionChange": {}
  }
}
```

---

## 2. Manifest Compliance Rules

- **NEVER include:** `author`, `homepage`, `enumValues`, or `parameters` in events (causes upload errors)
- **ALWAYS include:** `id`, `name`, `vendor`, `version`, `newInstancePrefix`, `webcomponents`
- **Tag mismatch:** Manifest `webcomponents[0].tag` must match the JS registration string
- **Integrity hash:** Never edit manually; always use build scripts

---

## 3. Core APIs (Shared VIC Classes)

- `VICWidgetBase`: Widget base class (extend for all widgets)
- `VICEventBus`: Cross-widget event bus (pub/sub)
- `VICDataHub`: Data sharing, filter, and selection management
- `VICThemeProvider`: Theming and color management

---

## 4. Data Flow & System Diagrams

- See `ARCHITECTURE.md` for project structure and data flow diagrams
- Widget-specific API and event details: see each widget's `README.md`

---

## 5. Troubleshooting

- **Tag mismatch:** Manifest tag must match JS registration
- **Integrity hash:** Never edit manually; always use build scripts
- **Upload errors:** Remove forbidden fields, check required fields
- **API changes:** Update tests and documentation when widget API changes

---

## 6. References

- See `DEVELOPMENT.md` for coding standards and generator usage
- See `BUILD_DEPLOY.md` for build and deployment
- See `TESTING.md` for test strategy