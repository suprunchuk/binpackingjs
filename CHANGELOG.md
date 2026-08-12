# Changelog

## 4.1.0 (2026-05-14)

### New Features

- **Dynamic float-safe factoring** — `pack2D` now uses the same integer factoring as `pack3D`, shared via a common helper. The factor is computed from input precision (up to 10 decimal places) instead of a hardcoded `100000`
- **`factor` option** — pass a custom scale to `pack2D` / `pack3D`, or `1` to disable factoring

### Bug Fixes

- Fixed 2D float precision rejecting valid placements (e.g. `0.1 + 0.2 > 0.3`)

---

## 4.0.2 (2026-05-14)

### Bug Fixes

- Fixed case-sensitive import paths on Linux: `src/2D` / `src/3D` renamed to `src/2d` / `src/3d` to match package exports

---

## 4.0.1 (2026-05-14)

### Bug Fixes

- Fixed `Score.subtract` adding instead of subtracting
- Fixed `maxWeight: 0` treated as unlimited (now correctly rejects all items)
- Fixed infinite recursion in 3D `packToBin` / `getBiggerBinThan` via a visited set
- Fixed 3D packed output mixing factored and original units
- Fixed 3D volume overflow by computing volume from original units
- Fixed `pruneFreeList` skipping the last free rectangle

### Other

- Added audit-issue tests, benchmarks, and load/stress tests

---

## 4.0.0 (2026-05-13)

### Breaking Changes

- **TypeScript rewrite** — entire codebase rewritten in TypeScript with strict mode
- **Immutable design** — input objects (boxes, items, bins) are never mutated; packing returns new result objects
- **New functional API** — `pack2D()` and `pack3D()` replace the mutation-based class API
- **ESM-first** — published as ES modules with CommonJS fallback; sub-path exports (`binpackingjs/2d`, `binpackingjs/3d`) enable tree-shaking
- **Bun toolchain** — webpack, Babel, and mocha replaced with Bun for build, test, and install
- **Zero runtime dependencies** — all dev dependencies removed from production bundle

### New Features

- `pack2D(options)` — functional API returning `{ packedBins, unpackedBoxes }`
- `pack3D(options)` — functional API returning `{ packedBins, unfitItems }`
- Full TypeScript type definitions shipped with the package
- `PackedBox2D` includes `rotated` flag and `sourceBox` reference back to input
- `PackedItem3D` includes `position`, `dimension`, `rotationType`, and `sourceItem` reference
- `PackedBin2D` includes `efficiency` percentage
- Sub-path exports for smaller bundles: `import { pack2D } from 'binpackingjs/2d'`

### Bug Fixes (carried from v3.1.0)

- Fixed 2D `pruneFreeList()` skipping free rectangles due to misplaced loop increment (#42)
- Fixed 3D `scoreRotation()` choosing suboptimal rotations that waste depth space (#37)
- Fixed 3D `getBiggerBinThan()` using `this.bins` instead of `this.bins.length` in loop condition

### Migration

See the [migration guide in the README](README.md#migrating-from-v3).

---

## 3.1.0 (2026-05-13)

### Bug Fixes

- Fix 2D `pruneFreeList` bug: `i++` moved to outer loop so free rectangles are not skipped during pruning (#42, credit to @traaan PR #27)
- Fix 3D `scoreRotation` heuristic: use tiling efficiency instead of squared dimension ratios (#37)
- Fix broken 2D paper link in README (#29)

### Other

- Add bug reproduction tests for #42 and #37
- Upgrade all dependencies, fix security vulnerabilities
- Upgrade mocha 8 to 11

---

## 3.0.2

- Support constraining box rotation in 2D packing
- Update dependencies

## 3.0.1

- Fix scoring rotation order in 3D packing
- Floating point precision handling via `factoredInteger()`

## 3.0.0

- Initial public release
- 2D bin packing with 4 heuristic strategies
- 3D bin packing with 6 rotation types and weight constraints
- UMD build for browser and Node.js
