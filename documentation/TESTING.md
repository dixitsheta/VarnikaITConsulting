# Testing Guide

_Unit, E2E, harness, and migration testing for SAC custom widgets._

---

## 1. Unit Testing (Jest)

- Run all unit tests:
  - `npm test`
- Test files: `*.test.js` in widget or shared folders
- Example:
```javascript
describe('MyWidget', () => {
  test('should initialize with default properties', () => {
    expect(widget.caption).toBe('Default Caption');
    expect(widget.theme).toBe('#007AFF');
  });
});
```
- Keep unit tests in sync with widget API and property changes.

---

## 2. Integration & E2E Testing (Cypress)

- Run E2E tests:
  - `npm run test:e2e`
- Covers widget behavior in browser, property sync, and event handling
- Example:
```javascript
describe('SAC Integration', () => {
  test('should handle property changes from SAC', () => {
    const mockChangedProps = { caption: 'New Caption', theme: '#ff0000' };
    widget.onCustomWidgetAfterUpdate(mockChangedProps);
    expect(widget.caption).toBe('New Caption');
    expect(widget.theme).toBe('#ff0000');
  });
});
```
- Update E2E tests when widget lifecycle or event contracts change.

---

## 3. Harness Testing (Playwright)

- Run harness tests:
  - `npm run test:harness`
- Uses Playwright to validate widget runtime in browser
- Harness pages in each widget's `dev/` folder
- Example: Visual regression with Playwright
```javascript
describe('Visual Regression', () => {
  test('should render chart correctly', async () => {
    await page.goto('http://localhost:3000/widget-preview');
    await page.waitForSelector('my-widget');
    const screenshot = await page.screenshot();
    expect(screenshot).toMatchImageSnapshot();
  });
});
```
- Use harness tests for browser compatibility and visual checks.

---

## 4. Migration & Versioning Tests

- When updating widget versions, add migration tests to ensure backward compatibility.
- Example:
```javascript
describe('Widget Migration', () => {
  test('should migrate v1 to v2 properties', () => {
    // Simulate old property
    widget.setProperty('oldPropertyName', 'value');
    widget._migrateV1ToV2();
    expect(widget.getProperty('newPropertyName')).toBe('value');
  });
});
```
- Always test migration logic when incrementing major/minor versions.

---

## 5. Troubleshooting & Best Practices

- If tests fail, check for:
  - Outdated snapshots or test data
  - Widget property or event changes
  - Browser compatibility issues
- Keep tests up to date with widget API and version changes
- See `REFERENCE.md` for common errors and solutions

---

## 6. References

- See `BUILD_DEPLOY.md` for build and deployment
- See `REFERENCE.md` for troubleshooting and API details