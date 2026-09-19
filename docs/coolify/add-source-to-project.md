## Add Source to the Project

- Coolify Web UI -> Projects -> < coolify-project-name > ( production )
  - Click +New Resource
    - Applications 
      - Click Git Repository ( with GitHub App ) Deploy button
        - Choose GitHub App: Click < github-app-name-vm > button
          - Choose repository: -> Repository: < github-username >/< github-repo-name >
          - Click Load Repository
            - Build configuration
              - Branch: dev
              - Build Pack: Docker Compose
              - Change Compose file location to:
                - /docker-compose.yml
                - ATTENTION : File extension is .yml not .yaml. Please check & edit.
              - Continue
            - If successful, this loads docker-compose.yml, opens configuration menu.
              - Application details
                - Set Name: < coolify-application-name >
                  - personal-blog
            - Save changes

---

### Continue with : Configure the Payload Project [coolify/configure-payload-dev](../payload/publish-payload-app.md#configure-the-payload-project-coolifyconfigure-payload-dev)
