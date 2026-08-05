# Daily Dev

After finishing initial development, and production environment setup, we use the development environment daily as follows.

## Add ssh keys to ssh agent 

```bash
ssh-add ~/.ssh/< vm-user-key >
```

```bash
ssh-add ~/.ssh/< vm-coolify-rootkey >
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
10.9.172.90 traefik.devblogserver.my-domain.com coolify.devblogserver.my-domain.com 
10.9.172.90 www.devblogserver.my-domain.com
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

Commit or revert any changes, and make status "nothing to commit, working tree clean".

---

```bash
git branch --show-current
```
If it is not dev, switch to dev branch

```bash
git switch dev
```

If dev branch does not exist, create one:
```bash
git switch -c dev
```

---

### Update Payload CMS version if a new version exists

https://payloadcms.com/docs/getting-started/installation

```bash
$ pnpm update payload --latest
```

```bash
$ pnpm update @payloadcms/* --latest  
```

```bash
"@types/node@*" was updated to 22.19.9, not 26.1.2
```

Select a proper Node version , I choose v22.23.2   (Latest LTS: Jod)

```bash
$ nvm install v22.23.2
```

```bash
$ nvm use v22.23.2
```

```bash
$ nvm alias default v22.23.2
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
  ```Dockerfile
  # .....
  ARG NODE_VERSION=22.23.2-alpine
  # .....
  ```
  - Node.js Stable version
    - https://nodejs.org/en
    - v22.23.2
  - A suitable tag for NODE_VERSION
    - https://hub.docker.com/_/node
    - 22.23.2-alpine


- Check Docker image version in docker-compose.yml, change if necessary.
```yaml
services:
  payload:
    # .....
    image: custom-payload-website:3.87.0-build.21
```

- Check version in package.json, bump up version if necessary.
```javascript
{
  "name": "personal-blog",
  "version": "0.2.13",
.....
}
```

### Local Development Mode
- Make changes to switch to Local Development Mode
  - Stop Payload service in VM
  - Mongo URL (public) must be enabled, and DATABASE_URL must be set in .env file.
    - MongoDB service is running
  - .env file content must be similar to VM Payload CMS Environment Variables
    - But set NEXT_PUBLIC_SERVER_URL=http://localhost:3000
  - next.config.ts  comment out output setting
    ```typescript
    // output: 'standalone',
    ```
- Run server
  ```bash
  cd < payload-app-full-path >
  ```
  ```bash
  pnpm install
  ```
  ```bash
  pnpm dev
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
  pnpm install
  ```
  ```bash
  pnpm build
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
- Coolify UI on VM (`https://coolify.devblogserver.my-domain.com`)
  - Project -> < project-name > -> payload 
    - Stop -> Check "Run Docker Clean ..." -> Continue -> Confirm
      - Wait, see the red "Exited" label
    - Click "Reload Compose file"
    - Save
    - Click Advanced -> Force Deploy
    - Wait 3-4 minutes after deployment finishes.

    - Open https://www.< dev-domain-name >/admin
      - Login
      - Click Seed your database
        - We do this after a deployment
        - TODO: Implement a backup/restore process to persist existing content between deployments.
        - Open https://www.< dev-domain-name >
        - Home page renders with images and default theme

### Update documentation
- Update documentation about changes.

Continue to [daily-prod](daily-prod.md) if everything is OK.

### TODO: Add tests

---

## Return back to README : [Readme](../../README.md)
