# Releasing @linewlarsen/ngx-treeview

This repo is a workspace with two parts:

| File | Purpose | Published? |
|---|---|---|
| `package.json` (root) | Workspace: Angular, ng-packagr, TypeScript, demo app, tests | No (`"private": true`) |
| `projects/ngx-treeview/package.json` | The library: name, version, `peerDependencies` | **Yes** |

The library is built in **partial compilation mode** (`tsconfig.lib.prod.json`), so a published
version works across all patch and minor versions of the Angular major it supports.
A new Angular **minor/patch** release does not require a new release of this library.

---

## New Angular major (e.g. 22 → 23)

### 1. Prepare

```bash
git checkout master && git pull
git checkout -b ng23
nvm use <Node version required by the new Angular major>
npm ci
```

Check Node requirements: https://angular.dev/reference/versions

### 2. Upgrade the workspace

```bash
npx ng update @angular/core@23 @angular/cli@23
```

- Let the migrations run. Review the diff, especially component decorators and templates.
- Optional migrations (e.g. Karma → Vitest): decide separately, do not mix with the upgrade.
- If `npm install` warns about blocked install scripts, approve them by name (not pinned):
  ```bash
  npm install-scripts ls
  npm install-scripts approve --no-allow-scripts-pin <pkg>
  ```

Check the official update guide for manual steps:
https://angular.dev/update-guide

### 3. Verify

```bash
npm run build 2>&1 | grep -i "compilation mode"   # must say: partial
npm run build
npx ng test --watch=false
npx ng serve                                       # check demo: checkboxes, filter, dropdown
```

### 4. Update the library manifest

In `projects/ngx-treeview/package.json`, choose one:

| peerDependencies | Version bump | When |
|---|---|---|
| `">=22.0.0 <24.0.0"` | minor (e.g. 4.1.0) | Code still works on the previous major. Consumers can upgrade at their own pace. **Preferred.** |
| `"^23.0.0"` | major (e.g. 5.0.0) | Code changes only work on the new major, or you want to drop the old one. |

Also check `dependencies.tslib` matches what Angular uses.

### 5. Docs

- `CHANGELOG.md`: add a new entry at the bottom
- `README.md`: update the **Compatibility** table
- Copy README to the library (this is what npm shows):
  ```bash
  cp README.md projects/ngx-treeview/README.md
  ```

### 6. Publish

```bash
npm run build
npm publish ./dist/ngx-treeview --access public --dry-run   # check name, version, file list
npm publish ./dist/ngx-treeview --access public
```

The file list should only contain `fesm2022/`, `types/`, `package.json`, `README.md`, `LICENSE`.

### 7. Merge and tag

```bash
git checkout master
git merge ng23
git push
git tag v4.1.0 && git push --tags
```

### 8. Update consuming apps

```bash
npm install @linewlarsen/ngx-treeview@^4.1.0
npm run lint && npm test && npx ng build
```

---

## Bugfix / small change (no Angular upgrade)

```bash
git checkout -b fix/<description>
# make the change
npm run build && npx ng test --watch=false
```

Bump `version` in `projects/ngx-treeview/package.json` (patch: 4.0.1, new feature: 4.1.0),
add a `CHANGELOG.md` entry, then follow steps 6–8 above.

---

## Known issues and gotchas

- **Never run `npm publish` from the repo root.** Always publish `./dist/ngx-treeview`.
  The root is marked `"private": true` as a safeguard.
- **`npm audit` reports `braces` via Karma and `gh-pages`.** No patched version exists.
  These are dev-only and are not part of the published package. Do not run `npm audit fix --force`
  (it suggests installing `karma@4.0.0`).
- **TypeScript strictness**: the library is compiled with `"strict": false` (`tsconfig.base.json`).
  Turning on `strict` changes the public `.d.ts` types and should be a separate, deliberate change.
- **Change detection**: all components explicitly use `ChangeDetectionStrategy.Eager` to keep
  the pre-Angular 22 behaviour. Switching to OnPush is a behavioural change for consumers.
- **Tests that read the DOM after `tick()`** must call `fixture.detectChanges()` again;
  newer Angular versions do not refresh the fixture implicitly.
- **GitHub authentication from WSL**: use `gh auth login` (HTTPS) or an SSH key.
  Passwords are not accepted for Git operations.