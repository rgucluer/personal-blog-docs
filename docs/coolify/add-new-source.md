## Coolify - Add a new Source

Following Coolify documentation https://coolify.io/docs/applications/ci-cd/github/setup-app

Visit `https://coolify.< dev-domain-name >`

- Coolify Web UI -> Sources -> New Source
  - GitHub App
    - Name: < github-app-name-vm >
      - personal-blog-for-vm
    - Continue
    - There are two options:
      - Automated Installation
        - Use this for creating a new GitHub App for your source repository
      - [Manual Installation](#manual-installation)
        - If GitHub App already exists, you can add it to Coolify with Manual Installation.

### Automated Installation

- Webhook endpoint: Use an instance endpoint
- Instance endpoint: Use https://coolify.< dev-domain-name >
- Preview deployment access: Do not update pull requests
- Register with GitHub
  - Confirm access
    - Authenticate
  - Create GitHub App: 
    - GitHub App name: < github-app-name-vm >
      - The name must be unique across GitHub.
      - personal-blog-for-vm
    - Complete GitHub Installation
      -Warning: Repository access has not been installed yet. Complete this step before attaching the source to an application.
  - Click `Install Repositories`
  - Authorization Form - Install < github-app-name-vm >
    - Install on your personal account < github-username >:
    - Click: Only select repositories
      - Select from "Select repositories"
        - `< github-username >`/`< github-repo-name >`
    - Click Install
  - We are back in Coolify UI
    - Sources -> < github-app-name-vm >
      - Settings -> General
        - App Name: < github-app-name-vm >

---

### Continue with : Add Source to the Project [coolify/add-source-to-project](../payload/publish-payload-app.md#add-source-to-the-project-coolifyadd-source-to-project)

---

### Manual Installation

- Coolify Web UI -> Sources -> Add+
  - New GitHub App
    - Name: < github-app-name-vm >
      - personal-blog-for-vm
    - Continue
    - Manual Installation
      - Continue
      - Apply [add-new-source-manual](add-new-source-manual.md)

---

### Continue with : Add Source to the Project [coolify/add-source-to-project](../payload/publish-payload-app.md#add-source-to-the-project-coolifyadd-source-to-project)

---

## Errors

- Failed to fetch GitHub App information: Integration not found
  - Fill in App Id, and Installation Id

---

On GitHub Web -> GitHub App
- https://github.com/settings/apps/`< github-app-name >`

Coolify Web UI -> GitHub App
- https://coolify.`< dev-domain-name >`/source/github/`< Source ID >`

---

## References

- https://coolify.io/docs/applications/ci-cd/github/setup-app
