### 404 page not found

[404 page not found](../troubleshoot/t404-page-not-found.md)

---

### Error: #1 [internal] load local bake definitions

```bash
corepack enable pnpm && pnpm install --frozen-lockfile;
.....
Error type: App\Exceptions\DeploymentException
Error code: 0
Location: /var/www/html/app/Traits/ExecuteRemoteCommand.php:242

```
- [Apply troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md) 

### Payload does not render images
- In progress
- Apply [troubleshoot/render-images](../troubleshoot/render-images.md) 

### Deployment Error [ERR_PNPM_IGNORED_BUILDS]

During VM or VPS Payload CMS deployment

```bash
[ERR_PNPM_IGNORED_BUILDS] Ignored build scripts: esbuild@0.28.1, sharp@0.34.2, sharp@0.34.5, unrs-resolver@1.12.2

Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.
```

Run `pnpm approve-builds` command on development machine locally, it will create a pnpm-workspace.yaml file. Add this file name to Dockerfile initial copy command

```Dockerfile
# .....
# Install dependencies based on the preferred package manager
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* pnpm-workspace.yaml ./
# .....
```

Apply steps in [troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md)

### Error during seeding
- An error occured while seeding
  - Check for proper file/directory ownership in payload container
  - Still getting the error, Apply [troubleshoot/seeding](../troubleshoot/seeding.md)

### Container keeps restarting
`Error: Cannot find module '/home/node/app/server.js'`
- Comment out existing volumes in docker-compose.yml 
- Do not use volumes in payload service

### Payload build error
- ERR_PNPM_OUTDATED_LOCKFILE  Cannot install with "frozen-lockfile" because pnpm-lock.yaml is not up to date with < ROOT >/package.json
- Apply [troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md)

### Error: It looks like you're trying to use `tailwindcss` directly as a PostCSS plugin
- Apply [troubleshoot/tailwindcss](../troubleshoot/tailwindcss.md)

### SSL Certificate Error net::ERR_CERT_AUTHORITY_INVALID
- Check DNS settings on your InternetServiceProvider
- Check Firewall settings
- Check [Server Traefik configuration](../coolify/configure-coolify-traefik.md)
- Check [Application Traefik configuration](../coolify/set-traefik-for-new-app.md) for details
- Make necessary changes, restart Proxy, Redeploy application

### pnpm dev ends in infinite loop, FATAL: An unexpected Turbopack error occurred.

Log contains "Next.js package not found"

- Apply [troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md) for details

### Updates were rejected because the tip of your current branch is behind
  - Read Git Documentation https://git-scm.com/docs/git-pull 
