## Configure the Project

- Coolify Web UI -> Projects -> < coolify-project-name > ( production ) ( payload-project )
  - Resource -> Application: < coolify-application-name > ( personal-blog )
    - Settings -> General
      - Click Reload Compose
      - Name: < coolify-application-name >
        - personal-blog
      - Description: Personal blog application made with Payload CMS
        - Save changes
      - Build pipeline
        - Build strategy: Compose
        - Base Directory: /
        - Check: Preserve Repository During Deployment
        - Docker compose location : /docker-compose.yml
        - Save changes
      - Docker Compose
        - Check : Escape special characters in labels
      - Deployment lifecycle
        - Post-deployment: `/app/after-deploy.sh`
        - Save changes
    - Settings -> Domains
      - Add Domain
        - Add domain Form
          - Service: personal-blog
          - Protocol: https
          - Domain: < prod-domain-name >
            - my-domain.com
            - cpm-cms.my-domain.com
          - Save
          - This will also add www.< prod-domain-name >
      - Click settings for https://< prod-domain-name >
        - www redirect: Redirect to www
        - Save
    - Settings -> Environment Variables
      - Add the following variables
        - For every variable
          - Value type: Single line
          - Build time: Available during build
          - Interpolation: Interpolate $VARIABLES
          - Runtime: Available in the container
      - NODE_OPTIONS: --no-deprecation --max-old-space-size=2048
      - HOSTNAME: 0.0.0.0
      - DATABASE_URL: Get value from mongodb-payload Mongo URL (internal), and paste as value. 
      - PAYLOAD_SECRET: < copy value from local .env file to here >
        - You can also enter a new secret. Changing the secret value after Payload application creation, seeding will result in an error. So save secret values in a safe place.
      - NEXT_PUBLIC_SERVER_URL : `https://www.< prod-domain-name >`
      - CRON_SECRET: < copy value from local .env file , or Enter_your_password_here >
      - PREVIEW_SECRET: < copy value from local .env file , or Enter_your_password_here >

    - Settings -> Advanced
      - Build
        - Build cache: Use Docker build cache
        - Build arguments: Inject Build Args automatically
        - Source commit availability: Runtime only
      - Container:
        - Container naming: Generated name (rolling updates)
      - Deployment
        - Auto Deploy: Manual deployments only
      - Git
        - Submodules: Clone submodules
        - Git LFS: Enabled
        - Clone depth: Shallow clone
      - Docker Compose
        - Compose deployment: Managed by Coolify
        - Predefined network: Connect to predefined network
      - Proxy
        - Gzip compression: Enabled
        - Path prefixes: Strip prefixes

    - Deploy -> Git Source
      - Connected source: < github-app-name-vps >  ( personal-blog-vps )
      - Repository:
        - < github-username >/< github-repo-name >
      - Branch: 
        - main
      - Commit SHA
        - HEAD
      - Save

    - Deploy -> Preview Deployments
      - Disabled ( Showing `Enable Preview Deployments` button )

    - Operations -> Resource Limits
      - CPU
        - CPU limit: 0.5
          - Number is a fractional number. 0.000 means no limit.
        - CPU Weight: 512
      - Memory
        - Memory reservation: 2g
        - Memory Limit: 2g
        - Swappiness: 1
        - Memory and Swap Limit: 2g
        - Save changes
        - ( Set Memory values to according to your setup. )

    - Settings -> General -> Access -> Public access
      - https://< prod-domain-name >
        - Click Manage Domains
      - Check Domains for personal-blog

### Continue with: Deploy Application [payload/publish-payload-cms-vps](../payload/publish-payload-cms-vps.md#deploy--redeploy-payload-application)


