# Publish a Payload CMS on a Virtual Machine

## Open Coolify Web UI of Virtual Machine

- Start Virtual Machine
- Visit `https://coolify.< dev-domain-name >`

## Coolify - Add a new Project

- Coolify Web UI -> Projects -> New Project
  - New Project
    - Name: personal-blog
    - Description: Personal Blog made with Payload CMS
    - Create project

## Create a MongoDB using Coolify [payload/create-mongodb](create-mongodb.md)

## Install nvm, and Node on Development PC [nextjs/install-nvm-node](../nextjs/install-nvm-node.md)

## Create a Payload CMS Application

### Method One: Create app with pnpx [payload/method-one/create-payload-cms-m1](method-one/create-payload-cms-m1.md)
If you are creating a new Payload CMS application, and did not committed to a git repository yet, use this method.

### Method Two: Create app with cloning a git repository [payload/method-two/create-payload-cms-m2](method-two/create-payload-cms-m2.md)
If you already have a git repository, use this method.

## Add a new source ( GitHub App ) to Coolify [coolify/add-new-source](../coolify/add-new-source.md)

## Add Source to the Project [coolify/add-source-to-project](../coolify/add-source-to-project.md)
We added the source (GitHub App) to Coolify Sources section in previous step. Now we will connect that to our Coolify Project . 

## Configure the Payload Project [coolify/configure-payload-dev](../coolify/configure-payload-dev.md)

## Deploy / Redeploy Payload Application
- !ATTENTION! Deploy operation will delete all content. 
  - TODO: Implement Backup/Restore operations before/after deployment.
- Coolify UI on VM (https://coolify.< dev-domain-name >)
  - Projects : < coolify-project-name > -> Applications : < coolify-application-name >
    - Stop service if it is already running. 
    - Reload Compose File
    - Actions -> Deploy (without cache)
  - Wait until the message "Container payload-... Started"
    - Click debug icon for more build information
  - Check the green label above, if it stays green "Running(healthy) for a couple of ten seconds...
  - After deployment, node will build Payload CMS. It takes time (3 - 4 minutes).

## Check the Application
- Check `https://www.< dev-domain-name >`
  - Works, good `:)` .
  - Not working, bad `:(`, try [troubleshooting](#troubleshooting).

- Click `Visit the admin dashboard`
  - `https://www.< dev-domain-name >/admin`
  - Create your first user, or login
  - On Dashboard Click "Seed your database", wait ...
    - If successful we get the `Database seeded! You can now visit your website` message.
      - Or just a "done" prompt right of "Seed your database" link.
    - If process fails "An error occured while seeding", check for file permissions and ownership in payload service container. For more information read troubleshooting below.

- Visit `https://www.< dev-domain-name >` , or click "visit your website" link

- Page renders with images.

- Learn more ... [https://payloadcms.com/docs/getting-started/what-is-payload](https://payloadcms.com/docs/getting-started/what-is-payload)

---

## Continue with:

### Production Environment [production](../../../README.md#production-environment-production)

### Daily Development Operations [daily-dev](../development.md#daily-operations-of-development-environment-daily-dev)

---

## References
- https://payloadcms.com/docs/production/deployment
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/ssr 
- https://github.com/coollabsio/coolify-examples/tree/v4.x/nextjs/spa
- https://coolify.io/self-hosted/
- https://coolify.io/docs/knowledge-base/proxy/traefik/redirects
- https://coolify.io/docs/knowledge-base/docker/compose
- https://nextjs.org/docs/messages/next-image-unconfigured-localpatterns


## TODO: 
- Add backup/restore documentation
- Implement Payload CMS as a Coolify Service

---

## Troubleshooting
Troubleshooting [troubleshoot/ts-publish-payload-app](../troubleshoot/ts-publish-payload-app.md)

---

### References:
- We use the Payload Website template, https://github.com/payloadcms/payload/tree/main/templates/website .
- https://payloadcms.com/docs/getting-started/what-is-payload
- https://payloadcms.com/docs/production/deployment

