# Create a MongoDB using Coolify

- Coolify UI -> Projects -> personal-blog (production) 
  - Resources
    - +Add Resource (or +New )
    - Databases
      - Mongo DB
        - Click Deploy
          - Settings:
            - General:
              - Database details
                - Name: mongodb-payload
                - Public access
                  - Access: Public through TCP proxy
                  - Public port: 27017
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
            - Check for (green) Running label on top

---

### Continue with : Install nvm, and Node [nextjs/install-nvm-node](publish-payload-app.md#install-nvm-and-node-on-development-pc-nextjsinstall-nvm-node)

---

## Optional: Install MongoDB Compass (Debian based)

On Developer PC

https://www.mongodb.com/docs/compass/install/?operating-system=linux&package-type=.deb

```bash
wget https://downloads.mongodb.com/compass/mongodb-compass_1.49.6_amd64.deb
```

```bash
sudo dpkg -i mongodb-compass_1.49.6_amd64.deb
```

```bash
mongodb-compass
```

### Create new connection
- Start MongoDB service in Coolify
- Click "Add new connection"
- URI: Paste Mongo URL (public) here
  - Delete IP address ,and enter IP address of Virtual Machine
  - Save & Connect
  - Disconnect
  
