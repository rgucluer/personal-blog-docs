## Add Source to the Project

- Coolify Web UI -> Projects -> < project-name >
  - Click Resources +New
    - Applications -> Git Based
      - Click Private Repository ( with GitHub App )
        - Select a GitHub App: Click < github-app-name-vm > button
          - Repository: < github-repo-name >
          - Click Load Repository
            - Branch: dev
            - Build Pack: Docker Compose
            - Change Docker Compose Location to:
              - /docker-compose.yml
              - ATTENTION : File extension is .yml not .yaml. Please check & edit.
            - Continue
            - If successful, this loads docker-compose.yml, opens configuration menu.
            - Set Name: < coolify-application-name >
              - personal-blog
            - Save

---

### Continue with : Configure the Payload Project [coolify/configure-payload-dev](../payload/publish-payload-app.md#configure-the-payload-project-coolifyconfigure-payload-dev)
