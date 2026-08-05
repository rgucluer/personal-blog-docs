# Fix pnpm errors

```bash
$ cd < personal-blog-source >
```

## pnpm 10 to pnpm 11 update

### Edit package.json

- Remove the `packageManager` key from package.json .
- Remove `pnpm` key from end of package.json .
- Modify `engines` key, add 11 to pnpm
```json
{
.....
  "devDependencies": {
    .....
  },
  "engines": {
    "node": "^18.20.2 || >=20.9.0",
    "pnpm": "^9 || ^10 || ^11"
  }  
}

```

### Delete pnpm-lock.yaml file

pnpm-lock.yaml is generated with `pnpm install` command or `corepack use pnpm@latest-11` command referencing package.json content. We will use this command in the next section. If error occurs after installing and removing npm packages apply steps in [frozen-lockfile](../../troubleshoot/frozen-lockfile.md).


```bash
$ rm -rf ./node_modules/ ./.next pnpm-lock.yaml
```

```bash
$ pnpm store prune
```

---

### Install/update pnpm , and npm packages

```bash
$ cd < payload-app-full-path >
```
```bash
$ corepack use pnpm@latest-11
```

```bash
Installing pnpm@11.10.0 in the project...

[WARN] 1 deprecated subdependencies found: tsconfck@3.1.6
.....
[ERR_PNPM_IGNORED_BUILDS] Ignored build scripts: esbuild@0.28.1, sharp@0.34.2, sharp@0.34.5, unrs-resolver@1.12.2

Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.

.....
```

### Run approve-builds

```bash
$ pnpm approve-builds
```

```bash
? Choose which packages to build (Press <space> to select, <a> to toggle all, <i> to invert selection)
 ○ esbuild
❯● sharp
 ○ unrs-resolver

↑↓ navigate • space select • a all • i invert • ⏎ submit
```

Navigate with arrow keys, select with <kbd>Space</kbd> key, accept&continue with <kbd>ENTER</kbd> key

```bash
? The next packages will now be built: sharp.
Do you approve? (y/N)
```

<kbd>y</kbd> , <kbd>ENTER</kbd>

## Check

```bash
$ cd < payload-app-full-path >
```
```bash
$ corepack use pnpm@latest-11
```
```bash
Installing pnpm@11.10.0 in the project...

✓ Lockfile passes supply-chain policies (verified 13m ago)
Lockfile is up to date, resolution step is skipped
Already up to date
Done in 965ms using pnpm v11.10.0
```

### Continue with : Change source code [payload/method-one/change-source-code](create-payload-cms-m1.md#make-some-changes-in-source-code-change-source-code) .
