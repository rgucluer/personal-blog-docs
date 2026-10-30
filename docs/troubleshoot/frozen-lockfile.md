Run Virtual Machine, database service must be runnning. Stop payload service in VM, if it is running .

On Dev PC
```bash
$ cd < payload-app-full-path >
```

Check the version of the Docker image, update if necessary
```Dockerfile
# .....
ARG NODE_VERSION=24.22.1-alpine
# .....
```

- Check Payload CMS https://github.com/payloadcms/payload
  - Check tags/releases
```text
v3.92.1
```

- Check version in package.json, bump up version if necessary.
  - https://github.com/rgucluer/personal-blog/blob/main/package.json
  - Update Payload [payload/update-payload](../payload/update-payload.md)
- Check Docker image version in docker-compose.yml, change if necessary.
```yaml
services:
  payload:
    # .....
    image: custom-payload-website:3.92.1-build.1
```

- Set output to standalone in next.config.ts .
```javascript
.....
  output: 'standalone',
.....
```


```bash
$ rm -rf ./node_modules/ ./.next pnpm-lock.yaml
```

```bash
$ pnpm store prune
```

```bash
$ corepack enable pnpm
```

```bash
$ corepack prepare pnpm@latest-11 --activate
```

```bash
$ pnpm install
```

If got an Error:
  ```bash
  ERR_PNPM_IGNORED_BUILDS] Ignored build scripts: esbuild@0.28.1, sharp@0.34.2, sharp@0.34.5, unrs-resolver@1.12.2

  Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.
  ```

  ```bash
  $ cd < payload-app-full-path >
  ```

  ```bash
  $ pnpm approve-builds
  ```
  ```bash
  ? Choose which packages to build (Press <space> to select, <a> to toggle all, <i> to invert selection)
  ❯● esbuild
  ● sharp
  ● unrs-resolver
  ```
  Select all with <kbd>a</kbd>, <kbd>Enter</kbd> to continue

  ```bash
  ? The next packages will now be built: esbuild, sharp, unrs-resolver.
  Do you approve? (y/N)
  ```

  <kbd>y</kbd>, <kbd>Enter</kbd> to continue

```bash
$ pnpm dev
```

```bash
▲ Next.js 16.2.6 (Turbopack)
- Local:         http://localhost:3000
- Network:       http://< dev-pc-local-ip >:3000
- Environments: .env
✓ Ready in 279ms
- Experiments (use with caution):
  ⨯ turbopackServerFastRefresh
  ✓ webpackMemoryOptimizations
```

Check `http://localhost:3000`

Fix any error if occurs. If page renders with/without pictures, it is OK. Stop the service CTRL + C 

```bash
$ pnpm build
```

```bash
$ git add .
```

```bash
$ git commit
```

Depending on your current branch
```bash
$ git push origin dev
```
or
```bash
$ git push origin main
```

- Check all environment variables on Virtual Machine 

- Coolify UI -> Projects -> < coolify-project-name > -> < coolify-application-name >
  - Stop application if it is running
  - Reload Compose File

- Coolify UI -> Projects -> < coolify-project-name > -> < coolify-application-name > -> Redeploy
or
- Coolify UI -> Projects -> < coolify-project-name > -> < coolify-application-name > -> Advanced Force Deploy (without cache)

- Wait 3-4 minutes after deployment finishes.

- Open https://www.< dev-domain-name >/admin
  - Login
  - Click Seed your database
    - We do this after a deployment
    - Open https://www.< dev-domain-name >
    - Home page renders with images and default theme

- TODO: Implement a backup/restore process to persist existing content between deployments.

### Troubleshoot

- Error during pnpm build
  - Error: Failed to collect page data for /posts/page/[pageNumber]
    - Start Virtual Machine
    - Be sure database is running
