## Coolify - Add a new Source

Following Coolify documentation https://coolify.io/docs/applications/ci-cd/github/setup-app

Visit `https://coolify.< domain-name >`

- Coolify Web UI -> Sources -> New Source
  - GitHub App
    - Name: < github-app-name-vps >  ( payload-cms-vps, cpm-cms, etc)
    - Continue
    - There are two options:
      - Automated Installation
        - Use this for creating a new GitHub App for your source repository
      - [Manual Installation](#manual-installation)
        - If GitHub App already exists, you can add it to Coolify with Manual Installation.

### Automated Installation

- Webhook endpoint: Use an instance endpoint
- Instance endpoint: Use https://coolify.my-domain.com
- Preview deployment access: Do not update pull requests
- Register with GitHub
  - Confirm access
    - Authenticate
  - Create GitHub App: 
    - GitHub App name: < github-app-name-vps >
      - The name must be unique across GitHub.
      - personal-blog-vps
  - Click Create GitHub App for < github-username >
  - Allow browser if browser wants permission for this operation
  - Click Install Repositories on GitHub
  - Install `< github-app-name-vps >`
    - Install on your personal account < github-username >:
      - Click: Only select repositories
        - Select from "Select repositories"
          - `< my-github-username >`/`< git-repo-name >`
      - Click Install
- We are back in Coolify UI 
  - Sources -> < github-app-name-vps >
    - Settings -> General
      - App Name: < github-app-name-vps >
      - Save changes

### Continue with [Add Source to the Project (VPS)](../payload/publish-payload-cms-vps.md#add-source-to-the-project-vps-coolifyadd-source-to-project-vps)

---

### Manual Installation

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: < github-app-name-vps >
      - Example: personal-blog-vps
    - Continue
    - Apply [add-new-source-manual-vps](add-new-source-manual-vps.md)

---

### Continue with [Add Source to the Project (VPS)](../payload/publish-payload-cms-vps.md#add-source-to-the-project-vps-coolifyadd-source-to-project-vps)

---

On GitHub Web -> GitHub App
- https://github.com/settings/apps/`< github-app-name >`

Coolify Web UI -> GitHub App
- https://coolify.`< domain-name >`/source/github/`< Source ID >`


