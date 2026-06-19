
- Check ownership of files/directories in payload container

- Wipe & start over
  - ATTENTION, all data will be deleted.
  - Delete payload service
    - Stop payload service
    - Delete payload service (Danger Zone)
  - Delete database service
    - Stop mongodb-payload service
    - Delete mongodb-payload service (Danger Zone)
  - Recreate database service
    - Apply [payload/create-mongodb.md](../payload/create-mongodb.md)
  - Recreate payload service
    - Add Source to the Project
      - Apply [coolify/add-source-to-project](../coolify/add-source-to-project.md)
        - Project: personal-blog
        - GitHub App: app-personal-blog-vm
        - Repository: personal-blog
        - Docker Compose, /docker-compose.yml, dev branch
        - Continue
        - Set Name: personal-blog-dev
        - Save

    - Configure the Payload Project
      - Apply [coolify/configure-payload-dev](../coolify/configure-payload-dev.md)
        - Name: personal-blog-dev
        - Description: Personal Blog
        - Domains for payload: https://www.devblogserver.my-domain.com,https://devblogserver.my-domain.com
        - Diffrent entries from the document
          - NEXT_PUBLIC_SERVER_URL=https://www.devblogserver.my-domain.com
          - DATABASE_URL=< Mongo URL (internal) from Coolify UI >

    - Enter Mongo URL (public) to local .env file DATABASE_URL , save
      - Change IP address to VM IP address ( ....@`IP ADDRESS`/?directConnection=true )

    - Deploy Payload Project
      - VM -> Coolify -> Projects -> personel-blog -> personel-blog-dev
        - Advanced -> Force Deploy

  - Check/Set service name in Traefik Dynamic configuration for payload service
    - service value of payload-https route must be correctly set
      - payload-abc...yz@docker
      - Save

  - Restart Proxy - Traefik
    - VM -> Coolify -> Servers -> devblogserver -> Restart Proxy

  - Check https://www.devblogserver.< domain-name >
    - Click "Visit the admin dashboard"
    - Create a new Payload admin user
      - Welcome to your dashboard!
        - Click "Seed your database"
          - Seeding with data
            - Message: `Database seeded! You can now visit your website`
            - OK, we are good.
            - Click `visit your website`
            - Homepage renders with images. 
          - Message: "An error ocurred during seeding"
            - Problem continues
            - Hımm, not so good. ( Did not happen in my setup after applying the steps above )

