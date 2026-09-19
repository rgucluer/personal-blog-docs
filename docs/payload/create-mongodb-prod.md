# Create a MongoDB using Coolify

- Coolify UI -> Projects -> payload-project (production) 
  - +Add Resource (or +New )
  - Databases
    - Mongo DB 
      - Click Deploy
        - Settings:
          - Name: mongodb-payload
          - Public access:
            - Access: Private
          - Save changes
        - Resource Limits
          - CPU
            - CPU limit: 0.25
            - CPU Weight: 256
          - Memory:
            - Memory reservation: 1g
            - Memory Limit: 1g
            - Maximum and Swap Limit: 1g
            - Swappiness: 1
          - Save changes
        - Start/Restart
          - Close "Database Startup" form after "Database started." message
          - Check for Running(green) label on top

---

### Continue with: Create a local copy of Payload CMS Website template[payload/publish-payload-cms-vps](publish-payload-cms-vps.md#create-a-local-copy-of-payload-cms-website-template)

---

