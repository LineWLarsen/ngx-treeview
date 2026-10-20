
# [1.0.0](https://www.npmjs.com/package/@charmedme/ngx-treeview) (2022-06-01)

### Enhancement:

- Upgrade to Angular 13
- Upgrade to Bootstrap 4.6
- Upgrade to new package-lock file
----------
# [2.0.0](https://www.npmjs.com/package/@charmedme/ngx-treeview) (2022-08-06)

### Enhancement:

- Upgrade to Bootstrap 5
----------
# [2.0.4](https://www.npmjs.com/package/@charmedme/ngx-treeview) (2022-08-06)

### Fixes:
- Rollup fixes of 2.0.1...2.0.3
- Fix an alignment issue with the a treeview when it has no children
----------
# [2.0.5](https://www.npmjs.com/package/@charmedme/ngx-treeview) (2022-08-09)

### Enhancement:

- Add an option to toggle between a compact view and the default view

### Fixes:

- Fix an alignment issue with the a treeview when it has no children

----------

# [2.0.6](https://www.npmjs.com/package/@charmedme/ngx-treeview) (2022-08-11)

### Fixes:

- Fix the checkbox focus border was cut-off due to an overflow on the treeview-container

----------

# [2.0.7](https://www.npmjs.com/package/@charmedme/ngx-treeview) (2022-08-11)

### Enhancement:

- Update readme

----------

# [3.0.0](https://www.npmjs.com/package/@charmedme/ngx-treeview) (2026-02-13)

### Enhancement:

- Upgrade workspace and library to Angular 21
- Upgrade TypeScript, ng-packagr, and related build tooling to Angular 21-compatible versions
- Upgrade `gh-pages` to `^6.3.0` to address the prototype pollution advisory (GHSA-8mmm-9v2q-x3f9)

### Fixes:

- Fix demo startup (`npm run start`) by using root base href for local development
- Keep GitHub Pages compatibility by setting `--base-href /ngx-treeview/` in `build:demo`
- Refresh tests/configuration for Angular 21 migration compatibility

### Breaking Changes:

- Upgrade Angular peer dependency range to `^21.0.0`
- Re-release as major version to reflect Angular 21 compatibility changes from `2.0.8`

----------
# [22.0.0](https://www.npmjs.com/package/@linewlarsen/ngx-treeview) (2026-10-20)

Fork of [@charmedme/ngx-treeview](https://github.com/charmedme/ngx-treeview), published as `@linewlarsen/ngx-treeview`.

### Enhancement:

- Upgrade workspace and library to Angular 22
- Upgrade TypeScript to 6.0 and ng-packagr to 22
- Build library in partial compilation mode (`compilationMode: "partial"`), making the published package compatible across Angular patch and minor versions
- Migrate demo app to the `@angular/build` application builder (esbuild/Vite)
- Migrate tests to the `@angular/build:karma` builder; remove legacy `test.ts` and `polyfills.ts`
- Set explicit `ChangeDetectionStrategy.Eager` on all components to preserve existing behaviour under Angular 22's new OnPush default

### Fixes:

- Fix `ng-packagr` builder reference in `angular.json` (was the long-removed `@angular-devkit/build-ng-packagr`)
- Fix `publish` script (`--folder` is not a valid npm flag)
- Fix dropdown test relying on implicit change detection after `tick()`
- Remove obsolete View Engine option `enableResourceInlining`, `baseUrl` (deprecated in TypeScript 6) and `tslint.json`
- Remove unused `@angular/animations` dependency

### Breaking Changes:

- Package renamed from `@charmedme/ngx-treeview` to `@linewlarsen/ngx-treeview`
- Angular peer dependency range is now `^22.0.0`
- Requires Node.js 22.22+ or 24.13+ (Angular 22 requirement)

----------
