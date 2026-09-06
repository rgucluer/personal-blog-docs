## Configure the Payload Project

- Coolify Web UI -> Projects -> < coolify-project-name > ( production )
  - Resource -> Application: < coolify-application-name >
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
    - Settings -> Domains
      - Add
        - Add domain Form
          - Service: personel-blog
          - Protocol: https
          - Domain: < dev-domain-name >
          - Save
          - This will also add www.< dev-domain-name >
    - Settings -> Environment Variables
      - Add the following variables
        - For every variable
          - Value type: Single line
          - Build time: Available during build
          - Runtime: Available in the container
          - Interpolation: Interpolate $VARIABLES
      - NODE_OPTIONS=--no-deprecation --max-old-space-size=2048
      - HOSTNAME=0.0.0.0
      - DATABASE_URL: Get value from mongodb-payload Mongo URL (internal), and paste as value. 
      - PAYLOAD_SECRET: < copy value from local copy .env file to here >
      - NEXT_PUBLIC_SERVER_URL : https://www.< dev-domain-name >
      - CRON_SECRET: < copy value from local copy .env file to here , or Enter_your_password_here >
      - PREVIEW_SECRET: < copy value from local copy .env file to here , or Enter_your_password_here >

    - Settings -> Advanced
      - Build
        - Build cache: Use Docker build cache
        - Build arguments: Inject Build Args automatically
        - Source commit availability: Runtime only
      - Container:
        - Container naming: Generated name (rolling updates)
      - Deployment
        - Auto Deploy: Manual deployments only
        - Preview Deployments: Disabled
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
      - Repository:
        - < github-username >/< github-repo-name >
      - Branch: 
        - dev
      - Commit SHA
        - HEAD
      - Save
 
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
      - 2 configured domains -> (Gear) Manage Domains
      - Check Domains for payload app

---

## Continue with : Deploy Application [payload/publish-payload-app](../payload/publish-payload-app.md#deploy--redeploy-payload-application)

