## Add Source to the Project

- Coolify Web UI -> Projects -> < coolify-project-name > ( production )
  - Click +New resource
    - Applications 
      - Click Git Repository ( with GitHub App ) Deploy button
        - Select a GitHub App: Click < github-app-name-vm > button
          - Repository: < github-username >/< github-repo-name >
          - Click Load Repository
            - Branch: dev
            - Build Pack: Docker Compose
            - Change Docker Compose file to:
              - /docker-compose.yml
              - ATTENTION : File extension is .yml not .yaml. Please check & edit.
            - Continue
            - If successful, this loads docker-compose.yml, opens configuration menu.
            - Set Name: < coolify-application-name >
              - personal-blog
            - Save changes

---

### Continue with : Configure the Payload Project [coolify/configure-payload-dev](../payload/publish-payload-app.md#configure-the-payload-project-coolifyconfigure-payload-dev)
