# Personal Blog Production Environment

Please follow this documentation after reading and applying [Development Environment](install-personal-blog-dev.md) documentation. 

## Install Personal Blog Production Environment

We will use the git repository; https://github.com/rgucluer/personal-blog main branch. We also use the cpm-cms documentation( https://github.com/rgucluer/personal-blog-docs , docs/cpm-cms ) as a reference, make changes only if it is necessary. We will install on a virtual private server.
- Following [docs/cpm-cms/production.md](../cpm-cms/production.md)
- Apply [Initial Setup of a Virtual Private Server - cpm-cms/production/initial_setup](../cpm-cms/production/initial_setup.md)
- Apply [Install nvm, and Node - cpm-cms/nextjs/install-nvm-node](../cpm-cms/nextjs/install-nvm-node.md)
- Apply [Set swap - cpm-cms/set-swap](../cpm-cms/set_swap.md)
- Apply [Install & run Coolify on a Virtual Private Server - cpm-cms/coolify/install-prod-coolify](../cpm-cms/coolify/install-prod-coolify.md)
  - Backup your /data/coolify/source/.env file on the VPS to a safe place
- Apply [Configure Coolify - Set Hetzner API Key if you use Hetzner - cpm-cms/coolify/set_hetzner_api_key](../cpm-cms/coolify/set_hetzner_api_key.md)  
- Apply [Configure Coolify - Setup Wildcard SSL Certificates with Traefik - cpm-cms/coolify/configure-coolify-traefik-vps](../cpm-cms/coolify/configure-coolify-traefik-vps.md)
- Apply [Set e-mail with Resend - cpm-cms/coolify/coolify-email-resend](../cpm-cms/coolify/coolify-email-resend.md)
- Take a snapshot of the VPS. Can be handy later.
- Apply [Implement a Payload CMS with Coolify on a Virtual Private Server](../cpm-cms/payload/publish-payload-cms-vps.md)
  - [cpm-cms/payload/publish-payload-cms-vps](../cpm-cms/payload/publish-payload-cms-vps) 
  - For personal-blog our git repository is https://github.com/rgucluer/personal-blog
- Apply [Coolify - Add a new Source ( Virtual Private Server ) - cpm-cms/coolify/add-new-source-vps](../cpm-cms/coolify/add-new-source-vps.md)
  - Github App Name: < github-app-name-vps >
    - app-personal-blog-vps
    - Automated
      - Selected endpoint: https://coolify.< domain-name >
      - Uncheck `Preview Deployments`
      - Register now
      - Confirm access ( GitHub Authentication )        
        - Authenticate
      - Create App name
        - GitHub App Name : app-personal-blog-vps
        - Click "Create GitHub App for `< my-github-username >`"
      - Click Install Repositories on GitHub
      - Authorization Request for GitHub - Install `< github-app-name-vps >`
        - Click: Only select repositories
          - Select from "Select repositories"
            - `< my-github-username >`/personal-blog
        - Click Install
      - We are back in Coolify UI Sources - GitHub App Form -> 
        - App Name: app-personal-blog-vps
        - Save

- Apply [Add Source to the Project (VPS) - coolify/add-source-to-project-vps](../cpm-cms/coolify/add-source-to-project-vps.md)
[coolify/add-source-to-project-vps](../coolify/add-source-to-project-vps.md)
  - Coolify Web UI -> Projects -> payload-project ( production )
    - Select a GitHub App: app-personal-blog-vps
      - Branch: main
      - Build Pack: Docker Compose
      - Docker Compose Location: /docker-compose.yml
      - Continue
      - Set Name: personal-blog-prod
        - Save
- Apply [Configure the Payload Project (VPS) - coolify/configure-payload-vps](../cpm-cms/coolify/configure-payload-vps.md)
  - Name: personal-blog-prod
  - Configuration -> Git Source
    - Repository:
      - < github-username >/personal-blog
    - Branch: 
      - main
    - Commit SHA
      - HEAD
    - Save

- Apply [Deploy / Redeploy Payload Application - ](../cpm-cms/payload/publish-payload-cms-vps.md#deploy--redeploy-payload-application)
  - Projects -> payload-project -> personal-blog-prod
    - Advanced -> Force deploy (without cache)

- Apply [Set Traefik for the new application (VPS) - cpm-cms/coolify/set-traefik-for-new-app-vps](../cpm-cms/coolify/set-traefik-for-new-app-vps.md)
  - Traefik Dynamic Configuration Filename: personal-blog.yaml
- Apply [Check the Application - cpm-cms/payload/publish-payload-cms-vps](../cpm-cms/payload/publish-payload-cms-vps.md#check-the-application)

- Page renders with images.

