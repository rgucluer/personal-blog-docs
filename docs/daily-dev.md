# Daily Operations of Development Environment

After finishing initial development, and production environment setup, we use the development environment daily as follows.

## Add ssh keys to ssh agent 

```bash
ssh-add ~/.ssh/< vm-user-key >
```

```bash
ssh-add ~/.ssh/< vm-root-key >
```

```bash
ssh-add ~/.ssh/< github-user-key > 
```

## Start Virtual Machine
```bash
multipass start vmblog
```
```bash
multipass list
```

Note the IPv4 for later use .

## Check & Note the IP address
```bash
Name                    State             IPv4             Image
vmblog                  Running           10.9.172.90     Ubuntu 24.04 LTS
                                          10.0.1.1
                                          10.0.0.1
                                          10.0.2.1
```
If the IP address is different then the previous run, make necessary changes.

Changes for IP
- /etc/hosts on Developer PC
```bash
.....
10.9.172.90 www.devblogserver.my-domain.com
10.9.172.90 traefik.devblogserver.my-domain.com coolify.devblogserver.my-domain.com 
10.9.172.90 devblogserver.my-domain.com 
10.9.172.90 devblogserver
```

## ssh to Virtual Machine
Me must have the ability to ssh to the Virtual Machine with root user. This is necassary for Coolify.

```bash
ssh root@< virtual-m-ip >
```
or
```bash
ssh root@devblogserver.my-domain.com
```
## Update apt packages
```bash
apt update
```

If there are packages to upgrade:
```bash
apt upgrade
```

Reboot if necessary.

Disconnect

## Open Coolify Web Interface
Open Browser: http://`< virtual-m-ip >`:8000/

If you finished domain & TLS setting properly you can use:

`https://coolify.devblogserver.my-domain.com`

Check `https://traefik.devblogserver.my-domain.com/ping`

Check `https://traefik.devblogserver.my-domain.com/dashboard/`

Check `https://www.devblogserver.my-domain.com`

Check `https://www.devblogserver.my-domain.com/admin`

## Development steps

```bash
$ cd < payload-app-full-path >
```
```bash
$ git config --list
```
```bash
$ git status
```

---

Commit or revert, and push any changes on current branch, and make status "nothing to commit, working tree clean".

---

```bash
$ git branch --show-current
```
If it is not dev, switch to dev branch

```bash
$ git switch dev
```

If dev branch does not exist, create one:
```bash
$ git switch -c dev
```

---

### Update Payload CMS version if a new version exists

- Stop Payload service in VM

```bash
$ cd < payload-app-full-path >
```

```bash
$ corepack enable
```

```bash
$ corepack enable pnpm && corepack use pnpm@latest-11
```

```bash
$ pnpm update payload --latest
```

```bash
$ pnpm update @payloadcms/* --latest  
```

---

### Check version of the following
- Node docker image
  - Check the version of the Docker image in Dockerfile, update if necessary
  - < payload-app-full-path >/Dockerfile
  ```Dockerfile
  # .....
  ARG NODE_VERSION=24.22.1-alpine
  # .....
  ```
- https://nodejs.org/en
  - v24.22.1 Latest LTS

- Check Docker image version in docker-compose.yml, change if necessary.
```yaml
services:
  payload:
    # .....
    image: custom-payload-website:3.92.0-build.1
```

- Check version in package.json, bump up version if necessary.
```javascript
{
  "name": "personal-blog",
  "version": "0.2.21",
.....
}
```

### Local Development Mode
- Make changes to switch to Local Development Mode
  - Stop Payload service in VM
  - Mongo URL (public) must be enabled, and DATABASE_URL must be set in .env file.
    - MongoDB service is running in VM
  - .env file content must be similar to VM Payload CMS Environment Variables
    - But set NEXT_PUBLIC_SERVER_URL=http://localhost:3000
  - next.config.ts  comment out output setting
    ```typescript
    // output: 'standalone',
    ```
- Run server
```bash
$ cd < payload-app-full-path >
```

```bash
$ pnpm install
```

```bash
$ pnpm dev
```

- Open a web browser, navigate to http://localhost:3000
- This renders the homepage of Payload with/without images.
- Open http://localhost:3000/admin
  - Login
  - Click "Seed your database"
  - Wait seeding to finish.
  - Open http://localhost:3000
  - This renders the homepage of Payload with images.
- Make changes, check results.
- <kbd>CTRL</kbd> + <kbd>C</kbd> on terminal to stop the service

### Local Production Mode
- Make changes to switch to Local Production Mode
  - next.config.ts  uncomment output setting
  ```typescript
  output: 'standalone',
  ```

```bash
$ pnpm install
```

```bash
$ pnpm build
```

- Copy < payload-app-full-path >/public into < payload-app-full-path >/.next/standalone
- Copy < payload-app-full-path >/.next/static into < payload-app-full-path >/.next/standalone/.next
```bash
node .next/standalone/server.js
```

- Open a web browser, navigate to http://localhost:3000
- This renders the homepage of Payload with/without images.
- Open http://localhost:3000/admin
  - Login
  - Click "Seed your database"
  - Wait seeding to finish.
  - Open http://localhost:3000
  - This renders the homepage of Payload with images.
- <kbd>CTRL</kbd> + <kbd>C</kbd> on terminal to stop the service

#### Commit changes
```bash
git add .
```
```bash
git commit
```
Enter your message, save & exit

#### git push
```bash
git push origin dev
```

#### Redeploy on VM
- !ATTENTION! Deploy operation will delete all content. 
- TODO: Implement Backup/Restore operations before/after deployment.
- Coolify UI on VM (`https://coolify.devblogserver.my-domain.com`)
  - Project -> < coolify-project-name > -> personal-blog
    - Actions -> Stop -> Check "Run Docker Clean ..." -> Continue -> Confirm
      - Wait for `Exited` label
    - Click "Reload compose"
    - Actions -> Deploy (without cache)
      - Check the green label above stays green "Running(healthy) for a couple of ten seconds...
    - After deployment, node will build Payload CMS. It takes time (3 - 4 minutes).
    - Open https://www.< dev-domain-name >/admin
      - Login
      - Click Seed your database
        - We do this after a deployment
        - Open https://www.< dev-domain-name >
        - Home page renders with images and default theme

### Update documentation
- Update documentation about changes.

---

### Back to [README](../README.md)

---

References:
- https://payloadcms.com/
- https://payloadcms.com/docs/getting-started/installation

---

### TODO: Add tests
