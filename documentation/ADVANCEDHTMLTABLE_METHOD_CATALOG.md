## advanced-html-table.js — Method catalog

This document catalogs methods in `src/com.vic.sap.sac.widget.advancedHTMLTable/advanced-html-table.js` derived from the lifecycle call-stack analysis you provided.

Legend
- Category: lifecycle | setter | getter | render | data | hierarchy | helper | cleanup | event
- Used: yes/no (reachable from SAC lifecycle hooks)
- Origin: new (GPT parser / recent additions) | legacy (pre-existing) | shared (delegates to shared module)

### Summary Table

| Method Name | Category | Used | Origin | Reachable From | Short description / notes | Newest replacement |
|-------------|----------|------|--------|-----------------|-------------------------|-------------------|
| onCustomWidgetInit | lifecycle | yes (stub) | legacy | connectedCallback (real init) | SAC hook. Currently a no-op stub; actual init in connectedCallback per repo pattern. |
| onCustomWidgetBeforeUpdate | lifecycle | yes | legacy | SAC lifecycle | Copies changedProperties into this._properties; simple normalization. | _updateWidgetProperties |
| onCustomWidgetAfterUpdate | lifecycle | yes | legacy | SAC lifecycle | Main orchestrator: property handling, data processing, rendering scheduling. Very large. |
| onCustomWidgetResize | lifecycle | yes | legacy | SAC lifecycle | Applies container width/height. Contains a console.warn (to be removed). | (no change) |
| onCustomWidgetDestroy | lifecycle | yes | legacy | SAC lifecycle | Calls _cleanup to remove listeners and timeouts. |
| _updateWidgetProperties | helper | yes | legacy | onCustomWidgetAfterUpdate / onCustomWidgetBeforeUpdate | Centralizes property normalization and shallow copy. Candidate to be canonical property update entry. | (canonical) |
| _applyTheme | render | yes | legacy | _updateWidgetProperties / _reapplyVisualsFromProperties | Updates CSS classes and theme-specific visuals. Calls _updateFooterPaginationVisuals. |
| _updateFooterPaginationVisuals | render | yes | legacy | _applyTheme / _reapplyVisualsFromProperties | Applies CSS vars and pagination visuals. | (no change) |
| _applyVisualChanges | render | yes | legacy | _applyTheme / _reapplyVisualsFromProperties / _renderTable | Applies fontSize, rowHeight, borders, hover behavior, row numbers, etc. |
| _reapplyVisualsFromProperties | render | yes | legacy | setTimeout in onCustomWidgetAfterUpdate | Recomputes visual props from this._properties and reapplies via _applyVisualChanges. | (no change) |
| _scheduleRender | helper | yes | legacy | onCustomWidgetAfterUpdate | requestAnimationFrame wrapper that ultimately calls _renderTable. |
| _renderTable | render | yes | legacy | _scheduleRender / data processing pipeline | Orchestrates _renderHeader, _renderBody, _renderPagination; reattaches event listeners and applies visuals. | (no change) |
| _renderHeader | render | yes | legacy | _renderTable | Renders table header. |
| _renderBody | render | yes | legacy | _renderTable | Renders body rows; reattachment of event listeners for rows. | (no change) |
| _renderPagination | render | yes | legacy | _renderTable | Renders pagination controls. |
| _renderPagination | render | yes | legacy | _renderTable | Renders pagination controls. | (no change) |
| _reattachEventListeners | event | yes | legacy | _renderTable | Attaches delegated handlers for clicks, selection, keyboard; stores handlers to remove in _cleanup. | _cleanup (ensures removal) |
| _handleTableBodyClick | event | yes | legacy | _reattachEventListeners | Handles click targeting rows/cells. |
| _handleTableBodyClick | event | yes | legacy | _reattachEventListeners | Handles click targeting rows/cells. | (no change) |
| _handleRangeSelectionStart / Move / End | event | yes | legacy | _reattachEventListeners | Range selection handlers. | (no change) |
| _cleanup | cleanup | yes | legacy | onCustomWidgetDestroy | Removes popups, clears timers, removes event listeners, clears workers. Ensure covers document/window handlers. |
| _cleanup | cleanup | yes | legacy | onCustomWidgetDestroy | Removes popups, clears timers, removes event listeners, clears workers. Ensure covers document/window handlers. | (no change) |
| _processSACMetadata | data | yes | legacy | onCustomWidgetAfterUpdate -> data path | Parses and builds sac metadata maps (dimensions/measures). | (no change) |
| _processSACDataFromBinding | data | yes | legacy/new | onCustomWidgetAfterUpdate | Top-level data processing router. Accepts forceLegacy flag. Routes to GPT parser OR legacy pipeline. |
| _processSACDataFromBinding | data | yes | legacy/new | onCustomWidgetAfterUpdate | Top-level data processing router. Accepts forceLegacy flag. Routes to GPT parser OR legacy pipeline. | _processSACDataWithGPTParser (new) |
| _processSACDataWithGPTParser | data/hierarchy | yes | new | _processSACDataFromBinding when feature flag enabled | New optimized pipeline: buildSACHierarchyModel, flatten, aggregate, set this._data/_columns/_hierarchyModel, then _renderTable. | (new) |
| buildSACHierarchyModel | hierarchy | yes | new | _processSACDataWithGPTParser | Typed-array based hierarchy model builder (Int32Array) — performance-focused. Candidate to move to module. |
| buildSACHierarchyModel | hierarchy | yes | new | _processSACDataWithGPTParser | Typed-array based hierarchy model builder (Int32Array) — performance-focused. Candidate to move to module. | (new) |
| intersectSortedArrays | helper | yes | new | buildSACHierarchyModel | Two-pointer set intersection helper used by buildSACHierarchyModel. | (new) |
| _transformSACBindingDataToTable | data | yes (legacy path) | legacy | _processSACDataFromBinding fallback | Normalizes SAC binding data shape -> table rows. |
| _transformSACBindingDataToTable | data | yes (legacy path) | legacy | _processSACDataFromBinding fallback | Normalizes SAC binding data shape -> table rows. | DataTransformer (proposed) / _processSACDataWithGPTParser |
| _transformArrayDataToTable | data | yes (legacy) | legacy | _transformSACBindingDataToTable | Array-shape transformer. | DataTransformer (proposed) |
| _transformObjectToTableRow / _transformSACDataToTable | data | yes | legacy | various | Object/row normalization helpers. |
| _transformObjectToTableRow / _transformSACDataToTable | data | yes | legacy | various | Object/row normalization helpers. | DataTransformer (proposed) |
| _generateColumnsFromSACBindingData / _generateColumnsFromArrayData / _generateColumnsFromObjectData / _generateColumnsFromSACData | helper | yes | legacy | data transformation | Column generation for different input shapes; duplicated logic — good candidate to centralize into DataTransformer. | DataTransformer (proposed) |
| _buildHierarchyFromParentId | hierarchy | yes (legacy) | legacy | _transform... -> _generateColumns -> _buildHierarchyFromParentId | Legacy parent-id based hierarchy builder. Complex; consider moving to shared module or deprecating after GPT parser stabilization. |
| _buildHierarchyFromParentId | hierarchy | yes (legacy) | legacy | _transform... -> _generateColumns -> _buildHierarchyFromParentId | Legacy parent-id based hierarchy builder. Complex; consider moving to shared module or deprecating after GPT parser stabilization. | buildSACHierarchyModel (new) |
| _hierarchyBuilder.buildHierarchyFromParentId | hierarchy | yes (shared) | shared | _buildHierarchyFromParentId when available | Delegates to shared module when present. Preferred path for maintainability. | (shared) |
| _detectParentIdFields / _determineHierarchyMode / _parseMetadataForHierarchy | helper | yes | legacy | _buildHierarchyFromParentId | Helpers used by legacy hierarchy logic. |
| _detectParentIdFields / _determineHierarchyMode / _parseMetadataForHierarchy | helper | yes | legacy | _buildHierarchyFromParentId | Helpers used by legacy hierarchy logic. | (no change) |
| _simpleApplyFilters | helper | yes | legacy | after data processed -> called before _renderTable | Applies client-side filters to normalized data. | (no change) |
| _processChangedData | data | yes | legacy | onCustomWidgetAfterUpdate when changedData given | Routes changedData shapes to _processSACData. |
| _processChangedData | data | yes | legacy | onCustomWidgetAfterUpdate when changedData given | Routes changedData shapes to _processSACData. | (no change) |
| _processSACData / _transformSACDataToTable / _extractCellValue | data | yes | legacy | internal conversions called from different places | Helpers used broadly in legacy path; candidate to move into DataTransformer. | DataTransformer (proposed) |
| _applyDynamicColumnWidths | render | yes | legacy | _renderTable (via setTimeout) | Computes and applies column widths; called async for measurement. |
| _applyDynamicColumnWidths | render | yes | legacy | _renderTable (via setTimeout) | Computes and applies column widths; called async for measurement. | (no change) |
| _updateNoDataDisplay | render | yes | legacy | _renderTable | Shows/hides no-data UI. | (no change) |
| _logHierarchyDebug / _logEmptyCellDebug | helper | yes | legacy | debugging helpers called optionally | Debug helpers — can be gated or removed. |
| _logHierarchyDebug / _logEmptyCellDebug | helper | yes | legacy | debugging helpers called optionally | Debug helpers — can be gated or removed. | _debugLog (proposed) |
| _analyzeDataStructure / _analyzeEmptyCells | helper | yes | legacy | used when analyzing samples (debugging/telemetry) | Analysis helpers that examine sample rows and produce diagnostics. Can be limited to dev mode. | (no change) |
| _filterRowsByKeyFigures | helper | yes | legacy | used during transform/aggregation | Filtering helper for key figure-based filters. | (no change) |
| _filterRowsByKeyFigures | helper | yes | legacy | used during transform/aggregation | Filtering helper for key figure-based filters. |

### Observations & recommendations (delta)
- The core runtime is split in two: a new GPT-optimized path (new functions: _processSACDataWithGPTParser, buildSACHierarchyModel, intersectSortedArrays) and a large legacy pipeline (many _transform* and hierarchy helpers). The legacy pipeline is reachable and used as a fallback.
- Quick wins (low-risk):
  - Add a single `_debugLog()` helper and replace console.* calls. You already removed many logs but a final sweep should remove remaining console.warn/log/info (I will proceed with that in the next todo item).
  - Consolidate column-generation helpers into a small `DataTransformer` module (medium risk) — will remove duplication and make tests easier.
  - Move GPT parser (buildSACHierarchyModel) into a small module and inject it (medium risk). Keep a feature flag for fallback.

### File pointers (approximate locations)
- Lifecycle hooks & main orchestrator: around lines ~1848–2039 in `advanced-html-table.js` (onCustomWidgetAfterUpdate spans a large region). 
- Resize/destroy: around lines ~5676–5694.
- Data & metadata parsing: ~2116+ for _processSACMetadata and near the large _processSACDataFromBinding function.
- GPT parser related functions: near the top/middle of the data processing area (where buildSACHierarchyModel and _processSACDataWithGPTParser were added).

### Next steps (what I'll do now)
1. Persist this catalog file (done).
2. Update the todo list: mark this catalog task complete and set the next task (write method catalog file) in-progress.
3. Then implement the first quick-win: add `_debugLog()` and replace remaining console.warn/log/info calls.

If you want the table in another format (CSV, JSON), or a finer-grained call graph (per-line references with exact line numbers), tell me and I'll extract them next.
